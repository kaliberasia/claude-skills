# BigQuery Queries for Weekly Review

**Purpose:** Query templates for pulling weekly performance data via the `bash bq` wrapper script.

**Usage:** Substitute placeholders with values from client's `config/bigquery-config.json`

---

## Query Template Placeholders

From client config file:

- `{project_id}` - BigQuery project (usually "kaliberasia")
- `{dataset}` - Client's dataset name (e.g., "jal_yoga", "watch_exchange")
- `{table}` - Platform table name (e.g., "jal_yoga_google", "we_meta")
- `{conversion_column}` - Client's conversion column name (varies by client)
- `{week_start}` - Monday date in 'YYYY-MM-DD' format
- `{week_end}` - Sunday date in 'YYYY-MM-DD' format

---

## Query 1: Weekly Performance Summary

**Purpose:** Core metrics with WoW and 4-week average comparison

```sql
WITH date_ranges AS (
  SELECT
    DATE('{week_start}') as current_week_start,
    DATE('{week_end}') as current_week_end,
    DATE_SUB(DATE('{week_start}'), INTERVAL 7 DAY) as last_week_start,
    DATE_SUB(DATE('{week_end}'), INTERVAL 7 DAY) as last_week_end,
    DATE_SUB(DATE('{week_start}'), INTERVAL 28 DAY) as four_week_start,
    DATE_SUB(DATE('{week_end}'), INTERVAL 7 DAY) as four_week_end
),

current_week AS (
  SELECT
    SUM(spend) as spend,
    SUM(impressions) as impressions,
    SUM(clicks) as clicks,
    SUM({conversion_column}) as conversions,
    SAFE_DIVIDE(SUM(clicks), SUM(impressions)) as ctr,
    SAFE_DIVIDE(SUM(spend), SUM(clicks)) as cpc,
    SAFE_DIVIDE(SUM(spend), SUM({conversion_column})) as cpa
  FROM `{project_id}.{dataset}.{table}`
  WHERE date BETWEEN (SELECT current_week_start FROM date_ranges)
    AND (SELECT current_week_end FROM date_ranges)
),

last_week AS (
  SELECT
    SUM(spend) as spend,
    SUM(impressions) as impressions,
    SUM(clicks) as clicks,
    SUM({conversion_column}) as conversions,
    SAFE_DIVIDE(SUM(clicks), SUM(impressions)) as ctr,
    SAFE_DIVIDE(SUM(spend), SUM(clicks)) as cpc,
    SAFE_DIVIDE(SUM(spend), SUM({conversion_column})) as cpa
  FROM `{project_id}.{dataset}.{table}`
  WHERE date BETWEEN (SELECT last_week_start FROM date_ranges)
    AND (SELECT last_week_end FROM date_ranges)
),

four_week_avg AS (
  SELECT
    AVG(weekly_spend) as spend,
    AVG(weekly_impressions) as impressions,
    AVG(weekly_clicks) as clicks,
    AVG(weekly_conversions) as conversions,
    AVG(weekly_ctr) as ctr,
    AVG(weekly_cpc) as cpc,
    AVG(weekly_cpa) as cpa
  FROM (
    SELECT
      DATE_TRUNC(date, WEEK(MONDAY)) as week,
      SUM(spend) as weekly_spend,
      SUM(impressions) as weekly_impressions,
      SUM(clicks) as weekly_clicks,
      SUM({conversion_column}) as weekly_conversions,
      SAFE_DIVIDE(SUM(clicks), SUM(impressions)) as weekly_ctr,
      SAFE_DIVIDE(SUM(spend), SUM(clicks)) as weekly_cpc,
      SAFE_DIVIDE(SUM(spend), SUM({conversion_column})) as weekly_cpa
    FROM `{project_id}.{dataset}.{table}`
    WHERE date BETWEEN (SELECT four_week_start FROM date_ranges)
      AND (SELECT four_week_end FROM date_ranges)
    GROUP BY week
  )
)

SELECT
  'current_week' as period,
  cw.spend,
  cw.impressions,
  cw.clicks,
  cw.conversions,
  cw.ctr,
  cw.cpc,
  cw.cpa,
  SAFE_DIVIDE((cw.spend - lw.spend), lw.spend) as wow_spend_change,
  SAFE_DIVIDE((cw.conversions - lw.conversions), lw.conversions) as wow_conv_change,
  SAFE_DIVIDE((cw.cpa - lw.cpa), lw.cpa) as wow_cpa_change,
  fwa.spend as four_week_avg_spend,
  fwa.conversions as four_week_avg_conversions,
  fwa.cpa as four_week_avg_cpa
FROM current_week cw, last_week lw, four_week_avg fwa

UNION ALL

SELECT
  'last_week' as period,
  spend,
  impressions,
  clicks,
  conversions,
  ctr,
  cpc,
  cpa,
  NULL as wow_spend_change,
  NULL as wow_conv_change,
  NULL as wow_cpa_change,
  NULL as four_week_avg_spend,
  NULL as four_week_avg_conversions,
  NULL as four_week_avg_cpa
FROM last_week;
```

---

## Query 2: Campaign Performance Breakdown

**Purpose:** Individual campaign performance for the review week

```sql
SELECT
  campaign_name,
  SUM(spend) as spend,
  SUM(impressions) as impressions,
  SUM(clicks) as clicks,
  SUM({conversion_column}) as conversions,
  SAFE_DIVIDE(SUM(clicks), SUM(impressions)) as ctr,
  SAFE_DIVIDE(SUM(spend), SUM(clicks)) as cpc,
  SAFE_DIVIDE(SUM(spend), SUM({conversion_column})) as cpa
FROM `{project_id}.{dataset}.{table}`
WHERE date BETWEEN DATE('{week_start}') AND DATE('{week_end}')
GROUP BY campaign_name
ORDER BY spend DESC;
```

---

## Query 3: MTD Pacing Status

**Purpose:** Month-to-date spend vs budget for pacing analysis

```sql
WITH month_info AS (
  SELECT
    DATE_TRUNC(CURRENT_DATE(), MONTH) as month_start,
    LAST_DAY(CURRENT_DATE()) as month_end,
    EXTRACT(DAY FROM LAST_DAY(CURRENT_DATE())) as total_days,
    EXTRACT(DAY FROM CURRENT_DATE()) as days_elapsed
)

SELECT
  campaign_name,
  SUM(spend) as mtd_spend,
  -- Budget should come from config, not query
  -- This query provides spend; compare to config budget in analysis
  COUNT(DISTINCT date) as days_with_data
FROM `{project_id}.{dataset}.{table}`
WHERE date BETWEEN (SELECT month_start FROM month_info) AND CURRENT_DATE()
GROUP BY campaign_name
ORDER BY mtd_spend DESC;
```

---

## Query 4: Platform Comparison (Multi-Platform Clients)

**Purpose:** Compare performance across Google, Meta, etc.

**Note:** Only run if client config shows multiple platforms. Run query for each platform table and aggregate results in analysis.

```sql
-- Run this query for each platform table (google, meta, youtube, etc.)
SELECT
  '{platform_name}' as platform,  -- Substitute with platform name
  SUM(spend) as spend,
  SUM(impressions) as impressions,
  SUM(clicks) as clicks,
  SUM({conversion_column}) as conversions,
  SAFE_DIVIDE(SUM(clicks), SUM(impressions)) as ctr,
  SAFE_DIVIDE(SUM(spend), SUM(clicks)) as cpc,
  SAFE_DIVIDE(SUM(spend), SUM({conversion_column})) as cpa
FROM `{project_id}.{dataset}.{table}`
WHERE date BETWEEN DATE('{week_start}') AND DATE('{week_end}');
```

---

## Query 5: Video Performance Metrics (Video/Awareness Campaigns)

**Purpose:** Track video completion rates, ThruPlays, and engagement funnel for awareness campaigns

**Note:** Only run if client has video/awareness campaigns (Meta awareness, YouTube). Check campaign names for "Awareness", "Video", "ThruPlay" keywords.

```sql
WITH date_ranges AS (
  SELECT
    DATE('{week_start}') as current_week_start,
    DATE('{week_end}') as current_week_end,
    DATE_SUB(DATE('{week_start}'), INTERVAL 7 DAY) as last_week_start,
    DATE_SUB(DATE('{week_end}'), INTERVAL 7 DAY) as last_week_end
),

current_week AS (
  SELECT
    campaign_name,
    SUM(spend) as spend,
    SUM(impressions) as impressions,
    SUM(video_plays) as video_plays,
    SUM(video_p25_watched_actions) as video_25_complete,
    SUM(video_p50_watched_actions) as video_50_complete,
    SUM(video_p75_watched_actions) as video_75_complete,
    SUM(video_p95_watched_actions) as video_95_complete,
    SUM(video_p100_watched_actions) as video_100_complete,
    SUM(video_thruplay_watched_actions) as thruplay,
    SAFE_DIVIDE(SUM(video_plays), SUM(impressions)) as vtr,
    SAFE_DIVIDE(SUM(spend), SUM(video_thruplay_watched_actions)) as cost_per_thruplay,
    SAFE_DIVIDE(SUM(video_p100_watched_actions), SUM(video_plays)) as completion_rate,
    SAFE_DIVIDE(SUM(spend), SUM(video_plays)) as cost_per_view
  FROM `{project_id}.{dataset}.{table}`
  WHERE date BETWEEN (SELECT current_week_start FROM date_ranges)
    AND (SELECT current_week_end FROM date_ranges)
    AND (LOWER(campaign_name) LIKE '%awareness%'
      OR LOWER(campaign_name) LIKE '%video%'
      OR LOWER(campaign_name) LIKE '%thruplay%')
  GROUP BY campaign_name
),

last_week AS (
  SELECT
    campaign_name,
    SUM(spend) as spend,
    SUM(impressions) as impressions,
    SUM(video_plays) as video_plays,
    SUM(video_thruplay_watched_actions) as thruplay,
    SAFE_DIVIDE(SUM(video_plays), SUM(impressions)) as vtr,
    SAFE_DIVIDE(SUM(spend), SUM(video_thruplay_watched_actions)) as cost_per_thruplay
  FROM `{project_id}.{dataset}.{table}`
  WHERE date BETWEEN (SELECT last_week_start FROM date_ranges)
    AND (SELECT last_week_end FROM date_ranges)
    AND (LOWER(campaign_name) LIKE '%awareness%'
      OR LOWER(campaign_name) LIKE '%video%'
      OR LOWER(campaign_name) LIKE '%thruplay%')
  GROUP BY campaign_name
)

SELECT
  cw.campaign_name,
  cw.spend,
  cw.impressions,
  cw.video_plays,
  cw.thruplay,
  cw.vtr,
  cw.cost_per_thruplay,
  cw.cost_per_view,
  cw.video_25_complete,
  cw.video_50_complete,
  cw.video_75_complete,
  cw.video_95_complete,
  cw.video_100_complete,
  cw.completion_rate,
  SAFE_DIVIDE((cw.vtr - lw.vtr), lw.vtr) as wow_vtr_change,
  SAFE_DIVIDE((cw.thruplay - lw.thruplay), lw.thruplay) as wow_thruplay_change,
  SAFE_DIVIDE((cw.cost_per_thruplay - lw.cost_per_thruplay), lw.cost_per_thruplay) as wow_cost_per_thruplay_change
FROM current_week cw
LEFT JOIN last_week lw ON cw.campaign_name = lw.campaign_name
ORDER BY cw.spend DESC;
```

**Column Mapping Notes:**
- Meta uses `video_plays`, `video_thruplay_watched_actions`, `video_pXX_watched_actions`
- YouTube may use different column names - check config `video_metrics` mapping
- If columns don't exist, query will fail - only run for platforms with video data

---

## Query Execution Strategy

### Step 1: Load Client Config

Read `{client-name}/config/bigquery-config.json` to get:
- `dataset` - Client's BigQuery dataset
- `platforms` - Object with platform configs (google, meta, etc.)
  - Each platform has: `table`, `conversion_column`
- `budgets` - Monthly budgets by campaign (for pacing comparison)
- `targets` - Target CPA/ROAS by campaign (for status indicators)

### Step 2: Determine Which Queries to Run

**Always run:**
- Query 1: Weekly Performance Summary (for overall metrics)
- Query 2: Campaign Performance Breakdown (for campaign-level analysis)
- Query 3: MTD Pacing Status (for monthly pacing section)

**Conditionally run:**
- Query 4: Platform Comparison - Only if `config.platforms` has multiple platforms
- Query 5: Video Performance Metrics - Only if client has video/awareness campaigns (check campaign names for "Awareness", "Video", "ThruPlay" in Query 2 results, or check config for `video_metrics` mapping)

### Step 3: Execute Queries via `bash bq` Wrapper Script

**Primary method — use the built-in weekly-review command:**
```bash
bash bq weekly-review --client {client-slug}

# Custom date range
bash bq weekly-review --client {client-slug} --week-start {YYYY-MM-DD} --days 7
```

This returns per-platform breakdown, campaign-level metrics, totals with CTR/CVR, and budget/KPI targets from config — covering Queries 1-3 in a single call.

**For custom queries (platform comparison, video metrics, etc.):**
```bash
bash bq query --sql "SELECT ... FROM \`kaliberasia.{dataset}.{table}\` ..."
```

**Parallel execution preferred** - Run independent queries simultaneously for speed.

### Step 4: Handle Query Results

**Expected result format from `bash bq`:**
```json
{
  "success": true,
  "data": {
    "platforms": { ... },
    "campaigns": [ ... ],
    "totals": { ... }
  }
}
```

**Parse and structure data for analysis:**
- Extract current week metrics
- Extract comparison metrics (last week, 4-week avg)
- Calculate WoW changes (if not in query results)
- Map campaign data to targets from config

---

## Error Handling

### Query Failures

**Table not found:**
- Error: "Not found: Table {project_id}:{dataset}.{table}"
- Action: Check config table name, verify table exists in BigQuery

**Column not found:**
- Error: "Unrecognized name: {conversion_column}"
- Action: Try fallback "conversions", or check config for correct column name

**No data returned:**
- Empty result set
- Action: Check date range (is data available?), verify campaigns were active

**Date range errors:**
- Error: "Invalid date format"
- Action: Ensure dates are 'YYYY-MM-DD' format, no timezone

### Data Quality Checks

After receiving results, validate:

1. **All zeros:** If spend/conversions all zero, flag for investigation
2. **Extreme outliers:** If WoW change >500%, flag as anomaly
3. **Missing campaigns:** If config lists campaigns not in results, note in analysis

---

## Example: Complete Query Execution

**Client:** Watch Exchange
**Config values:**
- project_id: "kaliberasia"
- dataset: "watch_exchange"
- platforms.google.table: "we_google"
- platforms.google.conversion_column: "conversions"
- platforms.meta.table: "we_meta"
- platforms.meta.conversion_column: "fb_conversions"

**Dates:**
- week_start: "2025-01-13" (Monday)
- week_end: "2025-01-19" (Sunday)

**Queries to run:**

1. Weekly summary - Google: Substitute table="we_google", conversion="conversions"
2. Weekly summary - Meta: Substitute table="we_meta", conversion="fb_conversions"
3. Campaign breakdown - Google
4. Campaign breakdown - Meta
5. MTD pacing - Google
6. MTD pacing - Meta

**Aggregate results** from both platforms for overall client performance.

---

## Notes

- **Always use SAFE_DIVIDE** to avoid division by zero errors
- **Date formats must be 'YYYY-MM-DD'** without timezone
- **Query through yesterday** - never include today (incomplete data)
- **Conversion column varies** - always use config value, never hardcode "conversions"
- **Budget and targets** come from config, not queries - queries provide actuals only
