<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-11-04

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- React 19
- Vue
- Svelte

## Decision Outcome

Chosen option: React 19, because React 19 builds both the responsive anonymous chat page and the authenticated admin dashboard from one codebase, rendering streamed answers, citations, the privacy notice and the consent-gated follow-up form.

## Consequences

A SPA needs its own auth-token handling for the admin area, but keeps one team and one build for both faces of the product.
<!-- sdlc:auto:end adr -->