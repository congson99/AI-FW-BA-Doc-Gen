# Rule: Business Rules

## Main Principle

Business Rules define business-level constraints and behaviors.

They describe rules that affect multiple fields, objects, statuses, or business processes.

Do not repeat field-level validations, UI behavior, or Acceptance Criteria unless they represent an important business constraint.

---

## Writing Quality

- Keep each rule short and focused on one business behavior.
- Use present tense.
- Use consistent terminology from Data Definition.
- Do not add inferred rules not present in the source.
- Do not include UI behavior.
- Do not restate field validations or Acceptance Criteria.

---

## Include When Present

- Permission enforcement rules
- Numbering or auto-generation rules
- Status assignment rules
- State transition rules
- Calculation rules
- Cross-field rules
- Cross-entity rules
- Concurrency rules
- Transactional or all-or-nothing business rules