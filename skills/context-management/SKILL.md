---
name: context-management
description: >
  Managing multiple simultaneous development contexts. Covers context file system,
  naming conventions, state transitions, and switching protocols. Use when juggling
  multiple tasks, switching between work items, or setting up context tracking.
---

# Context Management — Multiple Simultaneous Workflows

## 🎯 Core Mental Model

**"Work on ONE context per session, but maintain ALL active contexts."**

You'll have 2–5 active contexts at once. That's normal. The system keeps them organized so nothing gets lost.

---

## 📁 The Context File System

### Directory Structure
```
your-project/
├── contexts/
│   ├── active/       ← Currently working on (2–5 files)
│   ├── backlog/      ← Planned but not started
│   └── archive/      ← Completed or abandoned
└── investigations/   ← Exploration-specific logs
```

### Naming Convention
```
contexts/active/
├── explore_[problem].md       ← Investigations
├── hotfix_[bug].md            ← Critical bugs
├── feature_[name].md          ← New features
└── improve_[what].md          ← Refactoring/optimization
```

**Pattern:** `[context-type]_[short-description].md`

---

## 📝 Context File Template

Every context file follows this structure:

```markdown
# [CONTEXT_TYPE]: [Short Title]

**Status:** 🟢 Active / 🟡 Blocked / 🔴 Urgent / ✅ Done
**Started:** YYYY-MM-DD
**Last Updated:** YYYY-MM-DD HH:MM
**Branch:** [git branch name if applicable]
**Time Invested:** [rough estimate]

---

## 🎯 Outcome Goal
[One sentence: What does "done" look like?]

## ✅ Success Criteria
- [ ] [Specific, testable criterion]
- [ ] [Tests pass / Bug fixed / Feature works]

---

## 📊 Current State

### What Works
- [Confirmed working items]

### What's Broken/Unknown
- [Problems or unknowns]

### What's In Progress
- [Current active work]

---

## 📝 Session Log

### Session YYYY-MM-DD
**Goal:** [What you wanted to accomplish]
**Actions:** [What you did]
**Discoveries:** [What you learned]
**Blockers:** [What's stopping progress]
**Next Session:** [Exactly where to pick up]

---

## 🧪 Evidence/Tests
- Test file: `tests/test_[x].py` — Status: ❌/✅/⏳
- Coverage: [X]% (target: 80%)

## 🔗 Related Contexts
- Depends on: [contexts that must complete first]
- Blocks: [contexts waiting on this]
```

Full template available at: `skills/context-management/templates/context-file.md`

---

## 🔄 Daily Workflow

### Morning Routine (10 min)
1. **List** active contexts: `ls contexts/active/`
2. **Read** status of each (first 10 lines)
3. **Identify** most urgent (🔴), most blocked (🟡), most ready (🟢)
4. **Choose ONE** to work on today

**Priority order:** 🔴 URGENT → 🟡 BLOCKED (if unblockable) → 🟢 READY

### During Sessions
- **Starting:** read context file, focus on "Next Session" section
- **Mid-session** (every 30–45 min): append progress to session log
- **Ending:** update Current State, check Success Criteria, write Next Session

### End of Day (5 min)
1. Commit context updates: `git add contexts/ && git commit -m "context updates"`
2. Quick status check across all active contexts
3. Archive any completed contexts

---

## 🚨 Context Switching Protocol

When you need to switch between contexts mid-session:

### === PAUSE CURRENT CONTEXT ===

```bash
# Step 1: Checkpoint work
git add .
git commit -m "WIP: [context] — pausing for [reason]"

# Step 2: Update context file
# Set Status: 🟡 PAUSED
# Add: Paused at [timestamp]
# Add: State: [what's half-done]
# Add: Resume point: [exact next steps]
```

### === SWITCH TO NEW CONTEXT ===

```bash
# Step 3: Create/open new context file
# Step 4: Switch branch if needed
git checkout -b [type]/[name]

# Step 5: Work on new context (follow its protocol)
```

### === RESUME PREVIOUS CONTEXT ===

```bash
# Step 6: Return to branch
git checkout [previous-branch]

# Step 7: Update context
# Set Status: 🟢 Active (resumed)
# Read "Resume point" and continue
```

---

## 🎪 Managing Multiple Investigations

For concurrent investigations, use the **Hypotheses Board** pattern:

```markdown
## 🔬 Hypotheses Board

### ✅ TESTED — Confirmed
1. [Hypothesis] — Evidence: [test file]

### ❌ TESTED — Rejected
1. ~~[Hypothesis]~~ — Evidence: [why rejected]

### ⏳ PENDING TEST
1. [Hypothesis]
   - Test: [what to try]
   - File: tests/debug/test_[x].py
```

- One hypothesis at a time
- Append results, don't rewrite
- Investigation files go in `investigations/YYYYMMDD_[issue].md`

---

## 🚨 Managing Multiple Firefights

When multiple critical bugs exist simultaneously:

1. **Create hotfix contexts for ALL**
2. **Triage by severity:**
   - How many users affected?
   - What's the business impact?
   - Can it wait 2 hours?
3. **Work on MOST SEVERE first**
4. **Set others to** 🟡 Blocked — waiting for [X] to be fixed

---

## 🎯 Rules for Context Hygiene

### Rule 1: One Context = One Outcome
**Bad:** "Fix everything broken with voice processing"
**Good:** "Fix MP3 files >10MB failing voice detection"

### Rule 2: Update Context Files, Don't Remake Them
Append to session log, update checkboxes. Don't rewrite the whole file.

### Rule 3: Archive Aggressively
When ✅ Done or abandoned:
```bash
mv contexts/active/[name].md contexts/archive/$(date +%Y%m%d)_[name].md
```

### Rule 4: Max 5–7 Active Contexts
If >7: move some to backlog, archive completed ones, merge similar ones.

### Rule 5: Every Context Has a Branch (If Applicable)
- Explorations: usually no branch
- Hotfixes: always `hotfix/[name]`
- Features: always `feature/[name]`
- Improvements: usually `improve/[name]`

---

## 🔄 Context Lifecycle

```
NEW PROBLEM/IDEA
    ↓
Needs immediate work?
    NO  → contexts/backlog/[name].md
    YES → contexts/active/[type]_[name].md
            ↓
        Work across sessions
            ↓
        Update after EVERY session
            ↓
        DONE     → contexts/archive/[date]_[name].md
        BLOCKED  → Status: 🟡, work on something else
        ABANDONED → archive with reason
```

---

## 📊 Context Dashboard

Quick visibility script:

```bash
#!/bin/bash
echo "========================================="
echo "  ACTIVE CONTEXTS — $(date +%Y-%m-%d)"
echo "========================================="
for f in contexts/active/*.md; do
  [ -f "$f" ] || continue
  echo "📄 $(basename "$f")"
  grep "Status:" "$f" | head -1
  grep "Outcome Goal" -A 1 "$f" | tail -1
  echo ""
done
echo "Backlog: $(ls contexts/backlog/*.md 2>/dev/null | wc -l) items"
echo "Archive: $(ls contexts/archive/*.md 2>/dev/null | wc -l) completed"
```

---

## 💡 Quick Tips

- **Find blocked contexts:** `grep -l "Status: 🟡" contexts/active/*.md`
- **Find today's work:** `grep "$(date +%Y-%m-%d)" contexts/active/*.md`
- **Daily standup with yourself:** read all contexts, generate 3-bullet summary
- **Context templates via command:** use `/ai-dev-advisor:create-context`
