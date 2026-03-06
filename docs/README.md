# Unisys Migration Assistant Platform

Enterprise platform for analyzing, assessing, and modernizing legacy Java applications. Combines static code analysis, AI-powered intelligence, and automated code transformation to deliver actionable migration strategies.


## Architecture
```
Eclipse Plugin (IDE Client)
        │
        ▼
Assessment Service ──▶ GenAI Service ──▶ OpenAI / Azure OpenAI
        │
        ▼
Template Config Service

```

## Modules

| Module               | Description                                              |
|----------------------|----------------------------------------------------------|
| assessment-service   | Code analysis, complexity scoring, and report generation |
| genai-service        | AI integration layer for OpenAI and Azure OpenAI         |
| template-config      | Automated code transformation and migration engine       |
| eclipse-plugin       | Eclipse IDE plugin for in-IDE assessment and migration   |


## Key Capabilities

- Automated analysis of Maven, Gradle, and Ant projects
- Three-tier complexity classification (L1 / L2 / L3)
- AI-powered Assessment reasons & migration recommendations 
- Framework migration (JAX-RS, EJB, JSF → Spring Boot)
- Java version upgrades (8 → 17 → 21)
- HTML report generation
- Interactive Eclipse plugin with 28+ migration actions


## Technology Stack

| Layer          | Technologies                                    |
|----------------|-------------------------------------------------|
| Language       | Java 17                                         |
| Framework      | Spring Boot 3.5.7                               |
| Build          | Maven Multi-Module                              |
| AI             | OpenAI GPT-4o, Azure OpenAI (Assistants API v2) |
| Analysis       | JavaParser, Checkstyle, OpenRewrite             |
| Reporting      | FreeMarker, Playwright (PDF)                    |
| IDE            | Eclipse RCP, SWT, OSGi                          |
| Security       | OAuth2 / JWT (Auth0)                            |
| Infrastructure | Docker, Kubernetes, Azure DevOps                |


## Getting Started

```bash
# Build all modules
cd unisys-migration-plugin
mvn clean install

# Run Assessment Service
cd assessment-service
mvn spring-boot:run
```

## Deployment

| Environment | URL                                |
|-------------|------------------------------------|      
| Integration | codeassessment-int.rcp.unisys.com  |
| QA          | codeassessment-qa.rcp.unisys.com   |
| Production  | codeassessment-prod.rcp.unisys.com |


## API Reference

| Method | Endpoint           | Description                              |
|--------|--------------------|------------------------------------------|
| GET    | `/`                | Health check                             |
| POST   | `/generate-report` | Generate assessment report               |
| GET    | `/processFiles`    | Process files with transformation config |


## Security

- OAuth2 Resource Server with JWT Bearer tokens
- Auth0 identity provider
- TLS enforced across all environments
- Non-root container execution
- SonarQube and Trivy scanning in CI/CD pipeline

---