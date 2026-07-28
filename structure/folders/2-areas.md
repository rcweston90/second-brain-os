# 2-areas

**Purpose:** Ongoing responsibilities and identity — no finish line. Things you maintain a standard in, not complete.

## The category map

Each area is a standing part of life you steward. The self-facing ones are built as **structured databases and dated logs, not prose pages** — queryable and retrieved on demand, never an always-on "about me" blob. Every one carries a **freshness mechanism** (a review date, an append-only log, a cadence) so it stays true instead of fossilizing. All of it is `visibility: private` and loaded *through a loop when relevant* — see the "retrieval over conditioning" principle in the [system map](../system-map.md).

| Area | Holds | Form | Stays fresh via |
|---|---|---|---|
| `identity/` | values, goals, principles, the decision log, credentials (`codes/`) | files + records | the decision review flywheel |
| `career/` | portfolio, branding, case studies, evidence log | files + a Base | activity log, review |
| `people/` | who matters — a relationship database with context & cadence | a `.base` database | *due for a check-in* (a `next_contact` date) |
| `health/` | health & fitness log, metrics, providers | dated log | append-only + periodic review |
| `finances/` | accounts overview, subscriptions, money goals (*credentials stay in `identity/codes/`*) | overview + Base | quarterly review |
| `home/` | home, living, logistics | notes/checklists | as-needed |
| `lists/` | books, resources, reference lists | notes | as-needed |
| `topics/` | the wiki — one hub page per recurring subject | notes | recurrence |

**Rule:** if it's something you *are* or *steward* indefinitely, it's here. If it has a finish line, it belongs in `1-projects`.

**The discipline — so a rich self-layer doesn't rot into a "mausoleum of old selves":** structured over prose · differentiated over one blob · every store carries a forcing function · private and gated, never ambient. A category with no freshness mechanism is decoration.

## The database pattern (worth copying)

The self-facing areas reuse the shape of the decision system: a small frontmatter contract per note, a `.base` view over the collection, and a **date field that drives an "overdue" view**. Example — a relationship database:

```yaml
type: person
relationship: family | friend | colleague | mentor
circle: inner | close | periphery
last_contact:   # date
next_contact:   # date — drives the "due for a check-in" view
visibility: private
```

The `next_contact` date is the forcing function: a view filtered to `now() > next_contact` surfaces exactly who you've let slip — the same trick as the decision system's `review_by`. That date field is what separates a living database from a stale address book.

**Lifecycle question this folder answers:** *Is this an ongoing part of who I am or what I maintain?*
