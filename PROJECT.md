# claude-usage — Project

## Purpose & Goals
Local dashboard for Claude Code usage. Reads the JSONL session logs Claude Code writes to `~/.claude/projects/` and surfaces token counts, model breakdown, cost estimates, and session history in a browser UI. Fills the gap that Anthropic's own UI doesn't cover — especially useful on Pro and Max plans where the progress bar shows % used but not token-level detail.

## Target User
Personal use (Garrett). Also public on GitHub for any Claude Code user. Works on API, Pro, and Max plans.

## Tech Stack
| Layer | Choice |
|---|---|
| Core | Pure Python 3.8+ standard library only |
| Database | SQLite at `~/.claude/usage.db` |
| Dashboard | `http.server` + single-page HTML/JS (Chart.js from CDN) |
| Entry point | `cli.py` — `scan`, `today`, `stats`, `dashboard` subcommands |
| GitHub | gyates01/claude-usage |
| Deploy | Local only — `python cli.py dashboard` → localhost:8080 |

## Architecture Decisions

**Why standard library only (no pip, no venv)?**
Anyone running Claude Code already has Python. Adding a `requirements.txt` and venv setup creates friction for a tool that should be instant to run. The entire stack (`sqlite3`, `http.server`, `json`, `pathlib`) ships with Python 3.8+. Trade-off: no rich CLI output (no `rich` library), but the dashboard handles visualization.

**Why SQLite at `~/.claude/` instead of reading JSONL directly?**
JSONL files grow large — scanning everything on every dashboard load would be slow. SQLite lets the scanner run incrementally (file path + mtime tracking) so re-runs only process new/changed files. The dashboard queries are instant regardless of total history size.

**Why a fork of phuryn/claude-usage?**
Original upstream repo was `phuryn/claude-usage`. Forked and moved to `gyates01/claude-usage` for ownership and customization. Upstream may have diverged — this is now an independent fork.

## Key Dependencies & Gotchas
- No external packages — pure stdlib
- `~/.claude/usage.db` not committed to git (personal usage data)
- Cowork sessions not captured (server-side, no local JSONL)
- Model pricing table in `dashboard.py` is hardcoded — needs manual update when Anthropic changes API prices
- Hub card links to this as `python cli.py dashboard` + localhost:8080

## Lessons Learned
- **Incremental scanning is essential:** First version re-scanned all JSONL on every dashboard load. For users with months of history this was 2–5 seconds of blocking I/O. Switching to mtime-based incremental tracking made re-scans near-instant.
