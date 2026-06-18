# Rule: Acceptance Criteria

## Main Principle

Acceptance Criteria describe expected system behavior at a business level.

ACs must be:

- Short
- Clear
- Testable
- Business-focused
- Grouped by behavior type

---

## Writing Quality

- Write ACs from the system's perspective.
- Use present tense.
- Use "The system" when describing system behavior.
- Keep ACs short, clear, and business-focused.
- Each AC should describe a single observable behavior.
- Each AC must be independently testable.
- Avoid vague terms such as "correctly" or "properly".
- Avoid generic outcomes without observable results.
- Avoid implementation details.
- Do not mix validation, processing, persistence, and response in one AC.
- Keep AC wording consistent with Business Rules and Messages.
- Keep ACs within the same group at a similar level of detail.
- Do not invent validations, permissions, statuses, calculations, notifications, integrations, or background processes unless supported by the source.

---

## Group-Specific Rules

- Permission ACs must cover unauthorized request handling and access control behavior if present in source.
- Access Control ACs must name the specific permission constant required, in SCREAMING_SNAKE_CASE format: `<VERB>_<FULL_NOUN>`. Derive from the feature name — expand abbreviations using the project context (e.g., "Create PO" → `CREATE_PURCHASE_ORDER`, "Update PR" → `UPDATE_PURCHASE_REQUEST`). Do not use the abbreviation as-is.
- Search ACs must include searchable fields, matching rule, and minimum keyword rule if present.
