Start a development work session. Review all active contexts and recommend what to focus on.

Read the skill at `skills/context-management/SKILL.md` for the daily workflow pattern.

Follow this process:

1. **List active contexts**: Read all files in `contexts/active/` (if the directory exists). For each, extract:
   - File name
   - Status (🟢/🟡/🔴/✅)
   - Outcome goal
   - Last updated date
   - Next session steps

2. **If no contexts/ directory exists**, tell the user and offer to create the structure:
   ```
   contexts/active/
   contexts/backlog/
   contexts/archive/
   investigations/
   ```

3. **Prioritize and recommend**:
   - 🔴 URGENT items first
   - 🟡 BLOCKED items (check if unblockable now)
   - 🟢 ACTIVE items (recommend most impactful)

4. **Show session checklist**:
   ```
   □ Know which context I'm working on
   □ Know my ONE outcome for this session
   □ Know what test proves success
   □ Have the context file open
   ```

5. **Offer to start**: Ask which context to work on, then load it and show the "Next Session" steps.

If the user specifies a context: $ARGUMENTS — load that context directly.
