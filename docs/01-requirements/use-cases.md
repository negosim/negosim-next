# Principal Use Cases

**Status:** Draft for review

These use cases describe user goals without committing to specific classes or module structure.

## UC-01 — Run one bilateral negotiation

**Primary actor:** Researcher/developer  
**Goal:** Run two parties in a specified domain under a selected protocol and deadline.

**Preconditions**
- A supported domain/preference configuration is available.
- Two valid parties and a protocol are configured.
- The run settings are valid.

**Main flow**
1. The user defines the domain/preferences and selects two parties.
2. The user selects a protocol and deadline.
3. The framework validates the configuration.
4. The session starts and the protocol coordinates party turns.
5. Offers are recorded and the protocol checks agreement and termination conditions.
6. The framework returns a structured outcome and makes analysis data available.

**Alternative flows**
- Invalid configuration: execution does not start and an actionable diagnostic is returned.
- Deadline reached: the session ends without agreement, unless the approved protocol semantics specify otherwise.
- Party explicitly terminates: the session records the agreed termination outcome.
- A party/component raises an error: the framework reports the failure with context; it must not silently mislabel it as a normal negotiation outcome.

**Postconditions**
- A session outcome and relevant offer history are available, including failure/termination information as agreed.

## UC-02 — Add a custom agent

**Primary actor:** Agent developer  
**Goal:** Implement a new negotiation party.

**Main flow**
1. The developer reads the public party contract and a minimal example.
2. The developer implements the required behavior.
3. Contract tests verify the implementation.
4. The agent is configured in a session.
5. The session runs without changes to unrelated core modules.

**Acceptance check:** A small custom agent can participate in an example negotiation using only documented extension points.

## UC-03 — Compose a BOA agent

**Primary actor:** Researcher  
**Goal:** combine reusable bidding, acceptance, and opponent-model components.

**Main flow**
1. Select compatible component implementations.
2. Configure a BOA-style party.
3. Validate the configuration.
4. Run the party in a negotiation.
5. Observe the result and component/analysis outputs.

**Acceptance check:** the framework rejects missing/incompatible components clearly and runs a valid composition.

## UC-04 — Run an EUBOA-style agent

**Primary actor:** Researcher  
**Goal:** run an agent that also models user preferences and uses elicitation.

**Main flow**
1. Select user model and elicitation components.
2. Select bidding, acceptance, and opponent-model components.
3. Configure the agent and its user/elicitation context.
4. Run a session and capture relevant analysis.

**Open point:** whether user interaction must be supported in the first non-GUI release, or whether only non-interactive/default elicitation is required initially.

## UC-05 — Run a tournament/experiment

**Primary actor:** Researcher  
**Goal:** compare agents or component combinations over multiple configurations.

**Main flow**
1. Specify domains/preferences, candidate agents/components, protocols, and run settings.
2. Ask the framework to generate the requested session combinations.
3. Execute the sessions.
4. Aggregate outcomes and analysis.
5. Export or access structured results.

**Acceptance check:** the run's configuration and results can be traced to each generated session.

## UC-06 — Implement an alternative protocol

**Primary actor:** Framework developer  
**Goal:** add a new turn-taking/agreement/deadline policy.

**Main flow**
1. Read the protocol contract.
2. Implement the lifecycle behavior.
3. Run protocol contract tests.
4. Configure a session to use the protocol.
5. Verify lifecycle outcomes and analysis events.

**Acceptance check:** adding the protocol does not require modifying bid/preference classes.

## UC-07 — Reproduce a legacy scenario

**Primary actor:** Researcher / maintainer  
**Goal:** verify that a selected behavior from the old framework remains consistent in the new framework.

**Main flow**
1. Choose a representative legacy scenario.
2. Record its input configuration and relevant expected behavior.
3. Run or characterize the scenario against the legacy implementation where feasible.
4. Add a deterministic or tolerance-based test for the intended behavior.
5. Run the equivalent scenario in NegoSim Next.
6. document any intentional differences.

**Acceptance check:** each compatibility claim identifies the scenario and its test; known defects are not silently treated as required behavior.


## UC-08 — Analyze a negotiation while it is running

**Primary actor:** Researcher / analytics developer  
**Goal:** Observe negotiation events and inspect supported agent-model interfaces during execution.

**Main flow**
1. The researcher implements or configures an analyzer using the public Analytics contract.
2. The analyzer subscribes to relevant session events before the run starts.
3. During execution, it receives ordered events such as offers, responses/rejections where represented, timing/protocol changes, and termination/outcome events.
4. Where permitted by the public contract, the analyzer reads the parties' Utility Space and their Opponent Model/User Model interfaces.
5. The analyzer compares model predictions with simulation-known ground truth where available and records custom metrics.
6. The framework makes the analyzer's outputs available independently of built-in reporting.

**Constraints**
- Actual preferences/utility and an agent's estimates must be distinguishable.
- Ground truth exposed for evaluation must not be injected into the agent's normal decision interface.
- An analyzer must use public contracts, not private class fields.

**Acceptance check:** a runnable example analyzer observes a complete bilateral session and evaluates at least one model prediction against known simulation data.
