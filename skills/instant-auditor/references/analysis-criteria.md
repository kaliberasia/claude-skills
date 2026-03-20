# Analysis Criteria for Weekly Review

**Purpose:** Performance evaluation thresholds and diagnostic patterns for weekly analysis.

**Source:** Extracted from optimization framework - core criteria always loaded, action recommendations loaded only when needed.

---

## Status Indicators (Green/Yellow/Red)

### Campaign-Level Performance Status

**Based on actual vs. target CPA/ROAS:**

| Status | Criteria | Meaning | Action Required |
|--------|----------|---------|-----------------|
| 🟢 Green | Within ±10% of target | On target, performing well | Monitor |
| 🟡 Yellow | Within ±10-25% of target | Slight concern, watchlist | Monitor closely |
| 🔴 Red | Over ±25% of target | Needs attention | Investigate & act |

**Examples:**
- Target CPA: $50
  - 🟢 Actual: $45-55 (within 10%)
  - 🟡 Actual: $38-45 or $55-62.50 (10-25% off)
  - 🔴 Actual: <$38 or >$62.50 (over 25% off)

**Note:** Being under target CPA can also be 🔴 if volume is severely limited (losing impression share, constrained by budget).

---

### WoW Change Thresholds

**For week-over-week metric changes:**

| Change Magnitude | Status | Interpretation |
|------------------|--------|----------------|
| ±0-5% | Normal | Typical weekly fluctuation |
| ±5-15% | Notable | Worth mentioning, may indicate trend |
| ±15-30% | Significant | Requires explanation |
| ±30-100% | Major | Investigate cause immediately |
| >±100% | Anomaly | Flag for investigation, likely data issue or major event |

---

### Pacing Status

**Based on MTD spend vs. budget:**

```
Pacing Status = (MTD Spend % / Days Elapsed %)

Expected pacing = MTD spend % should equal days elapsed %
```

| Status | Criteria | Meaning |
|--------|----------|---------|
| 🟢 On Pace | 0.90 - 1.10 ratio | Spending evenly through month |
| 🟡 Slight Underpacing | 0.75 - 0.90 ratio | Behind pace, may need adjustment |
| 🟡 Slight Overpacing | 1.10 - 1.25 ratio | Ahead of pace, monitor budget |
| 🔴 Severe Underpacing | <0.75 ratio | Significantly behind, investigate |
| 🔴 Severe Overpacing | >1.25 ratio | At risk of exhausting budget early |

**Example:**
- Days elapsed: 15 / 30 = 50% of month
- MTD spend: $12,000 / $20,000 budget = 60% of budget
- Ratio: 60% / 50% = 1.20 (🟡 Slight overpacing)

---

## Diagnostic Patterns by Symptom

**Use these patterns to identify root cause layer when red flags appear.**

### CPA Spike (Worse Than Target)

**Likely causes:**

1. **Campaign/Bid Strategy Layer**
   - Bid strategy changed or learning period
   - Budget constraints causing erratic bidding
   - Quality Score declined

2. **Ad Group/Creative Layer**
   - Creative fatigue (CTR declining)
   - Audience saturation
   - Competitive pressure increased

3. **Landing Page/Conversion Layer**
   - LP changes or tracking issues
   - Conversion rate drop (check if clicks maintained but conversions down)

**Investigation steps:**
- Check if CPA spike is across all campaigns or isolated to specific one(s)
- Compare CTR, CPC, conversion rate to identify which funnel stage broke
- Review recent changes (bid adjustments, creative rotations, LP updates)

---

### CTR Decline

**Likely causes:**

1. **Creative Fatigue**
   - Creative running >30-45 days
   - Frequency >3 in target audience
   - Engagement metrics declining

2. **Audience Saturation**
   - Same audience targeted for extended period
   - Impression share high but CTR declining
   - Reach plateauing

3. **Competitive Pressure**
   - New competitors in auction
   - Ad rank declining despite stable bids
   - Impression share lost to rank

**Investigation steps:**
- Check creative age and frequency
- Review impression share metrics (lost to rank vs budget)
- Analyze audience overlap and saturation

---

### Volume Drop (Impressions/Clicks Down)

**Likely causes:**

1. **Budget Constraints**
   - Impression share lost to budget increasing
   - Daily budget exhaustion earlier in day
   - Campaigns paused mid-month

2. **Bid Competitiveness**
   - Bids too low for target positions
   - CPC increasing, volume declining
   - Impression share lost to rank

3. **Targeting Too Narrow**
   - Recent targeting restrictions
   - Audience exclusions limiting reach
   - Keyword pauses reducing coverage

**Investigation steps:**
- Check impression share lost to budget vs rank
- Review recent targeting changes
- Compare bid levels to average CPC

---

### Conversion Rate Drop

**Likely causes:**

1. **Landing Page Issues**
   - Recent LP changes
   - Page load speed degradation
   - Mobile vs desktop experience gap

2. **Audience Quality Shift**
   - Targeting expansion brought lower-quality traffic
   - Device mix changed (mobile typically lower CVR)
   - Geographic mix changed

3. **Tracking Issues**
   - Conversion tag broken
   - Attribution window changed
   - Platform reporting delays

**Investigation steps:**
- Check if clicks maintained but conversions dropped (isolates to LP/tracking)
- Review device, geography, audience segment performance
- Validate conversion tracking is firing

---

## Trend Identification

**Look for these patterns in 4-week comparison:**

### Positive Trends (Worth Scaling)

- **Efficiency improving:** CPA declining over 4 weeks while volume maintained
- **Volume growing sustainably:** Impressions/clicks up while CPA stable or improving
- **Engagement strengthening:** CTR trending up, creative resonating better

### Negative Trends (Needs Intervention)

- **Efficiency degrading:** CPA creeping up over multiple weeks
- **Volume declining:** Impressions/clicks trending down despite stable budget
- **Engagement weakening:** CTR declining over time (creative fatigue, saturation)

### Seasonal/Cyclical Patterns

- **Day-of-week effects:** Certain days consistently perform better/worse
- **Month-end effects:** Performance changes in last week of month
- **Seasonal shifts:** Performance aligns with known seasonal patterns (holidays, etc.)

---

## Campaign Priority Classification

**From config - use to prioritize deep-dive analysis:**

| Priority | Meaning | Analysis Depth |
|----------|---------|----------------|
| P0 | Critical revenue drivers | Deep-dive if any yellow/red flags |
| P1 | Important, growth initiatives | Investigate reds, note yellows |
| P2 | Pilots, low priority | Monitor reds, may pause if underperforming |

**When multiple campaigns have issues, triage by priority:**
- Reds in P0 campaigns → Immediate investigation required
- Reds in P1 campaigns → Investigate if budget/time allows
- Reds in P2 campaigns → Note issue, consider pausing if severe

---

## Metrics Significance Thresholds

**Minimum data requirements for statistical significance:**

| Metric | Minimum for Significance | Reason |
|--------|--------------------------|--------|
| CTR change | 50+ impressions | Avoid noise from small samples |
| Conversion rate change | 30+ clicks | Need sufficient click volume |
| CPA comparison | 10+ conversions | CPA highly volatile with <10 conv |
| WoW performance | Full 7-day week | Avoid day-of-week effects |

**If below thresholds:** Note in analysis as "insufficient data for meaningful comparison"

---

## Analysis Workflow Integration

### Green/Yellow Status → Monitor Mode

**What to include in analysis:**
- Highlight what's working (green campaigns)
- Note what to watch (yellow campaigns)
- No deep-dive investigation needed
- Focus on trends and opportunities to scale

**Example output:**
> "SG Buy campaign performing well (🟢 CPA $47 vs $50 target). MY/ID Buy showing slight CPA elevation (🟡 $62 vs $50 target, +24%) - monitoring trend, no action required yet."

### Red Status → Investigation Mode

**Trigger progressive loading:**
- Load `.claude/context/routines/reference/optimization-framework.md`
- Use diagnostic patterns (above) to identify layer
- Generate hypotheses using optimization framework's hypothesis template
- Draft recommended actions using framework's action types

**Example output:**
> "Meta Prospecting campaign flagged (🔴 CPA $82 vs $50 target, +64%).
>
> **Diagnosis:** CPA spike isolated to this campaign. CTR down 28% WoW, CPC stable - indicates creative fatigue at ad level.
>
> **Hypothesis:** Primary creative running 42 days with frequency 3.8. If we rotate in new creative variant (from library), expect CTR recovery to 2.0%+ and CPA back to target within 5-7 days.
>
> **Recommended Action:** Pause current creative, activate Creative #23 (tested in SG market, proven CTR 2.4%). Monitor daily for 3 days."

---

## Quality Standards for Analysis

**Every red flag must have:**
1. **Status indicator** - 🔴 with variance % from target
2. **Diagnosis** - Which layer (campaign/creative/LP) and which metric broke
3. **Hypothesis** - Root cause theory with supporting data
4. **Recommendation** - Specific, actionable next step with expected outcome

**Yellow flags should have:**
1. **Status indicator** - 🟡 with variance % from target
2. **Note** - Brief context on what to monitor
3. **No action required** - Explicitly state monitoring only

**Green performance should highlight:**
1. **Status indicator** - 🟢 with performance vs target
2. **What's working** - Brief note on success factor
3. **Opportunity** - Any chance to scale or replicate success

---

## Video/Awareness Campaign Status Indicators

**Applies to:** Meta Awareness (ThruPlay), YouTube video campaigns, video-optimized campaigns

### Video Performance Status Thresholds

| Status | Criteria | Meaning | Action Required |
|--------|----------|---------|-----------------|
| 🟢 Green | VTR >25% AND Cost/ThruPlay <$0.05 | Strong engagement | Monitor, consider scaling |
| 🟡 Yellow | VTR 15-25% OR Cost/ThruPlay $0.05-0.10 | Moderate engagement | Monitor, prepare creative refresh |
| 🔴 Red | VTR <15% OR Cost/ThruPlay >$0.10 | Weak engagement | Investigate & refresh creative |

**VTR = Video View Rate (video plays / impressions)**

**ThruPlay = Videos watched to completion or at least 15 seconds**

---

### Video Completion Funnel Benchmarks

**Healthy completion funnel (Meta):**

| Completion % | Benchmark | Interpretation |
|-------------|-----------|----------------|
| **25% completion** | >70% of plays | Hook is working (first 3-5 seconds) |
| **50% completion** | >40% of plays | Mid-section holds attention |
| **75% completion** | >25% of plays | Strong narrative/value prop |
| **95% completion** | >15% of plays | Highly engaged audience |
| **100% completion** | >10% of plays | Excellent creative quality |

**ThruPlay rate:** >30% of plays (15s+ watch or completion)

**Drop-off diagnosis:**
- **High 25%, low 50%:** Hook works, but mid-section loses interest
- **High 50%, low 75%:** Strong first half, weak second half or too long
- **All low:** Poor creative quality, wrong audience, or ad fatigue

---

### Video Campaign Diagnostic Patterns

#### VTR Declining WoW (>15% drop)

**Likely causes:**

1. **Creative Fatigue**
   - Creative running >14 days with high frequency (>3)
   - Audience saturation (same people seeing repeatedly)
   - Solution: Rotate new creative, expand audience

2. **Hook Weakness**
   - First 3 seconds not grabbing attention
   - Wrong opening frame or messaging
   - Solution: Test new hooks, analyze 3-second video plays

3. **Audience Mismatch**
   - Targeting too broad or misaligned with creative
   - Interest targeting not resonating
   - Solution: Refine targeting, test new LAL audiences

4. **Competitive Pressure**
   - Similar competitors flooding same audience
   - CPM increasing alongside VTR decline
   - Solution: Differentiate creative angle, explore new audiences

**Investigation steps:**
- Check creative age and frequency metrics
- Compare VTR across different audiences/placements
- Review completion funnel to identify drop-off point
- Check CPM trends (if CPM rising + VTR declining = competitive pressure)

---

#### High VTR but Low ThruPlay Rate

**Symptom:** VTR >25% but ThruPlay <20% of plays

**Likely causes:**

1. **Strong hook, weak payoff**
   - First 3-5 seconds grab attention, but rest doesn't deliver
   - Solution: Tighten narrative, cut to 15s if currently longer

2. **Video too long for placement**
   - 30s+ videos on mobile placements
   - Solution: Test 15s edits, optimize for mobile-first

3. **No clear CTA or value prop**
   - People watch but don't see reason to complete
   - Solution: Front-load value proposition, add mid-roll engagement hook

---

#### Cost per ThruPlay Increasing

**Symptom:** Cost per ThruPlay rising >20% WoW, even if VTR stable

**Likely causes:**

1. **Audience saturation**
   - Exhausted LAL audience or retargeting pool
   - Frequency >3.5 on awareness campaign
   - Solution: Expand LAL %, add new seed audiences, increase budget to reach new users

2. **Bid/budget constraints**
   - Budget too low to reach fresh audience
   - Bid cap restricting delivery
   - Solution: Increase budget or remove bid cap for awareness campaigns

3. **Platform competition**
   - CPM increasing across all video campaigns
   - Seasonal or competitive pressure
   - Solution: Accept higher cost if volume maintained, or shift budget timing

---

### Video Campaign WoW Change Thresholds

**For video-specific metrics:**

| Change Magnitude | Status | Interpretation |
|------------------|--------|----------------|
| ±0-10% VTR change | Normal | Typical weekly fluctuation |
| ±10-20% VTR change | Notable | Watch closely, may indicate trend |
| ±20-35% VTR change | Significant | Investigate creative or audience fatigue |
| >±35% VTR change | Critical | Likely creative fatigue or audience saturation - act immediately |

**ThruPlay volume changes follow standard WoW thresholds (±15% = significant).**

---

### Video Campaign Priority in Analysis

**Green/Yellow Status (VTR >15%, Cost/ThruPlay <$0.10):**
- Brief note on performance
- Highlight completion funnel if exceptionally strong
- Mention WoW trends if notable (>10% change)
- No deep-dive needed

**Red Status (VTR <15% OR Cost/ThruPlay >$0.10):**
- Full diagnostic using patterns above
- Identify drop-off point in completion funnel
- Generate hypothesis (creative fatigue, audience saturation, etc.)
- Draft action (creative refresh, audience expansion, etc.)

---

### Integration with Conversion Campaigns

**When analyzing awareness + conversion campaigns together:**

1. **Funnel flow analysis:**
   - Are awareness ThruPlays driving retargeting pool growth?
   - Check retargeting campaign audience size week-over-week
   - If retargeting pool growing but conversions flat → retargeting creative issue
   - If retargeting pool flat → awareness not feeding funnel

2. **Budget allocation:**
   - If awareness VTR strong (>25%) but retargeting underperforming → shift budget down-funnel
   - If awareness weak but retargeting strong → reduce awareness, focus on warm audiences
   - If both strong → opportunity to scale both

3. **Creative consistency:**
   - Awareness creative should align with retargeting messaging
   - If awareness uses humor/emotion, retargeting should continue that tone
   - Mismatched messaging creates drop-off between awareness → retargeting

---

## Notes

- **Use config targets, not arbitrary thresholds** - Every client has different target CPA/ROAS
- **Context matters** - A 20% CPA increase might be red for one client, acceptable for another (depends on target and business model)
- **Volume + efficiency trade-off** - Sometimes CPA slightly above target is acceptable if volume is strong (check with strategy)
- **Be diagnostic, not just descriptive** - Always explain WHY metrics changed, not just THAT they changed
- **Video metrics are leading indicators** - VTR and ThruPlay issues today become conversion issues tomorrow (awareness feeds retargeting pool)
