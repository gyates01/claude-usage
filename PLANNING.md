# claude-usage — Planning

## Overview
Claude Code usage dashboard — forked from phuryn/claude-usage, moved to gyates01/claude-usage. Pure Python, no dependencies. Scans local JSONL session logs and serves a browser dashboard with token counts, cost estimates, and model breakdown.

## Phase Status

| Phase | Description | Status | Completed |
|---|---|---|---|
| 1 | Core: JSONL scanner, SQLite DB, CLI (`scan`, `today`, `stats`, `dashboard`) | ✅ Complete | 2026-04 |
| 2 | Dashboard: Chart.js charts, model filter, 30s auto-refresh, bookmarkable URLs | ✅ Complete | 2026-04 |
| 3 | Hub integration: repo moved to gyates01, hub card added | ✅ Complete | 2026-04-23 |

---

## Phase 1 — Core
Incremental JSONL scanner (path + mtime tracking). SQLite at `~/.claude/usage.db`. CLI subcommands: `scan`, `today`, `stats`, `dashboard`. Cost estimates using April 2026 API pricing for opus/sonnet/haiku.

---

## Phase 2 — Dashboard
Single-page HTML/JS served by `http.server`. Chart.js for visualizations. Model filter with bookmarkable URL params. Auto-refresh every 30 seconds. Captures token breakdown (input, output, cache write, cache read).

---

## Phase 3 — Hub Integration
Repo forked/moved from `phuryn/claude-usage` to `gyates01/claude-usage`. Hub card added: shows `python cli.py dashboard` command and links to localhost:8080 (local-only — no deploy needed).

---

## v2 Ideas (not scheduled)
- Update model pricing table when Anthropic releases new models / changes pricing
- Per-project breakdown (usage grouped by `~/.claude/projects/<project-name>`)
- Weekly/monthly trend charts
- Export to CSV
- Detect and flag unusually expensive sessions
