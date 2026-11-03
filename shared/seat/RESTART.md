# Seat recovery

**Status:** live checklist (reboot tested 2026-11-02; scheduler smoke 2026-11-03).  
**Goal:** after reboot / macOS update / stack death, restore a known-good seat.

## When to use

- macOS update or reboot  
- Logout / crash  
- Herdr (or equivalent) / `moshi-hook` / Tailscale stopped  
- Skills or KB checkout missing or stale  

## Ordered checklist (manual first)

1. **Machine** — Mac mini powered on; seat user session available (auto-login if that becomes policy).  
2. **Network** — Tailscale connected; phone can see the node.  
3. **Runtime** — durable agent host up (Herdr-core / agreed multiplexer).  
4. **Phone door** — `moshi-hook` (or agreed equivalent) running and paired.  
5. **Agent** — Claude Code installed; Claude plan auth present. Later also Codex / Cursor CLI / FM as added. Run `~/seat/skills/link-skills.sh`; it reports clashes and broken links.  
6. **Repos** — skills repo present; KB repo present **on this seat**; `git pull` if policy says so.  
7. **Scheduler** — see § Scheduler below.  
8. **Smoke** — from phone: Moshi → attach → Claude Code responds to a trivial prompt. Optional: kickstart the scheduler smoke job.  

## Verified after a real reboot (2026-11-02)

Seat details: user `agentseat`, LAN `192.168.50.249`, Tailscale `100.86.191.33` (`mac-mini-anna`). **FileVault is ON and automatic login is OFF on purpose** (stolen-Mac risk). So a reboot needs a person.

After any reboot or macOS update:

1. **Unlock the disk.** From a PC on the home LAN (or remote into another home PC first): `ssh agentseat@192.168.50.249`, type the Mac password when it says the system is locked. Tailscale is not up yet, so use the LAN address.
2. **Log in at the desktop.** Someone with the password logs in at the Mac's screen (Anna can). An SSH login is **not** enough: Tailscale and Moshi start only from a desktop login.
3. **Tailscale did NOT reconnect by itself** (app was open but "stopped"). Fix over SSH: `/Applications/Tailscale.app/Contents/MacOS/Tailscale up`. Check why "Launch at login" did not connect it.
4. **Moshi daemon** started by itself and stayed paired (`moshi-hook doctor`).
5. **Herdr panes are lost** on reboot (server not running). From the PC: `ssh -t agentseat@100.86.191.33 "cd ~/seat && herdr"` (terminal Herdr, the one Moshi Chat View can see; **not** the Herdr desktop app). In a pane run `cd ~/seat && claude` (start in `~/seat` so it reads `AGENTS.md`). On the phone, attach through Moshi's session picker (Herdr tab), not "Skip".

Open gap: logging in at the desktop **remotely**. Ideas to test: Screen Sharing at the login screen; or run Tailscale and the Moshi daemon as system services that start without a login.

## Scheduler (step 5 smoke — 2026-11-03)

LaunchAgents only run when `agentseat` has a **desktop login** (FileVault + no auto-login). After reboot, finish the login steps above first.

| Piece | Path |
|-------|------|
| Script | `~/seat/scheduler/smoke.sh` |
| Output | `~/seat/scheduler/smoke-last-run.txt` |
| Logs | `~/seat/scheduler/logs/` |
| Plist | `~/Library/LaunchAgents/com.agentseat.scheduler-smoke.plist` |
| PC copies | `ops/scheduler/` in this repo |

Reload + force one run **[SSH OK]** (user must already be logged in at the Mac):

```bash
launchctl bootout gui/$(id -u)/com.agentseat.scheduler-smoke 2>/dev/null || true
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.agentseat.scheduler-smoke.plist
launchctl kickstart -k gui/$(id -u)/com.agentseat.scheduler-smoke
cat ~/seat/scheduler/smoke-last-run.txt
```

Expect a fresh UTC timestamp and `ok`. Job is armed every 86400s (`StartInterval`).

### Job search (Mondays 08:00 local) — 2026-11-03

| Piece | Path |
|-------|------|
| Script | `~/seat/scheduler/job-search.sh` |
| Plist | `~/Library/LaunchAgents/com.agentseat.job-search.plist` |
| Logs | `~/seat/scheduler/logs/job-search-*.log`, `job-search-last-run.txt` |
| Topic | `~/seat/kb/maciej/jobs/job-search/` |
| Runner | Claude Code `-p` on **Pro OAuth** (`bypassPermissions`; not API key / not `--bare`) |

```bash
launchctl bootout gui/$(id -u)/com.agentseat.job-search 2>/dev/null || true
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.agentseat.job-search.plist
# optional force run now (uses Pro quota):
launchctl kickstart -k gui/$(id -u)/com.agentseat.job-search
```

Monday news digest still not wired.

## Later: `restore.sh`

Automate steps 2–6 checks and safe starts. Do **not** auto-run destructive git or re-auth. Keep smoke as a human or optional flag.

## Notes

- Closing Moshi on the phone must not be required to “fix” the seat — agents live on the mini.  
- Document real paths and service names here when they exist.  
- After every macOS update, run this checklist once and note anything that broke.
