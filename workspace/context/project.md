# Project Context

## System Overview

The Inventory Platform provides a centralized and reusable inventory management module that can be integrated into different business systems requiring inventory capabilities.

---

## Modules / Feature Areas

### Product Management

| Feature | Description | User Stories |
|---|---|---|
| Product Management | Manage the list of products in the system, including creating, updating, viewing, and deleting products. | Manage Products, Save and Manage Product Filters, Create Product, View Product Detail, Edit Product Info, Delete Product, Activate / Deactivate Product, Track Product Logs |
| Product Stock | View and track the current stock quantity of each product in the system. | View Product Stock, Stock Movement History |
| Purchase Request | Allow users to create requests for purchasing products when stock needs to be replenished. | Create PR, Manage PR, View PR Detail, Update PR, Submit PR, Approve / Reject PR, Delete PR, Cancel PR, View PR Activity Log, Reopen PR, Close PR |

### Inbound Management

| Feature | Description | User Stories |
|---|---|---|
| Purchase Order | Manage purchase orders sent to suppliers based on approved purchase requests. | Create PO, Manage PO, View PO Information, Update PO, Delete PO, Submit PO, Approve / Reject PO, Reopen PO, Cancel PO, Close PO, View PO Activity Log |
| Inbound Receipt | Record goods received from suppliers and update stock accordingly. | Manage IR, View IR Information, Update IR, Delete IR, Approve IR, Receive IR |

### Outbound Management

| Feature | Description | User Stories |
|---|---|---|
| Stock Issue | Manage customer orders and handle the process of issuing goods from inventory. | Create SI, View SI Information, Update SI, Delete SI, Approve SI, Receive SI |

### Master Data

| Feature | Description | User Stories |
|---|---|---|
| Warehouse | Manage warehouse information and storage locations. | Manage Warehouses, Create Warehouse, View Warehouse Detail, Update Warehouse Info, Activate / Deactivate Warehouse, Delete Warehouse |
| Supplier | Manage supplier information used for procurement processes. | Create Supplier, Manage Suppliers, View Supplier Information, Delete Supplier, Update Supplier |
| Customer | Manage customer information for sales activities. | Create Customer, Manage Customers, View Customer Information, Delete Customer, Update Customer |
| Product | Manage foundational product-related data such as categories, stock status. | Create Product Category, Manage Product Category, Update Product Category, Delete Product Category, Create Unit of Measure, Manage Unit of Measure, Update Unit of Measure, Delete Unit of Measure |

### System Configuration

| Feature | Description | User Stories |
|---|---|---|
| Product Configuration | Allows administrators to configure system rules for handling file imports and product-related behaviors. | View Product Configuration, Update Product Configuration |

### Authorization

| Feature | Description | User Stories |
|---|---|---|
| Authorization | — | Validate User Access [BE Only], Centralized Permission Registry [BE Only] |

---

## Notes for AI

- Khi gen brief, đối chiếu tên feature với danh sách module để đặt đúng ngữ cảnh
