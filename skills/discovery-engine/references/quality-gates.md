# Quality Gates

## Phase 1: Brief Intake

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| CLIENT_NAME | Client name identified in brief | Ask user for client name |
| BUSINESS_GOAL | ≥1 primary objective stated | Ask user to clarify goal |
| CONSTRAINTS | ≥1 constraint identified | Ask user about limitations |
| BRAND_IDENTIFIED | ≥1 brand or product listed | Ask user for brand details |
| PROBLEM_STATED | "The Problem" section has substantive content | Ask user to describe the challenge |

**Gate logic:** ALL must pass to proceed.

## Phase 2: Domain Research

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| AGENT_COMPLETION | All spawned agents produced output files | Offer to rerun failed agents |
| OUTPUT_SUBSTANCE | Each output file >500 characters | Flag empty outputs, offer rerun |
| DIAGNOSTIC_COVERAGE | brand-diagnosis completeness ≥15/30 (if applicable) | Flag low coverage, ask if user can provide materials |

**Gate logic:** AGENT_COMPLETION must pass. Others are warnings.

## Phase 2.5: Research Verification

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| SOURCES_VALID | <20% of checked URLs are broken or invalid | Flag broken sources, suggest corrections |
| HALLUCINATION_FREE | No critical claims flagged as fabricated | Report flagged claims, offer to rerun |
| CROSS_CONSISTENT | No contradictions between research files on basic facts | Report contradictions, ask user to resolve |
| RELEVANCE_CHECK | All research files rated as relevant to the pitch | Flag irrelevant sections for removal |

**Gate logic:** Recommendation must be PASS or PASS WITH NOTES. NEEDS REVISION blocks progression.

## Phase 3: Strategic Research

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| OUTPUT_EXISTS | strategic-research.md exists and >1000 chars | Rerun strategic research agent |
| SOURCES_CITED | ≥10 source URLs included throughout | Rerun with explicit instruction to cite sources |
| MARKET_DATA | At least one market benchmark or industry data point | Rerun with focus on quantitative evidence |
| COMPETITOR_DEPTH | Named competitors analyzed with real evidence, not assumptions | Rerun with specific competitor focus |

**Gate logic:** OUTPUT_EXISTS and SOURCES_CITED must pass. Others are quality improvements.

## Phase 4: Strategy Draft (Soft Gates)

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| DIAGNOSIS_EXISTS | Root cause articulated (not just symptoms) | Regenerate — ask strategist to dig deeper |
| INSIGHT_EXISTS | An insight statement is present | Regenerate — provide more research context |
| POLICY_EXISTS | Guiding policy stated | Regenerate with tighter brief |
| ACTIONS_EXIST | ≥3 coherent actions defined | Regenerate action section |

**Gate logic:** All must exist, but quality is checked loosely — the consensus will pressure-test quality. The draft is allowed to be imperfect.

## Phase 5: Consensus Stress-Test

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| CONSENSUS_THRESHOLD | ≥3 items with 7+/10 agent agreement | Rerun with refined questions |
| ACTIONABLE_CRITIQUE | Consensus items cite specific sections and propose concrete changes | Rerun with instruction to be specific |
| NO_TOTAL_FRAGMENTATION | ≤2 items with zero consensus | Flag fragmented items, ask user for direction |

**Gate logic:** CONSENSUS_THRESHOLD must pass.

## Phase 6: Strategy Final (Strict Gates)

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| DIAGNOSIS_GATE | Root cause identified. Evidence-based. Explains structure of challenge. Smart outsider could understand it. Names what's being avoided. | Regenerate with explicit diagnosis feedback |
| INSIGHT_GATE | Human truth that reframes the problem. Not a fact or observation. Creates tension. Specific to this client (not generic). Connected to diagnosis. | Regenerate with more research context |
| STRATEGY_GATE | Specific to this client (swap test). Defines what to do AND what not to do. Simple enough to remember. A hypothesis that can be tested. | Regenerate with tighter constraints |
| ACTION_GATE | 3-5 coherent actions that reinforce each other as a system. Proximate objective defined and achievable. Something is being stopped/deprioritized. Quick wins identified. | Regenerate action section |
| KERNEL_EXISTS | Strategy kernel table present with all 7 elements (diagnosis, insight, leverage, guiding policy, actions, will not, proximate objective) | Add kernel if missing |
| CONSENSUS_INTEGRATED | Key consensus findings visibly incorporated — not just ignored | Cross-reference with consensus report |

**Gate logic:** ALL four primary gates (DIAGNOSIS, INSIGHT, STRATEGY, ACTION) must pass at full rigor. KERNEL_EXISTS and CONSENSUS_INTEGRATED are formatting/completeness checks.

## Phase 7: Deck Content

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| NARRATIVE_ARC | Follows: situation → challenge → evidence → insight → strategy → actions → proof | Restructure slide order |
| SLIDE_COUNT | 12-25 slides total | Merge (if >25) or expand (if <12) |
| HEADLINE_QUALITY | Every headline ≤10 words, makes one clear point | Rewrite weak headlines |
| ONE_IDEA_PER_SLIDE | No slide covers two separate concepts | Split overloaded slides |
| VISUAL_SPECIFICITY | Every slide has specific visual direction | Add concrete visual specs |
| SPEAKER_NOTES | Every slide has 2-3 sentence speaker notes | Add missing notes |
| DATA_INTEGRATION | ≥5 slides reference specific numbers from research | Pull more data points |
| STRONG_OPEN | Slide 2 grabs attention (not "about Antikode") | Restructure opening |
| BRAND_VOICE | All copy passes brand-voice quick check | Recalibrate copy |

**Gate logic:** NARRATIVE_ARC, SLIDE_COUNT, and STRONG_OPEN must pass. Others are quality improvements.

## Phase 8: Deck Production

| Gate | Pass Criteria | Fail Action |
|------|--------------|-------------|
| FILE_EXISTS | PPTX file generated at expected path | Debug pptx skill error |
| FILE_SIZE | PPTX file >100KB (not empty/corrupt) | Regenerate |

**Gate logic:** Both must pass.
