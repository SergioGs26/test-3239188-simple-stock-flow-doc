# System Overview — Simple Stock Flow

## 1. Purpose

Simple Stock Flow is an inventory and sales management system for handling products, stock quantities, sales transactions, and internal users.

Its data model defines five main entities: `Category`, `Product`, `Sale`, `SaleItem`, and `User`. These entities support the product catalog, stock control, sales registration, and the calculation of sales reports.

**Source:** `spec/data-model.md`, Sections 1 and 2.

## 2. System Goals

The system is intended to support the following business activities:

* Maintain a catalog of products with their names, prices, stock quantities, categories, and optional image references.
* Keep product stock from becoming negative.
* Register sales with their corresponding items and quantities.
* Preserve product information and unit prices as they were when a sale was registered.
* Identify the internal user responsible for each sale.
* Provide sales reports grouped by product for a selected date range.

These goals are derived from the documented entities, domain rules, and report model.

**Source:** `spec/data-model.md`, Sections 1, 2, and 6.

## 3. Main Users

The data model defines two user roles:

* **Admin:** An internal user with the `admin` role.
* **Seller:** An internal user with the `seller` role who can be associated with registered sales.

The model defines these roles, but the complete permissions for each role belong to the application authorization rules and must not be inferred from the database structure alone.

A customer or buyer is not represented as a separate entity in the current model.

**Source:** `spec/data-model.md`, Sections 1, 2.3, and 2.5.

## 4. Main Business Components

### 4.1 Product Catalog

The catalog contains products and their categories. Each product has a name, a positive price, a non-negative stock quantity, a category, and an optional image key.

The system uses a fixed set of five categories. Categories are seeded and treated as read-only reference data.

**Source:** `spec/data-model.md`, Sections 1, 2.1, and 2.2.

### 4.2 Sales Registration

A sale records a completed commercial transaction. It must contain at least one sale item and identify the user responsible for registering it.

Each sale item records a product reference, quantity, and historical product information. The same product cannot appear more than once in the same sale.

Adding a sale item and withdrawing stock must be handled as one domain operation.

**Source:** `spec/data-model.md`, Sections 2.2, 2.3, and 2.4.

### 4.3 Sales Reporting

The system calculates sales reports for a requested date range. Reports are grouped by product and use historical information captured in sale items.

Reports are calculated through a database read port. They are not stored as a separate entity or table.

The end of a date range cannot be earlier than its start.

**Source:** `spec/data-model.md`, Sections 1 and 6.

### 4.4 User Management

The system stores internal users with a unique username, a password hash, and a role.

Usernames are normalized to lowercase and trimmed. Plain-text passwords must not be handled by the domain.

**Source:** `spec/data-model.md`, Section 2.5 and Section 7.

## 5. Main Data Relationships

The documented model contains five entities:

* A category can classify multiple products.
* A product can appear in multiple sale items.
* A sale contains one or more sale items.
* A user can be associated with the sales they register.

A sale item belongs to its sale and preserves relevant values from the time of the transaction. The model defines foreign key policies for these relationships, including restrictions intended to protect referenced data.

**Source:** `spec/data-model.md`, Sections 2 and 5.

## 6. Important Business Constraints

The system must respect these documented rules:

* Product prices must be greater than zero.
* Product stock cannot be negative.
* A stock withdrawal cannot exceed the available quantity.
* Sale quantities must be greater than zero.
* Every sale must contain at least one item.
* A product cannot be repeated within the same sale.
* Registered sales are immutable.
* Sale item snapshots preserve historical values.
* Usernames must be unique and normalized.
* Passwords are represented by hashes, not plain-text values.

Not all these rules are enforced directly by the database. Their implementation status must be checked in `spec/data-model.md`, where rules are classified as database-enforced, domain-only, or pending.

**Source:** `spec/data-model.md`, Sections 2 and 4.

## 7. Scope Boundaries

This overview describes the system based on its data model. It does not define screens, API endpoints, deployment infrastructure, or a specific technology stack unless another approved document establishes them.

The current data model does not define:

* A separate customer or buyer entity.
* A persisted sales-report table.
* A separate currency column.
* Additional product attributes beyond the documented catalog fields.
* A separate domain-event table.

These boundaries prevent the overview from promising features that are not supported by the source specification.

**Source:** `spec/data-model.md`, Sections 1, 2, 3, and 11.

## 8. Source of Truth and Assumptions

The primary source for this overview is `spec/data-model.md`. It defines the current entities, attributes, relationships, constraints, access patterns, and documented limitations.

The following points should be treated carefully:

* The existence of a role does not, by itself, define all its permissions.
* A domain rule is not automatically a database constraint.
* The data model does not prove that a particular interface or external integration exists.
* Any new capability not supported by the source must be identified as an assumption and reviewed before it becomes a requirement.

The overview should be updated when an approved change modifies the domain model or the system's documented scope.
