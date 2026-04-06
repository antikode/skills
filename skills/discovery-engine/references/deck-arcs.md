# Deck Arc Templates

Slide structures for each project type. The `deck-content` skill selects the appropriate arc and generates slide-by-slide blueprints.

## CRM / Loyalty Arc (Insight-Led)

| # | Slide | Purpose | Key Element |
|---|-------|---------|-------------|
| 1 | Cover | First impression | Client name + Antikode + date |
| 2 | The Situation | Ground the room in context | Scale numbers, market position |
| 3 | The Real Challenge | Diagnosis — structural forces | 2-3 compounding issues |
| 4 | The Evidence | Make the case with data | 3-5 data points from research |
| 5 | The Insight | Emotional peak — reframe | One sentence that changes perspective |
| 6 | Competitive Landscape | Where client sits vs. market | Benchmark matrix |
| 7 | Why We Can Win | First-mover or unique edge | Source of advantage |
| 8 | The Strategy | Guiding policy | What we will do + will NOT do |
| 9 | The Architecture | How pieces connect | System diagram |
| 10 | The Experience | Customer journey | 3-4 key moments from customer POV |
| 11 | The Loyalty System | Reward mechanics | Tiers, points, challenges |
| 12 | vs. Competition | Named competitor comparison | Head-to-head table |
| 13 | Quick Wins | First 30 days | Tangible actions with effort estimates |
| 14 | The Roadmap | Phased timeline | 30/60/90/180 milestones |
| 15 | Proof Points | What client will know | Concrete deliverables per milestone |
| 16 | Risk Mitigation | Top 3 risks | Severity + mitigation |
| 17 | Why Antikode | Relevant credentials | Specificity over declaration |
| 18 | Investment Tiers | Engagement options | Foundation / Growth / Enterprise |
| 19 | Next Steps | Clear actions | What happens after this meeting |
| 20 | Thank You | Clean close | Contact info |

## Website Revamp Arc (Before/After)

| # | Slide | Purpose | Key Element |
|---|-------|---------|-------------|
| 1 | Cover | First impression | Client + Antikode + date |
| 2 | Current State | What exists today | Screenshots, performance scores |
| 3 | The Gaps | What's broken or missing | Audit findings (UX, perf, SEO) |
| 4 | User Impact | How gaps affect real users | Journey map with pain points |
| 5 | Market Benchmark | Where competitors are | Comparison matrix |
| 6 | The Vision | What the new site delivers | Before/after mockup or description |
| 7 | Technology Strategy | Why this stack | Architecture diagram |
| 8 | Key Features | What ships | Feature list with priority |
| 9 | Performance Targets | Measurable improvement | Score targets (PageSpeed, CWV) |
| 10 | SEO Migration | Preserving and improving ranking | Migration strategy |
| 11 | Content Strategy | What content changes | Content audit summary |
| 12 | The Roadmap | Phased delivery | Discovery → Design → Dev → Launch |
| 13 | Quick Wins | Immediate improvements | Fixes for current site |
| 14 | Investment | Pricing and scope | Tiered options |
| 15 | Why Antikode | Relevant case studies | Similar revamp projects |
| 16 | Next Steps | Clear actions | Discovery workshop → proposal |
| 17 | Thank You | Clean close | Contact info |

## New Website Arc (Vision-Led)

| # | Slide | Purpose | Key Element |
|---|-------|---------|-------------|
| 1 | Cover | First impression | Client + Antikode + date |
| 2 | The Opportunity | Why now, why a website | Market context + business need |
| 3 | The Audience | Who this is for | Persona summary |
| 4 | Competitive Landscape | What's out there | Competitor site analysis |
| 5 | The Vision | What we'll build | High-level concept |
| 6 | Information Architecture | Site structure | Sitemap / nav diagram |
| 7 | Design Direction | Look and feel | Mood board / style reference |
| 8 | Technology Stack | How we'll build it | Architecture + stack rationale |
| 9 | Key Features | Core functionality | Feature list with priority |
| 10 | Content Strategy | What content is needed | Content plan |
| 11 | Performance & SEO | Built-in optimization | Target metrics |
| 12 | The Roadmap | Build phases | Discovery → Design → Dev → Launch |
| 13 | Post-Launch | What happens after go-live | Maintenance, analytics, iteration |
| 14 | Investment | Pricing and scope | Tiered options |
| 15 | Why Antikode | Relevant case studies | Similar builds |
| 16 | Next Steps | Clear actions | Kickoff timeline |
| 17 | Thank You | Clean close | Contact info |

## General Pitch Arc (Standard Antikode)

| # | Slide | Purpose | Key Element |
|---|-------|---------|-------------|
| 1 | Cover | First impression | Client + Antikode + date |
| 2 | The Situation | Context and challenge | What's happening in client's world |
| 3 | The Challenge | Real diagnosis | Structural problem |
| 4 | The Evidence | Supporting data | Key findings from research |
| 5 | The Insight | Reframe | One sentence unlock |
| 6 | The Strategy | How we'll win | Guiding policy |
| 7 | The Solution | What we propose | Architecture / approach |
| 8 | Key Features | What ships | Prioritized feature list |
| 9 | The Roadmap | Phased delivery | Timeline with milestones |
| 10 | Quick Wins | Early value | 30-day actions |
| 11 | Why Antikode | Credentials | Relevant experience |
| 12 | Investment | Pricing | Tiered options |
| 13 | Next Steps | Actions | What happens next |
| 14 | Thank You | Close | Contact info |

## Arc Selection Logic

The `deck-content` skill reads `project.type` from the manifest or detects it from the strategy proposal:
- If strategy mentions "loyalty", "membership", "CRM", "points" → CRM/Loyalty arc
- If strategy mentions "revamp", "migration", "redesign" → Website Revamp arc
- If strategy mentions "new site", "greenfield", "build from scratch" → New Website arc
- Otherwise → General Pitch arc
