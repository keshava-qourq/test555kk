<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- MongoDB

## Decision Outcome

Chosen option: PostgreSQL 16, because PostgreSQL 16 with pgvector holds documents, chunks and embeddings, conversation logs and follow-up requests in one store, so retrieval and GDPR single-point deletion share one backup-and-restore system.

## Consequences

pgvector search is adequate at a few thousand conversations/month; a dedicated vector DB would only matter at far larger scale, which this product does not target.
<!-- sdlc:auto:end adr -->