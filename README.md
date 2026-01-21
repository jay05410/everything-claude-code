# Everything Claude Code

Multi-model agent orchestration framework for Claude Code.

## Installation

### Option 1: Claude Code Plugin (Recommended)

```bash
# Add marketplace
/plugin marketplace add https://github.com/affaan-m/everything-claude-code

# Install plugin
/plugin install everything-claude-code

# Setup
/ecc-setup
```

### Option 2: NPM Package

```bash
npm install -g everything-claude-code

# Setup globally
ecc setup --global

# Or setup for current project only
ecc setup --local
```

### Option 3: Manual Installation

```bash
git clone https://github.com/affaan-m/everything-claude-code.git
cd everything-claude-code
npm install && npm run build

# Copy to Claude config
cp -r agents/ ~/.claude/agents/
cp -r commands/ ~/.claude/commands/
cp -r skills/ ~/.claude/skills/
cp -r hooks/ ~/.claude/hooks/
cp -r rules/ ~/.claude/rules/
cp -r domain/ ~/.claude/domain/
cp -r config/ ~/.claude/config/
```

## Quick Start

```bash
# Initialize project domain files
/dm-init

# Or with planning document
/dm-init docs/PRD.md
```

## What It Does

**Multi-Model Agent Team:**
| Agent | Model | Purpose |
|-------|-------|---------|
| planner, architect | Gemini | Planning, system design |
| frontend-engineer | Gemini | UI/UX |
| backend-engineer | Claude | Code generation |
| code-reviewer, security-reviewer | o3-mini | Analysis |
| researcher | GPT-4o | Web search |

**Skills:**
- `orchestrate` - Multi-agent coordination
- `tdd-workflow` - Test-driven development
- `security-review` - Vulnerability detection
- `frontend-patterns` - React/Vue/Svelte/Angular
- `backend-patterns` - Node/Python/Go/Java/Rust

**Commands:**
| Command | Purpose |
|---------|---------|
| `/dm-init` | Initialize project domain files |
| `/dm-sync` | Sync domain files with code |
| `/plan` | Create implementation plan |
| `/tdd` | Test-driven development |
| `/code-review` | Code quality review |

## Configuration

### Stack Settings

Edit `~/.claude/config/stack.yaml`:

```yaml
active:
  language: "typescript"
  frontend: "react"
  frontend_framework: "nextjs"
  backend: "node"
  database: "postgresql"
```

### Project Config

Create `.claude/project-config.yaml`:

```yaml
project:
  name: "My Project"

stack:
  language: typescript
  frontend: react
  database: postgresql

structure:
  src/app: "Next.js pages"
  src/components: "React components"
  src/lib: "Utilities"
```

### MCP Servers

Add to `~/.claude.json`:

```json
{
  "mcpServers": {
    "openai": {
      "command": "npx",
      "args": ["-y", "@mzxrai/mcp-openai@latest"],
      "env": { "OPENAI_API_KEY": "sk-..." }
    }
  }
}
```

## Structure

```
├── agents/         # 20+ specialized AI agents
├── commands/       # Slash commands
├── skills/         # Reusable skill modules
├── hooks/          # Automation triggers
├── rules/          # Coding standards
├── config/         # Stack configuration
└── domain/         # Project domain templates
```

## Supported Stacks

**Languages:** TypeScript, Python, Go, Java, Kotlin, Rust, C++, C

**Frontend:** React, Vue, Svelte, Angular

**Backend:** Node.js, Python, Go, Java, Kotlin, Rust

**Database:** PostgreSQL, MySQL, MongoDB, SQLite

## License

MIT
