# Consensus Question Templates

Templates for the consensus STRESS-TEST phase. The consensus now reviews a DRAFT STRATEGY, not a blank slate. Questions are designed to find weaknesses, validate strengths, and propose improvements.

## How Consensus Works in This Pipeline

```
Old flow: Research → Consensus (generate strategy from scratch) → Strategy
New flow: Research → Strategy Draft → Consensus (stress-test the draft) → Strategy Final
```

The consensus agents receive:
1. The brief
2. All research (domain + strategic + verification)
3. **The draft strategy** (the thing being stress-tested)

Their job is to CRITIQUE and IMPROVE — not to build from scratch.

---

## Universal Stress-Test Template (All Project Types)

```
Read the strategy draft at src/brief/strategy-draft.md and ALL research files.
Your job is to stress-test this draft strategy — find weaknesses, validate
strengths, and propose specific improvements.

For each question, cite the specific section of the draft you're critiquing
and propose a concrete improvement (not a vague suggestion).

1. DIAGNOSIS CHECK: Is the root cause correctly identified? What's missing?
   Is there a deeper structural issue the draft didn't catch?

2. INSIGHT CHECK: Is the insight genuinely reframing? Or is it just an
   observation dressed up as insight? What would be a stronger insight?

3. STRATEGY CHECK: Does the guiding policy pass the swap test — could a
   competitor say the same thing? Is it specific enough? What would
   make it sharper?

4. ACTION CHECK: Do the coherent actions actually reinforce each other?
   Is anything missing? Is the proximate objective achievable and measurable?

5. RISK CHECK: What risks did the draft miss? Which stated risks are
   overblown? What's the #1 thing that could kill this project?

6. COMPETITIVE CHECK: Is the positioning against [COMPETITOR] convincing?
   What would [COMPETITOR] say in response to this pitch?

7. PHASING CHECK: Is the rollout sequence optimal? What should ship first
   vs. what can wait? Is the timeline realistic?

8. EVIDENCE GAPS: What claims in the strategy lack evidence? Where is the
   strategy making assumptions that should be tested?

9. CLIENT FIT: Given everything we know about [CLIENT], will this strategy
   resonate with their leadership? What will they push back on?

10. WHAT'S MISSING: What did the strategy completely miss? What would make
    this a 10/10 strategy instead of its current quality?
```

---

## CRM / Loyalty — Additional Questions

Add these AFTER the universal 10 if project type is `crm-loyalty`:

```
11. LOYALTY ARCHITECTURE: Is the points/tier/challenge structure right for
    this market? Would customers actually engage with it?

12. CHANNEL STRATEGY: Is the WhatsApp → App funnel optimal? What would
    break the conversion at each step?

13. CROSS-BRAND THESIS: Is the multi-brand ecosystem thesis realistic
    based on the evidence? What if customers don't overlap?

14. PRINCIPAL RISK: How would the brand principals react to this strategy?
    What specific objection would they raise?

15. ROI ARGUMENT: Is the business case strong enough to justify the
    investment? What numbers are missing?
```

---

## Website Revamp — Additional Questions

Add these AFTER the universal 10 if project type is `website-revamp`:

```
11. TECH STACK: Is the proposed technology the right choice? What are the
    risks of this stack for this specific client?

12. MIGRATION RISK: What could go wrong during migration? Is the SEO
    preservation strategy sufficient?

13. PERFORMANCE TARGETS: Are the performance targets realistic and
    meaningful? What benchmarks support them?

14. CONTENT STRATEGY: Is the content approach right? What content is
    missing from the plan?

15. POST-LAUNCH: What happens after launch? Is the maintenance and
    iteration plan realistic?
```

---

## New Website — Additional Questions

Add these AFTER the universal 10 if project type is `website-new`:

```
11. INFORMATION ARCHITECTURE: Is the proposed site structure optimal for
    the target audience? What's missing?

12. DESIGN DIRECTION: Does the visual approach match the brand and audience?
    What reference points support this direction?

13. MVP SCOPE: Is the MVP scope right? What's being left out that
    shouldn't be? What's included that could wait?

14. SCALABILITY: Will this architecture scale with the client's growth
    plans? What breaks first?

15. DIFFERENTIATION: Will this website actually stand out from competitors?
    What makes it meaningfully different?
```

---

## Usage Notes

- Replace `[CLIENT]` and `[COMPETITOR]` with actual values from the brief
- The consensus skill spawns 10 agents with slight framing variations
- Each agent reviews the SAME draft strategy from a different perspective
- Output shows agreement (7+/10), divergences, and outlier ideas
- The strategy-final phase uses this output to refine the draft
