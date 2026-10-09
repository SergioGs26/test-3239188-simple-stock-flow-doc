# Domain Events — Simple Stock Flow

## 1. Purpose

This document identifies important business events in Simple Stock Flow and describes their meaning.

The data model defines domain entities and operations, but it does not confirm a separate domain event publishing mechanism. Therefore, the events listed below are proposed domain concepts, not confirmed implemented features.

**Main source:** `spec/data-model.md`, Sections 2, 5, and 8.

## 2. What Is a Domain Event?

A domain event represents something meaningful that has already happened in the business.

For example, registering a sale is a business fact. A domain event could communicate that fact to another part of the application.

The existence of a business operation does not mean that the system already publishes an event for it. Event publishing, event storage, and event delivery must not be assumed without implementation evidence.

## 3. Proposed Domain Events

### 3.1 ProductCreated

**Meaning:** A new product has been added to the catalog.

**Related entity:** `Product`.

**Business relevance:** Other application components may need to know when a product becomes available.

**Status:** Proposed. The data model describes products but does not confirm this event is published.

**Source:** `spec/data-model.md`, Section 2.2.

### 3.2 ProductStockChanged

**Meaning:** The available stock of a product has changed.

**Related entity:** `Product`.

**Business relevance:** Stock changes when products are restocked or quantities are withdrawn during a sale.

**Business rule:** Stock must never become negative, and a withdrawal must not exceed the available quantity.

**Status:** Proposed. Stock operations are described in the data model, but event publication is not confirmed.

**Source:** `spec/data-model.md`, Section 2.2.

### 3.3 SaleRegistered

**Meaning:** A sale has been registered with its sale items.

**Related entity:** `Sale`.

**Business relevance:** The application may use this fact to notify other components or initiate additional processing.

**Business rules:** A sale must have at least one item, must identify the responsible user, and cannot contain the same product more than once.

**Status:** Proposed. The data model defines sale registration rules but does not confirm an event publishing mechanism.

**Source:** `spec/data-model.md`, Sections 2.3 and 5.

### 3.4 UserRegistered

**Meaning:** A new internal user has been registered.

**Related entity:** `User`.

**Business relevance:** The application may need to react to the creation of an operator account.

**Business rules:** Usernames must be unique and normalized. The domain must not handle plain-text passwords.

**Status:** Proposed. User creation is a domain concept, but publication of this event is not confirmed.

**Source:** `spec/data-model.md`, Section 2.5.

## 4. Event Consistency and Stock Operations

The most important consistency requirement concerns registering a sale and withdrawing product stock.

The data model states that adding a sale item and withdrawing stock must be handled as one domain operation. If an event is introduced for this process, it must not indicate that a sale was successfully registered when the corresponding operation failed.

A future implementation should define when the event is created and how it is published consistently with the operation.

**Important:** This document does not claim that an event queue, event bus, outbox, or asynchronous delivery mechanism currently exists.

**Source:** `spec/data-model.md`, Sections 2.2, 2.3, and 8.

## 5. Persistence and Audit Limitations

The data model defines five business entities and does not define a separate table for domain events.

It also describes audit considerations, but that does not establish that domain events are persisted or that an event history is available to users.

Therefore:

* Domain events are not treated as additional database entities.
* No event table is proposed as part of the confirmed current schema.
* No event history or delivery guarantee is assumed.
* Any future event persistence mechanism requires a separate design decision.

**Source:** `spec/data-model.md`, Sections 2, 3, and 8.

## 6. Event Summary

| Proposed event        | Related entity | Status                              |
| --------------------- | -------------- | ----------------------------------- |
| `ProductCreated`      | `Product`      | Proposed, not confirmed implemented |
| `ProductStockChanged` | `Product`      | Proposed, not confirmed implemented |
| `SaleRegistered`      | `Sale`         | Proposed, not confirmed implemented |
| `UserRegistered`      | `User`         | Proposed, not confirmed implemented |

These event names describe possible ways to communicate business facts. They are not evidence that the current application publishes or stores events.

## 7. Traceability

* Product creation and stock rules: `spec/data-model.md`, Section 2.2.
* Sale registration and sale item rules: `spec/data-model.md`, Sections 2.3 and 2.4.
* User rules: `spec/data-model.md`, Section 2.5.
* Relationships and foreign key policies: `spec/data-model.md`, Section 5.
* Audit considerations: `spec/data-model.md`, Section 8.

## 8. Assumptions and Open Questions

* Whether the application needs to publish domain events remains an open design question.
* The consumers, delivery mechanism, and failure handling for these proposed events are not specified.
* Event persistence and historical event queries are not confirmed requirements.
* The proposed events must be reviewed against the actual application architecture before being treated as implementation requirements.
