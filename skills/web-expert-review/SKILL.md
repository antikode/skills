---
name: web-expert-review
description: >
  Expert website audit skill for Antikode's experience design team. Produces a
  strategic, C-suite-ready markdown audit report covering UX, performance, SEO,
  AI readiness, navigation, accessibility, and competitive benchmarking.
  Trigger this skill when the user says "audit-web", "website audit",
  "audit this site", "review this website", "site review", "web audit",
  "UX audit", "run an audit", or any request to evaluate, review, or analyze
  a live website for pitch preparation. Also trigger when the user uploads a
  URL and asks for an expert review, competitive analysis, or UX teardown.
  This skill is specifically designed for the Indonesian and Southeast Asian
  market context. Even if the user doesn't say "audit" explicitly — if they
  want a website evaluated for improvement opportunities, use this skill.
---

# Web Expert Review

Expert website audit for Antikode pitch preparation.
Produces a strategic markdown report that a senior strategist would present to C-suite.

Read this entire file before starting any work.

---

## Command: `audit-web`

When the user says **"audit-web"**, begin the audit workflow immediately:
1. Check for `brief.md` first (Step 0). If it exists, also check for **brief/references/** folder (optional supporting frameworks)
2. Then proceed to pre-flight questions (Step 1), skipping anything already answered in the brief

**Note on References:** The brief/references/ folder contains optional deep-dive frameworks:
- `wcag-2.1-framework.md` — Use in Sections 4 & 6 if accessibility is a focus area
- `jtbd-framework.md` — Use in Section 8 for journey gap analysis
- `cx-maturity-model.md` — Use in Section 7 for competitive positioning & realistic roadmaps
- `technical-seo-checklist.md` — Use in Section 5 for detailed SEO evaluation

Only reference these if the brief or user request mentions them. Don't force them on every audit.

Do not ask "what would you like me to do?" — start working.

---

## Step 0 — Read the Brief

Before asking any questions, check if a file named **`brief.md`** exists in the conversation uploads or project files. Check **brief/references/** folder first. This folder may contain supporting reference frameworks for deeper analysis:
- `wcag-2.1-framework.md` — Accessibility audit framework
- `jtbd-framework.md` — Jobs to be Done analysis
- `cx-maturity-model.md` — Customer experience maturity assessment
- `technical-seo-checklist.md` — Technical SEO evaluation

These are optional context; only use if the brief mentions them.

**If `brief.md` is found:**
1. Read it in full
2. Extract the **brand/client name** — this becomes the `[brand]` slug used in the output filename
3. Extract any other audit inputs already provided (target URL, industry, competitors, conversion goal, audience, language, focus areas)
4. Store everything internally — do not ask the user to repeat anything already in the brief
5. Proceed to Step 1, asking only for inputs that are still missing

**If `brief.md` is not found:**
1. Ask the user: *"Do you have a brief.md file to upload? It helps me pre-populate the audit details. You can skip this and I'll ask the questions manually."*
2. If the user uploads it → read and extract as above
3. If the user skips → proceed to Step 1 and ask everything

### How to derive the brand slug

The brand slug is a lowercase, hyphenated version of the client/brand name from `brief.md`. This slug is used in the output filename.

| Brand name in brief | Slug |
|---------------------|------|
| Bank Mandiri | `bank-mandiri` |
| HokBen | `hokben` |
| Telkomsel | `telkomsel` |
| Paragon Corp | `paragon-corp` |
| Tzu Chi Hospital | `tzu-chi-hospital` |

If the brief contains both a parent company and a product brand, use the **product brand** (the one the website belongs to).

---

## Step 1 — Pre-Flight Questions

Check what you already have from `brief.md`. Only ask for what's still missing.

If **nothing** was provided via brief, collect all inputs below. If some were provided, skip those and ask only for the gaps — in a single turn.

### Required Inputs (use ask_user_input_v0)

```
Q1: Language for the report (single_select)
Options: English, Bahasa Indonesia, Bilingual (both)

Q2: Who is the primary audience for this report? (single_select)
Options: C-suite / decision makers, Marketing team, Product / tech team, Mixed stakeholders

Q3: What is the primary conversion goal? (single_select)
Options: Lead generation (form / WhatsApp), E-commerce purchase, App download / signup, Brand awareness / engagement, Other (I'll specify)
```

### Required Inputs (open-text)

These should be available in `brief.md` — read from the brief first. If any are unavailable, ask the user in the same message, before the widget:

> I need a few details before starting:
> 1. **Target URL** — the website to audit
> 2. **Client industry** — e.g., Telco, Healthcare, F&B, Fintech, Beauty, Retail, Energy, Web3
> 3. **Competitor URLs** — at least 2, ideally 3–4 (with a one-line description each)
> 4. **Anything specific** you want me to focus on? (optional)

Do NOT proceed until you have: target URL, industry, at least 2 competitor URLs, language choice, conversion goal, and a brand name (from brief or from the user). If any are missing, ask again — one specific question at a time.

---

## Step 2 — Site Research

Follow these rules strictly.

### 2A: Crawl the Target Site

Use `web_fetch` to retrieve the target URL. Then systematically fetch:

1. **Homepage** — the target URL itself
2. **Key subpages** — look for nav links in the HTML and fetch at least 5 inner pages (about, product/service pages, contact, blog/news, pricing if applicable)
3. **Subdomains** — this is critical. Many Indonesian enterprise sites host features on subdomains (e.g., `shop.brand.com`, `dealer.brand.com`, `app.brand.com`, `my.brand.com`). Search for subdomain references in the HTML and fetch them.

**Save a structured inventory** of every page you actually fetched and its status. You will reference this inventory when writing findings.

### 2B: Identify Navigation Type Correctly

A common error is misidentifying the navigation pattern. Use these definitions:

| Pattern | How to Identify |
|---------|----------------|
| **Hamburger menu** | A toggle button (usually ☰ or three lines) that reveals a vertical list of links. Typical on mobile-only or minimal sites. Look for `class="hamburger"`, `toggle`, `mobile-menu`, or an SVG with three horizontal lines. |
| **Mega menu** | A wide dropdown panel that appears on hover/click of a top-level nav item, showing multiple columns of links, sometimes with images or featured content. Look for `class="mega-menu"`, multi-column `<div>` grids inside `<nav>`, or dropdowns wider than 400px with nested link groups. |
| **Standard dropdown** | A single-column list that drops below a nav item. Narrower than a mega menu, no columns. |
| **Sticky/fixed nav** | The navigation bar stays visible when scrolling. Look for `position: fixed` or `position: sticky` on the `<nav>` or `<header>` element. |
| **Tab navigation** | Horizontal tabs that switch content panels on the same page without a page reload. |
| **Sidebar nav** | A persistent vertical menu on the left or right, common in dashboards and documentation sites. |

A site can use **multiple patterns simultaneously** — for example, a sticky mega menu on desktop that collapses into a hamburger on mobile. Report all observed patterns and specify which breakpoint each applies to.

When reporting navigation type: state what you observed in the HTML, not what you assume. If the site uses JavaScript rendering that hides the nav structure, say so explicitly and flag it as a crawl limitation.

### 2C: Detect Features Accurately — Including Subdomains

Another common error is reporting that a feature is missing when it actually exists on a subdomain or behind a JavaScript-rendered section.

**Before reporting any feature as missing, do ALL of these:**

1. Search the homepage HTML for keywords related to that feature (e.g., "dealer", "locator", "calculator", "chat", "WhatsApp")
2. Check if links point to a subdomain (e.g., `href="https://dealers.brand.com"`)
3. Use `web_search` with `site:brand.com [feature]` to find it on any subdomain
4. Check the footer — Indonesian enterprise sites often put tool links in the footer
5. Check the mobile viewport — some features only appear on mobile breakpoints

**If you still can't find it after all 5 checks:** report it as "Not found on primary domain or indexed subdomains" — not "Missing."

**If you find it on a subdomain:** report it as "Available on [subdomain URL]" and note whether it's well-integrated with the main site navigation or siloed.

### 2D: Crawl Competitors

Fetch at least the homepage + 2 inner pages for each competitor. Record the same feature inventory.

### 2E: Crawl Limitation Log

At the end of research, compile a list of pages/features you could not verify due to:
- JavaScript-only rendering (SPAs, React/Vue/Angular sites with no SSR)
- Login-gated content
- Geo-restricted pages
- Rate limiting or bot blocking

This log goes into the report footer. Honesty about limitations builds credibility.

---

## Step 3 — Analysis & Scoring

With your crawl data in hand, analyze across all dimensions defined in the Report Template below.

### Nielsen's 10 Heuristics Scoring

Score each heuristic 1–10 based on **observed evidence only**. For each score, cite the specific page and element that justified it. A score without a cited element is not valid.

| Score Range | Meaning |
|-------------|---------|
| 9–10 | Excellent — best-practice implementation, delightful |
| 7–8 | Good — functional with minor gaps |
| 5–6 | Adequate — works but has notable friction |
| 3–4 | Below average — clear usability problems |
| 1–2 | Poor — actively harms user experience |

### Performance Estimation

Since you cannot run Lighthouse directly, estimate based on:
- Image formats and sizes observed in HTML (WebP vs JPG/PNG, lazy loading attributes)
- Number of external scripts and stylesheets
- Presence of resource hints (`preconnect`, `preload`, `dns-prefetch`)
- Presence of service worker registration
- CDN usage (check asset domains)

Always state these are **estimated ranges based on HTML inspection**, not measured scores.

### Competitive Benchmark Matrix

For the feature matrix, only mark ✅ if you verified the feature exists through actual crawling. Mark ❌ only after completing the 5-check process from Step 2C. Use "Unverified" if you couldn't check due to crawl limitations.

---

## Step 4 — Write the Report

Follow the **Report Template** below for the exact structure. Every section, in order, no skipping, no renaming.

### Writing Principles

- **Lead with insight, not observation.** Don't say "The hero image is large." Say "The 2.4MB uncompressed hero image adds ~3 seconds to first paint on Indonesian 4G networks, directly impacting the 68% of users who arrive on mobile."
- **Be direct about problems.** Don't soften findings to the point of losing impact. A C-suite reader wants to know what's broken and what it costs them.
- **Frame every gap as an opportunity with a specific fix.** Never say "improve SEO." Say "Add JSON-LD Organization and Product schema to the homepage and all product pages to appear in Google AI Overviews — no competitor has done this yet."
- **Every bullet earns its place.** If a point doesn't name a specific page, element, or metric, delete it.
- **Industry context matters.** A missing WhatsApp CTA on an Indonesian F&B site is a critical gap. The same finding on a global SaaS platform is a nice-to-have. Calibrate severity accordingly.

Avoid these filler phrases entirely: "it is worth noting," "one could argue," "in conclusion," "it should be mentioned," "interestingly."

---

## Step 5 — Save and Present

### Output Filename

```
outputs/[brand-slug]-expert-audit.md
```

The `[brand-slug]` is derived from the client/brand name extracted in Step 0 (see slug table). Examples:
- Brand "Telkomsel" → `outputs/telkomsel-expert-audit.md`
- Brand "Bank Mandiri" → `outputs/bank-mandiri-expert-audit.md`
- Brand "Paragon Corp" → `outputs/paragon-corp-expert-audit.md`

### Save Steps

1. Save the report to `outputs/[brand-slug]-expert-audit.md`
2. Use `present_files` to share it
3. After saving, flag any sections where confidence is low due to crawl limitations
4. Inform the user that the audit report has been generated in markdown format.

---

## Quality Gate

Before saving, every section must pass:

1. **Inspiring** — Does it spark ideas or energy in the reader?
2. **Smartly on point** — Is every finding specific (names a page, element, or metric)?
3. **Wow factor** — Is there at least one insight that would make a CMO sit up?
4. **Honesty** — Are crawl limitations clearly disclosed?
5. **No hallucination** — Is every feature claim backed by an actual fetched page? Did you check subdomains before marking anything "missing"?

If any answer is no — revise before saving.

---

## What This Skill Never Does

- Report a feature as "missing" without completing the 5-check verification process
- Misidentify navigation patterns (hamburger vs mega menu vs dropdown) — always cite the HTML evidence
- Deliver findings that could apply to any website — every observation names a specific page or element
- Use vague recommendations like "improve SEO" or "enhance UX" without a concrete action
- Skip the competitive benchmark
- Save a report without the Antikode footer
- Proceed when a competitor URL or client industry is unclear
- Guess at PageSpeed scores — always frame as estimated ranges with methodology disclosed

---

## Markets & Industries Context

**Primary market:** Indonesia and Southeast Asia — B2B enterprise and consumer brands.

**Industries:** Telco, Healthcare, Entertainment, F&B, Beauty, Web3, Energy, Financial Services, Consumer Brands & Retail.

Always calibrate findings to the industry:
- **F&B / Retail in Indonesia:** WhatsApp CTA and store locator are table stakes. Missing either is a critical gap.
- **Fintech / Financial Services:** Trust signals (OJK license, security badges, testimonial proof) matter more than flashy UX.
- **Telco:** Self-service portals, plan comparison tools, and coverage maps are expected.
- **Healthcare:** HIPAA-equivalent compliance signals, doctor search, and appointment booking are baseline.
- **B2B Enterprise:** Case studies, ROI calculators, and clear pricing/contact paths drive conversion.

---
---

# Report Template

This is the exact output structure every audit report must follow.
Do not skip sections. Do not rename sections. Write them in this order.

---

## Report Header

Always start the report file with:

```markdown
# [Client Name] Website Audit
## Insight Summary for Pitch Preparation

**Audit Date:** [YYYY-MM-DD]
**Subject:** [client-url]
**Scope:** UX, Performance, SEO, AI Readiness, Navigation, Accessibility, Competitive Benchmark
**Purpose:** [One sentence — what this audit is for and what conversion outcome it targets]
```

---

## Section 1 — Executive Summary

Three parts, in this order:

1. **Opening paragraph:** Overall site verdict — is it functional, underperforming, strong but dated? State the why.
2. **Core tension:** One bold paragraph naming the gap between brand positioning and actual site experience. This is the hook that makes a CMO care.
3. **Key headline findings:** Bullet list, minimum 6 items. Each must name the exact page or element. No generic observations.

Example of a good headline finding:
> "The product comparison page loads 47 JS bundles but has no structured data — invisible to Google AI Overviews and Perplexity."

Example of a bad headline finding:
> "The website could benefit from better performance optimization."

---

## Section 2 — Site Performance

### Technical Stack
Identify: CMS platform, asset CDN, image formats in use, lazy loading implementation, JS framework if detectable.

### Performance Observations
Cover all of these:
- Hero image delivery — size, format, whether mobile/desktop variants exist
- JS-heavy interactive elements and their impact on Time to Interactive
- Service worker / PWA presence or absence
- Preconnect and resource hints (`<link rel="preconnect">`, `<link rel="preload">`)

### Performance Verdict
Estimated PageSpeed score range for both mobile and desktop, with justification based on what you observed in the HTML. Mobile is always the priority for Indonesian users — state this explicitly.

Always include: "These are estimated ranges based on HTML inspection, not measured Lighthouse scores."

### Recommendation
Specific CDN, image format, and rendering improvements. Name the exact images or scripts that need attention.

---

## Section 3 — Navigation & Information Architecture

### Current Navigation Map
Output as a code block tree showing the full nav structure:

```
├── Home
├── Products
│   ├── Category A
│   ├── Category B
│   └── Category C
├── About
│   ├── Company
│   └── Careers
├── Contact
└── [Footer links]
```

### Navigation Type
State the observed pattern (mega menu, hamburger, dropdown, etc.) and cite the HTML evidence that confirms it. If the site uses multiple patterns across breakpoints (e.g., mega menu on desktop, hamburger on mobile), report both and specify which applies where.

### Structural Issues
Number each issue explicitly:
- **Issue 1:** [specific problem with specific nav element]
- **Issue 2:** [etc.]

Cover: vague labels, buried tools/pages, duplicate content paths, outdated sections, important pages missing from nav, footer inconsistencies with main nav.

---

## Section 4 — Usability & Accessibility

### Usability Strengths
Minimum 3 strengths. For each, note how it could go further — nothing is "perfect."

### Lead Capture Gaps
Evaluate each of these for the Indonesian market:
- Floating CTA presence/absence
- **WhatsApp integration** — this is non-negotiable for Indonesian sites. If missing, flag as critical.
- Live chat / chatbot
- Exit-intent overlays
- Scroll-triggered prompts
- Contact form friction (number of fields, required fields, error handling)

### Mobile Experience
- Navigation depth on mobile (how many taps to reach key content?)
- CTA size and contrast (thumb-friendly? visible without scrolling?)
- Click-to-call visibility
- Touch target spacing

### Accessibility Concerns

**Reference:** See `brief/references/wcag-2.1-framework.md` for detailed accessibility audit criteria and checklists.

Evaluate against WCAG 2.1 AA standard:
- Keyboard navigation functionality (can users tab through all interactive elements?)
- Alt text quality on images (descriptive, not generic)
- WCAG color contrast compliance (4.5:1 for normal text, 3:1 for large text)
- Video captions (if video exists)
- Screen reader compatibility indicators (ARIA labels, semantic HTML)
- Touch target sizing (minimum 48x48 pixels on mobile)
- Focus indicators (visible keyboard focus state on all interactive elements)

**Indonesian Market Note:** Older demographic (40+) heavy users; contrast and readability critical for market penetration.

---

## Section 5 — AI Friendliness & SEO

**Reference:** See `brief/references/technical-seo-checklist.md` for comprehensive SEO audit framework and actionable checklists.

### Content Strategy
- Language consistency across pages (mixed languages without clear logic?)
- FAQ presence and quality
- Content volume and publishing cadence (blog/news section)
- Content length vs. topic depth (product pages 500–800 words; blog posts 1500–3000 words)

### Technical SEO

Use the technical SEO checklist to evaluate:

**Crawlability & Indexing:**
- Robots.txt: Does it allow crawling? Any critical pages accidentally blocked?
- XML Sitemap: Present, valid, and submitted to Google Search Console?
- Meta Robots Tags: Correct `index/noindex` values?
- Canonical Tags: Properly implemented for pagination, parameter URLs, duplicate content?
- HTTP Status Codes: Correct 200/301/404 codes?
- HTTPS/SSL: Entire site served over HTTPS? No mixed content warnings?

**Site Architecture:**
- URL patterns (clean, descriptive, or messy?)
- Title tag quality (unique per page? 50–70 characters? keyword-relevant?)
- Meta descriptions (compelling, 150–160 chars, unique per page?)
- Heading hierarchy (H1 per page, logical hierarchy H1→H2→H3?)
- Image optimization (alt text, WebP format, lazy loading, file size)

**Page Speed & Performance (Core Web Vitals):**
- LCP (Largest Contentful Paint): < 2.5 seconds
- FID (First Input Delay): < 100ms
- CLS (Cumulative Layout Shift): < 0.1
- Mobile responsiveness: Works on 320px–768px viewports
- Touch targets: Minimum 48x48 pixels

### AI Discoverability

This is a growing discovery channel — flag first-mover opportunities aggressively.

**Schema Markup & Structured Data:**
- Organization schema: Present with logo, contact info?
- Product schema: Price, rating, image, availability?
- FAQ schema: FAQ content marked with schema?
- LocalBusiness schema: For services with physical locations?
- BreadcrumbList: Navigation hierarchy marked?

**Content Crawlability:**
- JSON-LD structured data: What types are present? What's missing?
- Crawlability of JS-rendered content for LLMs (SPAs with SSR vs. client-side only)
- Spec data accessibility for AI search surfaces (Google AI Overviews, Perplexity, ChatGPT search)
- Whether key product/service information is in crawlable HTML vs locked in images, PDFs, or JS-rendered widgets

**Indonesian Market Note:** Bilingual sites need separate language declarations and `hreflang` tags; implement technical SEO for both `/id/` and `/en/` sections independently.

---

## Section 6 — UX Heuristics Evaluation (Nielsen's 10)

**Reference:** See `brief/references/wcag-2.1-framework.md` for accessibility compliance baseline that complements Nielsen's heuristics.

**How Nielsen's 10 Complements WCAG 2.1:**
- Nielsen evaluates **usability friction** (is the interface easy to use?)
- WCAG evaluates **accessibility compliance** (can assistive tech users access the site?)
- Together: A site can be usable but not accessible, or accessible but poorly designed.

Output as a markdown table:

```markdown
| # | Heuristic | Score | Key Finding |
|---|-----------|-------|-------------|
| 1 | Visibility of System Status | X/10 | [specific finding with page reference] |
| 2 | Match Between System & Real World | X/10 | [finding] |
| 3 | User Control & Freedom | X/10 | [finding] |
| 4 | Consistency & Standards | X/10 | [finding] |
| 5 | Error Prevention | X/10 | [finding] |
| 6 | Recognition Over Recall | X/10 | [finding] |
| 7 | Flexibility & Efficiency | X/10 | [finding] |
| 8 | Aesthetic & Minimalist Design | X/10 | [finding] |
| 9 | Help Users Recognize & Recover from Errors | X/10 | [finding] |
| 10 | Help & Documentation | X/10 | [finding] |
```

Every score must cite a specific page and element. A score without evidence is not valid.

**Scoring Tips per Heuristic:**

1. **Visibility of System Status** — Does user always know where they are? Is loading state clear? (Check homepage, category pages, checkout flow)
2. **Match Between System & Real World** — Does language/metaphor match user expectations? (Check product descriptions, CTAs, error messages)
3. **User Control & Freedom** — Can users undo/redo? Exit easily? (Check modal dialogs, wizards, navigation)
4. **Consistency & Standards** — Do buttons look the same? Links the same color? (Check all pages for pattern consistency)
5. **Error Prevention** — Are forms validated before submission? Confirmation dialogs? (Check forms, deletions, payment)
6. **Recognition Over Recall** — Are options visible or must users remember? (Check checkout, filters, navigation)
7. **Flexibility & Efficiency** — Can power users skip steps? Shortcuts available? (Check keyboard nav, direct links)
8. **Aesthetic & Minimalist Design** — Is visual hierarchy clear? Unnecessary elements removed? (Check clutter, whitespace, visual weight)
9. **Help Users Recognize & Recover from Errors** — Are errors explained clearly? Can users fix easily? (Check form errors, 404 pages, auth failures)
10. **Help & Documentation** — Is help easy to find? Is it searchable? (Check FAQ, documentation, contact options)

End with: **Overall UX Heuristic Score: X.X/10** (calculated average of all 10).

**WCAG Accessibility Score** (separate from Nielsen heuristics, for completeness):
Use the WCAG checklist to flag specific compliance violations (contrast, alt text, keyboard nav, etc.). Report as:
- ✅ Full compliance (WCAG 2.1 AA on all audited pages)
- ⚠️ Minor violations (1–3 isolated issues)
- ❌ Major compliance gaps (4+ violations or sitewide issues)

---

## Section 7 — Competitive Benchmark

**Reference:** See `brief/references/cx-maturity-model.md` for CX maturity framework that positions each competitor within industry benchmarks (Levels 1–5).

### Maturity Assessment

Before benchmarking features, assess each brand's CX maturity level:

| Level | Characteristics | Example |
|-------|---|---|
| **Level 1** (Reactive) | Basic web presence; email/phone contact only | Traditional brochure site |
| **Level 2** (Responsive) | Mobile-responsive; basic CRM; WhatsApp contact | Growing digital presence |
| **Level 3** (Reliable) | Omnichannel support; live chat; personalization | Mature e-commerce/fintech |
| **Level 4** (Proactive) | AI chatbots; predictive analytics; omnichannel agents | Advanced fintech/platform |
| **Level 5** (Adaptive) | Fully autonomous systems; customer co-creation | Hyperscale ecosystem (rare) |

**Assignment:**
- **Client:** Level ___ (e.g., Level 2 — has mobile, WhatsApp, email capture; missing live chat + personalization)
- **Competitor A:** Level ___ 
- **Competitor B:** Level ___
- **Competitor C:** Level ___

This shows the **maturity gap** and informs realistic roadmap recommendations.

### Competitors Analyzed
Numbered list with URL and one-line description:
1. [Competitor Name](URL) — one-line description of what they are
2. [etc.]

### Benchmark Matrix

```markdown
| Feature | [Client] | [Competitor 1] | [Competitor 2] | [Competitor 3] |
|---------|----------|----------------|----------------|----------------|
| Maturity Level | L2 | L3 | L3 | L2.5 |
| WhatsApp Integration | ✅ / ❌ / Partial | ... | ... | ... |
```

Use these labels only:
- ✅ = Verified present (you fetched the page and confirmed)
- ❌ = Verified absent (completed the 5-check process)
- Partial = Exists but incomplete or poorly implemented
- Unverified = Could not check due to crawl limitations

**Minimum feature rows** (add more if relevant to the industry):
- WhatsApp Integration
- Live Chat / Chatbot
- TCO / Savings Calculator
- Installment Calculator
- Model Comparison Tool
- 360° / Interactive Viewer
- Dealer / Store Locator with Map
- Online Booking / Scheduling
- Product / Service Education Content
- Customer Testimonials
- Social Proof / Awards
- Bilingual Content (ID + EN)
- Mobile App Integration
- Structured Data / Schema

### Key Competitive Insights & Opportunities

**Minimum 5 observations** (increased from 4):

1. **Maturity Gap:** Client is 1–2 levels behind Competitors A & B. Realistic pathway: L2 → L2.5 (6 months) → L3 (12 months)
2. **First-Mover Opportunities:** What features no competitor has? (e.g., "None of the 4 sites offer a financing calculator — first-mover advantage opportunity")
3. **Feature Parity:** Where is client matching competition? Where lagging?
4. **Quality Gaps:** Does competitor offer the feature but with poor UX? (e.g., "Chat available but responds in >30 min")
5. **Market Positioning:** Which competitor is closest in segment/audience? Which is most advanced?

Always flag any first-mover opportunity that no competitor has taken yet.

---

## Section 8 — User Journey Gaps

**Reference:** See `brief/references/jtbd-framework.md` for Jobs to Be Done methodology that complements Nielsen's usability focus.

**Why Both Frameworks Matter:**
- **Nielsen's 10 Heuristics** (Section 6) answer: "Is the interface easy to use?"
- **JTBD** (this section) answers: "Does the interface help users accomplish their actual goal?"

A site can be usable but not useful if it doesn't support the job users came to do.

### Current Journey Map

First, identify the **primary user jobs** (functional, emotional, social):

**Example: SME Loan Shopper**
- **Functional Job:** Get working capital approval in <7 days
- **Emotional Job:** Feel confident the decision won't backfire
- **Social Job:** Be seen as a savvy operator who found a good deal

Then map the observed journey:

```
Awareness (Search "quick business loan") 
  → Homepage (company landing)
  → Product page (loan features)
  → Eligibility check (calculator?)
  → Application (form)
  → Decision (approval/denial)
  → Onboarding (account setup)
  → Usage (payment, support)
```

Identify where the journey **breaks or stalls:**
- Where do users typically abandon? (e.g., "Application form has 15 fields; users drop off at field 8")
- Where are emotional barriers? (e.g., "No testimonials on product page; users doubt credibility")
- Where is reassurance missing? (e.g., "No approval timeline mentioned; users anxious about wait")

### Critical Journey Gaps

Minimum 5 gaps (increased from 4), each labeled with the user job it fails to support:

- **Gap 1: [Functional Job]** — [specific gap with evidence, e.g., "No approval timeline posted: Loan applicants don't know if they'll get money in 2 days or 2 weeks. Creates anxiety and may cause abandonment. Fix: Add prominent 'Typical approval time: 24–48 hours' on product page and application form."]

- **Gap 2: [Emotional Job]** — [e.g., "No customer testimonials on main product page: New borrowers can't see past success stories. Creates doubt. Fix: Add 3–5 testimonials with customer photos/names on product page; feature success metrics ('10,000+ loans approved', 'Average rating: 4.8★')."]

- **Gap 3: [Social Job]** — [e.g., "No easy share mechanism for social proof: Customers who got approved can't share their success. Misses word-of-mouth opportunity. Fix: Add 'Share your success' button after approval, with pre-filled social message."]

- **Gap 4: [Post-Purchase Support]** — [e.g., "No post-approval onboarding: Customers approved but confused about next steps. Creates regret. Fix: Send SMS + email with clear next steps; offer 1:1 call with account manager."]

- **Gap 5: [Nurture Path]** — [e.g., "No CTA between eligibility check and application: User calculates eligibility but hits dead end. No nudge to complete application. Fix: Show 'Next Step: Complete Your Application' CTA immediately after eligibility result."]

Must cover these themes (at minimum):
1. Nurture path between browsing and booking/purchase (how do users move to the next step?)
2. Social proof placement on product/service pages (testimonials, case studies, ratings visible?)
3. Post-service or aftersales visibility during pre-purchase research (what happens after customer buys?)
4. Post-inquiry or post-conversion follow-up flow (is customer abandoned after submitting form?)
5. Emotional reassurance (do users feel confident, or anxious?) — NEW

---

## Section 9 — Priority Recommendations

**Reference:** See `brief/references/cx-maturity-model.md` for maturity-aware roadmapping and realistic timelines.

**Roadmap Context (from Section 7 Maturity Assessment):**
- **Current Maturity Level:** L___ 
- **12-Month Target:** L___
- **ROI Driver:** [e.g., "Move from L2 to L3 to unlock omnichannel support and double lead capture"]

Every recommendation names the specific page, element, or system. Tier by effort and business impact.

### Quick Wins (Low effort, high impact)
Maximum 5 items. These should be implementable in days, not weeks. Examples:
- Add missing alt text to images
- Improve form error messages
- Add testimonials to product pages
- Update meta titles and descriptions
- Add WhatsApp contact widget

### Medium-Term (Revamp-level)
Minimum 5 items. These require design or development sprints (6–12 weeks). Examples:
- Implement live chat system
- Build product comparison tool
- Restructure navigation for better IA
- Add personalization engine
- Implement CRM integration for lead nurturing

Timeline to implement & measure: 6–12 weeks

### Strategic (Longer-term)
Minimum 4 items. These are platform-level or organizational changes (3–12 months). Examples:
- Migrate to omnichannel support (web + app + WhatsApp)
- Build customer data platform for personalization
- Implement AI chatbot with escalation to agents
- Launch mobile app with integrated loyalty program
- Restructure post-purchase support flow

Timeline: 3–12 months  
**Reference:** See CX Maturity Model for realistic timelines per maturity level upgrade.

---

## Section 10 — Comprehensive Audit Summary

A condensed, single-page summary of the entire audit, designed to be easily read or copy-pasted as a standalone brief for stakeholders.

**Structure:**

- **Client:** [Client Name]
- **Target URL:** [Client URL]
- **Audit Date:** [YYYY-MM-DD]

**Key Highlights by Section:**
- **Performance:** [1 sentence summarizing site speed and technical health]
- **Navigation & IA:** [1 sentence summarizing navigation structure and primary issues]
- **Usability & UX:** [1 sentence on the heuristic score and biggest accessibility/usability gap]
- **AI & SEO:** [1 sentence on SEO presence and AI readiness]
- **Competitive Positioning:** [1 sentence summarizing feature parity and gaps vs competitors]
- **User Journey Gaps:** [1 sentence on the biggest drop-offs in the conversion funnel]
- **Top Priority Actions:** [Top 3 recommendations pulled from Section 9]

---

## Report Footer

Always end with:

```markdown
---

*This audit was prepared by Antikode as part of a pitch for [service type].
All findings are based on publicly available information as of [Month Year].*

### Crawl Limitations
[List any pages or features that could not be verified, with reasons]
```

---

*Antikode — Experience Design Agency*
*web-expert-review skill*

---

### Supporting Reference Materials

This audit was enhanced using the following frameworks. Full details available in brief/references/:
- **WCAG 2.1 Framework** — Accessibility compliance (Sections 4 & 6)
- **Jobs to Be Done Framework** — User motivation analysis (Section 8)
- **CX Maturity Model** — Competitive positioning & roadmapping (Section 7 & 9)
- **Technical SEO Checklist** — SEO evaluation criteria (Section 5)
