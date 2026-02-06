Guide the user through research-first development when they need to learn unfamiliar technology before building.

Read the skill at `skills/tool-selection/SKILL.md` for the research-first pipeline methodology.
Read the template at `skills/tool-selection/templates/research-prompt.md` for the research prompt.

Follow the three-phase process with explicit pauses between phases:

## PHASE 1 — RESEARCH

1. **Clarify** what needs to be researched (from user description or $ARGUMENTS)
2. **Create context file** → `contexts/active/feature_[name].md` with Phase 1 = Research
3. **Build research prompt** from the template:
   - Fill in the `[TOPIC]`, `[PROJECT]`, `[CONSTRAINTS]` placeholders using the user's description
   - Choose the right variant (base, library evaluation, or pattern/architecture)
4. **Show the completed prompt** to the user, formatted for easy copy-paste
5. **Explain**: "Paste this into Claude Chat (chat.claude.ai). Chat has no file access, which keeps it focused on thinking and research rather than jumping to implementation."

**PAUSE**: Tell the user to go to Chat, run the research, and come back with their findings.

## PHASE 2 — PLAN

When the user returns with research findings:

1. **Read the context file** to recall the task
2. **Update context file** — add research findings to Session Log, check off Phase 1 items
3. **Explore the codebase** — read Key Files relevant to the implementation
4. **Design the approach** based on research findings + codebase reality:
   - What to build, where it fits in the existing code
   - Dependencies needed
   - Test strategy
   - Implementation steps in order
5. **Present the plan** for approval

**PAUSE**: Ask the user to review and approve the plan before proceeding.

## PHASE 3 — IMPLEMENT

On approval:

1. **Update context file** — check off Phase 2, record approved plan
2. **Hand off to test-first**: proceed with `/ai-dev-advisor:test-first` using the approved plan as the spec

Research topic: $ARGUMENTS
