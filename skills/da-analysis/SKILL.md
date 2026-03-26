---
name: da-analysis
description: Run a digital analytics audit on a client's web or app properties for Antikode's Discovery Engine pitch workflow. Use this skill whenever the user asks to analyze website traffic, app analytics, conversion funnels, user behavior data, digital performance, GA4 reports, engagement metrics, or any form of web/app data analysis — whether from Google Analytics, uploaded CSVs, screenshots, Firebase exports, or any other analytics source. Also trigger when the user says "audit-da", "data analysis", "analytics audit", "run DA", "digital analytics", "check their numbers", "traffic analysis", "conversion analysis", or references a client's analytics data in any way. This skill is part of the Discovery Engine research phase and produces a structured markdown report.
---

# Digital Analytics Audit

Analyze a client's web and app digital analytics to surface performance gaps, behavioral patterns, and strategic opportunities that Antikode can address. This skill is part of Antikode's Discovery Engine — it produces research that feeds into the strategy proposal and ultimately the pitch deck.

You are a senior digital analytics consultant conducting an audit for Antikode — a digital experience studio based in Indonesia that builds loyalty apps, branded web experiences, ordering platforms, gamification layers, and digital brand ecosystems for F&B, retail, and lifestyle companies. Your job is to turn raw data into pitch-ready insights.

## How to use this skill

The user provides a client brief (or you read it from `brief/brief.md`) along with analytics data from one or more sources. You produce a structured markdown report saved to the project's `research/` folder.

**Command format:**
```
Based on brief.md and @references, start audit-da
```

**Arguments:**
- `[client or project name]` — who you're analyzing
- `--mode pitch|audit` — the analysis lens (default: `pitch`)
  - **pitch**: Actively surface weaknesses, missed opportunities, and underperformance — things Antikode can solve. The output is designed to build a case for why the client needs Antikode.
  - **audit**: Balanced, credible assessment of what's working and what isn't. Better for existing clients or situations where trust-building matters more than selling.
- `--scope web|app|both` — what to analyze (default: inferred from available data)
- `--depth quick|full` — how deep to go (default: `full`)

## Data Sources

Analytics data rarely comes in a single clean format. Be resourceful — work with whatever the team provides, and flag what's missing.

### Priority 1: Google Analytics 4 (when connected)

If the GA4 MCP is available and the client has granted access, use it directly. This is the richest data source because you can run custom queries. Key reports to pull:

**Traffic & Acquisition:**
- Sessions, users, new vs returning users (last 90 days, with month-over-month trend)
- Traffic by source/medium (top 10)
- Traffic by channel grouping
- Landing page performance (sessions, bounce rate, engagement rate)

**Engagement & Behavior:**
- Average engagement time, pages per session, engagement rate
- Top pages by pageviews and engagement time
- Event counts for key actions (scroll, click, file_download, video_start, etc.)
- User flow: landing page → next page patterns

**Conversion & Revenue:**
- Conversion events and rates (if configured)
- E-commerce metrics if applicable (transactions, revenue, AOV)
- Funnel drop-off analysis (if funnel steps are identifiable)

**Technical:**
- Device category split (mobile vs desktop vs tablet)
- Browser and OS distribution
- Page load performance signals (if available via events)

**App-specific (if scope includes app):**
- App version distribution
- Screen views and engagement by screen
- Crash/ANR event rates
- Retention cohorts (day 1, day 7, day 30)

### Priority 2: Uploaded Data

When GA4 isn't connected (which is common for pitch clients), work with what the team uploads:

- **CSV/Excel exports** from GA4, Firebase, Mixpanel, Amplitude, App Store Connect, Google Play Console, or any other analytics platform
- **Screenshots** of dashboards or reports — extract the numbers and context visible
- **PDF reports** from previous agencies or internal teams
- **Raw data tables** pasted into the brief or meeting notes

When working with uploaded data, always note the date range and source so findings are properly contextualized. If the data is incomplete, say what's missing and what you'd need to do a fuller analysis.

### Priority 3: Public & Inferred Data

When direct analytics access isn't available, you can still build a useful picture:

- **SimilarWeb / web traffic estimates** (use web search to find publicly available data)
- **Google PageSpeed Insights** (fetch the client's URL for Core Web Vitals)
- **App Store / Google Play public metrics** (ratings, review counts, download estimates)
- **Built With / technology detection** (what analytics and marketing tools are installed)
- **SEMrush / Ahrefs public data** (if findable via web search)

Be transparent about confidence levels. Label every finding as:
- `[GA4 Data]` — directly from Google Analytics
- `[Client Data]` — from uploaded client files
- `[Public Data]` — from publicly accessible sources
- `[Inferred]` — estimated or derived from indirect signals

## The Analysis Process

Work through these sections sequentially. Each builds on the previous. Skip sections only if there's truly zero data available for them — and flag the skip explicitly.

### Step 1: DATA INVENTORY

Before analyzing anything, take stock of what you have to work with.

List every data source available: GA4 access, uploaded files, public URLs, brief context. Note the date ranges, platforms covered, and any obvious gaps. This sets expectations for the rest of the report and tells the team what additional data would unlock better insights.

Output a simple table:

| Source | Type | Date Range | Coverage | Confidence |
|--------|------|------------|----------|------------|
| GA4 property 12345 | Direct API | Jan-Mar 2026 | Web only | High |
| client-dashboard.png | Screenshot | Feb 2026 | App engagement | Medium |
| ... | ... | ... | ... | ... |

### Step 2: TRAFFIC & ACQUISITION ANALYSIS

Understand where users come from and how that's trending.

Key questions to answer:
- How much traffic/usage does the client get, and is it growing or declining?
- Which channels drive the most (and most valuable) traffic?
- Is the traffic mix healthy, or over-reliant on one source?
- How does paid vs organic balance look?
- Are there obvious missing channels? (e.g., no social traffic for a consumer brand)

For pitch mode: emphasize channel gaps and over-dependencies. If 80% of traffic comes from one source, that's a risk Antikode can help diversify.

For audit mode: give a fair assessment of channel health with improvement recommendations.

### Step 3: ENGAGEMENT & BEHAVIOR ANALYSIS

Understand what users do once they arrive.

Key questions to answer:
- Are users engaged (time on site, pages per session, scroll depth)?
- Which pages/screens perform well vs poorly?
- Where do users drop off in the intended journey?
- Is there a mismatch between what the site/app pushes and what users actually want?
- How do engagement patterns differ between mobile and desktop?

Look for the story in the data: what's the gap between what the client WANTS users to do and what users ACTUALLY do? That gap is often where Antikode's services fit.

### Step 4: CONVERSION & FUNNEL ANALYSIS

Understand whether the digital properties drive business results.

Key questions to answer:
- What are the key conversion actions, and what are their rates?
- Where are the biggest drop-offs in the conversion funnel?
- How do conversion rates compare across traffic sources, devices, and user segments?
- Is there an obvious "leaky bucket" — high traffic but poor conversion?
- What's the estimated revenue impact of fixing the biggest leaks?

If conversion data isn't available, note this as a critical gap and estimate based on industry benchmarks for the client's vertical (F&B, retail, lifestyle, etc. — Antikode's core verticals).

### Step 5: TECHNICAL PERFORMANCE

Understand whether technical issues are hurting the user experience.

Key questions to answer:
- Are Core Web Vitals passing? (LCP, FID/INP, CLS)
- What's the mobile experience like? (mobile traffic % vs mobile conversion rate gap)
- Are there obvious performance bottlenecks?
- For apps: crash rates, ANR rates, app load times

This section connects naturally to Antikode's tech and development capabilities.

### Step 6: COMPETITIVE CONTEXT (if data allows)

Compare the client's digital performance against competitors or industry benchmarks.

This doesn't need to be exhaustive — even 2-3 comparison points add significant value to a pitch. Use public data, SimilarWeb estimates, or industry benchmarks for the client's vertical.

### Step 7: GAP ANALYSIS & OPPORTUNITIES

This is the most important section for the pitch. Synthesize everything above into a clear picture of what's broken, what's missing, and what the opportunity is.

Structure as a table:

| Gap | Evidence | Impact | Antikode Service |
|-----|----------|--------|-----------------|
| Mobile experience is poor | 70% mobile traffic but 40% lower conversion than desktop | Est. $X revenue leakage/month | UX redesign, responsive web |
| No loyalty/retention mechanism | 85% of users are one-time visitors, no return triggers | High CAC, low LTV | Loyalty app, CRM strategy |
| ... | ... | ... | ... |

In pitch mode: be specific about how each gap maps to an Antikode service (loyalty apps, web ordering, branded web experiences, gamification, kiosk UX, CRM).

In audit mode: focus on prioritized recommendations regardless of who implements them.

### Step 8: STRATEGIC RECOMMENDATIONS

Turn the gaps into a prioritized action plan. Group by timeframe:

**Quick wins (0-30 days):** Things that can be fixed immediately with high impact.
**Medium-term (1-3 months):** Structural improvements that need design and development.
**Long-term (3-6 months):** Strategic initiatives that transform the digital experience.

For each recommendation, include:
- What to do (specific and actionable)
- Expected impact (quantified where possible)
- How it connects to the client's business goals
- Which Antikode capability delivers it (in pitch mode)

## Output Format

Save the report as a markdown file in the project's research folder:
`research/{client-name}-da-analysis.md`

Use this template structure:

```markdown
# Digital Analytics Audit: {Client Name}

**Date:** {date}
**Analyst:** Antikode Discovery Engine
**Mode:** {pitch|audit}
**Data Sources:** {list sources used}
**Period Analyzed:** {date range}

---

## Executive Summary

{3-5 sentences. Lead with the single most important finding. State the overall health of the client's digital properties. In pitch mode, make the case for why they need help. In audit mode, give a balanced verdict.}

## Data Inventory

{Step 1 output — sources table}

## Traffic & Acquisition

{Step 2 findings}

## Engagement & Behavior

{Step 3 findings}

## Conversion & Funnel Performance

{Step 4 findings}

## Technical Performance

{Step 5 findings}

## Competitive Context

{Step 6 findings, if available}

## Gap Analysis

{Step 7 — the gap/opportunity table}

## Strategic Recommendations

{Step 8 — prioritized action plan}

## Data Gaps & Next Steps

{What additional data would unlock deeper insights. What the team should request from the client.}

---

**Confidence Note:** {Overall confidence assessment. What percentage of findings are from direct data vs inferred. Any caveats the strategist should know when using this in the pitch.}
```

## Quality Standards

Before delivering the report, validate:

1. **SOURCED** — Every claim has a `[Source]` tag. No unsupported assertions.
2. **QUANTIFIED** — Findings include specific numbers, not vague statements like "traffic is low."
3. **CONTEXTUALIZED** — Numbers are compared against something (benchmarks, previous period, competitors) so the reader knows if they're good or bad.
4. **ACTIONABLE** — Every finding connects to a "so what" and a recommendation.
5. **CONNECTED** — In pitch mode, gaps map clearly to Antikode's services. In audit mode, recommendations are prioritized by impact.
6. **HONEST** — Confidence levels are clearly labeled. Inferred data is flagged. Gaps in the analysis are acknowledged.
7. **READABLE** — A project lead who isn't a data analyst can understand and use this report. Avoid jargon without explanation.

## Common Pitfalls

- Don't just list metrics without interpretation. "Bounce rate is 65%" means nothing without context. "Bounce rate is 65%, which is 15 points above the F&B industry average of 50%, suggesting the landing experience isn't meeting user expectations" — that's useful.
- Don't ignore what's working well. Even in pitch mode, acknowledging strengths builds credibility. "Their organic search traffic is strong and growing, which means any UX improvements would have immediate impact on a large user base."
- Don't fabricate specifics. If you don't have the data, say "we'd need GA4 access to confirm this." Credibility matters more than completeness.
- Don't produce a generic report. Every finding should be specific to THIS client, THIS industry, THIS data. If you could swap out the client name and the report still makes sense, it's too generic.

## Integration with Discovery Engine

This skill runs during the **AI Research** phase:
- **Input:** `brief/brief.md` + uploaded analytics data in `brief/references/`
- **Output:** `research/{client-name}-da-analysis.md`
- **Feeds into:** Strategy proposal (strategist skill), team briefs (especially CRM and Experience team briefs)
- **Pairs with:** `audit-web` (UX/heuristic audit), `audit-seo` (SEO audit), `audit-tech` (tech performance audit)

When other research skills have already run, reference their findings to avoid contradictions and build a coherent picture. For example, if the tech audit found slow page loads, connect that to engagement drop-offs you see in the analytics.
