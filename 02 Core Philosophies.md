The largest differentiator of the leverage you can pull from an AI system is the quality of the context you can pull from. 

This is the thesis. Everything below explains how.


---

# The Landscape

The idea of connecting AI to your notes has exploded. Dozens of projects now combine Obsidian with AI agents like Claude Code. Most of them build a database.

A database answers "what do I know?" An information operating system answers "what do I know, what have I decided, what am I doing, and what do I need to do next?"

This system is built to be the latter.

Three philosophical positions make this work.


---

# The Human Decides, the AI Executes

The danger of using AI is reliance on it for intent. In abstract creativity (artistic style, taste, vision) humans currently outperform. This means the human must be the visionary and the AI must be the executor.

So what does the AI do?

1. **Recall.** Ask your vault questions. "What decisions have I made about X?" AI reads your notes, connects them, and answers from your own work.

2. **Research.** AI already knows your projects, frameworks, and terminology. It uses this as the research baseline, not generic training data.

3. **Organization.** Claude tracks work items, maintains session-to-session memory, and keeps files, decisions, and priorities current so the next session picks up where the last left off.

4. **Execution.** Describe the output you want. AI figures out how to produce it. Code, documents, analysis, file operations.

The steam engine replaced the ox. The farmer still decided what to grow. Technology changes the execution path. It has never replaced the visionary. Someone must swing the hammer.

*Deeper: [[05 The System]] covers the mechanical details of how human authority is enforced.*


---

# Information Has Governance

Trust is encoded in the folder structure. `01_Inbox` is lowest trust. `05_Reference` is highest. `90_Archive` is historical, not higher trust. The number is the rank.

[[03 Architecture]] has the full tree. The short version:

| Folder | Trust Level | What Lives There |
|--------|------------|-----------------|
| `01_Inbox/` | Unprocessed | Anything you capture but have not sorted yet |
| `02_Projects/` | Active work | Efforts with end conditions |
| `03_Areas/` | Ongoing | Domains with no end date (clients, practices, interests) |
| `04_Knowledge/` | High trust | Your insights and frameworks, proven across domains |
| `05_Reference/` | Evidence | External material. Not your analysis. |
| `90_Archive/` | Historical | Completed or superseded. Browsable, not active. |

## Mechanical Enforcement

If a rule matters, it is enforced by code, not by instructions. This boils down to a fundamental I found: **"Conventions cannot force their own exercise; behavioral rules fail under task pressure. When a rule must be followed without exception, enforce it mechanically."**

Hooks (Python scripts on Claude Code actions) physically block writes to knowledge and reference. Destructive git commands are blocked. Auto-commit tracks every change.

We will also dive deeper into this when we talk about marshal patterns in [[05 The System#Skills]].

## Knowledge Maturation

Observations live in the session log. Recurring observations graduate to emerging patterns. Durable patterns earn promotion to active principles, which load at every startup and shape how the AI approaches your work.

Your system gets smarter over time because your knowledge is graduating through trust levels that you control. [[04 Navigating Architecture]] covers sorting, promotion, and conflict resolution.

*Deeper: [[Field Notes]] tracks the three tiers. [[Knowledge Promotion Criteria]] covers when notes graduate across folders.*


---

# State Lives in Files

AI sessions are stateless by default. Many systems solve this by giving the AI memory, but then your state lives inside the AI. We would much rather state live in local files and AI have access to it and update it.

## The Session Cycle

`/startup` loads state. `/close` writes new state back. Every session starts further ahead than the last. The compound interest across hundreds of sessions is what separates a knowledge system from a collection of notes.

## The Triad

Three files do most of the load-bearing work:

**Decision Log** holds settled decisions. The framework you chose, the approach you killed and why. Once a question is answered here, it does not get re-analyzed. It also works as a "What did i decide about x?" area.

**Field Notes** holds what you have learned. Observations from sessions, patterns that emerge across them, principles that survive long enough to become defaults.

**Roadmaps** holds work in motion. Active projects, blocked items, dependencies, triggers.

Past (decisions), patterns (learnings), future (plans). The session cycle pulls from all three at startup and writes back at close.

All three are plain markdown files. Switch AI tools tomorrow and they are still there, fully readable. [[04 Navigating Architecture]] covers routing. [[05 The System]] covers the mechanics.

*Deeper: [[Governance Architecture]] maps every governance file to its role and its neighbors.*


---

You now know the three positions this system is built on: the human decides, information has governance, and state lives in files. Everything else is implementation.

**Next:** [[03 Architecture]] shows you the map. [[04 Navigating Architecture]] shows you how these positions play out when you are actually using the system.
