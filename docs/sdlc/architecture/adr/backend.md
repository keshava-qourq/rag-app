<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Django 5
- Flask 3
- Express.js

## Decision Outcome

Chosen option: FastAPI 0.115, because The core work is receiving questions, running similarity search, calling OpenAI, and returning answers. Python has the richest ecosystem for document text extraction (PDF, DOCX) and embedding workflows. FastAPI provides async support and automatic OpenAPI documentation. FastAPI 0.115 is the approved version.

## Consequences

FastAPI BackgroundTasks are in-process and not durable — if the container restarts mid-indexing the document stays in 'processing' status. For pilot scale this is acceptable. A future migration to Celery or SQS would add durability.
<!-- sdlc:auto:end adr -->