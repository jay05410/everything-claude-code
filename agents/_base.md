---
name: _base
description: Common rules and patterns for all agents. Referenced by other agents.
---

# Base Agent Guidelines

## Stack Reference

All tech stack info in `config/stack.yaml`. Never hardcode versions.

## MCP Model Integration

Agents use different models via MCP based on task type:

| Agent Type | MCP | Tools |
|------------|-----|-------|
| Planning, Architecture | gemini | `mcp__gemini__generateContent` |
| Frontend/UI | gemini | `mcp__gemini__generateContent` |
| Code Review, Security | openai-o3-mini | `mcp__openai__chat` |
| Deep Analysis | openai-o3 | `mcp__openai__chat` |
| Research | openai-gpt4o | `mcp__openai__chat` |

### Invoking MCP Models

```
For Gemini tasks:
Use mcp__gemini__generateContent with your prompt

For OpenAI tasks:
Use mcp__openai__chat with model parameter (gpt-4o, o3-mini, o3)
```

### Model Selection by Task

| Task | Model | Reason |
|------|-------|--------|
| Planning, decomposition | Gemini | Large context, structured output |
| UI/UX design decisions | Gemini | Visual understanding |
| Code review | o3-mini | Fast logical analysis |
| Security analysis | o3-mini | Pattern detection |
| Architecture decisions | o3 | Deep reasoning |
| Complex debugging | o3 | Multi-step reasoning |
| Web search, docs lookup | GPT-4o | Tool use, browsing |

## Agent Delegation

When delegating to subagents:

```markdown
**TASK**: [Atomic, specific goal]
**EXPECTED OUTCOME**: [Concrete deliverables]  
**TOOLS**: [Tool whitelist]
**MUST DO**: [Requirements]
**MUST NOT DO**: [Forbidden actions]
**CONTEXT**: [File paths, patterns]
```

## Available Agents

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| `explore` | Codebase search | Finding code, understanding structure |
| `librarian` | External docs | Library usage, best practices |
| `oracle` | Deep analysis | Architecture, debugging |
| `frontend-ui-ux-engineer` | UI work | Visual/styling changes |
| `document-writer` | Docs | README, API docs |
| `planner` | Planning | Feature breakdown |
| `architect` | Design | System architecture |

## Output Standards

1. **Language**: English
2. **Format**: Markdown with clear headers
3. **Code**: Include file paths, fenced blocks with language
4. **Brevity**: Concise, no repetition

## Tool Usage

| Task | Tools |
|------|-------|
| Read code | Read, Grep, Glob |
| Edit code | Edit (prefer over Write) |
| Run commands | Bash |
| Search | Grep, ast-grep |
| Type check | lsp_diagnostics |

## Code Quality

- File: 200-400 lines typical, 800 max
- Function: 50 lines max
- Nesting: 4 levels max
- Coverage: 80% minimum

## Immutability (CRITICAL)

```typescript
// WRONG
user.name = 'New'
items.push(newItem)

// CORRECT
const updated = { ...user, name: 'New' }
const newItems = [...items, newItem]
```

## Verification

Before marking complete:
- [ ] lsp_diagnostics clean
- [ ] Tests pass
- [ ] No console.log
- [ ] No hardcoded secrets
- [ ] Follows existing patterns
