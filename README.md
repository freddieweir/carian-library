# Carian Library

External knowledge seekers — search, media, and AI services with controlled internet egress.

Part of [Carian Stacks](https://github.com/freddieweir/carian-stacks) |
[Observatory](https://github.com/freddieweir/carian-observatory) · **Library** · [Manor](https://github.com/freddieweir/carian-manor)

## What Library Does

| Capability | Service |
|------------|---------|
| AI search | Perplexica + SearXNG metasearch backend |
| Canary testing | Open-WebUI latest tag with hourly auto-updates |
| Privacy YouTube | Invidious + Companion playback proxy |
| RSS dashboard | Glance — feeds, system status, Docker monitoring |
| AI notebook | Open Notebook — self-hosted NotebookLM alternative |

## Services

| Container | Image | Role |
|-----------|-------|------|
| `cl-perplexica-service` | `itzcrazykns1337/perplexica` | AI-powered search engine |
| `cl-perplexica-searxng` | `searxng/searxng` | Privacy-focused metasearch backend |
| `cl-invidious-service` | `quay.io/invidious/invidious` | Privacy-respecting YouTube frontend |
| `cl-invidious-companion-service` | `quay.io/invidious/invidious-companion` | YouTube playback proxy |
| `cl-invidious-db-service` | `postgres:14` | PostgreSQL for Invidious |
| `cl-glance-service` | `glanceapp/glance` | RSS dashboard and system status |
| `cl-opennotebook-service` | `lfnovo/open_notebook` | Self-hosted NotebookLM alternative |
| `cl-opennotebook-db` | `surrealdb/surrealdb:v2` | SurrealDB for Open Notebook |
| `cl-open-webui-canary` | `ghcr.io/open-webui/open-webui` | Open-WebUI canary testing |

## Security Tier

Library is the **controlled egress** stack:

| Property | Detail |
|----------|--------|
| Internet access | Yes — services make external API calls (search, video, LLM APIs) |
| Inbound traffic | Proxied through Observatory's nginx (no direct exposure) |
| Authentication | Enforced by Manor's Authelia via `carian-shared` network |
| Auto-updates | Canary gets hourly updates from Observatory's watchtower |
| Network | Joins `carian-shared` (created by Observatory) |

## Quick Start

### Prerequisites

- Observatory must be running (creates `carian-shared` network)
- Docker Compose v2.20+
- 1Password CLI for secret injection

### Deploy

```bash
# 1. Configure environment
cp .env.example .env && vim .env

# 2. Inject secrets from 1Password
op inject -f -i .env -o .env.resolved

# 3. Start Library services
docker compose --env-file .env.resolved up -d

# 4. Verify
docker compose ps
```

## Deploy Order

```
1. Observatory  →  creates carian-shared network (must be first)
2. Manor        →  starts auth services
3. Library      →  joins network, starts knowledge services
```

---

<sub>Named after the [Academy of Raya Lucaria](https://eldenring.wiki.fextralife.com/Academy+of+Raya+Lucaria) from Elden Ring — the grand library whose scholars ventured beyond the estate walls in pursuit of forbidden knowledge.</sub>
