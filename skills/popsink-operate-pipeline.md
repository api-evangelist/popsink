---
name: Operate and monitor a Popsink pipeline
description: Inspect, pause, resume, and read logs and status for an existing replication pipeline.
api: openapi/popsink-onprem-api-openapi-original.json
operations:
  - pipelines_list_pipelines__get
  - pipelines_get_pipelines__pipeline_id__get
  - pipelines_pause_pipelines__pipeline_id__pause_post
  - pipelines_start_pipelines__pipeline_id__start_post
  - pipelines_logs_pipelines__pipeline_id__logs_get
  - pipelines_count_status_pipelines_count_status_get
---

# Operate and monitor a Popsink pipeline

Day-2 operations for existing Popsink replication pipelines.

## Steps
1. **Authenticate** with a JWT bearer token (`POST /auth/jwt/login`).
2. **Find the pipeline.** `GET /pipelines/` (`pipelines_list_pipelines__get`), paginated with `page`/`size`, or fetch one directly with `GET /pipelines/{pipeline_id}` (`pipelines_get_pipelines__pipeline_id__get`).
3. **Pause / resume.** `POST /pipelines/{pipeline_id}/pause` (`pipelines_pause_pipelines__pipeline_id__pause_post`) to stop replication; `POST /pipelines/{pipeline_id}/start` (`pipelines_start_pipelines__pipeline_id__start_post`) to resume.
4. **Read logs.** `GET /pipelines/{pipeline_id}/logs` (`pipelines_logs_pipelines__pipeline_id__logs_get`).
5. **Fleet overview.** `GET /pipelines/count-status` (`pipelines_count_status_pipelines_count_status_get`) returns counts by status across all pipelines.

## Rules
- Pause/start are state transitions with side effects — verify current state via `GET /pipelines/{pipeline_id}/status` before acting.
- Same FastAPI error envelope as the rest of the API (see `conventions/popsink-conventions.yml`).
