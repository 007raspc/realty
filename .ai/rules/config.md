---
paths:
  - 'config/**'
---

# Config

## Keep the application Laravel-first and dependencies justified
Keep the primary stack PHP, Laravel, PostgreSQL/PostGIS, Laravel queues, and Redis only when justified; stay in the Laravel frontend ecosystem absent a clear product reason. Python is not the application backend and may appear only as an isolated future ML/data-science service. Before adding a package or service, verify native/installed options, maintenance, license, security, and compatibility, and obtain approval for dependency changes.

## Inject production secrets at deployment runtime
Keep `.env` local and out of Git/images. Use `.env.example` for safe Sail defaults and `.env.production.example` only as a variable-name contract with empty secrets; inject real production values through the deployment platform. Add a Docker secret-file adapter only if a self-hosted Docker/Swarm target is explicitly chosen.
