# Cadlens API — Postman Collection

Official Postman collection for the [Cadlens CAD parsing API](https://cadlens.co).

Import this collection to explore and test all Cadlens API endpoints directly from Postman — no code required.

- API Docs: [cadlens.co/docs](https://cadlens.co/docs)
- Dashboard: [cadlens.co/dashboard](https://cadlens.co/dashboard)
- Pricing: [cadlens.co/pricing](https://cadlens.co/pricing)

---

## Import into Postman

1. Download [`cadlens-api.postman_collection.json`](cadlens-api.postman_collection.json)
2. Open Postman → **Import** → drag the file in
3. Set the `apiKey` collection variable to your Cadlens API key (get one at [cadlens.co](https://cadlens.co))

---

## Collection Variables

| Variable | Description |
|----------|-------------|
| `baseUrl` | API base URL (`https://api.cadlens.co`) |
| `jwt` | JWT token from login (set automatically by the auth request) |
| `apiKey` | Your Cadlens API key — paste your `cadl_xxx` key here |
| `jobId` | Populated automatically after a parse request |
| `keyId` | Populated automatically after creating an API key |

---

## Quickstart Flow

1. **Auth → Exchange OAuth code** — authenticate and get a JWT
2. **Keys → Create API Key** — generate a `cadl_xxx` API key; copy it to the `apiKey` variable
3. **Parse → Upload CAD File** — upload a DWG/DXF file; `jobId` is saved automatically
4. **Jobs → Get Job Status** — poll until `status` is `COMPLETED`
5. **Jobs → Get Job Result** — retrieve `vectorJson`, `layersJson`, and `metadata`
6. **Jobs → Get Preview Image** — get a presigned PNG preview URL

---

## Endpoints Covered

| Group | Endpoint |
|-------|----------|
| Auth | POST /v1/auth/google/exchange, POST /v1/auth/github/exchange, GET /v1/auth/me |
| Parse | POST /v1/parse |
| Jobs | GET /v1/jobs, GET /v1/jobs/:jobId, GET /v1/jobs/:jobId/result, GET /v1/jobs/:jobId/image, DELETE /v1/jobs/:jobId |
| Keys | GET /v1/keys, POST /v1/keys, DELETE /v1/keys/:keyId |
| Billing | POST /v1/billing/portal |
| Health | GET /health |

---

## Links

- [Cadlens CAD parsing API](https://cadlens.co)
- [API docs](https://cadlens.co/docs)
- [Sign up for free](https://cadlens.co)

---

## GitHub Topics

Add these topics to this repo for discovery:
`postman` `api-testing` `cad` `dwg` `dxf` `cad-api` `openapi` `developer-tools`
