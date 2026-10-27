# Non-Functional Requirements

**Status:** Updated draft for final review. Policy decisions are confirmed where stated; implementation-specific thresholds remain to be selected.

| ID | Quality attribute | Requirement / proposed target | Verification |
|---|---|---|---|
| NFR-001 | Correctness | Negotiation outcomes, timing boundaries, and agreement rules shall be specified and covered by automated tests. | Unit and integration tests for lifecycle boundaries. |
| NFR-002 | Reproducibility | Tests and examples shall be reproducible; stochastic components should allow a seed or controllable random source where practical. | Run tests repeatedly with fixed seeds and compare expected outcomes. |
| NFR-003 | Portability | Windows is the primary development environment; core library/tests must avoid Windows-only path assumptions. Linux compatibility should be tested where practical. | Run CI/tests on Windows and Linux if CI resources permit. |
| NFR-004 | Maintainability | Public contracts, module responsibilities, and material design decisions shall be documented. | Documentation review and API examples. |
| NFR-005 | Extensibility | New agents, strategies, models, protocols, and analytics subscribers should be implementable without modifying unrelated framework modules. Shared contracts should permit future multi-party support. | Extension examples and contract tests. |
| NFR-006 | Testability | Core logic shall be testable without GUI interaction or real-time waiting. | Unit tests use small deterministic domains and controllable time. |
| NFR-007 | Diagnostics | Invalid configuration and plugin-loading failures shall provide actionable error information. | Tests assert exception type/message context for invalid configurations. |
| NFR-008 | Compatibility | Important legacy capabilities and algorithms shall be mapped to new requirements/tests; strict import-level compatibility is not required. Intentional behavior changes must be documented. | Capability matrix and legacy-vs-new tests for representative scenarios. |
| NFR-009 | Packaging | The project shall declare supported Python versions and runtime/test dependencies in a standard machine-readable manifest. | Clean-environment installation and test run. |
| NFR-010 | Documentation | A new developer shall be able to install the project, run a sample negotiation, and run tests using documented steps. | Follow the instructions in a clean environment. |
| NFR-011 | API stability | Public API changes shall be intentional and documented; internal implementation details need not be treated as stable. | Review changelog/versioning policy when packaging is designed. |
| NFR-012 | Performance | Analytics observation must not impose unnecessary coupling or unbounded memory growth; establish representative benchmarks before numeric targets are set. | Benchmark event-heavy sessions and large experiment runs before optimization claims are made. |

## Analytics-specific quality requirements

- Event ordering and lifecycle points must be documented and tested.
- Analytics subscribers must be able to inspect documented model interfaces without relying on private implementation fields.
- Ground-truth preferences/utility must remain distinguishable from agent estimates and must not leak into the agent's ordinary decision interface.
- The framework should define and test what happens when an analyzer raises an exception, so analysis failures do not silently corrupt negotiation state or masquerade as negotiation outcomes.
- Data retention should be configurable or bounded for long tournaments; the exact policy is deferred to architecture/API design.

## Proposed release gates

A release should not be considered ready until:
- all required tests pass in the supported environment;
- examples execute from a clean installation;
- important legacy capabilities are mapped to requirements and tests, with any deferrals explicitly approved;
- public interfaces, Analytics event contracts, and lifecycle outcomes are documented;
- XML input works in the supported legacy cases;
- license and citation metadata are present;
- compatibility claims are backed by explicit tests rather than assumptions.

The exact Python version range, coverage target, performance thresholds, and release cadence remain to be set after dependency and implementation review.


## Confirmed project constraints

- The first deliverable is a Python library/API with tests and runnable examples; no GUI is planned for the initial release.
- Legacy algorithms and conceptual architecture should be preserved where feasible, while APIs may be redesigned and defects corrected.
- Legacy XML domain/preference input remains supported in the initial release.
- SOAP's default agreement rule is latest-bid equality and must be isolated behind an extensible contract.
