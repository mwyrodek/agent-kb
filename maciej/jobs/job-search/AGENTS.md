# Topic: job search (maciej)

## RULE #1: NEVER APPLY
Never apply to any job, never fill application forms, never send CVs or messages to recruiters. Only find, rank and list offers. Maciej applies himself.

## Goal
Find QA Architect / Test Architect / QA Lead (or similar senior QA) roles that match `criteria.md`, and keep a clean shortlist Maciej can act on.

## Schedule
Weekly, Mondays. Also on demand ("continue job search"). No scheduler job yet; run manually.

## How to run
1. Read `criteria.md`, `sources.md`, and the latest entries in `log.md`.
2. Search each source in `sources.md` for the roles in `criteria.md`.
3. Dedupe against `shortlist.md`, including its "Dropped" section:
   - Same offer = same company + same title + same pay range and stack (city may differ).
   - Found on another site → add that link to the existing entry ("also: <url>"); don't create a new entry.
   - Software house / agency with the same generic title but a different client, pay or stack → treat it as a different offer and add it.
   - Unsure → add it with the flag "dup?".
4. Add new matches to the top of `shortlist.md`, under a dated heading, using the format there.
5. Write a dated entry in `log.md`: sources checked, counts, anything notable, broken sources.
6. Update the row in `../../TOPICS.md` (last activity date).
7. Commit and push the KB (`agent-kb`). Maciej allowed pushing without asking for job search runs (2026-11-02).
8. Report to Maciej in short ADHD style: count of new offers, then every new offer with its plain full URL on its own line (he often reads over SSH, where markdown links are not clickable).

## Files
- `criteria.md`: what counts as a match.
- `sources.md`: where to look.
- `shortlist.md`: kept offers, newest first.
- `log.md`: run history.
