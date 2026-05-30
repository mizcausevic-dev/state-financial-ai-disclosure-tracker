# state-financial-ai-disclosure-tracker

> **State Financial Services AI Disclosure Tracker v0.1 draft.** A per-state lifecycle ledger of US state banking-regulator regulations, state-AG enforcement guidance, state legislative statutes applicable to financial services, and NMLS-coordinated state requirements. The Operator surface a bank, credit union, fintech, neobank, payment platform, or robo-advisor queries to know **which state's financial-services AI obligations govern an account-opening / credit-decision / payment-authorization on a given date**.

Part of the [Kinetic Gain Protocol Suite](https://suite.kineticgain.com).

> Status: v0.1 draft. Schema at [`schema/disclosure-event.schema.json`](./schema/disclosure-event.schema.json), per-state streams at [`states/`](./states/), Node verifier at [`src/verify.mjs`](./src/verify.mjs).

## Why this exists

Financial-services AI regulation lives at the intersection of: federal banking supervisors (OCC/FRB/FDIC), federal consumer protection (CFPB), federal market regulators (SEC/FINRA), federal AML (FinCEN), state banking regulators (NY DFS, CA DFPI, TX DOB, IL IDFPR, etc.), state-AG enforcement, and NMLS-coordinated requirements. A multi-state neobank operating in NY + CA + TX + IL + CO needs to know which state requires which AI obligation on which date. This tracker provides the per-state lifecycle ledger to answer that deterministically.

## Seed coverage (v0.1)

| State | Last lifecycle_state | Key citation |
| --- | --- | --- |
| **US-NY** | `amended` (effective 2023-11-01) | NY 23 NYCRR Part 500 — 2nd Amendment adds explicit AI-system-inventory + model-risk + external-audit requirements |
| **US-CA** (CCFPL) | `effective` (2020-09-25) | CA CCFPL (AB 1864) statute |
| **US-CA** (DFPI 10 CCR) | `effective` (2024-01-01) | CA DFPI 10 CCR 1060-1077 — CCFPL implementation rules covering BNPL + EWA + consumer finance |
| **US-CO** | `effective` (2026-02-01) | CO SB 24-205 — Colorado AI Act consequential-decision provisions covering credit + lending |
| **US-IL** | `effective` (2026-01-01) | IL HB 3773 / PA 104-0049 — IL AI in Consequential Decisions Act covering credit + lending |
| **US-TX** | `in-comment-period` | TX HB 1709 (TRAIGA) — Texas Responsible AI Governance Act |

**Multi-regulation convention**: when a state has multiple distinct regulations governing different aspects of financial-services AI (e.g. CA's CCFPL statute AND DFPI 10 CCR 1060-1077 implementation rules), each gets its own lifecycle stream file named `US-XX-<short-name>.ndjson`. The state machine validates each stream independently. The FinTech tracker is the first Suite tracker to encounter this multi-regulation pattern at scale — financial-services regulation is typically layered (statute + implementing regulation + supervisory guidance) in ways that the other verticals' regulatory landscapes are not.

## The shape

| Field | Purpose |
| --- | --- |
| `event_id`, `state`, `timestamp` | Append-only state-stamped identity |
| `lifecycle_state` | 9-state lifecycle |
| `regulatory_vehicle` | 6-vehicle taxonomy (state-banking-regulator-regulation / -guidance / state-AG-enforcement / state-legislative-statute / NMLS-coordinated / state-DOI-cross-applicable) |
| `citation` | Short label + long title + jurisdictional URI + session year |
| `effective_date` / `sunset_date` | REQUIRED when respective lifecycle_state |
| `scope` | 10-decision covered_decisions + 15-actor covered_actors + asset-size threshold + geographic scope |
| `obligation_kinds` | 16-doctrine obligation taxonomy including FinTech-specific obligations (ai-cybersecurity-policy-required, model-risk-management-framework-required, adverse-action-notice-state-overlay-required) |
| `regulator` | Primary agency + concurrent jurisdiction |
| `supersedes_event_id` | REQUIRED when amended / superseded |

## Quick start

```bash
npm install
npm run verify
```

Exit 0 on success, 1 on schema, 2 on illegal transition, 3 on broken supersedes, 4 on IO.

## Composes with

| Repo | Role |
| --- | --- |
| [`financial-decision-record-audit-stream`](https://github.com/mizcausevic-dev/financial-decision-record-audit-stream) | Per-decision audit events that must conform to whichever obligation_kinds were effective at the time |
| [`state-real-estate-ai-disclosure-tracker`](https://github.com/mizcausevic-dev/state-real-estate-ai-disclosure-tracker) | Sibling PropTech state tracker (mortgage-specific overlay) |
| [`state-insurance-ai-disclosure-tracker`](https://github.com/mizcausevic-dev/state-insurance-ai-disclosure-tracker) | Sibling InsurTech state tracker |
| [`state-employment-ai-disclosure-tracker`](https://github.com/mizcausevic-dev/state-employment-ai-disclosure-tracker) | Sibling HR Tech state tracker (NYC LL 144) |
| [`state-ai-disclosure-state-tracker`](https://github.com/mizcausevic-dev/state-ai-disclosure-state-tracker) | Sibling EdTech state tracker |
| [`fda-samd-classification-board`](https://github.com/mizcausevic-dev/fda-samd-classification-board) | Sibling HealthTech regulatory-lifecycle Operator |

## Compliance posture

FinTech-readiness scaffolding for state financial-services AI regulatory tracking. Does not establish compliance with any state regulation — local banking counsel + state banking-regulator engagement remain authoritative. Per the standing public-language guardrail: *readiness · evidence · posture · controls · scaffolding* — never "NY-DFS-compliant" or "CA-DFPI-attested" without an external attestation specific to the jurisdiction.

## License

MIT — see [`LICENSE`](./LICENSE).
