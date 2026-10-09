# Non-Functional Requirements — Simple Stock Flow

## 1. Purpose

This document describes the non-functional requirements derived from the system's data model. It focuses on data integrity, security-related constraints, concurrency, persistence, and consistency.

The source of truth is `spec/data-model.md`. Requirements that are not explicitly defined by the data model are marked as **Assumption** or **Not specified**.

## 2. Data Integrity

### NFR-01 — Prevent Negative Stock

The system must prevent product stock from becoming negative.

The database must enforce the `stock >= 0` constraint. The domain must also reject withdrawals greater than the available stock.

**Traceability:** `spec/data-model.md`, Section 2.2 and ADR-002.

### NFR-02 — Preserve Sale History

Once a sale has been registered, its historical information must remain unchanged through the domain operations defined by the model.

Sale items must preserve the product name, category name, and unit price recorded at the time of the transaction.

**Traceability:** `spec/data-model.md`, Sections 1, 2.3, 2.4, and ADR-004.

### NFR-03 — Maintain Transaction Consistency

Adding a sale item and withdrawing the corresponding product stock must be handled as one business operation.

The system must not leave the sale and stock in inconsistent states when an operation fails.

**Traceability:** `spec/data-model.md`, Section 2.3.

**Assumption:** The application will use an appropriate transaction mechanism to preserve database consistency when saving a sale.

### NFR-04 — Preserve Referential Integrity

Products must reference existing categories. Sale items must belong to a sale and reference their corresponding products according to the constraints defined in the data model.

The implementation must distinguish constraints enforced by PostgreSQL from rules enforced only by the domain.

**Traceability:** `spec/data-model.md`, Sections 2.1–2.4 and 5.

## 3. Security

### NFR-05 — Protect Passwords

The system must store password hashes instead of plain-text passwords. The domain must not receive or handle passwords in plain text.

**Traceability:** `spec/data-model.md`, Section 2.5 and D-09.

### NFR-06 — Validate User Roles

The system must recognize only the two roles defined by the model: `admin` and `seller`.

**Traceability:** `spec/data-model.md`, Sections 1 and 2.5.

**Not specified:** The complete authorization policy for each role and the authentication session mechanism are not defined by the data model.

## 4. Concurrency

### NFR-07 — Protect Stock Updates

Concurrent operations that modify product stock must preserve the non-negative stock invariant.

The implementation must follow the concurrency strategy documented in ADR-002.

**Traceability:** `spec/data-model.md`, Section 2.2 and ADR-002.

**Not specified:** A numerical limit for simultaneous users or transactions is not defined by the data model.

## 5. Data Storage and Consistency

### NFR-08 — Use the Defined Database

The system must use PostgreSQL 16.14 and the `simple_stock_flow` database, with the `sales` schema and the singular table names defined by the data model.

**Traceability:** `spec/data-model.md`, Sections 0 and 3.

### NFR-09 — Handle Product Images as References

The system must store an optional image key as a reference to external storage. The image binary itself must not be stored in the product record.

When no image is assigned, the value must be `NULL`, not an empty string.

**Traceability:** `spec/data-model.md`, Sections 1, 2.2, and D-08.

### NFR-10 — Calculate Sales Reports on Request

Sales reports must be calculated from the stored sales information for the requested date range. Reports must not be persisted as separate database entities.

**Traceability:** `spec/data-model.md`, Sections 1 and D-06.

## 6. Requirements Not Defined by the Data Model

The following aspects require additional decisions before measurable requirements can be established:

* Response-time targets and performance benchmarks.
* Availability and recovery objectives.
* Backup frequency and retention policies.
* Accessibility and supported user-interface standards.
* Maximum number of concurrent users.
* Detailed authorization rules for each role.

These items must not be treated as confirmed system requirements until they are agreed upon and documented.

## 7. Traceability Principle

Every requirement in this document must be traceable to `spec/data-model.md` or explicitly marked as an assumption or an unresolved decision.

The data model defines the data structure and its rules; it does not automatically define every operational, security, or performance requirement of the complete application.
