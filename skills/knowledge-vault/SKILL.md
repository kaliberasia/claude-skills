---
name: kaliber-knowledge-vault
description: "Codify session learnings into Kaliber's knowledge hub through structured reflection. Extracts reusable principles, processes, and gotchas from any working session — weekly reviews, campaign builds, creative testing, strategy discussions, marketing brainstorms, client analysis, or process improvement. Use this skill whenever a team member wants to capture what was learned during a session: 'codify this', 'what did we learn', 'save this to the knowledge hub', 'let's reflect on what worked', 'document what we figured out', 'capture this for the team', 'add this to our knowledge base', or any indication that session learnings should be preserved for future use. Also triggers when someone says 'knowledge capture', '/knowledge-capture', or 'reflect and save'. This skill is about institutional memory — turning working sessions into reusable organizational knowledge."
---

# Knowledge Capture — Session Reflection & Codification

## Purpose

Working sessions with Claudette produce insights that live only in the conversation — and die when the session ends. This skill extracts that knowledge before it's lost, separates the reusable from the situational, and codifies it into the knowledge hub where any team member (or co-pilot) can find and apply it later.

The goal is institutional knowledge compounding. Every session that ends with a capture makes the next session smarter.

## When This Skill Activates

**Triggers:**
- "Codify this" / "codify what we learned"
- "What did we learn?" / "reflect on this session"
- "Save this to the knowledge hub"
- "Document what we figured out"
- "Capture this for the team"
- "/knowledge-capture"

**Session types this handles (non-exhaustive):**
- Performance analysis & weekly reviews → analytical frameworks, diagnostic patterns
- Campaign builds & optimisations → tactical playbooks, platform-specific knowledge
- Creative testing & analysis → testing methodologies, creative insights
- Client strategy sessions → strategic frameworks, market intelligence
- Marketing & content work → content approaches, channel learnings
- Process improvement → workflow refinements, tool usage patterns
- Research & exploration → industry benchmarks, trend analysis

**Not this skill:**
- Documenting a single fact or data point ("Meta CPMs went up 15%") — just add it directly, no reflection protocol needed
- Updating client memory with client-specific context — use client memory.md directly
- Processing meeting transcripts — use the meeting-log skill

---

## Workflow

### Step 0: Identify the Session & the Person

Before extracting anything, establish context:

1. **Who is the team member?** Check the conversation for their name. If unclear, ask. The author of codified knowledge matters — it signals domain authority and helps the team know who to ask follow-up questions.

2. **What kind of session was this?** Scan the conversation history to understand:
   - What was the task? (weekly review, campaign build, creative analysis, strategy work, etc.)
   - Which client, if any?
   - What tools/platforms were involved?
   - Where did the session deviate from the standard approach? (This is where the good knowledge lives.)

Don't ask the user to summarize — you were there. Read the conversation.

---

### Step 1: Extract — Separate Signal from Noise

Work through three filtering lenses on the session. Do this internally — don't narrate the filtering process to the user.

**Lens 1: Client-specific vs. reusable**
Things that only apply to one client's situation go in client memory, not the knowledge hub. But often there's a general principle hiding inside a client-specific observation. Your job is to find it.

- Client-specific: "Castle Group's retargeting audience is too small for frequency caps to matter"
- Reusable principle extracted from it: "For accounts with retargeting audiences under 5K, frequency caps create delivery issues — let the algorithm self-regulate and monitor cost-per-incremental-conversion instead"

**Lens 2: Deliberate choice vs. default**
Defaults don't need documenting — they're already the path of least resistance. What's worth capturing are the moments where the session *departed* from the obvious approach and got a better result, or where following the default led to a problem.

- Not knowledge: "We used CPA bidding"
- Knowledge: "We switched from CPA to value-based bidding mid-campaign because the CPA target was throttling delivery on high-value leads — ROAS improved 40% with no change in lead quality"

**Lens 3: What would a new team member get wrong?**
This surfaces tacit knowledge — the stuff that experienced team members do instinctively but can't easily articulate. It's the most valuable type because it's the hardest to transfer without deliberate effort.

- "When CPL spikes but lead quality holds steady, don't flag it as a problem — the funnel is self-correcting and the higher CPL is buying better leads"
- "Always check if a Meta frequency spike is driven by a single ad set before diagnosing creative fatigue at the campaign level"

---

### Step 2: Present — Show the Team Member What You Found

Organize the extracted knowledge into three buckets and present it clearly.

**Format for presentation:**

```
Here's what I think is worth codifying from this session:

## Principles
[Reusable rules of thumb — each with the condition where it applies and where it doesn't]

1. **[The principle]**
   - Context: [What we were doing when this surfaced]
   - Why it matters: [What goes wrong if you don't know this]
   - Boundary: [When this does NOT apply]

## Process
[Sequencing, workflow, or analytical approach that produced better output]

1. **[The process step or sequence]**
   - Context: [What it improved over the previous/default approach]
   - Why it matters: [The quality or efficiency gain]

## Gotchas
[Counterintuitive findings or things that looked right but were wrong]

1. **[The gotcha]**
   - Context: [How we discovered it]
   - Why it matters: [The mistake it prevents]
```

**Important nuances:**
- Not every session will have all three buckets. Some sessions yield one principle and nothing else. That's fine — don't pad.
- If the session genuinely didn't produce reusable knowledge (it was purely execution), say so honestly. Not every session needs a capture.
- Be specific. "Analyse data carefully" is not a principle. "When comparing WoW performance, normalise for day-of-week effects before drawing conclusions — Tuesday spend patterns in SG are consistently 15-20% lower than Thursday" is.

---

### Step 3: Deliberate — Validate with the Team Member

This is the gate. Don't skip it.

Ask three questions:

1. **"Anything here that's too specific to this client/situation to generalize?"**
   → Helps catch overfitting. Sometimes what looks reusable is actually a quirk of one account.

2. **"Anything I missed — something you noticed during the session that I didn't pick up?"**
   → The team member often has context the co-pilot doesn't. They noticed a pattern but didn't say it out loud. This question surfaces it.

3. **"Should any of these update an existing doc rather than create a new one?"**
   → The team member may know that there's already a playbook or framework that this builds on.

Wait for their response. Incorporate additions, remove what they flag as too specific, and note which existing docs to update.

---

### Step 4: Codify — Write to the Knowledge Hub

#### 4a: Check for existing knowledge

Before creating new docs, check what already exists. Use progressive disclosure — don't read every file in the hub.

**Level 1: Scan the index**
```
Read knowledge_hub/INDEX.md
```
Look for docs with related tags or topics. If nothing looks related, skip to 4b.

**Level 2: Check frontmatter of candidates**
For any docs that might overlap, read just the first 30 lines (frontmatter + summary section). This tells you whether the doc covers the same ground.

**Level 3: Read full doc if relevant**
Only if the summary confirms overlap — read the full doc to understand what's already captured and where the new knowledge fits (update vs. new doc).

**Decision:**
- If an existing doc covers the same topic → update it (add new sections, refine existing content, update the `updated` date)
- If the new knowledge is adjacent but distinct → create a new doc and cross-reference
- If no overlap → create a new doc

#### 4b: Write or update docs

**For new docs**, use this format:

```markdown
---
title: "[Descriptive title — what this knowledge helps you do]"
category: [playbooks | frameworks | platform-intel | case-studies | benchmarks | research]
tags: [relevant, searchable, tags]
author: [Team member's name — the person who drove the insight]
created: [today's date, YYYY-MM-DD]
updated: [today's date, YYYY-MM-DD]
status: active
---

## Summary

[One paragraph — what this doc covers, when to use it, and who it's most relevant for.]

## [Content sections — structure based on knowledge type]

[For principles/frameworks: State the principle, explain the reasoning, give examples, note boundaries]
[For playbooks: Step-by-step with decision points and rationale]
[For platform intel: The behaviour, evidence, implications for campaign management]
[For case studies: Situation, approach, result, generalizable takeaway]

## Sources

- Session with [team member name], [date]
- [Any other sources: client data, platform documentation, external research]
```

**Category selection guide:**
- How to think about / analyse something → `frameworks/`
- How to do something step by step → `playbooks/`
- How a platform behaves or a quirk to be aware of → `platform-intel/`
- What we learned from a specific client engagement, generalized → `case-studies/`
- Quantitative reference data (CPL ranges, CTR norms, etc.) → `benchmarks/`
- Industry trends, market findings, external research → `research/`

**File naming:** Use lowercase kebab-case that describes the content, not the session.
- Good: `meta-frequency-vs-fatigue-diagnosis.md`, `lead-gen-cpl-quality-tradeoff.md`
- Bad: `session-notes-march-2026.md`, `jacqueline-weekly-review-learnings.md`

**File location:** Place in the category directory.
- `knowledge_hub/frameworks/performance-data-analysis-methodology.md`
- `knowledge_hub/platform-intel/meta-advantage-plus-ecommerce-thresholds.md`

#### 4c: Update the index

Add a row to the relevant section of `knowledge_hub/INDEX.md`:

```markdown
| [Doc title](category/filename.md) | tag1, tag2, tag3 | active | YYYY-MM-DD |
```

If updating an existing doc, update its `Updated` column in the index.

#### 4d: Confirm what was saved

Tell the team member:
- What was created or updated (with file paths)
- A one-line summary of each doc
- How it connects to anything already in the hub

---

### Step 5: Connect — Flag Downstream Implications

After codifying, check whether the new knowledge has implications for how existing skills or workflows operate. This is where knowledge capture becomes a flywheel — each capture potentially improves the system.

**Check for connections:**
- Does this change how weekly reviews should be run? (weekly-review, weekly-review-v2 skills)
- Does this affect how campaigns should be built? (gads-campaign-setup, execute-optimisation skills)
- Does this contradict or refine an existing knowledge hub doc?
- Does this suggest a new default or threshold that should be embedded in a skill?

**If connections exist, flag them — don't act on them:**

```
This new knowledge has potential implications:

- The weekly review skill currently [does X] — this suggests it should [do Y instead].
  Want me to update it?

- This refines what's in [existing doc] — specifically the section on [topic].
  Want me to reconcile them?
```

The team member decides whether to act on the connections. This is important because skill changes affect everyone, and the person who surfaced the knowledge is best positioned to judge whether it's proven enough to change the system.

---

## Quality Standards

These aren't arbitrary rules — each one exists because the knowledge hub becomes useless without them.

**Be specific.** Vague knowledge is worse than no knowledge because it creates false confidence. "Monitor creative performance" tells someone nothing. "Rotate static creatives when CTR drops below 60% of its first-week average over two consecutive weeks" gives them a decision rule.

**Include the 'so what'.** Every piece of knowledge should imply an action or decision. If reading it doesn't change what someone would do, it's not worth saving.

**State the boundary.** Every principle has conditions where it breaks down. Capturing those boundaries is as valuable as capturing the principle itself — it prevents misapplication.

**Cite the source.** "Internal testing with Client X, March 2026" or "Meta platform documentation" or "Pattern observed across 5 lead gen accounts." This lets future readers assess how much weight to give the knowledge.

**One topic per doc.** Mega-documents are where knowledge goes to hide. Atomic docs are findable, updatable, and composable. If a capture session produces three distinct insights, that's three docs.


---
*Built by [Kaliber](https://kalibergroup.netlify.app) — Singapore's AI-native marketing group*
