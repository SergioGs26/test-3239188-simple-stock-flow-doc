# System Scope — Simple Stock Flow

## 1. Purpose

This document defines the scope of Simple Stock Flow based on the requirements and data model described in `spec/data-model.md`. It identifies the main capabilities, business constraints, and boundaries supported by the current model.

## 2. In Scope

The system includes the following capabilities:

* **Product catalog:** Register and manage products with their names, prices, stock quantities, categories, and optional image keys.
* **Category management:** Use the five predefined categories. Categories are seeded and read-only.
* **Stock control:** Maintain product stock and prevent stock quantities from becoming negative.
* **Sales registration:** Register completed sales associated with an internal user.
* **Sale items:** Store the products included in each sale, their quantities, and snapshots of product names, unit prices, and category names at the time of the sale.
* **User management:** Support internal users with the `admin` and `seller` roles.
* **Sales reports:** Calculate reports grouped by product within a selected date range. Reports are generated from stored data instead of being saved as separate records.
* **Data integrity:** Apply validations and database constraints to preserve the consistency of products, categories, sales, and sale items.
* **Product deletion:** Use logical deletion for products, according to the data model.
* **Security and privacy:** Store password hashes instead of plain-text passwords and follow the privacy and retention rules defined in the data model.

**Source:** `spec/data-model.md`, Sections 2–9.

## 3. Business Constraints

The system must respect these rules:

1. Product prices must be greater than zero.
2. Stock quantities cannot be negative.
3. A sale must contain at least one sale item.
4. The same product cannot appear more than once in a single sale.
5. Sale item quantities must be greater than zero.
6. Sales represent completed commercial events and are immutable.
7. Sale items preserve product information as it was when the sale occurred.
8. Stock withdrawal and sale item registration must be handled atomically.
9. Usernames must be unique, lowercase, and trimmed.
10. User roles are limited to `admin` and `seller`.
11. A report's end date cannot be earlier than its start date.

**Source:** `spec/data-model.md`, Sections 2, 3, 5, and 6.

## 4. Out of Scope or Not Defined

The current data model does not define the following capabilities:

* Customer or buyer registration, because there is no customer entity.
* Customer relationship management.
* Saving reports as independent database records.
* Managing an unlimited set of categories or creating custom categories.
* Storing image files directly in the database; the model only defines an optional image key.
* Editing or deleting completed sales.
* Multi-currency support or a currency field.
* Additional product attributes not defined in the model, such as SKU or product description.
* A specific frontend framework, backend framework, deployment platform, or external integration, because these are not established by the data model.

These items should not be considered confirmed requirements unless they are defined in another approved project document.

## 5. Scope Boundaries and Assumptions

This document describes the scope supported by the current data model. It does not assume that every business rule is already enforced by a database constraint or that every application interface has been implemented.

Where implementation details or requirements remain unresolved, they must be reviewed against the pending items and declared technical debt in `spec/data-model.md`.

## 6. Traceability

The scope is based on the following sections of `spec/data-model.md`:

* Section 1: Domain glossary.
* Section 2: Entities and invariants.
* Section 3: Physical model.
* Section 4: Constraints and indexes.
* Section 5: Foreign key policy.
* Section 6: Access patterns and indexes.
* Section 7: Privacy and retention.
* Section 8: Auditing.
* Section 9: Seed strategy.
* Sections 11 and 13: Open gaps and declared debt.

This scope should be reviewed whenever the approved data model or project requirements change.
