# Style: Acceptance Criteria

Applies to `ac_<slug>.md` documents.

---

## Output Format

```md
## 2. Acceptance Criteria

### <Group 1>

**AC1:** <one testable statement>

**AC2:** <one testable statement>

### <Group 2>

**AC3:** <one testable statement>
```

---

## Groups

Use only groups relevant to the feature:

```
### Access Control
### Search / Lookup
### Validation
### Processing
### Data Persistence
### Concurrency
### Response
### Data Consistency
### Calculation
### Default Values
### Notification
### Audit / History
```

---

## Numbering

- Number ACs sequentially across all groups: AC1, AC2, AC3…
- Do not restart numbering per group.
- Use bold label format: `**AC1:**`
