# Session Checklist

## Before Starting (2 min)

```
□ Check active contexts: ls contexts/active/
□ Read the context file for today's focus
□ Know which CONTEXT I'm in (🔍 🔥 🏗️ ✨)
□ Know my ONE outcome for this session
□ Know what TEST will prove success
```

---

## During Session

### Every 30–45 Minutes
```
□ Append progress to context file session log
□ Run relevant tests
□ Check: Am I still on track for my ONE outcome?
□ Check: Am I stuck? (If >20 min no progress → switch to Chat)
```

### If Switching Contexts Mid-Session
```
□ git add . && git commit -m "WIP: [context] — pausing for [reason]"
□ Update current context: Status → 🟡 PAUSED, write Resume Point
□ Open new context file, read Next Session section
□ Switch git branch if needed
```

---

## After Session (3 min)

```
□ Update context file:
  □ Current State section
  □ Success Criteria checkboxes
  □ Full session log entry
  □ "Next Session" steps (be specific!)
□ Run tests: pytest tests/ -v
  □ All passing? → Ready to commit
  □ Failures? → Document in context file
□ Commit: git add . && git commit -m "[type]([scope]): [description]"
□ Commit contexts: git add contexts/ && git commit -m "context updates"
```

---

## End of Day (5 min)

```
□ Quick status check across ALL active contexts
□ Archive completed contexts (Status: ✅ Done):
  mv contexts/active/[name].md contexts/archive/$(date +%Y%m%d)_[name].md
□ Identify tomorrow's focus
□ Note any blockers that need external resolution
```

---

## Emergency Protocols

### "I'm completely stuck" (>30 min no progress)
1. STOP coding
2. Switch to Chat: describe problem + what you've tried
3. Get 3 alternative approaches
4. Return to Code with new strategy

### "Found a critical bug mid-session"
1. `git stash` (save current work)
2. Create `contexts/active/hotfix_[bug].md`
3. `git checkout -b hotfix/[bug]`
4. Fix with minimal scope
5. Merge, then `git checkout [previous-branch] && git stash pop`
6. Update both context files

### "Nothing works and I don't know why"
1. Create `contexts/active/explore_[mystery].md`
2. Switch to EXPLORATION mode
3. Build minimal reproduction
4. Binary search the problem
5. Document findings even if unsolved
