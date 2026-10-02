# ControlPlane — APIs as Code

Specs for the LuminaTech POC, managed from Git and imported into Boomi API Control Plane.

| File | Purpose |
|---|---|
| `open-proxy.yaml` | Open Proxy Specification (required by ACP): API name, version, backend and gateway path |
| `openapi.json` | OpenAPI 3.0 contract, used by ACP for documentation and scoring |
| `.spectral.yaml` | Lint rules: Spectral OpenAPI + OWASP, the same families ACP scores against |
| `.github/workflows/lint.yml` | CI: lints every pull request, push and tag |
| `.github/workflows/publish-to-acp.yml` | CD: on a tag, publishes the spec to ACP and gates on its score |

## Flow
1. Change the spec by pull request; `lint.yml` must pass.
2. Tag the release (`v1.0.1`, `v2.0.1`, …).
3. `publish-to-acp.yml` publishes `openapi.json` as that version of **LuminaTech Device Management API** in Boomi API Control Plane, re-lints it there, and fails if quality or security drops below `ACP_MIN_SCORE` (default 90). The deployed `v1` is never touched.

To publish an existing tag: Actions → *Publish spec to API Control Plane* → Run workflow → enter the tag.

Repo secrets: `ACP_PAT` (ACP personal access token: `API_READ`, `API_WRITE`, `JOBS_READ`) and `ACP_BASE_URL` (`https://<tenant>.backend.<region>.controlplane.boomi.com`).

Why a pipeline rather than ACP's Git link: ACP attaches Git spec files only to Universal APIs. This API is discovered from Cloud API Management, so the pipeline uploads the spec through the ACP API instead.
