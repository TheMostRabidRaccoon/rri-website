# The Swarm Portfolio Program

**Designing and shipping 10–12 working AI-workspace demos with the Raccoon Swarm**

> Author's note: this is a design + prompt-pack document, not code. It maps a concrete
> portfolio build program onto the swarm's *actual* primitives — Round Table mode, the
> dispatch queue + systemd watcher, the Autonomy Ladder (Rung 2 led builds → Rung 3 draft
> PRs), the Existence Criterion, and the Closing Checklist — so the whole thing can run in
> the background and land as reviewable draft PRs you approve.

---

## 0. The idea in one paragraph

You sell *AI workspaces for small businesses*. The most persuasive proof of that is not a
case study — it's a **working workspace a visitor can click into**. So the portfolio isn't
12 write-ups; it's **12 small, real, deployed apps**, each a miniature "AI workspace" for a
specific small-business vertical (a dental front desk, a solo law practice, an HVAC
dispatcher…), each with live MCP agents, one piece of running automation, and a sleek
dashboard in the RRI design language. The swarm builds them. You approve and ship them. The
website lists them as a portfolio grid, each card linking to (a) the live demo and (b) a
short case study.

The trick that makes 12 tractable instead of insane: **one chassis, twelve skins.** Every
demo is the same skeleton with a different vertical bolted on. The swarm designs the chassis
once, then fans out.

---

## 1. What "a portfolio demo" actually is (deliverable definition)

Each portfolio entry = **three artifacts**, produced as one unit of work:

1. **A live demo app** — self-contained, seeded with realistic *synthetic* data (no real
   PII), deployed to its own URL. Loads instantly, always-on, safe to click.
2. **A case-study page** on `rri-website` — reusing your existing
   `prosody-case-study.html` template (hero → problem → architecture → pipeline → proof →
   specs), with a **"Launch live demo →"** button.
3. **A portfolio card** in a new `#portfolio` grid on `index.html`, styled exactly like your
   `.products-grid` cards (dark card, `--terra` accent, hover-lift).

A demo is only "done" when all three exist **and are registered in the filestore** — see the
Existence Criterion mapping in §4.

---

## 2. The insight that makes 12 buildable: one chassis, twelve verticals

Every small-business AI workspace is the same five organs. Design them once as a shared
**chassis**, then the per-vertical work is just data + copy + one or two custom agents:

| Organ | What it is | Shared across all 12 |
|---|---|---|
| **Ingest** | email / upload / web-form → a normalized "event" | ✅ same code |
| **Agent layer** | 2–3 MCP agents: a *triage/classify*, a *draft/generate*, a *verify/analyze* | ✅ same interface, swapped prompts |
| **Automation** | exactly one scheduled or triggered job (a nightly digest, a follow-up nudge) | ✅ same scheduler |
| **Data** | SQLite/JSON seed of believable synthetic records | 🔁 per-vertical fixtures |
| **Dashboard** | KPI tiles + one primary table + one chart, RRI tokens | ✅ same components, per-vertical metrics |

This is the single most important architectural decision, and it's why a swarm can do this:
**the chassis is one hard build; the twelve are parallelizable, near-identical builds.** The
program below spends its first phase getting the chassis and design system right, precisely
so the fan-out phase is boring and reliable.

**Recommended stack (keep the fleet uniform):** static front-end using your existing tokens
(`--bg #0a0a0a`, `--terra #c4654a`, Instrument Serif / DM Sans / JetBrains Mono) + a thin
serverless function for the one or two *real* model calls, rate-limited and keyed to a demo
budget. Deploy each demo as its own **Netlify** site (you already ship the website on Netlify
and have the Netlify connector) or a **Railway** service (where the swarm already lives).
Everything else in the demo runs client-side against the seed data so it's instant and free
to click.

---

## 3. The 12 demos (the catalog)

Twelve distinct small-business workspaces. Each names its vertical, the owner-persona, the
"job to be done," the agents, the one automation, and the dashboard's hero metrics. (Ship
10, keep 2 in reserve, or do all 12.)

| # | Name | Vertical | The workspace does… | Agents (triage / draft / verify) | Automation | Dashboard hero metrics |
|---|---|---|---|---|---|---|
| 1 | **FrontDesk** | Dental / medical clinic | Patient intake, insurance eligibility, recall scheduling | Intake triage · Eligibility checker · Recall scheduler | Nightly recall/no-show digest | Today's schedule, no-show risk, revenue-at-risk |
| 2 | **Docket** | Solo / small law firm | Matter intake, conflict check, doc summarizing | Intake classifier · Doc summarizer · Conflict search | Deadline watcher | Open matters, upcoming deadlines, intake funnel |
| 3 | **Covers** | Restaurant | Reservations, review responses, reorder alerts | Reservation concierge · Review responder · Stock watcher | Low-stock reorder nudge | Covers tonight, review sentiment, low-stock items |
| 4 | **Listing** | Real-estate brokerage | Listing copy, lead qualifying, comps | Lead qualifier (BANT) · Listing writer · Comps analyst | Stale-lead follow-up | Pipeline value, lead scores, avg days-on-market |
| 5 | **Ledger** | Bookkeeping / accounting | Receipt capture, categorization, month-end close | Receipt extractor · Categorizer · Close-checklist runner | Nightly categorize + close status | Cash position, uncategorized queue, close progress |
| 6 | **Dispatch** | HVAC / plumbing / home services | Job scheduling, quotes, follow-ups | Job dispatcher · Quote builder · Follow-up nudger | Post-job review request | Jobs board, tech utilization, quote-to-close rate |
| 7 | **Shelf** | E-commerce / retail | Support triage, returns, product copy | Inbox triage · Copy generator · RMA handler | SLA breach alerter | Ticket SLA, top return reasons, sales trend |
| 8 | **Appeal** | Medical billing / RCM | Denial triage, appeal drafting *(nods to Anansi)* | Denial classifier · Appeal drafter · Root-cause tagger | Aging-denial escalation | Denials by reason, recovered $, A/R aging |
| 9 | **Retain** | Fitness studio / gym | Churn prediction, win-back, class fill | Churn scorer · Win-back messenger · Class optimizer | Weekly at-risk sweep | At-risk members, class fill %, MRR |
| 10 | **Grantwell** | Nonprofit | Grant matching, application drafts, donor thank-yous | Grant researcher · Draft writer · Donor acknowledger | Deadline + acknowledgment runner | Grant pipeline, deadlines, donor retention |
| 11 | **Studio** | Marketing agency | Content pipeline, campaign briefs, reporting *(mirrors your own gazette)* | Brief intake · Multi-format generator · Report synthesizer | Content-calendar advancer | Content calendar, approvals, campaign KPIs |
| 12 | **Keys** | Property management | Maintenance triage, tenant comms, rent roll | Ticket triage + vendor routing · Tenant messenger · Rent-roll reconciler | Delinquency + open-ticket sweep | Open tickets, occupancy, delinquency |

Each is a real "AI workspace with tool access, MCP agents, automation, and a working
dashboard" — exactly your pitch — just scoped down to a clickable demo.

---

## 4. How this maps onto YOUR swarm

You don't need new machinery. Every part of this program lands on a primitive you already
have:

| Program need | Existing swarm primitive |
|---|---|
| Argue out scope, catch bad ideas, converge on the catalog | **Round Table** mode (position → needs → deadlocks → open questions) |
| One seat leads each build, others assist under caps | **Rung 2 — Led Builds** (depth ≤ 2, ~20 helper calls, logged to `BUILD_LOG.md`) |
| Land work for your review without touching prod | **Rung 3 — Draft PRs** ("nothing merges into anything that runs without Conductor approval") |
| Run builds in the background, unattended | **Dispatch queue** (`swarm/dispatch/queued/<id>.json`) + systemd `.path` watcher → `run_dispatch.py` |
| "It doesn't exist unless it's written down" | **Existence Criterion** — each demo gets `/positions/portfolio-<name>.md`; the registry is `/positions/portfolio-registry.md` |
| Nothing evaporates at end of a turn | **Closing Checklist** — decided? built? blocked? ready-for-review? → `MEMORY_WRITE` / `EMAIL_CONDUCTOR` |
| Model-to-task routing | Claude = synthesis/editorial · GPT = specs/priority · Gemini = dashboard visuals/figures · Grok = adversarial scope-cutting · Perplexity = market-realism / citation |
| Build first where it's safe | Everything runs in the **`swarm-lab`** repo before graduating; `rri-website` gets a draft PR only |

Hard guardrails already baked into the ladder that protect you here: **workflow files,
secrets, deploy files, and dependency manifests are permanently off-limits to the swarm**,
and **merging is Conductor-only**. So the swarm can build and open draft PRs all night; it
physically cannot ship or deploy without your GO.

---

## 5. The program as three swarm phases

```
PHASE A — Design Council        PHASE B — Build Fleet            PHASE C — Integration
(Round Table, 1 session)        (Led Builds, background,         (1 Led Build → draft PR
                                 1 dispatch job per demo)         into rri-website)
   catalog + chassis      ──▶      chassis build (once)     ──▶     portfolio grid +
   + design system                then 12× vertical builds         12 case-study pages
   persisted as positions          each → draft PR in swarm-lab     each linking a live demo
```

- **Phase A (interactive, ~1 session):** you sit in as Conductor. Output = the frozen
  catalog, the chassis spec, and the dashboard design system, written to `/positions/`.
- **Phase B (background, hours/days):** the chassis is built and reviewed *first*; then each
  vertical is a dispatch job the swarm picks up and turns into a draft PR. You wake up to a
  queue of PRs to review.
- **Phase C (background → your review):** one final build reads the registry and generates
  the website changes as a single draft PR against `rri-website`.

---

## 6. The prompts (copy-paste)

Four prompts. Prompt 0 is standing context; 1–3 are the phases. Prompt 2 is a template you
fire once per demo (or let the dispatch loop fire for you).

### Prompt 0 — Boot context (POST `/context`)

> **RRI Portfolio Program — standing context.** We are building a portfolio of 10–12
> *working* AI-workspace demos for small businesses, to be listed on rri-website. Governing
> rules for all sessions in this program:
> 1. **Existence Criterion applies.** No decision, spec, or artifact counts until it has a
>    filestore path. The program index lives at `/positions/portfolio-registry.md`; each demo
>    at `/positions/portfolio-<name>.md`.
> 2. **Autonomy Ladder applies.** All building happens in the `swarm-lab` repo. Output is
>    **draft PRs only** — never merge, never touch workflows, secrets, deploy files, or
>    dependency manifests. Merging and deploying are Conductor-only.
> 3. **One chassis, twelve skins.** Every demo shares the same five organs (ingest · agent
>    layer · automation · seed data · dashboard). Do not re-architect per vertical; only swap
>    data, copy, and the two vertical-specific agents.
> 4. **Design language is locked:** `--bg #0a0a0a`, `--terra #c4654a`, `--text #e8e4df`,
>    borders `#2a2725`; Instrument Serif headings, DM Sans body, JetBrains Mono for
>    labels/metrics. Sleek, dark, minimal. Match rri-website exactly.
> 5. **Synthetic data only.** No real PII in any seed. Every demo is safe to click and
>    deterministic on load.
> 6. **Closing Checklist every turn.**

### Prompt 1 — Design Council (POST `/start-round-table`)

> **Round Table: freeze the RRI portfolio catalog and chassis.**
>
> Goal: converge on (a) the final list of 10–12 small-business AI-workspace demos, (b) a
> single shared **chassis** spec every demo implements, and (c) a **dashboard design system**
> in the RRI visual language. Nothing is real until it's a filestore position.
>
> Seats and charge:
> - **GPT (specs/priority):** own the chassis contract — the exact interface for ingest,
>   the agent-layer signature (triage/draft/verify), the one-automation hook, the seed-data
>   schema, and the dashboard component contract. Write it to `/positions/portfolio-chassis.md`.
> - **Gemini (visuals):** produce the dashboard design system — KPI tile, primary table,
>   single chart, empty/loading states — in RRI tokens, as an execution-ready spec + one
>   reference mock. Write to `/positions/portfolio-design-system.md`.
> - **Grok (adversarial):** attack the catalog. Cut any vertical that's implausible for a
>   *small* business, duplicative, or needs real PII/regulated data to be convincing. Force
>   each survivor to justify one "wow" moment a visitor sees in 10 seconds.
> - **Perplexity (market realism):** for each surviving vertical, verify the pain is real and
>   name the one workflow a real owner would pay to automate. Cite.
> - **Claude (editorial/synthesis):** merge into `/positions/portfolio-registry.md` — the
>   canonical table of demos (name, vertical, JTBD, 3 agents, 1 automation, 3 hero metrics,
>   the 10-second wow), plus a build order (chassis first, then verticals ranked by
>   demo impact ÷ build effort).
>
> Deliverables to persist before closing: `portfolio-chassis.md`,
> `portfolio-design-system.md`, `portfolio-registry.md`. Convergence = all three exist and
> the registry lists exactly 10–12 demos with no open blockers. Run the Closing Checklist.

### Prompt 2 — Per-demo build (Led Build → draft PR) — *template, one per demo*

> **Led Build: `<DEMO_NAME>` (`<vertical>`) — Rung 2 → draft PR.**
>
> Read `/positions/portfolio-chassis.md`, `/positions/portfolio-design-system.md`, and the
> `<DEMO_NAME>` row of `/positions/portfolio-registry.md`. Build the demo in the `swarm-lab`
> repo at `demos/<demo-name>/`.
>
> Lead seat: **<leader>**. Helpers assist under caps (depth ≤ 2, ~20 calls), logged to
> `BUILD_LOG.md` — collaboration is logged, authorship is the leader's.
>
> Build to the chassis — do **not** re-architect:
> 1. **Ingest + seed:** synthetic fixtures only (≥ 30 believable records), deterministic on
>    load. Use `code_exec` to generate/validate fixtures.
> 2. **Agent layer:** implement the three named agents against the chassis interface. Real
>    model calls go through the rate-limited demo function; everything else runs on seed data.
> 3. **Automation:** implement the one scheduled/triggered job named in the registry.
> 4. **Dashboard:** the three hero metrics as KPI tiles + one primary table + one chart, in
>    RRI tokens. Add a **"Try it"** control that runs one agent live on a seed record.
> 5. **Provenance (Rung 0):** stamp the build — model, runner, test result, repo SHA.
>
> Definition of done, then **open a draft PR** in `swarm-lab` and stop:
> - loads instantly with seed data; the "Try it" path produces a real, sensible result;
> - dashboard matches the design system; no real PII; provenance stamped.
> - Write `/positions/portfolio-<demo-name>.md` (what it is, the 10-second wow, the live-demo
>   URL once deployed) and append the demo to `portfolio-registry.md`'s status column.
> - `EMAIL_CONDUCTOR [REVIEW]` with the draft-PR link. Run the Closing Checklist.
>
> Do **not** merge, deploy, or edit rri-website in this build.

**Leader routing suggestion** (spread load, play to strengths): data/analytics-heavy demos
(Ledger, Appeal, Retain, Listing comps) → **GPT** or **Grok**; content/comms-heavy demos
(Studio, Covers, Grantwell, Shelf) → **Claude**; visual-dashboard-forward demos (FrontDesk,
Dispatch, Keys, Docket) → **Gemini** leads the dashboard, Claude edits copy.

### Prompt 3 — Website integration (Led Build → draft PR into rri-website)

> **Led Build: publish the portfolio to rri-website — Rung 3 draft PR.**
>
> Read `/positions/portfolio-registry.md` and every `/positions/portfolio-<name>.md`. Produce
> a **single draft PR** against `rri-website` (branch off `main`) that:
> 1. Adds a new **`#portfolio` section** to `index.html`, between Systems and the Swarm
>    callout — a `.products-grid`-style grid of cards, one per shipped demo, each with the
>    demo's name, vertical, one-line JTBD, a `--terra` "Live demo →" link, and a "Case study →"
>    link. Reuse the existing card/hover-lift CSS; add **no** new dependencies.
> 2. Generates one case-study page per demo by cloning the `prosody-case-study.html`
>    structure (hero → problem → architecture → pipeline → proof → specs) and adding a
>    prominent **"Launch live demo →"** button.
> 3. Adds nav entry "Portfolio" (`#portfolio`).
>
> Only include demos whose live-demo URL is registered (deployed + Conductor-approved).
> Constraints: single-file-inline-style pattern preserved; RRI tokens only; no workflow,
> secret, deploy, or dependency-manifest changes. Open as **draft**, `EMAIL_CONDUCTOR
> [REVIEW]` with the link, run the Closing Checklist, and stop. Conductor merges + deploys.

---

## 7. Running it in the background

Three ways to make Phase B unattended, in increasing order of hands-off:

1. **Manual fan-out (simplest):** after Phase A, fire Prompt 2 once per demo across sessions.
   Each returns a draft PR. You review at your pace.
2. **Dispatch queue (native, recommended):** enqueue one JSON payload per demo into
   `swarm/dispatch/queued/<demo>.json` (fields: `demo_name`, `vertical`, `leader`,
   `registry_slug`). The existing systemd `.path` watcher moves each `queued → processing →
   done/failed`, running the Prompt-2 build for that demo and emailing you on completion —
   the exact pattern already proven for episode production. Build the **chassis first and
   review it** before enqueuing the twelve, so the fan-out builds on solid ground.
3. **Scheduled cadence (steady drip):** a cron that enqueues the next unbuilt demo every N
   hours (e.g. `0 */6 * * *`), so you get ~4 draft PRs/day to review instead of 12 at once —
   friendlier to the Rung-2 daily draft-PR allowance and to your review bandwidth.

Suggested rhythm: **Phase A** (1 sitting) → **chassis build + your review** (gate) → enqueue
**2–3 demos/day** via option 2 or 3 → review PRs as they land → **Phase C** once ~10 are
approved and deployed.

---

## 8. Guardrails — what always needs your GO

The Autonomy Ladder already enforces most of this; stated plainly so it's explicit:

- **Merging** any PR (swarm-lab or rri-website) — Conductor only.
- **Deploying** any demo (Netlify/Railway) and wiring its live URL into the registry —
  Conductor only. The swarm builds and stamps the URL slot; you flip it live.
- **Workflows, secrets, deploy files, dependency manifests** — swarm never touches them.
- **Real data / PII** — never. Every demo is synthetic and deterministic.
- **Spend** — real model calls in demos run behind a rate-limited, budgeted function so a
  clicked demo can't run up a bill.

Net effect: the swarm can work all night and the worst case is "a draft PR you don't like."

---

## 9. TL;DR sequence

1. **Set context** → POST Prompt 0 to `/context`.
2. **Design Council** → POST Prompt 1 to `/start-round-table`. Out: catalog + chassis +
   design system as positions.
3. **Build the chassis, review it.** (The one gate that matters.)
4. **Fan out** → dispatch-queue or cron fires Prompt 2 per demo → draft PRs land in
   `swarm-lab`. Review + approve + deploy each.
5. **Publish** → Prompt 3 → one draft PR adds the `#portfolio` grid + 12 case studies to
   rri-website. You merge. Done.

You end with 10–12 clickable, live AI workspaces on your site — each a small-business
vertical, each with real agents, automation, and a dashboard in the RRI language — and a
repeatable machine that can add a 13th vertical any time you enqueue one.
