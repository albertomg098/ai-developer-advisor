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
- `/ai-dev-advisor:research` — research-first workflow for unfamiliar technology
- `/ai-dev-advisor:review-evidence` — check tests, coverage, and readiness
- `/ai-dev-advisor:switch-context` — checkpoint current work, switch tasks

### Context-Driven Workflow Rules

#### RULE 1: Auto-detect active context
At the START of every session, check the current git branch. If it matches a context file's `Feature Branch` field in `contexts/active/*.md`, automatically read that context file and announce:
> "Loaded context: [context name]. Current phase: [phase]. Last session: [date from log]."

If no branch matches, list available contexts and ask which one to work on.

#### RULE 2: Auto-update context files (MANDATORY)
After EVERY significant action, update the relevant context file in `contexts/active/`:
- **Check/uncheck Phase checkboxes** as tasks are completed (`- [x]`)
- **Append to Session Log** with dated entry summarizing what was done
- **Update Diagnosis** if the phase changes (e.g., 🔍→📋→🏗️)
- **Record decisions** under the chosen approach when a decision is made

This is NOT optional. The context file is the living record. Update it automatically without being asked.

#### RULE 3: Research handoff from Chat
When the user pastes research findings (from Claude Chat or elsewhere), immediately:
1. Read the matching context file
2. Update Phase 1 checkboxes based on what was researched
3. Add findings to the Session Log with today's date
4. Summarize what Phase 1 items remain (if any)
5. Ask: "Ready to move to Phase 2: PLAN, or more research needed?"

> 💡 **Tip:** Use `/ai-dev-advisor:research` to get a structured research prompt for Claude Chat before starting.

#### RULE 4: Plan mode activation
When the user says "plan" or "Phase 2" or "let's design the approach":
1. Read the context file
2. Enter plan mode (EnterPlanMode)
3. Explore all Key Files listed in the context
4. Design approach considering the research findings in the Session Log
5. Write the plan to the context file under a new `## Approved Plan` section
6. On approval: update context Phase 2 checkboxes, log the decision

#### RULE 5: Implementation with test-first
When the user says "implement" or "Phase 3" or approves the plan:
1. Read the context file (get the approved plan)
2. Write failing tests FIRST (use `/ai-dev-advisor:test-first`)
3. Run tests — confirm they fail (red)
4. Implement — make tests pass (green)
5. After EACH test passing: update context checkboxes and session log
6. Run full test suite at the end

#### RULE 6: Session end
When the user says "stop", "pause", "done for now", or ends the session:
1. Update context file with final status
2. Add Session Log entry: what was done, what's next
3. Show a summary of progress
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
