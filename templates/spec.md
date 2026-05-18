# 1. Brief

**Feature name:** Create Product Category

**Goal:** Allow authorized users to create product categories so that products can be classified during product creation.

**In scope:**

* Create product category
* Define category basic information

**Out of scope:**

* Update Product Category
* Delete Product Category
* Category management
* Category hierarchy
* Product assignment to category

---

# 2. Acceptance Criteria

**Access Control**

AC1: If the user does not have CREATE_PRODUCT_CATEGORY permission, the system rejects the request and returns an error response.

**Validation**

AC2: If the request violates defined validation rules, the system rejects the request and returns an error response.
AC3: If a product category with the same name already exists, the system rejects the request.

**Processing**

AC4: For a valid request, the system creates a new product category.

**Data Persistence**

AC5: For a successful request, the system persists the product category data.
AC6: If the request fails at any stage, no product category data is persisted.

**Concurrency**

AC7: If multiple requests attempt to create a product category with the same name concurrently, the system ensures that only one category is created and all other requests are rejected.

**Response**

AC8: If the request is successful, the system returns a success response with the created product category data.
AC9: If the request fails, the system returns a standardized error response.

**Data Consistency**

AC10: After successful creation, the product category must be retrievable with the same data as persisted.

---

# 3. Data Definition

## Product Category

### Field Definition:

| **Field**        | **Type** | **Required** | **Default**       | **Description**                                      |
|------------------|----------|--------------|-------------------|------------------------------------------------------|
| Name             | string   | Yes          | -                 | Name of the product category used for classification |
| Description      | string   | No           | -                 | Additional information about the category            |
| Created By       | user     | Yes          | Current user      | User who created the category                        |
| Created At       | datetime | Yes          | Current timestamp | Timestamp when the category was created              |
| Last Updated By  | user     | Yes          | Current user      | User who last updated the category                   |
| Last Updated At  | datetime | Yes          | Current timestamp | Timestamp when the category was last updated         |

### Field Validation Rules:

1. **Name**
   * Maximum length of 255 characters
   * Must be trimmed before validation and persistence
   * Must not be empty
   * Must be unique across all existing product categories (case-insensitive, ignore leading/trailing spaces)

2. **Description**
   * If provided, maximum length of 1000 characters

3. **Created By**
   * Automatically assigned from the current authenticated user

4. **Created At**
   * Automatically set to the current timestamp

5. **Last Updated By**
   * Initially set to the same value as created_by

6. **Last Updated At**
   * Initially set to the same value as created_at

### Field Validation Error Messages:

| **Field**      | **Rule**                       | **Message**                                              |
|----------------|--------------------------------|----------------------------------------------------------|
| Category Name  | Required (missing)             | Category Name is required.                               |
| Category Name  | Required (empty after trimming)| Category Name is required.                               |
| Category Name  | Max length (255 characters)    | Category Name must not exceed 255 characters.            |
| Category Name  | Invalid format (if applicable) | Category Name is invalid.                                |
| Category Name  | Duplicate                      | Category Name already exists. Please use a different one.|
| Description    | Max length (1000 characters)   | Description must not exceed 1000 characters.             |

---

# 4. Permission

| **Permission Key**       | **Description**                           |
|--------------------------|-------------------------------------------|
| CREATE_PRODUCT_CATEGORY  | Allows user to create new product category|

---

# 5. Business Rules

R1: The system enforces CREATE_PRODUCT_CATEGORY permission for all create requests.

R2: Category Name must not contain leading or trailing whitespace.

R3: Product category name must be unique across the system.

R4: Audit fields (created_by, created_at, last_updated_by, last_updated_at) are automatically set on successful creation.

---

# 6. API Response Messages

| **Case**                                            | **Type**         | **Message**                                              |
|-----------------------------------------------------|------------------|----------------------------------------------------------|
| User does not have CREATE_PRODUCT_CATEGORY permission| Error            | You do not have permission to create product category.   |
| Category Name is missing                            | Validation Error | Category Name is required.                               |
| Category Name is empty after trimming               | Validation Error | Category Name is required.                               |
| Category Name exceeds maximum length                | Validation Error | Category Name must not exceed 255 characters.            |
| Category Name is invalid format (if applicable)     | Validation Error | Category Name is invalid.                                |
| Product category name already exists                | Validation Error | Category Name already exists. Please use a different one.|
| Category Description exceeds maximum length         | Validation Error | Description must not exceed 1000 characters.             |
| System failure during processing                    | Error            | Failed to create product category. Please try again.     |
| System failure during persistence                   | Error            | Failed to create product category. Please try again.     |
| Create product category successfully                | Success          | Product category created successfully.                   |
