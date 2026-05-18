# Test Case Writing Rules

Generate Test Cases based on the Spec (AC, Business Rules) and BDD Scenarios.

---

## Document Structure

Section 1: Test Scenarios (high-level summary table)
Section 2: Detailed Test Cases

---

## Section 1: Test Scenarios Table

Header row: `Generated: <date> | Total Test Cases: N | AC Coverage: 100% | BDD Coverage: 100%`

Table columns: Scenario ID | Scenario Name | Type | Mapped To

**Type values:** Happy Path / Alternative Flow / Permission / Negative / Validation / Edge Case / Business Rule / Data Integrity / Error Handling / Concurrency / UI / Security

**Mapped To:** list all relevant AC IDs, Scenario IDs (S1, S2...), and Business Rule IDs (R1, R2...)

---

## Section 2: Detailed Test Cases

Each TC must have:

```
### TC-NNN: [Test case title]

- **Scenario:** TS-NNN
- **Mapped To:** AC-X, BDD-Scenario-SX, BR-RX
- **Priority:** P0 (Blocker) / P1 (Core) / P2 (Important) / P3 (Nice-to-have)
- **Test Focus:** Functional / UI / API / Permission / Validation / Business Rule / Data Integrity / Error Handling / Security / State Transition / Concurrency / Boundary
- **Automatable:** Yes / No / Partial

**Preconditions:**
- User role: [role + permissions]
- Data state: [required data setup]
- Environment: [what needs to be accessible]

**Steps:**
1. [step]
2. [step]
...

**Test Data:**
| Field | Value | Type |
|-------|-------|------|
| ...   | ...   | Valid / Invalid / Boundary / Simulated error |

**Expected Result:**

→ **Functional:**
- [observable system behavior]

→ **UI (if applicable):**
- [UI-specific behavior]
```

---

## Priority Guidelines

| Priority | Label | When to use |
|---|---|---|
| P0 | Blocker | Permission checks, happy path, core creation |
| P1 | Core | Field validation, business rules, error handling |
| P2 | Important | State transitions, edge cases, retry behavior |
| P3 | Nice-to-have | Format validation if conditional, cosmetic behavior |

---

## Coverage Requirements

- 100% AC coverage: every AC has at least one TC
- 100% BDD coverage: every BDD scenario maps to at least one TC
- Every business rule has at least one TC
- Every validation rule (required, max length, format, unique) has at least one TC with boundary values
- Every error/failure path has at least one TC

---

## Test Data Rules

- Use realistic test data (not "test123")
- For boundary tests: provide both valid boundary (N chars) and invalid boundary (N+1 chars)
- For duplicate tests: state the existing data in preconditions
- For permission tests: state the missing permission explicitly in Test Data table

---

## Quality Checklist

- Every TC maps to at least one AC and one BDD scenario
- Test Data table is complete and realistic
- Expected Result covers both Functional and UI behavior
- Preconditions clearly state user role and data setup
- Priority is justified
- Automatable flag is accurate
