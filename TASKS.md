# claude-usage — Active Tasks

_No active feature work. Project is stable and complete._

## Maintenance

- [x] Ingest TradingAgents usage logs — added `TRADINGAGENTS_LOG_DIR = ~/.tradingagents/usage-log` to `DEFAULT_PROJECTS_DIRS` in `scanner.py` (2026-06-02)
- [ ] Update model pricing table in `dashboard.py` when Anthropic changes API prices (currently hardcoded to April 2026 rates)
- [ ] Check upstream `phuryn/claude-usage` for any significant changes worth pulling in (incremental scanner improvements, new model support)
- [x] Verify TradingAgents row appears in dashboard after first run post-2026-06-02 (confirmed 2026-06-03)
- [x] Add claude-opus-4-7 and claude-opus-4-8 to pricing table in dashboard.py (2026-06-03)
