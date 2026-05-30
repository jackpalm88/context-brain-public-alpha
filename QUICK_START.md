# Context Brain — Quick Start (Public Alpha)

Context Brain is a semantic memory and knowledge graph API. It stores, retrieves, and connects knowledge chunks using embeddings, hub-based navigation, and governance-scored saves.

This guide gets you running locally with Docker and a synthetic demo dataset.

---

## Prerequisites

- Docker and Docker Compose (v2+)
- `bash` (for the demo seed script)
- `curl` (for health checks and API calls)
- An OpenAI API key (required for embeddings)
- An Anthropic API key (optional — used for some scoring features)

---

## 1. Clone the repository

```bash
git clone https://github.com/your-org/context-brain.git
cd context-brain
```

---

## 2. Create your environment file

```bash
cp .env.example .env
```

Open `.env` and fill in the required values:

| Variable | Required | Description |
|---|---|---|
| `OPENAI_API_KEY` | ✅ | OpenAI key for embeddings |
| `ANTHROPIC_API_KEY` | Optional | Anthropic key for scoring features |
| `DATABASE_URL` | ✅ | Postgres connection string |
| `POSTGRES_DB` | ✅ | Database name (default: `contentingestor`) |
| `POSTGRES_PASSWORD` | ✅ | Postgres password |
| `ADMIN_API_KEY` | ✅ | API key for admin endpoints |
| `API_PORT` | Optional | Port to expose (default: `8000`) |

**Do not commit `.env` to version control.**

---

## 3. Constitution rules (optional)

Context Brain supports a local governance config file for controlling save thresholds and scoring behaviour.

This file is **private and local** — it is excluded from Docker builds and should never be committed.

To use it:

```bash
cp constitutionrules.example.yaml constitutionrules.yaml
# Edit constitutionrules.yaml with your preferred thresholds
```

If `constitutionrules.yaml` is not present, the application uses built-in defaults.

To mount it into the running container:

```bash
# Add to docker-compose.yml volumes section under the api service (manual step):
# - ./constitutionrules.yaml:/app/constitutionrules.yaml
```

---

## 4. Start the services

```bash
docker compose up --build -d
```

This starts:
- `contentingestor-api` — the main API (port 8000 by default)
- `contentingestor-db` — PostgreSQL with pgvector

**pgAdmin is optional** and not started by default. See section 5 if you need it.

Wait ~10 seconds for the API to initialise, then run a health check:

```bash
curl -s http://localhost:8000/health | python3 -m json.tool
```

Expected response includes `"status": "healthy"`.

---

## 5. pgAdmin (optional — dev/debug only)

pgAdmin is gated behind the `devtools` profile and is not needed for normal use.

```bash
docker compose --profile devtools up -d pgadmin
```

Access at: `http://localhost:5050`

Login with `PGADMIN_EMAIL` and `PGADMIN_PASSWORD` from your `.env`.

---

## 6. Set up demo env vars

The seed script reads three environment variables. Set them in your shell before running it:

```bash
export CB_API_BASE=http://localhost:8000
export CB_USER_ID=<your-user-id>
export CB_WORKSPACE_ID=<your-workspace-id>
```

Replace `<your-user-id>` and `<your-workspace-id>` with real values from your Context Brain instance. These are returned during initial setup or can be retrieved from the API.

> These vars are also in `.env` under the `# --- Seed Demo (optional) ---` section for reference, but the seed script reads them from the shell environment directly.

---

## 7. Load synthetic demo data

```bash
bash seeds/demo/seed_demo.sh
```

This loads a small set of demo hubs, content items, edges, and decisions into your instance. It is idempotent — safe to run multiple times.

Expected output: a summary of hubs, content, edges, and decisions created.

---

## 8. Try your first query

```bash
curl -s -X POST http://localhost:8000/v1/ask \
  -H "Content-Type: application/json" \
  -d '{
    "query": "What is agent memory?",
    "user_id": "<your-user-id>",
    "workspace_id": "<your-workspace-id>",
    "language": "en",
    "top_k": 5
  }' | python3 -m json.tool
```

---

## 9. MCP integration (optional)

Context Brain exposes a stable MCP tool surface for agent integrations. The 12 stable tools are documented in `docs/mcp_contract_v1.md`.

To connect an MCP-compatible agent, point it at the running API and configure it with your `CB_USER_ID` and `CB_WORKSPACE_ID`.

---

## 10. Troubleshooting

**Health check fails or returns unhealthy**
- Check `docker compose logs contentingestor-api` for startup errors
- Verify `DATABASE_URL` and `POSTGRES_PASSWORD` are set correctly in `.env`
- Ensure `OPENAI_API_KEY` is valid — embedding calls fail silently if the key is missing

**Seed script fails with "Set CB_USER_ID"**
- You must `export` the vars before running the script (see section 6)

**pgAdmin not accessible**
- pgAdmin only starts with `--profile devtools` (see section 5)

**API returns 401 on admin endpoints**
- Set `ADMIN_API_KEY` in `.env` and pass it as `X-Admin-Key: <key>` header

---

## What not to do

- **Do not use private user IDs from examples** — always use your own `CB_USER_ID` and `CB_WORKSPACE_ID`
- **Do not commit `.env`** — it contains secrets
- **Do not commit `constitutionrules.yaml`** — it is private local config; `.gitignore` and `.dockerignore` exclude it by default
- **Do not run `docker compose down -v`** unless you intend to wipe all data — it deletes the Postgres volume

---

## What's next

- Read `docs/mcp_contract_v1.md` for the full MCP stable tool surface
- See `seeds/demo/` for the demo data format if you want to load your own content
- OpenAPI schema available at `http://localhost:8000/openapi.json` when the server is running

---

## Public-alpha boundary notes

- GPT Actions v1 scope is read-only.
- openapi.json is internal/generated reference; public GPT Actions use the dedicated public schema artifact.
- Demo seed guidance does not guarantee live runtime smoke across all environments.
