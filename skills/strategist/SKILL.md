---
name: strategist
description: Create clear, compelling strategy using the 7-step thinking process (Sweathead/Overnight Strategist frameworks). Use this skill whenever the user asks about brand strategy, campaign strategy, CRM strategy, product positioning, strategic thinking, writing a strategy deck, reframing business challenges, finding insights, or developing a strategic narrative. Also trigger when users mention "strategy", "positioning", "insight", "brand problem", "strategic brief", "pitch deck strategy", or want to go from a business challenge to a strategic direction — even if they don't explicitly say "strategist".
---

# Strategist

Build strategy that actually works — real thinking that solves real problems and gets people to act. Based on frameworks from Sweathead (Mark Pollard) and Overnight Strategist (Shehraz Ishak).

## How to use this skill

Accept a brief or topic from the user, then walk through the 7-step process below. Adapt depth based on context:

- **quick**: Hit the key steps fast — problem, insight, strategy, actions. Good for brainstorms or time pressure.
- **full** (default): Work through all 7 steps with the user, asking questions and pressure-testing along the way.
- **deep-dive**: Go deep on research, sub-problems, and multiple insight candidates before converging.

Arguments:
- `[brief or topic]` — what you're building strategy for
- `--type brand|campaign|crm|product` — what kind of strategy
- `--output story|framework|deck` — how to format the result (see Output Formats below)
- `--depth quick|full|deep-dive` — how deep to go

## The 7-Step Process

Work through these steps sequentially. Each step builds on the previous one. Don't skip ahead — the quality of later steps depends on getting the earlier ones right.

### Step 1: Name The Real Problem

Find the actual problem, not the obvious one.

Ask:
- What's really stopping us from winning?
- What's the human struggle behind the business number?
- If we solved this, what would change?

Watch out for:
- Vague problems ("we need more awareness")
- Problems that are actually solutions in disguise ("we need a TikTok strategy")
- Surface symptoms instead of root causes

Deliver one clear sentence that makes people go "yes, that's exactly it."

### Step 2: Break It Into Pieces

Big messy problems are hard to solve. Small pieces are easier.

Ask:
- What are the different parts of this problem?
- Which pieces matter most?
- What can we actually influence?

Break down by audience (who has this problem?), by moment (when does it happen?), or by barrier (what's blocking them?).

Deliver a simple map showing the pieces of the problem.

### Step 3: Dig Into Each Piece

Look at data AND talk to people. Numbers tell you what. People tell you why.

Look for:
- Patterns in the data
- Surprises that don't fit expectations
- Things people do vs. what they say they do
- Tensions between what people want and what they get

Deliver key findings for each piece of the problem.

### Step 4: Find The Insight

An insight is a truth nobody says out loud, but everyone recognizes when they hear it.

An insight is NOT a fact ("moms are busy"), an observation ("people check phones 150 times a day"), or a data point ("73% prefer X over Y").

An insight IS a human truth that reframes the problem, makes people say "I never thought of it that way", and opens doors to new possibilities.

Test it: Does this change how we see the problem? Does it point to a solution?

Deliver one sentence that unlocks the strategy.

### Step 5: Find Your Edge

Figure out why YOU are the right one to solve this problem.

Ask:
- What do we have that others don't?
- What do we do better than anyone?
- What do people already believe about us that we can build on?
- What permission do we have that competitors don't?

The edge could be a product truth, a brand truth, a cultural position, or an access point.

Deliver the unfair advantage in one sentence.

### Step 6: Write The Strategy

Crash the insight and the edge together. That's the strategy.

Use this formula:
```
For [audience] who [insight about their situation],
[brand] is the [edge/advantage]
that [what we will do differently]
so that [outcome we want].
```

Good strategy is simple enough to remember, clear enough to make decisions with, and inspiring enough to make people want to act.

Bad strategy is a list of things to do, a goal disguised as a strategy, or so broad it could apply to any brand.

Deliver one strategic statement that guides everything.

### Step 7: Make It Happen

Turn the strategy into action. Ideas are cheap. Execution is everything.

Create:
- Key actions (what do we actually DO?)
- Priorities (what matters most?)
- Quick wins (what can we do right now?)
- Success signals (how do we know it's working?)

Every action must tie back to the strategy. Ask: are we doing things that only WE would do?

Deliver a clear action plan that teams can run with.

## Output Formats

### Story Format (`--output story`)

Walk through the thinking as a narrative:
1. **Here's what's happening** (context)
2. **Here's the real problem** (challenge)
3. **Here's what we discovered** (insight)
4. **Here's why we can win** (advantage)
5. **Here's what we'll do** (strategy)
6. **Here's how it comes to life** (actions)

### Framework Format (`--output framework`)

Sweathead-style single-page strategy (The Four Points):

| Point | Answer |
|-------|--------|
| **Problem** | [One sentence] |
| **Insight** | [One sentence] |
| **Advantage** | [One sentence] |
| **Strategy** | [One sentence] |

### Deck Outline Format (`--output deck`)

Slide structure for presentations:
1. **The Situation** — what's happening
2. **The Challenge** — the real problem
3. **The Findings** — what we learned
4. **The Insight** — the unlock
5. **The Opportunity** — where we can win
6. **The Strategy** — how we'll win
7. **The Ideas** — how it comes to life
8. **The Plan** — what happens next

## Quality Standards

Before delivering any output, validate against these checks:

**Problems** — Is it specific (not vague)? A real problem (not a solution in disguise)? Root cause (not symptoms)? Connects business need to human need?

**Insights** — Is it a truth (not just a fact)? Does it reframe the problem? Would people say "I never thought of it that way"? Does it open doors?

**Strategy** — Simple enough to remember? Clear enough to decide with? Specific to this brand (not generic)? Connects insight to action?

**Actions** — Every action ties to strategy? Priorities are clear? Mix of quick wins and long-term moves? Teams can actually execute?

## Reference Material

For deeper thinking frameworks, examples of good vs. bad strategy, and additional tools like the Insight Formula, Strategy Bridge, "Only We" Test, and "So What" Ladder — read `references/frameworks.md`.

## Scope

Use this skill for: brand strategy, campaign strategy, CRM/loyalty strategy, product positioning, pitch deck strategy sections, reframing business challenges.

Do NOT use for: tactical execution details, media planning specifics, creative concepting (use after strategy is set), or raw data analysis (do that first, then strategize).

## Integration

After strategy is complete, hand off to `/sc:copywrite` for messaging, `/sc:implement` for execution planning, or `--output deck` to create a presentation outline.

Before starting, ensure you have: research or data to work with, a clear business objective, and stakeholder alignment on the problem space.
