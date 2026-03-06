# GenAI Service

AI integration layer providing a unified abstraction over OpenAI and Azure OpenAI APIs for intelligent code analysis and migration recommendations.

## Responsibilities

- Manage AI assistant lifecycle (create, retrieve, configure)
- Handle threaded conversations with file-aware context
- Upload code metadata for enriched AI analysis
- Process and format AI responses
- Support provider switching (OpenAI ↔ Azure OpenAI)

## Supported Providers

| Provider     | API                            | Model  |
|--------------|--------------------------------|--------|
| OpenAI       | Assistants API v2              | GPT-4o |
| Azure OpenAI | API version 2024-05-01-preview | GPT-4o |

## Integration Flow

1. Initialize provider configuration (OpenAI or Azure OpenAI)
2. Retrieve or create an AI assistant with system instructions
3. Upload code metadata as context files
4. Create a conversation thread and add analysis prompt
5. Execute a run and poll for completion
6. Retrieve AI response with recommendations

## Architecture

```
Consumer (Assessment Service / Eclipse Plugin)
        │
        ▼
  GenAI Service (Library JAR)
  ├── Configuration Layer (provider factory, settings)
  ├── Service Layer (assistant lifecycle, threads)
  ├── Client Layer (REST calls, response parsing)
  └── File Management (upload, CRUD, tracking)
        │
        ▼
  OpenAI / Azure OpenAI API
```

## Configuration

```properties
# Provider selection
genai.openai.subscription-type=OPEN_AI
genai.openai.model-name=gpt-4o
genai.openai.api-key=${OPENAI_API_KEY}
genai.openai.api-base-url=https://api.openai.com
genai.openai.assistant-model-temperature=0.3
```

## Dependencies

- Spring Boot Starter Web 3.5.7
- Jackson Databind (JSON processing)
- Gson
- Lombok
- SLF4J 2.0.17

## Design Patterns

| Pattern | Usage                                      |
|---------|--------------------------------------------|
| Factory | Provider-specific configuration creation   |
| Facade  | Simplified AI API surface                  |
| Strategy| Provider switching (OpenAI / Azure OpenAI) |
| Builder | HTTP request construction                  |