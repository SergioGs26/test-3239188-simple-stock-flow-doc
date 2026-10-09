# User Stories — Simple Stock Flow

## 1. Purpose

This document describes the user stories inferred from the data model defined in `spec/data-model.md`. Each story includes acceptance criteria and traceability to the source model.

The data model is the source of truth for entities, business rules, and data constraints. Any behavior not fully defined by the model is explicitly marked as an **Assumption**.

## 2. User Roles

The data model defines two internal operator roles:

* **Admin:** An internal operator with the `admin` role.
* **Seller:** An internal operator with the `seller` role.

The exact permissions assigned to each role must be confirmed separately. The data model does not define a complete authorization policy for every operation.

**Traceability:** `spec/data-model.md`, Sections 1 and 2.5.

## 3. User Stories

### US-01 — View Product Catalog

**As an** internal operator,
**I want** to view the available products,
**so that** I can check their prices, stock levels, and categories.

**Acceptance criteria:**

* Each product has a name, price, stock quantity, and category.
* A product may have an optional image reference.
* The system must not treat a negative stock quantity as valid.
* Products marked as logically deleted must not appear as active products.

**Traceability:** `spec/data-model.md`, Sections 1, 2.2, 3, and 6.

### US-02 — Manage Products

**As an** authorized internal operator,
**I want** to maintain product information,
**so that** the catalog reflects the current products available for sale.

**Acceptance criteria:**

* A product has a name, a price greater than zero, a non-negative stock quantity, a category, and an optional image key.
* The product name must not be empty and must be trimmed before storage.
* The selected category must exist.
* The image key must be treated as an opaque reference to external storage. If no image is assigned, its value must be `NULL`.
* Product stock must never become negative.
* Product removal must use logical deletion rather than physical deletion.

**Assumption:** The model supports product changes through domain operations, but it does not specify which role may create, update, or remove products.

**Traceability:** `spec/data-model.md`, Sections 1, 2.2, 3, 4, and 7.

### US-03 — View Product Categories

**As an** internal operator,
**I want** to view the available product categories,
**so that** I can identify the classification assigned to each product.

**Acceptance criteria:**

* The system uses five predefined categories.
* Each category has a unique name.
* Products must reference an existing category.
* Categories are read-only reference data and cannot be created, updated, or deleted through a category repository.

**Traceability:** `spec/data-model.md`, Sections 1, 2.1, and 9.

### US-04 — Register a Sale

**As a** seller,
**I want** to register a sale with its products and quantities,
**so that** the transaction is recorded and the corresponding stock is withdrawn.

**Acceptance criteria:**

* A sale records the operator responsible and the date and time of the transaction.
* A sale must contain at least one line before it can be confirmed.
* Each sale line references a product and records its quantity, product name, category name, and unit price as defined by the data model.
* The quantity in each line must be greater than zero.
* The same product cannot appear more than once in the same sale.
* Adding a sale line and withdrawing the corresponding stock must be handled as one business operation.
* A sale cannot withdraw more stock than is available.
* The product name and unit price recorded in a sale line must preserve the values from the time of the transaction.
* Once registered, a completed sale cannot be edited or deleted through the domain operations described by the model.
* The sale total and line subtotals must be calculated, not stored as database columns.

**Traceability:** `spec/data-model.md`, Sections 1, 2.2–2.4, 3, 4, and 6.

### US-05 — Consult Sales Reports

**As an** internal operator,
**I want** to consult a sales report for a selected date range,
**so that** I can review sales aggregated by product.

**Acceptance criteria:**

* The report uses a start date and an end date.
* The end date cannot be earlier than the start date.
* The report aggregates sales information by product over the selected period.
* The report is calculated when requested and is not stored as a separate database entity.
* Historical sale-line values must be used instead of relying only on the current product catalog.

**Traceability:** `spec/data-model.md`, Sections 1, 2.3–2.4, and 6.

### US-06 — Authenticate an Internal Operator

**As an** internal operator,
**I want** to authenticate using my account,
**so that** I can access the system as an identified user.

**Acceptance criteria:**

* Each user has a unique username.
* Usernames are stored in lowercase and trimmed by the domain.
* A user record stores a password hash rather than a plain-text password.
* The system supports the roles `admin` and `seller`.
* The domain must not receive or store the user's password in plain text.

**Assumption:** The data model defines users and password-hash storage, but it does not fully specify the login interface, session handling, or authentication protocol.

**Traceability:** `spec/data-model.md`, Sections 1, 2.5, 3, and 7.

## 4. General Notes

* The five database tables are `category`, `product`, `sale`, `sale_item`, and `user`.
* Database identifiers use English `snake_case`; domain entity names use singular `PascalCase`.
* Rules enforced only by the domain must not be described as database-enforced constraints.
* Pending database changes must remain identified as pending until verified in the data model.
* No customer entity is defined in the model. The user associated with a sale is an internal operator.
* The user stories describe behavior inferred from the data model; they do not replace the project's formal business requirements.

## 5. Source of Truth

All stories are based on `spec/data-model.md`. Assumptions are identified explicitly and must be validated against the project's authoritative requirements before implementation.
