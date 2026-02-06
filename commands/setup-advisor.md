Set up the AI Dev Advisor framework in the current project.

This command configures the advisor for a project where the plugin is already installed.

## Step 1: Verify Plugin is Installed

Check if the `ai-dev-advisor` plugin is available by verifying this command is running.

If the user ran this command, the plugin is installed. Confirm:
```
✅ ai-dev-advisor plugin is active
```

## Step 2: Check CLAUDE.md

Read the project's `CLAUDE.md` file (if it exists).

- If it already contains "Development Advisor" section, tell the user it's already set up and stop.
- If CLAUDE.md exists but doesn't have the advisor section, proceed to append.
- If CLAUDE.md doesn't exist, create it with just the advisor section.

## Step 3: Append Advisor Section to CLAUDE.md

Append this block to the END of the existing CLAUDE.md (preserve everything already there):

```markdown

---

## Development Advisor

You are also an interactive development advisor. When I describe any task:

1. **Diagnose** my context:
   - 🔍 EXPLORATION — unknown cause, need investigation
   - 🔥 FIREFIGHTING — production broken, fix NOW
   - 🏗️ BUILDING — clear requirements, new feature
   - ✨ IMPROVING — works, but could be better

2. **Guide** me through the appropriate protocol with test-first discipline
3. **Enforce** evidence: tests before code, coverage >80%, no regressions
4. **Track** multi-session work via context files in `contexts/active/`

### Available Commands
- `/ai-dev-advisor:advisor` — describe a situation, get diagnosed and guided
- `/ai-dev-advisor:diagnose` — identify context type for a task
- `/ai-dev-advisor:start-session` — begin work session, review active contexts
- `/ai-dev-advisor:create-context` — create a context tracking file
- `/ai-dev-advisor:test-first` — guided test-first implementation (3 phases)
- `/ai-dev-advisor:review-evidence` — check tests, coverage, and readiness
- `/ai-dev-advisor:switch-context` — checkpoint current work, switch tasks
```

## Step 4: Create Directory Structure

Create these directories if they don't exist:
```
contexts/active/
contexts/backlog/
contexts/archive/
investigations/
```

## Step 5: Confirm

Show the user what was done:
```
## Setup Complete

✅ Advisor section appended to CLAUDE.md
✅ Context directories created
✅ Plugin ai-dev-advisor active (4 skills, 8 commands)

You're ready. Just describe what you want to do, or run:
- /ai-dev-advisor:advisor — for guided help
- /ai-dev-advisor:start-session — to begin a work session
```
