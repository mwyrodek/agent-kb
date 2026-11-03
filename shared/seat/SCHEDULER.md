# Seat scheduler

How to add or change unattended jobs **from this Mac**: use the Claude skill `schedule-job` (`~/seat/skills/shared/schedule-job`).

## Idea

launchd fires a shell script → script runs **Claude Code** with the seat’s **Pro login** (`claude -p …`) → agent follows a topic `AGENTS.md` → writes KB → optionally git push.

Not API tokens. Not `--bare`.

## Live jobs

| Label | When | Script | Notes |
|-------|------|--------|-------|
| `com.agentseat.scheduler-smoke` | every 24h | `~/seat/scheduler/smoke.sh` | writes a timestamp file |
| `com.agentseat.job-search` | Mon 08:00 local | `~/seat/scheduler/job-search.sh` | Pro Claude; proven 2026-11-03 |

## Force a run

```bash
launchctl kickstart -k gui/$(id -u)/com.agentseat.job-search
tail -f ~/seat/scheduler/logs/job-search-last-run.txt
```

## After reboot

1. Unlock FileVault + desktop login (see `RESTART.md`).
2. Confirm: `launchctl print gui/$(id -u)/com.agentseat.job-search | grep state`
3. If missing: `bootstrap` the plist again (commands in `RESTART.md` / `schedule-job` skill).

## PC copies

Design-home mirrors (optional): `D:\git\MacAgent\ops\scheduler\` and `ops/RESTART.md`.
