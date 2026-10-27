# Open Questions — Requirements Review

**Status:** Updated draft — principal product decisions confirmed; implementation details remain open for their designated phases.

These questions distinguish product-level decisions from details that should be decided during domain modeling, architecture, or API design. Recommendations are not treated as approved decisions unless explicitly labeled.

## A. Product scope and users

### Q-01. What is the first target deliverable?
- **Decision: APPROVED.** A Python library/framework with documented API, automated tests, and runnable examples. No GUI in the initial release.

### Q-02. What negotiation scope is implemented first?
- **Decision: APPROVED.** The first release implements bilateral negotiation only. Shared domain and extension contracts should leave a reasonable path to future multi-party negotiation; multi-party execution itself is not a first-release requirement.

### Q-03. Who are the primary users?
- **Working assumption:** researchers running experiments and developers implementing agents, strategies, models, protocols, and analyzers. This assumption can be validated through examples and documentation without blocking domain modeling.

### Q-04. What does compatibility with the old project mean?
- **Decision: APPROVED.** Preserve important legacy capabilities, algorithms, and conceptual architecture, while redesigning the public API and fixing defects. Strict class/import compatibility is not required; equivalent tasks must remain possible through suitable new APIs. Intentional behavior changes must be documented and tested.

### Q-05. Which legacy capabilities must be present in the first release?
- **Decision: PRINCIPLE APPROVED; INVENTORY REQUIRED.** The baseline is to preserve all important capabilities of the original NegoSim. Before final release planning, cross-check the legacy source and developer reference, map each capability to a requirement and test, and explicitly justify any proposed deferral. Do not narrow the list merely for implementation convenience.

## B. Protocol semantics

### Q-06. How should agreement be defined in SOAP?
- **Decision: APPROVED.** Equality of the latest bids is the default agreement rule, behind an extensible agreement-rule contract.
- **Deferred detail:** exact comparison semantics for issue ordering, missing issues, and value types will be specified during Domain Modeling and Protocol design.

### Q-07. What should a session result distinguish?
- **Recommended starting contract:** agreement, deadline reached, party termination, and execution error should be distinguishable. Confirm exact enum/status names during Domain Modeling/API design.

### Q-08. How should time and deadlines be represented?
- **Deferred to Domain Modeling:** define normalized negotiation progress, deadline boundaries, and whether wall-clock limits are also needed. Avoid duplicated or inconsistent deadline checks.

## C. Domain and data formats

### Q-09. Which input formats are required?
- **Decision: APPROVED.** Preserve XML domain/preference input in the initial release.
- **Open but non-blocking:** whether JSON input is also included in the first release; decide after the XML loader and domain model are specified.

### Q-10. What utility range and numeric contract should be guaranteed?
- **Deferred to Domain Modeling:** inspect legacy utility-space implementations and strategy assumptions before choosing normalization, discounting, reservation-value, and comparison semantics.

## D. Analytics, experiments, and reproducibility

### Q-11. What must Analytics expose?
- **Decision: APPROVED.** Analytics must be extensible and able to observe events during a running negotiation, including offer exchange/rejection where represented, timing, protocol/lifecycle changes, and final outcomes. It must provide documented access to each agent's Utility Space and supported Opponent Model and User Model interfaces, so researchers can evaluate predictions against simulation-known ground truth when available.
- **Design constraints:** distinguish ground truth from model estimates; do not expose hidden ground truth through the agent's normal decision API; define event schema, ordering, subscription/lifecycle, and model-access contracts during architecture/API design.

### Q-12. Which experiment output formats are required?
- **Recommended default:** structured Python results first; CSV/JSON exporters can be added as explicit capabilities. Decide exact formats after reviewing legacy outputs and researcher needs. This does not block domain modeling.

### Q-13. Is deterministic reproducibility required for randomized agents?
- **Recommended direction:** yes, support seeded or injectable random sources where practical. Specify the public API during architecture/API design.

### Q-14. What happens when a session fails during a tournament?
- **Recommended direction:** record the failure with context and continue other independent sessions when safe, with a fail-fast option. Specify after the experiment/session lifecycle is designed.

## E. Engineering and release

### Q-15. Which Python versions and operating systems should be supported?
- **Decision on policy:** Windows is the primary development environment. Select the newest stable Python version that is supported by the project's required dependencies at implementation setup time. Add Linux test coverage where practical to prevent platform-specific path/runtime assumptions. Record the exact minimum/maximum Python versions once dependency compatibility is checked.

### Q-16. Which license and citation policy should be used?
- **Decision on intent: APPROVED.** The project should permit broad collaboration and reuse; redistributions must retain copyright/license notices, and scientific users should be given clear citation metadata for the NegoSim paper.
- **Important compatibility finding:** the supplied legacy source archive contains a GNU GPL version 3 license. If NegoSim Next reuses or adapts copyrightable legacy code, the new distribution must respect that license and compatibility constraints. Therefore do not switch to MIT merely by preference. **Recommended default for a code-derived rebuild:** GPL-3.0-only, with a `LICENSE` file and `CITATION.cff`; verify source provenance and authorship before release. If the implementation is genuinely independent, review licensing separately before changing this recommendation.
- A citation file encourages correct scientific attribution; the license itself should not be represented as requiring a paper citation unless its actual terms say so.

### Q-17. What is the release gate?
- **Working default:** installable package; end-to-end bilateral negotiation; preserved/ported important legacy capabilities; usable XML loading; extensible in-run Analytics; examples; automated tests; public API documentation; license and citation metadata. The detailed capability-to-test matrix must be reviewed before release.

## Approval record

Confirmed by project owner:
- Python library/API, tests, runnable examples, no initial GUI.
- Bilateral-only execution in the first release, with future multi-party extensibility as a design constraint.
- Preserve important legacy capabilities and algorithms; redesign APIs and fix bugs rather than requiring strict import compatibility.
- SOAP latest-bid equality as default behind an extensible rule.
- XML support in the initial release.
- Analytics observes in-run events and exposes documented Utility Space, Opponent Model, and User Model access for research evaluation, with ground truth separated from agent-visible estimates.
- Citation/attribution intent: retain copyright/license notices on redistribution and provide a citation path for scientific use.
- Windows as primary development environment; choose Python version after checking dependencies, with Linux compatibility tests where practical.

Deferred to later phases: exact event payloads and ordering; precise time/deadline and utility semantics; exact experiment export formats; random-seed API; tournament failure policy; exact Python version range; detailed legacy capability-to-test inventory. These are not reasons to block Domain Modeling unless new evidence shows a product-level conflict.

Phase 1 still requires the project owner's explicit final approval after reviewing the updated documents.
