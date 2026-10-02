# lumina-device-api — APIs as Code

Specs for the LuminaTech POC, managed from Git and imported into Boomi API Control Plane.

| File | Purpose |
|---|---|
| `open-proxy.yaml` | Open Proxy Specification (required by ACP): API name, version, backend and gateway path |
| `openapi.json` | OpenAPI 3.0 contract, used by ACP for documentation and scoring |
| `.spectral.yaml` | Lint rules: Spectral OpenAPI + OWASP, the same families ACP scores against |
| `.github/workflows/lint.yml` | CI: lints every pull request, push and tag |

## Flow
1. Change the spec by pull request; CI must pass.
2. Tag the release (`v1.0.0`, `v2.0.0`, …). ACP imports **each tag as a version** named after the tag.
3. In ACP: create the API with **Create new API from Git**, or on an existing Git API run **Actions → Scan the Git repository for tags**.
