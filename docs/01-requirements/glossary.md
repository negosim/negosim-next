# Domain and Requirements Glossary

**Status:** Draft; terminology will be refined during Phase 2 (Domain Modeling).

| Term | Working definition | Notes |
|---|---|---|
| Negotiation | A process in which parties exchange proposals under a protocol until an outcome is reached. | Initial scope is bilateral. |
| Issue | A dimension/attribute to be negotiated, such as price or delivery date. | Exact domain representation is deferred. |
| Value | A possible value assigned to an issue. | Values may be discrete in the initial supported domain format. |
| Bid | An assignment of values to the negotiation issues. | Legacy code distinguishes Bid from Offer. |
| Offer | A bid presented at a particular negotiation time by a party. | Whether the proposer is stored directly on Offer is an open design question. |
| Party / Agent | An entity participating in a negotiation and producing decisions/actions. | Legacy terminology uses both words; preferred public terminology needs approval. |
| Preference | A representation of how desirable possible outcomes are to a party. | In the legacy implementation, Preference includes behavior beyond passive data storage. |
| Utility | A numeric evaluation of an outcome from a party's perspective. | Range and normalization rules must be specified in Phase 2. |
| Utility space | A model/function used to evaluate bids under a party's preferences. | Relationship to Preference needs formal modeling. |
| Protocol | Rules coordinating turns, offers, agreement detection, deadline and termination. | SOAP is one observed legacy protocol. |
| Negotiation state | The lifecycle status/outcome of a negotiation. | Legacy numeric values are 0, 1, -1; new named states are proposed, not yet approved. |
| Offer history / negotiation table | Recorded offers associated with participating parties. | Legacy class: `NegoTable`. |
| Timeline | A representation of negotiation progress and/or deadline. | Exact time units and boundary semantics are open. |
| Bidding strategy | A component that proposes a candidate bid. | BOA component. |
| Acceptance strategy | A component that decides whether an opponent offer should be accepted. | BOA component. |
| Opponent model | A component that estimates the opponent's preferences or behavior. | BOA component. |
| User model | A component representing user preferences under uncertainty. | EUBOA-related component. |
| Elicitation strategy | A component deciding when/how preference information should be requested or elicited. | EUBOA-related component. |
| Session | One configured negotiation execution. | Legacy: `BilateralSession`. |
| Experiment / Tournament | A collection of sessions generated and run for comparison. | Legacy has more than one generation of session/tournament classes. |
| Analysis | Collection and aggregation of negotiation results and measures. | Exact schema and output format are open. |
| Legacy compatibility | Preservation of explicitly selected behaviors/APIs from the old implementation. | Must be stated per behavior; not assumed globally. |
