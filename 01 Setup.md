
Getting everything running. Follow these steps in order.


> This guide targets Windows. The vault structure and AI workflow are platform-agnostic, but setup instructions, paths, and shell commands are written and tested for Windows. If you're on Mac or Linux, the concepts apply but you'll need to adapt the specifics.


> [!important] Before you begin
> Every command in this guide uses `C:\All Vault\` as a placeholder. **Replace it with your vault's actual path** before running anything. You'll see it in Step 3 (shortcut) and Step 6 (environment variables).

If you already have an Obsidian vault or CLAUDE.md, see **Supplemental** at the bottom.

## Step 0: Get Your Vault

### If you cloned from GitHub:
`git clone` created a folder (e.g., `pkm-vault/`). Move or rename it wherever you want your vault to live. Something like `C:\Vault\` or `C:\Your Vault\` works. Avoid cloud-synced folders (OneDrive, Dropbox) because the sync engine conflicts with Git.

### If you downloaded the zip:
Extract it to a permanent location. Same guidance: pick a path, avoid cloud sync.

### If you already have an Obsidian vault:
Close your existing vault before opening this one. Do not run two vaults simultaneously in Obsidian.

If you want to merge later, drag the contents of this vault into your existing one. The `.obsidian/plugins/` folder contains pre-configured custom plugins that will add alongside your existing ones. The `.claude/` folder contains hooks and skills. If you already have a `.claude/` folder from Claude Code, see "If You Already Have a CLAUDE.md" in the Supplemental section below.


## Step 1: Open Your Vault

**New to Obsidian:** Open Obsidian, click **Open folder as vault**, select the folder from Step 0.

**Already using Obsidian:** If running as a separate vault, use **Open another vault** in the bottom-left vault switcher.

When the vault opens, Obsidian will ask if you trust the plugins in this vault. Click **Trust author and enable plugins**. This enables the pre-installed community and custom plugins that the system depends on.


## Step 2: Install Claude Code

This is the first thing you install. Once Claude is running, it helps you with the rest of setup.

Go to https://code.claude.com/docs/en/quickstart and follow their instructions.

A few things I learned the hard way:

- **Use PowerShell, not CMD.** Claude Code needs features that Command Prompt doesn't support.
- **Don't use admin PowerShell.** Regular user PowerShell. Admin creates permission problems later.
- **Ask Claude for help during this process.** It can see your vault, has web access, and can troubleshoot install issues.


## Step 3: Create a Launch Shortcut

- [ ] Right-click your desktop, **New > Shortcut**
- [ ] Paste this into "Type the location of the item" replacing the path with your vault's location:

```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -NoExit -Command "cd 'C:\All Vault\00_System\AI\Claude'; claude"
```

This launches Claude Code inside the AI working directory. You can also `cd` there manually and run `claude`.


## Step 4: Install Python and Git

### Python (via Claude)

- [ ] Launch Claude via your shortcut
- [ ] Tell it: **"install Python"**

Claude runs `winget install Python.Python.3`. Close and reopen your PowerShell window after this completes so the new PATH entries take effect.

### Git for Windows (manual)

`winget install Git.Git` triggers a UAC elevation prompt that doesn't always work cleanly when Claude runs it for you. Easier path: install it yourself.

- [ ] Go to [git-scm.com](https://git-scm.com) and download Git for Windows
- [ ] Run the installer with default settings (make sure "Add Git to PATH" stays checked)
- [ ] Close and reopen your PowerShell window
- [ ] Verify with `git --version`

**Why these are needed:**
- **Python** is required for all hook scripts (sandbox enforcement, auto-commit, git sync, content sanitization, and others).
- **Git for Windows** is required by Claude Code itself — it uses Git Bash internally to run commands even when launched from PowerShell. Also handles offline version control: Claude commits automatically as it works and uses git history to prevent overwriting parallel changes.

*Deeper: [[Hook System Architecture]] walks through every hook script.*


## Step 5: Install Plugins

Install these in order. Plugins 1-4 are from the Obsidian community directory. Plugins 5-6 are custom and ship with your vault.

### 1. [Local REST API](obsidian://show-plugin?id=obsidian-local-rest-api)
by Adam Coddington

- [ ] Install and enable
- [ ] Copy the API key it generates, you'll need it in Step 6

This is how Claude Code talks to Obsidian.

### 2. [MCP Tools](obsidian://show-plugin?id=mcp-tools)
by Jack Steam

- [ ] Install and enable
- [ ] Open the plugin's settings and click **"Install Server"** — this downloads a small native binary the plugin needs to talk to Claude Code. Without this step, MCP connection will fail.

Bridges Claude Code to Obsidian's REST API via MCP (Model Context Protocol), giving Claude access to read, search, and write vault files.

### 3. [Smart Connections](obsidian://show-plugin?id=smart-connections)
by Brian Petro

- [ ] Install and enable (free tier is sufficient)

Adds semantic search to your vault. Ask about "pricing strategy" and it surfaces your note titled "How We Set Rates." Claude uses this for every vault search.

### 4. [Tasks](obsidian://show-plugin?id=obsidian-tasks-plugin)
by Martin Schenck and Clare Macrae

- [ ] Install and enable (no configuration needed)

Turns `- [ ]` checkboxes into queryable tasks across your vault. Home files use Tasks query blocks to pull open items from Roadmap files, so checking off a task from your project's home page updates the source file automatically.

### 5. Vault Toolkit
Custom plugin (included with your vault).

- [ ] Enable in Settings > Community Plugins

One plugin, eight features, each independently togglable in settings. Handles folder templates, file archiving/relocation, bidirectional supersession links, code file viewing, dotfile visibility, tab path disambiguation, a home button, and reading view in a new window. All features on by default. See [[05 The System#Vault Toolkit]] for what each one does.

> **If you use Obsidian Sync:** The `.claude/` folder must be excluded from Sync, or synced deletions from other devices can destroy your skills and rules. Vault Toolkit adds this exclusion automatically when it loads. As a safety net, verify it's there: **Settings > Sync > Excluded folders** should list `.claude/`. If it doesn't, add it manually. See [[06 Advanced Use#Multi-Device Sync]] for why this matters.

### 6. Vault Dashboard
Custom plugin (included with your vault)

- [ ] Enable in Settings > Community Plugins

A visual project dashboard with up to three project tabs, each surfacing one active Roadmap. For each project: a "Where I'm At" narrative, work items grouped by status, per-WI quick actions (add notes, mark done), live session badges, and a stale-WI indicator. Click the dashboard icon in the left ribbon to open it.

---

### After enabling all plugins: restart Obsidian

- [ ] Close Obsidian (Ctrl+Q or click the X)
- [ ] Reopen Obsidian

Why this matters: some plugins register routes or hooks at load time and depend on other plugins being already loaded. The order plugins enable in matters less than getting everything reloaded together once. After this restart, all the plugin integrations are wired correctly and you can move on to environment variables.


## Step 6: Set Environment Variables

The vault ships with a pre-configured `.mcp.json` that connects Claude Code to Obsidian. You just need to set two environment variables so it knows your API key and vault location.

Set these via **Settings → System → About → Advanced system settings → Environment Variables → User variables**.

| Variable | Value | Purpose |
|----------|-------|---------|
| `VAULT_PATH` | Full path to your vault (e.g. `C:\All Vault`) | Tells Claude Code where to find Obsidian's MCP server |
| `OBSIDIAN_API_KEY` | The API key from Local REST API (Step 5) | Authenticates Claude Code with Obsidian |

- [ ] Set `VAULT_PATH` to your vault's location
- [ ] Set `OBSIDIAN_API_KEY` to the key you copied earlier during setup of Local REST API
- [ ] **Restart your terminal** (or log out and back in) for the variables to take effect

> [!note] Optional keys
> `BRAVE_API_KEY` (web search) and `XAI_API_KEY` (Twitter search) are covered in [[06 Advanced Use]] when you set up Deep Research. Discord bot keys live in a `.env` file next to the bot script, not in system environment variables.


## Step 7: Configure Obsidian Settings

Open Obsidian Settings (gear icon, bottom-left).

- [ ] **Default location for new notes:** Change to "In the folder specified below", set path to `01_Inbox`
- [ ] **Default location for new attachments:** Change to "In the folder specified below", set path to `00_System/Attachments`
- [ ] **Detect all file extensions:** Enable (lets the Code Viewer feature in Vault Toolkit open .py, .json, and other code files)
- [ ] **Use Wikilinks:** Confirm it's on (default)
- [ ] **Automatically update internal links:** Confirm it's on (default). Required for Vault Toolkit's file relocation to keep links intact


## Step 8: Verify Your Setup

- [ ] Launch Claude Code using your shortcut
- [ ] type "`/startup`"

Claude should test the Obsidian connection, read the Roadmaps, and report ready. If it fails, check your environment variables from Step 6, then fully close and reopen Obsidian and try again.

*Deeper: [[Troubleshooting]] catalogues the known failure modes across MCP, hooks, Smart Connections, and the sync layer — check there if something weird happens mid-session.*

From this point on, Claude is your partner for the rest of setup. You can ask it questions about anything in the docs, and it will search your vault to answer them.


## Step 9: Set Up Local Git

- [ ] Tell Claude: **"set up Git for my vault"**

Claude will initialize a local repository, create the .gitignore, and make the first commit. If you'd rather do it manually:

```bash
git init
git add -A
git commit -m "Initial vault setup"
```

Nothing is uploaded anywhere. Claude commits automatically as it works, so every edit is tracked and reversible. Git is the reason you can let AI modify your files with confidence.


> **Multi-device:** If you want cloud backup or multi-device access, [[06 Advanced Use#Multi-Device Sync]] has the full setup. It takes five minutes: create a private GitHub repo, add it as a remote, set one config value. After that, sync is automatic — startup pulls, session end pushes.


---

The infrastructure is done.

**Next:** [[02 Core Philosophies]], [[03 Architecture]], and [[04 Navigating Architecture]] explain why the system is built this way. They're conceptual, not hands-on. When you reach [[05 The System]], you'll be back to working with Claude.


---

## Receiving Updates

Your GitHub organization membership gives you access to the updates repository. When updates ship, the changelog explains what changed and why. Your Claude can read the changelog, diff against your current files, and propose changes. You review and approve each one.

Updates only touch system files (procedures, hooks, reference docs, skills). Your personal files (Decision Log, Field Notes, Roadmaps, knowledge, projects) are never modified by an update.

If you downloaded the zip instead of cloning from GitHub, you can still check for updates manually or email grae@matzekmedia.com for access to the updates repository.


---

## Supplemental

### If You Already Have an Obsidian Vault

You have two options: run this as a separate vault, or merge it into your existing one. We recommend starting separate and merging once you understand how the system works.

**Starting separate (recommended):**
Open this as its own vault. Use it for a week. Once you understand the folder structure and how Claude interacts with it, you'll know what to bring over and where it belongs.

**Merging into your existing vault:**
The system's files live in specific locations. Here's what you're moving and where it goes:

1. **`00_System/`** — Copy the entire folder into your vault root. This is the AI working directory, templates, and governance infrastructure. Nothing in here conflicts with your existing files.
2. **`01_Inbox/`** — If you already have an inbox folder, keep yours. Just know that Claude creates new notes here by default (configured in Step 7).
3. **`02_Projects/`**, **`03_Areas/`**, **`04_Knowledge/`**, **`05_Reference/`**, **`90_Archive/`** — These are the lifecycle folders. You don't need to reorganize your existing files into them immediately. Create the folders, move files over gradually. Claude uses folder paths to interpret what a file is (see [[03 Architecture]]), so files in the right place get better AI treatment — but nothing breaks if they aren't there yet.
4. **`.claude/`** — Copy the entire folder. This contains hooks, skills, rules, and agents. If you already have a `.claude/` folder, merge carefully — the system's hooks and skills are designed to work together.
5. **`.obsidian/plugins/`** — The custom plugins (Vault Toolkit, Vault Dashboard) install alongside your existing plugins. Community plugins you already have (Tasks, Smart Connections, etc.) keep their settings. If you have different versions, update to the latest from the Obsidian plugin directory.

Your existing notes, themes, hotkeys, and other plugins are unaffected. The system adds to your vault, it doesn't replace what's there.

**If you use Obsidian Sync across multiple devices:**
See [[06 Advanced Use#Obsidian Sync (optional, alongside Git)]] for the full explanation. Short version: Vault Toolkit auto-excludes `.claude/` from Sync. Verify the exclusion is in **Settings > Sync > Excluded folders** on every device.

### If You Already Have a CLAUDE.md

The vault ships with a `CLAUDE.md` designed for the system to work. If you have one from other projects, keep the starter's **Vault Structure**, **Context Management**, **Safety Rules**, and **Key Files** sections. Put your personal instructions in the **My Rules** section at the bottom. If you don't have an existing `CLAUDE.md`, use the starter as-is.
