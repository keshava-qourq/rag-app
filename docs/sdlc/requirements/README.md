# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-STACK-001 · approved · source: requirements:approved technology stack

Approved technology stack and runtime services for the RAG app pilot. Selected by the product owner.

- Backend: FastAPI 0.115 (Python) — chosen for ecosystem strength in PDF/DOCX text extraction and embedding workflows.
- Frontend: React 19 — lean client-side Q&A and admin UI; no SSR/SEO needs.
- Cloud: AWS — managed PostgreSQL (RDS), container hosting, path to auth/logging/autoscaling.
- Database: PostgreSQL 16 with pgvector — single store for metadata, chunk text, and vector embeddings.
- Vector store: pgvector inside PostgreSQL (no separate vector service).
- AI provider: OpenAI — embeddings + GPT-4-class answer generation; requires OPENAI_API_KEY.
- File storage: MinIO (S3-compatible), container bitnamilegacy/minio:2024.10.13 — stores original uploaded documents durably.
- Required API key: OPENAI_API_KEY.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

### REQ-SCOPE-001 · approved · source: requirements:delivery scope & capabilities

In-scope capabilities and explicitly out-of-scope items for the lightweight pilot (requirements version 1).

In scope:
- Admin document upload (PDF, DOCX, TXT, Markdown)
- Document library management (list, view, delete)
- Document embedding and indexing for semantic search
- Natural-language question answering with source citations
- OpenAI models for embeddings and answer generation
- pgvector in PostgreSQL for vector storage/retrieval
- MinIO for storing uploaded files
- No login required — anyone with the URL can access the app

Out of scope (do not design/build):
- Integration with external document systems (Google Drive, SharePoint, Confluence, Notion)
- Native mobile app (web-only)

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

### EPIC-001 · draft · source: requirements:EPIC-001

Admin document upload. Admins upload company documents (PDF, DOCX, TXT, Markdown) through a web interface; files are stored in MinIO. No login/RBAC at launch — admin interface is accessible to anyone with the URL.

Stories:
- US-001 [critical] Upload a document file through the web interface (AC-001: upload form accepts file selection and submission)
- US-002 [high] Reject unsupported file formats during upload (AC-002: unsupported formats like .xlsx/.pptx rejected with message; AC-003: empty or oversized files rejected with message)
- US-003 [critical] Store uploaded files in MinIO (AC-004: successful upload persists to MinIO and shows confirmation; AC-005: document appears in library with filename and upload timestamp)

Assumptions: Only an admin role manages documents, but there is no login/RBAC at launch; admin uploads common formats.

**Related:** US-001, US-002, US-003

### EPIC-002 · draft · source: requirements:EPIC-002

Document library management. Admins can list, view, and delete documents. Deletion removes the file from MinIO and its embeddings from pgvector.

Stories:
- US-004 [high] List all documents (AC-006: library shows filename, upload date, file format; AC-007: empty state message when no documents exist)
- US-005 [medium] View document details (AC-008: detail view shows filename, file format, upload timestamp, file size)
- US-006 [high] Delete a document (AC-009: confirmation prompt on delete; AC-010: on confirmation, file removed from MinIO, embeddings removed from pgvector, document removed from library)

Assumptions: No RBAC at launch; no usage analytics or audit logging, so deletions are not tracked.

**Related:** US-004, US-005, US-006

### EPIC-003 · draft · source: requirements:EPIC-003

Document embedding and indexing. On upload the system extracts text, chunks it, generates OpenAI embeddings, and stores them in pgvector. On deletion, embeddings are removed.

Stories:
- US-007 [critical] Extract and chunk text (AC-011: after upload to MinIO, system extracts text and splits into chunks; AC-012: corrupted/no-text files flagged as failed to index and shown in library with error status)
- US-008 [critical] Generate and store embeddings in pgvector using OpenAI (AC-013: per-chunk embedding generated with OpenAI text-embedding model and stored with text + document reference in pgvector; AC-014: missing/invalid OPENAI_API_KEY surfaces an error and document is not indexed)
- US-009 [high] Remove embeddings when a document is deleted (AC-015: all embedding vectors for the document removed from pgvector on deletion)

Assumptions: Uses OpenAI for embeddings (requires OPENAI_API_KEY); uses PostgreSQL+pgvector as vector store.

**Related:** US-007, US-008, US-009

### EPIC-004 · draft · source: requirements:EPIC-004

Natural-language question answering with source citations. Employees ask questions and get AI-generated answers grounded in uploaded documents, with citations naming source documents. Each question is independent (no conversation history). No source snippet preview at launch.

Stories:
- US-010 [critical] Ask a natural-language question and receive an AI-generated answer (AC-016: text input shown on load; AC-017: on submit, pgvector retrieves relevant chunks and OpenAI GPT-4-class model generates answer; AC-018: answer displayed)
- US-011 [critical] View source document citations alongside an answer (AC-019: answer includes citations naming source documents; AC-020: no relevant documents -> 'could not find relevant information' response)
- US-012 [high] Each question is independent with no conversation history (AC-021: subsequent question starts fresh, no retention; AC-022: page load/refresh shows no previous questions/answers)

Assumptions: OpenAI GPT-4-class model for answers (requires OPENAI_API_KEY); no conversation history persisted; no source snippet preview at launch.

**Related:** US-010, US-011, US-012

### EPIC-005 · draft · source: requirements:EPIC-005

Open-access web application. React frontend + Python (FastAPI) backend, browser-based. No authentication/RBAC for the pilot — anyone with the URL can use Q&A and admin interfaces. Auth/RBAC may be added later.

Stories:
- US-013 [critical] Access the question-answering interface without login (AC-023: Q&A interface available at app URL without login; AC-024: can submit questions and receive answers with citations without auth)
- US-014 [high] Access the admin document management interface without login (AC-025: admin upload/library/delete available without login; AC-026: no role distinction between admin and regular users)

Assumptions: Web app with React frontend and Python backend API; no RBAC at launch; no usage analytics or audit logging at launch.

**Related:** US-013, US-014

### NFR-001 · approved · source: requirements:NFR-001

Security NFR (accepted risk): No authentication or role-based access control at launch. Anyone who can reach the app URL can use both Q&A and admin interfaces. Auth may be added later.

Applies to US-013, US-014. Accepted pilot risk.

**Related:** US-013, US-014

### NFR-002 · approved · source: requirements:NFR-002

Privacy NFR: No conversation history persisted. Each question is independent; no prior questions/answers stored or displayed on load/refresh.

Applies to US-012.

**Related:** US-012

### NFR-003 · approved · source: requirements:NFR-003

Compliance NFR (accepted risk): No usage analytics or audit logging at launch. Document deletions and other admin actions are not tracked in an audit trail. Accepted pilot risk.

Applies to US-006.

**Related:** US-006

### NFR-004 · approved · source: requirements:NFR-004

Compatibility NFR: Web-based application accessed via browser. No native mobile app.

Applies to the whole system.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

### NFR-005 · approved · source: requirements:NFR-005

Usability NFR: Clear error and status feedback for rejected uploads (unsupported format, empty/oversized files), indexing failures (missing/invalid API key, document failed to index), and no relevant results for a question.

Applies to US-002, US-007, US-008, US-011.

**Related:** US-002, US-007, US-008, US-011
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-10-22

**Capabilities in scope:** Admin document upload (PDF, DOCX, TXT, Markdown), Document library management (list, view, delete), Document embedding and indexing for semantic search, Natural-language question answering with source citations, OpenAI models for document embeddings and answer generation, pgvector in PostgreSQL for vector storage and retrieval, MinIO for storing uploaded document files, Admin uploads in PDF, DOCX, TXT, and Markdown formats, No login required — anyone with the URL can access the app

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | FastAPI | FastAPI is the backend option on the table and it fits well: the core work is receiving a question, running a similarity search against pgvector, sending context to OpenAI, and returning an answer with citations. Python has the richest ecosystem for document text extraction (PDF, DOCX) and embedding workflows, which is central to your admin upload flow. It's also the stack your analyst assumed. |
| cloud | AWS | AWS is the cloud option on the table and suits an internal app that starts small and grows. You get managed PostgreSQL (RDS), container hosting for the backend and MinIO, and a clear path to add authentication, logging, and autoscaling as your user base grows. Since this is an internal company tool, AWS is likely already where your organization runs other infrastructure. |
| database | PostgreSQL | PostgreSQL with pgvector is already settled as your assumption, and it's the right call. You need to store document metadata, chunk text, and vector embeddings in one place. pgvector keeps everything in a single database so you don't run a separate vector service. PostgreSQL also handles the document library management (titles, upload dates, admin-managed status) as plain relational data. This is all the more sensible given the small pilot scale — one database to operate, not two. |
| frontend | React | The app is a straightforward question-and-answer interface: an admin upload/management area and an employee search box with answers and citations. There's no need for server-side rendering, SEO, or complex routing that Next.js adds. React keeps the frontend lean and matches the assumption your analyst already settled on. Since there's no login and no conversation history, the client-side state is minimal. |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Admin Document Upload

An admin uploads company documents (policies, handbooks, project docs, wikis) in PDF, DOCX, TXT, or Markdown format through a web interface. Uploaded files are stored in MinIO object storage for retrieval and processing. Assumption: Only an admin role manages documents — regular users never upload — but there is no login or role-based access control at launch, so the admin interface is accessible to anyone who can reach the app URL. Assumption: Admin uploads documents in common formats (PDF, DOCX, TXT, Markdown) through a web interface.

### US-001: Upload a document file through the web interface

**Priority:** critical

> As a team admin, I want to upload company documents in PDF, DOCX, TXT, or Markdown format through a web interface, so that the knowledge base contains the documents employees need to ask questions about.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | an admin is on the document upload page in the web application | the admin selects a file and submits the upload form | the upload form accepts file selection and allows submitting the file for upload |

### US-002: Reject unsupported file formats during upload

**Priority:** high

> As a team admin, I want to be notified when an uploaded file is not in a supported format, so that I do not waste time uploading files the system cannot process.

| ID | Given | When | Then |
|---|---|---|---|
| AC-002 | an admin is uploading a document | the admin selects a file with an unsupported format (e.g., .xlsx, .pptx) | the upload is rejected with a message indicating which formats are supported |
| AC-003 | an admin is uploading a document | the admin selects a file that is empty or exceeds a maximum file size limit | the upload is rejected with a message that the file is empty or too large |

### US-003: Store uploaded files in MinIO object storage

**Priority:** critical

> As a team admin, I want uploaded document files to be stored durably in MinIO object storage, so that the original files are retained and can be referenced or reprocessed if needed.

| ID | Given | When | Then |
|---|---|---|---|
| AC-004 | an admin has successfully uploaded a supported document file | the upload completes | the file is persisted in MinIO object storage and a success confirmation is shown to the admin |
| AC-005 | an admin has uploaded a document and it has been stored in MinIO | the admin navigates to the document library | the document appears in the document library with its filename and upload timestamp as metadata |

## EPIC-002: Document Library Management

An admin can list, view, and delete documents in the central library. Deleting a document removes the file from MinIO and its embeddings from pgvector. Assumption: No role-based access control at launch — the admin interface is accessible to anyone who can reach the app URL. Assumption: No usage analytics or audit logging at launch, so deletions are not tracked in an audit trail.

### US-004: List all documents in the library

**Priority:** high

> As a team admin, I want to view a list of all documents currently in the knowledge base, so that I can see what content is available and manage the library.

| ID | Given | When | Then |
|---|---|---|---|
| AC-006 | documents have been uploaded to the knowledge base | the admin opens the document library page | the library displays each document with its filename, upload date, and file format |
| AC-007 | no documents have been uploaded yet | the admin opens the document library page and no documents exist | the library displays an empty state message prompting the admin to upload documents |

### US-005: View document details

**Priority:** medium

> As a team admin, I want to view details about a specific document in the library, so that I can verify which documents are present and their metadata.

| ID | Given | When | Then |
|---|---|---|---|
| AC-008 | a document exists in the library | the admin selects a document from the library | the detail view shows the document filename, file format, upload timestamp, and file size |

### US-006: Delete a document from the library

**Priority:** high

> As a team admin, I want to delete a document from the library, so that outdated or incorrect documents are removed from the knowledge base and no longer surface in answers.

| ID | Given | When | Then |
|---|---|---|---|
| AC-009 | a document exists in the library | the admin clicks delete on a document | the system prompts the admin to confirm the deletion |
| AC-010 | the admin has confirmed deletion of a document | the admin confirms the deletion | the file is removed from MinIO, its embeddings are removed from pgvector, and the document no longer appears in the library |

## EPIC-003: Document Embedding and Indexing

When a document is uploaded, the system extracts text, chunks it into segments, generates vector embeddings using OpenAI's text-embedding model, and stores the embeddings in PostgreSQL with pgvector for semantic retrieval. When a document is deleted, its embeddings are removed. Assumption: Uses OpenAI for embeddings and needs an OPENAI_API_KEY configured in the backend. Assumption: Uses PostgreSQL with pgvector as the vector store for document embeddings and retrieval.

### US-007: Extract and chunk text from uploaded documents

**Priority:** critical

> As a team admin, I want the system to automatically extract and chunk text from documents I upload, so that the content is prepared for semantic search and retrieval.

| ID | Given | When | Then |
|---|---|---|---|
| AC-011 | a PDF, DOCX, TXT, or Markdown file has been uploaded and stored in MinIO | the upload completes and indexing begins | the system extracts the text content from the file and splits it into chunk segments suitable for embedding |
| AC-012 | a document file is corrupted or contains no extractable text | text extraction is attempted | the system flags the document as failed to index and makes it visible in the library with an error status |

### US-008: Generate and store embeddings in pgvector using OpenAI

**Priority:** critical

> As a team admin, I want the system to generate vector embeddings for each text chunk using OpenAI and store them in pgvector, so that documents are semantically indexed and retrievable for question answering.

| ID | Given | When | Then |
|---|---|---|---|
| AC-013 | text chunks have been extracted from an uploaded document | the embedding process runs | the system generates an embedding vector for each chunk using the configured OpenAI text-embedding model and stores the vectors with their associated text and document reference in pgvector |
| AC-014 | the OPENAI_API_KEY is missing or invalid | the embedding process attempts to call OpenAI | the system surfaces an error indicating the API key is missing or invalid and does not index the document |

### US-009: Remove embeddings when a document is deleted

**Priority:** high

> As a team admin, I want the system to remove all embeddings for a document when that document is deleted, so that deleted documents do not appear in search results or answers.

| ID | Given | When | Then |
|---|---|---|---|
| AC-015 | the admin has confirmed deletion of a document | the document is deleted | all embedding vectors associated with that document are removed from pgvector |

## EPIC-004: Natural-Language Question Answering with Source Citations

Employees ask natural-language questions through the web interface and receive AI-generated answers grounded in the uploaded documents, with citations pointing to the source documents. Each question is independent — no conversation history is persisted between questions. Source snippet previews are not provided at launch; citations reference the source document by name. Assumption: Uses OpenAI GPT-4-class model for answer generation and needs an OPENAI_API_KEY. Assumption: No conversation history is persisted — each question is independent. Assumption: No source snippet preview at launch.

### US-010: Ask a natural-language question and receive an AI-generated answer

**Priority:** critical

> As a employee, I want to type a natural-language question and receive an AI-generated answer, so that I can quickly find information from company documents without searching through them manually.

| ID | Given | When | Then |
|---|---|---|---|
| AC-016 | the employee is on the question-answering page | the page loads | the system displays a text input where the employee can enter a natural-language question |
| AC-017 | the employee has entered a question | the employee submits the question | the system uses pgvector to retrieve the most semantically relevant document chunks and generates an answer using the OpenAI GPT-4-class model |
| AC-018 | the system has generated an answer | the answer generation completes | the answer is displayed to the employee in the web interface |

### US-011: View source document citations alongside an answer

**Priority:** critical

> As a employee, I want each answer to include citations pointing to the source documents it was derived from, so that I can trust and verify the answer by referring to the original company documents.

| ID | Given | When | Then |
|---|---|---|---|
| AC-019 | the system has generated an answer for a question | the answer is displayed | the answer includes citations listing the names of the source documents the answer was derived from |
| AC-020 | no relevant documents are found for a question | the retrieval step returns no sufficiently relevant results | the system responds indicating it could not find relevant information in the knowledge base |

### US-012: Each question is independent with no conversation history

**Priority:** high

> As a employee, I want each question I ask to be treated independently with no conversation history, so that I understand that the system does not remember prior questions and each query stands alone.

| ID | Given | When | Then |
|---|---|---|---|
| AC-021 | an employee has received an answer to a question | the employee asks a subsequent question | the system does not retain the question or answer and starts a fresh independent interaction |
| AC-022 | an employee is on the question-answering page | the page is loaded or refreshed | no previous questions or answers are displayed |

## EPIC-005: Open-Access Web Application

The application is a web application built with a React frontend and a Node.js/Python backend API, accessed via browser. No authentication is required for the pilot — anyone who can reach the app URL can use the question-answering interface and the admin document management interface. Authentication and role-based access control may be added later as the app grows. Assumption: Built as a web application with a React frontend and a Node.js/Python backend API. Assumption: The app does not support role-based access control at launch (no login). Assumption: No usage analytics or audit logging at launch.

### US-013: Access the question-answering interface without login

**Priority:** critical

> As a employee, I want to access the question-answering interface directly via the app URL without logging in, so that I can use the knowledge base immediately without any authentication barrier during the pilot.

| ID | Given | When | Then |
|---|---|---|---|
| AC-023 | a person has the app URL in a web browser | the person navigates to the app URL | the question-answering interface is available for use without any login or authentication step |
| AC-024 | a person has accessed the app without logging in | the person interacts with the question-answering interface | the person can submit questions and receive answers with citations |

### US-014: Access the admin document management interface without login

**Priority:** high

> As a team admin, I want to access the document management interface without login during the pilot, so that I can manage the document library without setting up authentication, accepting the pilot risk that anyone with the URL can also manage documents.

| ID | Given | When | Then |
|---|---|---|---|
| AC-025 | a person has the app URL in a web browser | the person navigates to the admin interface | the admin document upload, library listing, and delete functions are available without any login or authentication step |
| AC-026 | any user accesses the admin interface | the user performs administrative actions | the user can upload, view, and delete documents with no role distinction between admin and regular users |
<!-- sdlc:auto:end epics -->
