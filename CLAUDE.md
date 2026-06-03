# claude-usage — Claude Instructions

## Skill routing
- Bugs → invoke `superpowers:systematic-debugging`
- Model pricing updates → edit `dashboard.py` cost table directly (no skill needed)

## Run commands
No install, no venv:
```bash
cd "H:/Other/Claude Projects/claude-usage"
python cli.py dashboard    # scan + open http://localhost:8080
python cli.py scan         # scan only (incremental)
python cli.py today        # today's usage by model (terminal)
python cli.py stats        # all-time stats (terminal)
```

## Stack conventions
- Pure Python standard library only (`sqlite3`, `http.server`, `json`, `pathlib`) — no pip installs, no venv
- DB at `~/.claude/usage.db` — shared across machines, not committed to repo
- Scanner is incremental: tracks file path + mtime, only processes new/changed JSONL files
- Dashboard auto-refreshes every 30 seconds; model filter state is bookmarkable via URL params

## Architecture
- `scanner.py` — parses `~/.claude/projects/**/*.jsonl`, writes to `~/.claude/usage.db`
- `dashboard.py` — HTTP server + single-page HTML/JS dashboard (Chart.js from CDN)
- `cli.py` — entry point for all commands

## Known gotchas
- Cowork sessions are NOT captured — they run server-side and don't write local JSONL transcripts
- Model pricing in `dashboard.py` is hardcoded to April 2026 API prices — update when Anthropic changes pricing
- On macOS/Linux use `python3`; on Windows use `python`
- Hub card shows `python cli.py dashboard` + localhost:8080 (local-only project, no deploy)
