# Product Roadmap — Simple Stock Flow

## 1. Purpose

This document presents a proposed development roadmap for Simple Stock Flow. It organizes the main capabilities into implementation stages based on the available data model.

**Note:** The stages and their order are proposals. No delivery dates, sprint durations, or release commitments have been specified.

## 2. Roadmap Overview

### Stage 1 — Data Foundations

**Goal:** Establish the basic data structures required by the system.

**Planned items:**
- Define and verify the product data structure.
- Load and consult the five initial product categories.
- Establish internal user records and the `admin` and `seller` roles.
- Verify the database relationships and constraints.

**Expected result:** The system has the basic data structures needed to manage products and internal users.

**Dependencies:** PostgreSQL 16.14 and the database structure defined in `spec/data-model.md`.

### Stage 2 — Product and Inventory Management

**Goal:** Support product consultation and maintain valid stock values.

**Planned items:**
- Consult the product catalog.
- Associate products with existing categories.
- Validate product prices according to the domain rules.
- Prevent negative stock values.
- Define and verify product management permissions.

**Expected result:** Product information and stock values can be managed consistently.

**Dependencies:** Stage 1 and the product and category data structures.

### Stage 3 — Sales Processing

**Goal:** Record completed sales while preserving inventory consistency.

**Planned items:**
- Record sales and their associated sale items.
- Validate product availability and requested quantities.
- Calculate sale totals from their items.
- Preserve product names, unit prices, and category names at the time of the sale.
- Process sale recording and stock withdrawal as one business operation.
- Test transaction consistency and concurrent sales scenarios.

**Expected result:** Completed sales are recorded with the corresponding inventory changes.

**Dependencies:** Stages 1 and 2, plus verified sales and inventory rules.

### Stage 4 — Sales History and Reporting

**Goal:** Allow authorized internal users to review recorded sales information.

**Planned items:**
- Consult completed sales.
- Define the required sales history filters.
- Generate sales reports by product for a selected date range.
- Verify report calculations against recorded sales.
- Confirm which report fields are required.

**Expected result:** Users can consult historical sales information and calculated sales summaries.

**Dependencies:** Stage 3 and confirmed reporting requirements.

### Stage 5 — Validation and Release Preparation

**Goal:** Verify that the implemented features satisfy the agreed requirements.

**Planned items:**
- Test product and category relationships.
- Test stock validation and sales transactions.
- Verify that sale items preserve historical product information.
- Test user permissions and password-hash storage.
- Validate report calculations.
- Document any remaining limitations or unresolved decisions.

**Expected result:** The team has evidence that the implemented capabilities satisfy the validated acceptance criteria.

**Assumption:** The project will require a formal release-validation stage. The exact testing strategy and release process have not been specified in the data model.

## 3. Proposed Prioritization

The proposed order is:

1. Data foundations.
2. Product and inventory management.
3. Sales processing and stock integrity.
4. Sales history and reporting.
5. Validation and release preparation.

Sales processing and stock integrity receive high priority because the data model defines the relationship between completed sales and inventory withdrawals as one business operation.

## 4. Dependencies and Risks

- Incorrect product or category relationships could affect sales operations.
- Insufficient stock validation could produce inconsistent inventory.
- Failure to preserve historical product information could affect the accuracy of sales records.
- Concurrent sales may require additional transaction and concurrency testing.
- Unconfirmed permissions could delay the definition of user access.
- Reporting requirements must be clarified before the report scope can be finalized.

## 5. Planning Limitations

The available data model does not specify:

- Delivery dates or deadlines.
- Sprint lengths or team assignments.
- Budget or resource estimates.
- A confirmed release schedule.
- The complete user interface or technology implementation details.
- The final acceptance and deployment process.

These details must be agreed upon by the project team before this roadmap becomes a committed delivery plan.

## 6. Traceability

This roadmap is based on `spec/data-model.md`. It organizes the documented entities, relationships, and business rules into proposed development stages.

The roadmap does not introduce confirmed requirements beyond those supported by the data model. Any additional planning decisions remain proposals until validated by the project stakeholders.