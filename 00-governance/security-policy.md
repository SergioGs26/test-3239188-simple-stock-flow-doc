# Security Policy

## Purpose

This policy establishes general security principles for Simple Stock Flow.

The policy guides future implementation and documentation. It does not claim that security controls have already been implemented or verified.

## Data Protection

The application should protect information handled during product, inventory, user, and sales operations.

Access to information and actions should be limited according to the responsibilities assigned to each user role.

The data model defines the roles `admin` and `seller`. Their exact permissions must follow the documented requirements and approved authorization rules.

## Credentials

User credentials must be handled securely.

The user model stores `password_hash`, not a field intended to store plaintext passwords. The implementation should use an appropriate password-hashing algorithm and must never store or log plaintext passwords.

The exact algorithm and configuration are implementation decisions unless separately specified.

## Data Integrity

Security controls must support the integrity of business operations.

In particular:

- Product prices must be positive.
- Product stock must not become negative.
- Sale creation and stock withdrawal must preserve the required atomic behavior.
- Sale records must preserve the documented historical information.
- Logical deletion must follow the defined product behavior.

These rules must remain consistent with `spec/data-model.md`.

## Input and Database Protection

Inputs must be validated before they are processed or stored.

Database access must use safe query practices to reduce the risk of injection attacks.

Validation must not replace the integrity constraints required by the data model.

## Access Control

Actions that modify products, users, or sales should be authorized according to the approved role permissions.

The application must not rely only on hiding interface options to enforce authorization.

## Secrets and Configuration

Passwords, access tokens, database credentials, and other secrets must not be committed to the repository.

Configuration containing sensitive values should be managed separately from source code.

## Security Incidents

If a security issue is identified, it should be recorded, assessed, and addressed according to its impact.

Relevant documentation must be updated when an approved correction changes the system's security behavior.

## Traceability

Requirements derived from the data model must reference the appropriate sections or constraints.

Security mechanisms not defined by the model must be documented as assumptions or approved implementation decisions.