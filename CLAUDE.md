# BA Documentation Generation Framework

AI-assisted framework for Business Analysts to generate and review Functional Specification documents using Claude.

## Commands

| Command | Purpose |
|---|---|
| `/generate-fs <feature-name>` | Generate FS from `features/<feature-name>/input.md` |
| `/generate-fs <JIRA-KEY>` | Generate FS by fetching requirements from a Jira ticket |
| `/review-fs <feature-name>` | Review AC/BDD of `features/<feature-name>/fs.md` |
| `/review-fs <JIRA-KEY>` | Review spec attached to a Jira ticket |

Commands are defined in `.claude/commands/`.

## Workflow

**Generate from file:**
1. Create `features/<feature-name>/input.md` with requirements
2. Run `/generate-fs <feature-name>`
3. FS saved to `features/<feature-name>/fs.md`

**Generate from Jira:**
1. Run `/generate-fs IN-350`
2. FS saved to `features/<feature-name>/fs.md`

**Review:**
1. Run `/review-fs <feature-name>` or `/review-fs IN-350`
2. Report saved to `features/<feature-name>/review.md`

## Rules

- `rules/rule_fs.md` — FS writing rules (used by generate-fs)
- `rules/reviewACBDD.md` — Review rules (used by review-fs)
- `templates/sample_fs.md` — Reference FS template (Create Warehouse)
