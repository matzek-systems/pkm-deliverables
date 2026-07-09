The AI layer. How Claude Code works with the vault structure: session cycle, governance files, protection, research, and analysis.


# Session Cycle

Every session follows the same pattern: load state, work, save state.

**`/startup`** pre-loads your current state:

- Roadmaps (what's active, blocked, waiting)
- Decision Log (settled questions)
- Field Notes (system observations)
- Home files (capture zone items you left between sessions)

**`/close`** sweeps the conversation for decisions and routes them to governance files. A cleanup script deletes the session's working memory.

Each session maintains a working memory file on disk. If the session crashes or you close the terminal, that file survives.

### Parallel Sessions and Batch Close

Multiple sessions can run at once. Each reads the others' working memory at startup and avoids overlapping on the same files.

`/batchclose` closes all open sessions at once: reads all working memory, deduplicates overlapping decisions and observations, merges before writing governance files.

### Weekly Review

`/weekly-review` runs at the end of your workweek. Triages your inbox, checks for stalled projects, promotes durable observations into Active Principles, prunes stale ones.

*Deeper: [[Session Startup Checklist]], [[Session Close Checklist]], [[Weekly Review]]*

# Session Cycle Diagram

```
 =====================
|    Session Start    |----------------------┐
 =====================                       |
   ^                                         | pre-loads
   |                                         v
┌──────────────────────────┐          ┌──────────────┐
|          VAULT           |<-------->|    Claude    |
|                          |  reads   |   Session    |
| ┌──────────────────────┐ |    +     |              |
| |  Governance Core     | |  writes  |  shaped by   |
| |  (auto-allowed)      | |          |  CLAUDE.md   |
| |                      | |          |    + skills  |
| |  Roadmaps            | |          └──────────────┘
| |  Decision Log        | |                 |
| |  Field Notes         | |               calls
| |  Session RAM         | |                 v
| |  Scratchpad          | |           ┌──────────────┐
| └──────────────────────┘ |<----------|  STRUCTURED  |-- calls --┐
|                          |  writes   | CAPABILITIES |           v
| ┌──────────────────────┐ |           |   /skills    |   ┌────────────┐
| |     PROTECTED        | |     ┌---->|   Research   |   | External   |
| |  Knowledge           | |     |     |   Analysis   |   | Sources    |
| |  Reference           | |     |     └──────────────┘   | web, X, yt |
| └──────────────────────┘ |     |                        └─────┬──────┘
└──────────────────────────┘     |                              |
   |  ^                          |                              |
   v  | writes to             enforces                        writes
 =====================      ┌────────────┐                      |
|    Session Close    |     |   HOOKS    |<─────────────────────┘
 =====================      | sandbox    |
                            | auto-commit|
                            | git sync   |
                            └────────────┘
```


---

# The Triad

### Roadmaps

Tracks work items per project or area.
- **Status:** active, ready, blocked, waiting, deferred, done
- **Dependencies:** `depends: WI-3` means this can't start until WI-3 is done
- **Triggers:** deferred items activate automatically when a condition is met
- **Where I'm At:** each Roadmap includes a narrative section with current project state, blockers, and next steps. Session close rewrites this automatically. The Vault Dashboard displays it and lets you quick-append notes between sessions.

Claude reads all Roadmaps at startup and reports what's ready, what's blocked, and what's waiting on you.

### Decision Log

When you settle a question, it goes here: what was decided, why, when. Claude checks the log before revisiting any topic.

### Field Notes

Three layers:

- **Session Log** (observations): raw notes from individual sessions. At session close, the close skill sweeps the conversation and writes new observations here.
- **Emerging Patterns**: observations that keep surfacing across multiple sessions. Not yet proven, but worth watching.
- **Active Principles**: patterns that have survived long enough to trust as defaults. Load at every startup and shape how Claude approaches your work.

Weekly review drives promotion. `/weekly-review` checks for patterns that have matured and proposes elevating them. You approve or reject.

*Deeper: [[Field Notes]] for the three-tier structure. [[Governance Architecture]] maps the full Triad.*


---

# How the System Protects Your Work

Five mechanisms, all editable plain text:

1. **Sandbox.** Writes to knowledge and reference folders are blocked.
2. **Destructive command blocking.** Git history rewrites, force pushes, and hard resets are blocked before they execute.
3. **Auto-commit.** Every file change is committed to Git with a contextual message. Every action is reversible.
4. **Governance snapshots.** Before any destructive operation and at session close, a timestamped copy of your governance files is written to disk.
5. **Completion enforcement.** Multi-step skills track which phases completed via filesystem markers. Nothing gets silently skipped.

Remote Git backup pushes automatically when a session ends.

*Deeper: [[Hook System Architecture]], [[Multi-Machine Setup]]*

## CLAUDE.md

The instruction set that shapes how Claude behaves. Loaded every session. The Triad tracks state. CLAUDE.md tracks behavior.

| Section              | Purpose                                | Can you edit?                    |
| -------------------- | -------------------------------------- | -------------------------------- |
| Vault Structure      | Tells Claude where things are          | Edit if you rename folders       |
| Reasoning Discipline | Rules for how Claude thinks            | Edit freely                      |
| Safety Rules         | Sandbox boundaries, write restrictions | Understand before changing       |
| Context Management   | Session RAM, compression recovery      | Hesitate to change               |
| Key Files            | What Claude reads and when             | Edit if you add governance files |
| My Rules             | Your personal preferences              | This is yours. Edit freely.      |

"My Rules" at the bottom is where you put your instructions. Everything above it is load-bearing infrastructure.


---

# Research

Built-in research with auto-selected depth:

| Level | Sources | When it triggers |
|-------|---------|-----------------|
| Quick | Vault + quick web lookup | Factual questions, status checks |
| Standard | Vault + web + Reddit + YouTube | Needs external context, comparative analysis |
| Deep | Standard + Twitter/X + adversarial testing | Strategically important, user explicitly requests |

Each output includes:
- **Source intelligence**: prioritizes source types based on the question
- **Methodology**: what was searched, what was kept, what was discarded and why
- **Conviction scoring** (Standard/Deep): 0-100 score with reasoning for why it isn't higher
- **Adversarial testing** (Deep only): counter-arguments generated against findings before delivery

Quick works now with no setup. Standard and Deep need API keys covered in [[06 Advanced Use]].

*Deeper: [[Deep Research]], [[Deep Search Pattern]], [[Research Quality Gates]], [[Research Output Template]]*


---

# Structured Analysis

Two modes for questions that need more than a single answer.

**Dialectic.** You have a proposition ("we should do X").
- **STEELMAN** builds the strongest case FOR
- **DEFENDER** builds the strongest case AGAINST
- For bigger questions, it auto-scales: scouts research angles, specialists synthesize and rank, STEELMAN stress-tests

**Prism.** You have a question, not a position ("what's the best approach to X?").
- **Scouts** investigate different domains in parallel
- **Signal** synthesizes patterns across findings
- **Critic** checks whether the analysis holds up
- Output is a scored assessment with specific recommendations
- Also auto-scales

Both produce structure and findings, not finished prose. Use them to stress-test ideas and surface angles you'd miss.

*Deeper: [[Multi-Agent Analysis]]*


---

# What Ships With the System

## Skills

| Command | What it does |
|---------|-------------|
| `/startup` | Pre-loads Roadmaps, Decision Log, Field Notes, Home files. Checks MCP connection and parallel sessions. |
| `/close` | Sweeps session for decisions, observations, status changes. Routes to governance files. Cleans up working memory. |
| `/batchclose` | Closes multiple sessions. Deduplicates and merges before writing. |
| `/weekly-review` | Inbox triage, stale project check, pattern promotion, system health. |
| `/research` | Structured research with auto-selected depth. |
| `/check` | Quality audit: file inconsistencies, stale cross-references, uncommitted changes. |
| `/check-updates` | Checks for new system updates from the updates repository. |

### Marshal Pattern

Complex workflows are the most likely to get steps skipped under pressure. Close, batchclose, and weekly review break into sub-skills that run in strict sequence. Each phase writes a completion marker before the next starts. If context compression fires mid-process, the marshal reads the markers and resumes from where it left off.

| Marshal | Sub-skills |
|---------|-----------|
| `/close` | cl-extract, cl-route, cl-cleanup |
| `/batchclose` | bc-sweep, bc-synthesize, bc-route, bc-cleanup |
| `/weekly-review` | wr-audit, wr-health, wr-learn, wr-wrap |

*Deeper: [[Skill Architecture]]*

## Hooks

| Hook | What it does |
|------|-------------|
| Sandbox | Blocks writes to knowledge and reference folders. |
| Bash sandbox | Blocks destructive Git commands. |
| Auto-commit | Commits every file change with a contextual message. |
| Auto-sync | Pushes changes to remote backup at session end. Startup pulls and reconciles. |
| Governance snapshot | Saves a timestamped copy of governance files before destructive operations and at session close. |

## Plugins

Two custom plugins ship with the vault.

### Vault Toolkit

One plugin, eight features, each independently togglable:

- **Home button**: ribbon icon, optional auto-open on launch
- **Folder templates**: auto-populates frontmatter in configured folders
- **File Mover**: right-click to archive or relocate to a `recommended_path`
- **Bidirectional frontmatter**: `supersedes` auto-populates `superseded_by` and `status: superseded` on the target
- **Code Viewer**: syntax-highlighted code files inside Obsidian
- **Dotfile visibility**: `.claude/` folder visible in file explorer
- **Tab path context**: parent folder shown in tab labels for disambiguation
- **Reading view in new window**: pop the current note out as a reading-view window

*Source: [obsidian-vault-toolkit](https://github.com/matzek-systems/obsidian-vault-toolkit)*

### Vault Dashboard

Visual project dashboard with up to three tabs, each surfacing one active Roadmap. Auto-discovers active roadmaps or pin specific projects in settings.

Per tab:
- **Where I'm At** narrative, editable inline via quick-append
- **Live session badge** when Claude is working on this project
- **Work items grouped by status** (Active, Blocked, Needs Testing, Ready, Waiting, Recently Done)
- **Per-WI actions** (add notes, mark done)

*Source: [obsidian-vault-dashboard](https://github.com/matzek-systems/obsidian-vault-dashboard)*


---

# Extensibility

- **Add skills.** Markdown file in `.claude/skills/`. Claude executes it when you type the command.
- **Edit CLAUDE.md.** Global rules, behavior changes, remove what doesn't fit.
- **Add rules.** Tell Claude a rule that should apply to specific files or paths, and it creates a scoped rule file in `.claude/rules/`. "Formal tone in client deliverables" lives here, not CLAUDE.md.
- **Create hooks.** Python script in `.claude/hooks/`. Runs before or after Claude takes an action.
- **Add MCP servers.** Connect Claude to any tool that speaks MCP.

Every piece is editable plain text. No compiled software, no locked configuration, no vendor dependency.


**Next:** [[06 Advanced Use]]
