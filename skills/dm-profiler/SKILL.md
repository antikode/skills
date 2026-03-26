---
name: antikode-dm-profiler
description: >
  Produces a full Decision Maker Intelligence Brief for Antikode's BD team
  before a sales or partnership pitch. Trigger this skill when the user says
  any of: "run the profiler", "profile this lead", "dm brief", "decision maker
  brief", "profile [name]", "build a brief for", "intelligence brief", or
  "who is [name] at [company]". Collects structured input via a form, researches
  the company and decision maker via web search, then outputs a complete
  three-section brief: Company Summary, DM Intelligence Profile, and
  Step-by-Step Outreach Strategy.
---

# Antikode Decision Maker Intelligence Brief — Skill

Produces a research-backed, actionable intelligence brief on a target decision
maker before a BD pitch. Always follow all steps in order.

---

## Context: Who is Antikode

Antikode is a digital experience studio based in Indonesia. Core services:
- Loyalty apps and loyalty program design
- Branded web experiences and ordering platforms
- Self-order kiosk UI
- Gamification and campaign layers
- Digital brand ecosystems

Primary verticals: F&B, lifestyle retail, health & beauty, fashion, QSR.
Known clients: HokBen, Maybank, AXIS, Paragon Corp, Tzu Chi Hospital.
The goal of every brief is to help the BD team walk into a first conversation
with enough intelligence to pitch the right service, through the right person,
with the right framing.

---

## Step 1 — Collect Input via Form

Show TWO sets of questions using `ask_user_input_v0` in a single call.

### Set 1 — Lead Brief (3 questions)

```
Q1: How did this lead come to your radar? (single_select)
Options:
- Warm intro / mutual contact
- Cold — we identified them
- Partner tip (e.g. ESB, vendor)
- Event or conference
- Inbound

Q2: What services does Antikode want to pitch? (multi_select)
Options:
- Loyalty app (new build)
- Loyalty program revamp
- Web ordering platform
- Self-order kiosk UI
- Gamification / campaign layer
- Brand web / digital experience
- Not sure yet — explore first

Q3: How much do you know about the decision maker? (single_select)
Options:
- I have their name and LinkedIn
- I have their name but not much else
- I only know the company — need you to find the right person
- I have detailed background already
```

### Set 2 — Confidence on Key Data (2 questions)

```
Q4: Do you have an internal contact at this company? (single_select)
Options:
- Yes — someone who knows us well
- Yes — but loose connection
- No internal contact

Q5: Do you know their current tech stack or agency partners? (single_select)
Options:
- Yes — confirmed intel
- Partial — some signals
- No — research needed
```

Also ask inline (open text, not in widget):
> "Paste everything you know about the company and decision maker below.
> Minimum: company name + website. Ideal: name, title, LinkedIn URL,
> how you found them, what the opportunity is, any internal contacts,
> current vendors, and any other context."

---

## Step 2 — Research Phase

After receiving the form and the free-text context, run the following
searches before writing a single word of the brief. Do not skip research
even if the user has provided substantial context — always verify and enrich.

### Company Research (run all of these)
1. `[Company name] Indonesia digital strategy 2024 2025`
2. `[Company name] CEO founder leadership team`
3. `[Company name] technology vendor POS loyalty app`
4. `[Company name] news expansion funding 2025 2026`
5. `[Company name] site:linkedin.com`

### Decision Maker Research (run all of these)
1. `[Full name] [company] LinkedIn`
2. `[Full name] [company] interview podcast 2024 2025`
3. `[Full name] career background previous companies`
4. `[Full name] [company] posts articles opinion`

### Gap Research (run if data is thin)
- If DM has low LinkedIn presence: search for their name + company in
  Indonesian business press (Kontan, DailySocial, Bisnis.com, Katadata)
- If company has no clear digital vendor: search `[company] agency partner
  digital marketing Indonesia`
- If org chart is unclear: search ZoomInfo or RocketReach for company
  employee directory

### Research Quality Rules
- Mark every fact as [Confirmed], [Inferred], or [Unverified]
- If conflicting signals are found, note the conflict and give
  your best interpretation with reasoning
- If the DM has very low public presence, flag this clearly in the brief
  and adjust confidence scores on all personality insights accordingly
- Always prefer primary sources (LinkedIn posts, press interviews,
  company announcements) over aggregator summaries

---

## Step 3 — Write the Brief

Produce the full brief in three sections. Write like a senior strategist
briefing a BD team 30 minutes before a pitch. Sharp, direct, no filler.

---

### SECTION A — COMPANY SUMMARY

Output as a table with these exact rows:

| Category | Detail | Source |
|---|---|---|
| Brand / Company | Overview, scale, parent group, funding status, IDX listed or private | |
| Key Brands & Portfolio | All brands operated, their positioning, market presence | |
| Tech Stack & Vendors | POS system, loyalty platform, ordering system, digital agency, any known tech partners | |
| Digital Maturity | Honest 1-line assessment: None / Basic / Functional / Advanced. Explain why. | |
| Business Goals (12–24 months) | What they're actively trying to achieve based on public signals | |
| Current Gaps | Where their digital experience falls short relative to their brand ambition | |
| Why Now | The specific reason this is the right moment to approach — tied to a real signal | |
| Antikode Entry Point | The single most credible first service to pitch given all of the above | |

---

### SECTION B — DECISION MAKER INTELLIGENCE PROFILE

Output as a table. Every row must have exactly three columns:
**Observation | Evidence & Source | Strategic Implication for Antikode's Pitch**

Produce one row per insight (not one row per section heading).
Organize rows under the following 10 section headers:

**1. Professional Background**
Cover: career trajectory, types of companies (startup / corporate / agency /
investment / tech), likely strategic frameworks they're familiar with,
years of experience, education signals.

**2. Personality & Communication Style**
Cover: how they write and speak publicly, tone (analytical / visionary /
relational / tactical), introvert vs. extrovert signals, formality level,
how they prefer to receive information.

**3. Strategic Interests & Topics**
Cover: what they post about, themes that recur, what they seem genuinely
excited about vs. just doing their job, industry ideas they promote or challenge.

**4. Leadership & Decision-Making Style**
Cover: data-driven vs. gut-feel, fast vs. slow, top-down vs. collaborative,
how they treat vendor relationships based on available signals,
comfort with experimentation.

**5. Professional Motivation & Career Narrative**
Cover: what they want to be known for, what success looks like for them
in this role, inferred career ambitions, legacy they're building.

**6. Current Pressures & Strategic Priorities**
Cover: what problems are on their desk right now, what they're being
measured on, company pressures that affect their decisions, hiring signals.

**7. Psychological Signals**
Cover: possible professional insecurities or identity tensions, reputation
goals, need for quick wins vs. long-term transformation, risk tolerance,
what would make them look good internally.

**8. Influence Network**
Cover: people they interact with publicly, communities or industry circles
they belong to, who has influence over them, who they trust, internal
champions or blockers.

**9. Recommended Pitch Strategy**
Cover: best conversation opener (specific, not generic), messaging angle
that would resonate with this person specifically, type of evidence they
would find convincing, ideal meeting format and length.

**10. Pitch Risks — What to Avoid**
Cover: messages or approaches that would land badly given their background,
framings that conflict with their identity or priorities, mistakes other
agencies probably make with this type of person.

---

### SECTION C — STEP-BY-STEP OUTREACH STRATEGY

Output as numbered steps with clear headers.

**Step 1 — Activation**
How to get the first contact. If a warm intro exists, how to activate it
(what to say, to whom, when). If cold, which channel and why.

**Step 2 — First Outreach Message**
Specify: channel (LinkedIn / WhatsApp / email / through internal contact).
Write the actual draft message. Max 5 sentences. Must feel human, not templated.
Reference something real about their company or recent activity.

**Step 3 — The First Call**
- Format recommendation (video / in-person / phone)
- 3 questions to ask (specific to this person and this opportunity)
- 3 things NOT to do in this call
- Goal of the call (what does success look like at the end of 30 minutes)

**Step 4 — The Follow-Up Proposal**
- What format to send (deck / short doc / Figma / Notion / email)
- What to include (structure it out)
- What to leave out
- Timeline recommendation (how quickly to send after the call)

**Step 5 — Expansion Play**
Once the first project kicks off, what to pitch next.
Show the logical progression from entry point → full partnership.

**Closing Tables:**

Table 1 — What to Prepare Before Any Meeting
| Asset | Why You Need It |

Table 2 — Pitch Risks
| Risk | Why | How to Avoid |

---

## Step 4 — Present the Brief

Output the full brief directly in the chat — no file unless the user asks
for a downloadable version.

End with this line:
> "Want me to also draft the actual outreach message for Step 2, or dig
> deeper into any specific section?"

---

## Output Rules (Non-Negotiable)

- **Research before writing.** Never rely on training data alone for
  company-specific or person-specific facts. Always web search first.
- **No generic advice.** Every insight must be tied to a specific
  observation about this person or this company. If you can't support
  it with evidence, don't write it.
- **Label confidence.** Use [Confirmed], [Inferred], or [Unverified]
  on any claim that isn't directly sourced.
- **No bullet points inside table cells.** Write in full sentences.
- **Flag low public presence.** If the DM is hard to find online,
  say so clearly and adjust confidence levels across the profile.
- **Tone.** Sharp, direct, senior strategist voice. Not academic,
  not salesy, not generic. Write like the brief matters.
- **Length.** Section A: compact table (~8 rows). Section B: as many
  rows as the evidence supports — quality over quantity.
  Section C: enough detail to actually execute, not just inspire.

---

## Trigger Phrases

This skill activates on any of:
- "run the profiler"
- "profile this lead"
- "dm brief"
- "decision maker brief"
- "profile [name]"
- "build a brief for"
- "intelligence brief"
- "who is [name] at [company]"
- "brief on [company]"
- "research [name]"

On any of these triggers: go directly to Step 1. Do not ask for
clarification first — show the form immediately.

---

## Quick Re-Run (Same Company, Different Person)

If the user says "profile another person at [same company]" or
"now do [name]":
- Skip the company research (reuse Section A)
- Run only the DM research steps for the new person
- Regenerate Section B and Section C only
- Much faster — typically 60% less research needed

---

## Notes

- The Antikode credential PDF (if present in the project) should be
  read before writing Section C to personalize the value proposition
  and reference real proof points
- Known Antikode clients to reference if no PDF is available:
  HokBen, Maybank, AXIS, Paragon Corp, Tzu Chi Hospital
- Always check if the company uses ESB as POS — this is a recurring
  signal in Indonesia F&B and indicates an active digital ordering
  conversation that Antikode can join
- Never fabricate contact details (email, phone). Only reference
  what is publicly confirmed.
- If the decision maker is the founder, adjust the pitch toward
  legacy and brand vision. If they're a hired executive, adjust
  toward KPIs and internal credibility.
