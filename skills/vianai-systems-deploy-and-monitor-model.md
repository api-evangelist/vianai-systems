---
name: Register, deploy, and monitor a model
description: Register a model and version with hila, deploy it, then run drift detection and an alert to monitor it.
api: https://docs.vian.ai/5.0-r2/api-list.html
operations:
  - POST /v1/user/login
  - POST /v1/model_deployment/createRegisteredModel
  - POST /v1/model_deployment/registerModelVersion
  - POST /v1/model_deployment/createModelDeployment
  - POST /v1/driftdetection/submit
  - GET /v1/driftdetection/status/{job_id}
  - POST /v1/alerts/submit
---

# Register, deploy, and monitor a model

Deploy a machine-learning model to hila and set up ongoing monitoring. All calls go to
your deployment's `webservices` host and require the bearer token from login.

## Steps

1. **Authenticate** — `POST /v1/user/login`; capture `access_token` and send it on the
   `Authorization` header for every following call.
2. **Register the model** — `POST /v1/model_deployment/createRegisteredModel`.
3. **Register a version** — `POST /v1/model_deployment/registerModelVersion` for the
   model created in step 2.
4. **Deploy it** — `POST /v1/model_deployment/createModelDeployment`.
5. **Run drift detection** — `POST /v1/driftdetection/submit`; the response returns a
   `job_id`.
6. **Poll the job** — `GET /v1/driftdetection/status/{job_id}` until it completes.
7. **Create an alert** — `POST /v1/alerts/submit` to be notified when monitoring
   thresholds are crossed.

## Conventions

- Async jobs follow submit → status → cancel: `POST .../submit` returns a `job_id`,
  `GET .../status/{job_id}` polls, `DELETE .../cancel/{job_id}` cancels.
- Auth and versioning as in `conventions/vianai-systems-conventions.yml`.

> Grounded in the published hila api-list. Request/response schemas are served by the
> per-deployment OpenAPI at `/docs` and are not reproduced here.
