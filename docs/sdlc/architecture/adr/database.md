<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Proposed  
Date: 2026-10-22

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL 8
- SQLite
- DynamoDB

## Decision Outcome

Chosen option: PostgreSQL 16, because PostgreSQL with pgvector stores document metadata, chunk text, and vector embeddings in a single database, avoiding a separate vector service. It also handles the document library as plain relational data. PostgreSQL 16 is the approved version.

## Consequences

pgvector's IVFFlat index is approximate — results may not be perfectly ordered for very large datasets. For pilot scale this is not an issue. HNSW index is available if precision becomes critical later.
<!-- sdlc:auto:end adr -->