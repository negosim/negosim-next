# NegoSim Next

NegoSim Next is the next-generation implementation of **NegoSim (Negotiation Simulator)**.

The original implementation is preserved at <https://github.com/negosim/negosim> and is used as a behavioral and historical reference during redevelopment. This repository is developed incrementally, with explicit requirements, documented design decisions, tests, and small Git commits.

## Project status

**Phase 1 — Requirements Engineering**. Requirements are being documented and reviewed before domain modeling, architecture decisions, or implementation.

## Development approach

1. Establish a baseline for the legacy implementation.
2. Define and review functional and non-functional requirements.
3. Formalize the domain model and terminology.
4. Document architecture and key decisions before implementation.
5. Implement in small, tested increments while comparing behavior with the legacy reference.

See [`docs/00-project/DEVELOPMENT_WORKFLOW.md`](docs/00-project/DEVELOPMENT_WORKFLOW.md) for the working agreements and [`docs/01-requirements/`](docs/01-requirements/) for requirements.

## Legacy reference

- Original repository: <https://github.com/negosim/negosim>
- New implementation: <https://github.com/negosim/negosim-next>

## License

The supplied legacy source archive includes a GNU GPL version 3 license. If code is reused or adapted, the new distribution must respect that license. The current plan is to use GPL-3.0-only for a code-derived rebuild and include `CITATION.cff` for scientific citation; verify provenance and final license files before release.
