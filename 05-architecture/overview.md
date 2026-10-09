# System Architecture Overview

## 1. Purpose

This document describes the system structure inferred from the data model in `spec/data-model.md`. It identifies the main domain components, their responsibilities, and the rules that the architecture must preserve.

**Source of truth:** `spec/data-model.md`.

## 2. Architecture Style

**Status: Assumption.** A layered architecture with domain logic separated from application coordination and persistence is proposed. The data model identifies domain entities, value objects, repositories, persistence adapters, and a read port for reports, but it does not fully specify the deployed architecture.

The proposed layers are:

* **Domain:** Contains business entities, value objects, and business rules.
* **Application:** Coordinates use cases and operations involving domain objects.
* **Persistence:** Maps domain data to the PostgreSQL database.
* **Read and reporting:** Retrieves aggregated sales information through a read port.

This separation helps keep business rules independent from database access.

## 3. Main Components

### 3.1 Domain

The domain contains the following entities:

* `Product`: Represents a catalog product, including its name, price, stock, category, and optional image key.
* `Sale`: Represents a completed commercial transaction and acts as an aggregate root.
* `SaleItem`: Represents a product line inside a sale. It belongs to the `Sale` aggregate.
* `User`: Represents an internal operator with the `admin` or `seller` role.
* `Category`: Represents a predefined product classification and is read-only.

The domain also includes value objects such as `Money` and `Quantity`. Business rules must be enforced according to the responsibility and enforcement status documented in `spec/data-model.md`.

**Traceability:** `spec/data-model.md` §§1–2.

### 3.2 Application

The application layer coordinates operations such as product management, sale registration, user-related operations, and sales reporting.

**Status: Assumption.** These use cases are inferred from the entities, invariants, and access patterns in the data model. The exact application services and their interfaces are not fully defined by the model.

### 3.3 Persistence

The persistence component stores data in PostgreSQL 16.14, using database `simple_stock_flow` and schema `sales`.

The schema contains five tables:

* `category`
* `product`
* `sale`
* `sale_item`
* `user`

The persistence mapping translates between domain classes using `PascalCase` and database identifiers using `snake_case`. Database tables remain singular.

**Traceability:** `spec/data-model.md` §§0 and 3.

### 3.4 Sales Reporting

Sales reports aggregate information by product over a date range. The report is calculated when requested through a read port; it is not stored as a separate database entity.

**Traceability:** `spec/data-model.md` §§1 and 6.

## 4. Architectural Rules

The architecture must preserve these rules:

1. `Product`, `Sale`, and `User` are aggregate roots.
2. `SaleItem` belongs to `Sale` and does not have an independent lifecycle.
3. Registering a sale line and withdrawing stock must be handled as one business operation.
4. A completed sale cannot be edited or deleted through the domain operations described by the model.
5. A product's stock cannot become negative. The database also enforces this through `ck_product_stock_non_negative`.
6. Product price must be greater than zero, but the model identifies this as a domain-only rule with database enforcement pending in task T-20.
7. Sale totals and line subtotals are calculated values, not stored columns.
8. The sale line preserves the product name and unit price from the time of the sale.
9. Passwords must not be exposed to the domain in plain text; the user record stores a password hash.
10. Categories are predefined reference data and do not have create, update, or delete operations through a category repository.

**Traceability:** `spec/data-model.md` §§1–2, 4, and 11.

## 5. Main Business Flow: Registering a Sale

The following flow is inferred from the domain rules. The exact application service and transaction implementation are not specified in this document.

1. An internal operator initiates a sale.
2. The application obtains the products involved in the sale.
3. Each requested line is validated against the domain rules.
4. The corresponding stock is withdrawn and the sale line is added as one business operation.
5. The sale must contain at least one line before it can be confirmed.
6. The completed sale is persisted with its sale lines.
7. The total is calculated from the line subtotals when required.

**Status: Assumption.** The sequence describes a proposed application flow based on the domain invariants; transaction boundaries and failure recovery must be defined by the implementation.

**Traceability:** `spec/data-model.md` §§1, 2.2–2.4, and 6.

## 6. Data Integrity and Persistence

The persistence layer must respect the database constraints and relationships documented in the model. It must not treat domain-only rules as if PostgreSQL already enforced them.

In particular:

* Stock non-negativity has a database constraint.
* The model identifies product price validation as pending database enforcement.
* Sale immutability is a domain rule based on the absence of edit and delete operations.
* Calculated totals must not be persisted as additional columns.
* The sale report must be generated from stored data rather than maintained as a separate persisted report.

**Traceability:** `spec/data-model.md` §§2–6 and 11.

## 7. Technical Constraints

| Concern             | Defined constraint                |
| ------------------- | --------------------------------- |
| Database engine     | PostgreSQL 16.14                  |
| Database schema     | `sales`                           |
| Database tables     | Five singular table names         |
| Database naming     | English `snake_case`              |
| Domain class naming | English `PascalCase`, singular    |
| Product stock       | Must remain non-negative          |
| Categories          | Five predefined seeded categories |
| Sales reports       | Calculated through a read port    |
| Password storage    | Password hash only                |

**Traceability:** `spec/data-model.md` §§0, 1, 2, 3, and 9.

## 8. Open Architectural Decisions

The data model does not fully define the following aspects:

* Application deployment topology.
* API protocol and endpoint design.
* User interface technology.
* Exact application service interfaces.
* Transaction implementation and recovery strategy.

These aspects should remain open until supported by an authoritative project decision. They must not be presented as existing features based only on the data model.

## 9. Summary

The proposed architecture separates business rules, application coordination, persistence, and reporting. Its main responsibility is to preserve the aggregate boundaries and data invariants defined by `spec/data-model.md`, while making the distinction between database-enforced rules, domain-only rules, and pending work explicit.
