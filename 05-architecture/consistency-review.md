# Architecture Consistency Review — Simple Stock Flow

## 1. Purpose

This document records the final consistency review of the system documentation.

The review checks whether the architecture, requirements, product documentation, domain model, and context documents remain consistent with `spec/data-model.md`.

## 2. Source of Truth

The authoritative source for this review is `spec/data-model.md`.

The architecture must reflect the entities, relationships, business rules, database constraints, and limitations documented in that specification.

## 3. Consistency Checklist

### 3.1 Domain Model

- The documentation identifies the five entities: `Category`, `Product`, `Sale`, `SaleItem`, and `User`.
- `SaleItem` belongs to a `Sale` and has no independent lifecycle.
- `Product`, `Sale`, and `User` are treated as aggregate roots.
- The system does not introduce a customer entity that is absent from the data model.

**Status:** Consistent with the documented model.

**Traceability:** `spec/data-model.md`, Sections 1 and 2.

### 3.2 Business Rules

- Product prices must be greater than zero.
- Product stock cannot become negative.
- Sale quantities must be greater than zero.
- A sale must contain at least one item.
- The same product cannot appear more than once in a sale.
- Sale registration and stock withdrawal must be handled atomically.
- Completed sales are immutable through the domain operations described by the model.
- Sale items preserve historical product information.

**Status:** These rules are documented. Their enforcement must be checked against the implementation status in the data model.

**Traceability:** `spec/data-model.md`, Sections 2, 4, and 11.

### 3.3 Persistence and Data Integrity

- The database is PostgreSQL 16.14.
- The database name is `simple_stock_flow`.
- The schema is `sales`.
- The five table names are `category`, `product`, `sale`, `sale_item`, and `user`.
- Database-enforced rules are distinguished from domain-only rules and pending work.
- Foreign key behavior follows the policy documented in the data model.

**Status:** The architecture overview documents these constraints. Pending database enforcement must not be described as already implemented.

**Traceability:** `spec/data-model.md`, Sections 0, 3, 4, 5, and 11.

### 3.4 Reporting

- Sales reports are grouped by product and filtered by a date range.
- The end date cannot precede the start date.
- Reports are calculated from stored sales information.
- Reports are not persisted as independent database entities.
- Historical sale-item values are used when preserving transaction history.

**Status:** Consistent with the documented reporting model.

**Traceability:** `spec/data-model.md`, Sections 1 and 6.

### 3.5 Security and Privacy

- User records store password hashes instead of plain-text passwords.
- The supported roles are `admin` and `seller`.
- Product images are represented by an optional external-storage key.
- Privacy, retention, and auditing must follow the rules specified in the data model.

**Status:** These constraints are documented. Authentication details and complete role permissions remain unspecified where the source model does not define them.

**Traceability:** `spec/data-model.md`, Sections 2, 7, and 8.

## 4. Open Decisions

The following implementation details must remain open until supported by an approved decision or another authoritative document:

- Deployment topology.
- API protocol and endpoint design.
- User interface technology.
- Exact application service interfaces.
- Transaction implementation and failure recovery.
- Complete permissions for each user role.
- Performance targets and availability objectives.

The layered architecture described in `overview.md` is a proposal, not proof that the application has already been implemented that way.

## 5. Final Assessment

The documentation establishes a consistent description of the domain, business rules, persistence constraints, and reporting behavior based on `spec/data-model.md`.

The main remaining responsibility is to preserve the distinction between confirmed requirements, assumptions, domain-only rules, and pending database enforcement.

Any future change must be reviewed against the authoritative data model before being accepted into the architecture documentation.