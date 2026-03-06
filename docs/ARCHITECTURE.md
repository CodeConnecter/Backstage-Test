# Architecture Overview

## System Context

The Unisys Migration Assistant Platform is a multi-module Java application that provides end-to-end legacy application modernization — from analysis through transformation.

```
┌─────────────────────────────────────┐
│          Eclipse IDE (Client)       │
│  ┌───────────────────────────────┐  │
│  │     Migration Assistant       │  │
│  │         Eclipse Plugin        │  │
│  └──────────────┬────────────────┘  │
└─────────────────┼───────────────────┘
                  │
┌─────────────────▼───────────────────┐
│         Backend Services            │
│                                     │
│  ┌──────────────────────────────┐   │
│  │     Assessment Service       │   │
│  │  • Code analysis             │   │
│  │  • Complexity scoring        │   │
│  │  • Report generation         │   │
│  └──────────┬───────────────────┘   │
│             │                       │
│  ┌──────────▼───────────────────┐   │
│  │       GenAI Service          │   │
│  │  • OpenAI / Azure OpenAI     │   │
│  │  • AI-powered recommendations│   │
│  └──────────────────────────────┘   │
│                                     │
│  ┌──────────────────────────────┐   │
│  │    Template Config Service   │   │
│  │  • Framework migration       │   │
│  │  • Code transformation       │   │
│  │  • OpenRewrite recipes       │   │
│  └──────────────────────────────┘   │
└─────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────┐
│          Infrastructure             │
│  Docker · Kubernetes · Azure DevOps │
│  Auth0 (OAuth2/JWT) · TLS 1.2/1.3   │
└─────────────────────────────────────┘
```

## Module Dependencies

```
eclipse-plugin
├── assessment-service
│   └── genai-service
└── template-config
```

## Data Flow

### Assessment Pipeline

1. User uploads a Java project (ZIP) via Eclipse Plugin or REST API
2. Assessment Service detects the build tool (Maven / Gradle / Ant)
3. Metadata is extracted: modules, packages, classes, dependencies
4. Complexity rules classify the project as L1 (Simple), L2 (Medium), or L3 (Complex)
5. GenAI Service submits metadata to GPT-4o for intelligent recommendations
6. Results are merged into an HTML/PDF report via FreeMarker templates

### Transformation Pipeline

1. Developer selects a migration action in Eclipse (e.g., JAX-RS → Spring Boot)
2. Template Config Service loads the YAML transformation configuration
3. Source files are parsed into AST using JavaParser
4. Annotations, imports, and types are transformed
5. Build files (pom.xml) are updated with new dependencies
6. Modified files are written back to the workspace

## Deployment Topology

Four Kubernetes environments are provisioned on Azure:

| Environment | Purpose             | Ingress                          |
|-------------|---------------------|----------------------------------|
| Demo        | Internal demos      | ClusterIP only                   |
| Integration | Development testing | codeassessment-int.rcp.unisys.com|
| QA          | Quality assurance   | codeassessment-qa.rcp.unisys.com |
| Production  | Live deployment     | codeassessment-prod.rcp.unisys.com|


### Container

- Base image: `eclipse-temurin:17-jre-alpine`
- Non-root user execution
- Exposed port: 8080

## Security

| Layer             | Implementation                         |
|-------------------|----------------------------------------|
| Authentication    | OAuth2 Resource Server with JWT        |
| Identity Provider | Auth0                                  |
| Transport         | TLS 1.2 / 1.3 with ECDHE cipher suites |
| Container         | Non-root, Alpine-based, Trivy scanned  |
| Code Quality      | SonarQube static analysis, Checkstyle  |

## Technology Stack

| Category       | Technologies                                       |
|----------------|----------------------------------------------------|
| Language       | Java 17                                            |
| Framework      | Spring Boot 3.5.7, Spring Security                 |
| Build          | Maven Multi-Module, OpenRewrite                    |
| AI             | OpenAI GPT-4o, Azure OpenAI (Assistants API v2)    |
| Analysis       | JavaParser 3.25.10, Checkstyle 12.3                |
| Reporting      | FreeMarker, Playwright (PDF)                       |    
| IDE            | Eclipse RCP, SWT, OSGi                             |
| Data           | Jackson, Gson, JSoup, Apache Tika                  |
| Infrastructure | Docker, Kubernetes (AKS), Azure DevOps             |



## Complexity Classification

| Level        | Criteria                                                  |
|--------------|-----------------------------------------------------------|
| L1 — Simple  | Read-only persistence, no UI, single framework, stateless |
| L2 — Medium  | Database CRUD, UI present, deprecated frameworks, stateful|
| L3 — Complex | Multiple frameworks, WebLogic, multithreading, EJB, JMS   |
