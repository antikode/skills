# Skill Selection Matrix

## Research Skills by Project Type

| Skill | `crm-loyalty` | `website-revamp` | `website-new` | `general-pitch` | Condition |
|-------|:---:|:---:|:---:|:---:|-----------|
| brand-checklist | **Always** | - | - | **Always** | When brand materials are incomplete |
| brand-diagnosis | **Always** | **Always** | **Always** | **Always** | Core skill for all projects |
| brand-voice | **Always** | - | **Always** | **Always** | Calibrates pitch tone |
| web-expert-review | **Always** | **Always** | **Always** | **Always** | Audits current digital presence |
| loyalty-analysis | *If data* | - | - | - | Only if transaction/POS data is available |
| da-analysis | *If data* | *If data* | - | - | Only if GA4/analytics access is available |
| decision-maker-profiler | - | *If known* | *If known* | **Always** | When stakeholder names are in the brief |

## Strategy Skills by Project Type

| Type | Primary Skill | Secondary Skill | Arguments |
|------|--------------|----------------|-----------|
| `crm-loyalty` | loyalty-strategist | - | `--type crm --depth full --output story deck` |
| `website-revamp` | pitch-strategist | pitchdeck-website-revamp | `--type product --depth full --output story deck` |
| `website-new` | pitch-strategist | tech-pitchdeck-website-new | `--type product --depth full --output story deck` |
| `general-pitch` | pitch-strategist | - | `--depth full --output story deck` |

## Deck Arc by Project Type

| Type | Arc | Key Narrative |
|------|-----|---------------|
| `crm-loyalty` | Insight-led | Pain → diagnosis → insight → ecosystem → proof |
| `website-revamp` | Before/After | Current state → gaps → vision → tech approach → ROI |
| `website-new` | Vision-led | Opportunity → vision → architecture → technology → roadmap |
| `general-pitch` | Standard Antikode | Situation → challenge → strategy → solution → partnership |

## Conditional Skill Logic

When deciding whether to include conditional skills, scan the brief for:

**loyalty-analysis triggers:**
- Keywords: "transaction data", "POS data", "sales data", "customer data available"
- Files: CSV, Excel, or database references in the brief

**da-analysis triggers:**
- Keywords: "Google Analytics", "GA4", "website traffic", "conversion data"
- Access: Client has shared analytics credentials or exports

**decision-maker-profiler triggers:**
- Named individuals in the brief (beyond just "CEO" or "marketing manager")
- Specific stakeholder names with titles
