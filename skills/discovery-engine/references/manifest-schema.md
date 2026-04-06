# Manifest Schema

The manifest file tracks pipeline state at `src/manifest.json`. It enables resumability, auditability, and progress reporting.

## Schema

```json
{
  "version": 2,
  "project": {
    "client": "string — client name",
    "slug": "string — lowercase, hyphenated (used for file naming)",
    "type": "crm-loyalty | website-revamp | website-new | general-pitch",
    "created_at": "ISO 8601 timestamp",
    "updated_at": "ISO 8601 timestamp"
  },
  "brief": {
    "source": "lark-url | file | text | existing",
    "source_ref": "string — URL or file path if applicable",
    "path": "src/brief/brief.md",
    "status": "pending | complete",
    "completed_at": "ISO 8601 timestamp | null"
  },
  "phases": {
    "domain_research": {
      "status": "pending | in_progress | complete | failed",
      "started_at": "ISO 8601 | null",
      "completed_at": "ISO 8601 | null",
      "agents": {
        "[skill-name]": {
          "status": "pending | in_progress | complete | failed | skipped",
          "output": "src/brief/research/[skill-name].md",
          "started_at": "ISO 8601 | null",
          "completed_at": "ISO 8601 | null",
          "error": "string | null"
        }
      },
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null",
        "notes": "string | null"
      }
    },
    "verification": {
      "status": "pending | in_progress | complete | failed | skipped",
      "output": "src/brief/research/verification-report.md",
      "recommendation": "PASS | PASS_WITH_NOTES | NEEDS_REVISION | null",
      "claims_checked": "number | null",
      "claims_flagged": "number | null",
      "completed_at": "ISO 8601 | null"
    },
    "strategic_research": {
      "status": "pending | in_progress | complete | failed",
      "output": "src/brief/research/strategic-research.md",
      "started_at": "ISO 8601 | null",
      "completed_at": "ISO 8601 | null",
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null"
      }
    },
    "strategy_draft": {
      "status": "pending | in_progress | complete | failed",
      "skill_used": "string | null",
      "output": "src/brief/strategy-draft.md",
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null",
        "gates_passed": []
      }
    },
    "consensus": {
      "status": "pending | in_progress | complete | failed | skipped",
      "output": "src/brief/research/strategy-consensus.md",
      "agent_count": "number | null",
      "consensus_items": "number | null",
      "divergence_items": "number | null",
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null"
      }
    },
    "strategy_final": {
      "status": "pending | in_progress | complete | failed",
      "output": "src/brief/strategy-proposal.md",
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null",
        "gates_passed": ["diagnosis", "insight", "strategy", "action"]
      }
    },
    "deck_content": {
      "status": "pending | in_progress | complete | failed",
      "output": "src/deck/[slug]-deck-architecture.md | null",
      "slide_count": "number | null",
      "quality_gate": {
        "passed": "boolean | null",
        "checked_at": "ISO 8601 | null"
      }
    },
    "deck_production": {
      "status": "pending | in_progress | complete | failed",
      "output": "src/deck/[slug]-YYMMDD-v1.pptx | null",
      "completed_at": "ISO 8601 | null"
    }
  },
  "checkpoints": {
    "post_domain_research": {
      "status": "pending | approved | revision_requested",
      "approved_at": "ISO 8601 | null",
      "notes": "string | null"
    },
    "post_strategy": {
      "status": "pending | approved | revision_requested",
      "approved_at": "ISO 8601 | null",
      "notes": "string | null"
    },
    "post_deck": {
      "status": "pending | approved | revision_requested",
      "approved_at": "ISO 8601 | null",
      "notes": "string | null"
    }
  },
  "decisions": [
    {
      "phase": "string — which phase",
      "note": "string — what was decided and why",
      "made_by": "orchestrator | user",
      "timestamp": "ISO 8601"
    }
  ]
}
```

## Status Values

| Status | Meaning |
|--------|---------|
| `pending` | Not yet started |
| `in_progress` | Currently running |
| `complete` | Successfully finished |
| `failed` | Errored — needs rerun or investigation |
| `skipped` | Deliberately skipped (logged in decisions) |

## Resume Logic

```
if no manifest → new project
if brief.status != "complete" → run intake
if domain_research.status == "in_progress" → check agents, rerun missing
if domain_research.status == "complete" AND verification.status == "pending" → run verification
if verification.status == "complete" AND post_domain_research.status == "pending" → present research for review
if post_domain_research.status == "approved" AND strategic_research.status == "pending" → run strategic research
if strategic_research.status == "complete" AND strategy_draft.status == "pending" → run strategy draft
if strategy_draft.status == "complete" AND consensus.status == "pending" → run consensus stress-test
if consensus.status == "complete" AND strategy_final.status == "pending" → run strategy refinement
if strategy_final.status == "complete" AND post_strategy.status == "pending" → present strategy for review
if post_strategy.status == "approved" AND deck_content.status == "pending" → run deck content
if deck_content.status == "complete" AND post_deck.status == "pending" → present deck for review
if post_deck.status == "approved" AND deck_production.status == "pending" → run deck production
if deck_production.status == "complete" → report complete
```

## Output Files

| Phase | Output Path |
|-------|------------|
| Brief Intake | `src/brief/brief.md` |
| Domain Research | `src/brief/research/[skill-name].md` |
| Verification | `src/brief/research/verification-report.md` |
| Strategic Research | `src/brief/research/strategic-research.md` |
| Strategy Draft | `src/brief/strategy-draft.md` |
| Consensus | `src/brief/research/strategy-consensus.md` |
| Strategy Final | `src/brief/strategy-proposal.md` |
| Deck Content | `src/deck/[slug]-deck-architecture.md` |
| Deck Production | `src/deck/[slug]-YYMMDD-v1.pptx` |
