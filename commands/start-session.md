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

3b. **If 3+ active contexts exist**, offer parallel setup:
    "You have [N] active contexts. Would you like to:
    a) Pick ONE to focus on this session
    b) Set up parallel worktree sessions → `/ai-dev-advisor:setup-parallel`"

3c. **If >5 active contexts**, suggest demoting low-priority items:
    "You have [N] active contexts (recommended max: 5–7). Consider moving lower-priority items to backlog:"
    - List the least-recently-updated or lowest-priority active contexts
    - Offer to move them: `contexts/active/[file]` → `contexts/backlog/[file]`

4. **Show backlog summary**:
   Read `contexts/backlog/` (if it exists and has files). Show:
   ```
   📋 Backlog: X items
   - [filename] — [outcome goal from file]
   - [filename] — [outcome goal from file]
   ```
   If any backlog item is now higher priority than active items, suggest promoting it:
   "📌 [backlog item] may be ready to start. Promote to active?"

5. **Show session checklist**:
   ```
   □ Know which context I'm working on
   □ Know my ONE outcome for this session
   □ Know what test proves success
   □ Have the context file open
   ```

6. **Offer to start**: Ask which context to work on (from active or backlog), then:
   - If active: load it and show the "Next Session" steps
   - If backlog: move it to `contexts/active/`, set Status to 🟢 Active, then load it

If the user specifies a context: $ARGUMENTS — load that context directly.
