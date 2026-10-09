# Product Backlog — Simple Stock Flow

## 1. Purpose

This document lists the proposed product backlog for Simple Stock Flow. The items are based on the available data model and describe the main capabilities needed to manage products, stock, sales, and sales reports.

**Important:** Priorities and acceptance criteria in this document are proposals. They must be validated with the project stakeholders before being treated as final requirements.

## 2. Backlog Items

### US-01 — Manage Products

**User story**

As an administrator, I want to manage product information so that the catalog stays updated.

**Acceptance criteria**

* A product can store its name, price, stock, category, and optional image key.
* The product must reference an existing category.
* The product price must be greater than zero according to the domain rule.
* Stock cannot be negative.
* Product creation, editing, and deletion permissions must be confirmed.

**Priority:** Proposed — High

### US-02 — Consult the Product Catalog

**User story**

As a seller, I want to consult the product catalog so that I can identify the products available for sale.

**Acceptance criteria**

* The system displays the available product information.
* The product category is associated with each product.
* The displayed stock reflects the current recorded inventory.
* Search, filtering, and sorting options are not specified in the current data model.

**Priority:** Proposed — High

### US-03 — Consult Categories

**User story**

As an administrator, I want to consult product categories so that products can be organized consistently.

**Acceptance criteria**

* The system uses the categories defined in the data model.
* The five initial categories are seeded and treated as read-only.
* Users cannot modify the initial category records through normal category management operations.

**Priority:** Proposed — High

### US-04 — Record a Sale

**User story**

As a seller, I want to record a sale so that the transaction is saved and the corresponding stock is updated.

**Acceptance criteria**

* A sale contains its corresponding sale items.
* Each sale item records the product name, unit price, and category name as they were at the time of the sale.
* Each sale item has a quantity greater than zero according to the domain rule.
* The sale and its stock withdrawals are handled as one business operation.
* If the operation fails, the system must not leave a partially recorded sale or an inconsistent stock balance.
* The sale total is calculated from its items rather than stored as a separate value.

**Priority:** Proposed — Highest

### US-05 — Validate Stock Availability

**User story**

As a seller, I want the system to validate stock availability so that a sale does not produce negative inventory.

**Acceptance criteria**

* The system checks whether enough stock is available for the requested quantity.
* Stock cannot become negative.
* The sale and inventory update must preserve consistency when transactions are processed.
* The specific behavior for simultaneous sales involving the same product must be defined and tested.

**Priority:** Proposed — Highest

### US-06 — Consult Sales History

**User story**

As an administrator, I want to consult recorded sales so that I can review completed transactions.

**Acceptance criteria**

* The system stores completed sales as commercial records.
* Sale records are treated as immutable completed facts.
* Sale items preserve the relevant product information and unit price from the time of the transaction.
* The available data model does not specify the exact search filters or interface for sales history.

**Priority:** Proposed — High

### US-07 — Generate Sales Reports

**User story**

As an administrator, I want to consult sales reports for a date range so that I can review sales performance by product.

**Acceptance criteria**

* The report uses a defined date range.
* Sales information is aggregated by product.
* Report values are calculated from the recorded sales data.
* Reports are generated through a read-oriented component and are not stored as separate persistent records.
* The exact report fields and date boundaries must be confirmed.

**Priority:** Proposed — High

### US-08 — Manage Internal User Roles

**User story**

As an administrator, I want internal users to have defined roles so that access can be organized according to their responsibilities.

**Acceptance criteria**

* The data model identifies the `admin` and `seller` roles.
* Passwords are stored as hashes rather than plain text.
* The system must not store plain-text passwords.
* The specific permissions assigned to each role must be confirmed before implementation.

**Priority:** Proposed — High

## 3. Proposed Implementation Order

The following order is a proposal and is not a confirmed release plan.

1. **Data foundations:** categories, products, and internal user roles.
2. **Inventory integrity:** stock validation and product availability.
3. **Sales operations:** recording sales and updating stock consistently.
4. **Sales history:** consulting completed transactions.
5. **Reporting:** generating sales summaries by product and date range.

## 4. Dependencies and Constraints

* Product records depend on valid category references.
* Sales depend on existing products and sufficient stock.
* Sale items must preserve relevant product information at the time of the sale.
* Sale recording and stock withdrawal must remain consistent as one business operation.
* The database technology defined in the data model is PostgreSQL 16.14, using the `simple_stock_flow` database and the `sales` schema.
* Database enforcement of some domain rules must be checked against the current implementation status.

## 5. Open Questions

* Which users can create, edit, or delete products?
* What specific permissions belong to the `admin` and `seller` roles?
* Can a completed sale be cancelled, refunded, or corrected? The data model does not define this behavior.
* Which fields must appear in the sales report?
* Which search and filtering options are required for products and sales?
* How should the system handle simultaneous sales of the same product?
* What authentication and session-management mechanisms will be used?
* Which acceptance tests will be required before each backlog item is considered complete?

## 6. Traceability

This backlog is derived from `spec/data-model.md`. The user stories describe proposed system capabilities based on the entities, fields, relationships, and business rules documented there.

Any feature not explicitly supported by the data model is identified as a proposal or an open question and must be validated before implementation.
