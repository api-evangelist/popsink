---
name: Create a Popsink replication pipeline
description: Authenticate, pick a source and target connector, register a CDC replication pipeline, and start it.
api: openapi/popsink-onprem-api-openapi-original.json
operations:
  - auth_jwt_login_auth_jwt_login_post
  - list_source_config_connectors_source_config_get
  - list_target_config_connectors_target_config_get
  - pipelines_register_pipelines__post
  - pipelines_start_pipelines__pipeline_id__start_post
  - pipelines_status_pipelines__pipeline_id__status_get
---

# Create a Popsink replication pipeline

Use the Popsink control-plane API to stand up a real-time (CDC) replication pipeline from a source system to a target.

## Prerequisites
- Base URL is the on-prem deployment host with the API mounted under `/api`.
- Auth is OAuth2 password flow issuing a JWT bearer token.

## Steps
1. **Authenticate.** `POST /auth/jwt/login` (`auth_jwt_login_auth_jwt_login_post`) with your username/password (form body). Capture the returned bearer token and send it as `Authorization: Bearer <token>` on every subsequent call.
2. **List available connectors.** `GET /connectors/source-config` (`list_source_config_connectors_source_config_get`) and `GET /connectors/target-config` (`list_target_config_connectors_target_config_get`) to choose the source and target connector configurations.
3. **Register the pipeline.** `POST /pipelines/` (`pipelines_register_pipelines__post`) with the source/target configuration in the `PipelineCreate` body. It is created in a draft state.
4. **Start it.** `POST /pipelines/{pipeline_id}/start` (`pipelines_start_pipelines__pipeline_id__start_post`).
5. **Confirm it is running.** Poll `GET /pipelines/{pipeline_id}/status` (`pipelines_status_pipelines__pipeline_id__status_get`).

## Rules
- Pagination on list endpoints uses `page` + `size` query params (`Page_*` envelope).
- There is no `Idempotency-Key` header; do not assume safe retries on `POST /pipelines/`. Check `GET /pipelines/` before re-creating.
- Errors are FastAPI-style: `401` unauthenticated, `403` insufficient role, `404` unknown id, `422` validation (see `errors/popsink-problem-types.yml`).
