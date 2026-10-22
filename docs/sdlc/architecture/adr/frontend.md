<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- React 19
- Next.js 15
- Vue 3
- Svelte 5

## Decision Outcome

Chosen option: React 19, because The app is a straightforward Q&A interface with an admin upload area and a search box. There is no server-side rendering, SEO, or complex routing need, so React's lean SPA model fits perfectly. React 19 is the approved version.

## Consequences

No SSR means initial load requires client-side JS. No built-in routing so a lightweight router or conditional rendering is used. These are acceptable for an internal pilot tool.
<!-- sdlc:auto:end adr -->