# Everything Claude Code

Multi-model agent orchestration for Claude Code.

## Installation

```bash
# Plugin (recommended)
/plugin install everything-claude-code

# Or NPM
npm install -g everything-claude-code
ecc setup --global
```

## MCP Setup (Required)

Add to `~/.claude.json`:

```json
{
  "mcpServers": {
    "gemini": {
      "command": "npx",
      "args": ["-y", "@aliargun/mcp-server-gemini"],
      "env": { "GEMINI_API_KEY": "your-key" }
    },
    "openai": {
      "command": "npx",
      "args": ["-y", "@mzxrai/mcp-openai"],
      "env": { "OPENAI_API_KEY": "your-key" }
    }
  }
}
```

Get keys: [Gemini](https://aistudio.google.com/apikey) | [OpenAI](https://platform.openai.com/api-keys)

## How It Works

```
Request → Orchestrator (Gemini) → Execution Plan → Agents → Checkpoints → Done
```

Non-trivial requests go through the orchestrator first, which analyzes intent and creates an execution plan with the right agents.

## Agent → Model

| Agent | Model | Purpose |
|-------|-------|---------|
| orchestrator | Gemini | Request analysis, planning |
| planner | Gemini | Feature breakdown |
| frontend-engineer | Gemini | UI components |
| backend-engineer | Claude | API routes |
| code-reviewer | o3-mini | Code quality |
| security-reviewer | o3-mini | Vulnerabilities |
| performance-optimizer | o3 | Bottlenecks |
| researcher | GPT-4o | Documentation |

## Stack Config

Edit `~/.claude/config/stack.yaml`:

```yaml
active:
  language: typescript
  frontend: react
  backend: node
  database: postgresql
```

## Structure

```
agents/        # AI agents (orchestrator, planner, frontend-engineer, etc.)
skills/        # Reusable workflows (tdd, security-review, etc.)
rules/         # Coding standards
config/        # Stack and MCP configuration
```

## License

MIT
