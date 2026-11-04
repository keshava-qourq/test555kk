<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- Google Cloud
- Azure

## Decision Outcome

Chosen option: AWS, because AWS runs the FastAPI service, PostgreSQL and the S3 document bucket in one EU-pinned account, giving the data-residency and audit controls the GDPR-level privacy promise needs without self-run servers.

## Consequences

Single-cloud coupling, accepted here for the managed EU-region storage, encryption and operational simplicity the MVP needs.
<!-- sdlc:auto:end adr -->