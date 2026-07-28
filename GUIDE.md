# The Second Brain OS — the full replication guide

The complete architecture: skeleton, conventions, governance, loops, publishing, dashboard, and the decision OS. Prerequisites and the quick-start live in the [README](README.md).

---

## 1. The premise

Three ideas do most of the work. Everything else is implementation.

1. **Folders track lifecycle, not topic.** Every folder answers *how actionable is this right now?* — never *what is it about?* Topic lives in tags and links, where it can be many things at once.
2. **Capture is unconstrained; discipline moves to automated loops.** Write anywhere, including the vault root. Nothing is enforced at capture time. Agent-run loops drain the mess on a cadence and hand back a short decision list.
3. **Every loop carries a kill criterion.** A written condition under which the loop deletes itself. Friction that stops paying for itself gets removed, not tolerated.

The failure mode this design exists to prevent: two earlier generations of the same vault died by redesigning the taxonomy instead of running it. Capture hotkeys were never learned; the "non-negotiable weekly triage" never ran once. **The fix was mechanical wiring and automation, not a better taxonomy or more resolve.**

---

## 2. Folder skeleton

Seven numbered lifecycle folders plus underscore-prefixed system folders. The number is sort order only; the name carries the meaning.

| Folder | Holds | Lifecycle question |
|---|---|---|
| `0-inbox` | Unprocessed capture | *Have I processed this?* |
| `1-projects` | Time-bound work with a defined outcome | *Am I working toward an outcome?* |
| `2-areas` | Ongoing responsibility & identity — no finish line; home of the wiki (`topics/`) | *Is this an ongoing part of me?* |
| `3-library` | Reference someone else produced | *Is this input I consult?* |
| `4-studio` | Original output — writing, sketches, apps | *Am I creating this?* |
| `5-journal` | Daily notes, append-only | *When did this happen?* |
| `6-archive` | Done, abandoned, or >12 months cold | *Is this retired?* |
| `_attachments` | Central asset store, never browsed | — |
| `_templates` | Note templates | — |
| `_meta` | System artifacts: audits, dashboards, sync state, setup guides for the system itself | — |
| `_meta/IA-decisions` | Dated architecture decision records | — |

**The three distinctions that carry the system.** Get these right and filing is nearly automatic:

- `1-projects` vs `2-areas` — time-bound vs ongoing. Projects *end*; areas *continue*. No finish line means it's an area.
- `3-library` vs `4-studio` — consumed vs produced. Someone else's work is library; yours is studio.
- `4-studio` vs `_meta` — creative output vs *about the system*. Essays and sketches are studio; anything that analyzes, tracks, or documents how the vault itself runs — including setup guides, even when authored — is `_meta`. Setup guides for a system live in the system's settings.

**Structural rules:**

- Folder depth ≤ 3 levels.
- Every lifecycle folder has an `_about.md` (tagged `type/folder-readme`) that is authoritative for that folder — this is what an agent reads to know where things belong.
- One `README.md` at root is the vault map and the single source of truth for organization.
- Note basenames must be **unique vault-wide** — wikilinks resolve by basename, so duplicates silently break links. Check before creating or renaming; grep backlinks before any rename. Moves are safe; renames are not.
- Attachments live centrally and are embedded with bare filenames (`![[image.png]]`) so they resolve from any folder. Never path-qualify them.
- The vault root is a **sanctioned capture surface**, not a mess to nag about. It gets drained weekly.

---

## 3. Conventions layer

**Frontmatter as state.** State lives in properties, not folder position. The important instance is the writing pipeline: `stage: seedling | draft | ready | published | dead`, plus `theme:` and `updated:`. The stage folders are holding locations only — *changing the `stage` value* is the signal. This was a deliberate correction: folders rotted because moving a file felt like work and nothing ever moved.

**Tags carry cross-cutting concerns**, folders carry lifecycle position. Three namespaces: `#status/`, `#type/`, `#topic/`.

**Projects** get one folder in `1-projects/` with an `_index.md` landing page containing: outcome (one sentence describing done), deadline, status, working notes, and a **dated decisions log**. Projects idle 90 days are archived.

**The wiki** lives *inside* the vault, under `2-areas` (`topics/`) — not a separate vault or a new top-level folder. A topic page is a hub in your own words: current position, links out to the projects, library notes, and essays that are the evidence, and the open questions. A subject earns a page when it recurs across two or more projects or essays; before that, the material stays where it is. Projects link into topics, topics link out to evidence — when a project archives, its topic page is the residue that survives. Keeping the wiki in the same vault is what lets the maintenance loops tend it like everything else, and what makes project ↔ wiki linkages one wikilink instead of a cross-system reference. (Browse the anonymized structure under [`structure/`](structure/).)

**Privacy markers** are structural, not conventional: `visibility: private` frontmatter means the content is never quoted into any other note, summary, or anything in the publishing pipeline.

---

## 4. Tooling stack

Deliberately thin. Every tool that failed to get adopted twice was removed rather than re-attempted.

| Layer | Choice | Why |
|---|---|---|
| Store | Obsidian vault, plain markdown, git-versioned | Portable, agent-readable, diffable |
| Structured views | Obsidian **Bases** (`.base` files) + **Dataview** | Pipeline and health views without maintaining lists by hand |
| Entry point | **Homepage** plugin → a dashboard note on startup | The system's state is the first thing seen |
| Tasks | tasks-plugin format inside project `_index.md` | Actions live next to the work, not in a separate app |
| Versioning | obsidian-git | Rollback points before structural changes |
| Meeting capture | A meeting recorder with folders (any will do) | The only capture habit that already existed |
| Agent | Claude Code, run against the vault directory | Does the filing; the human only decides |

**Explicitly skipped:** Templater hotkeys, QuickAdd, Web Clipper. All three depend on a capture-time reflex that failed to form twice. The loops make them unnecessary.

### How it's orchestrated in Obsidian

Obsidian is the store and the viewport; Claude is the runtime. The vault is a plain folder of markdown, so both can operate on the same files without an integration layer — Obsidian renders what the agent writes, the agent reads what you write in Obsidian.

**Plugin inventory:**

| Plugin | Kind | Job in this system |
|---|---|---|
| **Bases** | Core (built-in) | The pipeline views — writing pipeline, decision records. Native database views over frontmatter; this is what makes state-in-frontmatter usable |
| **Daily notes** | Core | Pointed at the journal folder, format `YYYY-MM-DD` — capture lands where the routing table expects it |
| **Templates** | Core | Serves `_templates/` (project index, decision record) |
| **Sync** | Core | Cross-device sync; git is the *versioning* layer, sync is the *transport* layer |
| **claude-sidebar** | Community | **The keystone** — runs Claude Code in an Obsidian sidebar pane (see below) |
| **homepage** | Community | Opens the dashboard on startup, so the health lights are the first thing seen |
| **obsidian-git** | Community | Versioning + rollback points before structural changes |
| **open-in-terminal** | Community | One keystroke from vault to a full terminal Claude Code session for heavy work |
| **obsidian-linter** | Community | Keeps frontmatter shape consistent — matters because frontmatter is state |
| **BRAT** | Community | Installs/updates plugins not in the community store |
| **Dataview** | Community | Powers the dashboard's health table — install it and enable "JavaScript Queries" or the table renders as a code block. The `.base` views work without it |

**How Claude works as a plugin.** The claude-sidebar plugin embeds the Claude Code CLI in a sidebar pane with the vault root as its working directory. That one fact does all the work:

- The vault's `CLAUDE.md` loads automatically every session — the constitution (routing, privacy, anti-patterns) is enforced without ever being pasted into a prompt.
- `.claude/skills/*/SKILL.md` become slash commands — the triage loop, the decision loop, the review loop are typed into the sidebar like any command.
- The agent reads and writes the same markdown Obsidian is displaying: file a note, flip a `stage:` property, append a decision record — the Bases views and dashboard update live, no export/import step, no plugin API.
- MCP connections (meeting recorder, GitHub) ride along with the Claude session, so capture loops pull external sources straight into vault files.

Two surfaces, one brain: the **sidebar** for in-context work — triage menus, a decision run, quick filing while reading a note — and the **terminal** for long sessions like a full audit or the publishing pipeline. Both load the same `CLAUDE.md` and the same skills, because both are just Claude Code with the vault as cwd. Nothing about the system lives in the plugin; the plugin is a window. If the sidebar plugin died tomorrow, the system runs unchanged from the terminal — which is exactly the disposability rule the loops live under, applied to the tooling.

**The startup-to-loop cycle in practice:** Obsidian opens → homepage lands on the dashboard → the health lights say whether anything is owed → if something's red, open the sidebar and run the loop it names → the agent files/stages/records, Obsidian re-renders → done. On a green day the whole interaction is reading one table.

---

## 5. The governance layer — `CLAUDE.md`

This is the single most replicable artifact and the thing that makes the vault agent-operable. A vault-root `CLAUDE.md` that every Claude Code session loads automatically. Five sections:

1. **Privacy — hard rules.** Named as rules, not preferences. Never open files whose names contain "code", "secret", "password", or "key"; never quote `visibility: private` notes; pass these rules to every subagent.
2. **IP hygiene.** What may never leave the vault — employer artifacts, client-identifying material, anything from the day job. *Patterns yes, artifacts no.* Points at a checklist of record.
3. **Routing table.** A literal thing → destination table. This is what lets an agent file without asking.
4. **House conventions.** Unique basenames, project structure, where state lives.
5. **Anti-patterns.** The documented failure modes, written as prohibitions: *do not redesign the taxonomy; do not create new meta/strategy documents unless explicitly asked; when a loop keeps getting skipped, shrink the loop rather than adding a discipline doc.*

Section 5 is the non-obvious one. Without it, an agent asked to "improve the vault" will helpfully propose a new taxonomy — the exact behavior that killed two previous generations.

Project-level `CLAUDE.md` files nest under this for anything with tighter rules (confidential work gets its own redaction rules).

---

## 6. The loops

Seven Claude Code skills in `.claude/skills/<name>/SKILL.md`. Each is a short procedural document — roughly 30–50 lines — not a program.

| Loop | Cadence | Job | Human time |
|---|---|---|---|
| capture-sync | Anytime, safe daily | Pull recordings from the meeting recorder, route by source folder, extract actions + essay seeds | ~0 |
| vault-triage | Weekly | Drain root + inbox; auto-file the obvious, offer a menu for the rest | ≤ 15 min |
| ship-next | Weekly | Run the next essay through editorial crit, stage it for publishing | 30 min editor session |
| publish | Event-triggered | Staged essay → MDX pull request on the site → filed as published | Minutes |
| activity-log | Biweekly | Record PR activity as private career evidence | ~0 |
| decide | When a real decision arrives | Tier-triage it, run the gut-first matrix + adversaries, file the record | Varies by tier |
| life-review | Quarterly | Close the season, advance/kill goals, run due decision reviews, check values drift | ~45 min |

### Anatomy of a loop (copy this shape)

Every skill has the same five parts:

1. **Frontmatter** — `name` and a `description` written to include the natural phrases that should trigger it ("drain the root", "what should I ship").
2. **State file** — a note in `_meta/sync/` holding `last_run` and a run log. Loops are incremental; they never re-process history.
3. **Procedure** — numbered steps, concrete enough to follow without judgment calls.
4. **Hard rules** — the things it must never do, restated locally rather than assumed from `CLAUDE.md`.
5. **Kill criterion** — the condition under which the loop is deleted.

### The division of labor

> The human only ever *decides where a note goes*. The agent does every move.

The triage loop is the clearest expression. It executes last week's approved decisions first, auto-files the no-judgment cases without asking (empty stubs to trash, clippings to the library with a "my take" stub appended, dated files to the journal, images to attachments), and then presents each remaining note as a **ranked menu of 2–4 real destinations** — best guess first, one line of why. The user picks; the agent files it immediately. Output is capped (~12 notes per run) and a dated triage note at the root serves as the receipt and the next run's input.

Two safety rules make it survivable: "delete" always means move to `.trash/`, never `rm`; and it never invents folders — if nothing fits, the note goes on the decision list.

### Capture routing by source, not by content

The capture loop routes recordings by which folder they were recorded into, because the source folder is a reliable privacy signal and the content is not:

| Source folder | Destination | What's written |
|---|---|---|
| Work | project `meetings/` | Full summary + action items |
| Solo thinking | project folder or writing seedling | Summary + source link |
| **Personal** | journal | **Link-stub only — title, date, id. No content, ever.** |
| External 1:1s | library transcripts | Summary; essay-relevant bits flagged as seedlings |

The personal rule is a bright line, not a heuristic: content is never even fetched. When in doubt about a recording, treat it as personal.

---

## 7. The publishing subsystem

The vault's whole reason to exist is output, so the writing path gets more machinery than anything else. The diagnosis it was built from: *the shipping valve was closed, not the writing engine* — six to eight essays sat at near-publishable quality while the pipeline built to publish them ran zero times.

**Three artifacts:**

- **A voice constitution** — the rubric every editing agent works under. Registers, protected signature moves, and a **ban list** of AI-writing tells that get flagged as lint. Its stated job is *refusal*: refusing sanitized substitutes for what the author actually sounds like. Rule of last resort: when torn between the author's rough phrasing and a smoother one, keep the author's.
- **An editorial gauntlet** — a crit an essay must survive before promotion. Not separate agents: three **prompt lenses one agent adopts in turn**, each trying to stop the piece for a different reason. The Skeptic attacks the argument (which claim is weakest?); the Cut-30% Editor attacks the length (which *whole ideas* go?); the Hostile Commenter attacks from the other side (what's the top dismissive comment, and does the essay pre-empt it?). They collapse into one stamped verdict block — SHIP AS-IS / SHIP AFTER EDITS / NOT READY — with prioritized edits the user accepts or rejects one by one.
- **A single-slot queue** — the ready-to-publish folder holds **exactly one** essay at all times. When it ships, the next is promoted. The staging loop works only from existing drafts; it never generates a new essay.

**Effectiveness is tracked, not assumed.** Accepted-edit counts are logged at the bottom of the voice constitution, so the kill criterion — zero accepted edits across two consecutive essays means strip the personas and revert to a plain copyedit — can be checked rather than argued about.

**Two gates before anything goes public:** an IP check (grep for employer names; confirm nothing load-bearing) and platform cuts prepared as a companion file (canonical site version, LinkedIn teaser, X thread). The agent prepares; **the human posts.** The agent never merges the PR and never posts to social.

---

## 8. The dashboard

One dashboard note at root, opened on startup, holding a health table that checks the system's own rules with Dataview JS and renders a red/yellow/green light per row:

| Signal | Target |
|---|---|
| Loose files at root | ≤ 5 |
| Inbox age | nothing older than 14 days |
| Essays staged for publishing | exactly 1 |
| Active projects idle > 90 days | 0 |
| Pipeline pieces touched in ≤ 14 days | ≥ 1 |
| Decisions past their review date | 0 |
| Goals file freshness | updated within 90 days |
| Current season | open and unexpired |

Below it: the loop table with when to run each, and live queries for the root drain and the inbox. On triage day the dashboard prints its own reminder.

The point is that **the rules are checked for you.** A rule that requires remembering is a rule that doesn't run.

---

## 9. Build order for a duplicate

**Sitting one — skeleton and governance (2–3 hours, most of it delegable to an agent):**

1. Create the seven lifecycle folders plus `_meta`, `_templates`, `_attachments`. Write an `_about.md` per folder — purpose, rule, the lifecycle question it answers.
2. Write `README.md` as the vault map: the folder table, the flow diagram, the conventions, and the honest note about what previously failed.
3. Write vault-root `CLAUDE.md`: privacy rules, IP hygiene, routing table, house conventions, anti-patterns.
4. Point daily notes at the journal folder with a `YYYY-MM-DD` format. Enable Dataview and Bases. Set the homepage to the dashboard.
5. Build the dashboard with the health table wired to actual paths.

**Sitting two — the loops (one sitting):**

6. Write the triage loop first. It's the one that keeps the vault alive, and it's the one that tests whether the routing table in `CLAUDE.md` is actually decidable.
7. Add the capture loop for whatever the existing capture habit is. Do not invent a new capture habit — instrument the one that already exists.
8. Add the shipping loops last, once there's a backlog worth shipping.

**Weeks one and two — run it manually.** Loops start hand-triggered. Automate on a schedule only after a loop has survived two clean weeks. Habit before machinery.

**Set a health-check date** roughly four weeks out with numeric targets. Any loop skipped two consecutive weeks gets *shrunk* at that review, not re-committed to.

---

## 10. Extension: the personal decision OS

The same architecture extends past knowledge work into life decisions. The transferable idea: **"weigh evenly" means pre-committing the weights before the decision arrives** — a values file with weights summing to 100 (set in calm), red lines and anti-goals that veto rather than weigh, and decision records scored against those pre-committed weights when it counts.

Four layers by stability:

- **Identity** — 5–8 weighted values extracted in one 45-minute interview from real episodes (peaks, regrets, envy, where the time and money actually went), plus committed if-then principles. Red lines are non-negotiables phrased as vetoes.
- **Goals** — 1/3/10-year horizons, each goal tied to the value it serves (a goal serving no value is usually someone else's goal, imported), plus anti-goals: what success is not allowed to cost. One-focus quarterly "season" with a named deliberately-neglected list.
- **Decisions** — three tiers by reversibility × stakes: a one-line log for the reversible-and-cheap, a 15-minute mini-matrix for the middle, and a full record for the irreversible-and-consequential — reframed question, explicit status-quo option, **gut call recorded before any scoring**, veto screen, weighted matrix, five adversarial lenses (regret at 10 days/months/years; steelman the loser; base rates with mechanism; full opportunity cost; who else the decision binds — they get a conversation, not a proxy score), falsifiable decision, pre-mortem, scheduled review.
- **Reviews** — every record gets a scheduled review asking *did the weights predict what actually mattered?* Weight edits flow back into the values file, dated and logged. Without this the matrix is a form; with it, the weights converge on the person you actually are.

Two loops run it (decide, life-review), each with the standard anatomy — state file, hard rules, kill criterion — and three health rows on the dashboard. Everything lives inside the existing areas folder; no new taxonomy.

**Full build:** the complete setup guide — artifacts in steady state, the ten-part T3 record, the values interview, kill criteria — is in [DECISION-OS.md](DECISION-OS.md).

---

## 11. What to copy and what not to

**Copy:**

- The lifecycle-not-topic folder model and the three distinctions.
- `CLAUDE.md` as a governance file, especially the anti-patterns section.
- The loop anatomy: state file, procedure, hard rules, kill criterion.
- The human-decides / agent-files division of labor.
- Health signals checked automatically on the homepage.
- Pre-committed weights for decisions, and the review that calibrates them.

**Don't copy:**

- The specific project folders, essay queue, or capture-tool configuration — those are contents, not structure.
- The number of loops. Seven is what this workload needed. Start with triage and add only when a specific pain recurs.
- Any plugin adopted for aspirational reasons. This vault's stack shrank on purpose.

**The load-bearing sentence, if only one thing survives the copy:**

> Write anywhere. All discipline lives in automated loops that drain the mess and hand back a capped decision list. The human does judgment; the agent does janitorial.

---

## Appendix — replication gotchas

- **Decide deliberately whether agent skills are versioned with the vault.** This vault's first loops originally lived only in an untracked second location — anyone cloning the repo got the documentation of the loops but not the loops. Keep skills in the tracked `.claude/skills/`.
- **Templates drift out of sync with the taxonomy faster than anything else** — this vault's inbox template still routed to folder names from two generations ago. Audit `_templates/` whenever folders change.
- **Don't maintain a hand-written copy of what the skill files already say.** Two sources of truth for the same procedures will diverge; either generate the human-readable readout or accept that the `SKILL.md` files win.
