# Domain Rules — Simple Stock Flow

## 1. Purpose

This document describes the business rules that govern products, sales, sale items, categories, and users. It also identifies where each rule is enforced according to the current data model.

**Main source:** `spec/data-model.md`, Sections 2, 4, and 5.

## 2. Category Rules

* A category must have a non-empty name. During initial seeding, category names are trimmed before being saved.
* Category names must be unique.
* The system uses five predefined categories created during the initial migration.
* Categories are reference data. The current domain does not provide operations to create, rename, or delete them.

**Enforcement status:**

* Name uniqueness is enforced by a database unique index.
* Required and trimmed names are currently enforced by the domain. Database enforcement for the non-empty rule is pending under T-20.

**Source:** `spec/data-model.md`, Sections 2.1 and 9.

## 3. Product Rules

* A product must have a non-empty name. The domain trims the name before saving it.
* A product price must be greater than zero.
* Product stock must never be negative.
* A stock withdrawal must fail when the requested quantity exceeds the available stock.
* Every product must reference an existing category.
* An absent image must be represented by `NULL`, not an empty string.
* Products use logical deletion instead of physical deletion.
* Money values are rounded to two decimal places using `MidpointRounding.AwayFromZero`.

**Enforcement status:**

* The database enforces non-negative stock through a check constraint.
* The database enforces the category relationship through a foreign key.
* The domain enforces positive prices and rejects withdrawals that exceed available stock. A database check for positive prices is pending under T-20.
* Image normalization is handled by the domain.
* Logical deletion uses `deleted_at` and a global filter.
* The database's `NOT NULL` constraint on the product name does not, by itself, reject an empty or whitespace-only name.

**Source:** `spec/data-model.md`, Sections 2.2, 3, 4, and 7.

## 4. Sale Rules

* Every sale must identify the user responsible for registering it.
* A sale must contain at least one sale item before it can be confirmed.
* The same product cannot appear more than once in a sale.
* Adding a sale item and withdrawing the corresponding stock must be handled as one domain operation.
* A registered sale cannot be edited or deleted through the domain's available operations.
* The sale total is calculated by adding the line subtotals. It is not stored in a separate database column.

**Enforcement status:**

* The user reference is required by the database, while the domain checks that the responsible user is valid.
* The minimum of one sale item is enforced by the domain; the database does not enforce it with a simple check constraint.
* Duplicate products are rejected by the domain and also prevented by a database unique index on `(sale_id, product_id)`.
* Stock withdrawal and line creation are coordinated by `Sale.AddItem`. The domain operation must preserve the sale and stock invariants.
* Immutability is supported by the absence of domain operations for editing or deleting registered sales.

**Source:** `spec/data-model.md`, Sections 2.3, 4, and 5.

## 5. Sale Item Rules

* A sale item must belong to a sale and reference a product.
* Its quantity must be greater than zero.
* The product name, unit price, and category name are captured at the time of the sale.
* The captured values must remain unchanged when the product catalog changes.
* The subtotal is calculated by multiplying the captured unit price by the quantity.
* A sale item cannot be created independently of its sale. Its creation is controlled by `Sale.AddItem`.

**Enforcement status:**

* The database enforces the required sale and product references through foreign keys and non-null constraints.
* The database cascades the deletion of sale items when their parent sale is deleted. The domain, however, does not provide a deletion operation for a registered sale.
* Positive quantity is currently enforced by the domain. Database enforcement is pending under T-20.
* Historical snapshots are created by the domain; the category name is also required by the database.

**Source:** `spec/data-model.md`, Sections 2.3, 2.4, and 5.

## 6. User Rules

* A username must be required and unique.
* Usernames must be normalized to lowercase and trimmed.
* A user must have a non-empty password hash.
* The only valid roles are `admin` and `seller`.
* The domain must never receive the plain-text password. A dedicated port handles password hashing.

**Enforcement status:**

* Username uniqueness is enforced by a database unique index.
* Lowercase and trimmed usernames are enforced by the domain. Database enforcement is pending under T-20.
* The domain checks that the password hash is non-empty; the database also requires the column to be non-null.
* Valid roles are checked by the domain. A database constraint for the allowed role values is pending under T-20.
* Keeping the plain-text password outside the domain is an architectural rule.

**Source:** `spec/data-model.md`, Sections 2.5, 4, and 7.

## 7. Reporting Rules

* A sales report groups information by product over a requested date range.
* The end of a date range cannot be earlier than its start.
* The report is calculated through a read port and is not stored as a separate entity or table.
* Reported product information must use the historical values captured in sale items, rather than assuming the current catalog values represent past sales.

**Source:** `spec/data-model.md`, Sections 1 and 6.

## 8. Rule Enforcement Summary

| Rule                                    | Current enforcement                             |
| --------------------------------------- | ----------------------------------------------- |
| Unique category name                    | Database                                        |
| Non-negative product stock              | Database check constraint                       |
| Positive product price                  | Domain; database check pending (T-20)           |
| Existing product category               | Database foreign key                            |
| No stock withdrawal beyond availability | Domain                                          |
| At least one item per sale              | Domain                                          |
| No duplicate product in a sale          | Domain and database unique index                |
| Positive sale item quantity             | Domain; database check pending (T-20)           |
| Historical sale item snapshots          | Domain; category name also required by database |
| Unique username                         | Database unique index                           |
| Normalized username                     | Domain; database enforcement pending (T-20)     |
| Allowed user roles                      | Domain; database enforcement pending (T-20)     |
| Valid report date range                 | Application layer                               |

Database constraints and domain rules have different responsibilities. A rule enforced only by the domain may be bypassed by direct database operations, so pending database constraints must not be described as already implemented.

## 9. Assumptions and Limitations

* The exact permissions for the `admin` and `seller` roles are not fully defined by the data model. A detailed authorization matrix remains an assumption.
* The model coordinates stock withdrawal and sale item creation through the domain, but this document does not claim that every possible failure scenario is protected by a database transaction unless that behavior is verified in the implementation.
* Rules marked as pending must be confirmed against the relevant implementation task before being considered complete.

## 10. Traceability

* Categories: `spec/data-model.md`, Sections 2.1 and 9.
* Products: `spec/data-model.md`, Sections 2.2, 3, 4, and 7.
* Sales: `spec/data-model.md`, Sections 2.3, 4, and 5.
* Sale items: `spec/data-model.md`, Sections 2.4 and 5.
* Users: `spec/data-model.md`, Sections 2.5, 4, and 7.
* Reports: `spec/data-model.md`, Section 6.
