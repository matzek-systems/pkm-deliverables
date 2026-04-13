Optional extensions that build on the core system. Everything in the previous docs works without these.

---

## Deep Research Setup

These tools matter when you need current web information, community sentiment, or triangulated research across multiple source types.

>If you don't want to set this up, you can run deep research in the claude.ai web app, download the results as markdown, and drag them into your vault.

| Tier | Sources | Cost |
|------|---------|------|
| Quick | Vault only | $0 (works now) |
| Standard | Vault + web + YouTube + Reddit | $0 (Brave Search free tier, 1,000 queries/month) |
| Deep | Standard + Twitter/X | ~$0.50 per session (xAI Grok API) |

### Brave Search (Standard Tier)

1. Create an account at [brave.com/search/api](https://brave.com/search/api/) and generate a Web Search API key. Free tier: 1,000 queries/month, credit card required for signup.
2. Set environment variable `BRAVE_API_KEY` (same place you set `VAULT_PATH` and `OBSIDIAN_API_KEY` in [[01 Setup]]).
3. Restart your terminal.

The vault's `.mcp.json` already includes the Brave Search server. Once the key is set, it works.

Reddit access works through Brave Search (no separate setup). YouTube transcripts use yt-dlp: `pip install yt-dlp`.

### Twitter/X Search (Deep Tier)

Optional. Standard covers most research needs.

1. Create an account at [console.x.ai](https://console.x.ai/) and generate an API key.
2. Set environment variable `XAI_API_KEY`.

No MCP server needed. Claude calls the API directly during research.

> **Note:** The deep-researcher agent (`.claude/agents/deep-researcher.md`) includes xAI/Twitter search by default. If you haven't set `XAI_API_KEY`, the Twitter step will error. Options: set the key, or delete the xAI/Twitter section from the agent file.

*Deeper: [[Deep Research Infrastructure]]*


---

## Unattended Task Execution (Ralph Loops)

Hand Claude a structured task and walk away. Two files: **PROMPT.md** (instructions, constraints, quality rules) and **plan.md** (task list with dependencies and acceptance criteria). The loop reads the plan, executes one task per iteration, and maintains its own **activity.md** log so each pass picks up where the last left off.

```powershell
cd "C:\Your Vault\00_System\AI\Claude"
Get-Content -Raw PROMPT.md | claude -p --dangerously-skip-permissions
```

The `--dangerously-skip-permissions` flag lets Claude execute without asking approval on each action. The name is intentionally scary. It's safe here because: (1) Git tracks every change, (2) the sandbox hook still blocks writes to knowledge and reference, and (3) the PROMPT.md constrains scope.

*Deeper: [[Ralph Loop]]*

---

## Discord Bot - BETA

Discord bot for vault access from your phone, text and voice. Send a message, get an answer from Claude with full vault context. Join a voice channel and talk to it. Capture ideas to your inbox without a laptop.

*Source: [discord-vault-bot](https://github.com/matzek-systems/discord-vault-bot)*

*Deeper: [[Discord Vault Bot]]*


---

## Multi-Device Sync

Work on your desktop, close the session, open the laptop, pick up where you left off.

### What happens automatically

**Startup** pulls from your remote and reconciles divergence. If both machines committed since the last sync, it merges, preserving both histories. Conflicts show as visible markers in Obsidian.

**Session end** commits and pushes to the remote. Runs on both normal exits (`/close`) and abnormal exits (closing the terminal).

### Setup

1. Create a **private** GitHub repository (free tier works).
2. On your **main machine**:
```bash
python 00_System/AI/Claude/tools/machine-bootstrap.py --repo your-username/your-vault
```
The script initializes git, adds the remote, pushes current state, creates missing local state files, validates hooks.

3. On your **second machine**, clone and run the same command:
```bash
git clone https://github.com/your-username/your-vault.git "/path/to/your/vault"
cd "/path/to/your/vault"
python 00_System/AI/Claude/tools/machine-bootstrap.py --repo your-username/your-vault
```

That's it. Startup pulls, close pushes.

### Obsidian Sync (optional, alongside Git)

Obsidian Sync handles real-time file delivery between devices, including mobile. Works alongside Git: Sync moves file contents, Git moves commit history.

**The `.claude/` folder must be excluded from Sync.** It contains `.md` and `.py` files that must stay together. On devices without the full system (phone, tablet), missing `.py` files look like deletions, and Sync propagates those back.

**Automatic:** Vault Toolkit adds `.claude/` to Sync's excluded folders on load.

**Manual fallback:** Settings > Sync > Excluded folders > add `.claude/`.

If you lose `.claude/` files and Git is initialized:
```bash
git checkout HEAD -- 00_System/AI/Claude/.claude/
```

*Deeper: [[Multi-Machine Setup]], [[Recovery Tags]]*


### Using Your Vault from Other Projects

Add the Obsidian MCP server to `.mcp.json` in any Claude Code project, and Claude can search and read your vault while working on unrelated code. Your vault becomes a persistent knowledge layer across all your Claude Code sessions.


And you're done. If you have any problems email me at grae@matzekmedia.com.