# Definition of Done

## Purpose

The Definition of Done establishes the minimum conditions required to consider a task complete in Simple Stock Flow.

The criteria must be applied according to the type and scope of the task.

## General Criteria

A task is complete when:

- Its objective has been fulfilled.
- The result matches the applicable requirements.
- No known contradiction with `spec/data-model.md` has been introduced.
- Related documents have been reviewed when necessary.
- The changes use clear and consistent terminology.
- The affected files are saved in the correct repository locations.
- The changes have been reviewed for spelling, formatting, and broken relative links.
- The Git changes have been committed using a descriptive message.
- The changes have been pushed to the required remote branch when remote delivery is required.

## Documentation Criteria

For a documentation task:

- The document explains its purpose and relevant content.
- Model-derived claims reference the appropriate section or constraint.
- Information not established by the model is labeled as an assumption or project agreement.
- The document does not describe unsupported features as implemented or confirmed.
- Related documents remain consistent after the change.

## Requirements and Domain Criteria

When a task affects requirements or domain rules:

- The behavior is consistent with the entities, relationships, and constraints in the data model.
- Business rules are not changed without an approved reason.
- Unresolved questions are identified instead of being presented as confirmed facts.

## Architecture Criteria

When a task affects architecture documentation:

- The proposed design supports the documented requirements.
- Decisions are identified as confirmed or proposed, as appropriate.
- The documentation does not claim that infrastructure, services, integrations, or deployments exist unless this is supported by project evidence.

## Verification

Completion must be checked against the criteria relevant to the task.

Automated tests, builds, deployments, or code reviews are required only when applicable to the actual scope of the task; this documentation assessment does not establish that such processes are already configured.