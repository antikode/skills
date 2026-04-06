---
name: deck-content
description: "Transform a strategy-proposal.md into a slide-by-slide deck blueprint for Antikode pitches. Produces a deck-architecture.md with title, key message, content elements, visual direction, and speaker notes for each slide. Use this skill when the user says 'create deck content', 'deck blueprint', 'slide plan', 'deck architecture', 'turn this strategy into a deck', 'slide content from strategy', or when the discovery engine orchestrator reaches the deck content phase."
---

# Deck Content Generator

Transforms a completed strategy proposal into a slide-by-slide deck blueprint ready for PPTX production.

## Inputs (Required)

1. `src/brief/strategy-proposal.md` — the strategy (must contain Strategy Kernel or Story format)
2. `src/brief/research/brand-voice.md` — tone calibration for all copy
3. `src/brief/research/brand-diagnosis.md` — visual identity reference, color palette, brand assets

## Inputs (Optional)

4. `src/brief/brief.md` — original brief for data points and context
5. `src/brief/research/web-expert-review.md` — for competitive benchmark slides
6. `src/brief/research/strategy-consensus.md` — for supporting evidence

## Step 1: Detect Project Type and Select Deck Arc

Read strategy-proposal.md and determine which arc to follow:

| Project Type | Deck Arc | Core Narrative |
|-------------|----------|----------------|
| `crm-loyalty` | Insight-led | Pain → diagnosis → insight → ecosystem → proof |
| `website-revamp` | Before/After | Current state → gaps → vision → technical approach → ROI |
| `website-new` | Vision-led | Opportunity → vision → architecture → technology → roadmap |
| `general-pitch` | Standard Antikode | Situation → challenge → strategy → solution → partnership |

## Step 2: Generate Slide Blueprint

For each slide, specify ALL of the following:

```markdown
### Slide [N]: [Working Title]

**Headline:** [The slide title as it appears on the slide — max 10 words]
**Key message:** [The one thing the audience should take away — one sentence]
**Content:**
- [Specific text, data points, bullet points, or table content]
- [Reference which source file this comes from]

**Visual direction:**
- Layout: [full-bleed image / split screen / centered text / data visualization / comparison table]
- Imagery: [what type of image or graphic]
- Charts: [if applicable, what data to visualize and how]

**Speaker notes:**
[2-3 sentences the presenter should say. Written in natural speech, not bullet points.]

**Source:** [which research/strategy file feeds this slide]
```

## Step 3: Standard Slide Arc (CRM/Loyalty)

This is the default for `crm-loyalty` projects. Adapt for other types.

| # | Slide | Purpose |
|---|-------|---------|
| 1 | Cover | Client name + Antikode + date. Clean, premium. |
| 2 | The Situation | Market context, scale, what's happening. Ground the room. |
| 3 | The Real Challenge | Diagnosis — not the obvious problem. The structural forces. |
| 4 | The Evidence | 3-5 key data points from research. Make the case with numbers. |
| 5 | The Insight | One sentence that reframes everything. This is the emotional peak. |
| 6 | Competitive Landscape | Benchmark matrix. Where the client sits vs. market. |
| 7 | Why We Can Win | Source of advantage — first-mover, timing, unique capability. |
| 8 | The Strategy | Guiding policy. What we will do AND what we will not do. |
| 9 | The Architecture | System diagram — how the pieces connect. Technical credibility. |
| 10 | The Experience | Customer journey — what it feels like from the other side. |
| 11 | The Loyalty System | Tiers, points, mechanics. The reward structure. |
| 12 | vs. Competition | Head-to-head comparison with named competitor (e.g., Yobo). |
| 13 | Quick Wins | First 30 days — tangible actions that build trust. |
| 14 | The Roadmap | Phased timeline with milestones. 30/60/90/180 day view. |
| 15 | Proof Points | What the client will know at each milestone. Concrete deliverables. |
| 16 | Risk Mitigation | Top 3 risks with severity and mitigation. Show maturity. |
| 17 | Why Antikode | Credentials through specificity, not declaration. Relevant case studies. |
| 18 | Investment Tiers | 2-3 engagement options (Foundation / Growth / Enterprise). |
| 19 | Next Steps | Clear, actionable. What happens after this meeting. |
| 20 | Thank You | Clean close with contact information. |

## Step 4: Apply Brand Voice

Before finalizing, run every headline and content block through the brand-voice calibration:

1. **Lead with their world, not ours** — every slide opens with the client's reality
2. **Credential through specificity** — show expertise by how precisely we describe the solution
3. **Match the premium register** — if brand aspiration is premium, the copy must match
4. **Use the terminology guide** — "membership" not "loyalty program", "intelligence" not "data collection"
5. **One idea per slide** — if reader remembers one sentence per slide, the full argument holds

## Quality Checklist

| Check | Pass Criteria |
|-------|--------------|
| NARRATIVE_ARC | Follows situation → challenge → evidence → insight → strategy → actions → proof |
| SLIDE_COUNT | 12-25 slides total |
| HEADLINE_QUALITY | Every headline is ≤10 words and makes one clear point |
| ONE_IDEA_PER_SLIDE | No slide tries to cover two separate concepts |
| VISUAL_SPECIFICITY | Every slide has specific visual direction, not "add relevant image" |
| SPEAKER_NOTES | Every slide has 2-3 sentence speaker notes in natural speech |
| DATA_INTEGRATION | At least 5 slides reference specific numbers from research |
| EMOTIONAL_PACING | The deck has tension (challenge), peak (insight), resolution (strategy) |
| STRONG_OPEN | Slide 2 grabs attention — not "about Antikode" |
| STRONG_CLOSE | Final slides create momentum toward next steps |
| BRAND_VOICE | All copy passes the brand-voice quick check (7 questions) |

## Output

Save to `src/deck/[client-slug]-deck-architecture.md`

Report to user:
- Total slide count
- Slide list with headlines
- Any gaps where content is thin
- Quality checklist pass/fail
