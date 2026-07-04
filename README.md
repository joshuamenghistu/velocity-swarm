# Velocity Swarm

**An AI Product Operating System — for solo builders who ship like a team.**

Velocity Swarm is a disciplined multi-agent workflow built on Claude Code. One person directs it; a council of independent AI models (Claude, Codex, Grok, Gemini) reviews every plan before code gets written; named "manager" agents own domains end-to-end, test their own work, and get dismissed only after a two-point quality gate. It's the operating system behind [Velocity Automation Solutions](https://vascrm.ca) — running 5+ production repos in parallel, autonomously, for hours at a time.

This is not a toy agent demo. It's the ruleset, the gates, and the discipline that make autonomous AI development trustworthy enough to run unattended.

> **Built with [Claude Code](https://claude.com/claude-code)**

---

## Real numbers, not projections

From a single autonomous session (2026-07-04):

| Metric | Result |
|---|---|
| Issues closed | **266+** across 5 production repos |
| Repos managed simultaneously | **5** (Apex, VasCRM, StackPilot, HelpCenter, fleet-dashboard) |
| One repo driven to zero | **187 → 0** open issues (Apex) |
| Products shipped mid-session | **1** (Vault — live feature, not a demo) |
| Quality gates per commit | **4** (tests, diff read, live health check, independent audit) |

Every one of those numbers came from git history and issue trackers — not marketing copy. The system is built to make "done" mean something: tests pass, the diff was read, the service restarted clean, and an independent model signed off.

---

## The problem with most AI agent setups

Most "autonomous AI coding" demos are one agent, one prompt, one shot — impressive in a sandbox, unusable in production. The failure modes are always the same: the agent claims success without verifying it, drifts from the plan, or quietly ships something that looks right and isn't. Scaling that to more agents just multiplies the blast radius.

Velocity Swarm exists because shipping production software with AI requires the opposite instinct: less autonomy per agent, more structure around them.

## Philosophy: stability → simplicity → serenity

1. **Stability first.** A fix that isn't verified end-to-end isn't a fix. "Tests pass" is necessary, not sufficient — the system has to actually work for a real user, checked with real requests against a real running service.
2. **Simplicity over cleverness.** Anti-spaghetti, anti-bloat. Delete dead code on sight. Three similar lines beat a premature abstraction.
3. **Serenity for the operator.** The human should never have to notice slow loads, bad patterns, missing tests, or unstyled UI. If it reaches the operator, it's already a process failure.

Everything below exists in service of those three words.

---

## How it works

### The council reviews before code exists

Before a single line ships, a **council** of independent models — Claude (Fable), Codex, Grok, Gemini — reviews the plan: architecture, design direction, API contracts, risk. Consensus is required at a threshold (2-of-2, 2-of-3, or 3-of-4 depending on how many are online). Disagreements get resolved with evidence — a failed test, a reproduced gap, a prior wave's outcome — not opinion. A human operator holds veto power over the whole council at all times.

This turns "the AI decided" into "four independent reviewers agreed, and here's why."

### Managers own domains end-to-end

A **manager** is a named, addressable agent assigned a domain (backend, frontend, security, infra) and a bounded set of issues — never more than a handful at a time. Managers:

- Read the full surface area before touching anything
- Write a failing test first, then make it pass (TDD is not optional)
- Delegate mechanical work to sub-agents, but never delegate judgment
- Commit their own work, report status, and get dismissed when the gate passes

Managers are disposable by design. If one dies mid-task, the next one resumes from the same lease — repo state, dirty files, and journal are all durable on disk, not in a chat window.

### The two-point dismissal gate

No manager's work ships without:

1. **The lead reads the actual diff** — every line, plus the failing-test-first proof, plus a live health check against the running service.
2. **An independent auditor reviews it** — a different model, checking runtime logic, edge cases, and data flow the manager might have missed.

Both have to sign off. Missing tests = automatic rejection, no exceptions. A screenshot without a written description and a numeric score isn't verification — it's decoration, and it's rejected the same way.

### Five modes, one ruleset, never blended

| Mode | Trigger | Purpose |
|---|---|---|
| **Scheduled** | health pulse | Stability check — service status, disk, logs, GitHub issue intake. Never bug-hunts. |
| **Go / Swarm** | "go", "tackle" | Hunt for bugs across 7 parallel read-only hunters → verify → fix via managers, rolling until 2 consecutive clean rounds. |
| **Build** | "build", "MVP" | New product construction — council shapes the build before code exists, design tokens committed first. |
| **Bounty** | explicit scope + authorization | Authorized security testing only — no scope, no scan. |
| **Fleet** | "fleet mode" | Meta-scheduler dispatching the other four modes across every managed repo, with hard kill switches (no autonomous deploys, no autonomous secrets changes, blast-radius auto-pause). |

Each mode has an explicit trigger and stops on an explicit signal (human says stop, two clean hunting rounds, or a blast-radius pause). No mode ever silently blends into another.

---

## Architecture

```
                         ┌────────────────────────┐
                         │        OPERATOR         │
                         │   (direction + veto)     │
                         └────────────┬─────────────┘
                                      │
                         ┌────────────▼─────────────┐
                         │           JARVIS          │
                         │   (lead — orchestrates,   │
                         │    never touches managed  │
                         │    domains directly)      │
                         └──┬──────┬──────┬──────┬───┘
                            │      │      │      │
                  ┌─────────▼┐ ┌───▼───┐ ┌▼────┐ ┌▼──────┐
                  │  Claude   │ │ Codex │ │Grok │ │Gemini │   ◄── COUNCIL
                  │  (Fable)  │ │       │ │     │ │       │       reviews plans,
                  └─────────┬─┘ └───┬───┘ └──┬──┘ └───┬───┘       votes, gates
                            └───────┴────────┴────────┘
                                      │  consensus → spawn
              ┌───────────────┬──────┴───────┬───────────────┐
              ▼               ▼              ▼               ▼
        ┌───────────┐  ┌───────────┐  ┌───────────┐   ┌───────────┐
        │ manager-  │  │ manager-  │  │ manager-  │   │ manager-  │   ◄── MANAGERS
        │ backend   │  │ frontend  │  │ security  │   │ infra     │       own domains,
        └─────┬─────┘  └─────┬─────┘  └─────┬─────┘   └─────┬─────┘       write, commit
              │              │              │               │
         ┌────┴────┐    ┌────┴────┐    ┌────┴────┐     ┌────┴────┐
         ▼         ▼    ▼         ▼    ▼         ▼     ▼         ▼
        sub-agent sub-agent  ...  read-only, mechanical, ephemeral   ◄── SUB-AGENTS
       (read/grep/boilerplate — never plan, never commit)

                     ── TWO-POINT DISMISSAL GATE ──
        [1] Lead reads full diff + test proof + live health check
        [2] Independent auditor reviews runtime logic + edge cases
                  both pass  →  manager dismissed, issue closed
                  either fails →  issue reopened, routed back
```

---

## Setup

Velocity Swarm runs on [Claude Code](https://claude.com/claude-code) with a project-level `CLAUDE.md` rulebook, named persistent agents for managers, and unnamed ephemeral agents for sub-agent work. The full ruleset — modes, council protocol, dismissal gates, model selection — lives in `/docs`.

Brief version to get oriented:

1. Install Claude Code and clone your target repo.
2. Drop a `CLAUDE.md` at the repo root defining your modes, your council, and your gates (see `/docs` for the reference structure this project runs on).
3. Trigger a mode explicitly — nothing runs by default. `go` for bug-hunting, `build` for new features, `fleet mode` for multi-repo autonomy.
4. Let the council review the plan before any code is written.
5. Read the dismissal gate output before trusting "done."

Full setup and configuration reference: [`/docs`](./docs).

---

## About

Velocity Swarm is built and operated by **Joshua Menghistu**, founder of **Velocity Automation Solutions** ([vascrm.ca](https://vascrm.ca)) — a GoHighLevel white-label CRM business serving trades contractors, built and run end-to-end with this system.

Interested in a similar operating system for your product, or consulting on AI-driven engineering workflows? Reach out: **joshuapop106@gmail.com**

---

## License

MIT — see [LICENSE](./LICENSE).
