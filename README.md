# Backstage Test — User Service

This repository contains a small Java `user-service` used for Backstage demo/testing.

What’s included
- `catalog-info.yaml`: Backstage component and API entities.
- `api/openapi.yaml`: OpenAPI definition for the `user-service-api` API entity.
- `docs/`: TechDocs content (rendered by Backstage TechDocs plugin). Includes `dependencies.md` and `api.md`.
- `pom.xml`: Maven project with direct dependencies.

Viewing in Backstage
- Import or register the `catalog-info.yaml` component in your Backstage catalog (point to this repository or upload the file).
- Open the `user-service` component page to access:
  - **Docs / TechDocs** (served from `docs/` because `catalog-info.yaml` uses `backstage.io/techdocs-ref: dir:docs`).
  - **API** view (Backstage reads `api/openapi.yaml` via the `API` entity).

Useful commands
- Generate a Maven dependency tree (full transitive view):

```powershell
mvn dependency:tree -DoutputFile=dependency-tree.txt
```

- Validate the OpenAPI file locally with a linter (example using `swagger-cli`):

```powershell
swagger-cli validate api\openapi.yaml
```

If you want, I can add a CI step to auto-generate the dependency report and publish TechDocs on changes.
