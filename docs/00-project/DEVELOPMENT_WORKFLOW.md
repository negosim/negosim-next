# Development Workflow

## Working agreement

NegoSim Next will be developed in small, reviewable stages. At the end of each stage, work pauses for the maintainer's review and approval before the next stage begins.

For each stage, the expected cycle is:

1. Inspect relevant legacy source and existing documentation.
2. State the problem and the engineering decision being considered.
3. Explain the rationale and alternatives where useful.
4. Document requirements, contracts, diagrams, or decisions before or alongside implementation.
5. Implement only the agreed scope.
6. Run relevant automated checks and record their results.
7. Compare behavior with the legacy reference where applicable.
8. Summarize changed files, known gaps, and exact Git commands.
9. Pause for maintainer review and approval.

## Git conventions

Use small commits that describe one coherent change. Suggested prefixes:

- `chore:` repository setup and maintenance
- `docs:` requirements, documentation, diagrams, and decisions
- `design:` interfaces and architecture decisions
- `feat:` new behavior
- `fix:` defect corrections
- `test:` tests and test infrastructure
- `refactor:` internal changes intended not to alter behavior

Do not commit generated caches, virtual environments, credentials, or local environment files.

## Branching

For this solo staged redevelopment, use `main` as the stable branch. Create short-lived topic branches when a stage is large enough to benefit from isolation; avoid introducing branch-management overhead for tiny documentation-only changes. Never force-push or rewrite shared history without an explicit reason.

## Testing principles

- Establish a reproducible environment before relying on test results.
- Keep unit tests fast and independent of GUI or external data when possible.
- Add integration tests for complete negotiation flows and protocol outcomes.
- Preserve a set of legacy behavior examples as characterization tests once expected outcomes are understood.
- A syntax compilation pass is not a substitute for tests.
- Report failed, skipped, and unrun tests separately; never describe an unrun test suite as passing.

## Scope control

Phase 0 records the baseline only. Requirements, domain modeling, architecture choices, and implementation are separate stages. A legacy defect is not silently reproduced or silently fixed: it is documented, then resolved through an explicit decision in the relevant stage.
