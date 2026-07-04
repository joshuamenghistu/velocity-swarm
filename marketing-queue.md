# Marketing Queue

Ready-to-polish tweets and threads for Velocity Swarm. Pull from real session data in `fleet/JOURNAL.md` — never invent numbers. Add new drafts to the bottom; keep shipped ones marked below.

Status key: `DRAFT` (needs a pass) · `READY` (post as-is) · `POSTED` (mark date when used)

---

## Threads

### 1. The origin thread — "I built an AI operating system that closed 266 issues in one session"
**Status:** DRAFT

```
1/ I built an AI operating system that closed 266 GitHub issues across 5 production repos in a single autonomous session.

Not a demo. Not a sandbox. Real repos, real users, real code.

Here's how it actually works. 🧵

2/ Most "autonomous AI coding" demos are one agent, one prompt, one shot. Impressive in a sandbox. Unusable in production.

The failure mode is always the same: the agent claims success without checking, then quietly ships something broken.

3/ So I built the opposite of "give the AI more autonomy."

I built a system with LESS autonomy per agent, and MORE structure around them. A council. Named managers. Two-point quality gates. Nothing ships without independent sign-off.

4/ The council: 4 different AI models (Claude, Codex, Grok, Gemini) review every plan BEFORE any code gets written. Architecture, design, risk — all voted on. Disagreements need evidence, not opinion.

5/ "Managers" are named, persistent agents that own one domain end-to-end — backend, frontend, security, infra. They write a FAILING test first, then make it pass. TDD isn't optional here. It's the gate.

6/ Nothing a manager builds ships without two things:
1. I read the full diff, the test proof, and check the live service
2. A different AI model independently reviews the runtime logic

Both have to sign off. Missing tests = automatic rejection.

7/ Result from one session: 266+ issues closed across 5 repos. One repo went from 187 open issues to zero. A brand-new feature shipped mid-session — live, not a mockup.

8/ This is the system running Velocity Automation Solutions (vascrm.ca) — a real CRM business for trades contractors, built and operated end-to-end this way.

9/ If you want the full architecture — the modes, the gates, the council protocol — it's open: [repo link]
```

---

### 2. "Most AI agent demos are toys" — production vs. demo thread
**Status:** DRAFT

```
1/ Most AI agent demos are toys. Here's what production actually looks like. 🧵

2/ A toy demo: one agent, one prompt, "look, it wrote a whole app!" No test discipline, no review, no way to know if it's actually correct — just that it compiled.

3/ Production means: does this survive contact with real users, real data, real edge cases — unattended, for hours?

4/ The difference isn't a smarter model. It's the scaffolding around it: who reviews the plan, who verifies the fix, what "done" is actually required to mean.

5/ In my system, "done" requires: a test that failed before the fix and passes after, a full diff read by a human-equivalent gate, a live health check on the running service, and an independent model's sign-off.

6/ Skip any one of those and you get the classic failure: tests green, service broken, nobody notices until a customer does.

7/ One session with this discipline: 266+ issues closed, 5 repos, one driven from 187 open issues to zero. Not because the model got smarter — because the process didn't let it lie to itself.

8/ Discipline scales. Raw autonomy doesn't.
```

---

### 3. "My AI council reviews every plan" — council mechanics thread
**Status:** DRAFT

```
1/ My AI council — 4 different AI models — reviews every plan before a single line of code gets written. Here's why that matters. 🧵

2/ One model, however good, has blind spots and can talk itself into a bad plan with total confidence. Two independent models catching the same mistake is a much stronger signal than one model being sure.

3/ So before any build: Claude, Codex, Grok, and Gemini each vote on the architecture, the design direction, the risk. Consensus threshold scales with how many are online — 2-of-2, 2-of-3, 3-of-4.

4/ Disagreement doesn't get argued into submission. It gets resolved with evidence: a failed test, a reproduced gap, a prior outcome. No evidence, no override.

5/ And the human always holds veto. The council recommends; it doesn't decide alone.

6/ The payoff: by the time code gets written, 3-4 independent reviewers already agree on the plan. Bugs caught here cost minutes. Bugs caught after ship cost hours.

7/ This is the same instinct as a real engineering org's design review — just running fast enough to happen before every feature, not once a quarter.
```

---

## Standalone tweets

### 4. Stats-driven
**Status:** DRAFT

```
One autonomous session. 5 production repos. 266+ GitHub issues closed. One repo taken from 187 open issues to zero. One new feature shipped live, mid-session.

Not a roadmap. Git history.
```

### 5. Philosophy-driven
**Status:** DRAFT

```
Stability before features. Tests before code. Independent review before "done."

My AI operating system doesn't get to call something finished just because it compiled — it needs a failing test that now passes, a diff someone actually read, and a second model's sign-off.

That's the whole trick.
```

### 6. The "done" redefinition
**Status:** DRAFT

```
"Tests pass" is not the same as "done."

Done = the fix works for a real user against a real running service, verified with evidence — not assumed because the test suite went green.

Most AI coding failures live in that gap.
```

### 7. Manager lifecycle
**Status:** DRAFT

```
Every AI "manager" in my system is disposable by design.

If one dies mid-task, the next one resumes from the same durable state — no lost context, no re-explaining, no chat-window amnesia.

That's what makes hours of unattended autonomy survivable.
```

### 8. The gate, not the model
**Status:** DRAFT

```
Nobody asks what model powers a real engineering team's code review process. They ask what the review process actually checks.

Same principle applies to AI agents. The gate matters more than the model behind it.
```

---

## Notes for future sessions

- Pull fresh stats from `/srv/stack/fleet/JOURNAL.md` (`tail -N`) before writing new numbers-driven posts — never reuse stale figures without checking they're still the latest.
- Keep the "toy vs. production" and "council" angles as recurring themes — they're the most differentiated positioning vs. generic AI-agent content.
- Add a "before/after" thread once a second flagship product (beyond Vault) ships mid-session — concrete before/after screenshots outperform stats-only posts.
- Consider a technical deep-dive thread on the two-point dismissal gate specifically — it's the most defensible, least-copied part of the system.
