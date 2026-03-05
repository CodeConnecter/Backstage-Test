# Unisys Migration Assistant Platform

The Unisys Migration Assistant platform provides automated tools to analyze and modernize legacy applications.  
It includes AI-assisted code analysis, template-driven reporting, and an Eclipse plugin for developer interaction.

## Architecture

The platform consists of the following components:

| Component | Description |
|-----------|-------------|
| Assessment Service | Core backend service responsible for performing code assessments |
| GenAI Service | Provides AI-powered analysis and recommendations |
| Template Config | Handles template configuration for reports |
| Eclipse Plugin | Developer-facing plugin used inside Eclipse IDE |

## System Architecture

Eclipse Plugin → Assessment Service → GenAI Service  
                                  ↘ Template Config

## Modules

### Assessment Service
Spring Boot application that:

- analyzes project source code
- generates modernization assessment reports
- integrates with AI services

### GenAI Service

Provides AI-based:

- code insights
- migration recommendations
- modernization strategies

### Template Config

Responsible for:

- report templates
- configuration management
- template-driven output generation

### Eclipse Plugin

Provides a UI within Eclipse for:

- triggering code assessments
- visualizing results
- exporting reports

## Technology Stack

- Java 17
- Spring Boot
- Maven Multi-module
- OpenAPI
- Eclipse Plugin Development
- Generative AI Integration

## Build Instructions

Build the full platform:

```bash
mvn clean install