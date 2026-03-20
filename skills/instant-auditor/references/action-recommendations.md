# Action Recommendations for Campaign Optimization

**Purpose:** Actionable optimization steps to address red flag campaigns identified in weekly review.

**When to load:** Only when red status campaigns (🔴) are identified and require action planning.

**Source:** Extracted from optimization framework - prioritization and execution guidance.

---

## Prioritize Optimization Action

**Priority Matrix:**

```
Priority = (Impact × Confidence) / Effort

Impact: High (3), Medium (2), Low (1)
Confidence: High (3), Medium (2), Low (1)
Effort: High (3), Medium (2), Low (1)
```

**Priority Levels:**
- **P0 (Score ≥ 6):** Execute immediately
- **P1 (Score 4-5):** Schedule this week
- **P2 (Score 2-3):** Batch for weekly review
- **P3 (Score ≤ 1):** Backlog or reject

**Use this to prioritize actions** when multiple red flags exist across campaigns.

---

## Optimization Action Types

### 1. Bid Adjustments

**When to Use:**
- CPA/ROAS off target by 10-30%
- Performance trending wrong direction but campaign structure is sound
- Quick win opportunity (low effort, low risk)

**Best Practices:**
- Adjust by 10-20% increments (avoid drastic changes)
- Monitor for 3-5 days before additional adjustments
- Use automated bidding when data volume supports it (50+ conversions/month)

**Example Actions:**
- Increase Target CPA from $50 to $55 (underspending, low volume)
- Decrease Target ROAS from 4.0 to 3.5 (overly aggressive, limiting spend)
- Add device bid adjustments: +20% mobile (if mobile converts better)

---

### 2. Budget Reallocations

**When to Use:**
- Budget exhaustion in high-performing campaigns
- Underutilization in low-performing campaigns
- Strategic shift in priorities

**Best Practices:**
- Reallocate gradually (10-20% shifts per week)
- Don't starve campaigns below minimum threshold ($10-20/day for Search)
- Monitor impression share lost to budget as leading indicator

**Example Actions:**
- Shift $300/week from Campaign A (CPA $80, target $50) to Campaign B (CPA $40, budget-constrained)
- Reduce budget on paused test campaign, reallocate to core evergreen campaign

---

### 3. Creative Rotations

**When to Use:**
- CTR declining >15% WoW
- Frequency >3 in target audience
- Creative age >30-45 days (platform dependent)
- A/B test winner identified

**Best Practices:**
- Always have 2-3 active creatives per ad group (for testing)
- Rotate, don't replace entirely (maintain some continuity)
- Document creative fatigue patterns to predict refresh needs

**Example Actions:**
- Pause Creative A (45 days old, CTR declining), activate Creative D (fresh variant)
- Scale winning creative from 30% impression share to 60%

---

### 4. Targeting Changes

**When to Use:**
- Audience/keyword segment analysis shows clear winners/losers
- Opportunity to expand reach without sacrificing efficiency
- Need to improve relevance (Quality Score, engagement)

**Best Practices:**
- Pause low performers only after sufficient data (50+ clicks for Search)
- Expand gradually (add similar audiences/keywords, not broad jumps)
- Test before scaling (pilot new targeting at 10-20% of budget)

**Example Actions:**
- Pause 15 keywords with CPA >2x target, <10 conversions in 30 days
- Add negative keywords based on search term report (irrelevant queries)
- Expand lookalike audience from 1% to 2% (if 1% performing well)

---

### 5. Structural Changes

**When to Use:**
- Fundamental campaign architecture issues (poor organization, conflicting targeting)
- Quality Score consistently low despite optimizations
- Strategic pivot (new products, markets, goals)

**Best Practices:**
- High-risk, high-effort - only when incremental optimizations insufficient
- Test new structure in parallel (don't blow up working campaigns)
- Allow 2-4 weeks for learning period

**Example Actions:**
- Restructure single campaign with 50 ad groups into 3 thematic campaigns
- Separate branded vs. non-branded search into distinct campaigns
- Create dedicated mobile campaign (if mobile experience/offers differ)

---

## Action Selection Guide

**For each red flag campaign, match symptom to action type:**

| Symptom | Likely Action Type | Example |
|---------|-------------------|---------|
| CPA 10-30% above target, no structural issues | Bid Adjustments | Increase Target CPA 15%, monitor 5 days |
| High-performing campaign hitting budget cap | Budget Reallocation | Shift $500 from underperforming campaign |
| CTR declining >15%, creative age >30 days | Creative Rotation | Pause fatigued creative, activate new variant |
| Keyword segment analysis shows clear losers | Targeting Changes | Pause 10 keywords with CPA >2x target |
| CPA consistently high despite tactical changes | Structural Changes | Separate campaigns by match type |

---

## Building Action Items for Weekly Master Doc

**Every action item must include:**

1. **Specific task** - Clear, actionable step (not vague "optimize")
2. **Target** - Which campaign/ad group/element
3. **Rationale** - Why this action (1 sentence, data-driven)
4. **Expected outcome** - What metric should improve and by how much
5. **Deadline** - Day of week for completion

**Template:**
```
- [ ] **[Action Type] in [Campaign]** - [Specific change]
  - Why: [Data-driven rationale]
  - Expected outcome: [Metric improvement]
  - Deadline: [Day of week]
```

**Good example:**
```
- [ ] **Rotate creative in Meta Prospecting** - Pause Creative A, activate Creative #23
  - Why: Creative A running 42 days, CTR down 28% WoW, frequency 3.8
  - Expected outcome: CTR recovery to 2.0%+, CPA back to $50 target
  - Deadline: Monday
```

**Bad example:**
```
- [ ] Optimize Meta campaign
```

---

## Hypothesis-Driven Action Planning

**Every action should be based on a hypothesis:**

**Hypothesis template:**
```
"[Metric] is [direction] because [root cause], evidenced by [data].
If we [proposed action], we expect [predicted outcome]."
```

**Example:**
```
"CTR is declining (-28% WoW) because the primary creative has run for 42 days and shows frequency increase (2.3 → 3.8), indicating creative fatigue. If we rotate in new creative variant (Creative #23), we expect CTR to recover to 2.0%+ within 5 days."

Action: Creative Rotation → Pause Creative A, activate Creative #23
```

**Flow: Hypothesis → Action Type → Specific Action Item**

---

## Notes

- **Only load this document when red flags exist** - Don't generate actions for green/yellow campaigns
- **Prioritize actions** - If multiple reds, use priority matrix to determine execution order
- **Be specific** - Every action item should be immediately executable by account manager
- **Set deadlines** - Actions without deadlines don't get done
- **Track outcomes** - Reference these actions in next week's review to evaluate impact
