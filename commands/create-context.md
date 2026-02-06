Create a new context file for tracking a development task.

Read the template at `skills/context-management/templates/context-file.md` for the standard structure.
Read the skill at `skills/context-management/SKILL.md` for naming conventions.

Follow this process:

1. **Determine context type** from the user's description:
   - `explore_` — investigation, unknown cause
   - `hotfix_` — critical bug fix
   - `feature_` — new functionality
   - `improve_` — refactoring, optimization

2. **Ask for details** (if not provided):
   - Short descriptive name (for filename)
   - Outcome goal (one sentence: what does "done" look like?)
   - Success criteria (2–4 testable items)
   - Priority/urgency level

3. **Decide placement — active or backlog:**
   - If priority is urgent/high, or the user says "start now" → `contexts/active/`
   - If priority is low, or the user says "later" / "not yet" / "someday" → `contexts/backlog/`
   - If unclear, ask: "Start working on this now, or add to backlog for later?"
   - Hotfixes always go to `contexts/active/` (they're urgent by definition)

4. **Create the context file** at `contexts/[active|backlog]/[type]_[name].md` using the template:
   - Fill in Status, Started date, Outcome Goal, Success Criteria
   - Leave Session Log empty for first entry
   - Set up Evidence/Tests section with placeholder

5. **Create supporting structure** if needed:
   - `mkdir -p contexts/active contexts/backlog contexts/archive investigations`
   - For features: suggest `git checkout -b feature/[name]`
   - For hotfixes: suggest `git checkout -b hotfix/[name]`
   - For explorations: suggest creating `investigations/YYYYMMDD_[name].md`

6. **Show next steps**:
   - If created in **active**: What the user should do first based on the context type
   - If created in **backlog**: "Added to backlog. Run `/ai-dev-advisor:start-session` when ready to promote it."

User's description: $ARGUMENTS
