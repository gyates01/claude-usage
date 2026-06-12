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
- Cowork sessions are NOT captured — they run server-side and don't write local JSONL transcripts (this is why some models, e.g. Opus 4.8, may never appear)
- Model pricing in `dashboard.py` is hardcoded to June 2026 API prices — update when Anthropic changes pricing
- Cache writes are priced by TTL: 5-minute writes at 1.25x input, 1-hour writes at 2x input (`cache_5m_tokens`/`cache_1h_tokens` columns); most Claude Code cache writes are 1h
- Claude Code auto-deletes transcripts after ~30 days — the DB is the only record of older usage. Never delete `usage.db` to rebuild; the Rescan endpoint merges instead (dedup via unique `message_id` index)
- Every scan makes a daily rotating backup to `~/.claude/usage-backups/` (keeps last 7) via `scanner.backup_db()`
- On macOS/Linux use `python3`; on Windows use `python`
- Hub card shows `python cli.py dashboard` + localhost:8080 (local-only project, no deploy)
