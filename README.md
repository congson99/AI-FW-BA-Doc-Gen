# BA Documentation Generation Framework

AI-assisted toolkit for Business Analysts to generate a complete documentation set from a single input using Claude Code.

---

## Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- Access to this repository

---

## Setup

```bash
git clone https://gitlab.tma.com.vn/delivery-center-32/common/inventory-management/inventory-ba.git
cd inventory-ba
claude
```

---

## Commands

| Command | Purpose |
|---|---|
| `/generate <feature-name>` | Generate all 6 artifacts from a local input file |
| `/generate <JIRA-KEY>` | Generate all 6 artifacts from a Jira ticket |
| `/generate-next <feature-name>` | Generate only the next missing artifact |
| `/review <feature-name>` | Review AC/BDD quality of a generated spec |
| `/archive <feature-name>` | Archive a completed feature |

---

## What Gets Generated

Each feature produces 6 files:

| File | Content |
|---|---|
| `ai-docs/spec.md` | Brief, AC, Data Definition, Permission, Business Rules, API Response Messages |
| `ai-docs/flow.md` | User Flow (numbered steps + branches) + States + State Transitions |
| `ai-docs/scenarios.md` | BDD Scenarios in Gherkin format, grouped by type |
| `ai-docs/tc.md` | Test Scenarios table + Detailed Test Cases |
| `ba-doc.md` | spec + flow merged (for BA handoff) |
| `qa-doc.md` | scenarios + tc merged (for QA handoff) |

---

## Typical Workflow

### Option A — From local requirements

**Step 1** — Create the feature folder and write requirements:

```
features/
  create-product/
    input.md    ← create this file
```

`features/create-product/input.md` example:
```markdown
Feature: Create Product

Fields:
- Product Name (required, max 255 chars)
- SKU (required, unique, max 100 chars, alphanumeric + hyphen + underscore)
- Price (required, > 0)
- Status: defaults to Active

Permission: CREATE_PRODUCT
```

**Step 2** — Generate all artifacts:
```
/generate create-product
```

---

### Option B — From Jira ticket

```
/generate IN-350
```

Claude fetches the ticket, derives the feature name, and generates all 6 artifacts.

---

### Review

```
/review create-product
```

Reads `ai-docs/spec.md` + `ai-docs/scenarios.md`, saves review report to `features/create-product/review.md`.

---

### Archive when done

```
/archive create-product
```

Moves `features/create-product/` → `archive/create-product/`. Git history is preserved.

---

## Feature Folder Structure

```
features/
  create-product/
    input.md              ← BA writes requirements here
    ai-docs/
      spec.md             ← generated
      flow.md             ← generated
      scenarios.md        ← generated
      tc.md               ← generated
    ba-doc.md             ← spec + flow merged
    qa-doc.md             ← scenarios + tc merged
    review.md             ← optional review report

archive/
  delete-product/         ← completed and archived features
    ...
```

---

## Repository Structure

```
inventory-ba/
├── CLAUDE.md
├── .claude/
│   ├── commands/
│   │   ├── generate.md          # /generate — fast-forward all artifacts
│   │   ├── generate-next.md     # /generate-next — next missing artifact
│   │   ├── review.md            # /review — AC/BDD quality review
│   │   └── archive.md           # /archive — move to archive/
│   └── settings.json
├── rules/
│   ├── rule_spec.md             # Spec writing rules (sections 1–6)
│   ├── rule_flow.md             # Flow and States writing rules
│   ├── rule_scenarios.md        # BDD Scenarios writing rules
│   ├── rule_tc.md               # Test Case writing rules
│   └── reviewACBDD.md           # AC/BDD review rules
├── templates/
│   ├── spec.md                  # Reference: Create Product Category Spec
│   ├── flow.md                  # Reference: Create Product Category Flow
│   ├── scenarios.md             # Reference: Create Product Category Scenarios
│   └── tc.md                    # Reference: Create Product Category TC
├── features/                    # Active features (one subfolder each)
└── archive/                     # Completed and archived features
```
