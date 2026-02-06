---
name: tool-selection
description: >
  Decision guide for choosing between Claude Chat, Claude Code, and Plan mode.
  Quick decision matrix, context-specific tool selection, Chat-to-Code pipeline
  patterns, and the research-first pipeline for unfamiliar technology.
  Use when unsure which tool to apply or designing multi-phase workflows.
---

# Tool Selection — Chat vs Code vs Plan Mode

## 🎯 Core Distinction

- **Chat** = Recipe creation (what to cook, how to cook it)
- **Code** = Executing the recipe (actually cooking)
- **Plan Mode** = Meal prep service (complex multi-step recipes)

---

## 📊 Quick Decision Matrix

| Situation | Chat | Code | Plan |
|-----------|------|------|------|
| "I don't know what's wrong" | ✅ | — | — |
| "I need to design an API" | ✅ | — | — |
| "Analyze these logs/errors" | ✅ | — | — |
| "Review my code for bugs" | ✅ | — | — |
| "I'm stuck and confused" | ✅ | — | — |
| "Implement feature (clear spec)" | — | ✅ | Maybe |
| "Fix a bug (know the fix)" | — | ✅ | — |
| "Create/modify files" | — | ✅ | — |
| "Run tests" | — | ✅ | — |
| "Create 5 related test files" | — | — | ✅ |
| "Refactor 20 files consistently" | — | — | ✅ |
| "Build complex multi-file feature" | Both | Both | ✅ |

---

## 🗣️ Chat — The Strategic Brain

### Use Chat When You Need To:

**THINK** (before acting)
- Compare approaches, evaluate trade-offs, make strategic decisions
- "I'm considering JWT vs OAuth vs rolling my own — help me decide"

**UNDERSTAND** (confusion or blockage)
- Generate hypotheses, create debugging checklists, get unstuck
- "My API returns 500 but logs show nothing — what am I missing?"

**ANALYZE** (data, logs, errors)
- Pattern recognition, data synthesis, root cause analysis
- "Analyze these 24h of logs — what are the top error patterns?"

**DESIGN** (architecture, APIs, data models)
- Trade-off evaluation, creative thinking, specification output
- "Design a database schema for multi-tenant conversation history"

**REVIEW** (code quality, security, performance)
- Critical analysis without code execution
- "Review this auth code for security vulnerabilities"

**LEARN** (how things work)
- Educational explanations with context
- "Explain FastAPI dependency injection with examples"

---

## 💻 Code — The Builder

### Use Code When You Need To:

**IMPLEMENT** (clear specifications)
- You know WHAT and HOW, just need execution
- "Implement POST /api/users/register following existing patterns"

**TEST** (create or run tests)
- Direct file manipulation and command execution
- "Create tests/test_voice.py and run them"

**DEBUG** (with known hypothesis)
- Execute a specific fix after Chat-based analysis
- "Fix src/audio.py line 45 — add None check before .get()"

**SCAFFOLD** (project setup, boilerplate)
- Bulk file creation with consistent structure
- "Create FastAPI microservice with src/, tests/, Dockerfile"

**REFACTOR** (simple, localized changes)
- Concrete changes with clear scope in 1–3 files
- "Extract duplicate validation into validate_email() function"

---

## 🎯 Plan Mode — The Orchestrator

### Use Plan Mode When:

**MULTI-FILE COORDINATED CHANGES**
- Rename across codebase, consistent updates to many files
- "Rename 'user' to 'member' throughout the entire codebase"

**COMPLEX MULTI-COMPONENT FEATURES**
- Implementation with layer dependencies
- "Implement auth system: models → repos → service → API → middleware → tests"

**BULK CONSISTENT OPERATIONS**
- Same operation across many files
- "Add type hints to all functions in src/"

**MIGRATIONS / MAJOR REFACTORING**
- High-risk changes requiring careful progression
- "Migrate from SQLAlchemy to Supabase step-by-step"

### When NOT to Use Plan Mode
- Single file changes
- Simple bug fixes
- Exploratory work
- When you want interactive course-correction

---

## 🔄 The Chat → Code → Plan Pipeline

### Full Workflow for Complex Features

```
Phase 1: DESIGN (Chat)
├── "I need to build [feature]. Design the architecture"
├── Output: Detailed spec
└── Decision: Simple or complex?

Phase 2a: SIMPLE (Code)
├── <5 files, straightforward
├── "Implement [feature] following [spec]"
└── Test and verify

Phase 2b: COMPLEX (Plan)
├── >5 files, many dependencies
├── "Implement [feature] step-by-step: [steps]"
└── Monitor each step

Phase 3: REVIEW (Chat)
├── Upload all new code
├── "Review implementation for issues"
└── List of improvements

Phase 4: REFINEMENT (Code)
├── "Apply review feedback: [list]"
└── Final verification
```

---

## 🔬 The Research-First Pipeline

### When to Use

Use research-first when you hear signals like:
- "I've never used [technology] before"
- "I need to evaluate different approaches"
- "I don't know which library to use"
- "How does [pattern] work? I need to understand before building"
- "This is new territory for me"

The key signal: the developer knows **WHAT** to build but not **HOW** (technology/approach is unfamiliar).

### 3-Phase Protocol

```
Phase 1: RESEARCH (Chat — no file access = focused thinking)
├── Create context file (contexts/active/feature_[name].md)
├── Build research prompt from template
├── Run in Claude Chat → get findings
└── PAUSE — user goes to Chat, returns with findings

Phase 2: PLAN (Code — full codebase access)
├── Receive pasted findings
├── Update context file with research results
├── Explore codebase — how does this fit?
├── Design approach: architecture + test strategy + steps
└── PAUSE — user approves plan

Phase 3: IMPLEMENT (Code — test-first)
├── Record approved plan in context file
└── Hand off to test-first workflow
```

### Phase Transition Phrases

Moving from Research → Plan:
> "Here are my research findings: [paste from Chat]. Let's plan the implementation."

Moving from Plan → Implement:
> "Plan looks good. Let's implement it test-first."

### Key Rules

1. **Research happens in Chat** — no file access keeps focus on thinking, not premature coding
2. **Plan and implement happen in Code** — needs codebase access for real design
3. **Context file required first** — create before researching so findings have a home
4. **Template saves time** — use `skills/tool-selection/templates/research-prompt.md` to build the Chat prompt
5. **Don't skip to implementation** — the whole point is: understand first, build second

---

## 🎯 Context-Specific Tool Selection

### 🔍 EXPLORATION
```
Primary:   Chat (analyze, generate hypotheses)
Secondary: Code (test hypotheses, gather evidence)
Rarely:    Plan (not helpful when exploring)

Flow: Chat → Code → Chat → Code → ... until root cause found
```

### 🔥 FIREFIGHTING
```
Primary:   Code (fast execution)
Secondary: Chat (if stuck or need review)
Rarely:    Plan (too slow for emergencies)

Flow: Code (reproduce) → Code (fix) → Code (verify) → Chat (post-mortem)
```

### 🏗️ BUILDING
```
Primary:   Code (implementation)
Often:     Chat (design before building)
Sometimes: Plan (if complex, >5 files)

Flow: Chat (design) → Code (tests) → Code/Plan (implement) → Chat (review) → Code (refine)
```

### ✨ IMPROVING
```
Primary:   Chat (analysis and strategy)
Secondary: Code (apply improvements)
Sometimes: Plan (bulk refactoring)

Flow: Chat (analyze) → Chat (prioritize) → Code/Plan (improve) → Chat (measure)
```

---

## 💡 Pro Tips

### Use Chat as Your "Rubber Duck"
Before implementing anything complex:
```
"I'm about to implement [X] by doing [Y].
What am I not considering? What will break? Simpler approach?"
```

### The "Stuck for 20 Minutes" Rule
```
Stuck in Code for >20 min without progress?
STOP → Switch to Chat → Get unstuck → Return to Code
```

### Context Switching = Tool Switching
```
Feel lost         → Switch to Chat
Know what to do   → Switch to Code
Clear multi-step  → Use Plan
```

---

## 📋 Decision Checklist

Before starting ANY work:

1. **Do I need to THINK or UNDERSTAND?** → Chat first
2. **Do I know exactly what to build and how?** → Code
3. **Is this >5 files with dependencies?** → Plan mode
4. **Am I stuck or confused?** → Switch to Chat
5. **Do I need to review or analyze?** → Chat
