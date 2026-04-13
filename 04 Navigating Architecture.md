How [[02 Core Philosophies]] and [[03 Architecture]] meet in practice.


---

# Knowledge vs Reference

**Did you write it, or did someone else?** Knowledge vs reference. **Tied to one area, or broadly useful?**

- **Area knowledge** (`03_Areas/.../03_Knowledge/`): your insight, specific to one domain
- **Global knowledge** (`04_Knowledge/`): your insight, proven across domains
- **Area reference** (`03_Areas/.../04_Reference/`): someone else's material, one area
- **Root reference** (`05_Reference/`): someone else's material, broadly useful
- **Project sources**: stays in the project folder

When sources conflict, Claude surfaces both claims with file paths. It does not pick one or synthesize middle ground. Global knowledge > area knowledge > reference. You decide. A project can override a knowledge note for its own scope without rewriting the original.

*Deeper: [[Conventions]]*


---

# Project vs Area

Can you write down what "done" looks like? If yes, it is a project. If no, it is an area.

**Projects have end conditions.** "Launch the website." "Ship the audit." When met, the project closes and archives. **Areas are ongoing.** Your consulting practice, creative side projects, health tracking. They evolve, never close.

Projects live inside areas when scoped to one domain, at root `02_Projects/` when they span areas. File Mover handles mistakes.


---

# Sorting Files

Not sure? `01_Inbox/`. The question is always **"what state is this in?"** not "what topic is this about."

| Situation | Goes in |
|-----------|---------|
| Your analysis of what worked for a client | `03_Areas/.../03_Knowledge/` |
| New product launch with timeline and deliverables | `02_Projects/Product Launch/` |
| Small one-week project within an area | `03_Areas/.../01_Projects/` |
| Content calendar (never "done") | `03_Areas/.../02_Ongoing/` |
| Article you found for your consulting work | `03_Areas/.../04_Reference/` |
| Book summary, broadly useful | `05_Reference/` |


---

# Routing to the Triad

When you have a thought, which governance file does it go in?

**Settled choice?** (pricing, tech stack, positioning) **Decision Log.** What you decided, why, when. Once logged, do not re-analyze.

**Something you observed?** (a client pattern, a workflow that worked, a failure mode) **Field Notes.** If the same observation recurs, it graduates to Emerging Patterns, then Active Principles.

**Work to be done?** (task, next step, blocked item) The right project's **Roadmap**, with status (active, ready, blocked, waiting).

**Not sure?** `01_Inbox/`. Process at session close or weekly review.

This is automatically done mid session and during session close. In all of my sessions I don't think I've manually edited these docs.

---

# File Lifecycle

## Knowledge Promotion

Promote area knowledge to global when it is: cross-cutting (2+ areas), general (not tool-specific), durable (two weeks without revision), and human-validated.

*Deeper: [[Knowledge Promotion Criteria]] and [[Field Notes]]*

## Supersession

When a note replaces another, add `supersedes: "[[Old Note]]"` in the new note's frontmatter. Vault Toolkit's Bidirectional feature auto-sets `superseded_by` and `status: superseded` on the old file. Claude skips superseded notes unless you are researching history.

## Finishing a Project

"Close this project" triggers extraction: decisions to the Decision Log, area insights to area knowledge, cross-cutting insights to global knowledge. Everything stays browsable in `90_Archive/` or you can delete it if you're feeling feisty.

*Deeper: [[Project Close]]*

## Archive by Default

Archive, not delete. Original folder structure mirrored. Search and wikilinks still work. Only mechanical files (empty templates, test output, temp scripts) get deleted.


---

# Cross-Linking

`[[Wikilinks]]` everywhere. For you, navigation. For Claude, context discovery: it follows links to pull connected notes. Search finds what you ask for. Wikilinks find what you did not know to ask for.

*Deeper: [[How Claude Code Searches Your Vault]]*


---

**Next:** [[05 The System]] covers the AI layer: session cycle mechanics, hooks, skills, and what Claude actually does at startup and close.
