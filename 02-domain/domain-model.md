# Domain Model — Simple Stock Flow

## 1. Purpose

This document describes the main domain entities, their relationships, and the business rules of Simple Stock Flow.

The model is reconstructed from `spec/data-model.md`. Any behavior that cannot be confirmed from the data model is identified as an assumption.

**Source:** `spec/data-model.md`, Sections 1 and 2.

## 2. Domain Entities

The system has five main entities: `Category`, `Product`, `Sale`, `SaleItem`, and `User`. Each one corresponds to a table in the `sales` database schema.

### 2.1 Category

A category classifies products in the catalog.

* Each category has a unique identifier and a name.
* The name is required, cannot be empty, and is trimmed by the domain.
* Category names must be unique.
* The system uses a fixed set of five categories.
* Categories are seeded during the initial database migration.
* Category maintenance is not supported by the current domain model.

**Source:** `spec/data-model.md`, Sections 1, 2.1, and 9.

### 2.2 Product

A product represents an item available in the catalog.

Its domain data includes:

* Identifier.
* Name.
* Price.
* Available stock.
* Category.
* Optional image key.

Business rules:

* The product name is required and cannot be empty. The domain trims it before saving.
* The price must be greater than zero.
* Stock cannot be negative.
* A stock withdrawal must fail if the requested quantity exceeds the available stock.
* Every product must belong to an existing category.
* An absent image is represented by `NULL`, not an empty string.
* Products are not physically deleted; the system uses logical deletion.

The product is an aggregate root. Its operations, such as changing the price, withdrawing stock, and restocking, must preserve its invariants.

**Source:** `spec/data-model.md`, Sections 1, 2.2, 4, and 7.

### 2.3 Sale

A sale represents a completed commercial transaction.

Its data includes:

* Identifier.
* Date and time of the sale.
* The user responsible for registering it.
* The sale lines associated with the transaction.

Business rules:

* Every sale must identify the user who registered it.
* A sale must contain at least one line before it can be confirmed.
* The same product cannot appear more than once in the same sale.
* Stock withdrawal and adding a sale line must be handled as one operation.
* Once registered, a sale cannot be edited or deleted.
* The total is calculated from its line subtotals and is not stored as a database column.

`Sale` is an aggregate root and controls the creation of its sale lines.

**Source:** `spec/data-model.md`, Sections 1, 2.3, and 5.

### 2.4 SaleItem

A sale item represents a product included in a sale. It belongs to the `Sale` aggregate and cannot exist independently of its sale.

Its data includes:

* Identifier.
* Product identifier.
* Product name captured at the time of the sale.
* Quantity sold.
* Unit price captured at the time of the sale.
* Category name captured at the time of the sale.
* Identifier of the parent sale.

Business rules:

* Every sale item must reference a product.
* Quantity must be greater than zero.
* Product name, unit price, and category name are snapshots of the values at the time of the sale.
* The subtotal is calculated from the unit price and quantity; it is not stored.
* A sale item must belong to a sale.
* Its creation is controlled by `Sale.AddItem`.

The snapshots preserve historical information even if the product's name, price, or category changes later.

**Source:** `spec/data-model.md`, Sections 1, 2.3, 2.4, and 5.

### 2.5 User

A user is an internal operator who authenticates and registers sales.

Its data includes:

* Identifier.
* Username.
* Password hash.
* Role.

Business rules:

* The username is required and unique.
* Usernames are normalized to lowercase and trimmed by the domain.
* The password hash is required and cannot be empty.
* The valid roles are `admin` and `seller`.
* The domain must never receive or store the plain-text password; password hashing is handled through a dedicated port.

A user is an aggregate root.

**Source:** `spec/data-model.md`, Sections 1, 2.5, and 7.

## 3. Domain Relationships

| Relationship       | Cardinality | Business meaning                                                                  |
| ------------------ | ----------- | --------------------------------------------------------------------------------- |
| Category — Product | One-to-many | A product belongs to exactly one category. A category may have multiple products. |
| Sale — SaleItem    | One-to-many | A sale contains one or more lines. A line belongs to one sale.                    |
| Product — SaleItem | One-to-many | A product may appear in different sales. Each line references one product.        |
| User — Sale        | One-to-many | A user may register multiple sales. Each sale identifies its responsible user.    |

The relationship between `Sale` and `Product` is many-to-many from a business perspective, resolved through `SaleItem`, which also stores the quantity and historical product values.

**Source:** `spec/data-model.md`, Sections 2 and 5.

## 4. Value Objects and Calculated Values

The domain model also identifies value objects and calculated values that do not have their own database tables.

* **Money:** Represents a monetary amount. It rounds values to two decimal places using `MidpointRounding.AwayFromZero`. Product prices must be strictly positive, even though the `Money` value object itself permits zero.
* **Quantity:** Represents a quantity and rejects values that are not strictly positive.
* **SaleItem.Subtotal:** Calculated by multiplying the captured unit price by the quantity.
* **Sale.Total:** Calculated by summing the sale line subtotals.
* **Date range:** Used by the application layer to define the period of a sales report. Its end date cannot precede its start date.
* **Sales report:** A read model calculated for a date range. It is not persisted as a separate entity.

**Source:** `spec/data-model.md`, Sections 1, 2.2, 2.3, and 6.

## 5. Main Domain Invariants

The following rules must remain true during domain operations:

1. Product stock must never become negative.
2. A product price must be greater than zero.
3. A sale must have at least one line before confirmation.
4. A product cannot be repeated within the same sale.
5. Adding a sale line and withdrawing the corresponding stock must be handled atomically.
6. Sale lines preserve the product information that existed at the time of the transaction.
7. Registered sales cannot be edited or deleted.
8. Usernames must be unique, lowercase, and trimmed.
9. User roles are restricted to `admin` and `seller`.
10. Categories are fixed reference data and are not maintained through the domain's write operations.

The data model distinguishes between rules enforced by the database and rules enforced only by the domain. These responsibilities must not be treated as equivalent.

**Source:** `spec/data-model.md`, Sections 2 and 4.

## 6. Assumptions and Limitations

* The data model identifies `admin` and `seller` roles, but it does not fully specify the permissions assigned to each role. Any detailed authorization matrix remains an assumption until documented elsewhere.
* The data model describes the operations and invariants of sales, but it does not establish a separate event-publishing mechanism. No domain event infrastructure is assumed here.
* A sale report groups information over a date range, but the complete presentation and user interface of that report are not defined by the data model.

These points must be validated against additional requirements before being treated as confirmed system behavior.

## 7. Traceability

| Domain element                        | Source                          |
| ------------------------------------- | ------------------------------- |
| Domain terminology                    | `spec/data-model.md`, Section 1 |
| Entity definitions and invariants     | `spec/data-model.md`, Section 2 |
| Database structure                    | `spec/data-model.md`, Section 3 |
| Enforcement of business rules         | `spec/data-model.md`, Section 4 |
| Entity relationships and foreign keys | `spec/data-model.md`, Section 5 |
| Sales reporting                       | `spec/data-model.md`, Section 6 |
| Privacy and retention                 | `spec/data-model.md`, Section 7 |
| Category seed data                    | `spec/data-model.md`, Section 9 |
