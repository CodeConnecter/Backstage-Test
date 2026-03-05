# Assessment Service

Assessment Service is responsible for managing and evaluating assessments within the platform.

## Features

- Create assessments
- Retrieve assessment results
- Manage assessment metadata

## Architecture

This service is part of the **Examples Platform** and interacts with:

- User Service
- Authentication Service
- PostgreSQL database

## APIs

| API | Description |
|----|----|
| Assessment API | Manage assessments |
| User API | Retrieve user information |
| Authentication API | Handle authentication and authorization |

## Dependencies

- User Service
- Authentication Service
- PostgreSQL Database

## Tech Stack

- Java
- Spring Boot
- REST APIs
- OpenAPI
- Backstage TechDocs

## Running Locally

```bash
mvn spring-boot:run