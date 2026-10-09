# Architecture Documentation

## 1. Purpose

This folder contains the architecture documentation for Simple Stock Flow. It explains the proposed system structure, the responsibilities of its main components, and the technical rules that must be preserved.

The documentation is based on `spec/data-model.md`. Details that are not established by the data model are identified as assumptions or open decisions.

## 2. Documents

| Document              | Purpose                                                                                                                                   |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `overview.md`         | Describes the architecture style, domain components, application responsibilities, persistence, reporting, and architectural constraints. |
| `decisions/README.md` | Will provide an index of the architecture decision records maintained in this folder.                                                     |

## 3. Main Architectural Responsibilities

The architecture separates the following responsibilities:

* **Domain:** Represents business entities, value objects, and business rules.
* **Application:** Coordinates use cases and business operations.
* **Persistence:** Stores and retrieves data using the PostgreSQL database.
* **Reporting:** Calculates sales information grouped by product and date range.

This is a proposed separation of responsibilities. The exact deployment, API design, user interface technology, and implementation details are not fully defined by the data model.

## 4. Rules That Must Be Preserved

The architecture must respect these rules:

* A sale is a completed and immutable commercial event.
* A sale item belongs to a sale and cannot exist independently.
* Stock withdrawal and sale registration must be handled atomically.
* Product stock cannot become negative.
* Product prices and sale quantities must respect their domain validations.
* Sale items preserve the product information captured at the time of sale.
* Sales totals and line subtotals are calculated values, not stored columns.
* Categories use predefined reference data.
* Passwords are stored as hashes, never as plain-text values.
* Sales reports are calculated from stored data rather than persisted as separate report records.

The documentation must distinguish database-enforced constraints from domain-only rules and pending work.

## 5. Source of Truth and Traceability

`spec/data-model.md` is the authoritative source for the current data model, including entities, invariants, physical schema, constraints, relationships, access patterns, privacy, auditing, and declared gaps.

Architecture changes must remain consistent with that document. Any additional technical choice must be documented as an assumption or supported by an approved decision record.

## 6. Open Decisions

The following topics remain open unless another approved document defines them:

* Deployment topology.
* API protocol and endpoint design.
* User interface technology.
* Exact application service interfaces.
* Transaction implementation and failure recovery.

These details must not be presented as implemented features without supporting evidence.

## 7. Review Checklist

Before accepting an architecture change, verify that:

1. It preserves the business rules in `spec/data-model.md`.
2. It respects the five entities and their relationships.
3. It distinguishes existing database constraints from pending enforcement.
4. It does not introduce unsupported entities or persisted calculated values.
5. Its assumptions and unresolved decisions are clearly documented.
