# GenAI Service — High-Level Design

## Introduction

The GenAI Service provides a unified AI integration layer that abstracts interactions with LLM providers (OpenAI and Azure OpenAI). It is consumed by the Assessment Service for code analysis and by the Eclipse Plugin for AI-assisted development.

### Design Goals

- Single API surface regardless of underlying AI provider
- Configurable AI behavior (model, temperature, instructions)
- File-aware analysis with code metadata uploads
- Resilient integration with error handling and retries

## System Context

```
Assessment Service ──┐
                     ├──▶ GenAI Service (Library JAR) ──▶ OpenAI / Azure OpenAI
Eclipse Plugin ──────┘
```

**Consumers:**
- Assessment Service — complexity analysis and Assessment reasons & migration recommendations
- Eclipse Plugin — code review, test generation, error fixing, documentation

## Architecture

```
┌─────────────────────────────────────────┐
│           Consumer Applications         │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│  Service Layer                          │
│  Assistant lifecycle and orchestration  │
├─────────────────────────────────────────┤
│  Client Layer                           │
│  HTTP communication, response parsing   │
├─────────────────────────────────────────┤
│  Configuration Layer                    │
│  Provider factory, runtime settings     │
├─────────────────────────────────────────┤
│  File Management Layer                  │
│  Upload, CRUD, usage tracking           │
└──────────────────┬──────────────────────┘
                   │
┌──────────────────▼──────────────────────┐
│  OpenAI / Azure OpenAI REST APIs        │
└─────────────────────────────────────────┘
```

## Integration Flow

```
Client                   GenAI Service           AI Provider
  │                          │                        │
  │ 1. Init provider         │                        │
  ├───────────────────────>  │                        │
  │ 2. Retrieve assistant    │  GET /assistants/{id}  │
  │                          ├──────────────────────> │
  │ 3. Upload file           │  POST /files           │
  │                          ├──────────────────────> │
  │ 4. Create thread + run   │  POST /threads, /runs  │
  │                          ├──────────────────────> │
  │ 5. Poll status           │  GET /runs/{id}        │
  │                          ├──────────────────────> │
  │ 6. Get response          │  GET /messages         │
  │<─────────────────────────┤<───────────────────────┤
```

## Provider Strategy

| Aspect         | OpenAI             | Azure OpenAI       |
|----------------|--------------------|--------------------|
| Base URL       | api.openai.com     | *.openai.azure.com |
| API Version    | v2                 | 2024-05-01-preview |
| Authentication | Bearer token       | api-key header     |
| Model          | GPT-4o             | GPT-4o (deployment)|

## Deployment

The GenAI Service is packaged as an embedded library JAR within the Assessment Service — no separate deployment is required. External calls go directly to OpenAI or Azure OpenAI cloud endpoints over HTTPS.

| Environment        | AI Provider  | Model  | Temperature |
|--------------------|--------------|--------|-------------|
| Demo / Integration | Azure OpenAI | GPT-4o | 0.3         |
| QA                 | OpenAI       | GPT-4o | 0.3         |
| Production         | Azure OpenAI | GPT-4o | 0.3         |


## Security

- API keys stored in Kubernetes Secrets
- TLS 1.2/1.3 for all external API calls
- Code metadata sanitized before AI submission
- Azure OpenAI supports regional deployment for data residency

