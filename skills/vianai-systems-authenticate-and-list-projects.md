---
name: Authenticate to hila and list projects
description: Obtain a bearer token from the hila REST API and use it to list the projects in your stack.
api: https://docs.vian.ai/5.0-r2/api-quickstart.html
operations:
  - POST /v1/user/login
  - POST /v1/projects/search
---

# Authenticate to hila and list projects

The hila REST API is served per deployment from the `webservices` subdomain of your
stack, e.g. `https://webservices.<STACK>.vianai.site`. Most calls require a bearer
access token.

## Steps

1. **Get an access token** — `POST /v1/user/login` with a JSON body
   `{"username": "USERNAME", "password": "PASSWORD"}`. Read `access_token` from the
   response and keep it for subsequent calls.
2. **List projects** — `POST /v1/projects/search`, sending the token on the
   `Authorization` header. The response contains the projects in your stack.

## Conventions

- Auth: bearer `access_token` on the `Authorization` header (see
  `authentication/vianai-systems-authentication.yml`).
- Versioning: all paths are under `/v1/`.
- List/query operations use `POST .../search` bodies, not GET query strings.

## Example

```bash
token=$(curl -s --location 'https://webservices.<STACK>.vianai.site/v1/user/login' \
  --header 'Content-Type: application/json' \
  --data-raw '{"username":"USERNAME","password":"PASSWORD"}' | jq -r '.access_token')

curl --location 'https://webservices.<STACK>.vianai.site/v1/projects/search' \
  --header 'Content-Type: application/json' \
  --header "Authorization: $token"
```
