# Problem Framing — Simple Stock Flow

## 1. Purpose

This document describes the problem that Simple Stock Flow aims to address, based on the available data model. Statements that are not directly supported by the model are identified as assumptions.

## 2. Problem Statement

**Assumption:** A business that manages products and sales needs a consistent way to maintain its product catalog, track available stock, register sales, and review sales results.

Without a system that applies the same business rules across these activities, product quantities and sales records may become inconsistent. This is a potential problem to validate with the intended users, not a confirmed finding.

**Related data model:** `spec/data-model.md`, Sections 1 and 2.

## 3. Evidence from the Data Model

The data model identifies several business rules that the system must preserve:

* Product stock must never become negative.
* A sale must contain at least one line before it can be confirmed.
* A sale line and its corresponding stock withdrawal must be handled as one business operation.
* Completed sales are immutable through the defined domain operations.
* Sale lines preserve historical product names, category names, and unit prices.
* Sales reports are calculated by product over a selected date range.

These rules show which consistency and reporting needs the system is designed to address. They do not prove how frequently these problems occur in a real business.

**Traceability:** `spec/data-model.md`, Sections 1, 2.2–2.4, and 6.

## 4. Target Users

The model defines two internal operator roles:

* **Admin:** An internal operator with the `admin` role.
* **Seller:** An internal operator with the `seller` role.

**Not specified:** The exact permissions of each role and the characteristics of the intended business have not been fully defined by the data model.

**Traceability:** `spec/data-model.md`, Sections 1 and 2.5.

## 5. Desired Outcome

**Assumption:** Simple Stock Flow aims to support more consistent product and sales management by applying the defined business rules, preserving historical sale information, and providing sales reports for selected date ranges.

The system must preserve the constraints documented in the data model regardless of the interface used to access the business operations.

**Traceability:** `spec/data-model.md`, Sections 1–2 and 6.

## 6. Open Questions

The following points require validation before the problem can be considered fully confirmed:

* How are products and sales currently managed?
* What difficulties do internal operators experience when tracking stock?
* Which sales indicators do administrators need to review?
* What permissions should be assigned to the `admin` and `seller` roles?
* How will the team measure whether the system improves the current process?

## 7. Traceability Principle

The data model is the source of truth for the entities, business rules, and data constraints described in this document.

Business context that is not defined by the model remains an assumption until validated with the intended users or documented in an authoritative project source.
