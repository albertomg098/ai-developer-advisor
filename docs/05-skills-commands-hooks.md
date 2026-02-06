# Skills, Commands, and Hooks — Advanced Features
## Leverage Claude Code's power features for maximum productivity

---

## 🎓 Skills — Expert Knowledge Modules

### What Are Skills?

Skills are **specialized knowledge documents** that Claude Code reads before working on specific tasks.

Think of them as **consultant playbooks** that teach Claude best practices for specific types of work.

Skills live in `skills/` and follow the Anthropic format: a `SKILL.md` file with YAML frontmatter describing when the skill should be used.

---

### Framework Skills (This Repo)

These skills are included in this framework and available in `skills/`:

```
skills/
├── dev-advisor/SKILL.md       ← Diagnosis, workflow routing, advisory patterns
├── test-first/SKILL.md        ← Evidence pyramid, red-green-refactor, test levels
├── context-management/SKILL.md ← Context files, switching, daily workflow
└── tool-selection/SKILL.md     ← Chat vs Code vs Plan decision matrix
```

| Skill | When It's Used |
|-------|---------------|
| **dev-advisor** | When the user asks what to do, describes a problem, or needs help choosing an approach |
| **test-first** | When implementing features, fixing bugs, or any task that produces code |
| **context-management** | When juggling multiple tasks, switching work items, or setting up tracking |
| **tool-selection** | When unsure which tool to apply or designing multi-phase workflows |

---

### How Skills Work

Skills are **automatically discovered** by Claude Code when they're in `skills/`. The YAML frontmatter tells Claude when to apply each skill:

```yaml
---
name: test-first
description: >
  Evidence-driven test-first development methodology. Covers the evidence
  pyramid, red-green-refactor workflow, test-level hierarchy, and universal
  test-first prompt templates. Use when implementing features, fixing bugs,
  or any task that produces code.
---
```

You can also reference skills explicitly:

```bash
# Claude will read the skill before responding
claude "Using the test-first skill, help me implement user authentication"
```

---

### Creating Your Own Skills

After completing a complex feature, extract reusable patterns:

```bash
# Create a new skill in skills/
mkdir -p skills/my-patterns

claude "Analyze the voice AI system we just built.
Extract reusable patterns into skills/my-patterns/SKILL.md with:
- YAML frontmatter (name + description)
- Architecture patterns
- Common pitfalls
- Code templates
- Testing strategies"
```

### Skill Template

```markdown
---
name: [skill-name]
description: >
  [When Claude should use this skill. Be specific about triggers
  and use cases.]
---

# [Skill Name]

## When to Use
[Describe triggers and situations]

## Core Patterns

### Pattern 1: [Name]
**Use case:** [When]
**Implementation:** [How]
**Why:** [Reasoning]

## Common Mistakes
1. [Mistake] → [Correct approach]

## Testing Strategy
[How to test this type of code]
```

---

## 💬 Commands — Reusable Slash Prompts

### What Are Commands?

Commands are **shortcuts for frequently used prompts**. They live in `commands/` and are invoked via `/ai-dev-advisor:<name>`.

---

### Framework Commands (This Repo)

These commands are included and ready to use:

| Command | Purpose | Usage |
|---------|---------|-------|
| `/ai-dev-advisor:advisor` | Full advisory interaction | Describe situation → get diagnosed and guided |
| `/ai-dev-advisor:diagnose` | Identify context type | Walk diagnosis tree → output context + protocol |
| `/ai-dev-advisor:start-session` | Begin work session | List active contexts → recommend focus → checklist |
| `/ai-dev-advisor:create-context` | Generate context file | Ask type/goal → create from template → suggest branch |
| `/ai-dev-advisor:test-first` | Test-first implementation | Phase 1: tests → Phase 2: implement → Phase 3: verify |
| `/ai-dev-advisor:research` | Research-first workflow | For unfamiliar tech — 3 phases with pauses |
| `/ai-dev-advisor:review-evidence` | Full validation gate | Tests → real inputs → evidence file → context update → ship |
| `/ai-dev-advisor:switch-context` | Switch work context | Checkpoint → commit WIP → update → load new context |
| `/ai-dev-advisor:setup-advisor` | Set up advisor in project | Appends to CLAUDE.md → creates directories |
| `/ai-dev-advisor:setup-parallel` | Parallel worktree sessions | Set up git worktrees for multiple simultaneous tasks |

### Using Commands

```bash
# Start your day
/ai-dev-advisor:start-session

# Get help with a situation
/ai-dev-advisor:advisor I have a production bug in checkout

# Implement something test-first
/ai-dev-advisor:test-first add email validation to signup form

# Check if your code is ready to ship
/ai-dev-advisor:review-evidence

# Switch to a different task
/ai-dev-advisor:switch-context feature_user_export
```

---

### Creating Your Own Commands

Add a markdown file to `commands/`:

```bash
# Create a new command
cat > commands/my-command.md << 'EOF'
Description of what this command does.

Steps to follow:
1. [Step 1]
2. [Step 2]

User's input: $ARGUMENTS
EOF
```

The `$ARGUMENTS` placeholder receives whatever the user types after the command name.

### Example: Custom Pre-Deploy Command

```markdown
# commands/pre-deploy.md

Run all pre-deployment checks:

1. Run full test suite: `pytest tests/ -v`
2. Check coverage: `pytest tests/ --cov=src --cov-fail-under=80`
3. Run linter: `ruff check src/`
4. Check for security issues in dependencies
5. Verify no TODO/FIXME in code being deployed

Report results and give GO/NO-GO verdict.

Target: $ARGUMENTS
```

---

## 🪝 Hooks — Automated Quality Gates

### What Are Hooks?

Hooks are **scripts that run automatically** on specific events (usually git events). They enforce your workflow rules without you having to remember.

---

### Essential Git Hooks

#### 1. Pre-Commit Hook (Quality Gate)

```bash
# .git/hooks/pre-commit
#!/bin/bash
echo "🔍 Running pre-commit checks..."

pytest tests/ -v --tb=short
if [ $? -ne 0 ]; then
    echo "❌ Tests failed. Fix tests before committing."
    exit 1
fi

pytest tests/ --cov=src --cov-fail-under=80 -q
if [ $? -ne 0 ]; then
    echo "⚠️  Coverage below 80%. Add tests or use --no-verify."
    exit 1
fi

echo "✅ All checks passed!"
```

#### 2. Commit-Msg Hook (Format Enforcement)

```bash
# .git/hooks/commit-msg
#!/bin/bash
commit_msg=$(cat "$1")

if ! echo "$commit_msg" | grep -qE '^(feat|fix|docs|test|refactor|chore)\([a-z-]+\): .+'; then
    echo "❌ Invalid commit format. Use: type(scope): message"
    echo "Examples: feat(auth): add JWT refresh, fix(voice): handle MP3 errors"
    exit 1
fi
```

#### 3. Pre-Push Hook (Final Safety)

```bash
# .git/hooks/pre-push
#!/bin/bash
echo "🚀 Running pre-push checks..."

pytest tests/ -v
if [ $? -ne 0 ]; then
    echo "❌ Tests failed. Cannot push."
    exit 1
fi

echo "✅ Safe to push!"
```

### Installing Hooks

```bash
claude "Set up git hooks for my project:
1. pre-commit: tests + coverage >80%
2. commit-msg: type(scope): message format
3. pre-push: full test suite

Make all executable and test them."
```

---

## 🎯 Combining Skills, Commands, and Hooks

### The Power Combo

```bash
# 1. COMMAND: Start your day
/ai-dev-advisor:start-session

# 2. COMMAND: Create test-first implementation
/ai-dev-advisor:test-first bulk CSV export

# 3. SKILL: Claude applies test-first methodology automatically
# (dev-advisor and test-first skills guide the process)

# 4. COMMAND: Full validation gate
/ai-dev-advisor:review-evidence
# Runs tests, validates real inputs, writes evidence file,
# updates context, and asks: "Commit and create PR?"

# 5. You say "yes" → committed, PR created, context archived
```

---

## 📋 Setup Checklist

**Set up once, benefit forever:**

```
Skills:
□ Review framework skills: ls skills/
□ Create a skill for your project's common patterns
□ Reference skills explicitly when needed

Commands:
□ Test each /ai-dev-advisor:* command
□ Create project-specific commands if needed

Hooks:
□ Create pre-commit hook (tests + coverage)
□ Create commit-msg hook (format enforcement)
□ Create pre-push hook (full validation)
□ Test hooks with sample commits
```

---

## 🎓 Practice Exercise

**Build your productivity system:**

```
Day 1: Skills
  1. Review framework skills in skills/
  2. Create a custom skill for your project patterns
  3. Test using the skill on a small task

Day 2: Commands
  1. Run each /ai-dev-advisor:* command
  2. Create one custom command for your workflow
  3. Use commands for a full work day

Day 3: Hooks
  1. Create pre-commit hook
  2. Try to commit without tests (should block)
  3. Add tests, commit should succeed

Day 4: Integration
  1. Full workflow: command → skill → hook
  2. Measure: how much faster?
```
