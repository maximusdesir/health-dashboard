# claude-health-skills

Custom Claude Code slash commands for health and performance tracking. Pull real-time biometric data from WHOOP, Withings, and Hevy, cross-reference with your training program, and get direct coaching reports — all inside your terminal.

> **Disclaimer:** These commands are self-tracking and decision-support tools only. They are not
> medical advice, clinical diagnostics, or a substitute for professional guidance. Consumer wearables
> (WHOOP, Withings) carry measurement error — treat all outputs as relative signals, not clinical
> readings. Recovery thresholds in these commands are WHOOP vendor-defined product bands, not
> independently validated clinical benchmarks. Supplement names in the templates are illustrative —
> consult a qualified clinician or registered dietitian before adopting any supplement protocol.
> The authors make no warranty of fitness for any purpose. Use at your own risk.

---

## Commands

| Command | Description |
|---|---|
| `/health-optimizer` | Daily note: pull today's recovery data, apply autoregulation, output a modified session plan + nutrition targets + recovery stack |
| `/audit` | Weekly coaching report: learning pace, training load, recovery trends, body composition, schedule analysis |

---

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
| [WHOOP MCP](https://github.com/briangaoo/whoop-mcp) | `/health-optimizer`, `/audit` | Community |
| [Withings MCP](https://withings-mcp.com/) | `/health-optimizer`, `/audit` | Community |
| [Hevy MCP](https://hevy.tomt.it/) | `/audit` | Optional |
| Google Calendar MCP | `/audit` | Optional |

Commands degrade gracefully when optional integrations are absent — they note which data is unavailable and continue with what's connected.

### Vault Structure

Commands expect an [Obsidian](https://obsidian.md) vault (or any folder) with this layout:

```
vault/
├── Inbox/
│   ├── Daily/        ← health-optimizer saves here
│   └── Weekly/       ← audit saves here
├── 1 Notes/
├── 2 Sources/
│   ├── Articles/
│   ├── YouTube/
│   └── Podcasts/
└── 6 Training/       ← optional: day files for autoregulation
    └── Monday.md, Tuesday.md, ... (any naming convention)
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
git clone https://github.com/maximusdesir/health-dashboard
cp health-dashboard/commands/* ~/.claude/commands/
```

### Option B — Project-local (only active inside your vault)

```bash
git clone https://github.com/maximusdesir/health-dashboard
mkdir -p your-vault/.claude/commands
cp health-dashboard/commands/* your-vault/.claude/commands/
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
```

### Training Day Files (optional, for `/health-optimizer`)

If you have per-day training files in `$VAULT_PATH/6 Training/`, the optimizer reads today's programmed session and applies your recovery score to modify it. The command looks for any file containing today's day name (e.g., `Monday`, `Tuesday`). Without these files it still generates nutrition targets and a recovery stack.

---

## Configure Your Targets

Before running `/audit`, open `commands/audit.md` and replace every `[YOUR_*]` placeholder in the **Goal Countdown** section with your actual goals:

```markdown
# Example — replace with your own:
| Bench Press | [current] | 225 lb | [gap] | ✅ / ⚠️ / ❌ |
| VO2 Max     | [current] | 52     | [gap] | ✅ / ⚠️ / ❌ |
```

For `/health-optimizer`, update the **Recovery Stack** template row with your actual supplement stack. The names in the template are illustrative — do not copy them without personalizing.

Common customizations:
- **Recovery thresholds** — adjust the 67%/34% gates and −20%/−40% heuristics in `health-optimizer.md`
- **Nutrition targets** — fill in your actual macros in the load table

- **Vault paths** — all paths resolve from `$VAULT_PATH`
- **Training day files** — name your files so the optimizer can find them by day of week

---

## How It Works

Claude Code custom commands are Markdown files stored in `~/.claude/commands/` (global) or `.claude/commands/` (project-local). When you type `/command-name` in a Claude Code session, the file is injected as a system prompt and Claude executes the instructions.

These commands use the [Model Context Protocol (MCP)](https://modelcontextprotocol.io) to connect Claude to external APIs — WHOOP, Withings, Hevy — at runtime. No data is stored by Claude; it reads live from your accounts each time.

### Evidence Standards

Each command file contains an **Evidence & Confidence** note at the top that grades the scientific support for each recommendation:
- **Strong** — consistent meta-analytic / multi-RCT support
- **Moderate** — some RCT support, mixed or limited
- **Weak** — mechanistic/small-n only; labeled `# heuristic` inline
- **Insufficient** — no good support; removed or flagged

Claims labeled `# heuristic` are practical starting points — adjust them based on your own response.

---

## License

[MIT](LICENSE)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Evidence-backed corrections and improvements are welcome.
