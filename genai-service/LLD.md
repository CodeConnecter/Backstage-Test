# GenAI Service — Low-Level Design

## Class Structure


### Configuration Layer

**GenAiProvider** — Enum defining supported providers: `OPEN_AI`, `AZURE_OPEN_AI`.
**GenAiConfiguration** — Interface contract for provider settings (API key, base URL, model, temperature, assistant ID, version).
**ConfigurationProviderFactory** — Creates provider-specific configuration instances based on the selected provider.
**ConfigHandler** — Manages runtime provider configuration; delegates to the factory for instantiation.


### Service Layer

**GenAiAssistantService** — Facade for all AI operations: assistant lifecycle, thread management, message handling, and file operations.
**GenAiAssistantClient** — REST client handling HTTP communication with OpenAI/Azure OpenAI APIs. Builds provider-specific URLs and headers.
**GenAiAssistantHelper** — Parses API responses: extracts assistant IDs, thread IDs, run statuses, and message content.
**OpenAiResponseErrorHandler** — Custom error handler mapping HTTP status codes to typed exceptions (401 → auth, 429 → rate limit, 5xx → server error).


### File Management Layer

**FileUploadApi** — Uploads files (code metadata) to the AI assistant context.

**FileManagementService** — CRUD operations for managed files with cleanup of unused files.
**FileUsageService** — Tracks active file references across sessions using a concurrent set.

### Data Models

**Data** — Generic container for paginated API list responses.
**Run** — Represents an assistant run with status tracking (queued → in_progress → completed/failed).

## Sequence: Code Assessment Analysis

```
Assessment Service         GenAI Service              AI Provider
       │                        │                          │
       │  retrieveOrCreate()    │                          │
       ├─────────────────────>  │  GET /assistants/{id}    │
       │                        ├─────────────────────────>│
       │                        │<─────────────────────────┤
       │<───────────────────────┤                          │
       │                        │                          │
       │  uploadFile()          │  POST /files             │
       ├───────────────────────>├─────────────────────────>│
       │<───────────────────────┤<─────────────────────────┤
       │                        │                          │
       │  createThread()        │  POST /threads           │
       ├───────────────────────>├─────────────────────────>│
       │<───────────────────────┤<─────────────────────────┤
       │                        │                          │
       │  addMessage(prompt)    │  POST /threads/{id}/msgs │
       ├───────────────────────>├─────────────────────────>│
       │                        │                          │
       │  createRun()           │  POST /threads/{id}/runs │
       ├───────────────────────>├─────────────────────────>│
       │                        │                          │
       │  pollRunStatus()       │  GET /runs/{id}  (loop)  │
       ├───────────────────────>├─────────────────────────>│
       │                        │<─────────────────────────┤
       │                        │  status: completed       │
       │                        │                          │
       │  getMessages()         │  GET /threads/{id}/msgs  │
       ├───────────────────────>├─────────────────────────>│
       │<───────────────────────┤<─────────────────────────┤
       │  (AI recommendations)  │                          │
```

## API Contracts

### OpenAI Endpoints

| Operation        | Method | Path                           |
|------------------|--------|--------------------------------|
| Get Assistant    | GET    | /v1/assistants/{id}            |
| Create Assistant | POST   | /v1/assistants                 |
| Create Thread    | POST   | /v1/threads                    |
| Add Message      | POST   | /v1/threads/{id}/messages      |
| Create Run       | POST   | /v1/threads/{id}/runs          |
| Get Run Status   | GET    | /v1/threads/{id}/runs/{run_id} |
| List Messages    | GET    | /v1/threads/{id}/messages      |
| Upload File      | POST   | /v1/files                      |
| Delete File      | DELETE | /v1/files/{id}                 |


### Azure OpenAI Endpoints

Same operations as OpenAI with these differences:
- Base path: `/openai/` instead of `/v1/`
- Query parameter: `?api-version=2024-05-01-preview`
- Authentication header: `api-key: {key}` instead of Bearer token

## Error Handling

| Error                  | HTTP Code | Strategy                                     |
|------------------------|-----------|----------------------------------------------|
| Authentication failure | 401       | Throw auth exception; check API key          |
| Rate limit exceeded    | 429       | Exponential backoff retry (max 3 attempts)   |
| Model overloaded       | 503       | Retry after `Retry-After` header             |
| Invalid request        | 400       | Log details, throw request exception         |
| Run failed             | —         | Check `run.last_error`, report to caller     |
| Network timeout        | —         | Retry once, then propagate timeout exception |

## Configuration Properties

```
genai.{provider}.subscription-type           → Provider type
genai.{provider}.api-key                     → API key
genai.{provider}.api-base-url                → Base URL
genai.{provider}.assistant-id                → Assistant ID
genai.{provider}.model-name                  → Model name
genai.{provider}.assistant-model-temperature → Temperature
genai.{provider}.assistant-version           → API version
genai.{provider}.assistant-name              → Assistant name
genai.{provider}.deployment-name             → Azure deployment name

Where {provider} = "openai" or "azure"
```


## Dependencies

```
genai-service
├── spring-boot-starter-web 3.5.7
├── jackson-databind
├── gson
├── lombok
└── slf4j-api 2.0.17
```
