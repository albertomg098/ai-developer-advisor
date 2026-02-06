# AI Dev Advisor Framework

An opinionated Claude Code plugin that turns Claude into an interactive AI development advisor — diagnosing situations, enforcing test-first discipline, and tracking progress across sessions.

---

## What This Is

This is a **Claude Code plugin** that provides:

- **4 Skills** that teach Claude Code evidence-driven development methodology
- **8 Slash Commands** for common advisor workflows (`/ai-dev-advisor:advisor`, `/ai-dev-advisor:test-first`, etc.)
- **Templates** for context tracking, investigations, and test-first development
- **Documentation** — a 7-day learning path for developers

**It is NOT a library or dependency.** It's a set of instructions and workflows that Claude Code reads and follows when helping you build software.

---

## Install as a Plugin

### Option 1: Install from GitHub

```bash
# In Claude Code, run:
/install-plugin https://github.com/albertomartin/claude-code-mental-model
```

### Option 2: Clone and install locally

```bash
git clone https://github.com/albertomartin/claude-code-mental-model
cd your-project
# Install from local path
/install-plugin /path/to/claude-code-mental-model
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
| `/ai-dev-advisor:review-evidence` | Run tests, check coverage, assess evidence pyramid, report readiness |
| `/ai-dev-advisor:switch-context` | Checkpoint current work, switch to a different context cleanly |
| `/ai-dev-advisor:setup-advisor` | Set up the advisor in a project — appends to CLAUDE.md, creates directories |

---

## Skills Included

Skills live in `skills/` and are automatically available when the plugin is installed:

| Skill | Description |
|-------|-------------|
| **dev-advisor** | Diagnosis decision trees, 4 development contexts, advisory patterns, workflow routing |
| **test-first** | Evidence pyramid, red-green-refactor, test levels, universal prompt templates |
| **context-management** | Context file system, naming conventions, switching protocol, daily workflow |
| **tool-selection** | Chat vs Code vs Plan decision matrix, context-specific tool selection |

---

## The 4 Development Contexts

The framework categorizes every task into one of four contexts:

| Context | When You're Here | Protocol |
|---------|-----------------|----------|
| 🔍 **EXPLORATION** | "I don't know what's wrong" | Hypothesis → test → document → repeat |
| 🔥 **FIREFIGHTING** | "Production is broken, fix NOW" | Reproduce → minimal fix → verify → deploy |
| 🏗️ **BUILDING** | "I know what to build" | Tests FAIL → implement → tests PASS → verify |
| ✨ **IMPROVING** | "It works, but could be better" | Baseline → improve → measure → verify |

---

## How It Works

1. **Describe your situation** — tell Claude what you're trying to do
2. **Get diagnosed** — the framework identifies your context
3. **Follow the protocol** — test-first, evidence-driven, step by step
4. **Track progress** — context files in `contexts/active/` persist across sessions
5. **Ship with confidence** — tests prove it works before you commit

### Multiple Contexts, Multiple Sessions

You can run several Claude Code sessions in parallel, each on a different task:

```
Terminal 1:  claude  →  "Fix the auth token bug"        → works on hotfix_auth_token.md
Terminal 2:  claude  →  "Add CSV export feature"         → works on feature_csv_export.md
Terminal 3:  claude  →  "Why are emails slow?"           → works on explore_email_delays.md
```

Context files (`contexts/active/*.md`) are what persist between sessions. Each session reads its context file at start and updates it at end. You don't need to create contexts manually — when you describe a task, the advisor offers to create one. For quick one-off tasks, skip context tracking entirely.

---

## Plugin Structure

```
ai-dev-advisor/
├── .claude-plugin/
│   └── plugin.json                   ← Plugin manifest
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
│   └── tool-selection/SKILL.md
├── commands/                         ← 8 slash commands (/ai-dev-advisor:*)
│   ├── advisor.md
│   ├── diagnose.md
│   ├── start-session.md
│   ├── create-context.md
│   ├── test-first.md
│   ├── review-evidence.md
│   ├── switch-context.md
│   └── setup-advisor.md
├── docs/                             ← 7-day learning path
│   ├── README.md
│   ├── 00-quick-start.md
│   ├── 01-context-management.md
│   ├── 02-chat-code-plan-guide.md
│   ├── 03-evidence-driven-dev.md
│   ├── 04-context-protocols.md
│   └── 05-skills-commands-hooks.md
└── reference/
    └── usage-analysis.html
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
