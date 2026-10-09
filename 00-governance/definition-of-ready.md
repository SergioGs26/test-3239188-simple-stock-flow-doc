# Definition of Ready

## Purpose

The Definition of Ready describes when a task has enough information to begin.

These criteria help prevent unclear requirements and unsupported design decisions.

## General Criteria

A task is ready when:

- Its objective is clear.
- Its expected result can be described.
- Its scope and affected areas are identified.
- Relevant dependencies are known or recorded.
- The available information is sufficient to begin the work.
- Important questions or missing information have been identified.

## Documentation Tasks

A documentation task is ready when:

- The document to create or update is identified.
- Its purpose and expected audience are clear.
- The relevant source documents are available.
- The required repository location is known.
- The expected level of detail is understood.
- Any applicable formatting or submission instructions are known.

## Model and Requirements Tasks

When a task depends on the data model:

- The relevant sections of `spec/data-model.md` have been identified.
- The entities, relationships, or constraints involved are understood.
- Any missing or ambiguous information is recorded.
- Assumptions are distinguished from confirmed requirements.

## Architecture Tasks

When a task affects architecture:

- The requirements being addressed are identified.
- Relevant domain rules and constraints are known.
- Dependencies on other components or decisions are recorded.
- Proposed solutions are not described as existing implementations without evidence.

## Handling Unclear Tasks

If essential information is missing, the task should be clarified before making decisions that could change the scope or contradict the data model.

Minor uncertainties may be documented as assumptions when the task can reasonably proceed without resolving them.