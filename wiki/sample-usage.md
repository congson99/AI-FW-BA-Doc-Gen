# Sample Usage

---

## Command Order

### Full workflow (FF)

```
1. Create features/<feature>/input.md
2. /generate <feature>          ← run all 5 generate steps
3. /review <feature>            ← run all 5 review steps (optional)
4. /archive <feature>           ← when done
```

---

### Step by step

```
# GENERATE — must follow order (dependency chain)
/generate-spec <feature>        → ai-docs/spec.md          (requires: input.md)
/generate-flow <feature>        → ai-docs/flow.md          (requires: spec.md)
/generate-scenarios <feature>   → ai-docs/scenarios.md     (requires: spec + flow)
/generate-tc <feature>          → ai-docs/tc.md            (requires: spec + scenarios)
/generate-docs <feature>        → ba-doc.md + qa-doc.md    (requires: 4 ai-docs)

# REVIEW — can run independently, any order
/review-spec <feature>          → review-spec.md           (AC quality)
/review-flow <feature>          → review-flow.md           (Flow completeness)
/review-scenarios <feature>     → review-scenarios.md      (BDD quality + coverage)
/review-tc <feature>            → review-tc.md             (TC coverage + quality)
/review-docs <feature>          → review-docs.md           (ba-doc + qa-doc consistency)

# ARCHIVE
/archive <feature>              → move to archive/
```

---

### From Jira ticket (no input.md needed)

```
/generate-spec IN-350           ← fetch from Jira, generate spec
/generate IN-350                ← fetch from Jira, generate all
```

---

## Quick Reference

| Command | Required Input | Output |
|---|---|---|
| `/generate-spec` | `input.md` | `ai-docs/spec.md` |
| `/generate-flow` | `spec.md` | `ai-docs/flow.md` |
| `/generate-scenarios` | `spec.md` + `flow.md` | `ai-docs/scenarios.md` |
| `/generate-tc` | `spec.md` + `scenarios.md` | `ai-docs/tc.md` |
| `/generate-docs` | 4 ai-docs | `ba-doc.md` + `qa-doc.md` |
| `/generate` | `input.md` | all 6 files |
| `/generate-next` | any | next missing artifact |
| `/review-spec` | `spec.md` | `review-spec.md` |
| `/review-flow` | `spec.md` + `flow.md` | `review-flow.md` |
| `/review-scenarios` | `spec.md` + `scenarios.md` | `review-scenarios.md` |
| `/review-tc` | `spec.md` + `scenarios.md` + `tc.md` | `review-tc.md` |
| `/review-docs` | 6 files | `review-docs.md` |
| `/review` | all | 5 review files |
| `/archive` | completed feature | move → `archive/` |
