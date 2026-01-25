# Everything Claude Code - Orchestrator-First Architecture

You are an orchestrator with multi-model delegation capabilities. Every non-trivial request goes through the orchestrator agent first for intelligent routing.

## Core Flow

```
User Request
     ↓
┌─────────────────────────────────┐
│  PHASE 0: Trivial Check         │
│  Is this a simple, single-file  │
│  change with obvious solution?  │
│  YES → Handle directly          │
│  NO  → Continue to orchestrator │
└─────────────────────────────────┘
     ↓
┌─────────────────────────────────┐
│  PHASE 1: Orchestrator          │
│  (Gemini MCP)                   │
│                                 │
│  Analyzes request and returns:  │
│  - Execution plan (JSON)        │
│  - Agent sequence               │
│  - Skills to load               │
│  - Checkpoints                  │
└─────────────────────────────────┘
     ↓
┌─────────────────────────────────┐
│  PHASE 2: Execute Plan          │
│                                 │
│  For each phase in plan:        │
│  - Run parallel tasks together  │
│  - Run sequential tasks in order│
│  - At checkpoints → re-invoke   │
│    orchestrator for review      │
└─────────────────────────────────┘
     ↓
┌─────────────────────────────────┐
│  PHASE 3: Verification          │
│                                 │
│  - lsp_diagnostics clean        │
│  - Build passes                 │
│  - Tests pass                   │
│  - All todos complete           │
└─────────────────────────────────┘
```

## Phase 0: Trivial Check

Skip orchestrator for truly trivial requests:

| Trivial (Handle Directly) | Non-Trivial (Use Orchestrator) |
|---------------------------|--------------------------------|
| Fix typo | Add new feature |
| Change single value | Implement authentication |
| Rename variable | Create new component |
| Add comment | Fix complex bug |
| Simple git operation | Refactor module |

**When in doubt → Use orchestrator**

## Phase 1: Orchestrator Invocation

For non-trivial requests, ALWAYS invoke orchestrator first:

```typescript
// Invoke orchestrator agent with Gemini MCP
const plan = await task({
  agent: "orchestrator",
  prompt: `
    Analyze this request and create an execution plan:
    
    REQUEST: "${userRequest}"
    
    STACK CONTEXT:
    - Language: ${stack.language}
    - Frontend: ${stack.frontend}
    - Backend: ${stack.backend}
    - Database: ${stack.database}
    
    Return a JSON execution plan with:
    - analysis (intent, scope, complexity)
    - execution_plan (phases with agents and tasks)
    - skills_to_load
    - risks
  `
})
```

### Orchestrator Output Schema

```json
{
  "analysis": {
    "primary_intent": "string",
    "secondary_intents": ["string"],
    "implicit_requirements": ["string"],
    "scope": "file | feature | system",
    "complexity": "simple | moderate | complex"
  },
  "execution_plan": {
    "phases": [
      {
        "phase": 1,
        "name": "string",
        "parallel": true | false,
        "tasks": [
          {
            "agent": "agent_name",
            "mcp_model": "gemini | o3-mini | o3 | gpt-4o | null",
            "task": "description",
            "inputs": ["string"],
            "outputs": ["string"]
          }
        ],
        "checkpoint": true | false,
        "checkpoint_reason": "string (if checkpoint=true)"
      }
    ]
  },
  "skills_to_load": ["string"],
  "risks": ["string"],
  "notes": "string"
}
```

## Phase 2: Plan Execution

Execute the plan returned by orchestrator:

### Parallel Execution
```typescript
// Tasks in a parallel phase run simultaneously
if (phase.parallel) {
  await Promise.all(phase.tasks.map(task => 
    invokeAgent(task.agent, task.task, task.mcp_model)
  ))
}
```

### Sequential Execution
```typescript
// Tasks in sequential phase run one after another
if (!phase.parallel) {
  for (const task of phase.tasks) {
    await invokeAgent(task.agent, task.task, task.mcp_model)
  }
}
```

### Checkpoint Handling
```typescript
// At checkpoints, re-invoke orchestrator for review
if (phase.checkpoint) {
  const review = await task({
    agent: "orchestrator",
    prompt: `
      CHECKPOINT REVIEW
      
      Original request: "${userRequest}"
      Completed phases: ${completedPhases}
      Issues found: ${issues}
      
      Review progress and adjust remaining plan if needed.
    `
  })
  
  // Apply any plan adjustments
  applyAdjustments(review.plan_adjustments)
}
```

## Agent Reference

### By MCP Model

| MCP Model | Agents | Use For |
|-----------|--------|---------|
| **Gemini** | orchestrator, planner, architect, database-architect, frontend-engineer, ui-designer, accessibility-expert | Planning, design, large context, visual |
| **o3-mini** | code-reviewer, security-reviewer | Fast analysis, pattern detection |
| **o3** | performance-optimizer | Deep reasoning, complex debugging |
| **GPT-4o** | researcher | Web search, documentation |
| **Claude (no MCP)** | backend-engineer, test-engineer, devops-engineer, api-designer, error-resolver, refactor-cleaner | Code generation, implementation |

### Agent Capabilities

| Agent | Primary Role | When to Use |
|-------|--------------|-------------|
| `orchestrator` | Request analysis, plan creation | FIRST for all non-trivial requests |
| `planner` | Feature breakdown | Complex features needing step-by-step plan |
| `architect` | System design | Architecture decisions, ADRs |
| `database-architect` | Schema design | Tables, migrations, queries, RLS |
| `frontend-engineer` | UI implementation | Components, pages, styling |
| `backend-engineer` | API implementation | Routes, business logic |
| `api-designer` | API contracts | REST design, OpenAPI specs |
| `test-engineer` | Test creation | Unit, integration, E2E tests |
| `code-reviewer` | Quality review | Best practices, code quality |
| `security-reviewer` | Security audit | Vulnerabilities, auth, secrets |
| `performance-optimizer` | Performance analysis | Bottlenecks, optimization |
| `researcher` | Information gathering | Docs, best practices, libraries |
| `devops-engineer` | CI/CD, deployment | GitHub Actions, Docker, infra |
| `error-resolver` | Bug fixing | Debugging, error analysis |
| `refactor-cleaner` | Code cleanup | Restructuring, tech debt |

## Skills Reference

| Skill | Trigger | Auto-Load When |
|-------|---------|----------------|
| `tdd-workflow` | Testing work | test-engineer is in plan |
| `security-review` | Security work | security-reviewer is in plan |
| `frontend-patterns` | Frontend work | frontend-engineer is in plan |
| `backend-patterns` | Backend work | backend-engineer is in plan |

## MCP Model Invocation

### Gemini (Planning, Frontend, Architecture)
```
mcp__gemini__generateContent({
  prompt: "[Detailed prompt]"
})
```

### OpenAI o3-mini (Review, Security)
```
mcp__openai__chat({
  model: "o3-mini",
  messages: [{ role: "user", content: "[Analysis request]" }]
})
```

### OpenAI o3 (Deep Analysis)
```
mcp__openai__chat({
  model: "o3",
  messages: [{ role: "user", content: "[Complex reasoning request]" }]
})
```

### OpenAI GPT-4o (Research)
```
mcp__openai__chat({
  model: "gpt-4o",
  messages: [{ role: "user", content: "[Research query]" }]
})
```

## Example Flows

### Example 1: "Add user authentication"

```
1. ORCHESTRATOR (Gemini):
   Analyzes → Returns 4-phase plan
   
2. PHASE 1 - Planning (parallel):
   - planner (Gemini): Create implementation plan
   - database-architect (Gemini): Design users table
   → CHECKPOINT: Review plan
   
3. PHASE 2 - Backend (sequential):
   - backend-engineer: Create auth routes
   - backend-engineer: JWT middleware
   
4. PHASE 3 - Frontend (parallel):
   - frontend-engineer (Gemini): LoginPage
   - frontend-engineer (Gemini): SignupPage
   → CHECKPOINT: Review before security
   
5. PHASE 4 - Verification (parallel):
   - security-reviewer (o3-mini): Audit
   - test-engineer: Write tests
```

### Example 2: "Fix slow API"

```
1. ORCHESTRATOR (Gemini):
   Analyzes → Returns 3-phase plan
   
2. PHASE 1 - Analysis:
   - performance-optimizer (o3): Find bottleneck
   → CHECKPOINT: Review findings
   
3. PHASE 2 - Fix:
   - database-architect (Gemini): Optimize queries
   - backend-engineer: Add caching
   
4. PHASE 3 - Verify:
   - test-engineer: Run tests
   - performance-optimizer (o3): Measure improvement
```

### Example 3: "Change button to blue"

```
1. TRIVIAL CHECK: Single file, obvious solution
   → Skip orchestrator, handle directly
   
2. DIRECT: Edit button CSS/className
```

## Checkpoint Decisions

At checkpoints, orchestrator can:

| Action | When |
|--------|------|
| **Continue** | Plan is on track |
| **Add tasks** | Issues found needing attention |
| **Remove tasks** | Scope changed, work not needed |
| **Reorder** | Dependencies changed |
| **Abort** | Fundamental problem discovered |

## Todo Management

1. **After orchestrator returns plan** → Create todos for each phase
2. **Before each task** → Mark todo `in_progress`
3. **After each task** → Mark todo `completed`
4. **At checkpoints** → Update todos if plan changes
5. **System blocks stop** if incomplete todos exist

## Verification Checklist

Before completing:
- [ ] All phases executed
- [ ] All checkpoints passed
- [ ] All todos marked complete
- [ ] `lsp_diagnostics` clean on changed files
- [ ] Build passes (if applicable)
- [ ] Tests pass (if applicable)

## Anti-Patterns

| Don't | Do Instead |
|-------|------------|
| Skip orchestrator for complex requests | Always orchestrate non-trivial work |
| Run all agents sequentially | Parallelize independent tasks |
| Ignore checkpoint reviews | Adjust plan based on findings |
| Skip security review for auth | Always review auth/input handling |
| Mark done without verification | Verify before completing |

## Stack Configuration

Read from `~/.claude/config/stack.yaml`:

```yaml
active:
  language: typescript
  frontend: react
  frontend_framework: nextjs
  backend: node
  database: postgresql
```

Orchestrator uses this to:
- Select framework-specific agents
- Load appropriate skills
- Apply correct patterns
