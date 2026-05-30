# Changelog

## [0.1] — 2026-05-29

### Added

- Initial schema with 9-state lifecycle + 6-vehicle regulatory_vehicle.
- 10-decision covered_decisions + 15-actor covered_actors + 16-doctrine obligation_kinds (including FinTech-specific ai-cybersecurity-policy-required + model-risk-management-framework-required + adverse-action-notice-state-overlay-required + data-residency-restriction).
- Conditional schema requirements (effective_date / sunset_date / supersedes_event_id).
- Node verifier — schema (1) + state-machine (2) + supersedes-reference (3) gates.
- Seed: US-NY (Part 500 + 2nd Amendment), US-CA (CCFPL + DFPI 10 CCR 1060-1077), US-CO (SB 24-205 effective), US-IL (HB 3773 effective), US-TX (HB 1709 in-comment-period).
- CI workflow.

### Not yet

- Additional states (MA, WA, NJ, MN, GA + more) added per PR.
- NMLS-coordinated-requirement seed entries.
- State-DOI-cross-applicable seed entries for insurance + banking overlap.
- Effective-date-as-of query helper.
