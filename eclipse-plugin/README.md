# Migration Assistant Eclipse Plugin

Eclipse IDE plugin providing an integrated interface for running modernization assessments, framework migrations, and AI-assisted development directly within the IDE.

## Features

- Generate assessment reports from within Eclipse
- One-click framework migrations (JAX-RS, EJB, JSF → Spring Boot)
- Java version upgrades (8 → 17 → 21, Spring Boot 2.x → 3.x)
- AI-powered code review, test generation, error fixing, and documentation
- Infrastructure generation (Dockerfile, Azure Pipelines, Swagger config)
- Code quality improvements (Checkstyle fixes, code cleanup)
- Git integration

## Available Actions

### Assessment & Reporting

- Generate modernization assessment report (HTML/PDF)
- Create project README documentation

### Framework Migrations

- JAX-RS → Spring Boot REST
- EJB → Spring Components
- JSF Migration
- JMS Modernization
- SLF4J Logging Migration

### Version Upgrades

- Java 17 / Java 21 upgrade
- Spring Boot 3 upgrade

### Configuration & Infrastructure

- Initialize Spring Boot project
- Generate application.properties
- Generate Dockerfile
- Generate Swagger/OpenAPI annotations
- JDBC / EclipseLink configuration
- Redis caching setup

### AI-Assisted Development

- Interactive AI code discussion
- AI-powered error fixing
- AI-generated Swagger documentation
- AI-generated test cases

### Code Quality

- Checkstyle violation auto-fix
- Code cleanup

## AI Prompt System

The plugin includes prompt templates for AI interactions:
- System prompt, code review, refactoring, test generation
- Error fixing, Swagger annotations, documentation
- Git commit messages, code discussion

## Technology

- Eclipse RCP (SWT/JFace)
- OSGi bundle architecture
- Embedded assessment-service and template-config JARs
- HTTP client for GenAI Service communication

## Dependencies

**Internal:** assessment-service, genai-service, template-config

**Bundled libraries:** JSoup, Jackson, Apache HttpClient5, Selenium, Flexmark

## Usage

1. Install the plugin in Eclipse (2023-06 or later)
2. Open a Java project
3. Right-click → **Migration Assistant** → select an action
4. View results in the IDE