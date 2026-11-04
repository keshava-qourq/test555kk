# TEST555 — SDLC Documentation

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start overview -->
Chatbot

**Documentation version:** v2
<!-- sdlc:auto:end overview -->

## Contents

<!-- sdlc:auto:start contents -->
| Document | State |
|---|---|
| [Requirements](requirements/README.md) | v1 approved |
| [Architecture](architecture/README.md) | 19 entries |
| [Development](development/README.md) | 1 entry |
| [Testing](testing/README.md) | 1 entry |
| [Deployment](deployment/README.md) | 1 entry |
| [Maintenance](maintenance/README.md) | 1 entry |
| [Data model](architecture/data-model.md) | generated |
| [API (OpenAPI)](architecture/api/openapi.yaml) | generated |
| [Decisions (ADRs)](architecture/adr/README.md) | 4 decisions |
| [Sprint plan](development/sprint-plan.md) | not generated yet |
| [Developer guide](development/developer-guide.md) | not generated yet |
| [API reference](development/api-reference.md) | 0 routes |
| [Test report](testing/test-report.md) | not run yet |
| [Tests](testing/test-cases.md) | 0 tests |
| [Deployment guide](deployment/deployment-guide.md) | not deployed yet |
| [Runbook](deployment/runbook.md) | not deployed yet |
| [Release notes](deployment/release-notes.md) | 0 releases |
| [Traceability matrix](traceability-matrix.md) | 17 requirements |
| [Changelog](CHANGELOG.md) | 2 versions |
<!-- sdlc:auto:end contents -->

## Open gaps

<!-- sdlc:auto:start open-gaps -->
### Requirements

- AC-003: the embeddable website widget is assumed deferred beyond this MVP (only the standalone chat page ships) -- marked 'assumption to confirm' by the source and not yet confirmed.
- AC-016: email-sending credential (SMTP or SendGrid) and configurable staff inbox address are marked 'assumption to confirm'.
- AC-007: the specific vector store and LLM API credential (OPENAI_API_KEY) are marked 'assumption to confirm' pending engineering-team sign-off.
- AC-003: embeddable widget deferral is an unconfirmed assumption.
- AC-007: vector store choice and LLM API credential are an unconfirmed assumption.
- AC-016: email credential and configurable staff inbox are an unconfirmed assumption.
- AC-022: visitors-never-get-admin-access is an unconfirmed assumption.
- AC-024: PDF/Word/plain-text as the exhaustive supported-format list is an unconfirmed assumption.
- GDPR: only notice, consent, and deletion-on-request are specified. The requirements do not mention a data-subject access/export (portability) request process, a data-breach notification procedure, a documented lawful basis / record of processing, or a defined data-retention period -- confirm with the product owner whether these are in scope for a 'GDPR-level' standard.
- NFR-006 names 'an EU region' generically; no specific AWS region is designated -- needs architecture-phase decision.

### Architecture

- Input does not state who approved the architecture or on what date.
- No explicit mapping is given from architecture elements to requirement IDs (US-001..US-017); traceability below is therefore left unlinked.
- A 'GDPR-level privacy standard' is referenced but no formal regulatory/compliance skill or control checklist was supplied, so compliance coverage cannot be verified against a named regulation.
- No requirement IDs are linked to this component in the input.
- Retention period and backup/restore policy for this single data store are not specified.
- No data-processing/region commitment for the OpenAI SaaS dependency is stated, which may be relevant to the EU-region privacy standard.
- No data-processing/region commitment for the SendGrid SaaS dependency is stated.
- No requirement IDs are linked to this critical path in the input.
- No requirement IDs are linked to these entities in the input.
- No explicit data retention/deletion schedule for conversations or follow-up requests beyond the ad-hoc GDPR deletion endpoint is stated.
- No requirement IDs are linked to these endpoints in the input.
- Authorization scheme for admin endpoints beyond JWT issuance at login is not detailed (e.g. token expiry, refresh).
- No requirement IDs are linked to the deployment topology in the input.
- 'Static hosting / dev server' for web-app leaves the actual production hosting mechanism unspecified.
- No requirement IDs are linked to this decision in the input.
- 'GDPR-level privacy standard' and 'audit controls' are referenced as motivation but no specific regulation or audit-control checklist is named for verification.

### Development

- Development phase has not started -- no plan, tickets, or code exist yet to document.

### Testing

- Testing phase has not started -- no test plans or results exist yet to document.

### Deployment

- Deployment phase has not started -- no deployment has been recorded yet.

### Maintenance

- Maintenance phase has not started -- there is nothing post-deployment to document yet.
<!-- sdlc:auto:end open-gaps -->
