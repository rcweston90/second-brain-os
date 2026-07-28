# Vault map

Single source of truth for how the vault is organized. Per-folder detail lives in each folder's `_about.md` ([folders/](folders/)); this is the overview.

**Organizing principle: lifecycle, not topic.** Folders answer one question — *how actionable is this right now?* — not *what is it about?* Knowledge flows: capture → act → maintain → reference → create → retire. The numeric prefix is sort order only; the name carries the meaning.

## The system

| Folder | Purpose | Lifecycle question |
|---|---|---|
| `0-inbox` | Unprocessed capture — anything new lands here first | *Have I processed this?* |
| `1-projects` | Active work with a defined outcome and an end (one subfolder + `_index.md` each) | *Am I working toward an outcome?* |
| `2-areas` | Ongoing responsibilities & identity — no finish line (`identity/`, `career/`, `lists/`, `topics/` — **the wiki**) | *Is this an ongoing part of me?* |
| `3-library` | Consumed reference someone else produced | *Is this input I consult?* |
| `4-studio` | Your original output (`writing/` pipeline, `sketches/`, `apps/`) | *Am I creating this?* |
| `5-journal` | Time-series daily notes, append-only | *When did this happen?* |
| `6-archive` | Cold storage — done / abandoned / >12mo | *Is this retired?* |
| `_attachments` | Central image/asset store — never browsed directly | — |
| `_templates` | Note templates | — |
| `_meta` | System artifacts — audits, dashboards, setup/operating guides for the system itself (never *your* creative output) | — |
| `_meta/IA-decisions` | Dated architecture decision records — running log | — |

## The three distinctions that carry the system

Get these right and filing is nearly automatic:

- **`1-projects` vs `2-areas`** — time-bound vs ongoing. Projects *end*; areas *continue*. No finish line ⇒ it's an area.
- **`3-library` vs `4-studio`** — consumed vs produced. Someone else's work ⇒ library; yours ⇒ studio.
- **`4-studio` vs `_meta`** — creative output vs *about the system*. Essays, sketches, specs ⇒ studio; anything that documents how the vault itself runs — including setup guides ⇒ `_meta`.

## Flow

```
capture → 0-inbox → weekly inbox-zero pass
                      ├── active, outcome-bound?    → 1-projects/<project>/_index.md
                      ├── ongoing / identity / ref? → 2-areas/{identity,career,lists,topics}
                      ├── consumed reference?       → 3-library/
                      ├── your own output?          → 4-studio/{writing,sketches,...}
                      └── not worth it?             → delete

writing matures → 4-studio/writing/  stage in frontmatter: seedling → draft → ready → published (or dead)
age out → 1-projects (>90d idle) → 6-archive/projects/ ; anything (>12mo) → 6-archive/
```

## Conventions

- **Attachments** live centrally; embeds use bare filenames (`![[image.png]]`) so they resolve from any folder — never path-qualify them.
- **Folder READMEs:** `_about.md`, tagged `type/folder-readme`. Authoritative for that folder — this is what an agent reads to know where things belong.
- **Writing stages live in frontmatter, not folders:** `stage:` (seedling · draft · ready · published · dead). The stage folders are holding locations only — *changing the property* is the signal. (Folders rotted because moving a file felt like work and nothing ever moved.)
- **Folder depth:** keep ≤ 3 levels.
- **Tags** carry cross-cutting concerns (`#status/`, `#type/`, `#topic/`); folders carry lifecycle position.
- **Basenames are unique vault-wide** — wikilinks resolve by basename, so duplicates silently break links.

## The discipline that makes it work

- **The weekly triage pass is non-negotiable.** The folder structure can't fix a process gap — only the cadence can. Two earlier generations died because triage never ran.
- **Promote or kill one writing piece each week.** The pipeline only works if state changes.
