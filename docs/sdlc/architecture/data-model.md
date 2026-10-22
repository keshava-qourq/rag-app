# Data Model

<!-- sdlc:auto:start entities -->
### documents

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| filename | varchar(255) | False | — | False |
| file_format | varchar(20) | False | — | False |
| file_size_bytes | bigint | False | — | False |
| minio_object_key | varchar(512) | False | — | False |
| upload_timestamp | timestamp with time zone | False | — | False |
| status | varchar(20) | False | — | False |
| error_message | text | False | — | True |

### document_chunks

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| document_id | uuid | False | documents.id | False |
| chunk_index | integer | False | — | False |
| chunk_text | text | False | — | False |
| embedding | vector(1536) | False | — | False |
<!-- sdlc:auto:end entities -->

<!-- sdlc:auto:start relationships -->
| Source | Target | Type |
|---|---|---|
| documents | document_chunks | one-to-many |
<!-- sdlc:auto:end relationships -->

<!-- sdlc:auto:start er-diagram -->
```mermaid
erDiagram
  documents {
    uuid id PK
    varchar_255_ filename
    varchar_20_ file_format
    bigint file_size_bytes
    varchar_512_ minio_object_key
    timestamp_with_time_zone upload_timestamp
    varchar_20_ status
    text error_message
  }
  document_chunks {
    uuid id PK
    uuid document_id FK
    integer chunk_index
    text chunk_text
    vector_1536_ embedding
  }
  documents ||--o{ document_chunks : "one-to-many"
```
<!-- sdlc:auto:end er-diagram -->
