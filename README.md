# BA Documentation Generation Framework

AI-assisted toolkit for Business Analysts to generate and review Functional Specification documents using Claude Code.

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
| `/generate-fs <feature-name>` | Generate FS from a local requirements file |
| `/generate-fs <JIRA-KEY>` | Generate FS by fetching requirements from Jira |
| `/review-fs <feature-name>` | Review AC/BDD of an existing FS |
| `/review-fs <JIRA-KEY>` | Review spec attached to a Jira ticket |

---

## How to Generate a Functional Specification

### Option A — From a requirements file

**Step 1** — Create a feature folder and write requirements:

```
features/
  create-product/
    input.md    ← create this file
```

`features/create-product/input.md` example:
```markdown
Feature: Create Product

Users need to create a new product with:
- Product Name (required, max 255 chars)
- SKU (required, unique, max 100 chars)
- Price (required, > 0)
- Status defaults to Active
```

**Step 2** — Run the command:

```
/generate-fs create-product
```

Output saved to `features/create-product/fs.md`.

---

### Option B — From a Jira ticket

```
/generate-fs IN-350
```

Claude fetches the ticket from Jira, generates the FS, and saves it to `features/<feature-name>/fs.md`.

---

## How to Review a Functional Specification

### Option A — Review a local FS

```
/review-fs create-product
```

Reads `features/create-product/fs.md`, saves the review report to `features/create-product/review.md`.

### Option B — Review from Jira

```
/review-fs IN-350
```

---

## Feature Folder Structure

Each feature lives in its own folder under `features/`:

```
features/
  create-product/
    input.md      ← requirements written by BA (for Option A)
    fs.md         ← generated Functional Specification
    review.md     ← generated review report
  delete-product/
    input.md
    fs.md
    review.md
```

Multiple features can be worked on in parallel — no cleanup needed between runs.

---

## Repository Structure

```
inventory-ba/
├── CLAUDE.md                        # Framework entry point
├── .claude/
│   ├── commands/
│   │   ├── generate-fs.md           # /generate-fs command
│   │   └── review-fs.md             # /review-fs command
│   └── settings.json                # Permissions
├── rules/
│   ├── rule_fs.md                   # FS writing rules (9-section structure)
│   └── reviewACBDD.md               # AC/BDD review rules and output format
├── templates/
│   └── sample_fs.md                 # Reference Functional Specification (Create Warehouse)
└── features/                        # One subfolder per feature
    └── <feature-name>/
        ├── input.md                 # Requirements (BA writes this)
        ├── fs.md                    # Generated Functional Specification
        └── review.md                # Generated review report
```

---

## Output Format

Generated Functional Specifications follow the 9-section structure defined in `rules/rule_fs.md`:

1. Brief
2. Acceptance Criteria
3. Data Definition
4. Permission
5. Business Rules
6. API Response Messages
7. User Flow
8. States
9. Scenarios

See `templates/sample_fs.md` for a complete reference example (Create Warehouse).
