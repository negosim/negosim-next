# Functional Requirements

**Status:** Revised draft for final review  
**Project:** NegoSim Next  
**Source of evidence:** legacy source inspection and project redevelopment goal

This document specifies externally meaningful capabilities. It intentionally does not prescribe class names, packages, or a final architecture.

## 1. Negotiation domain and offers

| ID | Requirement | Evidence/status | Acceptance criterion |
|---|---|---|---|
| FR-001 | The framework shall represent a bid as an assignment of values to negotiation issues. | Observed | A bid can be constructed and its issue/value assignments inspected. |
| FR-002 | The framework shall represent an offer as a bid associated with a point in negotiation time. | Observed | An offer exposes its bid and associated time. |
| FR-003 | The framework shall represent a party's preferences over the negotiation domain and provide a way to evaluate bids. | Observed | A configured preference/utility model can evaluate a valid bid. Exact utility semantics are to be specified in Domain Modeling. |
| FR-004 | The framework shall support negotiation domains/preferences loaded from data files, including the legacy XML format. | Confirmed | A valid supported domain can be loaded with a clear success result; invalid input produces an actionable error. |
| FR-005 | The framework shall represent the progress/time of a negotiation and support a deadline. | Observed | A protocol can determine whether the deadline has been reached using one documented time contract. |

## 2. Parties and decision-making

| ID | Requirement | Evidence/status | Acceptance criterion |
|---|---|---|---|
| FR-010 | A negotiation shall involve at least two parties for the initial bilateral scope. | Observed | A session can be configured with two distinct parties. |
| FR-011 | A party shall be able to produce a bid or explicitly terminate its participation, subject to the agreed public contract. | Observed in legacy (`EndNegotiation`); proposed contract needs confirmation | Tests distinguish a normal bid from explicit termination without recording an invalid offer accidentally. |
| FR-012 | The framework shall support directly implemented agents that make their own negotiation decisions. | Observed | A minimal custom party can be integrated using documented extension points. |
| FR-013 | The framework shall support composable BOA-style agents using a bidding strategy, acceptance strategy, and opponent model. | Observed | A BOA agent can be configured from compatible components and participate in a session. |
| FR-014 | The framework shall support the EUBOA-style path with user model and elicitation components in addition to the BOA components. | Observed | An EUBOA agent can be configured with its required components and participate in a session. |
| FR-015 | The framework shall support random and time-dependent example agents as reference implementations. | Observed | At least one random agent and the Boulware/Conceder family or their agreed equivalents can be run in a test scenario. |

## 3. Protocol and negotiation lifecycle

| ID | Requirement | Evidence/status | Acceptance criterion |
|---|---|---|---|
| FR-020 | A protocol shall coordinate party turns and the order in which negotiation actions occur. | Observed | A deterministic test protocol executes turns in a documented order. |
| FR-021 | A protocol shall determine when an agreement is reached according to an explicit, documented agreement rule. | Observed; confirmed default | Tests cover agreement and non-agreement cases, including the legacy equal-bid case if approved. |
| FR-022 | A negotiation shall end with a clearly represented outcome, including agreement and termination without agreement. | Observed | The final result distinguishes at least agreement from no agreement; explicit termination/deadline reasons are considered in the open questions. |
| FR-023 | The framework shall retain an ordered offer history associated with the relevant parties. | Observed | Offers can be retrieved by party and in chronological insertion order. |
| FR-024 | A protocol shall enforce the configured deadline consistently. | Observed; legacy has duplicated checks | A boundary test proves behavior immediately before, at, and after the deadline. |
| FR-025 | A protocol shall make session events/results available to the analysis layer. | Observed | A test verifies that offer/result events reach the configured analysis component at documented points. |
| FR-026 | The framework shall permit alternative protocols to implement the agreed protocol contract. | Observed as an abstract protocol/plugin pattern | A minimal custom protocol can be registered/constructed and run without modifying negotiation-domain classes. |

## 4. Sessions and experiments

| ID | Requirement | Evidence/status | Acceptance criterion |
|---|---|---|---|
| FR-030 | The framework shall provide a session-level entry point that assembles the required domain, parties, protocol, time, and analysis components. | Observed | A configured session can be started through one documented public entry point. |
| FR-031 | The framework shall support repeated experiments across combinations of domains/preferences and agents. | Observed | An experiment definition can generate more than one session from a specified set of inputs. |
| FR-032 | The framework shall support generating combinations of BOA components. | Observed | Given component sets, the generator produces the expected Cartesian product without silently omitting combinations. |
| FR-033 | The framework shall support generating combinations of EUBOA components. | Observed | Given user, elicitation, bidding, opponent, and acceptance component sets, the generator produces the expected combinations. |
| FR-034 | Experiment execution shall make it possible to associate each result with the configuration that produced it. | Proposed | A result record identifies the agents/components, domain/preferences, protocol, and relevant run settings. |

## 5. Analysis, observability, and extensibility

| ID | Requirement | Evidence/status | Acceptance criterion |
|---|---|---|---|
| FR-040 | The framework shall collect session-level negotiation results and make them available as structured data. | Confirmed direction | A completed session exposes a documented result object with outcome and relevant run metadata. |
| FR-041 | The framework shall support aggregate analysis across tournament/experiment runs. | Legacy capability to preserve | A multi-session run can produce an aggregate result over completed sessions. |
| FR-042 | Users shall be able to add custom agents, strategies, models, protocols, and analyzers through documented extension contracts. | Confirmed | A custom component can be added without edits to unrelated core logic. |
| FR-043 | Component construction/discovery shall fail with a useful diagnostic when a component or constructor contract is invalid. | Proposed, motivated by legacy dynamic-loading risks | Invalid component name/path/constructor yields an actionable exception or validation message. |
| FR-044 | The framework shall provide runnable examples demonstrating a complete bilateral negotiation and a small experiment. | Confirmed direction | A clean environment can execute each documented example successfully. |
| FR-045 | Analytics subscribers shall be able to observe relevant negotiation events while a session is running, not only after completion. | Confirmed | A test analyzer receives documented events in deterministic order during a session. |
| FR-046 | The event stream shall include offer-related events (including offers made and responses/rejections where represented), timestamps or negotiation-time values, lifecycle/protocol events, and the final outcome. | Confirmed direction; exact event schema deferred to API design | Event sequence and payloads are documented and tested for a representative negotiation. |
| FR-047 | Analytics shall provide documented access to each agent's Utility Space and to its Opponent Model and, where applicable, User Model, subject to explicit access contracts. | Confirmed | A custom analyzer can inspect supported model interfaces during a run without relying on private implementation fields. |
| FR-048 | The framework shall make it possible to evaluate opponent/user-model predictions against ground-truth preferences when ground truth is available in the simulation. | Confirmed | An example analyzer compares model predictions with simulation-known preferences while ensuring the negotiating agent does not receive hidden ground truth through its normal decision API. |
| FR-049 | Analytics shall distinguish ground-truth preferences/utility from an agent's estimated opponent/user preferences and beliefs. | Confirmed | Public data structures and tests distinguish actual preference data from model estimates. |
| FR-050 | The event and analysis contracts shall not prevent a future multi-party protocol/party model, even though the first release implements bilateral negotiation only. | Confirmed architecture constraint | Domain/API design review identifies no unnecessary fixed two-party assumptions in shared contracts; multi-party behavior itself is not a first-release acceptance requirement. |

## 6. Confirmed scope boundaries for the initial release

The project owner has confirmed the following:

- **Confirmed:** deliver a Python library/framework with a documented API, tests, and runnable examples.
- **Confirmed:** do not rebuild the GUI in the initial release.
- **Confirmed:** preserve valuable algorithms and the conceptual component architecture, while allowing API redesign and bug fixes.
- **Confirmed:** retain XML domain/preference support in the initial release.
- **Confirmed:** use equality of the latest bids as the default SOAP agreement rule, behind an extensible agreement-rule contract.
- **Confirmed:** first release implements bilateral negotiation; contracts should permit future multi-party support without implementing it now.
- **Confirmed:** Analytics is an extensible API, not a GUI/report-only feature. It must support in-run event observation and access to supported utility/model interfaces.
- **Confirmed:** strict source/import compatibility is not required. Equivalent APIs must support equivalent tasks, and all important legacy capabilities must remain achievable in the new framework.

## 7. Compatibility policy

The redevelopment shall preserve the intent of valuable algorithms and the conceptual architecture (including the direct-agent, BOA, and EUBOA approaches) where feasible. The public API may be redesigned, and defects may be corrected. Any intentional behavior change that affects experiment results or callers must be recorded, justified, and covered by tests. Accidental legacy defects are not requirements merely because they exist in the old source.

The initial release shall continue to load the supported legacy XML domain/preference format. Format-specific parsing should not leak throughout the core domain model.

## 8. Initial release scope and legacy capability preservation

The initial release implements bilateral negotiation only, while the domain and extension contracts should avoid assumptions that would make future multi-party negotiation needlessly difficult. The first-release capability inventory must be completed by cross-checking the legacy implementation and developer reference; it must not be narrowed merely for implementation convenience. Any capability proposed for deferral must be identified explicitly, with rationale and owner approval.

The initial release does not include a rebuilt GUI, network/distributed execution, or a database-backed experiment service unless separately approved. Preserving accidental bugs solely for compatibility is out of scope.
