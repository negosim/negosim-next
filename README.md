# NegoSim Next

NegoSim Next is the next-generation implementation of **NegoSim (Negotiation Simulator)**.

The original implementation is preserved at <https://github.com/negosim/negosim> and is used as a behavioral and historical reference during redevelopment. This repository is developed incrementally, with explicit requirements, documented design decisions, tests, and small Git commits.

## Project status

**Phase 0 — Legacy baseline and project setup** is in progress. The legacy implementation has been inspected from the available source archive; the initial findings are recorded in [`docs/00-project/LEGACY_BASELINE.md`](docs/00-project/LEGACY_BASELINE.md).

No new negotiation behavior has been implemented yet. Requirements and design decisions will be agreed before the new implementation begins.

## Development approach

1. Establish a baseline for the legacy implementation.
2. Define functional and non-functional requirements.
3. Formalize the domain model and terminology.
4. Document architecture and key decisions before implementation.
5. Implement in small, tested increments while comparing behavior with the legacy reference.

See [`docs/00-project/DEVELOPMENT_WORKFLOW.md`](docs/00-project/DEVELOPMENT_WORKFLOW.md) for the working agreements.

## Legacy reference

- Original repository: <https://github.com/negosim/negosim>
- New implementation: <https://github.com/negosim/negosim-next>

## License

The licensing and attribution approach for the new implementation must be confirmed against the original repository's license before source code is copied or adapted. See the legacy repository's `LICENSE.md`.
