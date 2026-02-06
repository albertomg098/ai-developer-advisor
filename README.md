# AI Dev Advisor Framework

An opinionated Claude Code plugin that turns Claude into an interactive AI development advisor — diagnosing situations, enforcing test-first discipline, and tracking progress across sessions.

---

## What This Is

This is a **Claude Code plugin** that provides:

- **4 Skills** that teach Claude Code evidence-driven development methodology
- **10 Slash Commands** for common advisor workflows (`/ai-dev-advisor:advisor`, `/ai-dev-advisor:test-first`, etc.)
- **Templates** for context tracking, investigations, and test-first development
- **Documentation** — a 7-day learning path for developers

**It is NOT a library or dependency.** It's a set of instructions and workflows that Claude Code reads and follows when helping you build software.

---

## Install as a Plugin

### Option 1: Marketplace Install (Recommended)

```
/plugin marketplace add albertomg098/ai-developer-advisor
/plugin install ai-dev-advisor@ai-dev-advisor-marketplace
```

### Option 2: Local Testing

Clone the repo and point Claude Code at it:

```bash
git clone https://github.com/albertomg098/ai-developer-advisor
claude --plugin-dir /path/to/ai-developer-advisor
```

### Uninstalling

```
/plugin uninstall ai-dev-advisor
```

### After Installing: Run Setup

Open Claude Code in your project and run:

```
/ai-dev-advisor:setup-advisor
```

This will:
- Append the advisor section to your existing CLAUDE.md (without overwriting anything)
- Create `contexts/active/`, `contexts/backlog/`, `contexts/archive/`, `investigations/`

### Start Using It

Just describe what you want:

```
I want to add user authentication with JWT
```

Claude will diagnose (🏗️ BUILDING), guide you test-first, and offer to create context tracking.

Or use commands explicitly: `/ai-dev-advisor:advisor`, `/ai-dev-advisor:test-first`, etc.

---

## Available Commands

Use these via `/ai-dev-advisor:<name>` in Claude Code:

| Command | What It Does |
|---------|-------------|
| `/ai-dev-advisor:advisor` | Full advisory interaction — describe your situation, get diagnosed and guided |
| `/ai-dev-advisor:diagnose` | Identify your development context (exploration, firefighting, building, improving) |
| `/ai-dev-advisor:start-session` | Begin a work session — review active contexts, get session checklist |
| `/ai-dev-advisor:create-context` | Generate a context tracking file from template |
| `/ai-dev-advisor:test-first` | Guided test-first implementation in 3 phases with pauses |
| `/ai-dev-advisor:research` | Research-first workflow for unfamiliar technology (3 phases with pauses) |
| `/ai-dev-advisor:review-evidence` | Run tests, check coverage, assess evidence pyramid, report readiness |
| `/ai-dev-advisor:switch-context` | Checkpoint current work, switch to a different context cleanly |
| `/ai-dev-advisor:setup-advisor` | Set up the advisor in a project — appends to CLAUDE.md, creates directories |
| `/ai-dev-advisor:setup-parallel` | Set up parallel worktree sessions for multiple tasks |

---

## Skills Included

Skills live in `skills/` and are automatically available when the plugin is installed:

| Skill | Description |
|-------|-------------|
| **dev-advisor** | Diagnosis decision trees, 4 development contexts, advisory patterns, workflow routing |
| **test-first** | Evidence pyramid, red-green-refactor, test levels, universal prompt templates |
| **context-management** | Context file system, naming conventions, switching protocol, daily workflow |
| **tool-selection** | Chat vs Code vs Plan decision matrix, context-specific tool selection, research-first pipeline |

---

## The 4 Development Contexts

The framework categorizes every task into one of four contexts:

| Context | When You're Here | Protocol |
|---------|-----------------|----------|
| 🔍 **EXPLORATION** | "I don't know what's wrong" | Hypothesis → test → document → repeat |
| 🔥 **FIREFIGHTING** | "Production is broken, fix NOW" | Reproduce → minimal fix → verify → deploy |
| 🏗️ **BUILDING** | "I know what to build" | Research (if needed) → Tests FAIL → implement → tests PASS → verify |
| ✨ **IMPROVING** | "It works, but could be better" | Baseline → improve → measure → verify |

---

## How It Works

1. **Describe your situation** — tell Claude what you're trying to do
2. **Get diagnosed** — the framework identifies your context
3. **Research if needed** — unfamiliar tech? Research in Chat first, then plan in Code
4. **Follow the protocol** — test-first, evidence-driven, step by step
5. **Track progress** — context files in `contexts/active/` persist across sessions
6. **Ship with confidence** — tests prove it works before you commit

### Multiple Contexts, Parallel Sessions

Use git worktrees to work on multiple tasks in parallel — each task gets its own directory, branch, and Claude Code session:

```bash
# Set up parallel sessions (or use /ai-dev-advisor:setup-parallel)
git worktree add ../myapp-hotfix-auth-bug/ hotfix/auth-bug
git worktree add ../myapp-feature-csv-export/ feature/csv-export
git worktree add ../myapp-explore-email-delays/ explore/email-delays

# Open a terminal for each:
cd ../myapp-hotfix-auth-bug/ && claude       # works on hotfix_auth-bug.md
cd ../myapp-feature-csv-export/ && claude    # works on feature_csv-export.md
cd ../myapp-explore-email-delays/ && claude  # works on explore_email-delays.md
```

Context files (`contexts/active/*.md`) are shared across all worktrees (same git repo). Each session auto-detects its branch and loads the matching context file. For quick one-off tasks, skip context tracking entirely.

---

## Plugin Structure

```
ai-dev-advisor/
├── .claude-plugin/
│   ├── plugin.json                   ← Plugin manifest
│   └── marketplace.json              ← Marketplace descriptor
├── .gitignore
├── CLAUDE.md                         ← Advisor identity (Claude reads this)
├── README.md                         ← You are here
├── skills/                           ← 4 Anthropic-format skills (with templates)
│   ├── dev-advisor/SKILL.md
│   ├── test-first/
│   │   ├── SKILL.md
│   │   └── templates/test-first-prompt.md
│   ├── context-management/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── context-file.md
│   │       ├── investigation-file.md
│   │       └── session-checklist.md
│   └── tool-selection/
│       ├── SKILL.md
│       └── templates/
│           └── research-prompt.md
├── commands/                         ← 10 slash commands (/ai-dev-advisor:*)
│   ├── advisor.md
│   ├── diagnose.md
│   ├── start-session.md
│   ├── create-context.md
│   ├── test-first.md
│   ├── research.md
│   ├── review-evidence.md
│   ├── switch-context.md
│   ├── setup-advisor.md
│   └── setup-parallel.md
└── docs/                             ← 7-day learning path
    ├── README.md
    ├── 00-quick-start.md
    ├── 01-context-management.md
    ├── 02-chat-code-plan-guide.md
    ├── 03-evidence-driven-dev.md
    ├── 04-context-protocols.md
    └── 05-skills-commands-hooks.md
```

---

## Learning Path

New to the framework? Read the docs in order:

| Day | Read | Practice |
|-----|------|----------|
| 1 | `docs/00-quick-start.md` | Create your first context file, do one session |
| 2 | `docs/03-evidence-driven-dev.md` | Build something test-first |
| 3 | `docs/02-chat-code-plan-guide.md` | Use Chat for design, Code for implementation |
| 4 | `docs/04-context-protocols.md` | Practice each context type |
| 5 | `docs/05-skills-commands-hooks.md` | Set up commands and hooks |
| 6–7 | `docs/01-context-management.md` | Maintain 2–3 active contexts |

---

## Core Principles

1. **Tests first, code second, evidence always** — never ship without proof it works
2. **One context, one session, one outcome** — focus beats multitasking
3. **Document now or debug forever** — context files are your memory

---

## Contributing

This is a living framework. As you use it:

- **Found a pattern that works?** Add it to the relevant skill
- **Missing a command?** Create one in `commands/`
- **Docs unclear?** Improve them in `docs/`
