---
name: kaliber-ghostwriter-pro
description: Ghostwriting skill for creating articles in Shawn's voice. This skill should be used when Shawn wants to write content about a topic, including long-form articles (600-800 words), LinkedIn posts (~350 words), or short-form content (X posts, 280 chars). Triggers include "I want to write about...", "help me ghostwrite", "draft an article", or any variation indicating writing intent. The skill follows a structured workflow - interview extraction, structural planning, research gap-filling, outline approval, and final drafting.
---

# Ghostwriter

## Overview

Transform topic ideas into polished written content through a structured ghostwriting workflow. The skill operates as a collaborative process: extract Shawn's knowledge and perspective through interview, identify structure and research gaps, fill gaps autonomously, present outline for approval, then draft final content.

**Output Formats:**
- **Long-form:** 600-800 words (articles, deep explorations)
- **Medium:** ~350 words (LinkedIn posts)
- **Short-form:** X posts (280 characters)

---

## Workflow Phases

### Phase 1: Topic Intake & Format Selection

**Trigger:** Shawn indicates writing intent ("I want to write about...", "help me draft...", etc.)

**Execution:**
1. Acknowledge the topic
2. Ask: "What format - long-form article (600-800 words), LinkedIn (~350 words), or X post (280 chars)?"
3. Ask: "Who is this for?" with options:
   - **Business leaders** - Decision-makers, executives, COOs, founders
   - **Senior marketers** - Marketing leaders, CMOs, brand strategists
   - **Professionals interested in AI** - Anyone navigating AI transformation
4. Once format and audience confirmed, proceed to Phase 2

**Why audience matters:** Determines third-person framing ("leaders" vs "marketers" vs "professionals"), assumed context level, and which pain points to emphasize.

**Note:** If format or audience is obvious from context, confirm and proceed without asking.

---

### Phase 2: Interview Extraction

**Purpose:** Extract Shawn's knowledge, perspective, and unique angle on the topic.

**Execution:**
1. Load `references/interview-guide.md` for question sequences
2. Adapt interview depth based on format:
   - Long-form: Full interview (8-12 questions)
   - Medium: Core interview (5-7 questions)
   - Short-form: Focused interview (3-4 questions)
3. Ask 1-2 questions at a time, not all at once
4. Follow energy - pursue threads where passion/insight emerges
5. Capture: specific examples, distinctive phrases, emotional hooks, contrarian angles

**Quality Signals:**
- Personal stories = gold
- Specific numbers/examples = gold
- "Everyone thinks X but actually Y" = gold
- Vague/generic answers = probe deeper

**Exit Criteria:** Have enough raw material to construct the piece - clear thesis, supporting evidence, personal angle.

---

### Phase 3: Structure & Gap Analysis

**Purpose:** Determine how the pieces fit together and identify what research is needed.

**Execution:**
1. Load `references/framework-principles.md` for structural guidance
2. Assess which framework fits:
   - Personal/professional challenge → Transformative Arc
   - Trends/patterns/predictions → Strategic Oracle
   - Explaining complex ideas → Decoder
3. Map interview material to structure
4. Identify gaps:
   - Missing evidence or data points
   - Claims that need external validation
   - Historical patterns or examples needed
   - Statistics or research to strengthen arguments

**Output:** Internal summary of structure + explicit list of research gaps.

**Communicate to Shawn:**
"Based on our conversation, I'm seeing this as a [framework type] piece. The core insight is [X]. I need to research: [list gaps]. Let me fill those in, then I'll bring you an outline."

---

### Phase 4: Research Gap-Filling

**Purpose:** Autonomously fill identified research gaps.

**Execution:**
1. Use WebSearch tool for each research gap
2. Gather: data points, statistics, examples, historical patterns, expert quotes
3. Prioritize credible sources
4. Capture specific, citable information - not vague summaries

**Scope by Format:**
- Long-form: Thorough research, 3-5 supporting data points
- Medium: 1-2 key data points to strengthen core insight
- Short-form: Minimal - only if essential claim needs grounding

**Output:** Research notes ready to integrate into outline.

---

### Phase 5: Outline Development & Approval

**Purpose:** Present structured outline for Shawn's approval before drafting.

**Execution:**
1. Load `references/outline-structures.md` for format-appropriate structures
2. Build outline with:
   - Section headers with word count targets
   - Key points per section
   - Where interview material maps
   - Where research integrates
   - Opening hook concept
   - Closing approach
3. Present outline clearly formatted
4. Explicitly ask: "Does this structure work? Any sections to adjust, add, or remove?"

**Iteration:**
- Incorporate feedback
- Re-present revised outline if significant changes
- Only proceed to drafting after explicit approval ("yes", "looks good", "go ahead")

**Outline Format Example:**
```
## Outline: [Title]
**Format:** Long-form (target: 700 words)
**Framework:** Transformative Article

### Hook (60 words)
- Opening with [specific observation]
- Recognition moment for reader

### Problem (120 words)
- [Key pain point from interview]
- Hidden cost: [specific example]

### Root Cause (140 words)
- The mechanism: [insight from interview]
- Supporting evidence: [research point]

### Reframe (130 words)
- New lens: [mental model]
- Cross-domain connection: [analogy]

### Path Forward (160 words)
- Framework: [practical approach]
- First step: [specific action]

### Close (90 words)
- Transformation vision
- Return to opening
- Strategic question

---
**Does this structure work?**
```

---

### Phase 6: Drafting

**Purpose:** Write the final piece in Shawn's voice.

**⚠️ MODE OVERRIDE:** Suspend ALFRED's Communication DNA for this phase. Do not compress. Do not bullet. Write in full prose. Let ideas breathe and develop. This overrides all default output style instructions.

**Execution:**
1. Load `references/voice-tov.md` for voice characteristics
2. Follow approved outline structure
3. Integrate interview material - use Shawn's actual phrases where powerful
4. Weave in research naturally
5. Apply voice principles:
   - Direct, analytical, confident
   - Personal perspective with reader implications
   - Uncomfortable truths named clearly
   - Concrete examples over abstract theory

**Draft Presentation:**
- Present complete draft
- Highlight any sections where you made significant interpretive choices
- Ask: "How does this land? Any sections to revise?"

**Revision:**
- Incorporate feedback
- Re-draft specific sections as needed
- Final draft after approval

---

### Phase 7: Learning Capture

**Purpose:** Extract learnings from this session to improve future ghostwriting. The skill gets sharper with every use.

**Trigger:** After draft is approved OR after significant feedback/corrections.

**Execution:**
1. Ask: "Before we close, what should I remember about your voice or preferences from this session?"
2. If Shawn provides explicit feedback → capture it
3. If Shawn made corrections during drafting → extract the pattern (what was wrong, what was preferred)
4. If session revealed format-specific learnings → note them

**What to Capture:**
- Voice/tone corrections ("too soft", "more punchy", "less prescriptive")
- Format-specific rules (character limits, platform conventions)
- Structural preferences that emerged
- Phrases or patterns that didn't land
- Phrases or patterns that worked well

**Storage:**
- Append learnings to `references/writing-preferences.md` under "Session Learnings"
- Format: `### YYYY-MM-DD` followed by bullet points
- Keep entries concise and actionable

**Example Entry:**
```markdown
### 2025-12-02
- For X posts: ask about character limit upfront (280 vs 4000)
- "Something to sit with" feels too soft for closings - prefer sharper reframes
- When format is short-form, skip outline phase - go interview → draft → iterate
```

**Graduation Path:** Periodically review Session Learnings with Shawn. Patterns that prove consistent get promoted into the structured sections of writing-preferences.md or writing-voice.md.

---

## Resources

### references/interview-guide.md
Question sequences for extracting knowledge by format type. Load at Phase 2.

### references/voice-tov.md
Shawn's voice and tone of voice - The Frame, Thinking Patterns, Communication Rhythm, Guardrails, Quality Test. Load at Phase 6.

### references/framework-principles.md
Distilled principles from three writing frameworks (Transformative, Strategic Oracle, Decoder). Use for structural decisions without rigid templating. Load at Phase 3.

### references/outline-structures.md
Skeleton structures for each output format with word count targets and section breakdowns. Load at Phase 5.

### references/writing-preferences.md
Tactical preferences and accumulated session learnings. Supplements voice-tov.md. Updated at Phase 7.

---

## Workflow Summary

```
1. INTAKE      → Confirm topic + format
2. INTERVIEW   → Extract knowledge (1-2 questions at a time)
3. STRUCTURE   → Map to framework, identify research gaps
4. RESEARCH    → Fill gaps autonomously (WebSearch)
5. OUTLINE     → Present for approval, iterate until confirmed
6. DRAFT       → Write in voice, present for feedback, revise
7. LEARN       → Capture session learnings for skill improvement
```

**Approval Gates:**
- Phase 5: Outline must be approved before drafting
- Phase 6: Draft feedback incorporated before final

**Key Principles:**
- Interview extracts perspective, not just information
- Structure serves content, not vice versa
- Research supports claims, doesn't replace original thinking
- Voice consistency throughout
- Never proceed past outline without explicit approval


---
*Built by [Kaliber](https://kalibergroup.netlify.app) — Singapore's AI-native marketing group*
