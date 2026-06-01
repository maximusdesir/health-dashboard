# claude-health-skills

Custom Claude Code slash commands for health and performance tracking. Pull real-time biometric data from WHOOP, Withings, and Hevy, cross-reference with your training program, and get direct coaching reports — all inside your terminal.

## Commands

| Command | Description |
|---|---|
| `/health-optimizer` | Daily note: pull today's recovery data, apply autoregulation, output a modified session plan + nutrition targets + recovery stack |
| `/audit` | Weekly coaching report: learning pace, training load, recovery trends, body composition, schedule analysis |
| `/summarize <url>` | Fetch a YouTube video, podcast, or article and save a structured source note to your vault |

## Prerequisites

### Claude Code
Install from [claude.ai/code](https://claude.ai/code) or via npm:
```bash
npm install -g @anthropic-ai/claude-code
```

### MCP Integrations

These commands require MCP servers connected to Claude Code. Add them via `/mcp` in a Claude Code session.

| Integration | Required for | Source |
|---|---|---|
| [WHOOP MCP](https://github.com/lekimluis/whoop-mcp) | `/health-optimizer`, `/audit` | Community |
| [Withings MCP](https://github.com/nicholasgrigoriadis/withings-mcp) | `/health-optimizer`, `/audit` | Community |
| Hevy MCP | `/audit` | Optional |
| Google Calendar MCP | `/audit` | Optional |

### Vault Structure

Commands expect an [Obsidian](https://obsidian.md) vault (or any folder) with this layout:

```
vault/
├── 0 Inbox/
│   ├── Daily/        ← health-optimizer saves here
│   └── Weekly/       ← audit saves here
├── 1 Notes/
├── 2 Sources/
│   ├── Articles/
│   ├── YouTube/
│   └── Podcasts/
└── 6 Training/       ← optional: day files for autoregulation
```

Set `VAULT_PATH` to your vault's absolute path:
```bash
# In ~/.bashrc, ~/.zshrc, or ~/.config/fish/config.fish
export VAULT_PATH="$HOME/your-vault-name"
```

If `VAULT_PATH` is unset, commands default to `~/vault`.

---

## Installation

### Option A — Global (works from any directory)

```bash
git clone https://github.com/YOUR_USERNAME/claude-health-skills
cp claude-health-skills/commands/* ~/.claude/commands/
```

### Option B — Project-local (only active inside your vault)

```bash
git clone https://github.com/YOUR_USERNAME/claude-health-skills
mkdir -p your-vault/.claude/commands
cp claude-health-skills/commands/* your-vault/.claude/commands/
```

Verify installation — open Claude Code and type `/` to see available commands.

---

## Usage

```bash
cd ~/your-vault
claude

# Inside Claude Code:
/health-optimizer          # run daily before training
/audit                     # run weekly (Sunday or Monday AM)
/summarize https://...     # save a source note
```

### Training Day Files (optional, for `/health-optimizer`)

If you have per-day training files in `$VAULT_PATH/6 Training/`, the optimizer reads today's programmed session and applies your recovery score to modify it. Without these files it still generates nutrition targets and recovery stack recommendations.

---

## Customization

Each `.md` file in `commands/` is plain Markdown — edit them directly to match your goals, metrics, and protocols.

Common customizations:
- **Recovery thresholds** — adjust the 67%/34% gates in `health-optimizer.md`
- **Nutrition targets** — fill in your actual macros in the load table
- **Audit goals** — replace the CS50x / bench press targets in `audit.md` with your own
- **Vault paths** — all paths resolve from `$VAULT_PATH`

---

## How It Works

Claude Code custom commands are Markdown files stored in `~/.claude/commands/` (global) or `.claude/commands/` (project-local). When you type `/command-name` in a Claude Code session, the file is injected as a system prompt and Claude executes the instructions.

These commands use the [Model Context Protocol (MCP)](https://modelcontextprotocol.io) to connect Claude to external APIs — WHOOP, Withings, Hevy — at runtime. No data is stored by Claude; it reads live from your accounts each time.

---

## License

MIT
