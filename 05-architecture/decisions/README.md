# Architecture Decision Records

## 1. Purpose

This folder documents important technical decisions that affect the architecture of Simple Stock Flow.

An Architecture Decision Record (ADR) explains a decision, its context, the alternatives considered, and its consequences.

## 2. Current Status

No architecture decision records have been added to this folder yet.

The current architecture overview is documented in `../overview.md`. The authoritative source for the data model and its constraints is `spec/data-model.md`.

Decisions referenced by the data model must not be treated as copied or approved architecture records in this folder unless their documents are available and their contents have been reviewed.

## 3. When to Create an ADR

Create an ADR when a technical decision significantly affects:

- System structure or architectural boundaries.
- Data persistence and transaction handling.
- Concurrency and data integrity.
- Security and privacy.
- Reporting and access to stored data.
- Dependencies or technologies that constrain future changes.

## 4. Recommended ADR Structure

Each decision record should include:

1. **Title:** A short description of the decision.
2. **Status:** Proposed, Accepted, Superseded, or Rejected.
3. **Context:** The problem that requires a decision.
4. **Decision:** The selected approach.
5. **Alternatives:** Other options considered.
6. **Consequences:** Benefits, limitations, and trade-offs.
7. **Traceability:** Links to relevant requirements and sections of `spec/data-model.md`.

## 5. Documentation Rules

- Use English for document names and technical content.
- Keep each ADR focused on one decision.
- Distinguish confirmed decisions from assumptions.
- Do not contradict the authoritative data model.
- Update or supersede an ADR when an approved decision changes.
- Do not describe a proposed decision as implemented without evidence.

## 6. Related Documentation

- `../overview.md` — Current architecture overview.
- `../README.md` — Architecture documentation guide.
- `spec/data-model.md` — Authoritative data model and constraints.

This index should be updated when new ADRs are added to this folder.