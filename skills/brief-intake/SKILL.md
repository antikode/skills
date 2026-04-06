---
name: brief-intake
description: "Normalize raw client input into a structured brief.md for the Discovery Engine pipeline. Accepts Lark document URLs, uploaded PDFs/docs, meeting notes, pasted text, or questionnaire responses. Use this skill when the user says 'intake brief', 'normalize this brief', 'create brief from [source]', 'process meeting notes into a brief', 'structure this brief', or provides raw client context that needs structuring for the pitch workflow."
---

# Brief Intake

Normalizes any raw client input into a structured `brief.md` that feeds the Discovery Engine pipeline.

## Input Detection

Detect the input type and process accordingly:

| Input Type | Detection | Processing |
|-----------|-----------|------------|
| **Lark URL** | Contains `larksuite.com/docx/` or `/wiki/` or `/doc/` | Use lark-doc skill: `lark-cli docs +fetch --doc [token] --as user` |
| **File path** | Ends in `.md`, `.pdf`, `.docx`, `.txt` or is a valid path | Read the file directly |
| **URL** | Starts with `http` | Use WebFetch to retrieve content |
| **Pasted text** | None of the above | Parse inline text directly |
| **"Brief already exists"** | User says brief exists | Read existing `src/brief/brief.md`, validate structure |

## Output Template

Extract and structure content into the following sections. Every section is required — if information is missing, write `[NOT PROVIDED — ask client]` and list it in Open Questions.

```markdown
# [Client Name] — Discovery Brief

## Client Overview
- **Client name:**
- **Industry/category:**
- **Brands/products:** (list with store counts, type, principal if franchise)
- **Scale:** (locations, employees, revenue if known)
- **Meeting attendees:** (names, roles)

## The Problem
**Stated problem:** [What the client says their problem is]
**Inferred problem:** [What the evidence suggests the real problem is — may differ from stated]
- Issue 1: [with supporting evidence]
- Issue 2: [with supporting evidence]
- Issue 3: [with supporting evidence]

## What the Client Wants
**Primary objectives:**
- [list]

**Secondary objectives:**
- [list]

**Brand perception goal:** [how they want to be perceived]
**Timeline/outlook:** [any stated timeline or future plans]

## Constraints
**Hard constraints:** [non-negotiable restrictions]
**Soft constraints:** [preferences, concerns, organizational limitations]

| Constraint | Detail |
|-----------|--------|
| [name] | [detail] |

## Proposed Approach
[Any approach already discussed in meetings or documents. If none, write "No approach discussed yet."]

## Current Tech Stack
| System | Status |
|--------|--------|
| [name] | [status] |

## Competitive Context
[Any competitors, alternative vendors, or market references mentioned. If none, write "No competitive context provided."]

## Open Questions
- [List everything that is missing or unclear]
- [These become inputs for research phase skill selection]

## Resolved Questions
| Question | Answer |
|----------|--------|
| [question] | [answer] |
```

## Quality Checklist

Before saving, verify these minimum requirements. If any fail, report the gap but still save the brief — the orchestrator will handle missing data.

| Check | Required | How to Verify |
|-------|----------|---------------|
| Client name identified | Yes | Client Overview has a name |
| Business goal stated | Yes | "What the Client Wants" has ≥1 primary objective |
| ≥1 constraint identified | Yes | Constraints section is not empty |
| ≥1 brand/product identified | Yes | Client Overview lists brands or products |
| Problem statement exists | Yes | "The Problem" section has content |
| Open questions listed | Yes | Gaps are acknowledged, not hidden |

## Output

1. Save structured brief to `src/brief/brief.md`
2. Report to user:
   - What was captured (section-by-section summary)
   - What is missing (from Open Questions)
   - Quality checklist pass/fail
   - Recommended next step: "Ready for research phase" or "Need more input on: [gaps]"
