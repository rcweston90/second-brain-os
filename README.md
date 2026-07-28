# The Second Brain OS

A complete architecture for an **agent-operated Obsidian vault**: seven lifecycle folders, a governance file the agent obeys, automated maintenance loops with built-in kill criteria, a publishing pipeline, and a personal decision system — all runnable by Claude Code working directly on your markdown files.

This repo is a replication guide, not a template. It documents a system that's been through three generations (two failed — the failures are part of the guide) so you can rebuild it in your own vault in about two sittings.

**Who it's for:** anyone whose beautifully organized notes system keeps dying of neglect, and who is willing to let an AI agent do the janitorial work while keeping every judgment call for themselves.

**→ The full architecture lives in [GUIDE.md](GUIDE.md).** This page is just what you need and what to do.

**Browse the system directly:**
- **[structure/](structure/)** — the annotated skeleton: a [system map](structure/system-map.md) (mermaid diagrams of the loops and decision gates), the [vault map](structure/vault-map.md), and the per-folder `_about.md` files an agent reads to know where things belong.
- **[DECISION-OS.md](DECISION-OS.md)** — the optional life-decision extension in full: weighted values, tiered decision records, adversarial review, and the calibration loop.

---

## What you need

**Required:**

- [Obsidian](https://obsidian.md) (desktop) — with core plugins **Bases** and **Daily notes** enabled
- [Claude Code](https://claude.com/claude-code) — CLI installed and authenticated; verify `claude` runs in a terminal first
- **git** — `git init` inside the vault; ignore `.obsidian/workspace*`

**Community plugins** (Settings → Community plugins):

- **Dataview** — then enable *JavaScript Queries* in its settings, or the dashboard renders as a dead code block
- **BRAT** — needed to install claude-sidebar (it's not in the official store)
- **claude-sidebar** (via BRAT) — runs Claude Code in an Obsidian sidebar pane; the keystone integration
- **Homepage** — opens your dashboard on startup
- **obsidian-linter** — keeps frontmatter well-formed (frontmatter is state in this system)
- *Optional:* obsidian-git, open-in-terminal

**Optional tools:** a meeting recorder with folders (only if you already have that habit — instrument the capture habit you have, don't adopt a tool to justify a loop), and the GitHub CLI for the publishing loops.

## What to do

1. **Install and verify.** Fresh vault — don't retrofit a live one yet. Confirm Claude Code runs against it.
2. **Sitting one (2–3 hours): skeleton + governance.** Build the seven lifecycle folders, the per-folder `_about.md` files, the vault map, the `CLAUDE.md` constitution, and the self-checking dashboard. [GUIDE.md §9](GUIDE.md#9-build-order-for-a-duplicate), steps 1–5 — most of it delegable to the agent itself.
3. **Sitting two: the loops.** Triage first (it tests whether your routing table is actually decidable), then a capture loop for whatever habit you already have, shipping loops last. §9, steps 6–8.
4. **Weeks one and two: run everything manually.** Automate nothing until a loop survives two clean weeks. Habit before machinery.
5. **Optional: the decision OS.** Extend the same architecture to life decisions — pre-committed value weights, tiered decision records, scheduled reviews. [GUIDE.md §10](GUIDE.md#10-extension-the-personal-decision-os).
6. **Four weeks in: health check.** Numeric targets, and any loop skipped two consecutive weeks gets *shrunk*, not re-committed to.

## The premise

1. **Folders track lifecycle, not topic** — every folder answers *how actionable is this right now?*
2. **Capture is unconstrained; discipline lives in agent-run loops** that drain the mess and hand back a capped decision list.
3. **Every loop carries a kill criterion** — a written condition under which it deletes itself.

> Write anywhere. All discipline lives in automated loops. The human does judgment; the agent does janitorial.

Full details, rationale, and the documented failure modes: **[GUIDE.md](GUIDE.md)**.
