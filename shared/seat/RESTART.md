# Seat recovery (outline)

**Status:** design stub — fill commands when implementing.  
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
5. **Agent** — Claude Code installed; Claude plan auth present. Later also Codex / Cursor CLI / FM as added.  
6. **Repos** — skills repo present; KB repo present **on this seat**; `git pull` if policy says so.  
7. **Scheduler** — launchd (or agreed) job entries loaded; next Monday-style / test timer still armed.  
8. **Smoke** — from phone: Moshi → attach → Claude Code responds to a trivial prompt. Optional: trigger a tiny scheduled job once.  

## Later: `restore.sh`

Automate steps 2–6 checks and safe starts. Do **not** auto-run destructive git or re-auth. Keep smoke as a human or optional flag.

## Notes

- Closing Moshi on the phone must not be required to “fix” the seat — agents live on the mini.  
- Document real paths and service names here when they exist.  
- After every macOS update, run this checklist once and note anything that broke.
