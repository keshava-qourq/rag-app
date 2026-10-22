# RAG app — SDLC Documentation

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start overview -->
RAG app

**Documentation version:** v2
<!-- sdlc:auto:end overview -->

## Contents

<!-- sdlc:auto:start contents -->
| Document | State |
|---|---|
| [Requirements](requirements/README.md) | v1 approved |
| [Architecture](architecture/README.md) | 25 entries |
| [Development](development/README.md) | 0 entries |
| [Testing](testing/README.md) | 0 entries |
| [Deployment](deployment/README.md) | 0 entries |
| [Maintenance](maintenance/README.md) | 0 entries |
| [Data model](architecture/data-model.md) | generated |
| [API (OpenAPI)](architecture/api/openapi.yaml) | generated |
| [Decisions (ADRs)](architecture/adr/README.md) | 4 decisions |
| [Sprint plan](development/sprint-plan.md) | 3 sprints |
| [Developer guide](development/developer-guide.md) | generated |
| [API reference](development/api-reference.md) | 1 route |
| [Test report](testing/test-report.md) | not run yet |
| [Deployment guide](deployment/deployment-guide.md) | not deployed yet |
| [Runbook](deployment/runbook.md) | not deployed yet |
| [Release notes](deployment/release-notes.md) | 0 releases |
| [Traceability matrix](traceability-matrix.md) | 14 requirements |
| [Changelog](CHANGELOG.md) | 2 versions |
<!-- sdlc:auto:end contents -->

## Open gaps

<!-- sdlc:auto:start open-gaps -->
### Architecture

- Specific OpenAI model name/version not identified beyond 'GPT-4-class model'.
- Order of deletion steps (MinIO first vs. database first) and rollback/consistency handling on partial failure not specified.
- Data types and constraints for each field not specified.
- Exact values for the status field (e.g., pending, indexing, indexed, failed) not enumerated.
- Vector embedding dimensionality not specified.
- Chunking strategy/parameters (chunk size, overlap) not specified in architecture.
- Maximum file size limit not specified.
- Request/response payload structure not specified.
- Pagination strategy not specified.
- Sorting/filtering parameters not specified.
- Response payload structure not specified.
- Error response for non-existent document_id not specified.
- Transaction/consistency guarantees across MinIO and PostgreSQL on partial failure not specified.
- Number of chunks retrieved for context (top-k) not specified.
- Citation format not specified.
- MinIO data persistence strategy on ECS Fargate (EFS volume or other) not specified.
<!-- sdlc:auto:end open-gaps -->
