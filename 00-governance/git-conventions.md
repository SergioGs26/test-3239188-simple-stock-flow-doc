# Git Conventions

## Purpose

These conventions help keep the Simple Stock Flow repository organized and make changes easier to review.

These practices are proposed project agreements unless they are explicitly required by the instructor or repository instructions.

## Branch Naming

Use descriptive branch names with a short prefix:

- `docs/` for documentation changes.
- `feature/` for new functionality.
- `fix/` for corrections.
- `refactor/` for internal improvements.

Examples:

- `docs/update-domain-rules`
- `feature/product-registration`
- `fix/stock-validation`

For a documentation-only assessment, work on the branch required by the instructor. Do not create additional branches if the assessment requires using `main`.

## Commit Messages

Use a short, descriptive message following this format:

`type(scope): description`

Common types:

- `docs`: documentation changes.
- `feat`: new functionality.
- `fix`: corrections.
- `refactor`: internal restructuring.
- `test`: test-related changes.

Examples:

- `docs(governance): add git conventions`
- `docs(domain): clarify sale rules`
- `fix(requirements): correct category behavior`

## Commit Guidelines

- Keep each commit focused on one logical change.
- Use clear descriptions that explain what changed.
- Avoid messages such as `update`, `changes`, or `final`.
- Review the files included in a commit before committing.
- Do not commit passwords, access tokens, or other secrets.

## Collaboration and Review

When collaboration or pull requests are used, changes should be reviewed before integration.

For this individual documentation assessment, follow the instructor's required Git workflow and repository instructions.

## Traceability

Changes affecting requirements, business rules, or architecture must remain consistent with `spec/data-model.md`.

If a change introduces a decision not defined by the data model, document it as an assumption or project agreement.