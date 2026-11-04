<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Django
- Flask

## Decision Outcome

Chosen option: FastAPI 0.115, because FastAPI 0.115 hosts the visitor and admin APIs and streams OpenAI answers token-by-token so chat feels responsive, while the rich Python AI/document-parsing ecosystem runs the RAG and ingestion pipelines.

## Consequences

Async streaming and background work must be wired carefully, but FastAPI's native async makes token streaming and job enqueueing straightforward.
<!-- sdlc:auto:end adr -->