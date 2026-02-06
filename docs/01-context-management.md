# Context Management - Working on Multiple Things Simultaneously
## How to juggle 3 investigations + 2 firefights + 1 feature without losing your mind

---

## 🎯 The Core Problem

**Old mental model:** "I should focus on ONE thing until done"
**Reality:** You have 3 investigations running, 2 critical bugs, 1 feature in progress
**New mental model:** "I work on ONE context per session, but maintain ALL active contexts"

---

## 📁 The Context File System

### Directory Structure
```
your-project/
├── src/              (your actual code)
├── tests/            (your actual tests)
├── contexts/         ← THE MAGIC HAPPENS HERE
│   ├── active/       ← Currently working on (2-5 files typically)
│   ├── backlog/      ← Planned but not started
│   └── archive/      ← Completed or abandoned
└── docs/             (architecture, specs, etc.)
```

### Context File Naming Convention
```
contexts/active/
├── explore_voice_mp3_format.md       (investigation #1)
├── explore_auth_token_expiry.md      (investigation #2)
├── explore_db_connection_leak.md     (investigation #3)
├── hotfix_prod_500_errors.md         (critical bug #1)
├── hotfix_email_not_sending.md       (critical bug #2)
└── feature_bulk_user_export.md       (feature work)
```

**Pattern:** `[context-type]_[short-description].md`
- `explore_*` - Investigations
- `hotfix_*` - Critical bugs
- `feature_*` - New features
- `improve_*` - Refactoring/optimization

---

## 📝 Universal Context File Template

Every context file follows the same structure (copy-paste this):

```markdown
# [Context Type]: [Short Title]

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
- [ ] [Another criterion]
- [ ] [Tests pass / Bug fixed / Feature works]

---

## 📊 Current State

### What Works
- [List anything confirmed working]

### What's Broken/Unknown
- [List problems or unknowns]

### What's In Progress
- [What you're actively working on right now]

---

## 📝 Session Log

### Session YYYY-MM-DD [Morning/Afternoon/Evening]
**Goal:** [What you wanted to accomplish]
**Actions Taken:**
- [What you actually did]
- [Commands run, files changed]

**Discoveries:**
- [What you learned]
- [Hypotheses confirmed or rejected]

**Blockers:**
- [What's stopping progress, if anything]

**Next Session:**
- [Exactly where to pick up]
- [First action to take]

---

## 🧪 Evidence/Tests
[Link to test files, test results, logs, screenshots, etc.]
- Test file: `tests/test_[something].py`
- Status: ❌ Failing / ✅ Passing / ⏳ Not written yet

---

## 🔗 Related Contexts
- Depends on: [other context files that must complete first]
- Blocks: [other contexts waiting on this]
- Related: [similar work]

---

## 📚 References
- Documentation: [links]
- Similar issues: [links]
- Stack Overflow: [links]
```

---

## 🔄 Daily Context Management Workflow

### Morning Routine (10 minutes)

```bash
# 1. List all active contexts
ls -lh contexts/active/

# 2. Read status of each
for f in contexts/active/*.md; do
  echo "=== $f ==="
  head -n 10 "$f"  # Shows status and goal
  echo ""
done

# 3. Identify:
# - Most URGENT (Status: 🔴)
# - Most BLOCKED (Status: 🟡)  
# - Most READY (Status: 🟢, clear next action)

# 4. Choose ONE to work on today based on:
# Priority: URGENT > BLOCKED > READY
# Energy: Hard problems in morning, easy in evening
```

### During Work Sessions

```bash
# Starting a session
claude "Read contexts/active/[chosen-context].md 
Focus on the 'Next Session' section.
What should we do first?"

# Mid-session (every 30-45 min)
claude "Append to contexts/active/[context].md session log:
Progress: [what we just accomplished]
Status: [current state]"

# Ending a session
claude "Update contexts/active/[context].md:
- Update 'Current State' section
- Update 'Success Criteria' checkboxes
- Add full session log entry
- Write 'Next Session' steps"
```

### End of Day (5 minutes)

```bash
# Commit all context updates
git add contexts/
git commit -m "Context updates: $(date +%Y-%m-%d)"

# Quick status check
for f in contexts/active/*.md; do
  grep "Status:" "$f"
done

# Archive completed contexts
for f in contexts/active/*.md; do
  if grep -q "Status: ✅ Done" "$f"; then
    mv "$f" contexts/archive/$(date +%Y%m%d)_$(basename "$f")
  fi
done
```

---

## 🎪 Managing Multiple Investigations Simultaneously

### Scenario: You have 3 active investigations

**Problem:** Each investigation discovers new things daily. How to not lose track?

**Solution:** Investigation Log Pattern

```markdown
# EXPLORATION: Voice MP3 Format Issues

Status: 🟢 Active (Day 3 of investigation)
Started: 2025-02-03
Last Updated: 2025-02-05 14:30

---

## 🎯 Mystery to Solve
Why do MP3 files >10MB fail voice detection, but WAV files of any size work?

## ✅ Investigation Milestones
- [ ] Isolate: Is it file size or format?
- [ ] Isolate: Is it encoding or duration?
- [ ] Root cause identified
- [ ] Fix verified

---

## 🔬 Hypotheses Board

### ✅ TESTED - Confirmed
1. **WAV files work regardless of size** (tested up to 50MB)
   - Evidence: tests/debug/test_wav_sizes.py all pass

### ✅ TESTED - Rejected
1. ~~It's a file size limit issue~~ 
   - Evidence: 5MB MP3 also fails, so not size-related
2. ~~It's a sampling rate issue~~
   - Evidence: Tested 44.1kHz and 48kHz, both fail

### ⏳ PENDING TEST
1. **MP3 decoder library missing or broken**
   - Test: Try decoding with pydub vs librosa vs audioread
   - File: tests/debug/test_mp3_decoders.py
   - Created: Not yet
   
2. **MP3 metadata confusing the processor**
   - Test: Strip all metadata, retry
   - File: tests/debug/test_mp3_stripped.py
   - Created: Not yet

---

## 📝 Daily Investigation Log

### Day 3 - Feb 5 Morning (1.5 hrs)
**Focus:** Test Hypothesis #3 (decoder library)
**Actions:**
- Created tests/debug/test_mp3_decoders.py
- Tested pydub: ❌ Fails with "ffmpeg not found"
- Tested librosa: ✅ Successfully decodes MP3
- Tested audioread: ✅ Works but slower

**Key Discovery:** 
Our code uses pydub, but ffmpeg isn't installed in production!
This is likely the root cause.

**Next Step:** 
1. Verify ffmpeg missing in production env
2. If confirmed, this is the fix
3. Move from EXPLORATION to FIREFIGHTING mode

---

## 🧪 Test Artifacts
- Minimal reproduction: tests/debug/mp3_minimal_repro.py
- Test data: tests/fixtures/audio/sample_11mb.mp3
- Logs: logs/voice_processing_2025-02-05.log

---

## 🔗 Related Contexts
- Blocks: feature_voice_multilingual.md (needs MP3 working)
```

### Key Pattern: Investigation Context Updates

**Don't rewrite the whole file each session.**  
**Just append to the log and update the hypotheses board.**

```bash
# Quick update pattern
claude "For contexts/active/explore_voice_mp3.md:
1. Add new session entry under Day 3
2. Move Hypothesis #1 from PENDING to TESTED-Confirmed
3. Update 'Next Step' section"

# This takes 30 seconds vs. rewriting everything
```

---

## 🚨 Managing Multiple Firefights Simultaneously

### Scenario: Production has 2 critical bugs

**Problem:** Both are urgent, but you can't work on both at once.

**Solution:** Triage Protocol

```bash
# 1. Create hotfix contexts for BOTH
claude "Create contexts/active/hotfix_prod_500_errors.md
and contexts/active/hotfix_email_not_sending.md
using the universal template"

# 2. Assess severity
# - hotfix_prod_500_errors: Affecting 100% of users
# - hotfix_email_not_sending: Affecting 10% of users

# 3. Work on MOST SEVERE first
git checkout -b hotfix/prod-500-errors

# 4. Log status of BOTH
claude "Update contexts/active/hotfix_email_not_sending.md:
Status: 🟡 Blocked - Waiting for 500 errors to be fixed first
Reason: Both issues may be related, fixing 500s first"

# 5. Document triage decision
echo "## Triage Decision
Working on 500 errors first because it affects all users.
Email bug will be addressed after 500s are resolved." \
>> contexts/active/hotfix_prod_500_errors.md
```

### Firefight Context - Key Differences

**Firefight contexts are SHORTER and MORE URGENT:**

```markdown
# HOTFIX: Production 500 Errors

Status: 🔴 CRITICAL - Production down
Started: 2025-02-05 10:15
Branch: hotfix/prod-500-errors

---

## 🎯 Outcome Goal
Stop 500 errors in production API within 2 hours

## ✅ Success Criteria
- [ ] Error rate <1% (currently 40%)
- [ ] Root cause identified
- [ ] Fix deployed
- [ ] Monitoring confirms fix

---

## 🚨 Impact
- Users affected: ~10,000 (all API users)
- Services down: /api/users/*, /api/orgs/*
- First detected: 2025-02-05 10:00
- Customer tickets: 15 (and counting)

---

## 🔍 Quick Facts
- Error message: "AttributeError: 'NoneType' object has no attribute 'get'"
- File: src/api/auth.py line 45
- Started after: Deployment at 09:45
- Last known good: Deployment at 08:30

---

## ⚡ Timeline (updates every 15-30 min)

**10:15** - Created hotfix branch
**10:20** - Rolled back deployment (error persists - not deployment!)
**10:30** - Identified: Database connection pool exhausted
**10:45** - Hypothesis: Recent traffic spike + no connection timeout
**11:00** - Fix: Added connection pool size limit + timeout
**11:15** - Deployed fix to production
**11:30** - ✅ Error rate dropped to <1%

---

## 🔧 The Fix
- File: src/database.py
- Change: `pool_size=100, max_overflow=50, pool_timeout=30`
- Test: tests/hotfix/test_db_connection_pool.py
- Status: ✅ Verified working

---

## 📚 Post-Mortem (after fixed)
- Root cause: [fill in after fixed]
- Prevention: [what would prevent this]
- Monitoring: [what alerts to add]
```

**Notice:** Much more real-time updates, timeline instead of session log.

---

## 🏗️ Managing Feature Work Alongside Firefights

### Scenario: You're building a feature, then a bug appears

**The Context Switch Protocol:**

```bash
# You're working on feature_bulk_export.md
# Suddenly: "Email notifications stopped working!"

# DON'T: Abandon feature work without documenting
# DO: Controlled context switch

# Step 1: Checkpoint current work
git add .
git commit -m "WIP: Bulk export - pausing for critical bug"

claude "Update contexts/active/feature_bulk_export.md:
Status: 🟡 Paused (critical bug discovered)
Paused at: [timestamp]
State: [describe exact state - what's half-done]
Resume: [exact next steps when returning]"

# Step 2: Create hotfix context
claude "Create contexts/active/hotfix_email_not_sending.md
Note in 'Related Contexts': Paused feature_bulk_export.md for this"

# Step 3: Switch branches
git checkout -b hotfix/email-not-sending

# Step 4: Work on hotfix (focus completely on this)
# ... fix the bug ...

# Step 5: Complete hotfix
git checkout main
git merge hotfix/email-not-sending

claude "Update contexts/active/hotfix_email_not_sending.md:
Status: ✅ Done
Completed: [timestamp]"

mv contexts/active/hotfix_email_not_sending.md \
   contexts/archive/20250205_email_fixed.md

# Step 6: Return to feature
git checkout feature/bulk-export

claude "Update contexts/active/feature_bulk_export.md:
Status: 🟢 Active (resumed after hotfix)
Resumed at: [timestamp]
Read 'Resume' section and continue from there"
```

---

## 📊 Context Dashboard (Your Daily View)

Create a simple script to see all contexts at a glance:

```bash
# contexts/dashboard.sh
#!/bin/bash

echo "========================================="
echo "  ACTIVE CONTEXTS - $(date)"
echo "========================================="
echo ""

for f in contexts/active/*.md; do
  if [ -f "$f" ]; then
    echo "📄 $(basename $f)"
    grep "Status:" "$f" | head -1
    grep "Outcome Goal" -A 1 "$f" | tail -1
    echo ""
  fi
done

echo "========================================="
echo "Backlog: $(ls contexts/backlog/*.md 2>/dev/null | wc -l) items"
echo "Archive: $(ls contexts/archive/*.md 2>/dev/null | wc -l) completed"
echo "========================================="
```

**Run every morning:**
```bash
bash contexts/dashboard.sh
```

**Output:**
```
=========================================
  ACTIVE CONTEXTS - 2025-02-05
=========================================

📄 explore_voice_mp3_format.md
Status: 🟢 Active (Day 3)
Solve MP3 >10MB voice detection failure

📄 hotfix_prod_500_errors.md
Status: 🔴 CRITICAL - Production down
Stop 500 errors within 2 hours

📄 feature_bulk_export.md
Status: 🟡 Paused (critical bug)
Complete bulk user export feature

=========================================
Backlog: 3 items
Archive: 12 completed
=========================================
```

---

## 🎯 Rules for Context Hygiene

### Rule 1: One Context = One Outcome
**Bad:** "Fix everything broken with voice processing"
**Good:** "Fix MP3 files >10MB failing voice detection"

### Rule 2: Update Context Files, Don't Remake Them
**Bad:** Rewrite entire context file each session
**Good:** Append to session log, update checkboxes

### Rule 3: Archive Aggressively
**When context is ✅ Done or abandoned:**
```bash
mv contexts/active/[name].md \
   contexts/archive/$(date +%Y%m%d)_[name].md
```

### Rule 4: Max 5-7 Active Contexts
**If you have >7 active contexts:**
- Some should be in backlog (not started yet)
- Some should be archived (actually done)
- Some should be merged (too similar)

### Rule 5: Every Context Has A Branch (If Applicable)
**Investigations:** Usually no branch (just exploring)
**Hotfixes:** Always branch `hotfix/[name]`
**Features:** Always branch `feature/[name]`
**Improvements:** Usually branch `improve/[name]`

---

## 🔄 Context Lifecycle Diagram

```
NEW PROBLEM/IDEA
    ↓
Does it need immediate work?
    ↓
NO → contexts/backlog/[name].md → Someday
    ↓
YES → contexts/active/[type]_[name].md
    ↓
Work on it across multiple sessions
    ↓
Update after EVERY session
    ↓
DONE → mv to contexts/archive/[date]_[name].md
BLOCKED → Status: 🟡, work on something else
ABANDONED → mv to archive/ with reason
```

---

## 💡 Pro Tips

### Tip 1: Use grep for quick context searches
```bash
# Find all blocked contexts
grep -l "Status: 🟡" contexts/active/*.md

# Find contexts waiting on something
grep -l "Blocks:" contexts/active/*.md

# Find contexts you worked on today
grep "$(date +%Y-%m-%d)" contexts/active/*.md
```

### Tip 2: Context templates in Chat
```bash
# Instead of copy-pasting template:
claude "Create contexts/active/explore_auth_timeout.md
using the universal context template for explorations.
Fill in:
- Outcome: Understand why auth tokens expire early
- Status: Active
- Started: today"
```

### Tip 3: Daily standup with yourself
```bash
# End of each day:
claude "Read all contexts/active/*.md files.
Generate a standup summary:
- What I completed today
- What I'm working on tomorrow  
- What blockers exist

Format as bullet points."
```

---

## 🎓 Practice Exercise

**Exercise: Manage 3 contexts simultaneously**

1. Create 3 contexts:
   ```bash
   contexts/active/explore_database_slow.md
   contexts/active/hotfix_login_broken.md
   contexts/active/feature_csv_export.md
   ```

2. Work on login hotfix (30 min)
   - Create branch
   - Fix bug
   - Update context
   - Merge and archive

3. Switch to feature work (30 min)
   - Read context
   - Make progress
   - Update context
   - Commit WIP

4. Start investigation (20 min)
   - Create investigation plan
   - Test first hypothesis
   - Update context
   - Leave for tomorrow

5. Next day: Resume where you left off
   - Read all 3 contexts
   - Choose what to work on
   - Continue seamlessly

---

**Next:** Read `02_CHAT_CODE_PLAN_GUIDE.md` to learn when to use which tool for each context type.
