---
paths:
  - '**'
  - composer.json
  - package.json
  - compose.yaml
  - '.env*'
  - 'compose.yaml,.env*'
---

# General

## Keep Property Intelligence focused on independent buyer analysis
Build the MVP around Paste URL → Analyze → Property Report → Compare → at most two better alternatives. This is an independent information/analytics product, not a listing marketplace or agency; do not add brokerage, negotiation, deal-closing, marketplace, mobile app, microservices, CRM, social-network, unlimited-subscription, automatic-brokerage, or custom-ML scope without an explicit decision. Resolve tradeoffs in this order: correctness/provenance, user trust, legal/media safety, maintainable domain model, usefulness, performance, visual polish.

## Keep the application Laravel-first and dependencies justified
Keep the primary stack PHP, Laravel, PostgreSQL/PostGIS, Laravel queues, and Redis only when justified; stay in the Laravel frontend ecosystem absent a clear product reason. Python is not the application backend and may appear only as an isolated future ML/data-science service. Before adding a package or service, verify native/installed options, maintenance, license, security, and compatibility, and obtain approval for dependency changes.

## Keep the application Laravel-first and dependencies justified
Keep the primary stack PHP, Laravel, PostgreSQL/PostGIS, Laravel queues, and Redis only when justified; stay in the Laravel frontend ecosystem absent a clear product reason. Python is not the application backend and may appear only as an isolated future ML/data-science service. Before adding a package or service, verify native/installed options, maintenance, license, security, and compatibility, and obtain approval for dependency changes.

## Use PostgreSQL 18 with PostGIS 3.6 in Sail
Use `postgis/postgis:18-3.6` for the Sail `pgsql` service. PostgreSQL 18 stores its persistent volume at `/var/lib/postgresql`; do not revert it to the pre-18 `/var/lib/postgresql/data` path.

## Enable PostGIS in Sail's testing database
Mount `database/enable-postgis-testing.sql` after Sail's testing-database initializer so both the default and `testing` databases have the `postgis` extension.

## Inject production secrets at deployment runtime
Keep `.env` local and out of Git/images. Use `.env.example` for safe Sail defaults and `.env.production.example` only as a variable-name contract with empty secrets; inject real production values through the deployment platform. Add a Docker secret-file adapter only if a self-hosted Docker/Swarm target is explicitly chosen.

## Keep local PostgreSQL private without fake TLS
Use DB_SSLMODE=disable for local Sail because the PostGIS container has no configured TLS certificates; keep production TLS mandatory. Publish the local PostgreSQL port only on 127.0.0.1 when host database tools need access, while Laravel connects internally through pgsql:5432.
