# Sample Usage — Step by Step

---

## Flow 1: Generate FS from local requirements file

### Step 1 — BA creates requirements file

BA creates `features/create-product/input.md`:

```markdown
Feature: Create Product

Users need to create a new product in the inventory system.

Fields:
- Product Name (required, max 255 chars)
- SKU (required, unique, max 100 chars, alphanumeric + hyphen + underscore, no spaces)
- Price (required, decimal > 0)
- Status: system-managed, defaults to Active

Business Rules:
- SKU must be unique across all products (case-insensitive)
- SKU is trimmed before validation and persistence
- Audit fields: Created By, Created At, Last Updated At — auto-assigned on creation

Permission:
- CREATE_PRODUCT
```

---

### Step 2 — BA runs command

```
/generate-fs create-product
```

---

### Step 3 — Claude reads rules and generates

Claude internally:
1. Reads `rules/rule_fs.md`
2. Reads `templates/sample_fs.md`
3. Reads `features/create-product/input.md`
4. Generates FS

---

### Step 4 — Claude outputs

```
✓ Saved to features/create-product/fs.md
```

Generated file `features/create-product/fs.md`:

```markdown
# 1. Brief

**Feature name:** Create Product

**Goal:** Allow authorized users to create a new product in the inventory system.

**In scope:**
* Create product
* Validate product data

**Out of scope:**
* Update product
* Delete product
* Manage products
* Product activation / deactivation

# 2. Acceptance Criteria

**Access Control**

**AC1:** If the user does not have CREATE_PRODUCT permission, the system rejects the request and returns an error response.

**Validation**

**AC2:** The system must validate the request data before processing.
**AC3:** If any validation rule fails, the system rejects the request and returns a validation error response.
**AC4:** If a product with the same SKU already exists, the system rejects the request and returns a duplicate error response.

**Processing**

**AC5:** For a valid request, the system creates a new product with Status = Active.
**AC6:** The system assigns system-managed fields including Created By, Created At, and Last Updated At.

**Data Persistence**

**AC7:** For a successful request, the system persists the product data.
**AC8:** If the request fails at any stage, no product data is persisted.

**Response**

**AC9:** If the request is successful, the system returns a success response with the created product data.
**AC10:** If the request fails, the system returns an error response.

# 3. Data Definition

## Product

### Field Definition:

| **Field**       | **Type**  | **Required** | **Default**       | **Values**        | **Description**                    |
|-----------------|-----------|--------------|-------------------|-------------------|------------------------------------|
| Product Name    | string    | Yes          | -                 | -                 | Display name of the product        |
| SKU             | string    | Yes          | -                 | -                 | Unique product identifier          |
| Price           | decimal   | Yes          | -                 | > 0               | Product price                      |
| Status          | enum      | Yes          | Active            | Active, Inactive  | Lifecycle status; system-managed   |
| Created By      | user      | Yes          | Current user      | -                 | User who created the product       |
| Created At      | datetime  | Yes          | Current timestamp | -                 | Creation timestamp                 |
| Last Updated At | datetime  | Yes          | Current timestamp | -                 | Last update timestamp              |

### Field Validation Rules:

**Product Name**
* Required
* Trim before validation and persistence
* Must not be empty after trimming
* Maximum 255 characters

**SKU**
* Required
* Trim before validation and persistence
* Must not be empty after trimming
* Maximum 100 characters
* Spaces are not allowed
* Allowed characters: letters (a–z, A–Z), numbers (0–9), hyphen (-), underscore (_)
* Must be unique across all products (case-insensitive)

**Price**
* Required
* Must be greater than 0

... (continues through all 9 sections)
```

---

## Flow 2: Generate FS from Jira ticket

### Step 1 — BA runs command with Jira key

```
/generate-fs IN-350
```

---

### Step 2 — Claude fetches from Jira

Claude internally fetches the Jira issue IN-350:
- Summary: "Create Unit of Measure"
- Description: fields, business rules, acceptance criteria from ticket

---

### Step 3 — Claude generates and saves

```
✓ Saved to features/create-unit-of-measure/fs.md
```

---

## Flow 3: Review an existing FS

### Step 1 — BA runs review command

```
/review-fs create-product
```

---

### Step 2 — Claude reads spec and review rules

Claude internally:
1. Reads `rules/reviewACBDD.md`
2. Reads `features/create-product/fs.md`
3. Reviews all AC and BDD Scenarios

---

### Step 3 — Claude outputs review report

```
✓ Saved to features/create-product/review.md
```

Generated file `features/create-product/review.md`:

```markdown
### Section 1: Assumptions & Gaps

| # | Item | Type | Notes |
|---|------|------|-------|
| 1 | Behavior when Price = 0 is not explicitly stated | `[Needs Clarification]` | AC does not specify if zero price is rejected or allowed |
| 2 | "Active" is the default status on creation | `[Assumed]` | Implied by processing AC but not stated in BR |

---

### Section 2: AC Quality Review

#### AC-1: Permission check

| Criterion    | Status | Notes |
|--------------|--------|-------|
| Testable     | ✅ | Clear pass/fail |
| Atomic       | ✅ | Single behavior |
| Unambiguous  | ✅ | |
| Bounded      | ✅ | |
| Actor-aware  | ✅ | |
| Result-clear | ✅ | States system rejects and returns error |
| In-scope     | ✅ | |

**Overall:** ✅ Good

---

#### AC-4: Duplicate SKU check

| Criterion    | Status | Notes |
|--------------|--------|-------|
| Testable     | ✅ | |
| Atomic       | ✅ | |
| Unambiguous  | ⚠️ | Does not specify if check is case-insensitive |
| Bounded      | ⚠️ | No mention of trimming before duplicate check |
| Actor-aware  | ✅ | |
| Result-clear | ✅ | |
| In-scope     | ✅ | |

**Overall:** ⚠️ Needs Improvement

**Issues:**
- Missing: case-insensitivity rule for duplicate SKU check
- Missing: whether trimming is applied before duplicate check

**Suggested fix:**
> AC4: If a product with the same SKU already exists (case-insensitive, after trimming), the system rejects the request and returns a duplicate error response.

---

### Section 3: AC Completeness Review

| Question | Status | Notes |
|----------|--------|-------|
| Do ACs cover the Happy Path end-to-end? | ✅ | AC5–AC9 cover it |
| Do ACs cover Alternative Flows? | ⚠️ | No AC for optional fields omitted |
| Do ACs cover Negative / Error scenarios? | ⚠️ | AC3 is generic — no AC per field validation |
| Do ACs cover all Business Rules listed? | ⚠️ | BR for trimming not explicitly covered in AC |
| Do ACs cover all steps/branches in the Flow? | ✅ | |
| Are there duplicate or overlapping ACs? | ✅ None | |
| Are there ACs that belong to a different story? | ✅ None | |

**Missing AC candidates:**

| # | Missing Behavior | Reason / Source |
|---|-----------------|-----------------|
| 1 | Reject request when Price ≤ 0 | Field validation rule |
| 2 | System trims text fields before validation | BR — trimming rule |
| 3 | Client-provided Status is ignored | Processing rule — system-managed field |

---

### Section 6: Overall Summary

| Area | Status | Key Issues |
|------|--------|------------|
| AC Quality | ⚠️ | AC-4 missing case-insensitive and trim clarification |
| AC Completeness | ⚠️ | No per-field negative ACs; trimming not in AC |
| BDD Quality | ✅ | Scenarios are clear and observable |
| BDD Coverage | ⚠️ | AC-3 generic — missing negative BDDs per field |
| Business Rules Coverage | ⚠️ | Trimming BR not mapped to any AC |
| Flow Coverage | ✅ | All flow branches covered |

**Priority Actions:**
1. Add AC for Price ≤ 0 rejection
2. Add AC for system trimming behavior
3. Add AC for ignoring client-provided Status
4. Clarify AC-4: add case-insensitive + trim before duplicate check
5. Add negative BDD scenarios per field validation rule
```

---

## Flow 4: No argument — Claude lists pending features

### Scenario: 3 features in various states

```
features/
  create-product/     → has input.md, no fs.md yet
  delete-product/     → has fs.md, no review.md yet
  create-supplier/    → has input.md, no fs.md yet
```

### BA runs without argument

```
/generate-fs
```

Claude responds:

```
Found features with input.md but no fs.md:
  1. create-product
  2. create-supplier

Which feature do you want to generate? (enter number or feature name)
```

```
/review-fs
```

Claude responds:

```
Found features with fs.md but no review.md:
  1. delete-product

Reviewing delete-product...
✓ Saved to features/delete-product/review.md
```
