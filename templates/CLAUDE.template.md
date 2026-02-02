# {PROJECT_NAME} - Claude Code Configuration

**This file is read by Claude Code to understand your project context and preferences.**

---

## Project Information

Generated: {GENERATED_DATE}
Last Updated: {UPDATED_DATE}
Configuration Version: 1.0

### Tech Stack

- **Language**: {LANGUAGE} {VERSION}
- **Frontend**: {FRONTEND} {VERSION}
- **Backend**: {BACKEND} {VERSION}
- **Database**: {DATABASE}
- **Other**: {OTHER_TOOLS}

### Project Characteristics

- **Complexity**: {COMPLEXITY} (simple | medium | complex)
- **Team Size**: {TEAM_SIZE} (solo | small | large)
- **Development Stage**: {STAGE} (early | active | maintenance)

---

## Configuration Settings

### User Preferences

```yaml
user:
  skill_level: "{SKILL_LEVEL}"  # beginner | intermediate | expert
  communication_language: "English"
  document_language: "English"
```

**Skill Level Effects:**
- **Beginner**: Detailed explanations, step-by-step guidance, more examples
- **Intermediate**: Balanced detail, assume basic knowledge
- **Expert**: Concise, technical, skip obvious explanations

### Workflow Mode

```yaml
workflow_mode: "{WORKFLOW_MODE}"  # auto | structured
```

**Modes:**
- **auto** (default): Orchestrator creates execution plans on-demand. Flexible and adaptive.
- **structured**: Use predefined workflows from `workflows/` directory for complex features.

{IF_STRUCTURED_MODE}
**Active Workflows:**
- `feature-development` - Full feature lifecycle (planning → implementation → testing → review)
- `code-review` - Comprehensive quality review workflow

**Active Teams:**
- `feature-team` - Full-stack development team
- `review-team` - Quality assurance team
{ENDIF}

---

## Agent Model Assignments

**All model assignments are managed in `config/stack.yaml` (Single Source of Truth)**

Current optimization strategy for this project:

| Tier | Model | Purpose | Agents |
|------|-------|---------|--------|
| **Orchestration** | Opus 4.5 | Multi-agent coordination | orchestrator, planner |
| **Frontend** | Gemini 3 Pro | Visual understanding, 2M context | frontend-engineer, ui-designer, accessibility-expert |
| **Architecture** | Gemini 3 Pro | Large context analysis | architect, database-architect, domain-architect |
| **Code Generation** | GLM-4.7 | 30% token savings | backend-engineer, api-designer, test-engineer |
| **Analysis** | GPT-5.2 | Advanced verification | code-reviewer, security-reviewer, performance-optimizer, error-resolver, researcher |
| **Documentation** | Gemini 3 Flash | Fast, visual analysis | doc-updater, publisher |
| **Utilities** | Sonnet 4.5 | Built-in, no extra cost | refactor-cleaner, devops-engineer, e2e-runner, tdd-guide, domain-sync |

**Estimated Cost**: {ESTIMATED_COST}/month with this configuration

To modify model assignments, edit `config/stack.yaml` and run `/setup` to reconfigure.

---

## Project-Specific Rules

<!-- Add your custom rules, conventions, and preferences below -->

### Code Standards
{CUSTOM_STANDARDS}

### Architecture Decisions
{CUSTOM_ARCHITECTURE}

### Testing Requirements

```yaml
testing_preference: "{TESTING_PREFERENCE}"  # tdd_required | optional | disabled
```

**Testing Preferences:**
- **tdd_required**: Write tests for all code (TDD methodology strictly followed)
- **optional** (default): Pragmatic testing - only test complex logic, critical paths
- **disabled**: Skip all test generation (you'll write tests manually if needed)

**When `optional` (default):**
- ✅ Tests written for: Complex business logic, algorithms, security-critical code
- ❌ Tests skipped for: Simple CRUD, getters/setters, static UI, boilerplate

**Kent Beck's principle**: "I get paid for code that works, not for tests."
Focus on valuable tests, not coverage percentage.

{CUSTOM_TESTING}

### Deployment Process
{CUSTOM_DEPLOYMENT}

---

## Quick Commands

- `/setup` - Reconfigure project settings
- `/help` - Context-aware help
- `/help what next` - Suggest next steps

---

## Notes

This configuration was optimized for your stack and preferences. You can:
- Modify this file directly for project-specific rules
- Run `/setup` to reconfigure based on new requirements
- Edit `config/stack.yaml` to change model assignments

**Do not** modify `README.md` - that's for general documentation.
**Do** modify this file (`CLAUDE.md`) - it's your project's Claude Code configuration.
