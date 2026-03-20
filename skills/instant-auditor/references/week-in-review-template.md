# Week-in-Review Template (Internal Analysis)

**Purpose:** Comprehensive internal performance analysis for agency use. This is the detailed analysis that informs action planning and the feedback loop between weekly reviews.

**Timing:** Friday afternoon, after week's data is complete

**Audience:** Internal team (account managers, strategists)

---

## Template

```markdown
# Week in Review - [Client Name] - Week [WXX] ([Date Range])

## Executive Summary
[2-3 sentence overview: performance vs. goals, key wins, critical issues]

## Performance Overview

### Key Metrics (WoW Comparison)
| Metric | This Week | Last Week | WoW Change | 4-Week Avg | vs. Avg |
|--------|-----------|-----------|------------|------------|---------|
| Spend | $X,XXX | $X,XXX | +X% | $X,XXX | +X% |
| Impressions | XXX,XXX | XXX,XXX | +X% | XXX,XXX | +X% |
| Clicks | X,XXX | X,XXX | +X% | X,XXX | +X% |
| Conversions | XXX | XXX | +X% | XXX | +X% |
| CTR | X.XX% | X.XX% | +X% | X.XX% | +X% |
| CPC | $X.XX | $X.XX | +X% | $X.XX | +X% |
| CPA / ROAS | $XXX / X.XX | $XXX / X.XX | +X% | $XXX / X.XX | +X% |

### Campaign Performance Breakdown
| Campaign | Spend | Conv. | CPA/ROAS | vs. Target | Status |
|----------|-------|-------|----------|------------|--------|
| [Campaign 1] | $XXX | XX | $XX / X.X | +X% | 🟢/🟡/🔴 |
| [Campaign 2] | $XXX | XX | $XX / X.X | +X% | 🟢/🟡/🔴 |

**Status Indicators:**
- 🟢 On target / performing well (within 10% of target)
- 🟡 Caution / slight concern (within 25% of target)
- 🔴 Issue / needs attention (over 25% of target)

### Video/Awareness Campaign Performance (if applicable)
| Campaign | Spend | Impr. | Plays | ThruPlays | VTR | CPV | Cost/TP | Status |
|----------|-------|-------|-------|-----------|-----|-----|---------|--------|
| [Awareness Campaign] | $XXX | XXX,XXX | XX,XXX | XX,XXX | XX% | $X.XX | $X.XX | 🟢/🟡/🔴 |

**Video Engagement Funnel:**
- **Video View Rate (VTR):** X.X% (plays/impressions) | WoW: +X%
- **25% completion:** XX,XXX (XX% of plays)
- **50% completion:** XX,XXX (XX% of plays)
- **75% completion:** XX,XXX (XX% of plays)
- **95% completion:** XX,XXX (XX% of plays)
- **100% completion:** XX,XXX (XX% of plays)
- **ThruPlay rate:** XX% of plays | WoW: +X%

**Video Status Indicators:**
- 🟢 Strong engagement: VTR >25%, Cost per ThruPlay <$0.05
- 🟡 Moderate engagement: VTR 15-25%, Cost per ThruPlay $0.05-0.10
- 🔴 Weak engagement: VTR <15%, Cost per ThruPlay >$0.10

## Last Week's Actions: Status & Impact

<!-- This section is populated by Step 1.5 of the weekly review workflow.
     It reads the PRIOR week's review to see what actions were planned and whether they were completed.
     If this is the first review for this client, this section is omitted.
     If actions were tracked (via /log-action), use completion data directly.
     If actions were untracked, infer from BigQuery data changes. -->

| # | Action (from W[XX-1] Review) | Priority | Status | Measured Impact |
|---|------------------------------|----------|--------|-----------------|
| 1 | [Action from prior review] | P0 | ✅ Done [Day] | [Quantified impact from this week's data] |
| 2 | [Action from prior review] | P0 | ✅ Done [Day] | [Quantified impact] |
| 3 | [Action from prior review] | P1 | ⏳ Not done | [Why — pushed, blocked, deprioritized] |
| 4 | [Action from prior review] | P2 | ✅ Done [Day] | [Quantified impact] |

**Completion Rate:** X/Y (XX%) — [summary: Xx P0 completed, Xx P1 missed, etc.]

**Key Takeaways:**
- [What worked from last week's actions — connect to this week's data]
- [What didn't get done and the consequence]
- [Patterns: are actions being executed? are they having the expected impact?]

## Key Insights

### What Worked
- [Insight 1: e.g., "New ad creative variant improved CTR by 32% in SG Buy campaign"]
- [Insight 2: e.g., "Expanded match keywords drove 15% more conversions at -8% CPA"]

### What Didn't Work
- [Insight 1: e.g., "Audience expansion test increased volume but CPA +45% over target"]
- [Insight 2: e.g., "Evening dayparting bid increase had no impact on conversion rate"]

### Trends & Observations
- [Trend 1: e.g., "CTR declining across all campaigns - potential market saturation"]
- [Trend 2: e.g., "Mobile conversion rate improving WoW (+12%) - consider mobile-specific creatives"]

## Issues & Risks
- [Issue 1: e.g., "Quality Score dropped from 7 to 5 in top-performing ad group - investigating"]
- [Issue 2: e.g., "Impression share lost to budget at 18% - recommend +$500/week increase"]

## Strategic Recommendations
1. **[Recommendation 1]** - Priority: High / Medium / Low
   - Context: [Why this matters]
   - Action: [What to do]
   - Expected Impact: [Quantified outcome]

2. **[Recommendation 2]**
   - Context:
   - Action:
   - Expected Impact:

## Active Tests

### Concluded Tests
**Test #XX: [Test Name]**
- Hypothesis: [What we expected]
- Duration: [Days]
- Result: ✅ Success / ❌ Failed / ⚠️ Inconclusive
- Data: [Key metrics]
- Decision: [Scale winner / Iterate / Abandon]

### Active Tests (Ongoing)
- Test #XX: [Name] - Day X of Y | Status: [Leading variant / Too early to call]

---

**Monthly Pacing Status**
- MTD Spend: $X,XXX / $X,XXX budget (XX%)
- Days Elapsed: XX / 30 (XX%)
- Pacing: On Track / Underpacing / Overpacing

## Action Items

<!-- This is the structured, parseable action plan for the Planning Week.
     These items get pushed to the pod action log by Step 8 of the weekly review.
     AMs update these via /log-action during the week.
     The next weekly review reads completion data from here to close the feedback loop. -->

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
<!-- Items from prior review's incomplete actions (status ⏳ or ❌) -->
- [ ] **[P1 — Overdue]** {Action from prior week} — *From W[XX-1], delayed due to {reason}* — Due: {Day} — Status: ⏳
```

---

## Data Sources for This Analysis

1. **BigQuery Performance Data:**
   - Weekly summary metrics (WoW, 4-week avg)
   - Campaign-level breakdown
   - Platform comparison (if multi-platform)

2. **Prior Week's Week-in-Review** (for feedback loop):
   - Action items with completion status
   - Strategic recommendations
   - Active tests

3. **Client Strategy/Roadmap:**
   - Strategic goals and KPIs
   - Current initiatives and priorities
   - Target CPA/ROAS by campaign

4. **Optimization Framework:**
   - Best practices for recommendations
   - Diagnostic patterns for issues
   - Action prioritization criteria

---

## Section Order Reference

The template follows this section order:

1. **Executive Summary** — headline performance and critical issues
2. **Performance Overview** — metrics tables, campaign breakdown, video (if applicable)
3. **Last Week's Actions: Status & Impact** — feedback loop from prior review's action items
4. **Key Insights** — What Worked / What Didn't / Trends
5. **Issues & Risks** — flagged problems requiring attention
6. **Strategic Recommendations** — narrative context and rationale for recommendations
7. **Active Tests** — concluded and ongoing experiments
8. **Monthly Pacing Status** — budget tracking
9. **Action Items** — structured, parseable action plan for the Planning Week

---

## Notes on Analysis Quality

**Be specific:** Don't just say "performance improved" - quantify with percentages and context.

**Be diagnostic:** When issues arise, identify the layer (campaign/ad group/keyword/creative) and propose specific actions.

**Be strategic:** Connect performance to business goals. Reference the client's strategy document when making recommendations.

**Be honest:** Don't hide underperformance. Frame it constructively with clear action plans.

**Close the loop:** Always reference what was planned vs. what was executed. The feedback loop is what makes this system improve over time.
