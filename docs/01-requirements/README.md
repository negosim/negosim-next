# Phase 1 — Requirements Engineering

**Status:** Updated draft for final review. Core product scope, compatibility policy, XML, Analytics, and attribution intent have been confirmed; final approval is still pending.

## Purpose

Define what NegoSim Next must do and the quality constraints it must satisfy before deciding its internal architecture or implementing its domain model.

## Evidence labels

- **Observed:** directly supported by the legacy source inspection.
- **Proposed:** a reasonable requirement for the new implementation, but it needs review.
- **Open:** cannot be settled safely without a product/design decision.

Do not interpret a legacy implementation detail as a requirement merely because it exists in the code. In particular, known defects and inconsistent APIs are not automatically behavior to preserve. The project owner has confirmed that valuable algorithms and conceptual architecture should be preserved, APIs may be redesigned, and defects should be corrected.

## Deliverables

1. `functional-requirements.md`
2. `non-functional-requirements.md`
3. `use-cases.md`
4. `glossary.md`
5. `open-questions.md`

## Review and approval

Review the open questions and proposed requirements with the project owner. Confirmed decisions now include: Python library/API with tests/examples and no initial GUI; bilateral execution first with future multi-party extensibility; preserve important legacy capabilities while redesigning APIs and fixing defects; latest-bid equality as the extensible default SOAP rule; retain XML; live, extensible Analytics with event and model access; and a citation/attribution approach compatible with the legacy license. Amend the documents as needed. The remaining questions are mainly domain/API implementation details and can be resolved in their designated phases. Domain Modeling starts only after the owner explicitly approves this updated Phase 1 package.

## Requirement IDs

IDs use `FR-###` for functional requirements and `NFR-###` for quality requirements. Keep IDs stable when wording changes; retire an ID rather than silently reusing it.
