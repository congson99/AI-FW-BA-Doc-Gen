# Flow

**[Start]**

1. **Access**

1A. **Access via Product Category Page (UI)**

* User accesses the Product Category Management page
* System checks CREATE_PRODUCT_CATEGORY permission
* If the user does not have permission, the system hides the Create action → **[End]**
* If the user has permission, the system displays the Create action
* User clicks Create Product Category
* System displays the Create Product Category page

1B. **Access via Direct URL**

* User directly accesses the Create Product Category page
* System checks CREATE_PRODUCT_CATEGORY permission
* If the user does not have permission, the system shows a No Permission page → **[End]**
* If the user has permission, the system displays the Create Product Category page

2. **Input and Submit**

* User inputs product category information
* User submits the creation request

3. **Validation**

* System trims input data before validation and persistence
* System applies validation rules
* System checks for duplicate category name
* If validation fails or a duplicate exists → The system displays validation errors on the Create Product Category page (back to **Input and Submit** step)

4. **Processing**

* System prepares the product category data
* System sets audit fields (created_by, created_at, last_updated_by, last_updated_at)
* If a system-level failure occurs during processing → The system displays a system error message on the Create Product Category page (back to **Input and Submit** step)

5. **Persistence**

* System persists the product category data
* If a system-level failure occurs during persistence → The system displays a system error message on the Create Product Category page (back to **Input and Submit** step)

6. **Response**

* System returns a success response and redirects to the Product Category Management page and refreshes the list

**[End]**

---

# States

## System States

1. Idle: The user is on the Product Category Management page (Create action may be visible or hidden based on permission).
2. Access Denied: The system shows a No Permission page when the user accesses the Create Product Category page without CREATE_PRODUCT_CATEGORY permission.
3. Input Form: The system displays the Create Product Category form and allows user input.
4. Submitting: The system processes the submitted request and validates input data, including duplicate category name check.
5. Processing: The system creates the product category and persists the data, including setting audit fields.
6. Success: The product category is created successfully and the system redirects to the Product Category Management page and refreshes the list.
7. Validation Failed: The system displays validation errors.
8. Error: The system displays a system error message.

## State Transitions

1. Start → Idle: The user accesses the Product Category Management page.
2. Start → Access Denied: The user directly accesses the Create Product Category page without CREATE_PRODUCT_CATEGORY permission.
3. Idle → Input Form: The user has CREATE_PRODUCT_CATEGORY permission and initiates the create action.
4. Input Form → Submitting: The user submits the creation request.
5. Submitting → Validation Failed: The input data is invalid.
6. Submitting → Processing: The input data is valid.
7. Processing → Success: The product category is created successfully.
8. Processing → Error: A system-level failure occurs during processing or persistence.
