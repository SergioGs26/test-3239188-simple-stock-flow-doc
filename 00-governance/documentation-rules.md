# Documentation Rules

## Purpose

This document defines how documentation should be organized, written, and maintained in Simple Stock Flow.

## Language and Writing Style

- Project documentation must be written in English.
- Use clear, direct, and consistent technical language.
- Prefer short sections with descriptive headings.
- Explain technical terms when they may be unclear to the intended audience.
- Avoid unnecessary repetition and unsupported claims.

## File and Folder Naming

- Use lowercase names.
- Separate words in file names with hyphens.
- Use descriptive names that reflect the document's content.
- Store each document in the repository section that matches its purpose.

Examples:

- `domain-rules.md`
- `product-backlog.md`
- `consistency-review.md`

## Source of Truth

`spec/data-model.md` is the main source of truth for the data model used in this assessment.

When documenting model-derived information:

- Reference the relevant section, decision, or constraint.
- Preserve the meaning of the original rule.
- Do not introduce entities, fields, relationships, or behaviors that contradict the model.
- Label information that is not established by the model as an assumption or project agreement.

## Consistency Between Documents

Documents that describe related topics must use consistent names and concepts.

When a change affects one document, review the related context, product, domain, requirements, and architecture documents as applicable.

In particular, check that:

- Scope matches the documented features.
- User stories match the documented business rules.
- Architecture supports the requirements.
- Assumptions are not presented as confirmed facts.

## Structure and Formatting

- Use Markdown headings in a logical order.
- Use tables when they make information easier to compare.
- Use code formatting for file paths, identifiers, constraints, and commands.
- Use relative links for files inside the repository.
- Check that relative links point to existing files.

## Maintenance

Update documentation when an approved change affects its content.

Do not leave obsolete instructions or contradictory descriptions after a change.

## Scope

Do not create documentation for components, integrations, deployment environments, or processes as if they already exist unless project evidence supports that claim.