# BDD Scenarios Writing Rules

Generate BDD Scenarios in Gherkin format based on the Spec and Flow.

---

## Scenario Groups (in order)

1. Happy Path
2. Alternative Flows
3. Permission Scenarios
4. Validation Scenarios (group by field)
5. Business Rule Scenarios
6. Data Validation / Input Transformation
7. Data Integrity / Audit Fields
8. System Error Scenarios
9. Data Consistency
10. Concurrency (if applicable)
11. Retry Behavior (if applicable)

---

## Gherkin Format Rules

- Use `Given / And / When / Then / And` keywords
- Each scenario has a title that describes the behavior (not the steps)
- `Given` sets up a real precondition — not just "Given I am on the page"
- `When` = one action only
- `Then` = observable system output (not internal state)
- Keep steps concise — focus on business behavior
- Do NOT hardcode UI labels/selectors unless testing UI specifically

---

## Coverage Requirements

- Every AC must have at least one happy-path scenario
- Every AC must have at least one negative/failure scenario (if applicable)
- Every flow branch must be covered by at least one scenario
- Every business rule must have a dedicated scenario
- Every validation rule (required, max length, format, duplicate) must have a scenario

---

## Naming Convention

- Use S1, S2, S3...
- Title format: `S1 – [Behavior description]`

---

## Good Scenario Example

```
S1 – Create [feature] successfully via UI

Given the user has [PERMISSION] permission
And the user is on the [Feature] Management page
When the user submits a valid creation request
Then the system creates and persists the [feature]
And the system redirects to the [Feature] Management page
And the system displays "[Feature] created successfully." as a toast notification
```

---

## Anti-patterns to Avoid

- `Given I open the browser` — meaningless precondition
- `When I fill in all fields and click Submit` — bundled action
- `Then the system works` — not observable
- Multiple `Then` clauses that belong to different ACs — split them

---

## Quality Checklist

- Every AC is covered by at least one scenario
- Every flow branch is covered
- Every business rule has a scenario
- Happy path + negative path per AC
- No duplicate scenarios
- Titles describe behavior, not steps
