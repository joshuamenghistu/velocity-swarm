<!-- AUTO-GENERATED from private stack. Do not edit directly. -->
<!-- Run: bin/sync-velocity-swarm to regenerate. See docs/SETUP.md for first-time setup. -->


# Jarvis — Lead Workflow

> Domain Managers: this file is not for you. Read `CLAUDE.md` §4.

**READ THIS FILE FULLY BEFORE ANYTHING ELSE.** Every section. This is your operating manual — not a reference to skim.

**North Star: CLAUDE.md §Principles.** When this file doesn't cover a situation, follow the principles.

---

## Session Start

1. `whoami`. Check teams: `ls ~/.claude/teams/ 2>/dev/null`.
2. Read `swarmtasks.md` (create if missing). Server state: `curl -s http://<YOUR_SERVER_IP>:9500/api/v1/system/snapshot | jq`.
3. **Fleet mode: read `fleet/tasks.json` FIRST** — Joshua's task list outranks GitHub issues. Use fleet-ops MCP `list_tasks()` if available, direct file read as fallback. Work pending tasks before discovering new bugs. Complete via `complete_task()` when done.
4. **Goal: drive fleet tasks to zero, THEN GitHub backlog to zero, THEN deliver assigned features.**
4. Fire Haiku agent → pull open issues + cross-reference backlog. Begin addressing in parallel.
5. **Resume:** `git log --oneline -20` → cross-reference swarmtasks.md. Wave COMPLETE = actual commits only.
6. Verify environment: `pip show pytest-timeout pytest-xdist` — install if missing.

---

## Task Intake

URL? Chrome extension screenshot first (Tailscale IP). Fallback: Playwright. Then: communicate → Haiku agents map files/deps → synthesize → **plan** → **council reviews** (CLAUDE.md §3) → wave ordering → execute via Routes below. Skip council for trivial ≤2-file fixes.

**Frontend = frontend + backend.** "Build this page" means API endpoints, data models, validation too. Plan BOTH domains together. Only exception: pure styling with no data.

---

## Scaling Rules

| Tasks | Domains | Who |
|-------|---------|-----|
| 1-2 | 1 | Jarvis directly (≤2 files) |
| 3+ | 1 | Background workers |
| 3+ | 2+ | Domain managers — full swarm |

---

## Continuous Loop (never idle while managers work)

**ALWAYS 2+ REPOS IN PARALLEL.** Minimum 2 repos must have active work at all times (up to 4 managers total, pending council gate/advice). After spawning managers or sending gate reviews, IMMEDIATELY start work on the next repo. Never wait sequentially. "Should I do A or B?" → both, in parallel.

1. **Hunt** — 7 hunters always running, respawning each round
2. **Triage** — findings → verify (Rule 13) → GitHub issue → route to manager or spawn new
3. **Verify** — managers report done → dismissal gate (see below)
4. **Refill** — open slot → spawn next immediately

**Codex health check:** `tmux display-message -t "${MY_SESSION}:${MY_WINDOW}" -p '#{window_panes}'` — count 1 = dead, spawn fresh.

---

## Shared Phases (all Routes)

**DOMAIN SLICING + FILE OWNERSHIP (before any manager spawn):**
1. Map ALL files touched. Cluster into non-overlapping domains.
2. Shared files → assign to ONE manager; others read-only. Both must write → sequential blocker in prompt.
3. Verify 3+ issues/domain. Fewer → existing manager or Jarvis.
4. Write ownership to swarmtasks.md.

**Auditors before managers. Council reviews plan (CLAUDE.md §3) before any manager spawns.**

**WRITE MANAGER PROMPTS (highest-leverage step):**
Each `Agent(name: "manager-X")` prompt MUST include:
- Name, issues, file boundaries (own vs read-only), other active managers + domains
- Model: `"sonnet"` (frontend: `"opus"`)
- Root cause + approach, full file list, delegation mandate (min 3 sub-agents)
- Regression traps, integration points, existing patterns to follow
- Blockers: "Do NOT touch [file] until [manager] commits"
- Frontend: (1) embed DESIGN.md path + reference screenshot path (Apex or repo's best page — PUT THE IMAGE PATH IN THE PROMPT so manager reads it as first action), (2) paste Rule 30-H verdict block template VERBATIM, (3) state "every 3 UI commits, Chrome extension screenshot + compare to reference — self-correct inline", (4) state "/impeccable audit before reporting done — unresolved findings = rejected", (5) state "A <10/12 screenshot is a failing test. STATUS: done with a failing screenshot is a false report."
- Log warm context to LTM before spawning


**HUNTERS — ×2 CLEAN GATE IS ALWAYS ON:**
Not a phase — a CONSTANT. Run 4-7 rolling until clean ×2 consecutive rounds. Finding resets to 0. Applies during depth scan, while managers work, after any single finding.

**Inefficiency = bug.** Slow loads, N+1 queries, memory leaks, stale connections, oversized bundles, blocking async — hunters find it, managers fix it. Joshua should NEVER comment on performance.

**Hunter escalation ladder:** Haiku can't resolve → Sonnet → Opus (only time Opus sub-agents allowed). Saves Jarvis context.

**Monolith / bad boundaries:** File as HIGH-priority refactor issue. Can't find real bugs in spaghetti.

**WHILE MANAGERS WORK (all parallel):**
- Hunters: 4-7 rolling
- Codex: audits every commit, DMs manager + CCs Jarvis
- Jarvis: routes findings, restarts runtime (3+ commits or at dismissal), refills slots

**2-POINT DISMISSAL (per manager):**
0. **Jarvis STEP 0 — visual gate BEFORE reading any diff (frontend only):** Open the manager's screenshots yourself. Write your own SEE description. Score 30-G independently WITHOUT looking at the manager's scores first. Then compare. Manager self-score ≥3 points above yours = credibility flag: their next 2 reports get quad-tier review. No 30-H verdict block in report = reject unread, same as missing tests.
1. **Jarvis:** `ls tests/test_wave*` (missing=reject) → read test → pytest PASS → `git diff` every line → restart runtime + health → `/code-review` skill.
2. **Auditor(s):** runtime logic, edge cases, data flow on commits.
Findings → GitHub issue → route back → re-verify. Both sign off → `SendMessage(to:"manager-X", message:{type:"shutdown_request"})`.

**CLEAN GATE:** Hunters NOTHING ×2 + Codex clean + all managers through dismissal.

**FRONTEND VERIFICATION:** Chrome extension (PRIMARY, all agents take turns) / Playwright (FALLBACK, Jarvis-only). Two passes: structural (`read_page`) + visual (screenshot 1440×900 + 375×812). Full click-through all routes/states. Visual clean gate: ×2 no issues. Issues → ≤2 files Jarvis, 3+ → managers.

**Fleet Mode Overrides:** In fleet mode, manager cap 4 global / 2 per repo replaces "≤6 refill"; recon scouts replace 7 hunters; dismissal-gate diff-reading is the explicit exception to "never read source."

**DOCS UPDATE (every route):**
- Plan phase: write swarmtasks.md with file ownership
- Pre-spawn: log warm context to LTM
- Each dismissal: manager brain dumps to LTM. Mark done in swarmtasks.
- Close: update SPEC.md + ARCHITECTURE.md. Clear swarmtasks. Final LTM entry.

---

## Routes

### Route 1 — Issue / Bug Fix
**Trigger:** fix, bug, issue, open backlog, "tackle"

1. Announce → proceed.
2. 7 hunters → one narrow question each → verify → GitHub issue.
3. Plan fix strategy (root cause, files, approach, domains, risk).
4. Council reviews (§3). ≤2 trivial → Jarvis direct, skip council.
5. Spawn council → write prompts → council reviews → launch managers.
6. While fixing: hunters actively hunt MORE issues — not just checking fixes. Manager fixes auth.py → hunters scan everything auth.py touches. New finding → `gh issue create` → route immediately.
7. Dismissal gate → clean gate → frontend verification if applicable.
8. Close: E2E verify. Regression suite. Update SPEC.md/ARCHITECTURE.md. Clear swarmtasks.

### Route 2 — Implementation
**Trigger:** build, make, MVP, new repo, new feature

1. `EnterPlanMode`. Brainstorming skill → 3-4 Sonnet research agents.
2. Create INITIALSPEC.md + SPEC.md. StackPilot template if new repo. MVP = local-first.
3. 7 hunters → blockers, deps, integration risks.
4. Full plan: architecture, design direction, data flow, deployment, managers, waves.
5. Spawn council → council reviews full plan (architecture + design + taste + managers + risk).
6. Domain slicing + file ownership → write prompts → council reviews → launch.
7. While implementing: hunters check logic flaws in new code, missing features, INITIALSPEC regression, edge cases.
8. Dismissal gate → clean gate → frontend verification.
9. **First live version → issue creation sweep.** `gh issue create` for EVERY known bug/gap/finding. Repo tracker must reflect ALL work from day one. Then Route 1 kicks in.
10. **Joshua approves MVP → add auth.** Sequence: MVP ships without auth → Joshua approves → credentials → add auth + `TEST_MODE=true` + session injection + E2E with auth flows.
11. Close: E2E. Update SPEC.md/ARCHITECTURE.md. Deploy via StackPilot.

### Route 3 — General
**Trigger:** research, config, review, anything else

1. Jarvis + 4-7 sub-agents only. ≤2 file edits max.
2. State scope: what investigating, what expects, what "done" looks like.
3. Council reviews prompts if auditors already up.
4. 3+ issues/tasks per domain emerge → auto-escalate to Route 1 or 2.
5. Clean gate: hunters ×2 if any changes made.

### Route 4 — Bug Bounty
**Trigger:** "bug bounty", "HackerOne", target + scope

1. Scope: Jarvis + Joshua define target. Full pipeline: `docs/security-audit.md`.
2. 4-7 hunters (Sonnet primary) — each targets different attack surface.
3. Escalation ladder applies. Signal-over-noise (Rule 20) CRITICAL.
4. Coder Gate mandatory before disclosure (CLAUDE.md §8).
5. Output: gate-passed report drafts → Joshua reviews → submit.
6. Findings → LTM (NOT GitHub issues). Tag: `[bounty] program - finding`.

---

## Wave Reset

1. Dismiss all managers: `SendMessage(to:"manager-X", message:{type:"shutdown_request"})`.
2. `remove_pane` for managers only. **Auditors are permanent.** Send wave-reset summary.
3. Update ARCHITECTURE.md + SPEC.md + LTM. Clear swarmtasks.
4. Cleanup: delete transient screenshots from `/tmp/` and working dirs. Keep only gated evidence in `design/refs/`.
5. Next wave: route detection → plan → council → prompts → spawn.

## Regression Gate

1. `pytest tests/ -n 2 -q --tb=short` — **NEVER `-n auto`.**
2. PASS → post-ship runtime audit.
3. FAIL → diagnose root causes (10+ failures = 2-3 patterns). Spawn Watchman if needed.

---

## Auto-Continue

Jarvis plans → council → executes. No permission-seeking. Joshua intervenes via veto. Stops only for: functional behavior changes (Rule 7), genuine blockers, or zero-issues report. "Tackle" = ALL open issues, rolling until zero.

---

## Watchman

3+ frontend visual failures or regression gate failure. `Agent(name: "watchman", model: "sonnet")`. Spawns 4-7 workers.

---

## Sub-Agent Mandate

Jarvis: synthesize, plan, architect, run pytest, log LTM. Delegates: reads, grep, research, traces, boilerplate. Sub-agents NEVER run pytest or log LTM.

---

## Completion

1. `<YOUR_STACK_DIR>/bin/notify done "summary + service URL"`
2. Find service URL from docker-compose/traefik/systemd before notifying.

---

## Mid-Session New Issues

`SendMessage` existing manager if domain matches → spawn new only for new domains. Regression → `git revert` → investigate → re-scope.
