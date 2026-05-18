# Spec Writing Rules

Generate sections 1–6 of the Functional Specification in a clean, concise BA style.

---

## Section Order

1. Brief
2. Acceptance Criteria
3. Data Definition
4. Permission
5. Business Rules
6. API Response Messages

---

## Global Style

- Use concise, professional BA language
- Short sentences, active voice, present tense
- Avoid technical/backend wording
- Keep wording consistent across AC, BR, and Data Definition

**Prefer:**
- "The system rejects the request."
- "The system trims all text fields before validation."

**Avoid:**
- "The system shall perform validation to ensure that..."
- "The backend system will proceed to..."

---

## Section 1: Brief

- Feature name (short)
- Goal: 1 sentence only
- In Scope: concise bullet list
- Out of Scope: concise bullet list
- Do not include validation logic here

---

## Section 2: Acceptance Criteria

- Group by category: Access Control / Validation / Processing / Data Persistence / Concurrency / Response / Data Consistency
- Use AC1, AC2, AC3...
- Each AC = one behavior, max 2 sentences
- Do not repeat "the system returns validation error" in every AC
- Do not mix processing + persistence in the same AC

---

## Section 3: Data Definition

Split into 3 sub-sections:

### 3.1 Field Definition Table
Columns: Field | Type | Required | Default | Description

### 3.2 Field Validation Rules
Bullet list per field. Keep each rule short. Optional field rules apply only when value is provided.

### 3.3 Field Validation Error Messages
Table: Field | Rule | Message
- Messages must be user-facing, concise, consistent
- Avoid technical terms

---

## Section 4: Permission

Simple table: Permission Key | Description

---

## Section 5: Business Rules

- Use R1, R2, R3...
- Business behavior only — do NOT repeat field validation rules
- Keep each rule short

---

## Section 6: API Response Messages

Table: Case | Type | Message
- Type values: Success / Validation Error / Error
- Keep messages concise
- Group similar validation errors when possible

---

## Quality Checklist

- ACs are short and testable
- No duplicated validations across sections
- Consistent terminology throughout
- User-facing error messages are concise
- Tables are readable
- No implementation details
