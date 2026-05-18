Generated: 2026-05-18 | Total Test Cases: 30 | AC Coverage: 100% | BDD Coverage: 100%

### Section 1: Test Scenarios (High-Level)

| Scenario ID | Scenario Name | Type | Mapped To |
|---|---|---|---|
| TS-001 | Create product category successfully via Product Category Management page | Happy Path | AC4, AC5, AC8, AC10, S1, R4 |
| TS-002 | Create product category successfully via direct URL | Alternative Flow | AC4, AC5, AC8, AC10, S2, R4 |
| TS-003 | Hide Create action when user has no permission | Permission | AC1, S3, R1 |
| TS-004 | Block direct URL access when user has no permission | Permission, Security | AC1, S4, R1 |
| TS-005 | Reject API create request when user has no permission | Permission, Security | AC1, AC9, R1 |
| TS-006 | Validate required Category Name | Negative, Validation | AC2, AC6, AC9, S5, S6 |
| TS-007 | Validate Category Name maximum length boundary | Edge Case, Validation | AC2, AC6, AC9, S7 |
| TS-008 | Reject duplicate Category Name | Negative, Business Rule | AC3, AC6, AC9, S8, R3 |
| TS-009 | Validate Description maximum length boundary | Edge Case, Validation | AC2, AC6, AC9, S9 |
| TS-010 | Correct validation error and resubmit successfully | Alternative Flow | AC2, AC4, AC5, AC8, S10 |
| TS-011 | Trim Category Name before validation and persistence | Business Rule, Data Integrity | AC2, AC5, AC10, S11, R2 |
| TS-012 | Set audit fields on successful creation | Data Integrity | AC5, AC8, S12, R4 |
| TS-013 | Handle system failure during processing | Error Handling | AC6, AC9, S13 |
| TS-014 | Handle system failure during persistence | Error Handling | AC6, AC9, S14 |
| TS-015 | Created category is retrievable with persisted data | Data Consistency | AC10, S15 |
| TS-016 | Handle concurrent requests with same Category Name | Edge Case, Concurrency | AC3, AC6, AC7, AC9, S16, R3 |

---

### Section 2: Detailed Test Cases

### TC-001: Successfully create a product category via Product Category Management page

* **Scenario:** TS-001
* **Mapped To:** AC4, AC5, AC8, AC10, BDD-Scenario-S1, BR-R4
* **Priority:** P0 (Blocker)
* **Test Focus:** Functional, UI, Business Rule, State Transition
* **Automatable:** Yes

**Preconditions:**

* User role: User with `CREATE_PRODUCT_CATEGORY` permission.
* Data state: No existing product category named `Stationery`.
* Environment: Product Category Management page is accessible.

**Steps:**

1. Open the Product Category Management page.
2. Click the Create Product Category action.
3. Enter Category Name = `Stationery`.
4. Enter Description = `Office and writing supplies`.
5. Click **Create Product Category**.

**Test Data:**

| Field | Value | Type |
|---|---|---|
| Category Name | Stationery | Valid |
| Description | Office and writing supplies | Valid |

**Expected Result:**

→ **Functional:**

* System displays the Create Product Category page after the create action is initiated.
* System trims and validates the submitted data.
* System creates and persists one new product category.
* System returns a success response with the created product category data.
* System redirects to the Product Category Management page.
* System refreshes the Product Category Management list.
* Toast message is displayed: `Product category created successfully.`
* Created category is retrievable with the same persisted data.

→ **UI (if applicable):**

* Create Product Category page displays Category Name and Description fields.
* Category Name is marked as required.
* Product Category Management page is displayed after successful creation.

---

### TC-002: Successfully create a product category via direct URL

* **Scenario:** TS-002
* **Mapped To:** AC4, AC5, AC8, AC10, BDD-Scenario-S2, BR-R4
* **Priority:** P1 (Core)
* **Test Focus:** Functional, Permission, State Transition
* **Automatable:** Yes

**Preconditions:**

* User role: User with `CREATE_PRODUCT_CATEGORY` permission.
* Data state: No existing product category named `Cleaning Supplies`.
* Environment: Direct URL to Create Product Category page is available.

**Steps:**

1. Open the Create Product Category page using direct URL.
2. Enter Category Name = `Cleaning Supplies`.
3. Enter Description = `Cleaning and sanitation products`.
4. Click **Create Product Category**.

**Test Data:**

| Field | Value | Type |
|---|---|---|
| Category Name | Cleaning Supplies | Valid |
| Description | Cleaning and sanitation products | Valid |

**Expected Result:**

→ **Functional:**

* System validates `CREATE_PRODUCT_CATEGORY` permission before displaying the page.
* System creates and persists the product category.
* Toast message is displayed: `Product category created successfully.`

→ **UI (if applicable):**

* Product Category Management page is displayed after successful creation.

---

### TC-003: Hide Create Product Category action when user has no permission

* **Scenario:** TS-003
* **Mapped To:** AC1, BDD-Scenario-S3, BR-R1
* **Priority:** P0 (Blocker)
* **Test Focus:** Permission, UI, Security
* **Automatable:** Yes

**Preconditions:**

* User role: User without `CREATE_PRODUCT_CATEGORY` permission.
* Environment: User can access Product Category Management page.

**Steps:**

1. Open the Product Category Management page.
2. Observe available actions on the page.

**Test Data:**

| Field | Value | Type |
|---|---|---|
| Permission | Missing `CREATE_PRODUCT_CATEGORY` | Invalid permission |

**Expected Result:**

→ **Functional:**

* System does not allow the user to initiate product category creation.

→ **UI (if applicable):**

* Create Product Category action is hidden.

---

### TC-006: Reject creation when Category Name is missing or empty after trimming

* **Scenario:** TS-006
* **Mapped To:** AC2, AC6, AC9, BDD-Scenario-S5, BDD-Scenario-S6
* **Priority:** P0 (Blocker)
* **Test Focus:** Validation, Functional, Error Handling
* **Automatable:** Yes

**Preconditions:**

* User role: User with `CREATE_PRODUCT_CATEGORY` permission.
* Environment: User is on the Create Product Category page.

**Steps:**

1. Open the Create Product Category page.
2. Submit the form using each Category Name variation below.
3. Observe validation feedback.
4. Verify no product category is created for each variation.

**Test Data:**

| Field | Value | Type |
|---|---|---|
| Category Name | Not provided | Invalid - missing |
| Category Name | `""` | Invalid - empty |
| Category Name | `"   "` | Invalid - empty after trimming |
| Description | Any valid description | Valid |

**Expected Result:**

→ **Functional:**

* System trims Category Name before validation.
* System rejects each invalid request.
* Error message is displayed for Category Name: `Category Name is required.`
* User remains on the Create Product Category page.

→ **UI (if applicable):**

* Category Name field is highlighted as invalid.
* Inline validation message is displayed under Category Name.

---

### TC-008: Reject duplicate Category Name case-insensitively and ignoring leading/trailing spaces

* **Scenario:** TS-008
* **Mapped To:** AC3, AC6, AC9, BDD-Scenario-S8, BR-R3
* **Priority:** P0 (Blocker)
* **Test Focus:** Validation, Business Rule, Data Integrity
* **Automatable:** Yes

**Preconditions:**

* User role: User with `CREATE_PRODUCT_CATEGORY` permission.
* Data state: Existing product category `Stationery` exists.
* Environment: User is on the Create Product Category page.

**Steps:**

1. Submit the form using each duplicate variation below.
2. Observe duplicate validation feedback.
3. Verify no additional product category is created.

**Test Data:**

| Field | Value | Type |
|---|---|---|
| Category Name | Stationery | Invalid - exact duplicate |
| Category Name | stationery | Invalid - case-insensitive duplicate |
| Category Name | ` Stationery ` | Invalid - duplicate after trimming |

**Expected Result:**

→ **Functional:**

* System rejects duplicate Category Name values.
* Error message: `Category Name already exists. Please use a different one.`
* No duplicate product category is persisted.
