The map. Every folder, what it holds, why it is there.

# Vault Structure

```
00_System/
	AI/
		Claude/
			.claude/
				hooks/
				skills/
			Reference/
				Procedures/
			Roadmaps/
		    Scratchpad/
		    00_Home.md
		    CLAUDE.md
		    Decision Log.md
		    Field Notes.md
    Attachments/
    Templates/
01_Inbox/
02_Projects/
	Project Name/
		00_Home.md
		(subfolders per project needs)
03_Areas/
	Area Name/
		00_Home.md
		01_Projects/
		02_Ongoing/
		03_Knowledge/
		04_Reference/
04_Knowledge/
05_Reference/
90_Archive/
```

---

# 00_System/

### AI/Claude/

Claude Code launches from this folder. Reads and writes across the vault. Knowledge and reference are hook-protected.

| Subfolder   | What it holds                                                                                                                                                            |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| .claude/    | Skills (pre-built workflows like startup, close, research) and hooks (Python scripts that enforce rules and automate housekeeping). Shown by the `Vault Toolkit` plugin. |
| Reference/  | System documentation.                                                                                                                                                    |
| Roadmaps/   | Work items per project or area, with dependencies, triggers, and status.                                                                                                 |
| Scratchpad/ | Working documents, session memory, analysis drafts. Ephemeral.                                                                                                           |
| tools/      | Utility scripts, session state tracking.                                                                                                                                 |

Governance files sit at the Claude/ root:

| File            | Role |
| --------------- | ---- |
| 00_Home.md      | Claude's landing page. Links to areas, projects, and a capture zone for brain dumps between sessions. |
| CLAUDE.md       | Operational instructions loaded every session. Your rules, preferences, and standards in a file you edit directly. |
| Decision Log.md | Settled decisions. Claude checks this before revisiting any topic. |
| Field Notes.md  | Observations mature into principles. Active principles load at startup and shape Claude's approach. |

### Attachments/

All vault media lives here. Obsidian's attachment location points here, so anything you paste goes here automatically.

### Templates/

Folder templates (Vault Toolkit plugin). New files auto-populate frontmatter based on which folder they are created in.

> **Frontmatter** is the YAML block between `---` markers at the top of a note (created, status, type, description). Vault Toolkit auto-populates it. `status` drives Roadmap queries, `type` helps Claude interpret purpose, `recommended_path` tells Vault Toolkit where to relocate on right-click.

*Deeper: [[Conventions]] for the full spec.*


---

# Home Files

Every project, area, and the AI sandbox has a `00_Home.md` landing page. Each has a **Capture Zone** (brain dumps, triaged at close) and **Open Tasks** (from Roadmaps) pulled via a tasks block.

| Location | Holds                                      |
| -------- | ------------------------------------------ |
| Project  | goal, done condition, key documents        |
| Area     | domain statement, current focus, key files |
| Claude's | system-wide landing page                   |

*Deeper: [[Home Files]] for the full spec*


---

# 01_Inbox/

Capture zone. Not sure where it goes? Here. Sorted at weekly review or session close.


---

# 02_Projects/

Active efforts with end conditions. Every project gets a `00_Home.md`. Everything else is organized per project needs. No mandatory subfolder template.


---

# 03_Areas/

Ongoing responsibilities with no end state.

```
03_Areas/
	Area Name/
		00_Home.md
		01_Projects/      <- small, area-bound projects
		02_Ongoing/       <- living systems, no end date
		03_Knowledge/     <- area-specific knowledge
		04_Reference/     <- external material for this area
```

| Subfolder | What goes here |
|-----------|---------------|
| 01_Projects/ | Small, area-bound projects. Larger efforts go to root `02_Projects/`. |
| 02_Ongoing/ | Living systems with no end date. Content planning, client accounts, journaling. |
| 03_Knowledge/ | Your area-specific insights and frameworks. Not someone else's. |
| 04_Reference/ | External material for this area. Cross-cutting material goes to root `05_Reference/`. |

All four exist in every area. Empty is fine.


---

# 04_Knowledge/

Global, cross-cutting knowledge created by you. Principles, mental models, frameworks that proved universally useful. Highest-trust content. Hook-protected.


---

# 05_Reference/

Externally authored material. Not your analysis. The source material itself.

PDFs, book summaries, article clippings, research from other people. Cross-area reference lives here. Area-specific reference goes to that area's `04_Reference/`. Project-specific sources stay in the project. Hook-protected like knowledge.


---

# 90_Archive/

Completed or superseded work. Archive over deletion by default. Browsable without digging through Git history.

Mirrors the active vault structure:

```
90_Archive/
  02_Projects/          <- completed projects
  03_Areas/             <- archived area content
  04_Knowledge/         <- superseded knowledge
```

Navigate it the same way you navigate the active vault, just under `90_Archive/`.

---

**Next:** [[04 Navigating Architecture]] covers how to sort files, route decisions, and use the system day to day.
