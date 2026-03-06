# Assessment Service

Core backend service for analyzing Java codebases and generating modernization assessment reports.

## Responsibilities

- Analyze project structure, dependencies, and framework usage
- Support Maven, Gradle, and Ant build systems
- Classify project complexity (L1 / L2 / L3)
- Generate HTML and PDF assessment reports
- Integrate with GenAI Service for AI-powered recommendations

## REST API

| Method | Endpoint           | Description                | Auth   |
|--------|--------------------|----------------------------|--------|
| GET    | `/`                | Health check               | Public |
| POST   | `/generate-report` | Generate assessment report | JWT    |

### POST /generate-report

Accepts a ZIP archive of a Java project and returns a modernization report.

**Request:** `multipart/form-data`

| Parameter | Type   | Required | Description                 |
|-----------|--------|----------|-----------------------------|
| projectZip| file   | Yes      | ZIP archive of the project  |
| provider  | string | Yes      | `OPEN_AI` or `AZURE_OPEN_AI`|

**Response:** HTML or PDF report (200), 400 for invalid input, 401 for missing auth.

## Analysis Pipeline

1. Extract ZIP and detect build tool (Maven / Gradle / Ant)
2. Parse project metadata — modules, packages, classes, dependencies
3. Detect frameworks (JAX-RS, EJB, JSF, WebLogic, JPA)
4. Apply complexity classification rules
5. Submit metadata to GenAI Service for AI analysis
6. Render report using FreeMarker templates

## Complexity Classification

| Level        | Description                                                        |
|--------------|--------------------------------------------------------------------|
| L1 — Simple  | Read-only persistence, no UI, single framework, stateless          |
| L2 — Medium  | Database CRUD, UI present, deprecated frameworks                   |
| L3 — Complex | Multiple frameworks, WebLogic, multithreading, EJB, authentication |

## Dependencies

**Internal:** genai-service

**Key external libraries:**
- Spring Boot 3.5.7 (Web, Security, Actuator)
- JavaParser 3.25.10
- Checkstyle 12.3
- Apache Maven Model 3.9.11
- Gradle Tooling API 9.0.0
- FreeMarker (template rendering)
- Playwright (PDF generation)

## Security

- OAuth2 Resource Server with JWT Bearer tokens (Auth0)
- Public endpoints: `/`, `/swagger-ui/**`, `/v3/api-docs/**`
- All other endpoints require a valid JWT

## Build & Run

```bash
mvn clean install
mvn spring-boot:run
# API: http://localhost:8080
# Swagger: http://localhost:8080/swagger-ui.html
```

---