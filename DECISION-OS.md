# The Personal Decision OS — setup guide

*How the decision system is set up and run, written from a working state so you could duplicate it. It's an optional extension of the Second Brain OS ([GUIDE.md](GUIDE.md) §10 is the summary; this is the full build). All examples in this doc are fictional.*

---

## 1. What this is

A structure that makes life decisions weigh evenly. The insight it's built on:

> **"Weigh evenly" is not something you do at decision time.** Under stress, whatever is loudest gets the weight. The only way to weigh evenly is to set the weights *before* the decision arrives — in calm — and score options against the pre-committed weights when it counts.

So the system is mostly not about decisions. It's about maintaining a small set of upstream files — weighted values, red lines, goals, a quarterly focus — so that when a decision shows up, the context is already in a form that can be applied: by you, or by an agent working for you.

What it is not: a formula that outputs answers. The matrix is a lens; the human decides. The system's job is to make sure the deciding happens against everything that matters instead of whatever is loudest.

## 2. The four layers

Ordered by stability — each layer changes an order of magnitude more often than the one above:

| Layer | Files | Changes | Role |
|---|---|---|---|
| **L0 Identity** | `Values.md`, `Principles.md` | ~never (edits only via calibration) | What matters, weighted; what to do when known situations recur |
| **L1 Goals** | `Goals.md`, `Season.md` | Quarterly | What the years should produce; what this quarter is for |
| **L2 Decisions** | `decisions/` records, `Decision log.md`, `Decisions.base` | As decisions arrive | The records, in three tiers |
| **L3 Reviews** | `/decide` (surfaces due reviews), `/life-review`, dashboard rows | Continuous | The flywheel that keeps L0–L2 honest |

A decision is where the layers meet: options are scored against L0 weights and the L1 goals the decision touches, vetoed by red lines and anti-goals, recorded in L2, and calibrated by L3.

All of it lives inside `2-areas/identity/` — one area, no new taxonomy.

## 3. The artifacts, in steady state

### `Values.md` — the load-bearing file

5–8 named values, weights summing to **100**. The sum is the discipline: a value that costs nothing to hold isn't a value, and the sum forces the trade-offs at commit time instead of decision time. Per value: a one-line definition in your own words, weight, one line each for what honoring/violating it looks like, and the interview episodes that earned it a slot.

Two more sections:

- **Red lines** — non-negotiables, phrased as vetoes an agent can apply ("No option that…"). Red lines are never weighted. An unacceptable option must not be able to launder itself through a high matrix total.
- **Weight-change log** — dated one-liners; every calibration edit, with the decision that prompted it. In steady state this log grows a line or two per quarter. Weights that never change are as suspicious as weights that change monthly.

### `Principles.md` — committed if-then rules

Values say what matters; principles say what to *do* when a known situation recurs — phrased if-then so they can be applied mid-decision ("When I feel rushed → ask who benefits from my hurry"). Only **signed** rules live here, rewritten in your own words. Collected wisdom stays in the library; a principle you didn't rewrite is decoration.

### `Goals.md` + `Season.md`

Three horizons (1yr concrete-and-dated, 3yr directional, 10yr identity-level), ≤ 5 goals each, **every goal tied to the value it serves**. A goal serving no value gets flagged at review — it's usually someone else's goal, imported. Plus **anti-goals**: what success is not allowed to cost. Anti-goals veto like red lines; they are never weighed.

`Season.md` is the bridge to actual weeks: the quarter's **one** focus, which goal it serves, an end date — and the *deliberately neglected* list, which is the load-bearing part. Neglect happens either way; naming it makes it a choice instead of a guilt.

### `decisions/` — the records

Three tiers, chosen by **reversibility × stakes**:

| Tier | Test | Process | Time |
|---|---|---|---|
| **T1 Hallway** | Reversible AND cheap | One line in `Decision log.md` | 30 sec |
| **T2 Door** | Reversible but expensive, or hard to reverse but small | Mini-matrix: options × top-3 values, gut first | 15 min |
| **T3 Gate** | Hard to reverse AND consequential | Full record: veto screen, matrix, five adversaries, pre-mortem, scheduled review | 1–2 sittings |

Unsure → round up. An over-processed T2 costs 30 minutes; an under-processed T3 costs a year. The T1 log matters more than it looks: small calls leave a trace, so recurring ones become visible at review and get promoted to a real record.

`_meta/Decisions.base` is the pipeline view: **Open** · **Due for review** · **Calibration** (reviewed, grouped by outcome).

### The T3 record — order is load-bearing

1. **Question** — as stated, then *reframed*. The stated question is usually not the real one ("Should I take this job?" is usually "What do I want the next 3 years to be for?").
2. **Options** — always including status quo, explicitly, described as what it actually looks like in 12 months. The invisible default wins by going unexamined.
3. **Gut call — before any scoring is visible.** The gut is data. Gut-vs-matrix disagreement is the single most informative output the system produces: either the weights are wrong or the gut knows something unstated. Investigate; never average.
4. **Veto screen** — options vs red lines + anti-goals. Vetoed options are struck, never scored.
5. **Matrix** — surviving options × values (pre-committed weights) + any goal the decision touches. Scores 1–5, each with a one-line rationale; agent drafts, human corrects. **Weights are never edited mid-decision** — mid-decision edits are rationalization wearing a methodology.
6. **Five adversaries** — because the numbers can't catch what a hostile question can:
   - *Regret (10/10/10)* — each option at 10 days / 10 months / 10 years; which regret compounds, which decays
   - *The Advocate* — steelman the losing option; what would have to be true for it to be right?
   - *The Base-rate skeptic* — what happens to most people who choose the leader, and why exactly would this case differ? Mechanism, not feeling.
   - *The Accountant* — full cost of the leader: money, time, energy, attention, and the best alternative use of each
   - *The Board* — who else does this bind? Anyone bound gets a live conversation, never a proxy score.
7. **Verdict block** — gut vs matrix, aligned or split, the one adversary finding that most threatens the leader, who must be talked to first.
8. **Decision + why** — one dated paragraph, written *falsifiably*: the future review grades this sentence.
9. **Pre-mortem** — "it's `review_by` and this was wrong because…", each failure line with an early signal worth watching.
10. **Review** — filled at `review_by`: what happened, what the weights underrated, weight edit proposed → the weight-change log.

## 4. The loops

**`/decide`** — runs whenever a decision arrives. Opens by surfacing any record past its `review_by` (reviews come before new decisions — that's what keeps calibration real), loads the identity and goals layers plus past records on similar topics, flags staleness (values > 6 months old, season expired), tier-triages, then walks the record in the order above. Files one record and stops — no side artifacts.

**`/life-review`** — quarterly. Closes the season (what shipped, what the neglect actually cost) and opens the next; advance/amend/kill on every goal — nothing survives by default; runs all due decision reviews; scans the T1 log for recurring calls to promote; checks values drift against the weight-change log; one-screen report. State in `_meta/sync/life-review-log.md`.

**Dashboard** — three health rows checked automatically, same idiom as the rest of the vault: decisions past review (target 0) · goals fresh within 90d · season current.

Review windows: T2 and reversible T3 → +3 months; one-way T3 → +1 year (with an optional +1 month "how does it feel" note).

### The Obsidian wiring

The decision OS needs no plugin of its own — it rides the vault's existing stack (full inventory: [GUIDE.md](GUIDE.md) §4):

- **claude-sidebar** is the runtime: Claude Code embedded in an Obsidian sidebar pane with the vault as its working directory. That makes `/decide` and `/life-review` slash commands typed in the sidebar, auto-loads `CLAUDE.md` (privacy rules, routing) every session, and lets the agent write records that Obsidian renders live. Heavy sessions (the values interview) run the same skills from a terminal via open-in-terminal — same brain, bigger window.
- **Bases** (core plugin) renders `_meta/Decisions.base` — the Open / Due for review / Calibration views over record frontmatter. No query language to maintain; the frontmatter contract *is* the schema.
- **Dataview** powers the three dashboard health rows (decisions past review, Goals freshness, Season current). ⚠️ Not currently installed in this vault — install + enable JavaScript Queries or those lights won't render. The `.base` views work without it.
- **obsidian-linter** keeps record frontmatter well-formed — which matters more here than anywhere, because `status`/`review_by` drive the review flywheel.

## 5. Setup sequence

**Sitting 1 — the values interview (~45 min; the only hard part).** Agent-assisted, four rounds:

1. **Episodes, not adjectives.** Peaks (moments that felt deeply right — what was honored?), troughs (regretted decisions, or success that felt wrong — what was violated?), envy (a leading indicator of an unfed value), and the audit: where did the last 3 months of discretionary time and money *actually* go? The gap between revealed and stated preference is the interesting part.
2. **Name and define.** Agent proposes 8–10 candidates with evidence; you rename in your own words, merge, and kill down to 5–8.
3. **Force the weights.** Pairwise trade-offs ("A conflicts with B and you can only honor one this year — which?") until a strict ranking exists; distribute 100 across it. Sanity check against last year's actual behavior. Aspirational weights are allowed but must be labeled — the review loop will keep flagging the gap.
4. **Red lines.** "What would you not do for any amount of the things you just weighted?" Usually 2–4, each phrased as a veto test.

**Sitting 2 — goals + season (~30 min).** Fill the three horizons, tie each goal to its value, write the anti-goals, open the first season with its deliberate-neglect list.

**Then: the first real decision runs the loop manually.** No automation, no scheduled anything, until one decision has gone question → record → review honestly. Habit before machinery — the same rule the rest of the vault runs on.

**Confirm the principles when they come up.** The drafted if-then rules get signed, rewritten, or killed opportunistically — a decision that touches one is the natural moment.

## 6. Running it — what steady state looks like

- **Most weeks: nothing.** The system is dormant until a decision arrives or a quarter ends. That's by design — a decision system that demands weekly attention gets abandoned.
- **A decision arrives** → `/decide`. T1s take 30 seconds. T2s take 15 minutes. T3s take a sitting or two, and the record is the artifact — reread it at the review, not before.
- **A quarter ends** → `/life-review`, ~45 minutes. This is where the flywheel actually turns: due reviews run, weights get their calibration edits, goals get advanced or killed, the next season opens.
- **The dashboard watches the rest.** If nothing is red, nothing is owed.

**The calibration flywheel is the whole game.** Every review asks one question — *did the weights predict what actually mattered?* — and the answer flows back into `Values.md` as a dated weight edit. A worked example of the full cycle — including a review that catches a missing value — is worth keeping as a fictional record (e.g. a "choosing the next role" decision) so the template is never encountered empty. Without the review step, the system is a form you fill out to feel rigorous. With it, the weights converge on the person you actually are.

## 7. Rules that don't bend

- **Gut before totals.** Always recorded first; splits are investigated, never averaged away.
- **Vetoes before matrix.** Red lines and anti-goals kill options outright; they never carry weights.
- **Weight edits only at reviews.** Never mid-decision.
- **Decisions that bind another person get a live conversation** — the matrix does not proxy anyone's vote.
- **Everything is private.** Records and the identity layer carry `visibility: private`; nothing feeds essays or anything public without explicit per-case consent. Agents never open `codes/`, and private notes may inform a live conversation but are never quoted into artifacts.
- **Self-context is retrieved, never ambient.** The identity layer is loaded *through* a loop when a decision calls for it (that's what `/decide` does), never nailed into every turn as a standing profile. Full principle: *retrieval over conditioning* — see [structure/system-map.md](structure/system-map.md).

## 8. Kill criteria — every component earns its keep

The system inherits the vault's core survival rule: loops that get skipped get *shrunk*, not disciplined.

| Component | Dies when | Falls back to |
|---|---|---|
| Matrix + adversaries | 2 consecutive real T3s made around it | Plain dated decision journal: question, gut, decision, review date |
| Tiers | Everything gets classed T1 to dodge process | T1 log + T3 gate only |
| `/life-review` | Skipped 2 consecutive quarters | 5-line checklist inside the Monday triage |
| `Season.md` | 2 seasons close untouched | Deleted; goals carry alone |
| Any adversary lens | Nothing load-bearing for 3 straight T3s | Retired, with a note why |

The fallback is always *smaller and still running*, never *the same thing plus more resolve*.

## 9. Duplicating this in another vault

1. Create the identity files (`Values.md`, `Principles.md`, `Goals.md`, `Season.md`) and a `decisions/` folder with an `_about.md` stating the tier rubric and frontmatter contract inside whatever your vault's "ongoing/identity" area is. Don't invent a new top-level home for it.
2. Copy the record template and the base/query views, adjusted to your paths.
3. Write the two loops as agent skills with the standard anatomy: trigger phrases, state file, numbered procedure, hard rules, kill criterion.
4. Wire the three health signals into whatever your vault already checks automatically.
5. Run the values interview. **Nothing works before this and everything works after it** — the rest of the system is just plumbing for applying that one file under stress.

**The load-bearing sentence, if only one thing survives the copy:**

> Set the weights in calm. Record the gut before the math. Let hostile questions attack the leader. Schedule the review that grades the reasoning — and let the weights learn from it.
