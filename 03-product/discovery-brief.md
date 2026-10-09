# Product Discovery Brief — Simple Stock Flow

## 1. Purpose

This document summarizes the current understanding of Simple Stock Flow, its intended users, the capabilities supported by the data model, and the questions that must be answered before implementation decisions are finalized.

The available source of truth is `spec/data-model.md`.

## 2. Product Overview

Simple Stock Flow is a proposed system for managing products, inventory, sales, internal users, and sales reports.

The data model defines product and category information, completed sales and their items, and internal user roles. It also establishes rules intended to maintain data consistency during sales and stock updates.

**Assumption:** The system is intended to simplify daily inventory and sales operations for a business. The exact business context and operational workflow have not been specified.

## 3. Target Users

The data model identifies two internal user roles:

- **Admin:** An internal role. Its exact permissions must be confirmed.
- **Seller:** An internal role. Its exact permissions must be confirmed.

The model does not define customer accounts or a customer entity. It also does not specify whether customers interact directly with the system.

## 4. Problems and Needs to Validate

The data model supports the following operational needs:

- Maintaining product information associated with valid categories.
- Preventing negative stock values.
- Recording completed sales and their associated items.
- Preserving product information and unit prices at the time of a sale.
- Keeping sale recording and stock withdrawal consistent.
- Consulting sales information and calculating reports by product over a date range.

**Not specified:** The source does not provide user interviews, measured business problems, market research, or evidence about how often these problems occur. These points must be investigated with stakeholders rather than treated as validated findings.

## 5. Current Product Capabilities

The data model supports planning for these capabilities:

1. Product management and category consultation.
2. Internal user records with `admin` and `seller` roles.
3. Sales recording with associated sale items.
4. Stock validation and consistent inventory updates.
5. Consultation of completed sales.
6. Sales reporting by product for a selected date range.

The exact screens, workflows, permissions, and report fields are not fully defined by the data model.

## 6. Business Rules to Preserve

- Every product must reference an existing category.
- Product stock must not be negative.
- A product price must be greater than zero according to the domain rule.
- A sale item must have a quantity greater than zero according to the domain rule.
- Sale items preserve the product name, unit price, and category name from the time of the sale.
- Sale recording and stock withdrawal must be treated as one business operation.
- Sale totals are calculated from their items rather than stored separately.
- Completed sales represent immutable commercial facts.
- Passwords must be stored as hashes, not as plain-text values.
- The initial five categories are seeded and treated as read-only.

Implementation details and database enforcement should be verified against the current state of the project.

## 7. Discovery Questions

### Users and Permissions

- Who will use the system in daily operations?
- What actions can an administrator perform?
- What actions can a seller perform?
- Can a user have more than one role?

### Product and Inventory Management

- Who can create, edit, or remove products?
- Is product deletion allowed when the product has already been sold?
- Are stock adjustments allowed outside the sales process?
- Is there a need to record the reason for a stock adjustment?

### Sales

- How should a seller select products and quantities?
- Can a completed sale be cancelled, refunded, or corrected?
- What information must be displayed on a sale receipt?
- How should simultaneous sales involving the same product be handled?

### Reporting

- Which sales indicators are required?
- Should reports include quantities, revenue, or both?
- Which date range options are needed?
- Who is allowed to access reports?

### Technical and Operational Needs

- What authentication and session-management mechanisms are required?
- What backup and recovery procedures are expected?
- What performance requirements should be established?
- What testing and release criteria must be met?

## 8. Risks and Unknowns

- Role permissions have not been fully defined.
- Product deletion and sale cancellation behavior remain unresolved.
- The exact report fields and filters have not been specified.
- Concurrent sales require careful validation to preserve stock consistency.
- Some domain rules may need corresponding database constraints.
- No confirmed usability research or stakeholder validation is included in the available data model.

## 9. Proposed Next Steps

1. Review the discovery questions with the project stakeholders.
2. Confirm the permissions for each internal role.
3. Define the product, inventory, and sales workflows.
4. Agree on the required reports and their fields.
5. Validate the acceptance criteria for the backlog items.
6. Update the requirements and roadmap based on confirmed decisions.

These steps are proposals and do not represent an agreed project schedule.

## 10. Traceability

This document is based on `spec/data-model.md`.

Statements directly supported by the data model are presented as current system rules or capabilities. Business context, user problems, interface behavior, and implementation decisions not defined by the source are identified as assumptions or open questions.