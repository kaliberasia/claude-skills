---
name: kaliber-ad-creator
description: Generate a 3-part static ad sequence (Seed → Personalise → Resolve) that does the narrative work of a video ad using separate visual (CEP) and copy (insecurity) mechanics. Use this skill when a client needs performance creative for paid social — Meta, TikTok, Instagram Stories — and wants static ads that stop the scroll, build tension, and convert without relying on motion. Also trigger when the user says "I need ad creatives", "static ads for [client]", "build a 3-part ad sequence", "creative brief for paid social", "scroll-stopping ads", "we need statics not video", "ad copy for Instagram", "social ad concepts for [client]", "performance creative for [client]", "creative strategy for Meta", or any variation asking for static ad creative, ad copy with visual direction, or a multi-ad narrative sequence. Works across any client category — health, e-commerce, B2B, F&B, finance, etc. Requires a structured intake interview before creative work begins.
---

This skill generates a **Creative Performance Loop sequence** — three acts engineered as a narrative arc where each ad has a single job. Together they function like a 3-act video. The three acts are **not equally distributed** in deployment — the algorithm will favour one act heavily based on audience size and engagement signal. The skill accounts for this by generating multiple variants of the act most likely to dominate spend.

## CREATIVE MECHANICS — READ FIRST

Before generating any creative, read `references/creative-mechanics.md` to understand the two separate systems governing this skill:

1. **Visual system (CEP logic)** — Category Entry Points anchor the image in a moment the audience already lives in. The visual pre-qualifies the viewer before copy is read. Uses Romaniuk's 7Ws framework.
2. **Copy system (insecurity logic)** — Names an anxiety the audience hasn't fully articulated, at the depth where it stings. Three mechanics: pre-awareness manufacturing, unnamed fear, social exposure moment.

These are separate systems. Do not write copy that describes a CEP situation. Do not design a visual that tries to name a fear. Each medium has its mechanism — let them do different jobs.

---

## PHASE 0: INTAKE INTERVIEW (MANDATORY — DO NOT SKIP)

Ask all questions in a single block. Do not begin strategic or creative work until every question has been answered with sufficient depth. If an answer is vague or uses demographic language instead of behavioural language, probe once before proceeding.

**STATIC AD SEQUENCER — CLIENT INTAKE**

Answer each question as specifically as possible. Vague answers produce generic ads. Behavioural specificity produces creative that stops the scroll.

**1. Product / Service**
What exactly is being advertised? Include: the category, the core mechanism or promise, and any specific verified claims usable in copy — dosages, percentages, clinical terms, certifications, awards, before/after data.

**2. Target Audience — Behavioural Profile**
Do not describe demographics. Describe behaviour. What are they already doing, buying, or believing before they encounter this ad? What have they tried before? What have they invested in — financially, emotionally, or in terms of time and identity?

**3. The Prior Investment / Sunk Cost**
What has this audience already committed to that this product protects, extends, or rescues? The ad must attach itself to something they have already bought into. If you're unsure, say so — we will derive it together from the behavioural profile.

**4. Known Anxieties or Side Effects**
What is this audience privately worried about as a consequence of their existing behaviour or investment — even if they haven't connected the dots yet? Push past the obvious surface fear to the one that stings. What would they be embarrassed to admit?

**5. The Social Exposure Moment**
Has there been a real customer story, review, or moment where the problem became visible to someone else — not just the user? Describe the most specific version you have. Name the third party and the setting. If you don't have one, say so and we will construct a credible hypothetical from real audience patterns.

**6. CEP Status — Critical Gate**
Do you have Category Entry Point research for this audience? CEPs are the specific needs, occasions, or situations — defined by Byron Sharp and Ehrenberg-Bass — that lead a consumer to think about a product category. They are not "pain points." They are the exact situational and emotional moments that precede category consideration.

- **If YES:** List the top 3 CEPs. For each, describe: the trigger situation, the emotional state, and whether it skews functional (what they're doing) or emotional (how they're feeling).
- **If NO:** Flag this. The skill will recommend triggering the **cep-research-mapping skill** first, or will generate working CEP hypotheses to be treated as assumptions until validated.

**7. Identity Prop**
What single object, image, or visual symbol would instantly signal "this ad is for someone like me" without any text? It should appear naturally in the audience's daily life or physical environment. It must pre-qualify the viewer before the copy is read.

**8. Platforms, Formats, and Constraints**
Where will these ads run? (Meta Feed, Meta Stories, Instagram Stories, TikTok, etc.) Any aspect ratio, text overlay percentage, or compliance/regulatory constraints?

**9. Reference Creative**
Share competitor ads, reference images, or "ads that made you stop scrolling." What do you want this work to learn from? What do you want to deliberately do differently?

**10. What Has Already Been Tested**
What creative has already run for this product? What performed, what didn't, and what is your hypothesis about why?

---

### CEP GATE — DECISION LOGIC

After receiving intake answers, evaluate Question 6:

- **If CEPs are provided and specific:** Proceed to Phase 1, using the CEPs as anchor inputs for visual direction only. Copy will be governed by the insecurity mechanics.

- **If CEPs are absent or vague:** Output the following before proceeding:

> **CEP Gap Detected**
>
> CEPs govern the visual strategy for this sequence — they determine whether the image stops the right person in the first place. Without validated CEPs, the visual direction will be assumption-based.
>
> **Recommended action:** Run the **cep-research-mapping skill** on this audience before proceeding.
>
> **To proceed without CEP research:** I will generate working CEP hypotheses using Romaniuk's 7Ws framework based on the intake answers. These are assumptions to be validated through creative testing — not confirmed insights. Type **"proceed with hypotheses"** to continue.

### COMPLIANCE CHECK

After intake, before proceeding to Phase 1, evaluate regulatory risk based on Questions 1 and 8:

**Flag if the category is regulated** (health/supplements, finance, insurance, alcohol, gambling, pharmaceuticals). For regulated categories:
- Note specific claim restrictions (e.g., no disease claims for supplements, no guaranteed returns for finance)
- Flag any intake answers that contain unverifiable claims
- Add a `**Compliance note:**` line to each ad variant in the output specifying what needs legal review
- When in doubt, err toward factual/verifiable language in Act 3's proof block

Do not skip this step. Powerful copy that violates advertising standards wastes everyone's time.

---

## PHASE 1: STRATEGIC DECONSTRUCTION

Complete this diagnostic using the intake answers. Output it as a labelled section. User must confirm or correct before Phase 2 begins.

**1. Sunk Cost / Prior Investment**
State precisely what the audience has already committed to. Name the financial, behavioural, or emotional investment.

**2. The Unnamed Fear**
Name the specific anxiety at the depth it hasn't been said aloud. Not the obvious concern — the one that stings. One sentence, no hedging.

**3. The Social Exposure Moment**
Name the third party. Name the setting. One specific, concrete scene.

**4. Identity Prop**
Name the visual object. Confirm it appears naturally in the audience's daily context.

**5. CEPs in Play — Visual Anchors**
List 1–3 CEPs this sequence will use as visual anchors. For each state:
- The trigger situation (7Ws: when, where, while, how feeling)
- The emotional state at that moment
- Which act will use this CEP as its visual anchor
- Whether it is a functional CEP (situation-driven) or emotional CEP (feeling-driven)

**6. The Reframe Mechanic**
- **Before belief:** what they currently think that keeps them from buying
- **After belief:** what they need to believe for the product to make obvious sense

**7. Algorithm Weight Prediction**
Predict which act will receive the most platform spend and why. Set variant counts accordingly:
- **Act 1 (Seed):** Broadest audience — 3–5 headline variants minimum
- **Act 2 (Personalise):** Mid-funnel — 2–3 variants
- **Act 3 (Resolve):** Retargeting only — 1–2 variants

Adjust based on whether the client is running primarily cold traffic or retargeting.

---

## PHASE 2: THE 3-ACT SEQUENCE

Each act has one job. One emotion. Generate the number of variants set in Phase 1.

### ACT 1 — SEED (Anxiety Activation)
**Job:** Stop the scroll with a visual CEP that pre-qualifies. Then name the unnamed fear directly. Create cognitive dissonance. The product is absent or secondary.

**Visual:** Anchored in the highest-value CEP from Phase 1. The identity prop is present. The scene should be recognisable from the audience's daily life — not aspirational, not clinical. The viewer should feel: *this is my situation.*

**Copy:**
- **Headline:** Name the tension between what's working and what's being silently lost. Short. Direct. Uses pre-awareness manufacturing or unnamed fear mechanic.
- **Subhead:** Make the hidden price visceral and list-specific. Two lines maximum.
- **Tone:** Direct. Empathetic but not soft. You are naming something they already feel but haven't said. Do not hedge. Do not comfort. Let the discomfort breathe.
- **CTA:** Curiosity only. Low commitment. ("Learn more" / "See what's happening")

**Variants:** Test different unnamed fears. Test different levels of specificity in the subhead. Test different contrast structures in the headline. Label A/B/C.

### ACT 2 — PERSONALISE (Social Proof via Story)
**Job:** Make the abstract fear real through a specific human story. The social exposure moment lives here. The audience must see a real person — not an idealised version.

**Visual:** Warm, lifestyle, real-feeling. A split or contrast composition works — before/after where the "after" is complicated, not triumphant. No clinical imagery. No stock photo energy. The identity prop reappears.

**Copy:**
- **Headline:** State the win first, then the complication.
- **Body:** Two sentences. Emotional tension only.
- **Product line:** One line. The product is the resolution, not the hero.
- **CTA:** Urgency without pressure. ("Don't wait for the warning signs")
- **Tone:** Human. Specific. The third party in the story must be named. The setting must be named. Vagueness kills recognition.

**Variants:** Test different protagonist demographics if the audience is broad. Test different social exposure moments. Test different third parties.

### ACT 3 — RESOLVE (Clinical Permission)
**Job:** Convert the emotionally primed viewer. Rational proof meets emotional readiness. Give them the evidence to justify what they already want to do.

**Visual:** Product hero in a lifestyle context. The identity prop reappears — closing the visual loop from Act 1. The CEP from Act 1 echoes here, but the emotional register shifts from anxiety to confidence.

**Copy:**
- **Headline:** The product's mechanism of action, stated with clinical specificity. Specific > general. Always.
- **Proof block:** 3–5 claims with dosages, qualifiers, or clinical terms. Lead with the ingredient tied to the fear named in Act 1. Include one unexpected technical item that signals formulation depth. Use checkmarks or a clean list — this is a rational scan, not a read.
- **CTA:** Direct. Transactional. Confident. ("See the full nutrient profile" / "Start today") Do not soften. You have earned the conversion.
- **Tone:** Confident. Clinical but not cold. This is permission to buy — not a pitch.

**Variants:** Test CTA phrasing. Test proof block ordering (lead with the primary fear vs secondary fears).

---

## PHASE 3: OUTPUT FORMAT

Generate the following for each ad and each variant:

```
## ACT [1/2/3] — [SEED/PERSONALISE/RESOLVE] — VARIANT [A/B/C]

**Strategic job:** [one sentence]
**Copy mechanic in use:** [pre-awareness manufacturing / unnamed fear / social exposure]
**CEP anchoring visual:** [which CEP, stated as a situation + emotional state]
**Algorithm role:** [cold / warm / retargeting] — [spend weight: high / medium / low]

**Headline:**
**Subhead / Body:**
**CTA (on-ad):**
**CTA (platform button):**

**Visual direction:**
- CEP scene: [describe the recognisable daily-life moment being depicted]
- Identity prop: [object, placement, how it pre-qualifies before copy is read]
- Composition: [layout and hierarchy]
- Mood: [2–3 words]
- What NOT to show: [specific traps for this category or audience]

**Compliance note:** [any claims requiring legal review, or "No regulated claims"]
**Variant hypothesis:** [what this variant is testing vs. other variants in this act]
**Targeting note:** [audience segment, funnel stage, bid strategy recommendation]
```

### Output Persistence

Save the complete creative brief to the client's output folder:
`delivery/pod-[region]/[client]/01_outputs/creative-briefs/[Client]_StaticAdSequence_[Date].md`

If the client folder doesn't exist yet or you're unsure of the path, save to the current working directory and note the path for the user.

---

## PHASE 4: REVISION & NEXT STEPS

After presenting the sequence, ask the user:

**"Which of these three things do you want to do next?"**

1. **Revise** — "Want to change any act, variant, or angle? Tell me which act and what's off — I'll regenerate just that part while keeping the rest intact."
2. **Generate visuals** — "Want me to generate draft images for any of these ads? I can use the visual direction to create mockups via `generate-image`." (Uses `fal-ai-media` or `generate-image` wrapper)
3. **Deploy** — "Ready to build these into a campaign? I can set up the ad structure via `execute-optimisation` (Meta) or generate a landing page via `lp-generate` if the ads drive to one."

If the user requests revisions:
- Regenerate only the specific act/variant requested — do not regenerate the entire sequence
- Preserve the strategic deconstruction from Phase 1 unless the user explicitly changes a strategic input
- If the revision changes Act 1's unnamed fear, flag that Acts 2 and 3 may need updating for narrative coherence
- Track revision count — if 3+ rounds of revisions on the same act, surface: "We've iterated on this act several times. Want to step back and revisit the Phase 1 strategy?"

---

## CRITICAL RULES

- **Visuals use CEP logic. Copy uses insecurity logic. These are separate systems.** Do not conflate them.
- **Direct copy outperforms clever copy in static.** Name the thing. Say it plainly. The discomfort is the hook.
- **Specificity is the proof in both copy and visuals.** A hairdresser is more powerful than "someone noticed." A specific dosage converts better than "clinically proven."
- **Never open with the product.** The product earns its place at Act 3.
- **Do not resolve the tension in Act 1.** Let the anxiety breathe. Premature resolution kills the sequence.
- **One emotion per act.** Anxiety (Act 1). Recognition (Act 2). Permission (Act 3).
- **Act 1 carries the most spend. It needs the most variants.** A weak Act 1 starves Acts 2 and 3 of retargeting audience.
- **The identity prop threads every act.** Visual continuity makes the sequence feel like one narrative even when encountered out of order.
- **The social exposure moment must be concrete.** "People noticed" is not a moment. "His hairdresser asked what was wrong" is a moment.

---

## SKILL DEPENDENCIES

- **Upstream:** `cep-research-mapping` — run first if client lacks validated CEPs (Phase 0, Question 6)
- **Downstream:** `execute-optimisation` — deploy the ads via Meta/TikTok once approved
- **Downstream:** `lp-generate` — if the ad sequence drives to a landing page, generate it from the same strategic brief
- **Downstream:** `fal-ai-media` / `generate-image` — generate draft visuals from the visual direction

---

## KALIBER CREATIVE PERFORMANCE LOOP ALIGNMENT

- **Diagnose** — Phase 0 intake + CEP gate ensures creative is built on validated audience insight, not assumption
- **Sequence** — 3-act structure with algorithm weighting and separate visual/copy mechanics replaces single-ad thinking
- **Optimise** — Variants per act make every output independently testable across copy mechanic, CEP, and proof structure
- **Learn** — Targeting notes and variant hypotheses feed back into CEP research, audience segmentation, and the next brief

When presenting to clients: *"Orchestration is the new optimisation — we're not running three ads, we're running one argument across three touchpoints, with the visual doing one job and the copy doing another."*


---
*Built by [Kaliber](https://kalibergroup.netlify.app) — Singapore's AI-native marketing group*
