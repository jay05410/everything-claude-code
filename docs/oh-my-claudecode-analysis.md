# oh-my-claudecode Repository Analysis

**Repository:** https://github.com/Yeachan-Heo/oh-my-claudecode
**Version:** 3.9.4 (Feb 2026)
**Stars:** 4k | **Forks:** 314 | **License:** MIT
**Tech Stack:** TypeScript 69.1%, Node.js ≥20.0.0

---

## 1. Overall Architecture and Structure

### Core Architecture Pattern

oh-my-claudecode implements a **skill-based routing orchestrator** rather than traditional agent swapping:

```
User Request
     ↓
CLAUDE.md Auto-Routing Layer
     ↓
Layered Skill Composition
     ├─ Execution Layer (default builder, orchestrator, planner)
     ├─ Enhancement Layer (0-N capability injectors: parallel, git, UI/UX)
     └─ Guarantee Layer (optional: ralph skill for persistence)
     ↓
Intelligent Model Tier Selection
     ↓
32-Agent Pool (Haiku → Sonnet → Opus)
     ↓
Evidence-Based Verification
```

### Directory Structure

```
oh-my-claudecode/
├── agents/                   # 35 agent definitions (tiered)
│   ├── architect.md, architect-low.md, architect-medium.md
│   ├── executor.md, executor-low.md, executor-high.md, deep-executor.md
│   ├── designer.md, designer-low.md, designer-high.md
│   ├── code-reviewer.md, code-reviewer-low.md
│   ├── security-reviewer.md, security-reviewer-low.md
│   ├── qa-tester.md, qa-tester-high.md
│   ├── researcher.md, researcher-low.md
│   ├── scientist.md, scientist-low.md, scientist-high.md
│   ├── planner.md, analyst.md, critic.md, writer.md, vision.md
│   ├── build-fixer.md, build-fixer-low.md
│   ├── git-master.md
│   ├── explore.md, explore-medium.md, explore-high.md
│   ├── tdd-guide.md, tdd-guide-low.md
│   └── templates/            # Base templates for consistency
│       ├── base-agent.md
│       ├── tier-instructions.md
│       └── README.md
├── skills/                   # 37+ reusable skills
│   ├── autopilot/, ultrawork/, ultrapilot/, ralph/
│   ├── swarm/, pipeline/, ecomode/
│   ├── deepsearch/, research/, analyze/
│   ├── plan/, orchestrate/, ralplan/
│   ├── deepinit/, omc-setup/, ralph-init/
│   ├── tdd/, build-fix/, code-review/, security-review/
│   ├── frontend-ui-ux/, writer-memory/, hud/
│   ├── git-master/, project-session-manager/
│   └── help/, note/, skill/, cancel/, doctor/, release/
├── src/                      # Core orchestration engine (TypeScript)
├── commands/                 # CLI interface layer
├── bridge/                   # Integration with Claude Code
├── hooks/                    # 31 lifecycle hooks
│   └── hooks.json
├── docs/                     # Comprehensive documentation
│   ├── AGENTS.md, ARCHITECTURE.md, FEATURES.md
│   ├── TIERED_AGENTS_V2.md, DELEGATION-ENFORCER.md
│   ├── ANALYTICS-SYSTEM.md, SYNC-SYSTEM.md
│   ├── REFERENCE.md, MIGRATION.md, COMPATIBILITY.md
│   └── design/, partials/, shared/
├── examples/                 # Usage demonstrations
├── templates/                # Project scaffolding
├── .omc/                     # Local project state
│   ├── state/                # Workflow state
│   ├── plans/                # Generated work plans
│   └── notes/                # Notepad wisdom system
└── package.json
```

---

## 2. Key Features and Capabilities

### 2.1 Multi-Agent Orchestration (32 Specialized Agents)

**Tiered Agent System** - Three capability levels mapped to Claude models:

| Tier | Model | Use Case | Cost Efficiency |
|------|-------|----------|-----------------|
| **LOW** | Haiku | Simple, single-focus tasks | Lowest cost |
| **MEDIUM** | Sonnet | Moderate multi-step work | Balanced |
| **HIGH** | Opus | Complex system-wide analysis | Premium reasoning |

**Weighted Average Cost Reduction:** ~47% through intelligent routing

**Agent Categories:**
- **Architecture:** architect (low/medium/high)
- **Execution:** executor (low/high), deep-executor
- **Research:** researcher (low), explore (medium/high)
- **Design:** designer (low/high), vision
- **Quality:** code-reviewer (low), qa-tester (high), security-reviewer (low)
- **Science:** scientist (low/high)
- **Planning:** planner, analyst, critic, writer
- **Utilities:** build-fixer (low), git-master, tdd-guide (low)

### 2.2 Execution Modes (6 Primary Strategies)

| Mode | Performance | Use Case | Key Feature |
|------|-------------|----------|-------------|
| **Autopilot** | Autonomous | End-to-end development | 5-phase workflow (expand → plan → execute → QA → validate) |
| **Ultrapilot** | 3-5x faster | Multi-component systems | Parallel execution with 5 concurrent workers |
| **Ultrawork** | Parallel | Maximum throughput | Coordinated parallel agent orchestration |
| **Ralph** | Persistent | Critical tasks | Continues until verified complete (architect approval) |
| **Swarm** | Coordinated | Independent parallel tasks | Atomic task claiming |
| **Ecomode** | 30-50% savings | Budget-conscious | Token-efficient parallel execution |
| **Pipeline** | Sequential | Multi-stage processing | Chained agent data flow |

### 2.3 Magic Keywords (Zero Learning Curve)

**Natural Language Activation:**
```
"autopilot: build a REST API"
"ralph ulw: migrate database"
"plan this feature"
"tdd: implement auth"
```

**Inline Keywords:**
- `ultrawork`, `ulw` → Parallel orchestration
- `autopilot`, `build me` → Full autonomous
- `analyze`, `investigate` → Deep analysis
- `ralph`, `don't stop` → Persistence
- `tdd`, `test first` → Test-driven
- `plan this`, `plan the` → Planning interview

### 2.4 Delegation-Enforcer System

**Pre-Tool-Use Hook Mechanism:**
1. Intercepts `Task`/`Agent` tool calls
2. Retrieves target agent's default model from config
3. Auto-injects model parameter if not specified
4. Preserves explicit overrides for tactical adjustments

**Handoff Protocol:**
```yaml
subagent_type: executor  # → sonnet
subagent_type: architect # → opus
subagent_type: code-reviewer-low # → haiku
```

### 2.5 Analytics & Cost Monitoring

**Automatic Token Tracking:**
- HUD renders token usage every cycle (<100ms total)
- TokenExtractor calculates deltas from previous state
- Correlates usage with active agents

**Budget Oversight:**
- Warning: $2 threshold
- Critical: $5 threshold
- Model-specific multipliers for estimation:
  - Haiku: 30% of input tokens
  - Sonnet: 40% of input tokens
  - Opus: 50% of input tokens

**Historical Analysis:**
- Offline transcript processing from `~/.claude/projects/`
- Auto-runs every 24 hours
- Deduplication via `hash(sessionId + timestamp + model)`

### 2.6 Notepad Wisdom System

**Plan-Scoped Knowledge Capture:**
```
.omc/notes/
├── learnings.md     # Patterns, conventions, successful approaches
├── decisions.md     # Architecture decisions with timestamps
├── issues.md        # Blockers and workarounds
└── problems.md      # Open questions
```

### 2.7 State Synchronization

**Single-Source-of-Truth Architecture:**
- `package.json` → canonical source
- Propagates to: README.md, docs/REFERENCE.md, .github/CLAUDE.md, ARCHITECTURE.md, CHANGELOG.md

**Dynamic Metadata:**
- Agent count: scan `.yaml`/`.yml` in `agents/`
- Skill count: scan `.md` in `skills/`
- Ensures "documentation always reflects current state"

**Atomic Operations:**
1. Read entire file to memory
2. Apply all replacements to string
3. Compare original vs modified
4. Write only if changed

**Integration Points:**
- Pre-commit hooks verify sync
- CI/CD catches stale docs in PRs
- `npm version` auto-triggers sync

---

## 3. How It Differs from Standard Claude Code

### 3.1 Standard Claude Code

```
User Request
     ↓
Single Claude Instance
     ↓
Sequential Tool Use
     ↓
Manual Verification
```

### 3.2 oh-my-claudecode Approach

```
User Request
     ↓
Auto-Routing Layer (CLAUDE.md)
     ↓
Skill Composition (3 layers)
     ↓
Parallel Multi-Agent Execution
     ↓
Automatic Tier Selection
     ↓
Evidence-Based Verification
```

### 3.3 Key Differentiators

| Aspect | Standard | oh-my-claudecode |
|--------|----------|------------------|
| **Agent Selection** | Manual | Automatic routing |
| **Execution** | Sequential | Parallel (3-5x faster) |
| **Model Selection** | User chooses | Intelligent tier routing (47% cost savings) |
| **Persistence** | None | Ralph mode (retry until verified) |
| **Cost Control** | Manual | Automatic (Ecomode, tier routing) |
| **Verification** | Manual | Architect approval required |
| **State Management** | None | .omc/ directory with notepad system |
| **Analytics** | None | Automatic token tracking + HUD |
| **Learning Curve** | Command-based | Natural language keywords |

---

## 4. Best Practices and Patterns

### 4.1 Agent Definition Structure

**YAML Frontmatter + XML Sections:**

```yaml
name: architect
description: Code analysis and debugging expert
model: opus
disallowedTools: Write, Edit
```

**Structured Components:**

1. **`<role>`** - Identity statement (Oracle archetype)
2. **`<role_boundaries>`** - What agent IS and IS NOT
3. **`<critical_constraints>`** - Hard blocks (forbidden tools)
4. **`<operational_phases>`** - Execution methodology
5. **`<anti_patterns>`** - Prohibitions
6. **`<response_requirements>`** - Quality gates
7. **`<specialized_protocols>`** - Domain-specific workflows
8. **`<tool_strategy>`** - Semantic tool mappings

**Example from Architect:**

```xml
<role>
You are the Oracle—a code analyzer and debugger.
Operational mode: ANALYZE, ADVISE, RECOMMEND (NOT IMPLEMENT)
Output: Analysis only, no code changes
</role>

<role_boundaries>
<!-- What you ARE -->
- Code analyzer
- Architecture debugger
- Root cause investigator

<!-- What you are NOT -->
- Requirements gatherer → Metis (planner)
- Implementation executor → Prometheus
- QA verifier → QA-Tester

<!-- Handoff Decision Table -->
| Scenario | Delegate To | Why |
|----------|-------------|-----|
| User asks "why is X slow?" | self | Analysis question |
| User says "fix the bug" | Executor | Implementation work |
| Need test verification | QA-Tester | Quality assurance |
</role_boundaries>

<critical_constraints>
BLOCKED ACTIONS:
- Write, Edit tools (no file modifications)
- Making code changes
- Direct implementation

ALLOWED:
- Read, Grep, Glob, LSP tools
- Analysis and recommendations
</critical_constraints>
```

### 4.2 Evidence-First Mindset

**Verification Protocol (from Architect):**

```xml
<verification_before_completion>
CRITICAL: "No claims without fresh evidence"

PROCESS:
1. IDENTIFY claim to verify
2. EXECUTE appropriate verification tool
3. CITE specific file:line references
4. ONLY THEN make assertion

BAD:  "The auth middleware should handle this"
GOOD: "auth.ts:42-55 implements JWT verification,
       but line 48 doesn't validate exp claim"
</verification_before_completion>
```

### 4.3 Systematic Debugging Protocol (Circuit Breaker)

```xml
<systematic_debugging>
Phase 1: Hypothesis formation
Phase 2: Evidence collection
Phase 3: Root cause isolation
Phase 4: Fix recommendation

CIRCUIT BREAKER:
After 3 failed fix attempts → STOP
Re-examine architecture with Architect agent
Prevents infinite debugging loops
</systematic_debugging>
```

### 4.4 Template-Based Agent Inheritance

**Base Template Structure:**

```markdown
# {{AGENT_NAME}}

<role>
{{ROLE_DESCRIPTION}}
</role>

<critical_constraints>
{{TIER_INSTRUCTIONS}}
</critical_constraints>

<{{AGENT_SPECIFIC_PROTOCOL}}>
{{CUSTOM_INSTRUCTIONS}}
</{{AGENT_SPECIFIC_PROTOCOL}}>

<!-- Shared verification protocols inherited from base-agent.md -->
```

**Tier Instructions (tier-instructions.md):**

```markdown
## LOW (Haiku)
- Simple, single-focus tasks
- Concise responses
- Escalate if complexity exceeds boundary

## MEDIUM (Sonnet)
- Moderate multi-step work
- Thorough structure
- Cross-file coordination

## HIGH (Opus)
- Complex system-wide analysis
- Nuanced trade-offs
- Architectural reasoning
```

**Escalation Triggers:**
- Task exceeds complexity boundary
- Multiple failed attempts (>2)
- Cross-system dependencies can't be traced
- Security-sensitive changes
- Irreversible operations

### 4.5 Planner Agent Best Practices (Prometheus)

**Phase-Based Interview Workflow:**

```
Phase 1: Interview Mode (default)
├─ Adaptive questioning
├─ Distinguish codebase facts vs user preferences
└─ Use explore agent for codebase questions

Phase 2: Pre-Generation
└─ Consult Metis (analyst) to identify requirement gaps

Phase 3: Plan Generation
└─ Save structured plans to .omc/plans/

Phase 3.5: Confirmation (MANDATORY)
└─ Display summary, await explicit user approval

Phase 4: Handoff
└─ Direct to /oh-my-claudecode:start-work
```

**Smart Question Protocol:**

```xml
<smart_questions>
CODEBASE FACTS (use explore agent, NOT user):
- "What authentication system do we use?"
- "Where is the user model defined?"
- "What's our current test coverage?"

USER PREFERENCES (ask via AskUserQuestion):
- "What's your deadline?"
- "How important is performance vs simplicity?"
- "What's your risk tolerance?"

RULE: Ask ONE question at a time
</smart_questions>
```

### 4.6 Autopilot 5-Phase Workflow

```
1. EXPANSION
   └─ Requirements + technical specification

2. PLANNING
   ├─ Execution plan
   └─ Critic validation

3. EXECUTION
   ├─ Ralph (persistence)
   └─ Ultrawork (parallel implementation)

4. QA
   ├─ Build validation
   ├─ Lint checks
   ├─ Test execution
   └─ Fix cycles (up to 10 iterations)

5. VALIDATION
   ├─ Functional review
   ├─ Security audit
   └─ Quality verification
```

**State Persistence:**
- Phase transitions saved
- Cancellation/resume support
- Iteration limits configurable

### 4.7 Tool Access by Tier

| Tool | LOW (Haiku) | MEDIUM (Sonnet) | HIGH (Opus) |
|------|-------------|-----------------|-------------|
| Read, Grep, Glob | ✅ | ✅ | ✅ |
| WebSearch, WebFetch | ❌ | ✅ | ✅ |
| Write, Edit (simple) | ✅ | ✅ | ✅ |
| Write, Edit (complex) | ❌ | ✅ | ✅ |
| LSP Tools | ✅ | ✅ | ✅ |
| AST Tools | ❌ | ✅ | ✅ |

### 4.8 Delegation Categories (Semantic Routing)

| Category | Model Tier | Thinking Budget | Use Case |
|----------|------------|-----------------|----------|
| `ultrabrain` | Opus | High | Complex reasoning |
| `visual-engineering` | Opus | Medium | UI/UX design |
| `artistry` | Opus | Medium | Creative work |
| `standard` | Sonnet | Low | Typical implementation |
| `simple` | Haiku | Minimal | Quick lookups |

---

## 5. Configuration and Customization Approaches

### 5.1 Installation

**Claude Code Plugin (Required):**
```
/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode
```

**Setup:**
```
/oh-my-claudecode:omc-setup
```

Choose:
- **Project-scoped:** `./.claude/CLAUDE.md` (recommended)
- **Global:** `~/.claude/CLAUDE.md`

### 5.2 Configuration Hierarchy

```
Global Config (~/.claude/CLAUDE.md)
     ↓
Project Config (./.claude/CLAUDE.md) ← OVERRIDES
     ↓
Runtime CLAUDE.md (auto-generated by skills)
```

**Re-run setup after plugin updates** to apply latest config.

### 5.3 State Directory Structure

**Local Project:**
```
.omc/
├── state/              # Workflow state
├── plans/              # Generated work plans
├── notes/              # Notepad wisdom
│   ├── learnings.md
│   ├── decisions.md
│   ├── issues.md
│   └── problems.md
└── analytics/          # Token usage data
```

**Global:**
```
~/.omc/
├── config/             # Global preferences
├── cache/              # Performance cache
└── analytics/          # Cross-project usage
```

### 5.4 HUD Configuration

**Statusline Display:**
```
/oh-my-claudecode:hud
```

**Metrics Displayed:**
- Active agent + tier
- Current skill
- Token usage (input/output)
- Estimated cost
- Budget alerts

**Performance Targets:**
- HUD render: <100ms total
- Token extraction: <5ms per cycle
- Summary loading: <10ms (mtime-cached)
- CLI startup: <500ms

### 5.5 Customization Points

**Agent Customization:**
1. Fork agent definitions in `agents/`
2. Modify tier instructions
3. Add custom protocols
4. Adjust tool permissions

**Skill Composition:**
1. Create new skills in `skills/`
2. Define activation keywords
3. Specify agent requirements
4. Add to CLAUDE.md auto-routing

**Hook Extensions:**
1. Add custom hooks to `hooks/hooks.json`
2. Trigger on lifecycle events:
   - Prompt submission
   - Tool execution
   - Session completion
   - Error recovery

---

## 6. Innovative Ideas to Adopt

### 6.1 Tiered Agent System with Cost Optimization

**Problem Solved:** Opus is expensive, Haiku is fast but limited

**Solution:** Intelligent tier routing with 47% cost reduction

**Implementation for everything-claude-code:**

```yaml
# agents/_base.agent.yaml
agent:
  config:
    tiers:
      low:
        model: "haiku"
        use_for: "Simple single-file tasks, quick lookups, fast iterations"
        max_complexity: "single-focus"
        escalate_if:
          - "Multi-file coordination needed"
          - "Architecture decisions required"
          - "Cross-system dependencies"

      medium:
        model: "sonnet"
        use_for: "Standard implementation, moderate reasoning"
        max_complexity: "multi-step workflows"
        escalate_if:
          - "Complex architectural reasoning"
          - "System-wide impact analysis"
          - "Novel algorithm design"

      high:
        model: "opus"
        use_for: "Complex architecture, deep reasoning, critical decisions"
        max_complexity: "unlimited"

# agents/architect-low.agent.yaml
agent:
  metadata:
    id: "architect-low"
    name: "Quick Architect"
    tier: "low"
    inherits: "architect"

  config:
    model: "haiku"
    escalation_agent: "architect"  # Routes to medium/high when needed

# agents/architect.agent.yaml (medium - default)
agent:
  metadata:
    id: "architect"
    tier: "medium"
  config:
    model: "sonnet"
    escalation_agent: "architect-high"

# agents/architect-high.agent.yaml
agent:
  metadata:
    id: "architect-high"
    tier: "high"
  config:
    model: "opus"
```

**Orchestrator Integration:**

```json
{
  "execution_plan": {
    "phases": [
      {
        "phase": 1,
        "tasks": [
          {
            "agent": "architect-low",
            "task": "Quick file-level analysis",
            "auto_escalate": true,
            "escalation_triggers": [
              "Cross-system dependencies detected",
              "Multiple failed attempts (>2)"
            ]
          }
        ]
      }
    ]
  }
}
```

### 6.2 Evidence-First Verification Protocol

**Problem Solved:** Agents make claims without verification

**Solution:** Mandatory cite-before-claim protocol

**Implementation:**

```yaml
# agents/_base.agent.yaml
agent:
  rules:
    - "VERIFICATION PROTOCOL: No claims without fresh evidence"
    - "PROCESS:"
    - "  1. IDENTIFY what needs verification"
    - "  2. EXECUTE appropriate tool (Read, Grep, LSP)"
    - "  3. CITE specific file:line references"
    - "  4. ONLY THEN make assertion"
    - ""
    - "BAD:  'The auth middleware should handle this'"
    - "GOOD: 'auth.ts:42-55 implements JWT verification, but line 48 lacks exp validation'"
```

**Enforcement Hook:**

```yaml
# hooks/pre-response-verification.yaml
hook:
  name: "Evidence Verification"
  trigger: "before_response"

  check:
    - pattern: "claim_without_citation"
      block: true
      message: "Response contains claims without file:line citations. Re-verify."

  require:
    - "At least one Read/Grep/LSP tool call per assertion"
    - "File paths + line numbers for all code references"
```

### 6.3 Circuit Breaker Pattern for Debugging

**Problem Solved:** Infinite debugging loops waste tokens

**Solution:** Hard limit with automatic escalation

**Implementation:**

```yaml
# skills/error-resolver/skill.md
## Debugging Circuit Breaker

**IRON LAW:** After 3 failed fix attempts → STOP

**Escalation Protocol:**
1. Count fix attempts in state
2. After attempt #3, if tests still fail:
   - STOP execution
   - Document failure pattern
   - Escalate to architect-high for root cause analysis
   - Architect examines system design, not symptoms

**State Tracking:**
```yaml
# .omc/state/debug-session.yaml
debug:
  target_error: "Auth middleware test failure"
  attempts:
    - iteration: 1
      change: "Added null check"
      result: "Still failing"
    - iteration: 2
      change: "Fixed async handling"
      result: "Still failing"
    - iteration: 3
      change: "Updated test mock"
      result: "Still failing"
  circuit_breaker_triggered: true
  escalated_to: "architect-high"
```

### 6.4 Smart Question Protocol (Planner Agent)

**Problem Solved:** Users burdened with answering questions AI can answer

**Solution:** Distinguish codebase facts vs user preferences

**Implementation:**

```yaml
# agents/planner.agent.yaml
agent:
  persona:
    principles:
      - "NEVER ask users questions the codebase can answer"
      - "Use researcher agent for codebase facts"
      - "Only ask users about preferences, deadlines, priorities"

  rules:
    - "CODEBASE FACTS (use researcher, NOT user):"
    - "  - What auth system do we use?"
    - "  - Where is the user model?"
    - "  - What's our test coverage?"
    - ""
    - "USER PREFERENCES (ask via AskUserQuestion):"
    - "  - What's your deadline?"
    - "  - Performance vs simplicity?"
    - "  - Risk tolerance?"
    - ""
    - "RULE: Ask ONE question at a time"

  menu:
    - trigger: "plan"
      action: |
        1. Use researcher to gather codebase context
        2. Ask user ONLY preference questions (one at a time)
        3. Generate plan in .plans/
        4. Display summary and await approval
        5. Direct to /start-work (don't implement)
```

### 6.5 Notepad Wisdom System

**Problem Solved:** Context loss between sessions, repeated mistakes

**Solution:** Persistent knowledge capture with auto-timestamps

**Implementation:**

```yaml
# skills/notepad-wisdom/skill.md
## Notepad System

**Auto-Capture Points:**
- After successful complex task → learnings.md
- Architecture decision made → decisions.md
- Blocker encountered → issues.md
- Open question identified → problems.md

**Format:**
```markdown
# learnings.md
## 2026-02-01: Database Migration Pattern
Context: User authentication refactor
Learning: Always run migrations in transaction with rollback plan
Specific: See `migrations/20260201_auth.sql:15-42` for pattern
Tags: #database #migration #best-practice

## 2026-01-28: Test Flakiness Resolution
Context: E2E tests intermittent failures
Learning: Mock external services, don't rely on network
Specific: `tests/setup.ts:10-25` shows mock server pattern
Tags: #testing #e2e #reliability
```

**Integration:**
- Orchestrator reads learnings before task planning
- Avoids repeating documented mistakes
- Suggests proven patterns from history
```

**Directory Structure:**

```
.plans/
├── current-plan.md          # Active work plan
├── learnings.md             # Successful patterns
├── decisions.md             # Architecture decisions (ADR-lite)
├── issues.md                # Known blockers + workarounds
└── problems.md              # Open questions
```

### 6.6 Magic Keywords (Natural Language Activation)

**Problem Solved:** Command memorization friction

**Solution:** Inline keyword detection in natural language

**Implementation:**

```yaml
# config/magic-keywords.yaml
keywords:
  execution_modes:
    - trigger: ["autopilot", "build me", "create fully"]
      skill: "autopilot"
      description: "Full autonomous execution"

    - trigger: ["ralph", "don't stop", "persist until"]
      skill: "ralph"
      description: "Keep trying until verified complete"

    - trigger: ["parallel", "ulw", "ultrawork"]
      modifier: "parallel_execution"
      description: "Run tasks in parallel"

    - trigger: ["tdd", "test first", "test-driven"]
      skill: "tdd-workflow"
      description: "Red-Green-Refactor cycle"

  composable:
    - trigger: ["ralph ulw", "ralph parallel"]
      skills: ["ralph", "ultrawork"]
      description: "Persistent + parallel execution"

# Orchestrator usage:
# User: "ralph ulw: migrate database schema"
# → Activates: ralph skill + ultrawork modifier
# → Plan: Persistent execution with parallel migrations
```

### 6.7 Delegation Enforcer (Pre-Tool-Use Hook)

**Problem Solved:** Manual model selection for every sub-task

**Solution:** Automatic model injection via hooks

**Implementation:**

```yaml
# hooks/delegation-enforcer.yaml
hook:
  name: "Auto-Model Injection"
  trigger: "pre_tool_use"

  targets:
    - tool: "Task"
      action: "inject_model"
    - tool: "Agent"
      action: "inject_model"

  logic: |
    1. Intercept Task/Agent tool call
    2. Parse subagent_type parameter
    3. Lookup agent's default model from agents/{subagent_type}.agent.yaml
    4. If model not explicitly specified:
       - Inject default model
    5. If model explicitly specified:
       - Preserve override (tactical adjustment)
    6. Execute modified tool call

# Example:
# Input:  Agent(subagent_type="architect")
# Lookup: agents/architect.agent.yaml → model: "opus"
# Output: Agent(subagent_type="architect", model="opus")
```

### 6.8 Analytics & Cost Monitoring (HUD System)

**Problem Solved:** Blind spending, no visibility into token usage

**Solution:** Real-time HUD + historical analysis

**Implementation:**

```yaml
# skills/analytics/hud-system.md
## Real-Time HUD

**Display Components:**
```
┌─────────────────────────────────────────────┐
│ Atlas 🎯 (Opus) | Plan Phase               │
│ Tokens: 12,450 in / ~4,980 out              │
│ Cost: ~$0.52 | Budget: $4.48 remaining      │
│ Skills: [autopilot, parallel]               │
└─────────────────────────────────────────────┘
```

**Update Cycle:**
- Render: <100ms total
- Token extraction: <5ms per cycle
- Summary loading: <10ms (mtime-cached)

**Budget Alerts:**
- Warning: Yellow at $2 spent
- Critical: Red at $5 spent
- Block execution: Optional threshold

**Historical Analysis:**
- Scan ~/.claude/projects/ transcripts
- Parse actual token usage (not estimates)
- Run every 24 hours
- Deduplicate via hash(sessionId + timestamp + model)

**Storage:**
```yaml
# .analytics/session-2026-02-01.yaml
session:
  date: "2026-02-01"
  total_tokens: 45230
  total_cost: 2.34

  by_agent:
    - agent: "architect"
      model: "opus"
      tokens: 15400
      cost: 1.20

    - agent: "backend-engineer"
      model: "sonnet"
      tokens: 22100
      cost: 0.88

    - agent: "test-engineer"
      model: "haiku"
      tokens: 7730
      cost: 0.26

  cost_efficiency:
    tier_routing_savings: 47%
    vs_all_opus: 4.42
```

### 6.9 State Synchronization System

**Problem Solved:** Documentation drifts out of sync with code

**Solution:** Single-source-of-truth with auto-sync

**Implementation:**

```yaml
# scripts/sync-metadata.yaml
sync:
  source: "package.json"  # Single source of truth

  targets:
    - file: "README.md"
      patterns:
        - pattern: "Version: (.*)"
          replace: "Version: ${version}"
        - pattern: "Agents: (\\d+)"
          replace: "Agents: ${agent_count}"

    - file: "CLAUDE.md"
      patterns:
        - pattern: "everything-claude-code v(.*)"
          replace: "everything-claude-code v${version}"

    - file: "workflows/*/workflow.yaml"
      patterns:
        - pattern: "version: \"(.*)\""
          replace: "version: \"${version}\""

  computed_values:
    agent_count:
      source: "agents/*.agent.yaml"
      compute: "count_files"

    skill_count:
      source: "skills/**/skill.md"
      compute: "count_files"

    workflow_count:
      source: "workflows/*/workflow.yaml"
      compute: "count_files"

  operations:
    - mode: "sync"       # Apply changes
    - mode: "dry-run"    # Preview only
    - mode: "verify"     # Check status (exit 0=synced, 1=out-of-sync)

  atomic_writes:
    - read_entire_file
    - apply_all_replacements_to_string
    - compare_original_vs_modified
    - write_only_if_changed

  hooks:
    - trigger: "pre-commit"
      action: "verify"
      fail_if_out_of_sync: true

    - trigger: "npm version"
      action: "sync"

    - trigger: "ci/cd"
      action: "verify"
      fail_pr_if_stale: true
```

### 6.10 Template-Based Agent Inheritance

**Problem Solved:** Duplicate configuration across similar agents

**Solution:** Base templates with tier overlays

**Implementation:**

```yaml
# agents/templates/base-agent.template.yaml
agent:
  metadata:
    module: "core"
    hasSidecar: false

  config:
    tools: ["Read", "Grep", "Glob"]
    inherits: "_base"

  persona:
    communication_style: |
      {{TIER_COMMUNICATION_STYLE}}

  rules:
    - "VERIFICATION PROTOCOL: No claims without fresh evidence"
    - "{{TIER_CONSTRAINTS}}"
    - "{{AGENT_SPECIFIC_RULES}}"

# agents/templates/tier-low.template.yaml
tier:
  level: "low"
  model: "haiku"

  constraints:
    - "Simple, single-focus tasks only"
    - "Concise responses (under 500 tokens)"
    - "ESCALATE if task exceeds complexity boundary"

  escalation_triggers:
    - "Multi-file coordination needed"
    - "Architecture decisions required"
    - "Cross-system dependencies"
    - "Multiple failed attempts (>2)"

  communication_style: |
    Brief and focused. Single-purpose responses.

# agents/architect-low.agent.yaml (generated from templates)
agent:
  metadata:
    id: "architect-low"
    name: "Quick Architect"
    tier: "low"
    template: "base-agent"
    tier_template: "tier-low"

  config:
    model: "haiku"
    tools: ["Read", "Grep", "Glob", "LSP"]
    escalation_agent: "architect"

  # Injected from tier-low template
  persona:
    communication_style: "Brief and focused. Single-purpose responses."

  rules:
    - "VERIFICATION PROTOCOL: No claims without fresh evidence"
    - "Simple, single-focus tasks only"
    - "Concise responses (under 500 tokens)"
    - "ESCALATE if task exceeds complexity boundary"

  # Agent-specific additions
  specialized_protocols:
    quick_analysis: |
      1. Read target file
      2. Run LSP diagnostics
      3. Cite specific issues with line numbers
      4. If >3 issues or cross-file → escalate to architect
```

**Benefits:**
- No duplication of verification protocols
- Consistent tier behavior
- Easy to update shared rules
- Clear inheritance chain

---

## 7. Comparison with everything-claude-code

### 7.1 Architectural Similarities

| Aspect | oh-my-claudecode | everything-claude-code |
|--------|------------------|------------------------|
| **Multi-Agent** | 32 tiered agents | 24 agents with MCP multi-model |
| **Orchestration** | Skill-based routing | Orchestrator-first with JSON plan |
| **Agent Format** | YAML frontmatter + XML | YAML agent definitions |
| **State Management** | .omc/ directory | Frontmatter + .state/ |
| **Workflows** | Autopilot 5-phase | Step-file workflows with JIT loading |
| **Cost Optimization** | Tiered agents (Haiku/Sonnet/Opus) | MCP model routing (Gemini/o3/GPT-4o) |

### 7.2 Key Differences

| Feature | oh-my-claudecode | everything-claude-code |
|---------|------------------|------------------------|
| **Agent Tiers** | LOW/MEDIUM/HIGH per role | Single agent + MCP model selection |
| **Activation** | Magic keywords inline | Slash commands + orchestrator |
| **Execution Modes** | 7 modes (autopilot, ralph, ulw) | Orchestrator phases (parallel/sequential) |
| **Analytics** | Built-in HUD + token tracking | Not implemented |
| **Persistence** | Ralph mode (retry until verified) | Checkpoint system |
| **Skills** | 37 reusable skills | Skills referenced in plans |
| **Templates** | Base templates for agents | Workflow step templates |
| **Sync System** | Automated metadata sync | Manual updates |
| **Notepad** | Wisdom system (learnings/decisions) | Not implemented |

### 7.3 Recommended Adoptions

**High Priority:**

1. **Tiered Agents** → Add -low/-high variants for cost optimization
2. **Evidence-First Protocol** → Enforce verification before claims
3. **Circuit Breaker** → Stop infinite debugging loops
4. **Notepad Wisdom** → Capture learnings/decisions
5. **Analytics HUD** → Real-time token/cost visibility

**Medium Priority:**

6. **Smart Questions** → Distinguish codebase facts vs user prefs
7. **Magic Keywords** → Natural language activation
8. **Delegation Enforcer** → Auto-inject model based on agent
9. **State Sync** → Keep docs in sync with code

**Low Priority:**

10. **Template Inheritance** → Already have _base.agent.yaml
11. **Ralph Mode** → Similar to checkpoint system

---

## 8. Implementation Roadmap for everything-claude-code

### Phase 1: Cost Optimization (Week 1)

**Add Tiered Agents:**

```yaml
# agents/architect-low.agent.yaml
agent:
  metadata:
    id: "architect-low"
    tier: "low"
    inherits: "architect"
  config:
    model: "haiku"
    mcp_model: null
    escalation_agent: "architect"

# agents/architect.agent.yaml (medium - rename existing)
agent:
  metadata:
    tier: "medium"
  config:
    model: "sonnet"
    escalation_agent: "architect-high"

# agents/architect-high.agent.yaml
agent:
  metadata:
    tier: "high"
  config:
    model: "opus"
    mcp_model: "gemini"
```

**Update Orchestrator:**
- Add tier selection logic to execution plans
- Implement auto-escalation on complexity detection

### Phase 2: Verification & Quality (Week 2)

**Evidence-First Protocol:**

```yaml
# agents/_base.agent.yaml
agent:
  rules:
    - "VERIFICATION PROTOCOL: No claims without fresh evidence"
    - "PROCESS: IDENTIFY → EXECUTE tool → CITE file:line → ASSERT"
    - "Every code reference must cite specific file:line"
```

**Circuit Breaker:**

```yaml
# skills/error-resolver/circuit-breaker.md
## Debugging Circuit Breaker

After 3 failed fix attempts:
1. STOP execution
2. Document failure pattern
3. Escalate to architect-high
4. Examine system design, not symptoms
```

### Phase 3: Analytics & Visibility (Week 3)

**Token Tracking:**

```yaml
# .analytics/session-tracking.yaml
session:
  id: "uuid"
  date: "2026-02-01"
  total_tokens: 0
  total_cost: 0.0
  by_agent: []
  by_phase: []
```

**HUD System:**

```bash
# Display in terminal
┌─────────────────────────────────────────────┐
│ Atlas 🎯 (Gemini) | Planning Phase          │
│ Tokens: 8,420 in / ~3,368 out               │
│ Cost: ~$0.15 | Budget: $9.85 remaining      │
│ Skills: [tdd-workflow, frontend-patterns]   │
└─────────────────────────────────────────────┘
```

### Phase 4: Knowledge Management (Week 4)

**Notepad Wisdom:**

```
.plans/
├── current-plan.md
├── learnings.md
├── decisions.md
├── issues.md
└── problems.md
```

**Smart Questions (Planner):**

```yaml
agent:
  rules:
    - "Use researcher for codebase facts"
    - "Ask users only preferences/deadlines/priorities"
    - "One question at a time"
```

---

## 9. Critical Insights

### 9.1 What Works Exceptionally Well

1. **Tiered Agents = 47% Cost Savings**
   - Haiku for simple tasks
   - Sonnet for standard work
   - Opus for complex reasoning
   - Automatic escalation prevents waste

2. **Evidence-First Protocol = Higher Quality**
   - Forces tool use before claims
   - File:line citations mandatory
   - Prevents hallucination

3. **Circuit Breaker = No Infinite Loops**
   - Hard limit at 3 attempts
   - Escalates to architecture review
   - Saves tokens + time

4. **Magic Keywords = Zero Learning Curve**
   - "ralph ulw: migrate database"
   - Natural language activation
   - Composable modifiers

5. **Notepad Wisdom = Institutional Memory**
   - Captures learnings automatically
   - Prevents repeated mistakes
   - Builds project knowledge base

### 9.2 Potential Pitfalls

1. **Too Many Agents** (35 agents)
   - Maintenance burden
   - Role overlap confusion
   - Consider consolidation

2. **Complex State Management** (.omc/ + hooks + sync)
   - Multiple state sources
   - Sync complexity
   - Potential for drift

3. **Magic Keywords Can Conflict**
   - "build me" vs "build the"
   - Ambiguous triggers
   - Need careful design

### 9.3 Unique Competitive Advantages

1. **Cost Optimization** (47% savings)
2. **Parallel Execution** (3-5x speedup)
3. **Persistence** (Ralph mode)
4. **Analytics** (Real-time visibility)
5. **Zero Learning Curve** (Natural language)

---

## 10. Conclusion

oh-my-claudecode represents a **mature, production-ready multi-agent orchestration system** with strong focus on:

- **Cost efficiency** through intelligent tier routing
- **Quality** via evidence-first protocols
- **Developer experience** through magic keywords
- **Visibility** via real-time analytics
- **Reliability** through circuit breakers and persistence

**Key Takeaways for everything-claude-code:**

1. **ADOPT:** Tiered agents, evidence-first protocol, circuit breaker, notepad wisdom, analytics
2. **ADAPT:** Magic keywords to our orchestrator-first architecture
3. **AVOID:** Too many agents (35 vs our 24), complex state sync
4. **INNOVATE:** Combine their tier system with our MCP multi-model approach

**Recommended Priority:**
1. Tiered agents (HIGH ROI: 47% cost savings)
2. Analytics HUD (HIGH visibility)
3. Evidence protocol (HIGH quality)
4. Circuit breaker (MEDIUM token savings)
5. Notepad wisdom (MEDIUM long-term value)

---

**Analysis Date:** 2026-02-01
**Repository Version:** 3.9.4
**Analysis Depth:** Comprehensive (architecture, features, patterns, code examples)
