# OpenAPI documents

| File | Description |
|------|-------------|
| `verification_api.json` | OpenAPI 3.0.3 document for the DIDWW Verification API, JSON |
| `verification_api.yaml` | The same document, YAML |

Both files describe the same API version — named in [`../VERSION`](../VERSION) and in
`info.version`. They are generated from one source and validated against each other in CI.

These paths are stable across releases: each release overwrites the files in place. See the
[repository README](../README.md) for raw URLs, authentication and what the document covers.
