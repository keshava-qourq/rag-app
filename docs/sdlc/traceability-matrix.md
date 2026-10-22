# Traceability Matrix

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start legend -->
Built from recorded links, not written by a model: requirement key → plan item (`source_stable_key`) → development ticket (`stable_key`) → the ticket's test steps → the last live deployment that started after the work finished.
<!-- sdlc:auto:end legend -->

<!-- sdlc:auto:start matrix -->
| Requirement | Epic | Title | Plan items | Sprint | Design | Code | Commits | Ticket | Tests | Deployed |
|---|---|---|---|---|---|---|---|---|---|---|
| US-001 | EPIC-001 | Upload a document file through the web interface | US-001 | 1 | POST /api/documents/upload, backend_api, documents, frontend, object_storage | — | — | not started | not run | — |
| US-002 | EPIC-001 | Reject unsupported file formats during upload | US-002 | 1 | POST /api/documents/upload, backend_api, documents, frontend | — | — | not started | not run | — |
| US-003 | EPIC-001 | Store uploaded files in MinIO object storage | US-003 | 1 | POST /api/documents/upload, backend_api, database, documents, object_storage | — | — | not started | not run | — |
| US-004 | EPIC-002 | List all documents in the library | US-004 | 2 | GET /api/documents, backend_api, database, documents, frontend | — | — | not started | not run | — |
| US-005 | EPIC-002 | View document details | US-005 | 2 | GET /api/documents/{document_id}, backend_api, documents, frontend | — | — | not started | not run | — |
| US-006 | EPIC-002 | Delete a document from the library | US-006 | 3 | DELETE /api/documents/{document_id}, backend_api, database, document_chunks, documents, frontend, object_storage | — | — | not started | not run | — |
| US-007 | EPIC-003 | Extract and chunk text from uploaded documents | US-007, US-007-T1, US-007-T2 | 2 | POST /api/documents/upload, backend_api, database, document_chunks, documents, object_storage | — | — | not started | not run | — |
| US-008 | EPIC-003 | Generate and store embeddings in pgvector using OpenAI | US-008 | 2 | POST /api/documents/upload, ai_provider, backend_api, database, document_chunks | — | — | not started | not run | — |
| US-009 | EPIC-003 | Remove embeddings when a document is deleted | US-009 | 2 | DELETE /api/documents/{document_id}, backend_api, database, document_chunks | — | — | not started | not run | — |
| US-010 | EPIC-004 | Ask a natural-language question and receive an AI-generated answer | US-010, US-010-T1, US-010-T2, US-010-T3 | 3 | POST /api/questions/ask, ai_provider, backend_api, database, document_chunks, frontend | — | — | not started | not run | — |
| US-011 | EPIC-004 | View source document citations alongside an answer | US-011 | 3 | POST /api/questions/ask, backend_api, document_chunks, documents, frontend | — | — | not started | not run | — |
| US-012 | EPIC-004 | Each question is independent with no conversation history | US-012 | 3 | frontend | — | — | not started | not run | — |
| US-013 | EPIC-005 | Access the question-answering interface without login | US-013 | 1 | frontend | — | — | not started | not run | — |
| US-014 | EPIC-005 | Access the admin document management interface without login | US-014 | 1 | frontend | — | — | not started | not run | — |
<!-- sdlc:auto:end matrix -->
