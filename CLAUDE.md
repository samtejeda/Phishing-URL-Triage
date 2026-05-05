# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project purpose

Self-hosted n8n workflow that triages suspicious URLs against VirusTotal, URLScan.io, and Google Safe Browsing, producing a structured verdict. The repo is a portfolio project — no application code, only Docker config, workflow exports, and documentation.

## Running locally

```bash
cp .env.example .env   # fill in credentials
docker compose up -d   # starts n8n on http://localhost:5678
docker compose logs -f # tail logs
docker compose down    # stop
```

n8n persists state in the `n8n_data` Docker volume. Workflow JSON files live in `workflows/` and are mounted read-only into the container at `/home/node/workflows`.

## Key files

- [docker-compose.yml](docker-compose.yml) — single-service Compose stack; basic auth and API keys sourced from `.env`
- [.env.example](.env.example) — canonical list of required env vars; `.env` is gitignored
- [workflows/](workflows/) — exported n8n workflow JSON files (import via n8n UI)
- [docs/screenshots/](docs/screenshots/) — supporting images for the README

## Env vars

| Variable | Purpose |
|---|---|
| `N8N_BASIC_AUTH_USER` | n8n UI username |
| `N8N_BASIC_AUTH_PASSWORD` | n8n UI password |
| `VIRUSTOTAL_API_KEY` | VirusTotal v3 API |
| `URLSCAN_API_KEY` | URLScan.io API |
| `GOOGLE_SAFE_BROWSING_API_KEY` | Google Safe Browsing v4 API |

## Custom slash commands

- `/docker-clean` — runs Docker disk hygiene (prune containers, images, volumes). Defined in [.claude/commands/docker-clean.md](.claude/commands/docker-clean.md). Safe to run anytime n8n is up; skips volume prune if n8n is stopped.

## Workflow export/import

Export a workflow from the n8n UI as JSON and save it to `workflows/<name>.json`. The file is purely declarative — no build step required. Import by dragging the file into the n8n canvas or using **Workflows → Import from file**.
