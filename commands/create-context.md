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

3. **Create the context file** at `contexts/active/[type]_[name].md` using the template:
   - Fill in Status, Started date, Outcome Goal, Success Criteria
   - Leave Session Log empty for first entry
   - Set up Evidence/Tests section with placeholder

4. **Create supporting structure** if needed:
   - `mkdir -p contexts/active contexts/backlog contexts/archive investigations`
   - For features: suggest `git checkout -b feature/[name]`
   - For hotfixes: suggest `git checkout -b hotfix/[name]`
   - For explorations: suggest creating `investigations/YYYYMMDD_[name].md`

5. **Show next steps**: What the user should do first based on the context type.

User's description: $ARGUMENTS
