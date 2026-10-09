# Product Vision — Simple Stock Flow

## 1. Purpose

This document defines the product vision for Simple Stock Flow. It describes the intended value of the system based on the available data model. Business goals that have not been confirmed are marked as assumptions.

## 2. Vision Statement

**Assumption:** Simple Stock Flow aims to provide a simple and consistent way for internal operators to manage products, record sales, maintain stock integrity, and consult sales information.

The system is intended to keep the product catalog and sales records consistent by applying the business rules defined in the data model.

## 3. Target Users

The data model identifies two types of internal operators:

* **Admin:** An operator with the `admin` role.
* **Seller:** An operator with the `seller` role.

Both roles belong to the internal operation of the system. The model does not define the complete permissions for each role.

**Traceability:** `spec/data-model.md`, Sections 1 and 2.5.

## 4. Product Objectives

The following objectives are based on the capabilities and rules described in the data model:

1. Maintain a product catalog with names, prices, stock quantities, categories, and optional image references.
2. Prevent product stock from becoming negative.
3. Register sales with their corresponding sale items and stock withdrawals.
4. Preserve historical sale information, including product names, category names, and unit prices.
5. Support sales reports aggregated by product over a selected date range.
6. Authenticate internal operators using stored password hashes.

**Traceability:** `spec/data-model.md`, Sections 1, 2, and 6.

## 5. Expected Value

**Assumption:** The system may help internal operators reduce inconsistencies in product and sales records, maintain reliable stock information, and review sales activity more easily.

These expected benefits must be validated with the intended users. The data model defines system rules and structures, but it does not provide measured evidence of business improvements.

## 6. Product Scope

### Included in the defined model

* Product management.
* Read-only product categories.
* Internal operator accounts and roles.
* Sale registration and sale items.
* Stock withdrawals associated with sales.
* Sales reports calculated over a date range.
* Optional product image references.

### Not established by the data model

The following capabilities are not confirmed by the data model and require separate decisions:

* Customer accounts or customer management.
* Supplier and purchasing management.
* Barcode scanning.
* External payment processing.
* Detailed role-based permissions.
* Inventory forecasting.
* Mobile applications or third-party integrations.

The absence of these elements from the data model does not automatically mean they are permanently excluded from the product.

## 7. Product Principles

The product should preserve these principles:

* **Data integrity:** Business operations must respect the documented stock and sales rules.
* **Historical consistency:** A registered sale preserves the values recorded at the time of the transaction.
* **Clear responsibilities:** Domain rules, application coordination, and database constraints must have distinct responsibilities.
* **Traceability:** Product documentation must remain consistent with `spec/data-model.md`.
* **Transparency:** Unconfirmed decisions must remain identified as assumptions.

**Traceability:** `spec/data-model.md`, Sections 1–6.

## 8. Success Measures

**Assumption:** The team may evaluate the product using measures such as:

* Successful registration of valid sales.
* Rejection of sales that exceed available stock.
* Consistency between sale items and stock changes.
* Correct calculation of reports for selected date ranges.
* Successful authentication of valid internal operators.

Specific targets, test procedures, and acceptance thresholds must be agreed upon separately.

## 9. Open Questions

* What business context and user needs should guide the product priorities?
* What exact permissions should each internal role have?
* Which measures will be used to evaluate the product's success?
* Which additional capabilities, if any, should be considered for future versions?

## 10. Traceability Principle

The data model is the source of truth for the entities, constraints, and business rules described in this document.

Statements about user needs, business benefits, and future capabilities remain assumptions until validated or formally documented.
