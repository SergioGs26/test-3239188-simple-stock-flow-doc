# Security Rules

## Purpose

These rules describe technical practices that should be followed when implementing or modifying Simple Stock Flow.

They are implementation guidelines and must not be interpreted as evidence that the application already has these controls.

## Input Validation

- Validate required fields before processing them.
- Reject invalid values according to the applicable requirements.
- Product prices must be greater than zero.
- Stock quantities must not become negative.
- Usernames must follow the normalization and uniqueness rules defined by the data model.
- Validate identifiers before using them to retrieve or modify records.

Validation must be consistent with `spec/data-model.md`.

## Database Queries

- Use parameterized queries or an equivalent safe database access mechanism.
- Do not build database queries by directly concatenating untrusted input.
- Handle database errors without exposing sensitive internal information.
- Preserve the integrity of related records during database operations.

## Password Handling

- Never store plaintext passwords.
- Store password hashes in the `password_hash` field.
- Use an appropriate password-hashing algorithm.
- Do not include passwords or password hashes in application logs or error messages.
- Do not commit credentials or secrets to the repository.

## Authorization

- Enforce permissions on the server side.
- Check the authenticated user's permissions before allowing protected operations.
- Apply the approved permissions for the `admin` and `seller` roles.
- Do not assume that authentication alone grants permission to perform every action.

The exact permissions for each role must be defined by the applicable requirements or an approved project decision.

## Sales and Inventory Integrity

- A sale must contain at least one sale item.
- A product must not appear more than once in the same sale when the data model prohibits duplicate sale lines.
- Stock withdrawal and sale-line creation must preserve the required atomic behavior.
- A failed sale operation must not leave stock or sale records in an inconsistent state.
- Historical sale-item information must preserve the documented product name, unit price, and category name.
- Monetary calculations must follow the rounding rule defined in the data model.

Refer to `spec/data-model.md` for the authoritative business constraints.

## Error Handling and Logging

- Return clear errors without exposing passwords, secrets, or unnecessary internal details.
- Avoid logging sensitive authentication data.
- Record relevant failures when needed for troubleshooting, without exposing protected information.

## Configuration and Dependencies

- Keep secrets outside tracked source files.
- Review dependencies before introducing them.
- Document important security-related implementation decisions.
- Do not claim that a particular security mechanism is implemented until it has been verified.

## Verification

Security-related changes should be checked against the applicable requirements and business rules.

Where implementation and tests exist, verify input validation, authorization, database safety, and sale and stock consistency.

For documentation-only work, identify these items as implementation guidance rather than claiming that tests have passed.