# Rule: Business Rules

## Main Principle

Business Rules define business behavior and cross-field rules.

They should not simply repeat all field validation rules unless the rule is important at the business level.

---

## Writing Quality

- Keep each rule short and focused on one business behavior.
- Use present tense.
- Use consistent terms from Data Definition.
- Do not add inferred business rules not present in the source.
- Do not mix UI display rules into Business Rules.
- Do not repeat every required field rule.

---

## Include When Present

- Permission enforcement rules
- Numbering / auto-generation rules
- Search behavior rules
- Concurrency rules
- Status assignment rules
- Rollback rules
