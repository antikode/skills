---
name: discovery-engine
description: "Antikode's end-to-end pitch/strategy workflow orchestrator. Takes a brief (URL, file, or text) and runs the full Discovery Engine pipeline: init, intake, domain research, verification, strategic research, strategy draft, consensus stress-test, strategy refinement, deck content, and PPTX generation. Use this skill when the user says 'run discovery engine', 'start pitch workflow', 'run the pipeline', 'discovery engine for [client]', 'start a pitch for [client]', 'discovery-engine init', 'initialize project', or any request to run the full Antikode pitch/strategy process. Also use when the user says 'resume pipeline', 'check pipeline status', 'discovery-engine status', or 'where are we' to resume an interrupted workflow."
---

# Discovery Engine — Agentic Workflow Orchestrator

The master orchestrator for Antikode's pitch/strategy pipeline. Manages project initialization + 8 phases from brief intake to deck production, with human checkpoints and quality gates.

## Commands

| Command | What It Does |
|---------|-------------|
| `/discovery-engine init` | Initialize a new project — collect info, validate skills, create manifest |
| `/discovery-engine run` | Run the full pipeline from current state (auto-resumes) |
| `/discovery-engine status` | Show pipeline progress and next action |
| `/discovery-engine research` | Run domain research phase only |
| `/discovery-engine strategy` | Run strategic research + strategy draft + consensus + final |
| `/discovery-engine deck` | Run deck content + production |
| `resume pipeline` | Continue from where we left off |
| `rerun [phase]` | Force-regenerate a specific phase |

---

## Step 0: Init (Required First Run)

**Trigger:** `/discovery-engine init` or first time running any discovery-engine command without a manifest.

### 0.1 Collect Project Info

Ask the user for (or detect from context):

```
1. Client name:        [required — e.g., "Ateria Group"]
2. Project slug:       [auto-generate from name — e.g., "ateria"]
3. Project type:       [ask user to confirm — crm-loyalty / website-revamp / website-new / general-pitch]
4. Brief source:       [Lark URL / file path / "will paste" / "will provide later"]
5. Key constraints:    [optional — anything the user wants to flag upfront]
```

If the user provides a brief source in the init command, note it for Phase 1.

### 0.2 Validate Prerequisites

Check that required skills are available:

```
Required for ALL project types:
  - brief-intake
  - brand-diagnosis
  - brand-voice (except website-revamp)
  - web-expert-review
  - stochastic-multi-agent-consensus
  - deck-content
  - pptx

Required by project type:
  - crm-loyalty:     + brand-checklist, loyalty-strategist
  - website-revamp:  + pitch-strategist, pitchdeck-website-revamp
  - website-new:     + pitch-strategist, tech-pitchdeck-website-new
  - general-pitch:   + brand-checklist, pitch-strategist

Optional (enhance quality if available):
  - loyalty-analysis, da-analysis, decision-maker-profiler
```

Report: "All [N] required skills found. [M] optional skills available."
If any required skill is missing: warn user and list what's missing.

### 0.3 Scaffold Directories

Ensure all output directories exist:

```bash
mkdir -p src/brief/research
mkdir -p src/brief/references
mkdir -p src/deck/assets
mkdir -p src/team-brief
```

### 0.4 Create Manifest

Write `src/manifest.json` with:
- Project metadata (client, slug, type, created_at)
- Brief source noted (if provided)
- All phases set to `pending`
- Empty checkpoints and decisions

See `references/manifest-schema.md` for the full schema.

### 0.5 Report Init Complete

```
Project initialized:
  Client:  [name]
  Type:    [type]
  Slug:    [slug]
  Brief:   [source or "not provided yet"]

  Required skills: [N]/[N] available
  Optional skills: [M] available

  Manifest: src/manifest.json

Next step: [if brief source provided] "Say 'run' to start the pipeline."
           [if no brief source] "Provide a brief source to begin:
            - Lark URL
            - File path
            - Paste text directly"
```

---

## Pipeline Overview

```
Init:      Project setup              → src/manifest.json
Phase 1:   Brief Intake               → src/brief/brief.md
Phase 2:   Domain Research (parallel)  → src/brief/research/*.md
Phase 2.5: Research Verification       → src/brief/research/verification-report.md
           ── CHECKPOINT 1: Human reviews research ──
Phase 3:   Strategic Research          → src/brief/research/strategic-research.md
Phase 4:   Strategy Draft              → src/brief/strategy-draft.md
Phase 5:   Consensus Stress-Test       → src/brief/research/strategy-consensus.md
Phase 6:   Strategy Final              → src/brief/strategy-proposal.md
           ── CHECKPOINT 2: Human reviews strategy ──
Phase 7:   Deck Content                → src/deck/[client]-deck-architecture.md
           ── CHECKPOINT 3: Human reviews deck plan ──
Phase 8:   Deck Production             → src/deck/[client]-YYMMDD-v1.pptx
```

**Two types of research, clearly separated:**
- **Domain Research** (Phase 2): About the client's world — brand, web presence, competitive landscape, data analysis. Parallel agents using existing skills.
- **Strategic Research** (Phase 3): About the strategy itself — market benchmarks, industry data, consumer behavior, problem investigation. Deep-dive using WebSearch/WebFetch.

**Two-pass strategy:**
- **Draft** (Phase 4): First pass using all research. Produces a complete but untested strategy.
- **Consensus** (Phase 5): 10 agents stress-test the draft — finding weaknesses, not building from scratch.
- **Final** (Phase 6): Incorporates consensus findings into the refined strategy.

---

## Auto-Resume Logic

**CRITICAL:** Before any action, read `src/manifest.json` to check pipeline state.

```
if no manifest             → run Init (Step 0)
if manifest exists         → parse state, report status, resume from next pending phase
if user says "init"        → run Init even if manifest exists (re-initialize)
if user says "status"      → report current state, next action, completed phases
if user says "run"         → continue from next pending phase
if user says "rerun [X]"   → force-regenerate phase X
if user says "skip to [X]" → mark skipped phases, jump to X
```

---

## Phase 1: Brief Intake

Accept input from user. Detect type:
- Lark URL → invoke `brief-intake` skill
- File path → invoke `brief-intake` skill
- Pasted text → invoke `brief-intake` skill
- "Brief already exists" → read existing `src/brief/brief.md`

After brief.md is created, **auto-detect project type** by scanning brief content (if not already set during init):

| Keywords Found | Project Type |
|---------------|-------------|
| CRM, loyalty, retention, points, membership, rewards, repeat visits | `crm-loyalty` |
| website revamp, redesign, re-platform, migrate, WordPress, current site | `website-revamp` |
| new website, build from scratch, no existing site, greenfield | `website-new` |
| None of the above | `general-pitch` |

**Update manifest:** Set brief status to `complete`, confirm project type.

See `references/manifest-schema.md` for the full schema.

---

## Step 2: Domain Research (Parallel Agents)

**Purpose:** Learn about the client's world — their brand, digital presence, competitors, data.

Select research skills based on project type. See `references/skill-matrix.md` for the full mapping.

**Quick reference:**

| Type | Always Run | Conditional |
|------|-----------|-------------|
| `crm-loyalty` | brand-checklist, brand-diagnosis, brand-voice, web-expert-review | loyalty-analysis (if txn data), da-analysis (if analytics) |
| `website-revamp` | brand-diagnosis, web-expert-review | da-analysis, decision-maker-profiler |
| `website-new` | brand-diagnosis, brand-voice, web-expert-review | decision-maker-profiler |
| `general-pitch` | brand-checklist, brand-diagnosis, brand-voice, web-expert-review | decision-maker-profiler |

**For each selected skill, spawn a background Agent:**

```
Prompt template for each agent:

"Read the brief at src/brief/brief.md. Then invoke the skill '[skill-name]'
with the Skill tool. The client is [client name], a [industry description].
Project type: [type]. [Any additional context from the brief].

After running the skill, save the output to:
src/brief/research/[skill-name].md

IMPORTANT: For every claim, statistic, or finding, include the source URL
or reference. If you cannot verify a claim, mark it as [UNVERIFIED].
Do not fabricate data, statistics, or URLs."
```

Spawn ALL agents simultaneously using multiple Agent tool calls in a single message. Set `run_in_background: true` for each.

**Update manifest** as each agent completes. **Log decisions** for any skipped skills.

---

## Step 2.5: Research Verification

After ALL domain research agents complete, spawn a **verification agent** to fact-check.

```
Prompt for verification agent:

"You are a research verification agent. Your job is to fact-check the research
outputs produced by other agents. For each research file in src/brief/research/,
verify the following:

1. SOURCE VERIFICATION: For every claim with a URL or source reference,
   use WebFetch to verify the URL exists and the claim matches the source.
   Flag any broken links or mismatched claims.

2. HALLUCINATION CHECK: Look for:
   - Statistics or numbers that seem fabricated (no source cited)
   - Company details that could be wrong (store counts, founding dates, features)
   - Product features or pricing that may be outdated or invented
   - Competitor information that seems made up
   For any suspicious claim, use WebSearch to cross-reference.

3. RELEVANCE CHECK: For each research file, assess:
   - Does this research directly support the pitch for [client]?
   - Are the findings actionable (not just general knowledge)?
   - Is the competitive analysis comparing the right competitors?

4. CONSISTENCY CHECK: Across all research files:
   - Do the files agree on basic facts (store counts, brand names, etc.)?
   - Are there contradictions between research outputs?

Output a verification report to src/brief/research/verification-report.md with:
- Verification Summary (total checked, verified, flagged, broken)
- Flagged Issues (claim, file, reason, suggested correction)
- Broken or Invalid Sources
- Cross-File Consistency
- Recommendation: PASS / PASS WITH NOTES / NEEDS REVISION
"
```

**Quality Gate 2.5:** Recommendation is `PASS` or `PASS WITH NOTES`. If `NEEDS REVISION`, report issues and offer to rerun specific agents.

---

## CHECKPOINT 1: Human Reviews Domain Research

Present to the user:
1. Summary of each research file (key findings)
2. Verification report summary
3. Any data gaps
4. Manifest status

Ask: **"Review the research in src/brief/research/. When ready, say 'proceed' or tell me what to rerun."**

---

## Step 3: Strategic Research (Deep Dive)

**Purpose:** Gather the strategic evidence the strategy skill needs — market benchmarks, industry data, consumer behavior, competitive intelligence. This is DIFFERENT from domain research — it answers strategic questions, not brand/web questions.

Spawn a **strategic research agent:**

```
Prompt:

"You are a strategic research agent. Read the brief at src/brief/brief.md and
all domain research in src/brief/research/.

Your job is to gather STRATEGIC EVIDENCE that the strategy skill will need.
This is not about the client's brand or website — that's already done.
This is about the market, the industry, and the strategic context.

Research the following using WebSearch and WebFetch:

1. MARKET BENCHMARKS
   - What do consumers in [CLIENT's market/country] expect from [product category]?
   - What are typical engagement/adoption/conversion rates for similar solutions?
   - What are the industry benchmarks for success metrics?

2. COMPETITIVE INTELLIGENCE
   - Deep dive on named competitors from the brief and research
   - How do competitor solutions actually work? (pricing, features, UX)
   - What are competitors' strengths and weaknesses based on real evidence?
   - Are there competitors the research missed?

3. CONSUMER BEHAVIOR
   - How does the target audience behave in this category?
   - What channels, platforms, and tools do they use?
   - What are their expectations, frustrations, and unmet needs?
   - Any relevant behavioral data or surveys?

4. INDUSTRY TRENDS
   - What trends are shaping this category right now?
   - What technology shifts or regulatory changes are relevant?
   - What are the best-in-class examples globally?

5. STRATEGIC PRECEDENTS
   - Has anyone solved a similar problem in a different market?
   - What worked and what didn't? What can we learn?

IMPORTANT: Every finding MUST have a source URL. No fabricated data.
Mark anything you cannot verify as [UNVERIFIED].

Save output to src/brief/research/strategic-research.md with clear sections
and source citations throughout."
```

**Quality Gate 3:** Output exists, >1000 chars, has source URLs throughout.

---

## Step 4: Strategy Draft (First Pass)

**Purpose:** Produce a complete but untested strategy using ALL research.

Select strategy skill based on project type:

| Type | Strategy Skill | Arguments |
|------|---------------|-----------|
| `crm-loyalty` | `loyalty-strategist` | `--type crm --depth full --output story deck` |
| `website-revamp` | `pitch-strategist` (+ `pitchdeck-website-revamp` for tech section) | `--type product --depth full --output story deck` |
| `website-new` | `pitch-strategist` (+ `tech-pitchdeck-website-new` for tech section) | `--type product --depth full --output story deck` |
| `general-pitch` | `pitch-strategist` | `--depth full --output story deck` |

**Input:** brief + ALL domain research + strategic research. Do NOT include consensus (doesn't exist yet).

Save output to `src/brief/strategy-draft.md`.

**Quality Gate 4:** Basic strategy gates — diagnosis exists, insight exists, guiding policy exists, ≥3 coherent actions. This is a draft so gates are softer — it will be refined after consensus.

---

## Step 5: Consensus Stress-Test

**Purpose:** 10 agents review and challenge the draft strategy — finding weaknesses, not building from scratch. This is a VALIDATION step, not a generation step.

**Invoke `stochastic-multi-agent-consensus` skill:**

```
Prompt: Read ALL of the following:
- src/brief/brief.md
- All files in src/brief/research/ (domain + strategic + verification)
- src/brief/strategy-draft.md (THE DRAFT STRATEGY TO STRESS-TEST)

Each agent should REVIEW the draft strategy and answer:

1. DIAGNOSIS CHECK: Is the root cause correctly identified? What's missing?
   Is there a deeper structural issue the draft didn't catch?

2. INSIGHT CHECK: Is the insight genuinely reframing? Or is it just an
   observation dressed up as insight? What would be a stronger insight?

3. STRATEGY CHECK: Does the guiding policy pass the swap test — could
   a competitor say the same thing? Is it specific enough? What would
   make it sharper?

4. ACTION CHECK: Do the coherent actions actually reinforce each other?
   Is anything missing? Is the proximate objective achievable and measurable?

5. RISK CHECK: What risks did the draft miss? Which stated risks are
   overblown? What's the #1 thing that could kill this project?

6. COMPETITIVE CHECK: Is the positioning against [COMPETITOR] convincing?
   What would [COMPETITOR] say in response?

7. PHASING CHECK: Is the rollout sequence optimal? What should ship first
   vs. what can wait? Is the timeline realistic?

8. EVIDENCE GAPS: What claims in the strategy lack evidence? Where is the
   strategy making assumptions that should be tested?

9. CLIENT FIT: Given everything we know about [CLIENT], will this strategy
   resonate with their leadership? What will they push back on?

10. WHAT'S MISSING: What did the strategy completely miss? What would make
    this a 10/10 strategy instead of its current quality?

Each agent should be specific — cite the section of the draft they're
critiquing and propose concrete improvements, not vague suggestions.
```

Save output to `src/brief/research/strategy-consensus.md`.

**Quality Gate 5:** ≥3 items with 7+/10 agreement. Divergences documented.

---

## Step 6: Strategy Final (Refined)

**Purpose:** Incorporate consensus findings into the final strategy. This is the second pass.

Spawn a strategy refinement agent:

```
Prompt:

"Read the strategy draft at src/brief/strategy-draft.md and the consensus
stress-test at src/brief/research/strategy-consensus.md.

Your job is to REFINE the draft strategy by incorporating the consensus findings.
Also read all research files for reference.

For each consensus item:
- If 7+/10 agents agreed on a weakness → fix it
- If there's a strong divergence → present both options and recommend one
- If an outlier idea is genuinely valuable → incorporate it
- If the consensus validated a section → keep it as-is

Produce the FINAL strategy proposal with both Story format and Deck outline.
This should be the strongest possible version of the strategy.

Save to src/brief/strategy-proposal.md"
```

**Quality Gate 6:** Full strategy gates (strict):
- Diagnosis Gate: root cause identified, evidence-based, structural
- Insight Gate: human truth that reframes, specific to this client
- Strategy Gate: passes swap test, defines do AND don't
- Action Gate: 3-5 coherent reinforcing actions, proximate objective
- Kernel exists with all 7 elements

---

## CHECKPOINT 2: Human Reviews Strategy

Present:
1. Strategy kernel (diagnosis, insight, leverage, guiding policy)
2. What changed from draft → final (consensus improvements)
3. Top 3 coherent actions
4. What we're choosing not to do
5. Quality gate results

Ask: **"Review the full strategy at src/brief/strategy-proposal.md. Say 'proceed to deck' or give direction for revision."**

---

## Step 7: Deck Content

Invoke `deck-content` skill with:
- `src/brief/strategy-proposal.md`
- `src/brief/research/brand-voice.md`
- `src/brief/research/brand-diagnosis.md`

Save output to `src/deck/[client-slug]-deck-architecture.md`.

**Quality Gate 7:** 12-25 slides, each with title + key message + content, narrative arc follows story format.

---

## CHECKPOINT 3: Human Reviews Deck

Present slide list with headlines. Ask: **"Review the deck plan. Say 'generate pptx' or adjust."**

---

## Step 8: Deck Production

Invoke `pptx` skill with:
- Deck architecture file
- Brand diagnosis for visual identity
- Any assets in `src/deck/assets/`

Save output to `src/deck/[client-slug]-YYMMDD-v1.pptx`.

**Mark manifest complete.** Report final status to user.

---

## Resumability

On EVERY invocation, the first action is reading `src/manifest.json`. The orchestrator never assumes a fresh start.

| Manifest State | Action |
|---------------|--------|
| No manifest | New project → Step 1 |
| `research.status = "in_progress"` | Check which agents completed, rerun only missing |
| `research.status = "complete"`, verification pending | Run verification |
| verification complete, checkpoint pending | Present research for review |
| checkpoint 1 approved, `strategic_research.status = "pending"` | Run strategic research |
| strategic research complete, `strategy_draft.status = "pending"` | Run strategy draft |
| draft complete, `consensus.status = "pending"` | Run consensus stress-test |
| consensus complete, `strategy_final.status = "pending"` | Run strategy refinement |
| strategy final complete, checkpoint 2 pending | Present strategy for review |
| checkpoint 2 approved, `deck_content.status = "pending"` | Run deck content |
| deck content complete, checkpoint 3 pending | Present deck for review |
| checkpoint 3 approved, `deck_production.status = "pending"` | Run deck production |
| All phases complete | Report "Pipeline complete" with output locations |

User commands:
- "resume" → continue from next pending phase
- "rerun [phase]" → force-regenerate that phase
- "skip to [phase]" → jump ahead (mark skipped phases)
- "status" → show manifest summary

---

## Error Handling

| Error | Recovery |
|-------|---------|
| Agent fails silently | Quality gate catches empty/short output, offers rerun |
| Verification finds hallucinations | Report flagged issues, offer to rerun specific research |
| Strategy draft is weak | Consensus will catch it; if consensus also weak, rerun with more research |
| Skill not available | Log in manifest decisions, skip with user notification |
| Manifest corrupted | Back up to `.manifest.json.bak`, create fresh manifest |
| User interrupts mid-phase | Manifest tracks last completed step, resume on next invocation |
