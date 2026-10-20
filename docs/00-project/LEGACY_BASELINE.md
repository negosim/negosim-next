# Legacy Baseline — NegoSim

- **Status:** Initial inspection; Phase 0
- **Reference repository:** <https://github.com/negosim/negosim>
- **Inspection input:** `negosim.zip` supplied for this redevelopment
- **Inspection date:** 2026-10-20

## 1. Purpose

This document records the observed starting point of the legacy NegoSim implementation. It is not a specification of ideal behavior and does not imply that every observed behavior is correct. Requirements and intended behavior will be clarified in Phase 1.

## 2. Scope observed

The supplied archive contains 282 archive entries and 117 Python source files. It includes application code, test modules, XML utility/domain data, a GUI, and user documentation. It also contains generated Python bytecode/cache files and IDE metadata; these are not appropriate source artifacts to carry into the new implementation.

The root README is minimal and points users to the user manual. No `requirements.txt`, `pyproject.toml`, `setup.py`, `Pipfile`, or `environment.yml` was found at the inspected root/depth.

## 3. Legacy subsystem map

| Area | Observed responsibility |
|---|---|
| `core/` | Bid/Offer, preferences and bid space, timeline and state, party abstractions, negotiation table, sessions, tournaments, and component contracts |
| `protocols/` | Negotiation orchestration, including `SOAP` and `SOAPUncertainty` |
| `Agents/`, `Agents_uncertainty/` | Concrete parties/agents, including time-dependent, random, BOA, and EUBOA variants |
| `bidding_strategies/`, `acceptance_strategies/`, `opponent_models/` | Reusable negotiation components |
| `user_models/`, `users/`, `elicitation_strategies/` | User preference/modeling and elicitation-related components |
| `utility_spaces/`, `Domains/` | Utility-space implementations and XML preference/domain data |
| `analysis/`, `AnalysisTournament/` | Session/tournament analysis and output |
| `Run/` | Session and tournament entry points |
| `GUI/`, `controller.py`, `Model.py`, `configurations.py` | GUI, configuration, and application coordination |
| `test/` | `unittest` test modules for selected classes and workflows |

This is a high-level inventory, not a final architecture decision.

## 4. Initial behavior worth preserving as a reference

The following behaviors appear central to the framework and must be explicitly reviewed before implementation changes are made:

1. A negotiation party proposes a `Bid`; an `Offer` associates a bid with a time.
2. The shared negotiation table records offers per party.
3. A protocol coordinates turns, asks parties to act, checks agreement/deadline/termination, and updates analysis.
4. The legacy `SOAP` protocol cycles through parties and recognizes agreement when the latest bids have equal issue-item mappings.
5. Party behavior can be implemented directly or assembled from reusable BOA components: bidding strategy, acceptance strategy, and opponent model.
6. The EUBOA path adds user-model and elicitation components.
7. Session and tournament layers construct and run negotiations; tournament helpers generate component combinations.
8. Utility spaces/preferences and XML domain data determine bid evaluation and search.

These are observations to validate in requirements engineering, not promises that every edge case or defect will be preserved.

## 5. Baseline checks performed

The supplied archive was extracted to a temporary inspection directory. The checks below were run against that extracted snapshot using Python 3.13 in the inspection environment.

| Check | Result | Notes |
|---|---|---|
| Python syntax compilation | PASS | `python -m compileall -q .` exited successfully. This checks syntax/bytecode compilation, not runtime correctness. |
| Default unittest discovery | NOT USEFUL | `python -m unittest discover -s test -v` discovered 0 tests because the test filenames use an uppercase `Test...` prefix, which does not match the default lowercase `test*.py` pattern on this environment. |
| Explicit unittest discovery | FAIL | `python -m unittest discover -s test -p 'Test*.py' -v` ran 25 tests: 10 passed and 15 errored. |

### Observed test failures

- Many integration tests fail before negotiation begins because path construction includes Windows-style backslashes, e.g. a path like `Domains\\laptop/laptop_buyer_utility.xml`, which is not resolved correctly on Linux.
- At least one `NegoTable` test constructs the object using an argument order/signature that does not match the current constructor contract.
- At least one `TimeLine` test calls the constructor without the required `deadline_type` argument.
- `TestBOA` also errors during setup due to the same domain-file path issue.

The test results are a baseline for this inspection environment, not a claim that the original project fails identically on its historical development machine. Runtime dependencies and the original supported Python version have not yet been established.

## 6. Known design/consistency risks to validate

Prior source inspection identified these likely issues; each must be rechecked against the current source and clarified before being changed:

- Inconsistent naming, including `core/AbstractProtocl.py` versus the `AbstractProtocol` class name.
- Protocol code calls `NegoTable.get_parties()` in some places even though the inspected table API exposes `get_party_ids()`.
- Party IDs and party objects are mixed in some APIs and comparisons; some IDs are compared using `is` / `is not`.
- `SOAPUncertainty` appears to use table methods and party arguments inconsistently with the current interfaces.
- `StateInfo` and `NegoTable` overlap in the representation of negotiation state/history.
- Deadline handling and agreement checks may be duplicated across the protocol flow.
- Some filesystem paths are platform-specific.
- Similar/parallel implementations exist, including `BilateralSession` / `BilateralSession2`, `BilateralTournament` / `BilateralTournament2`, and multiple EUBOA agent implementations.
- The legacy project has no discovered dependency manifest, making reproducible setup uncertain.

These are investigation items, not automatic authorization to refactor them in Phase 0.

## 7. Phase 0 constraints

- Do not modify the legacy repository as part of creating the new implementation.
- Do not copy the legacy code wholesale into `negosim-next` before the license and reuse approach is confirmed.
- Do not implement new negotiation behavior in Phase 0.
- Keep observed legacy behavior, intended behavior, defects, and proposed changes clearly distinguished in documentation.
- Record material design choices as Architecture Decision Records (ADRs) once those decisions are reached.

## 8. Exit criteria

Phase 0 is complete when:

- the new repository has a minimal project README and `.gitignore`;
- this baseline and the development workflow are committed;
- the initial syntax/test status is recorded;
- the user confirms the baseline before Phase 1 begins.
