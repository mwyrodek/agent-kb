# MacAgent design brief

**Status:** idea / design only — no implementation committed.  
**Updated:** 2026-11-01 (Herdr + Claude Code first; wife no-provider-choice front)  
**Seat:** spare Mac mini (always on for the experiment)  
**Origin:** shaped while studying MEGA.dev S01 harness / remote / skills ideas; adapted for personal use (not a course Challenge deliverable).

**Source of truth for the live plan:** this file + `ops/RESTART.md` + `README.md`.  
`reference/lessons/` and `reference/mindmaps/` are **MEGA course copies** — they still talk about **Pi**; that is historical study material, **not** our near-term stack (Herdr + Claude Code).

---

## 1. One-line idea

An always-on **Mac mini agent seat**: one machine, one macOS account, **Herdr** as durable runtime, **Moshi** as the phone door. **Near-term agent path: Claude Code** (uses Claude plan properly). Later extend with **OpenAI / Codex**, **Cursor CLI**, and **local Mac FM**. Skills and knowledge live in **two private git repos**. Scheduled jobs required. Wife **starts on Claude Code only**; later **Proposal A** (facade-per-chat) so she still never picks providers when more engines exist.

---

## 2. Picture

```text
Phones (Moshi + Chat View)          Scheduler (launchd / cron-class)
        │                                    │
   Tailscale                                 │ wakes / starts job
        │                                    ▼
Mac mini — one macOS account — always on (experiment)
        │
   Herdr (durable panes)
        │
   Agents (phased)
        ├── now:     Claude Code
        ├── later:   OpenAI / Codex, Cursor CLI, local FM
        ├── Coding desk (you)           ← day one
        ├── Research desk (wife)        ← Front 1: Claude only → later Proposal A
        ├── Family ops desk             ← Front 2, later
        ├── Scheduled jobs
        ├── skills repo / kb repo
        └── ops restore
```

Someone can use the Mac at the keyboard (GUI). At the same time, remote Moshi/SSH sessions run agents in the **background** for the same account. No second macOS login for “wife vs you.”

**Scheduled work** runs on the seat without anyone chatting: timer fires → skill/criteria → agent in Herdr → result in KB/file/notify.

**Billing note:** Pi + Claude OAuth draws **extra usage** (not Claude plan). Prefer **Claude Code** for Claude-plan work. OpenAI ChatGPT login in other tools often **does** use plan usage — different rule.

---

## 3. Locked decisions

| Topic | Decision |
|-------|----------|
| Hardware | Mac mini as seat (not cloud VPS for foundation) |
| Power | Stay on for experiment; sleep / Wake-on-LAN **parked** |
| Network | Tailscale for reachability |
| macOS accounts | **One** account; person/desk identity in **our** setup |
| Concurrent use | Background agent sessions while someone uses the GUI — intended |
| Durable runtime | **Herdr terminal (TUI, `herdr` 0.9.5)** — not the Herdr desktop app. Verified 2026-11-02: Moshi Chat View sees agents in the TUI server only; panes in the app's server (`herdr-core` / `hc`) are invisible to Moshi. PC access: `ssh -t agentseat@100.86.191.33 "cd ~/seat && herdr"`. The Herdr app is installed on the PC but unused for the seat. |
| Phone | **Moshi** (+ Chat View) |
| Agent rollout | **1) Claude Code** → **2) OpenAI/Codex** → **3) Cursor CLI** → **4) local Mac FM** |
| Wife later | Moshi + same seat; **v1 = Claude Code only** (no picker). **Later = Proposal A** facade-per-chat router (Front 9) |
| Skills storage | **Private skills git repo** — reusable on other machines |
| Knowledge storage | **Separate private KB repo** — not cloned everywhere with skills |
| Model billing | Routes must match provider rules (Claude plan via Claude Code; ChatGPT plan via supported login paths; API/OpenRouter when intentional) |
| Course Limen / Ubuntu VPS | Optional inspiration later — **not** foundation |
| Scheduled / triggered jobs | **Required capability** |
| Early content | **Written KB** + **retro skill** |
| Implementation | **Not started** |

---

## 4. Desks and timing

| Desk | Who | When | Role |
|------|-----|------|------|
| Coding | You | **Day one** | Code, harness, multi-model workers |
| Research | Wife | **Front 1 (later)** | Scientific / her work; her skill priority. **Start:** Claude Code only. **Later:** Proposal A facade-per-chat |
| Family ops | Both | **Front 2 (later)** | Plans, reminders, follow-ups; **no calendar** at first |

**Rules**

- Soft walls: namespaces and session identity, not separate Mac users.  
- Sharing is allowed (not a hide-everything vault).  
- Personal packs win on **name clash** for that person’s desk.  
- Family/shared desk: **shared first**; if not enough, **you + wife packs at equal weight**.

---

## 5. Skill and knowledge priority

Namespaces in **both** repos: `shared/`, `maciej/`, `anna/` (and later `family/` if useful — not created yet).

### Personal desk (you or wife)

```text
1. that person’s pack
2. shared
3. the other person’s pack — only if explicitly allowed for that session
```

Example: she asks → her `literature-review` beats yours if both exist.

### Family / shared desk

```text
1. shared
2. if that is not enough → your personal + wife’s personal, equal weight
```

---

## 6. Two private repos

| Repo | Contents | Sync policy |
|------|----------|-------------|
| **Skills** | Portable `SKILL.md` procedures and helpers | Clone/pull on **any** machine that needs the craft |
| **KB** | Growing notes / memory / research store | Seat first; **do not** require full KB on every skills machine |

**Built 2026-11-01:** `github.com/mwyrodek/agent-skills` and `agent-kb` (private), cloned on the seat to `~/seat/skills` and `~/seat/kb`. Each has `shared/`, `maciej/`, `anna/` and a README. Seat auth: fine-grained GitHub token limited to these two repos (Contents read/write; expiry not recorded here — check github.com/settings/personal-access-tokens, token `mac-seat`).

Open detail: exact folder layout inside each repo beyond the namespaces. Priority rules above apply regardless of layout.

Course-aligned idea: harness-agnostic skills (git / later npx skills / Skills-over-MCP); KB as a separate evolving system.

---

## 7. Agent / subscription rollout

| Phase | Agent | Why |
|-------|--------|-----|
| **Now** | **Claude Code** in Herdr | Uses Claude Pro/Max **plan** correctly (unlike Pi+Claude OAuth extra usage) |
| **Next** | **OpenAI / Codex** in Herdr | ChatGPT plan path; handoff / second capacity |
| **Then** | **Cursor CLI** | Another installed CLI in the farm |
| **Then** | **Local Mac FM** | Cheap/private light tasks |

**You (power user)** may still pick tools while building.

**Wife path (phased)**  
1. **Start (when Front 1 opens):** **Claude Code only** via Moshi/Herdr — no provider choice. Multiple chats per topic are fine (context limits).  
2. **Later (Front 9 / Proposal A):** facade-per-chat router — still no provider menu; seat may route Claude / OpenAI / Cursor / FM underneath. Many chats still allowed; A ≠ one forever thread.

**Adding any agent:** run `~/seat/skills/link-skills.sh` so the agent sees our skills (each agent reads skills from its own folder; the repo stays the only copy). Verify the agent's skills path first, then add it to `TARGETS` in the script.

**Pi** is **not** the near-term seat agent. Optional later for multiprovider experiments only. Mentions of Pi elsewhere in this brief are billing warnings or “later / optional,” not the setup target.

---

## 8. Phone door (Moshi)

- Moshi connects to the mini over Tailscale/SSH (local-first; agents stay on the seat).  
- **Chat View** presents the live agent as phone chat (agent in **Herdr** pane; `moshi-hook` on host). Near-term: Claude Code.  
- Same phone stack for you (day one) and wife (later, with Front 9 routing).  
- Chat View may need Pro / trial limits — note at implement time.

---

## 9. Runtime and durability

- **Herdr** durable panes: close phone/laptop ≠ kill agents.  
- Course “close the lid” = you are away; Mac mini has **no lid** — seat simply stays on.  
- Later: handoff / spawn across Claude Code, Codex, Cursor CLI, FM (Front 9 + phase 10).

Limen-style job folders / worktrees: **expansion socket**, not day-one requirement.

---

## 9b. Scheduled and triggered jobs

**Intent:** the seat is not only “chat when I’m on Moshi.” It must be able to **run work on a clock** (and later on events).

**Example (design target):** every **Monday morning**, prepare a list of **interesting news / topics** from stated **criteria** (skill + maybe KB preferences), and leave a digest you can open on the phone.

| Piece | Role |
|-------|------|
| Timer | macOS `launchd` (or equivalent) — seat is always on, so local schedule is enough for v1 |
| Job definition | Skill + criteria (in skills repo); optional preferences in KB |
| Runner | Start agent in Herdr (near-term Claude Code) — unattended |
| Output | Write digest to KB (or agreed path); optional notify (Moshi / ntfy later) |
| Desk | Usually **you** or **shared** criteria — not wife day one |

**Design rules**

- Schedule ≠ calendar sync (Front 2 calendar stays separate).  
- Jobs use the same skill/KB priority as the desk they belong to.  
- Unattended jobs need clear **tool allowlists / approvals policy** later (no silent destructive shell).  
- Recovery manual must restart the **scheduler entries** after macOS updates if they break.

**Day one:** interactive Herdr + Claude Code + Moshi is enough.  
**Soon after foundation:** scheduled job socket. Monday news digest is the motivating example. Front 9 / Proposal A later.

---

## 10. Ops / recovery (accepted front)

Failures **will** happen (macOS updates, reboot, Herdr/hook stop, Tailscale drop).

Design includes:

1. Human **manual** — ordered restore checklist  
2. Later **script** — checks + start services  
3. Smoke test — Moshi → Claude Code “hello”

Draft outline: [ops/RESTART.md](../ops/RESTART.md).

**Rule:** agents are durable between small failures; the seat is not immortal — documented restore beats tribal knowledge.

---

## 11. User journey (target behaviour)

Fine-tune as we go. This is the intended loop, not a shipped feature list.

### Attach (near-term)

1. **PC:** Herdr → Mac mini seat → **Claude Code** pane (new or reattach).  
2. **Phone:** Moshi → same seat → Claude Code (Chat View when set up).  

Same machine, same repos, same desks — two doors.

### Continue work (example)

You say: *“I want to continue research on XYZ.”*

The agent should then:

1. Know this is **your** desk (skill/KB priority).  
2. Load / follow the relevant **skill(s)** for that kind of work.  
3. Find **XYZ** in the **written knowledge base**.  
4. **Continue** in this durable Herdr session.

```text
You (Herdr or Moshi)
  → Claude Code on mini (later: routed agent)
      → skills / kb
      → work continues
```

**Wife near-term:** Moshi → Claude Code (any number of topic chats).  
**Wife later (Proposal A):** same, but facade may route engines underneath — still no “which CLI?” wizard.

---

## 12. Two important starters (before lots of polish)

We will refine skills and KB layout as real needs appear. Two things to establish **early**:

### A. Written knowledge base

- KB is **real markdown (or agreed format) on disk** in the private KB repo — not only chat memory.  
- Agents and humans read/write it; “continue XYZ” depends on XYZ existing (or being created) there.  
- Seat keeps a checkout; other machines need not clone the whole KB.  
- Start simple: namespaces `shared/` / `maciej/` / `anna/`, topics as folders or notes, map/index later if needed.

### B. Retro skill (improvements)

- A portable skill (skills repo) whose job is to **look for improvements**: after work, or on a schedule, review sessions / KB / skills and suggest or draft upgrades (clearer notes, missing criteria, skill gaps, stale topics).  
- Name in repo: e.g. `skills/shared/retro/` or `skills/maciej/retro/` — exact path TBD.  
- Pairs with the written KB: retro proposes changes; humans (or a careful agent pass) land them in KB/skills.  
- Optional later: scheduled retro run (same scheduler socket as Monday digest).

**Rule:** chat is ephemeral; **KB + skills are the durable system**. Retro keeps that system from rotting.

---

## 13. Open fronts (not day one)

| # | Front | Notes |
|---|--------|------|
| 1 | Wife research desk | Moshi; her skill/KB priority |
| 2 | Family ops | Shared tasks/reminders; no calendar initially |
| 3 | Calendar adapter | Later plug-in on top of file-based tasks |
| 4 | Sleep / WoL | Parked |
| 5 | Limen-like jobs / GitHub doorbell | Optional |
| 6 | Ops restore script | After manual is proven |
| 7 | Exact skills/KB repo layouts | When first real content lands |
| 8 | Scheduled jobs | Required; e.g. Monday morning digest |
| 9 | **Seamless provider routing (Proposal A)** | **Later.** Target: facade-per-chat (not one forever chat); she never picks providers. **Until then:** wife = Claude Code only. Spike A after multi-agent farm exists (Codex etc.) and before she needs non-Claude capacity daily. |
| 10 | Agent farm expansion | After Claude Code: OpenAI/Codex → Cursor CLI → local FM |
| 11 | **Job search (first real use, Maciej)** | Find **Test Architect / QA Lead** roles: **remote in Europe** or **hybrid in Poland**. Own topic context in KB (criteria, sources, shortlist, log). Candidate for the first scheduled job. |
| 12 | **Dev + AI news digest with feedback loop (future, not now)** | Daily/regular list of interesting news and concepts from the developer and AI world. Maciej marks each item interesting or not; the next day's list is curated from that feedback, so it improves over time. Needs: feedback log in KB, scoring/criteria that evolve, scheduler socket (§9b). Extends the Monday digest idea. |
| 13 | Skills to adopt from Matt Pocock (MIT, `github.com/mattpocock/skills`) | **Now:** `grill-me` + `grilling` (copy with credit into `skills/shared/`). **Later:** `handoff` (summarise a session so a fresh agent continues) and `writing-for-agents` (style guide for writing skills/AGENTS.md). Matt's `retro` is code-specific, so we write our own non-code retro (see §12B). |
| 14 | Glossary (parked) | A shared glossary of our own terms (seat, desk, topic, route, KB, skill…) with "avoid" synonyms, like Matt's `GLOSSARY.md`. Add when terms start to clash. Likely lives in `kb/shared/`. |

**Multiple topics, separate contexts (requirement):** the seat serves many unrelated uses. Each topic gets its own folder in the KB with its own context file, and the agent is started from that folder (one Herdr workspace per topic) so topics do not bleed into each other.
| 11 | **REMINDER: set up Herdr skill + Moshi skill** | Install the Herdr skill for the seat agent (`herdr --skill` / `npx skills add herdrdev/herdr --skill herdr -g`) and the Moshi Skill (getmoshi.app/docs, "Moshi Skill"), so Claude Code can drive Herdr and Moshi itself. Ask the user before writing to config locations. Do after Moshi + `moshi-hook` basics work. |

---

## 14. Explicit non-goals (for now)

- Implementing the seat in this folder as a product  
- Cloud VPS as required foundation  
- Wake-on-LAN dependency  
- One blended agent that mixes coding + family + her research without desks  
- Forcing wife through a provider/CLI picker  
- Auto cost optimizer across all subscriptions (related to Front 9, not the same)  
- Hard OS isolation between spouses  
- Treating Pi+Claude OAuth as “included in Claude plan”  

---

## 15. Suggested build order (when implementing — not now)

1. Mini always on + Tailscale + one account  
2. **Herdr** + **Claude Code** + Moshi smoke test  
3. Skills repo + KB repo (start real notes) + retro skill v0  
4. Restore manual after reboot  
5. Scheduler socket + later Monday digest  
6. Add **OpenAI/Codex** to Herdr (your farm; not required for wife v1)  
7. Add **Cursor CLI** → **local Mac FM**  
8. Front 1 (wife desk) on **Claude Code only**  
9. **Proposal A / Front 9** when she needs non-Claude capacity or multi-route UX  
10. Front 2 (family) when ready  

---

## 15b. Setup target (near-term) — Herdr + Claude Code

When we implement the first slice:

1. Herdr on seat (and client as needed)  
2. Claude Code installed + logged into Claude plan  
3. `herdr integration install claude` (or current equivalent)  
4. Moshi + moshi-hook; Claude Code inside Herdr/tmux pane for Chat View  
5. Smoke: phone and PC both reach the same Claude Code session family  

Extend later in order: OpenAI/Codex → Cursor CLI → FM. Wife v1 does not need Front 9; Proposal A comes after the multi-agent farm exists.

---

## 16. References in this folder

See [reference/SOURCES.md](../reference/SOURCES.md). Course copies are **reference**, not instructions to follow blindly on macOS.
