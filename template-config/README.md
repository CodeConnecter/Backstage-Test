# Template Config Service

Automated code transformation engine that migrates legacy Java frameworks to modern equivalents using rule-driven transformations, AST manipulation, and OpenRewrite recipes.

## Responsibilities

- Migrate legacy frameworks (JAX-RS, EJB, JSF, JMS) to Spring Boot
- Upgrade Java versions (8 → 17 → 21) and Spring Boot (2.x → 3.x)
- Generate infrastructure files (Dockerfile, Azure Pipelines, Swagger config)
- Auto-fix code quality issues (Checkstyle, unused imports, dead code)
- Process files using YAML-driven transformation configurations

## REST API

| Method | Endpoint        | Description                           | Auth |
|--------|-----------------|---------------------------------------|------|
| GET    | `/processFiles` | Process files with YAML configuration | JWT  |

**Parameters:** `projectPath` (project directory) and `ymlFileName` (transformation config).

## Transformation Capabilities

### Framework Migrations

| Source             | Target                          |
|--------------------|---------------------------------|
| JAX-RS annotations | Spring Boot REST annotations    |
| EJB components     | Spring Services                 |
| JSF managed beans  | Spring Controllers              |
| JMS messaging      | Spring JMS / RabbitMQ listeners |

### Version Upgrades

| Upgrade               | Key Changes                                             |
|-----------------------|---------------------------------------------------------|
| Java 8 → 17           | Text blocks, records, sealed classes, pattern matching  |
| Java 17 → 21          | Virtual threads, sequenced collections, record patterns |
| Spring Boot 2.x → 3.x | javax → jakarta namespace, Spring Security 6            |

### Infrastructure Generation

- Dockerfile (multi-stage, Alpine-based)
- Azure Pipelines YAML
- application.properties
- Swagger/OpenAPI configuration class
- Spring Boot main class
- Database configuration (EclipseLink, JDBC, HikariCP)

### Code Quality

- Checkstyle violation auto-fix
- Unused import and dead code removal
- JUnit test modernization
- System.out → SLF4J logging migration


## Dependencies

**Key external libraries:**
- Spring Boot 3.5.7 (Web, Actuator)
- JavaParser 3.25.10 (AST manipulation)
- Apache Maven Model Builder 3.9.6
- OpenRewrite (recipe-based refactoring)
- SpringDoc OpenAPI UI 2.8.14
- JavaEE API 8.0 (legacy reference)
- Apache Commons Lang3 3.19.0

## Used By

- assessment-service
- eclipse-plugin