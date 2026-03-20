---
name: kaliber-instant-auditor
description: Execute comprehensive weekly performance reviews for marketing clients every Monday or Friday. This skill should be used when the user requests a weekly review, week-in-review, or weekly analysis for a client. It queries BigQuery directly for live data, analyzes performance against targets, generates hypotheses for issues, and produces a comprehensive week-in-review report with a feedback loop from prior actions and pushes approved action items to the pod action log.
---

# Weekly Review

## Overview

Execute systematic weekly performance reviews for Kaliber Group's performance marketing clients. This skill orchestrates the complete weekly review process: from pulling live BigQuery data to analyzing performance, diagnosing issues, generating actionable hypotheses, closing the feedback loop on prior actions, and producing structured outputs.

**Two Week Contexts:**
- **Analysis Week** - The previous week's campaign data being reviewed (e.g., Week 49 = Mon-Sun of last week)
- **Planning Week** - The current week being planned for (e.g., Week 50 = the week starting today)

**Outputs Generated:**
1. **Week-in-Review Analysis** - Comprehensive internal performance report for the **Analysis Week**, including feedback on prior actions and structured action items, saved to `{client}/01_outputs/weekly-reports/[YYYY - WXX] - Week in Review.md` (where WXX = Analysis Week)
2. **Pod Action Log Entry** - Approved action items for the **Planning Week**, appended to the pod-level action log at `{pod}/pod-action-log/[YYYY - WXX].md` (where WXX = Planning Week) — **only written after user approval**

**Feedback Loop:**
- Step 1.5 loads the **prior** Week-in-Review (which contains action items for the Analysis Week)
- Checks which actions were completed (via `/log-action` tracking) or infers from BigQuery data changes
- Generates "Last Week's Actions: Status & Impact" section in the current report
- Incomplete actions carry forward as "Carrying Forward" items in the new action plan

**Workflow with Deliberation Checkpoint:**
- Steps 0-1.5: Date context, prior review loading (feedback loop)
- Steps 2-6: Data gathering, analysis, and Week-in-Review report generation
- **Step 7: Deliberation Checkpoint** — User reviews analysis and proposed actions before committing
- Steps 8-9: Pod action log update (after approval) and final summary

**Key Features:**
- Queries BigQuery directly via `bash bq` wrapper script
- Uses client config for all parameters, targets, and budgets
- **Feedback loop:** References prior week's actions and measures their impact
- Progressive loading: Deep-dive analysis only when red flags appear
- **Deliberation checkpoint:** User reviews and approves action plan before it's committed
- Autonomous analysis: Complete data gathering and report generation, gated approval for action commitments
- Status-driven analysis: Green/Yellow = monitor, Red = investigate & act
- **Pod action log:** Cross-client action tracking at pod level (replaces Weekly Master Document)

## Automated Mode (Analysis Only)

**When the prompt contains "automated mode" or "analysis only"**, execute a streamlined workflow:

**What changes:**
- Execute **Steps 0 through 6** normally (date context → load prior review → load config → query BigQuery → analyze → diagnose → generate Week-in-Review)
- **Skip Steps 7, 8, and 9 entirely** — no deliberation checkpoint, no pod action log push, no final summary presentation
- **Never prompt for user input** — make best-judgment calls on any ambiguity:
  - Date context: Use system date, default week calculations (no confirmation needed)
  - Client identification: Taken from the prompt directly
  - Analysis decisions: Apply standard thresholds without asking
- If a critical error occurs (missing config, BigQuery unreachable), output a clear error summary to stdout and exit gracefully
- If a non-critical error occurs (one query fails, partial data), note it in the report and continue with available data
- Still write the Week-in-Review report to the standard output path: `{client}/01_outputs/weekly-reports/[YYYY - WXX] - Week in Review.md`
- Output a one-line summary when complete:
  - Success: `AUTOMATED_SUCCESS: {client} - Week-in-Review saved to {path}`
  - Failure: `AUTOMATED_FAILURE: {client} - {reason}`

**After automated reviews are generated:**
- AMs should review each client's Week-in-Review
- Deliberate with Claudette on the proposed action items
- Use `/commit [client]` to push the finalized action plan to the pod action log
- This replaces Steps 7-8 for automated reviews

**What stays the same:**
- All analysis logic, archetype patterns, status indicators, diagnostic depth
- Report quality standards — automated reports should be indistinguishable from manual ones
- File output paths and naming conventions
- Progressive loading and red flag deep-dives

---

## When to Use This Skill

**Primary Triggers:**
- User says "Run weekly review for [Client]"
- User says "Generate week-in-review for [Client]"
- User says "Weekly performance analysis for [Client]"
- User requests "all weekly reviews" (for multiple clients)

**Typical Timing:**
- **Fridays (afternoon):** After week's data is complete ✅ Recommended
- **Mondays (morning):** For start-of-week planning

## Complete Workflow

### Step 0: Get Current Date from System

**CRITICAL: Always run this first to establish today's date accurately.**

Execute bash command to get current date:

```bash
date +"%A, %B %d, %Y"
```

This returns today's date in format: "Monday, December 22, 2025"

**Store this date for all subsequent calculations.** Use this as the source of truth for:
- Determining Analysis Week dates
- Determining Planning Week dates
- Calculating Comparison Week dates
- Week number calculations

**Why this matters:** Using the system date ensures accurate week calculations and prevents confusion about which week is being analyzed.

---

### Step 1: Establish Date Context and Client Identification

**Confirm with user:**
- Today's date (state clearly for reference)
- Client name
- **Analysis Week** (the week being reviewed - default: last completed Monday-Sunday)
- **Planning Week** (the week ahead - default: current week starting today)

**Default Week Calculation (when running on Monday):**
- **Analysis Week:** Previous Monday-Sunday (e.g., if today is Mon Dec 16 = W51, analyze W50: Dec 9-15)
- **Planning Week:** Current week (e.g., W51: Dec 16-22 - the week just starting)
- **Comparison Week:** The week before Analysis Week (for WoW comparison)

**Default Week Calculation (when running on Friday):**
- **Analysis Week:** Current Monday-Friday (note: weekend data incomplete)
- **Planning Week:** Following week
- **Comparison Week:** Previous full week

**Custom override:** Accept user-specified date ranges for Analysis Week

**If any ambiguity, ask user to confirm before proceeding.**

**Example confirmation (Monday scenario - MOST COMMON):**
```
Today is Monday, December 16, 2024 (Week 51).

ANALYSIS WEEK (reviewing last week's data):
  Week 50: Monday, Dec 9 - Sunday, Dec 15, 2024

PLANNING WEEK (planning for this week):
  Week 51: Monday, Dec 16 - Sunday, Dec 22, 2024

COMPARISON WEEK (for WoW analysis):
  Week 49: Monday, Dec 2 - Sunday, Dec 8, 2024

Outputs will be:
  - Week-in-Review: [YYYY - W50] - Week in Review.md (Analysis Week)
  - Pod Action Log: pod-action-log/[YYYY - W51].md (Planning Week)
  - Prior Review (feedback loop): [YYYY - W49] - Week in Review.md (Comparison Week)

Proceed with these date ranges? [If yes, continue]
```

**Example confirmation (Friday scenario):**
```
Today is Friday, December 13, 2024 (Week 50).

ANALYSIS WEEK (reviewing this week so far - note: Sat/Sun incomplete):
  Week 50: Monday, Dec 9 - Friday, Dec 13, 2024 (partial)

PLANNING WEEK (planning for next week):
  Week 51: Monday, Dec 16 - Sunday, Dec 22, 2024

COMPARISON WEEK (for WoW analysis):
  Week 49: Monday, Dec 2 - Sunday, Dec 8, 2024

Outputs will be:
  - Week-in-Review: [YYYY - W50] - Week in Review.md (Analysis Week)
  - Pod Action Log: pod-action-log/[YYYY - W51].md (Planning Week)
  - Prior Review (feedback loop): [YYYY - W49] - Week in Review.md (Comparison Week)

Proceed with these date ranges? [If yes, continue]
```

---

### Step 1.5: Load Prior Week's Review (Feedback Loop)

**Purpose:** Load the prior cycle's Week-in-Review to see what actions were planned and whether they were completed. This closes the feedback loop between weekly reviews.

**Timing logic:**

```
Review Cycle A (e.g., Monday Feb 3, W06):
  → Analyzes W05 data (Analysis Week = W05)
  → Generates "W05 - Week in Review.md" with Action Items for W06 execution
  → Pushes actions to pod-action-log/2026-W06.md

During W06:
  → AMs execute actions, log via /log-action
  → Updates both pod-action-log/2026-W06.md AND "W05 - Week in Review.md"

Review Cycle B (e.g., Monday Feb 10, W07):
  → Analyzes W06 data (Analysis Week = W06)
  → Step 1.5: Load "W05 - Week in Review.md" — the PRIOR review
  → This review has action items for W06 with completion data
  → Cross-reference with W06 BigQuery data to measure impact
```

**Rule:** Load the Week-in-Review for the **Comparison Week** (= Analysis Week minus 1). This is always the review that generated action items for the current Analysis Week.

**Search paths (try in order):**
1. `{client}/01_outputs/weekly-reports/{YYYY} - W{Comparison} - Week in Review.md`

**Extract into `prior_review_context`:**

| Field | Source |
|-------|--------|
| `action_items[]` | From the `## Action Items` section — priority, task, campaign, status (✅/⏳/❌), outcome |
| `recommendations[]` | From `## Strategic Recommendations` |
| `active_tests[]` | From `## Active Tests` |
| `data_quality` | One of: `tracked`, `untracked`, `missing` |

**Determine `data_quality`:**
- **`tracked`** — Action items have completion data (checkboxes marked, outcomes filled). This means `/log-action` was used during the week.
- **`untracked`** — Action items exist but no completion updates (all still `⏳`). AMs didn't use `/log-action` — infer from BigQuery.
- **`missing`** — No prior review found. First review for this client or gap in history.

**Graceful degradation:**
- `missing` → Skip feedback loop entirely. Note: "First review for this client — feedback loop starts next cycle." Do NOT generate a "Last Week's Actions" section.
- `untracked` → Use inference from BigQuery data changes during Analysis Week (see inference rules below)
- `tracked` → Use completion data directly

**Inference rules (when `data_quality == "untracked"`):**

Compare BigQuery data for Analysis Week vs. Comparison Week:

| BigQuery Signal | Inference |
|----------------|-----------|
| Campaign spend $X → $0 | Campaign likely paused |
| Campaign spend $0 → $X | Campaign likely activated |
| Campaign spend +20%+ | Budget likely increased |
| CPC shift ±20%, volume stable | Bid strategy likely adjusted |
| Sharp CTR discontinuity | Possible creative rotation |
| New campaign appears in data | New campaign launched |

When using inference, clearly label in the report: "(inferred from spend data)" or "(inferred — not confirmed via /log-action)"

**Store `prior_review_context` for use in Step 6.**

---

### Step 2: Load Client Context

Load context files in priority order. Confirm critical files exist before proceeding.

**Critical (Always Load):**

**1. BigQuery Config** - `{client}/config/bigquery-config.json`

Required fields:
- `archetype` - Client type (lead_gen, ecommerce, awareness, full_funnel)
- `primary_kpi` - What to optimize for (cpa, roas, vtr, funnel_efficiency)
- `project_id` - BigQuery project (usually "kaliberasia")
- `dataset` - Client's dataset name
- `platforms` - Object with platform configs (google, meta, youtube, etc.)
  - Each platform: `enabled`, `table`, `conversion_column`
- `budgets` - Monthly budgets by campaign (for pacing comparison)
- `targets` - Target CPA/ROAS by campaign (for status indicators)
- `reporting` - Which sections to include (platform_breakdown, campaign_breakdown, etc.)

If missing: Alert user, cannot proceed without data access.

**After loading config, immediately load archetype definition:**

**2. Archetype Definition** - `.claude/context/archetypes.md`

Read the section matching `config.archetype` to understand:
- Which metrics matter for this client type
- What diagnostic patterns to apply
- How to frame insights and recommendations
- What red flag thresholds to use

This shapes the entire analysis approach.

**3. Ads Strategy** - `{client}/context/ads-strategy.md`

Contains: Campaign architecture, benchmarks, optimization priorities, funnel structure

Critical for: Strategic recommendations aligned with campaign goals

If missing: Proceed with data-only analysis, note in output.

**4. Memory** - `{client}/context/memory.md`

Contains: Key decisions, client preferences, historical learnings, quirks, **and Analysis Exclusions**

Critical for: Client-specific context that affects recommendations (e.g., "client prefers conservative scaling", past decisions, known sensitivities)

**Analysis Exclusions section:** If present, contains campaigns or metrics to exclude from performance analysis. These are intentionally underperforming items kept running for strategic/contractual reasons. Check this section before flagging campaigns as red/yellow.

If missing: Proceed but flag that historical context is unavailable.

**Recommended (Load if Available):**

**5. Media Plan** - `{client}/context/media-plan.md`

Contains: Budget allocation, forecasts, targets by channel/campaign

Used for: Budget pacing context and allocation decisions

**Load if More Context Needed:**

**6. Client Context** - `{client}/context/client-context.md`

Contains: Business overview, audience, brand positioning, contract scope

When to load: If analysis raises strategic questions, if recommending new initiatives, or if unfamiliar with the client

Note: Memory should capture key operational constraints from client context. Consult this for onboarding or deep strategic discussions, not routine weekly reviews.

---

### Step 3: Query BigQuery via `bash bq` Wrapper Script

**Load reference:** `references/bigquery-queries.md`

**Execute queries using the `bash bq` wrapper script** (run from project root `__kali_copilot/`):

**Primary method — use the built-in weekly-review command:**
```bash
bash bq weekly-review --client {client-slug}

# Custom date range
bash bq weekly-review --client {client-slug} --week-start {YYYY-MM-DD} --days 7
```

This returns per-platform breakdown, campaign-level metrics, totals with CTR/CVR, and budget/KPI targets — covering Queries 1-3 in a single call.

**For custom queries (platform comparison, video metrics, etc.):**
```bash
bash bq query --sql "WITH date_ranges AS (...) SELECT ... FROM \`kaliberasia.{dataset}.{table}\` ..."
```

**Always gather:**
1. **Weekly Performance Summary** - Overall metrics with WoW and 4-week avg
2. **Campaign Performance Breakdown** - Individual campaign metrics
3. **MTD Pacing Status** - Month-to-date spend by campaign

**Conditionally run (via `bash bq query --sql`):**
4. **Platform Comparison** - Only if client has multiple platforms in config
5. **Video Performance Metrics** - Only if client has video/awareness campaigns (check campaign names for "Awareness", "Video", "ThruPlay" in Query 2 results)

**Query execution:**
- Substitute placeholders with values from client config
- Run queries in parallel for speed
- Store results for analysis

**Error handling:**
- Table not found: Check config table name
- Column not found: Try fallback "conversions"
- No data: Verify date range and campaign status
- If query fails, diagnose and inform user

---

### Step 4: Analyze Performance Using Archetype Patterns

**References to use:**
- `references/analysis-criteria.md` - Base analysis criteria
- `.claude/context/archetypes.md` - Archetype-specific patterns (already loaded in Step 2)

**Apply archetype-specific analysis:**

The archetype (from config) determines:
- Which metrics to prioritize
- What thresholds to use for red/yellow/green
- How to frame insights
- What diagnostic patterns to apply

| Archetype | Primary Focus | Key Metrics |
|-----------|---------------|-------------|
| `lead_gen` | CPL efficiency | CPL, lead volume, CVR |
| `ecommerce` | ROAS | ROAS, revenue, AOV, transactions |
| `awareness` | Video engagement | VTR, CPV, reach, frequency |
| `full_funnel` | Funnel flow | Stage-by-stage metrics |

**Apply status indicators** based on archetype thresholds:

For `lead_gen` / `ecommerce`:
- 🟢 Green: Within ±10% of target CPA/ROAS
- 🟡 Yellow: Within ±10-25% of target
- 🔴 Red: Over ±25% of target

For `awareness`:
- 🟢 Green: VTR >25%, CPV <$0.05
- 🟡 Yellow: VTR 15-25%, CPV $0.05-0.10
- 🔴 Red: VTR <15%, CPV >$0.10, Frequency >4.0

For `full_funnel`:
- Status = weakest funnel stage
- Check stage-to-stage conversion rates

**Check Analysis Exclusions first:**
Before assigning status indicators, check `memory.md` for the Analysis Exclusions section. For excluded campaigns/metrics:
- Skip red/yellow flagging — do not treat as performance issues
- Still include data in reports for visibility
- Note as "Excluded per memory: [Reason]" in the campaign breakdown
- Do not recommend actions for excluded items

**For each campaign (not in exclusions):**
1. Identify which archetype metrics apply to this campaign
2. Compare actual performance to target from config
3. Apply archetype-specific thresholds
4. Assign status indicator (🟢🟡🔴)
5. Note WoW changes (±15% or more = significant)

**Identify patterns using archetype diagnostic patterns:**
- Campaigns with reds → Flag for deep-dive investigation using archetype diagnostic flows
- Campaigns with yellows → Note for monitoring
- Campaigns with greens → Highlight what's working
- Trends across 4-week comparison
- Cross-reference with archetype-specific warning signs

**Pacing analysis:**
```
Pacing Ratio = (MTD Spend % / Days Elapsed %)

Example:
- MTD Spend: $12,000 / $20,000 = 60%
- Days Elapsed: 15 / 30 = 50%
- Ratio: 60% / 50% = 1.20

Status:
- 0.90-1.10 = 🟢 On Pace
- 0.75-0.90 or 1.10-1.25 = 🟡 Slight under/overpacing
- <0.75 or >1.25 = 🔴 Severe under/overpacing
```

---

### Step 5: Deep Diagnosis for Red Flags

**Only when red status campaigns are identified:**

**Load:** `references/action-recommendations.md`

**Use for:**
- Prioritizing actions using priority matrix
- Selecting appropriate action type (bid, budget, creative, targeting, structural)
- Drafting specific action items with deadlines

**For each red flag campaign:**

1. **Dig to root cause** - Don't stop at "what", keep asking "why"
   - CPA spike? → Check CTR, CPC, conversion rate → Which layer broke? → Why did that layer break?
   - CTR decline? → Check creative age, frequency, audience saturation → What's the actual driver?
   - Volume drop? → Check impression share lost to budget vs rank → Why is rank declining?

   **Trace the causal chain:** metric moved → because of X → which happened because of Y → which means we should Z

2. **Generate hypothesis** - Using hypothesis template:
   ```
   "[Metric] is [direction] because [root cause], evidenced by [data].
   If we [proposed action], we expect [predicted outcome]."
   ```

   **If data doesn't reveal the cause:** Form a hypothesis based on reasoning, then recommend tests to validate it. "I don't know why" is never acceptable — "Here's my hypothesis and how we'd test it" is.

3. **Be honest about what's missing**
   - If data is insufficient to draw conclusions — say so explicitly
   - If hypothesis is formed on limited data — flag exactly what's missing
   - Distinguish clearly between "the data shows X" and "I'm inferring X because..."
   - Better to say "I can't give a confident recommendation without Y data" than to guess

4. **Draft recommended action** - Using action-recommendations.md:
   - Match symptom to action type (bid adjustment, creative rotation, etc.)
   - Create specific action item with task, rationale, expected outcome, deadline
   - Prioritize using priority matrix if multiple reds exist
   - **No cookie cutter suggestions** — every recommendation must be earned through analysis

**For yellow flags:**
- Brief note on what to monitor
- No deep-dive analysis required
- Explicitly state "monitoring only"

**For green performance:**
- Highlight what's working
- Note any opportunity to scale or replicate success

---

### Step 6: Generate Week-in-Review Analysis (for ANALYSIS WEEK)

**This output documents the Analysis Week** - the previous week's campaign performance that was just reviewed.

**Template Selection (Priority Order):**

1. **First, scan client-templates folder for custom template:**
   - Check `{client}/context/client-templates/` for any template files
   - Look for files like `report-template.md`, `week-in-review-template.md`, or similar
   - Any markdown file in this folder is treated as a potential custom template

2. **If client template found:** Use it as the primary template structure
   - Preserve client-specific sections, formatting, and structure
   - Apply the client's preferred metrics presentation
   - Maintain any client-specific attribution views (e.g., Platform + GA4 DD + GA4 LC)

3. **If no client template found (folder empty):** Fall back to standard template
   - Load: `references/week-in-review-template.md`

**Why client templates first:** Different clients have different reporting needs, attribution models, and stakeholder preferences. A custom template ensures the output matches what the client expects.

**Note:** The `client-templates/` folder is created empty during onboarding. Only add templates when the client has specific formatting needs different from the standard.

**Apply archetype-specific report framing:**

| Archetype | Lead With | Metrics Emphasis |
|-----------|-----------|------------------|
| `lead_gen` | "Generated X leads at $Y CPL" | CPL trends, lead volume, funnel efficiency |
| `ecommerce` | "Drove $X revenue at Y ROAS" | ROAS trends, revenue pacing, AOV analysis |
| `awareness` | "Reached X users with Y% VTR" | VTR benchmarks, frequency, reach growth |
| `full_funnel` | Stage-by-stage summary | Funnel flow, stage conversion rates |

**Process:**
1. Populate all sections with analyzed data from the **Analysis Week**
2. **Frame insights using archetype language** (from archetypes.md)
3. Include status indicators (🟢🟡🔴) for all campaigns
4. Add diagnostic reasoning for all red flags (using archetype diagnostic patterns)
5. Include hypotheses and recommended actions from Step 5
6. Reference client strategy for strategic recommendations
7. **Generate "Last Week's Actions: Status & Impact" section** (from `prior_review_context` — see below)
8. **Generate structured "Action Items" section** at the end (see below)

**6a. "Last Week's Actions: Status & Impact" section:**

Insert AFTER the Performance Overview campaign breakdown, BEFORE "Key Insights."

**If `prior_review_context.data_quality == "tracked"`:**

```markdown
## Last Week's Actions: Status & Impact

| # | Action (from W{Comparison} Review) | Priority | Status | Measured Impact |
|---|-------------------------------------|----------|--------|-----------------|
| 1 | {Action text} | P0 | ✅ Done {Day} | {Quantified impact from Analysis Week data} |
| 2 | {Action text} | P0 | ✅ Done {Day} | {Impact} |
| 3 | {Action text} | P1 | ⏳ Not done | {Why — pushed, blocked, deprioritized} |

**Completion Rate:** X/Y (XX%) — {breakdown by priority}

**Key Takeaways:**
- {What worked — connect completed actions to Analysis Week performance data}
- {What didn't get done and the consequence}
- {Patterns in execution}
```

**If `prior_review_context.data_quality == "untracked"`:**
- Use inference from BigQuery data (see Step 1.5 inference rules)
- Label clearly: "(inferred from spend/performance data — not confirmed via /log-action)"
- Still generate the section — inference is better than nothing

**If `prior_review_context.data_quality == "missing"`:**
- **Omit this section entirely.** Add a brief note: "First review for this client — feedback loop starts next cycle."

**Measuring impact:** Cross-reference completed actions with Analysis Week BigQuery data. Example:
- Action: "Pause underperforming Google campaigns" → Check if those campaigns show $0 spend in Analysis Week → If reallocated, check recipient campaigns' performance
- Action: "Scale Meta BAU to $500/day" → Check actual Meta BAU spend in Analysis Week → Check lead volume and CPL change

**6b. Structured "Action Items" section:**

Place at the **end** of the report, as the final section. This replaces the old "Next Week Priorities" section.

```markdown
## Action Items

### P0 - Critical
- [ ] {Action} — Due: {Day} — Status: ⏳
  - Campaign: {name}
  - Rationale: {data-driven why}
  - Expected: {quantified outcome}

### P1 - High Priority
- [ ] {Action} — Due: {Day} — Status: ⏳
  - Campaign: {name}
  - Rationale: {data-driven why}
  - Expected: {quantified outcome}

### P2 - Should Do
- [ ] {Action} — Due: {Day} — Status: ⏳

### P3 - Nice to Have
- [ ] {Action}

### Carrying Forward
- [ ] **[{Priority} — Overdue]** {Action from prior review} — *From W{Comparison}, delayed due to {reason}* — Due: {Day} — Status: ⏳
```

**"Carrying Forward" items** come from `prior_review_context.action_items` where status is ⏳ or ❌ (incomplete). These are re-prioritized and assigned new due dates.

**Keep "Strategic Recommendations"** in the body of the report for narrative context and rationale. But the **actionable items** are extracted into the structured Action Items section that machines (`log-action`, `pod-action-log`) can parse.

**Conditional sections based on config.reporting:**
- Include platform breakdown if `reporting.include_platform_breakdown: true`
- Include campaign breakdown if `reporting.include_campaign_breakdown: true`
- Include video metrics if `reporting.include_video_metrics: true`
- Include location breakdown if `reporting.include_location_breakdown: true`

**Quality standards:**
- All metrics populated with actual data (no placeholders)
- Every red flag has diagnosis + hypothesis + recommendation
- Yellow flags have monitoring notes
- Green performance highlights successes
- Strategic recommendations tied to client goals
- **Report framing matches archetype** (don't talk ROAS for lead_gen clients)
- **Feedback loop populated** when prior review data available
- **Action Items section is structured and parseable** — checkboxes, priorities, due dates, campaign references

**Save to:** `{client}/01_outputs/weekly-reports/[YYYY - WXX] - Week in Review.md`
- **WXX = Analysis Week number** (e.g., if analyzing W50, file is `2024 - W50 - Week in Review.md`)

**Format:** Complete markdown document following template structure exactly

---

### Step 7: Deliberation Checkpoint — User Review Before Committing Actions

**Purpose:** Pause for user review of analysis findings and proposed actions before pushing to the pod action log. The Week-in-Review serves as the deliberation document.

**CRITICAL: This is a mandatory checkpoint. Do NOT proceed to Step 8 until user explicitly approves.**

**Note:** When reviews are generated in automated mode, Steps 7-8 are skipped.
The AM reviews the output asynchronously, deliberates with Claudette, and uses
`/commit` to push the finalized action plan to the pod action log.

**Present to user:**

```markdown
# Weekly Review Analysis Complete — Awaiting Approval

**Client:** [Client Name]
**Analysis Week:** W[XX] ([Date Range])
**Planning Week:** W[XX] ([Date Range])

---

## Week-in-Review Report

**Written to:** `{client}/01_outputs/weekly-reports/[YYYY - WXX] - Week in Review.md`

This comprehensive analysis is available for your review. Key sections:
- Performance Overview (metrics vs. targets)
- Campaign Status Breakdown (🟢🟡🔴)
- Last Week's Actions: Status & Impact (feedback loop)
- Diagnosis of Red Flags
- Strategic Recommendations

---

## Proposed Action Items for Planning Week (W[XX])

Based on the analysis, here are the proposed actions to push to the pod action log:

### P0 - Critical (Must Do This Week)
- [ ] [Action 1] — [Campaign] — [Rationale]
- [ ] [Action 2] — [Campaign] — [Rationale]

### P1 - High Priority
- [ ] [Action 3] — [Campaign] — [Rationale]

### P2 - Should Do
- [ ] [Monitor 1] — [What to watch]

### P3 - Nice to Have
- [ ] [Backlog item]

### Carrying Forward (from prior week)
- [ ] [Overdue item] — *originally from W[XX-1]*

---

## Overall Health Assessment

**Proposed Status:** 🟢/🟡/🔴
**Rationale:** [1-2 sentence explanation]

---

## Questions / Adjustments Needed?

Before I push these actions to the pod action log and finalize the Week-in-Review, please review:

1. **Are the proposed actions correct?** (Add, remove, or modify?)
2. **Are priorities assigned correctly?** (Promote/demote any items?)
3. **Is the overall health assessment accurate?**
4. **Any context I'm missing that changes the plan?**

---

**Reply with:**
- "Approved" or "Looks good" — to push actions to the pod action log
- "[Your changes]" — to adjust actions before committing
- "Hold" — to pause and discuss further
```

**User responses:**

- **Approved:** Proceed to Step 8 to push actions to the pod action log
- **Changes requested:** Incorporate feedback, re-present the action plan for approval
- **Hold:** Wait for further discussion, do not proceed until user confirms

**Why this checkpoint matters:**
- The pod action log drives the team's weekly execution
- Actions committed should be deliberate, not auto-generated
- User may have context not captured in the analysis (client conversations, external factors)
- Prevents "garbage in, garbage out" from propagating to execution

---

### Step 8: Push Approved Actions to Pod Action Log (for PLANNING WEEK)

**PREREQUISITE: Only execute this step AFTER user approval in Step 7.**

**This step pushes approved action items to the pod-level action log** — a cross-client task board that lives at the pod root. It replaces the old Weekly Master Document generation.

**Process:**

**1. Determine pod path:**
- From client folder location (e.g., `delivery/pod-singapore/jal-yoga/` → pod = `delivery/pod-singapore/`)

**2. Construct log path:**
- `{pod}/pod-action-log/{YYYY} - W{Planning}.md`
- Where `{Planning}` = Planning Week number (the week being planned for)

**3. If file doesn't exist — create with header:**

```markdown
# Pod {Region} — Week {Planning Week Number} Action Log
**Week of:** {Mon Date} - {Sun Date}, {YYYY}
**Created:** {today's date}

---
```

**4. Append client section to the pod action log:**

```markdown
## {Client Name}
**Review:** [{Analysis Week} Week-in-Review]({relative path to Week-in-Review})
**Health:** 🟢/🟡/🔴
**Finalized:** {today's date}

### P0 - Critical
- [ ] {Action} — Due: {Day} — Status: ⏳
  - Campaign: {campaign name}
  - Rationale: {why}
  - Expected: {outcome}

### P1 - High Priority
- [ ] {Action} — Due: {Day} — Status: ⏳
  - Campaign: {campaign name}
  - Rationale: {why}
  - Expected: {outcome}

### P2 - Should Do
- [ ] {Action} — Due: {Day} — Status: ⏳

### P3 - Nice to Have
- [ ] {Action} — Due: {Day} — Status: ⏳

### Carrying Forward
- [ ] **[{Priority} — Overdue]** {Action} — *From W{Comparison}, delayed due to {reason}* — Due: {Day} — Status: ⏳

---
```

**5. Link to review:**
- Include relative path to the Week-in-Review file in the `**Review:**` line
- This is used by `/log-action` to find the correct review for backfilling

**If file already exists (another client was reviewed first this week):**
- Read the existing file
- Append the new client section at the end (before any trailing content)
- Do NOT overwrite existing client sections

**Action item format requirements:**
Every action pushed must have:
- Priority level (P0/P1/P2/P3)
- Checkbox `[ ]` for tracking
- Specific task (not vague "optimize")
- Target campaign/ad group
- Due day (Monday/Tuesday/etc. of Planning Week)
- Status indicator (`⏳` initially)
- For P0/P1: Rationale and Expected outcome

---

### Step 9: Present Completed Outputs to User

**Delivery format:**

```markdown
# Weekly Review Complete - [Client Name]

**Analysis Week:** Week [W-Analysis] (e.g., W50: Dec 9-15)
**Planning Week:** Week [W-Planning] (e.g., W51: Dec 16-22)
**Overall Status:** 🟢/🟡/🔴

---

## Week-in-Review Analysis (ANALYSIS WEEK: W[XX])

**What this is:** Comprehensive performance report with feedback loop and structured action plan.

Saved to: `{client}/01_outputs/weekly-reports/[YYYY - W{Analysis}] - Week in Review.md`

**Key Highlights:**
- [Summary of overall performance for Analysis Week]
- [Number of red/yellow/green campaigns]
- [Feedback loop: X/Y actions from prior week completed, key impact]
- [Top 2-3 new insights or actions]

[Show preview or key sections]

---

## Pod Action Log (PLANNING WEEK: W[XX])

**What this is:** Cross-client task board with this client's approved action items for the week ahead.

Updated: `{pod}/pod-action-log/[YYYY - W{Planning}].md`

**Actions pushed:**
- P0 - Critical: [count] items
- P1 - High Priority: [count] items
- P2 - Should Do: [count] items
- P3 - Nice to Have: [count] items
- Carrying Forward: [count] items

---

**Next Steps (for Planning Week W[XX]):**
1. [High-priority action flagged, if any]
2. Use `/log-action` as you complete actions during the week — this feeds the next review's feedback loop
3. Pod action log is the single source for tracking execution across clients

---

Files created/updated:
- Week-in-Review (Analysis Week W[XX]): [file path]
- Pod Action Log (Planning Week W[XX]): [file path]
```

---

## Workflow Variations

### Multiple Clients

**Command:** `"Run all weekly reviews"` or `"Weekly reviews for all clients"`

**Process:**
1. Identify all clients with BigQuery configs (scan `*/config/bigquery-config.json`)
2. Run complete workflow for each client sequentially
3. Present summary table:

| Client | Status | Spend | Conv. | CPA/ROAS | vs. Target | Key Issue |
|--------|--------|-------|-------|----------|------------|-----------|
| Client A | 🟢 | $X,XXX | XXX | $XX | -5% | None |
| Client B | 🟡 | $X,XXX | XXX | $XX | +18% | CPA elevation |
| Client C | 🔴 | $X,XXX | XXX | $XX | +45% | Creative fatigue |

4. Provide individual weekly reviews for each

### Custom Date Range

**Command:** `"Weekly review for [Client] - week of Jan 13-19"`

**Process:**
1. Use specified dates as the **Analysis Week** instead of default calculation
2. Calculate **Planning Week** as the week following Analysis Week
3. Adjust **Comparison Week** to the week before Analysis Week
4. Proceed with standard workflow

**Example:**
- User specifies: "week of Jan 13-19" (W03)
- Analysis Week: W03 (Jan 13-19) - the data being reviewed
- Planning Week: W04 (Jan 20-26) - actions for the following week
- Comparison Week: W02 (Jan 6-12) - for WoW analysis

### Quick Performance Check

**Command:** `"How did [Client] perform this week?"` or `"Quick check on [Client]"`

**Process:**
1. Execute Steps 1-4 only (load context, query data, analyze)
2. Present quick summary: key metrics, status, top issues
3. Offer to generate full weekly review if needed

---

## Error Handling

### BigQuery Failures

**Table not found:**
- Check config table name, verify in BigQuery console
- Inform user: "Table {table} not found - check config"

**Column not found:**
- Try fallback "conversions"
- If fails, inform user: "Conversion column '{column}' invalid - update config"

**No data returned:**
- Check date range (is data available?)
- Verify campaigns active during period
- Inform user: "No data for {dates} - campaigns may be paused"

### Missing Client Context

**Config missing:**
- Cannot proceed - alert user immediately
- "BigQuery config required at {path} - cannot access performance data"

**Strategy missing:**
- Proceed with data-only analysis
- Note in output: "Strategic recommendations limited - no strategy document"

**Other context missing:**
- Proceed without, note what's unavailable
- Skip relevant sections (e.g., no experiments log = no test outcomes)

### Incomplete or Invalid Data

**Partial week:**
- If mid-week, note data is incomplete
- Suggest waiting for full week or proceed with caveat

**Extreme outliers (>500% WoW change):**
- Flag as anomaly requiring investigation
- May indicate data quality issue or major event

**All zeros:**
- Check campaign status (paused?)
- Verify table has recent data
- Flag for user review

---

## Best Practices

1. **Always confirm date range with user** - Prevents confusion about week boundaries

2. **Load only needed context** - Config always, strategy if available, others as needed

3. **Query BigQuery directly** - Use the `bash bq` wrapper script for all data access

4. **Use config for all parameters** - Budgets, targets, conversion columns from config

5. **Be diagnostic, not descriptive** - Explain WHY metrics changed, not just THAT they changed

6. **Progressive loading for reds** - Only load action recommendations when red flags appear

7. **Checkpoint before committing** - Always pause at Step 7 for user approval before pushing to pod action log

8. **Specific action items** - Every red flag needs a corresponding action with clear deadline

9. **Status-driven depth** - Green/Yellow = monitor, Red = investigate & act

10. **Close the feedback loop** - Always load prior review in Step 1.5, reference what was planned vs. executed

11. **Structured action items** - Use the parseable format (checkboxes, priorities, due dates) so `/log-action` and pod action log can track them

---

## Integration with Campaign Management Routines

### Feeds Into (Downstream)

- **Pod Action Log** - Approved action items pushed to `{pod}/pod-action-log/` for cross-client tracking
- **`/log-action` Skill** - AMs log completed actions during the week, which updates both the pod log and the Week-in-Review
- **Next Week's Review** - This week's review (with action completion data) feeds the next review's "Last Week's Actions" section

### Pulls From (Upstream)

**Always:**
- **BigQuery Config** - Data access, parameters, targets, budgets, archetype
- **Archetypes Definition** - Analysis patterns and thresholds by client type
- **Ads Strategy** - Campaign architecture, benchmarks, priorities
- **Memory** - Client preferences, historical learnings
- **BigQuery Tables** - Live performance data

**If available:**
- **Media Plan** - Budget allocation, forecasts

**If more context needed:**
- **Client Context** - Business overview, audience, brand positioning

### References (Support Documents)

- **Archetypes** - Client type definitions with analysis patterns, metrics, thresholds (`.claude/context/archetypes.md`)
- **Action Recommendations** - Loaded only when red flags appear (optimization action types and prioritization)
- **Analysis Criteria** - Base analysis criteria (green/yellow/red thresholds)
- **BigQuery Queries** - Query templates for `bash bq` execution

---

## Resources

### .claude/context/archetypes.md
Client archetype definitions — the playbook for how to analyze each client type.

**Load when:** After loading config (Step 2) — read the section matching `config.archetype`

**Contains:**
- Four archetypes: lead_gen, ecommerce, awareness, full_funnel
- Metrics that matter for each
- Diagnostic patterns (what to check when things go wrong)
- Report framing guidance
- Red flag thresholds

This skill also includes reference documents in the `references/` directory:

### Client-Specific Report Templates (Priority)
Custom report templates in client folders take precedence over the standard template.

**Scan location:**
- `{client}/context/client-templates/` folder

**What to look for:**
- Any markdown file in the folder (e.g., `report-template.md`, `week-in-review-template.md`)
- Folder is created empty during onboarding — add templates only when needed

**When found:** Use client template structure, preserve their formatting, attribution views, and section organization.

**When empty:** Fall back to standard template at `references/week-in-review-template.md`

**Example:** Calecim uses geography-first structure with multi-attribution (Platform + GA4 DD), which differs from the standard template — their custom template lives in `context/client-templates/report-template.md`.

### references/week-in-review-template.md
Standard template for week-in-review analysis. **Used only when no client template exists.**

**Load when:** No client-specific template found in Step 6

**Contains:**
- Full analysis structure with all sections
- Performance overview tables
- Optimizations executed, test outcomes
- Key insights, strategic recommendations
- Next week priorities

### Prior Week's Week-in-Review
The previous cycle's Week-in-Review, which contains action items with completion status.

**Load when:** Step 1.5 — feedback loop loading

**Search paths:**
1. `{client}/01_outputs/weekly-reports/{YYYY} - W{Comparison} - Week in Review.md`

**Extract:** Action items (with status), strategic recommendations, active tests

### Pod Action Log
Cross-client task board at pod level. Created/updated in Step 8.

**Location:** `{pod}/pod-action-log/{YYYY} - W{Planning}.md`

**Created by:** Weekly review Step 8 (first client review of the week creates the file)
**Updated by:** `/log-action` skill during the week, subsequent client reviews append sections

### references/bigquery-queries.md
Query templates for pulling performance data via `bash bq` wrapper script.

**Load when:** Executing queries (Step 3)

**Contains:**
- All 4 query templates with placeholder substitution guide
- Query execution strategy
- Error handling patterns
- Example complete query execution

### references/analysis-criteria.md
Performance evaluation thresholds and diagnostic patterns.

**Always load:** Core analysis criteria used in Step 4

**Contains:**
- Status indicator thresholds (green/yellow/red)
- WoW change interpretation
- Pacing status calculation
- Diagnostic patterns by symptom (CPA spike, CTR decline, volume drop, etc.)
- Campaign priority classification
- Metrics significance thresholds

### references/action-recommendations.md
Actionable optimization steps and action types for addressing red flag campaigns.

**Load when:** Red status campaigns (🔴) identified requiring action planning (Step 5)

**Contains:**
- Priority matrix for action prioritization
- 5 optimization action types (bid, budget, creative, targeting, structural)
- When to use each action type and best practices
- Action selection guide matching symptoms to actions
- Action item template with required components
- Hypothesis-driven action planning

---

**Skill Version:** 4.0.1
**Last Updated:** 2026-02-20
**Maintained By:** Kaliber Group

---

## Changelog

### v4.0.2 (2026-02-23)
- **`/commit` skill integration:** Automated Mode section now documents the intended follow-up workflow
  - Added "After automated reviews are generated" block with `/commit` instructions
  - Step 7 updated with note that automated reviews use `/commit` instead of Steps 7-8
  - Closes the gap where automated reviews had no path to the pod action log

### v4.0.1 (2026-02-20)
- **BigQuery MCP references removed:** Updated all references from `mcp__bigquery__query` to the `bash bq` wrapper script
  - Step 3 rewritten to use `bash bq weekly-review --client` as primary data access method
  - Custom queries now use `bash bq query --sql` instead of MCP tool invocation
  - `bigquery-queries.md` reference updated with new execution examples
  - Aligns skill documentation with actual infrastructure (direct API via Python script)

### v4.0.0 (2026-02-13)
- **Feedback loop architecture:** Weekly reviews now close the loop on prior week's actions
  - New **Step 1.5:** Loads prior Week-in-Review to check action completion status
  - New **"Last Week's Actions: Status & Impact"** section in reports (Step 6)
  - Inference engine: When `/log-action` wasn't used, infers action completion from BigQuery data changes
  - Graceful degradation: `tracked` → `untracked` → `missing` with clear labeling
- **Pod Action Log replaces Weekly Master Document:**
  - **Step 8 rewritten:** Pushes approved actions to `{pod}/pod-action-log/{YYYY} - W{XX}.md`
  - Cross-client task board at pod level (one file per week, multiple clients)
  - `/log-action` skill updates both pod log and Week-in-Review
  - Incomplete actions carry forward to next week's review
- **Structured Action Items section:** Replaces scattered "Next Week Priorities" / "Strategic Recommendations"
  - Parseable format: checkboxes, priority levels, due dates, campaign references
  - Machine-readable by `/log-action` and session-wrap agent
  - "Carrying Forward" subsection for overdue items from prior week
- **Template updated:** `week-in-review-template.md` restructured with new section order
  - Removed: "Optimizations Executed This Week", "Project Work Completed", "Next Week Priorities"
  - Added: "Last Week's Actions: Status & Impact", "Action Items" (structured)
- **Step 9 updated:** Shows pod action log path and action count instead of Weekly Master Doc
- **Integration updated:** Documents feedback loop flow with `/log-action` skill

### v3.4.0 (2026-01-14)
- **Deliberation checkpoint added:** New Step 7 introduces a mandatory user approval checkpoint between analysis and Weekly Master Gameplan generation
  - Week-in-Review report (Step 6) serves as the deliberation document
  - Proposed action plan presented to user for review before committing
  - User can approve, request changes, or hold
  - Weekly Master Document (Step 8) only written after explicit approval
- **Step renumbering:** Old Step 7 → Step 8, Old Step 8 → Step 9
- **Memory promoted to critical:** `memory.md` moved from "Recommended" to "Critical (Always Load)" — historical context is essential for informed recommendations
- **Client Context demoted:** Moved to new "Load if More Context Needed" section — memory should capture key constraints; full context only needed for strategic questions or unfamiliar clients
- **Action Log removed:** No longer loaded by default
- **Why:** Ensures action plans are deliberate and incorporate user context before being committed to execution documents

### v3.3.0 (2026-01-14)
- **Template scanning updated:** Now scans `{client}/context/client-templates/` folder instead of multiple locations
  - Single, dedicated folder for all client customizations
  - Folder created empty during onboarding — add templates only when needed
  - Falls back to standard template when folder is empty
- **Resources section updated:** Simplified template scanning documentation

### v3.2.0 (2026-01-13)
- **Client-specific template support:** Step 6 now scans client folder for custom report templates before falling back to standard template
  - Checks `{client}/context/report-template.md`, `{client}/config/report-template.md`, and any `*report*template*.md`
  - Preserves client-specific structure, attribution views, and formatting
  - Standard template only used when no client template found
- **Resources section updated:** Added Client-Specific Report Templates documentation

### v3.1.0 (2026-01-09)
- **Archetype-driven analysis:** Config now includes `archetype` field that shapes entire analysis
  - Step 2 updated to load archetypes.md after config
  - Step 4 renamed to "Analyze Performance Using Archetype Patterns"
  - Step 6 updated with archetype-specific report framing
  - Added archetype thresholds table for status indicators
- **Config schema v2:** Added required fields: `archetype`, `primary_kpi`, `reporting`
- **Resources updated:** Added archetypes.md reference documentation

### v3.0.0 (2026-01-09)
- **Client folder restructure:** Updated all file paths to match new simplified architecture
  - Context files now flat under `{client}/context/` (removed context-static/, context-dynamic/ subfolders)
  - Removed experiments-log.md and roadmap.md references (deprecated)
  - Added client-context.md, media-plan.md, memory.md to context loading
  - Updated routines path from `00_routines/` to `routines/`
- **Claudette operating principles integration:**
  - Step 5 rewritten as "Deep Diagnosis for Red Flags" — emphasizes root cause analysis
  - Added causal chain tracing methodology
  - Added intellectual honesty requirements (flag missing data, distinguish data vs inference)
  - Explicit "no cookie cutter recommendations" guidance

### v2.2.0 (2025-12-22)
- Added **Step 0: Get Current Date from System** to hardcode today's date using bash command
- Ensures accurate date calculations by using system date as source of truth
- Prevents date confusion in week calculations

### v2.1.0 (2025-12-19)
- Added explicit distinction between **Analysis Week** (previous week being reviewed) and **Planning Week** (current week being planned)
- Updated Step 1 with clear date context examples for Monday vs Friday scenarios
- Clarified output file naming: Week-in-Review uses Analysis Week number, Weekly Master Doc uses Planning Week number
- Updated Steps 6, 7, and 8 with clear week context labels


---
*Built by [Kaliber](https://kalibergroup.netlify.app) — Singapore's AI-native marketing group*
