# Functional Specification (FS) Writing Rules — Clean, Readable, BA Style

## Objective

Generate a complete Functional Specification (FS) document in a clean, concise, business analyst style.

The document must:

* Be easy to read and review
* Avoid repetitive wording
* Keep Acceptance Criteria short and clear
* Separate business rules from validation rules
* Use consistent terminology throughout the document
* Focus on system behavior, not implementation details

---

# FS Structure

Use the following section order:

1. Brief
2. Acceptance Criteria
3. Data Definition
4. Permission
5. Business Rules
6. API Response Messages
7. User Flow
8. States
9. Scenarios

---

# Global Writing Rules

## Writing Style

* Use concise and professional BA language
* Prefer short sentences
* Avoid duplicated statements across sections
* Avoid overly technical/backend wording unless necessary
* Keep wording consistent across AC, BR, and Scenarios
* Use active voice
* Use present tense

### Preferred Style

Good:

* "The system rejects the request."
* "The system trims all text fields before validation."
* "Barcode must be unique."

Avoid:

* "The system shall perform validation to ensure that..."
* "In the event that validation fails..."
* "The backend system will proceed to..."

---

# Section Rules

# 1. Brief

Keep this section short.

Include:

* Feature name
* Goal
* In Scope
* Out of Scope

## Rules

* Goal = 1 sentence only
* Scope lists must be concise
* Do not include validation logic here

### Example

## 1. Brief

### Feature Name

Create Product

### Goal

Allow authorized users to create a new product.

### In Scope

* Create product
* Validate product data
* Upload product image
* Generate barcode image

### Out of Scope

* Update product
* Delete product
* Inventory management

---

# 2. Acceptance Criteria

## Main Principle

Acceptance Criteria must be:

* Short
* Clear
* Testable
* Business-focused

Avoid long paragraphs.
Avoid combining multiple validations into one AC.

## Formatting Rules

* Use AC1, AC2, AC3...
* Each AC should contain only one objective
* Maximum 2 sentences per AC
* Prefer bullet grouping by category

## Recommended Structure

### Access Control

### Validation

### Processing

### Persistence

### Response

---

## Preferred AC Style

Good:

* "AC1. Users must have CREATE_PRODUCT permission."
* "AC2. Product Name is required."
* "AC3. SKU must be unique."
* "AC4. The system trims text fields before validation and persistence."
* "AC5. The system ignores client-provided values for system-managed fields."

Avoid:

* Long conditional paragraphs
* Repeating "the system returns validation error response" in every AC
* Mixing validation + processing + persistence in the same AC

---

## Recommended AC Template

### Access Control

* AC1. Users must have [PERMISSION_KEY] permission.
* AC2. Users without permission cannot access the feature.

### Validation

* AC3. The system validates all request fields before processing.
* AC4. Required fields must not be empty after trimming.
* AC5. Unique fields must not duplicate existing records.
* AC6. Image files must match allowed formats and size limits.

### Processing

* AC7. The system trims text fields before validation and persistence.
* AC8. System-managed fields are auto-assigned.
* AC9. Client-provided values for system-managed fields are ignored.
* AC10. Barcode Image is generated only after validation succeeds.

### Persistence

* AC11. The system persists data only when all validations succeed.
* AC12. The system rolls back all changes if any processing step fails.

### Response

* AC13. The system returns a success response after successful persistence.
* AC14. The system returns validation errors when validation fails.
* AC15. The system returns a system error when processing fails.

---

# 3. Data Definition

Split into 3 sub-sections:

1. Field Definition
2. Field Validation Rules
3. Validation Messages

---

## 3.1 Field Definition

Use table columns:

* Field
* Type
* Required
* Default
* Values
* Description

## Rules

* Keep descriptions short
* Do not repeat validation details excessively
* Mention only key constraints

### Example

| Field        | Type   | Required | Default | Values           | Description          |
| ------------ | ------ | -------- | ------- | ---------------- | -------------------- |
| Product Name | String | Yes      | -       | Max 255 chars    | Product display name |
| SKU          | String | Yes      | -       | Unique           | Product SKU          |
| Status       | Enum   | Yes      | Active  | Active, Inactive | System-managed       |

---

## 3.2 Field Validation Rules

Use bullet points per field.

### Rules

* Keep each rule short
* Avoid repeating "The system validates"
* Separate validation rules from business rules
* Optional field rules apply only when value is provided

### Example

### Product Name

* Required
* Trim before validation
* Must not be empty after trimming
* Maximum 255 characters

### SKU

* Required
* Trim before validation
* Maximum 100 characters
* Must be unique

---

## 3.3 Validation Messages

Use table:
| # | Field | Rule | Message |

## Rules

* Message must be user-facing
* Keep wording consistent
* Avoid technical terms

### Good Examples

* "Product Name is required."
* "SKU already exists."
* "Image size must not exceed 20MB."

Avoid:

* "Validation failed for field SKU"
* "Unexpected invalid request format"

---

# 4. Permission

Use simple table:

| Permission Key  | Description                    |
| --------------- | ------------------------------ |
| CREATE_PRODUCT  | Allow users to create products |

Keep descriptions short.

---

# 5. Business Rules

## Main Principle

Business Rules define business behavior.

Do NOT repeat field validations already defined earlier unless necessary.

## Formatting

Use:

* BR1
* BR2
* BR3

Keep each rule short.

### Good Examples

* BR1. SKU must be unique across active products.
* BR2. Barcode Image is generated from Barcode and Barcode Format.
* BR3. System-managed fields cannot be updated by clients.

Avoid:

* Repeating max length rules
* Repeating required field rules

---

# 6. API Response Messages

Use table:
| Case | Type | Message |

## Rules

* Keep messages concise
* Group similar validation errors when possible
* Avoid redundant rows

### Example

| Case                 | Type             | Message                       |
| -------------------- | ---------------- | ----------------------------- |
| Create success       | Success          | Product created successfully. |
| Duplicate SKU        | Validation Error | SKU already exists.           |
| Invalid image format | Validation Error | Invalid image format.         |
| Upload failure       | Error            | Failed to upload image.       |

---

# 7. User Flow

## Rules

* Use numbered flow
* Keep steps short
* Focus on business flow
* Avoid low-level technical details

## Recommended Flow

1. Access Feature
2. Input Data
3. Validate Request
4. Process Request
5. Persist Data
6. Return Response

### Example

1. User accesses the Create Product page.
2. User enters product information.
3. The system validates the request.
4. The system uploads the image and generates Barcode Image.
5. The system saves the product.
6. The system returns a success response.

---

# 8. States

Use 2 sub-sections:

## System States

Short one-line descriptions.

### Example

* Idle: The feature is waiting for user action.
* Processing: The system is processing the request.
* Success: The request is completed successfully.

---

## State Transitions

Format:
State A → State B: Trigger

### Example

* Input Form → Submitting: User submits the form.
* Processing → Success: Processing succeeds.
* Processing → Error: System error occurs.

---

# 9. Scenarios

## Rules

* Use Gherkin format
* Keep steps concise
* Focus on business behavior
* Avoid excessive technical detail

Use:

* S1, S2, S3...
* Descriptive titles

---

## Scenario Groups

### Happy Path

### Alternative Flows

### Validation Scenarios

### Business Rule Scenarios

### System Error Scenarios

### Edge Cases

### State-Based Scenarios

### Security Scenarios

---

## Preferred Scenario Style

### S1 — Create product successfully

Given the user has CREATE_PRODUCT permission
And valid product data is provided
When the user submits the request
Then the system creates the product successfully

---

# Important Clean-Up Rules

## Avoid Repetition

Do not repeat the same statement in:

* AC
* BR
* Validation Rules
* Scenarios

Each section has its own purpose.

---

## Separation of Concerns

### Acceptance Criteria

Describe expected outcomes.

### Validation Rules

Describe field-level constraints.

### Business Rules

Describe business behavior.

### Scenarios

Describe testable flows.

---

# Final Quality Checklist

Before generating the FS, ensure:

* ACs are short and readable
* No duplicated validations across sections
* Consistent terminology
* Clear separation between validation and business logic
* User-facing error messages are concise
* Scenarios are testable
* Formatting is consistent
* Tables are readable
* No unnecessary technical implementation details
* Document feels review-friendly for BA, QA, and Dev teams
