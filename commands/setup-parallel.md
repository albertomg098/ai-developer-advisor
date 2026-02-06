Set up parallel worktree sessions for multiple development tasks.

Read the skill at `skills/context-management/SKILL.md` for the worktree methodology.

Follow this process:

## Step 1: Gather Tasks

1. **If $ARGUMENTS provided**: parse the comma-separated task descriptions
2. **If no arguments**: ask the user to list 2–5 tasks they want to work on in parallel
3. **For each task**: determine context type (explore/hotfix/feature/improve) and a short kebab-case name

Example input: "Fix auth bug, Add CSV export, Investigate slow emails"
→ `hotfix_auth-bug`, `feature_csv-export`, `explore_slow-emails`

## Step 2: Verify Git State

1. Run `git status` — working directory must be clean (no uncommitted changes)
   - If dirty: ask the user to commit or stash first, then re-run
2. Run `git worktree list` — show existing worktrees
   - If any conflict with planned names: warn and ask how to proceed
3. Identify the current branch as the base branch (usually `main` or `master`)

## Step 3: Create Context Files

For each task:

1. Create `contexts/active/[type]_[name].md` using the context file template
2. Set the **Branch** field to `[type]/[name]` (e.g., `hotfix/auth-bug`)
3. Set **Status** to 🟢 Active

After all context files are created:
```bash
git add contexts/active/ && git commit -m "contexts: parallel setup for N tasks"
```

## Step 4: Create Worktrees

Determine the project directory name from `basename $(pwd)`.

For each task:
```bash
git branch [type]/[name]
git worktree add ../[project]-[type]-[name]/ [type]/[name]
```

Naming convention: `../[project-name]-[type]-[name]/`
Example: `../myapp-hotfix-auth-bug/`, `../myapp-feature-csv-export/`

## Step 5: Show Instructions

Display a summary table and launch instructions:

```
## ✅ Parallel Sessions Ready

| Task | Branch | Directory | Command |
|------|--------|-----------|---------|
| Fix auth bug | hotfix/auth-bug | ../myapp-hotfix-auth-bug/ | cd ../myapp-hotfix-auth-bug/ && claude |
| Add CSV export | feature/csv-export | ../myapp-feature-csv-export/ | cd ../myapp-feature-csv-export/ && claude |
| Slow emails | explore/slow-emails | ../myapp-explore-slow-emails/ | cd ../myapp-explore-slow-emails/ && claude |

### How to Start
Open a NEW terminal for each task and run the command above.
Each session will auto-detect its branch and load the matching context file.

### When a Task is Done
1. Merge the branch: `git checkout main && git merge [branch]`
2. Remove the worktree: `git worktree remove ../[directory]/`
3. Archive the context: `mv contexts/active/[file] contexts/archive/$(date +%Y%m%d)_[file]`
```

Tasks to set up: $ARGUMENTS
