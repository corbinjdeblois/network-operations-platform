# Development workflow

Work from a small, verifiable outcome in the [roadmap](docs/roadmap.md). The product is a network operations platform; preserve that scope while implementing one dependency-complete step at a time.

## Before a session

1. Read [current status](docs/project-status.md), the relevant requirement, and the task's direct predecessors.
2. Inspect the actual working tree and existing changes. Do not overwrite unrelated work.
3. Choose a ready task and state the expected behavior, acceptance evidence, and operating environment.
4. Check the relevant architecture decision, data contract, and lab boundary before introducing a new dependency or interface.

The initial Git baseline contains documentation and repository configuration. T-002 continues with Python packaging, dependency locks, offline tests, and CI. Do not assume sample or planned commands in design documents are available executables.

## Complete one path

Implement the smallest complete behavior that satisfies the chosen task. Keep domain logic independent of external devices where practical, then verify the actual integration in the isolated lab. For reliability work, include the failure case that matters: incorrect input, failed network access, interrupted storage, or stale data.

Use short branches and focused commits. A useful commit title states the resulting behavior. Reference task and requirement IDs in commit bodies or pull requests, such as `T-007` and `FR-02`, without inventing issue numbers.

## Commit conventions

Use `type(scope): imperative summary`, omitting the scope when it adds no useful context. Typical types are `docs`, `feat`, `fix`, `test`, `build`, `ci`, and `chore`. Keep the subject concise, ideally within 72 characters.

Examples: `docs: establish network operations platform design baseline`, `feat(collector): capture a lab router configuration over SSH`, and `fix(storage): preserve the baseline after an interrupted capture`.

Keep each commit coherent and reviewable. Use a body for motivation, significant tradeoffs, task references, and verification when helpful. Describe the final change rather than the editing session; do not use vague subjects such as `updates` or claim checks that were not run.

## Review expectations

A change should explain the user-visible result, important tradeoffs, verification performed, and remaining limitations. Review input validation, secret handling, failure behavior, compatibility, and the operational implications of new services.

Keep source and documentation in sync. Update affected requirements, decisions, lab steps, runbooks, and task status. Add installation/usage commands to the README only after testing them from a clean environment.

## Public evidence rules

Commit fabricated fixtures and synthetic lab evidence only. Public CI gets no workplace credentials or configs. Scan candidate commits and inspect generated screenshots, logs, and metadata. Do not assume ignored files are safe to store in this OneDrive-synced checkout.

## End-of-session handoff

Update the roadmap with completed work and evidence. Update the current checkpoint with actual changes, checks run and their results, blockers, and the next ready task. A future developer or coding assistant should be able to resume without relying on chat history.

For a change in scope, keep IDs stable, record the decision, adjust dependencies, and re-estimate affected tasks. For a failed feasibility check, record the result and choose a documented fallback rather than claiming the original design is verified.
