---
name: _base
description: Common rules and patterns for all agents. This file is referenced by other agents.
---

# Base Agent Guidelines

## Stack Reference

All version and tech stack information is centralized in `config/stack.yaml`.
Never hardcode versions - always reference the config file.

## MCP Model Integration

When an agent is assigned a non-Claude model (see `agent_models` in stack.yaml):

```
gemini    -> Use gemini MCP tools: gemini_generate_text, gemini_analyze_image
openai    -> Use openai MCP tools: ask_openai, openai_chat
o3/o3-mini -> Use o3 MCP tools: o3_reason, o3_analyze
```

## Output Standards

1. **Language**: All agent outputs in English
2. **Format**: Markdown with clear headers
3. **Code**: Include file paths, use fenced code blocks with language tags
4. **Brevity**: Be concise, avoid repetition

## Tool Usage

| Task | Preferred Tools |
|------|-----------------|
| Read code | Read, Grep, Glob |
| Edit code | Edit (prefer over Write for existing files) |
| Run commands | Bash |
| Search codebase | Grep, ast-grep |
| Type checking | lsp_diagnostics |

## Code Quality Rules

Reference `config/stack.yaml` standards section:
- File size: 200-400 lines typical, 800 max
- Function size: 50 lines max
- Nesting depth: 4 levels max
- Test coverage: 80% minimum

## Immutability Pattern (CRITICAL)

```typescript
// WRONG
user.name = 'New'
items.push(newItem)

// CORRECT
const updated = { ...user, name: 'New' }
const newItems = [...items, newItem]
```

## Error Handling

```typescript
try {
  const result = await operation()
  return { success: true, data: result }
} catch (error) {
  console.error('Context:', error)
  return { success: false, error: 'User-friendly message' }
}
```

## Verification Checklist

Before marking task complete:
- [ ] Code compiles (lsp_diagnostics clean)
- [ ] Tests pass (if applicable)
- [ ] No console.log in production code
- [ ] No hardcoded secrets
- [ ] Follows existing patterns in codebase
