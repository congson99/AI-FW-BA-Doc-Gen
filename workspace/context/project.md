# Project Context

## System Overview

The Inventory Platform provides a centralized and reusable inventory management module that can be integrated into different business systems requiring inventory capabilities.

---

## Modules / Feature Areas

### Product Management

| Feature | Description |
|---|---|
| Product Management | Manage the list of products in the system, including creating, updating, viewing, and deleting products. |
| Product Stock | View and track the current stock quantity of each product in the system. |
| Purchase Request | Allow users to create requests for purchasing products when stock needs to be replenished. |

### Inbound Management

| Feature | Description |
|---|---|
| Purchase Order | Manage purchase orders sent to suppliers based on approved purchase requests. |
| Inbound Receipt | Record goods received from suppliers and update stock accordingly. |

### Outbound Management

| Feature | Description |
|---|---|
| Stock Issue | Manage customer orders and handle the process of issuing goods from inventory. |

### Master Data

| Feature | Description |
|---|---|
| Warehouse | Manage warehouse information and storage locations. |
| Supplier | Manage supplier information used for procurement processes. |
| Customer | Manage customer information for sales activities. |
| Product | Manage foundational product-related data such as categories, stock status. |

### System Configuration

| Feature | Description |
|---|---|
| Product Configuration | Allows administrators to configure system rules for handling file imports and product-related behaviors. |

---

## Notes for AI

- Khi gen brief, đối chiếu tên feature với danh sách module để đặt đúng ngữ cảnh
- Actor mặc định nếu không rõ: dùng "authorized users"
- Goal phải phản ánh nghiệp vụ, không phải kỹ thuật
