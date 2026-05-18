# 1. Brief

**Feature name:** Create Warehouse

**Goal:** Allow authorized users to create warehouse records so that inventory can be assigned and managed under defined warehouses.

**In scope:**

* Create warehouse
* Define warehouse basic information

**Out of scope:**

* Update Warehouse
* Delete Warehouse
* Manage Warehouses
* Warehouse activation / deactivation
* Warehouse hierarchy
* Inventory assignment to warehouse during creation

# 2. Acceptance Criteria

**Access Control**

**AC1:** If the user does not have CREATE_WAREHOUSE permission, the system rejects the request and returns an error response.

**Validation**

**AC2:** The system must validate the request data before processing.
**AC3:** If any validation rule fails, the system rejects the request and returns a validation error response.
**AC4:** If a warehouse with the same Warehouse Code already exists, the system rejects the request and returns a duplicate error response.

**Processing**

**AC5:** For a valid request, the system creates a new warehouse with Status = Active.
**AC6:** The system assigns system-managed fields including Created By, Created At, and Last Updated At.

**Data Persistence**

**AC7:** For a successful request, the system persists the warehouse data.
**AC8:** If the request fails at any stage, no warehouse data is persisted.

**Concurrency**

**AC9:** If multiple requests attempt to create a warehouse with the same Warehouse Code concurrently, the system ensures that only one warehouse is created.

**Response**

**AC10:** If the request is successful, the system returns a success response with the created warehouse data.
**AC11:** If the request fails, the system returns an error response.

**Data Consistency**

**AC12:** After successful creation, the warehouse must be retrievable with the same data as persisted.

# 3. Data Definition

## Warehouse

### Field Definition:

| **Field**       | **Type** | **Required** | **Default** | **Values**        | **Description**                          |
|-----------------|----------|--------------|-------------|-------------------|------------------------------------------|
| Warehouse Code  | string   | Yes          | -           | -                 | Unique identifier of the warehouse       |
| Warehouse Name  | string   | Yes          | -           | -                 | Human-readable name                      |
| Address         | string   | No           | -           | -                 | Warehouse address                        |
| Status          | enum     | Yes          | Active      | Active, Inactive  | Lifecycle status                         |
| Created By      | user     | Yes          | Current user| -                 | User who created the warehouse           |
| Created At      | datetime | Yes          | Current timestamp | -           | Timestamp when the warehouse was created |
| Last Updated At | datetime | Yes          | Current timestamp | -           | Timestamp of the last update             |

### Field Validation Rules:

1. **Warehouse Code**
   * Maximum length of 100 characters
   * Must be trimmed before validation and persistence
   * Must not be empty after trimming
   * Spaces are not allowed
   * Allowed characters: letters (a–z, A–Z), numbers (0–9), hyphen (-), underscore (_)
   * Must be unique across all warehouses (case-insensitive, ignoring leading/trailing spaces)
2. **Warehouse Name**
   * Maximum length of 255 characters
   * Must be trimmed before validation and persistence
   * Must not be empty after trimming
3. **Address**
   * If provided, maximum length of 255 characters
   * Must be trimmed before validation and persistence
4. **Status**
   * Must be system-assigned
   * Default value is Active
   * Any client-provided value must be ignored
5. **Created By**
   * Automatically assigned from the current authenticated user
6. **Created At**
   * Automatically set to the current timestamp
7. **Last Updated At**
   * Initially set to the same value as Created At
   * Automatically set to the current timestamp

### Validation Messages:

| **Field**       | **Rule**              | **Message**                                                                         |
|-----------------|-----------------------|-------------------------------------------------------------------------------------|
| Warehouse Code  | Required              | Warehouse Code is required.                                                         |
| Warehouse Code  | Empty after trimming  | Warehouse Code is required.                                                         |
| Warehouse Code  | Max length            | Warehouse Code must not exceed 100 characters.                                      |
| Warehouse Code  | Invalid format        | Warehouse Code may contain only letters, numbers, hyphen (-), or underscore (_).    |
| Warehouse Code  | Contains space        | Warehouse Code must not contain spaces.                                             |
| Warehouse Code  | Duplicate             | Warehouse Code already exists. Please use a different one.                          |
| Warehouse Name  | Required              | Warehouse Name is required.                                                         |
| Warehouse Name  | Empty after trimming  | Warehouse Name is required.                                                         |
| Warehouse Name  | Max length            | Warehouse Name must not exceed 255 characters.                                      |
| Address         | Max length            | Address must not exceed 255 characters.                                             |

# 4. Permission

| **Permission Key**  | **Description**                              |
|---------------------|----------------------------------------------|
| CREATE_WAREHOUSE    | Allows user to create a new warehouse record |

# 5. Business Rules

R1: The system enforces CREATE_WAREHOUSE permission for all create requests.
R2: Warehouse Code must not contain leading or trailing whitespace.
R3: Warehouse Code must be unique across all warehouses.
R4: Audit fields (created_by, created_at, last_updated_at) are automatically set on successful creation.

# 6. API Response Messages

| **Case**                                        | **Type**         | **Message**                                                                      |
|-------------------------------------------------|------------------|----------------------------------------------------------------------------------|
| User does not have CREATE_WAREHOUSE permission  | Error            | You do not have permission to create warehouse.                                  |
| Warehouse Code is missing                       | Validation Error | Warehouse Code is required.                                                      |
| Warehouse Code is empty after trimming          | Validation Error | Warehouse Code is required.                                                      |
| Warehouse Code exceeds maximum length           | Validation Error | Warehouse Code must not exceed 100 characters.                                   |
| Warehouse Code contains invalid characters      | Validation Error | Warehouse Code may contain only letters, numbers, hyphen (-), or underscore (_). |
| Warehouse Code contains space                   | Validation Error | Warehouse Code must not contain spaces.                                          |
| Warehouse Code already exists                   | Validation Error | Warehouse Code already exists. Please use a different one.                       |
| Warehouse Name is missing                       | Validation Error | Warehouse Name is required.                                                      |
| Warehouse Name is empty after trimming          | Validation Error | Warehouse Name is required.                                                      |
| Warehouse Name exceeds maximum length           | Validation Error | Warehouse Name must not exceed 255 characters.                                   |
| Address exceeds maximum length                  | Validation Error | Address must not exceed 255 characters.                                          |
| System failure during processing                | Error            | Failed to create warehouse. Please try again.                                    |
| System failure during persistence               | Error            | Failed to create warehouse. Please try again.                                    |
| Create warehouse successfully                   | Success          | Warehouse created successfully.                                                  |

# 7. User Flow

**[Start]**

**Access**

**1A. Access via Manage Warehouse Page (UI)**

User accesses the Warehouse Management page
System checks CREATE_WAREHOUSE permission
If the user does not have permission, the system hides the Create action → **[End]**
If the user has permission, the system displays the Create action
User clicks Create Warehouse
System displays the Create Warehouse page

**1B. Access via Direct URL**

User directly accesses the Create Warehouse page
System checks CREATE_WAREHOUSE permission
If the user does not have permission, the system shows a No Permission page → **[End]**
If the user has permission, the system displays the Create Warehouse page

**Input and Submit**

User inputs warehouse information
User submits the creation request

**Validation**

System trims input data before validation and persistence
System applies validation rules
System checks for duplicate Warehouse Code
If validation fails or a duplicate exists → The system displays validation errors on the Create Warehouse page (back to **Input and Submit** step)

**Processing**

System prepares the warehouse data
System sets audit fields (created_by, created_at, last_updated_at)
If a system-level failure occurs during processing → The system displays a system error message on the Create Warehouse page (back to **Input and Submit** step)

**Persistence**

System persists the warehouse data
If a system-level failure occurs during persistence → The system displays a system error message on the Create Warehouse page (back to **Input and Submit** step)

**Response**

System returns a success response and redirects to the Warehouse Management page and refreshes the list

**[End]**

# 8. States

### System State

1. Idle: The user is on the Manage Warehouse page (Create action may be visible or hidden based on permission).
2. Access Denied: The system shows a No Permission page when the user accesses the Create Warehouse page without CREATE_WAREHOUSE permission.
3. Input Form: The system displays the Create Warehouse page and allows user input.
4. Submitting: The system processes the submitted request and validates input data, including duplicate Warehouse Code check.
5. Processing: The system creates the warehouse and prepares data, including assigning audit fields.
6. Success: The warehouse is created successfully and the system redirects to the Manage Warehouse page and refreshes the list.
7. Validation Failed: The system displays validation errors.
8. Error: The system displays a system error message.

## State Transitions

1. Start → Idle: The user accesses the Manage Warehouse page.
2. Start → Access Denied: The user directly accesses the Create Warehouse page without CREATE_WAREHOUSE permission.
3. Idle → Input Form: The user has CREATE_WAREHOUSE permission and initiates the create action.
4. Input Form → Submitting: The user submits the creation request.
5. Submitting → Validation Failed: The input data is invalid or duplicate Warehouse Code exists.
6. Validation Failed → Input Form: The user remains on the Create Warehouse page and can correct the input.
7. Submitting → Processing: The input data is valid.
8. Processing → Success: The warehouse is created successfully.
9. Processing → Error: A system-level failure occurs during processing or persistence.
10. Error → Input Form: The user remains on the Create Warehouse page and can retry the submission.

---

# 9. Scenarios

**Happy Path**

**S1 — Create warehouse successfully with valid data**

Given the user has CREATE_WAREHOUSE permission
When the user accesses the Create Warehouse page
Then the system displays the warehouse creation form
And no warehouse exists with Warehouse Code "WH-001"
And the request contains valid Warehouse Code, Warehouse Name, and Address
When the user submits the warehouse creation request
Then the system returns success message: "Warehouse created successfully."
And the response includes the created warehouse data
And the response includes Warehouse Code, Warehouse Name, Address, Status, Created By, Created At, and Last Updated At
And the response values match the persisted warehouse data
And the warehouse data is persisted

**S2 — Created warehouse is retrievable with persisted data**

Given a warehouse has been created with Warehouse Code "WH-001"
When the warehouse is retrieved by Warehouse Code "WH-001"
Then the returned warehouse data matches the persisted warehouse data

**Alternative Flows**

**S3 — Create warehouse without Address**

Given the user has CREATE_WAREHOUSE permission
When the user accesses the Create Warehouse page
Then the system displays the warehouse creation form
And no warehouse exists with Warehouse Code "WH-002"
And the request contains valid Warehouse Code and Warehouse Name
And Address is not provided
When the user submits the warehouse creation request
Then the system returns success message: "Warehouse created successfully."
And the response includes the created warehouse data
And the warehouse data is persisted without Address

**Security Scenarios**

**S22 — Reject warehouse creation request without permission**

Given the user does not have CREATE_WAREHOUSE permission
And the request contains valid warehouse creation data
When the user submits the warehouse creation request
Then the system rejects the request
And the system returns authorization error
And the system returns error message: "You do not have permission to create warehouse."
And no warehouse data is persisted
