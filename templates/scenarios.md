**Happy Path**

**S1 – Create product category successfully via UI**

Given the user has CREATE_PRODUCT_CATEGORY permission
And the user is on the Product Category Management page
When the user clicks Create Product Category
Then the system displays the Create Product Category page
And the user inputs valid product category information
When the user submits the creation request
Then the system validates the input data
And the system creates and persists the product category
And the system redirects to the Product Category Management page
And the system refreshes the list
And the system displays "Product category created successfully." as a toast notification

**S2 – Create product category successfully via direct URL**

Given the user has CREATE_PRODUCT_CATEGORY permission
When the user directly accesses the Create Product Category page
Then the system displays the Create Product Category page
And the user inputs valid product category information
When the user submits the creation request
Then the system creates and persists the product category
And the system redirects to the Product Category Management page
And the system refreshes the list
And the system displays "Product category created successfully." as a toast notification

**Permission Scenarios**

**S3 – Hide Create action when user has no permission**

Given the user does not have CREATE_PRODUCT_CATEGORY permission
When the user accesses the Product Category Management page
Then the system hides the Create Product Category action

**S4 – Block access via direct URL when no permission**

Given the user does not have CREATE_PRODUCT_CATEGORY permission
When the user directly accesses the Create Product Category page
Then the system displays a No Permission page

**Validation Scenarios – Category Name**

**S5 – Category Name is missing**

Given the user is on the Create Product Category page
When the user submits the form without Category Name
Then the system displays the error message "Category Name is required." for the Category Name field (inline)
And the user remains on the Create Product Category page

**S6 – Category Name is empty after trimming**

Given the user is on the Create Product Category page
When the user submits Category Name with only spaces
Then the system trims the input
And the system displays "Category Name is required."
And the user remains on the Create Product Category page

**S7 – Category Name exceeds maximum length**

Given the user is on the Create Product Category page
When the user submits Category Name longer than 255 characters
Then the system displays the error message "Category Name must not exceed 255 characters." for the Category Name field (inline)
And the user remains on the Create Product Category page

**S8 – Category Name already exists**

Given an existing product category with the same name exists
When the user submits the creation request
Then the system displays the error message "Category Name already exists. Please use a different one." for the Category Name field (inline)
And the user remains on the Create Product Category page

**Validation Scenarios – Description**

**S9 – Description exceeds maximum length**

Given the user is on the Create Product Category page
When the user submits Description longer than 1000 characters
Then the system displays the error message "Description must not exceed 1000 characters." for the Description field (inline)
And the user remains on the Create Product Category page

**Retry Behavior**

**S10 – User corrects validation error and resubmits**

Given the user is on the Create Product Category page
And validation errors are displayed
When the user corrects the input
And submits the form again
Then the system validates the input
And the system proceeds with creation if valid

**Data Validation – Input Transformation**

**S11 – Input trimming before processing**

Given the user is on the Create Product Category page
When the user submits Category Name with leading and trailing spaces
Then the system trims the input values
And the system uses the trimmed values for validation and persistence

**Data Integrity – Audit Fields**

**S12 – Audit fields on successful creation**

Given the user submits a valid creation request
When the system creates the product category
Then the system sets created_by to the current authenticated user
And the system sets created_at to the current timestamp
And the system sets last_updated_by to the same value as created_by
And the system sets last_updated_at to the same value as created_at

**System Error Scenarios**

**S13 – System failure during processing**

Given the user submits a valid creation request
When a system-level failure occurs during processing
Then the system displays an error message "Failed to create product category. Please try again." as a toast notification
And the user remains on the Create Product Category page

**S14 – System failure during persistence**

Given the user submits a valid creation request
When a system-level failure occurs during persistence
Then the system displays an error message "Failed to create product category. Please try again." as a toast notification
And the user remains on the Create Product Category page

**Data Consistency**

**S15 – Created category is retrievable with correct data**

Given a product category is created successfully
When the system retrieves the product category data
Then the retrieved data matches the data that was persisted

**Concurrency**

**S16 – Concurrent requests with same category name**

Given multiple users are on the Create Product Category page
And no product category with the given name exists
When the users submit creation requests with the same category name at the same time
Then the system creates only one product category
And the system returns a success response for one request
And the system rejects all other requests with an error response
And the system displays the error message "Category Name already exists. Please use a different one." for the Category Name field (inline)
And only one product category record is persisted
