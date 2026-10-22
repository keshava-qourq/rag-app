# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

App shell, upload form with validation, and MinIO file storage

Capacity: 20 points · Committed: 16

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Upload a document file through the web interface | 5 | backend-agent | — |
| US-002 | Story | Reject unsupported file formats during upload | 2 | backend-agent | US-001 |
| US-003 | Story | Store uploaded files in MinIO object storage | 5 | backend-agent | US-001 |
| US-013 | Story | Access the question-answering interface without login | 2 | frontend-agent | — |
| US-014 | Story | Access the admin document management interface without login | 2 | frontend-agent | — |

## Sprint 2: Sprint 2

Document library views, text extraction and chunking, OpenAI embeddings in pgvector, embedding removal capability

Capacity: 20 points · Committed: 20

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-004 | Story | List all documents in the library | 3 | frontend-agent | US-003 |
| US-005 | Story | View document details | 2 | frontend-agent | US-004 |
| US-007 | Story | Extract and chunk text from uploaded documents | 8 | backend-agent | US-003 |
| ↳ US-007-T1 | Task | Implement text extraction for PDF, DOCX, TXT, and Markdown files | — | backend-agent | — |
| ↳ US-007-T2 | Task | Implement text chunking and persist chunks to database | — | backend-agent | — |
| US-008 | Story | Generate and store embeddings in pgvector using OpenAI | 5 | backend-agent | US-007 |
| US-009 | Story | Remove embeddings when a document is deleted | 2 | backend-agent | US-008 |

## Sprint 3: Sprint 3

Document deletion, question-answering with citations, independent question handling

Capacity: 20 points · Committed: 17

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Delete a document from the library | 5 | backend-agent | US-004, US-009 |
| US-010 | Story | Ask a natural-language question and receive an AI-generated answer | 8 | backend-agent | US-008 |
| ↳ US-010-T1 | Task | Implement pgvector similarity search for question embedding | — | backend-agent | — |
| ↳ US-010-T2 | Task | Implement OpenAI answer generation with retrieved context | — | backend-agent | — |
| ↳ US-010-T3 | Task | Implement question input and answer display in React frontend | — | frontend-agent | — |
| US-011 | Story | View source document citations alongside an answer | 3 | frontend-agent | US-010 |
| US-012 | Story | Each question is independent with no conversation history | 1 | frontend-agent | US-010 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
