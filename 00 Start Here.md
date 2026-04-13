# Start Here

This vault is a working system, not a template. The documents below explain how it all works so you can use it and make it yours.


> [!warning]
> Anything in this vault can be passed to Claude as context. If you have information you don't want shared with AI, keep it in a separate vault or outside this system.

## Prerequisites

- Active Claude subscription
- Obsidian installed


---

## How to Use This Series

Read the docs in order. Each one builds on concepts from the one before it.

Your vault ships with a **System Roadmap** that tracks your progress through setup. It lives at `Roadmaps/_System Roadmap.md`. If you ever get lost, check that file for your next step.


### [[01 Setup]] - Install

**Explains:**
- Getting your vault onto your machine (GitHub clone or zip)
- Claude Code installation and launch shortcut
- Required plugins (community + custom)
- Settings changes and Git setup

**After you're done:** Claude Code is running, plugins installed, Git tracking changes.


### [[02 Core Philosophies]] - Why

Three theses: the human decides (AI executes), information has governance (folders encode trust, hooks enforce it), state lives in files (session cycle, the Triad). The landscape section covers why most systems in this space build databases, not operating systems.

### [[03 Architecture]] - The Map

Full vault folder tree, each folder's purpose, area subfolders, home files, the AI working directory and governance files.

### [[04 Navigating Architecture]] - How to Use It

Knowledge vs reference, project vs area, sorting files, routing to the Triad, file lifecycle (promotion, supersession, project close, archiving), cross-linking.

### [[05 The System]] - The AI Layer

Session cycle (`/startup`, `/close`, `/batchclose`), the Triad in detail, protection mechanisms, research with conviction scoring, structured analysis (Dialectic and Prism), CLAUDE.md, skills, hooks, plugins.

### [[06 Advanced Use]] - Extensions

Deep research with external APIs, unattended task execution (Ralph Loops), Discord bot, multi-device sync.


**Let's dive in:** [[01 Setup]]
