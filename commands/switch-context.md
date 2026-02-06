Switch from the current development context to a different one.

Read the skill at `skills/context-management/SKILL.md` for the switching protocol.

Follow this process:

## Step 0: Check for Worktrees

1. Run `git worktree list`
2. If the target context has an active worktree:
   - Tell the user: "Context [name] has a worktree at `../[path]/`. Open a new terminal there instead of switching."
   - Stop — no switching needed.
3. If no worktree for the target, proceed with sequential switching below.

## Step 1: Checkpoint Current Work

1. **Check for uncommitted changes**: `git status`
2. **If changes exist**:
   - Stage and commit as WIP: `git add . && git commit -m "WIP: [context] — switching to [target]"`
   - Or stash: `git stash`
3. **Update current context file**:
   - Set Status to 🟡 PAUSED
   - Add: "Paused at: [timestamp]"
   - Add: "State: [what's in progress]"
   - Add: "Resume point: [exact next steps when returning]"

## Step 2: Identify Target Context

1. **If target specified** ($ARGUMENTS): look for it in `contexts/active/` first, then `contexts/backlog/`
2. **If not specified**: list all contexts from both active and backlog, grouped:
   ```
   🟢 Active contexts:
   - [filename] — [outcome goal] — [status]

   📋 Backlog contexts:
   - [filename] — [outcome goal]

   Switch to which one?
   ```
3. **If target is in backlog**: promote it first — move from `contexts/backlog/` → `contexts/active/`, set Status to 🟢 Active
4. **Read target context file** — show current state and next steps

## Step 3: Switch

1. **Switch git branch** if applicable:
   - `git checkout [target-branch]`
   - Or `git stash pop` if resuming stashed work
2. **Update target context file**:
   - Set Status to 🟢 Active (resumed)
   - Add: "Resumed at: [timestamp]"
3. **Show the "Next Session" section** from the target context

## Step 4: Confirm

Show summary:
```
## Context Switch Complete

**From:** [previous context] → Status: 🟡 PAUSED
**To:** [new context] → Status: 🟢 Active
**Branch:** [current branch]
**Next step:** [first action from context file]
```

Target context: $ARGUMENTS
