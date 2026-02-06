# Claude Code Mastery - Quick Start Guide
## Read This First Every Morning

---

## 🎯 The One Core Principle

**Evidence-Driven Development: Never ship code without proof it works.**

```
Code without tests = Fiction
Code with tests = Evidence
Code with tests + documentation = Truth
```

---

## 📋 Your Daily Ritual (5 Minutes)

### Every Morning:
```bash
# 1. Check active contexts
ls contexts/active/

# 2. Read each active context
cat contexts/active/*.md

# 3. Decide: What's my ONE focus today?
# Write it down: _______________________________

# 4. Open corresponding context file
# contexts/active/[that-focus].md
```

### Before ANY Claude Code Session:
```
□ I know which CONTEXT I'm in (check contexts/active/)
□ I know my ONE outcome for this session
□ I know what TEST will prove success
□ I have the context file open
```

### After EVERY Claude Code Session:
```
□ Updated the context file with progress
□ Ran tests (and they pass, or I know why they don't)
□ Committed OR documented why not
□ Wrote "next step" for tomorrow
```

---

## 🧭 The 4 Contexts - Quick Reference

### 🔍 EXPLORATION
**You feel:** "WTF? I don't even know what's broken"
**File:** `contexts/active/explore_[problem].md`
**Mode:** Investigation, hypothesis testing
**Success:** Root cause identified OR investigation documented for help

### 🔥 FIREFIGHTING  
**You feel:** "This MUST be fixed NOW"
**File:** `contexts/active/hotfix_[critical-bug].md`
**Branch:** `hotfix/[bug-name]`
**Mode:** Minimal fix, no scope creep
**Success:** Bug fixed, test proves it, merged back

### 🏗️ BUILDING
**You feel:** "I know what to build and how"
**File:** `contexts/active/feature_[name].md`
**Branch:** `feature/[name]`
**Mode:** Test-first development
**Success:** Feature complete, all tests pass, documented

### ✨ IMPROVING
**You feel:** "It works, but could be better"
**File:** `contexts/backlog/improve_[what].md`
**Mode:** Optional polish work
**Success:** Measurable improvement, no regressions

---

## 🎪 Managing Multiple Active Contexts

**Reality check:** You'll have 2-5 active contexts at once. That's NORMAL.

```
contexts/
├── active/                    ← Work in progress
│   ├── explore_voice_mp3.md   (started Mon, checking daily)
│   ├── feature_bulk_export.md (main focus today)
│   └── hotfix_auth_token.md   (just discovered, must fix)
├── backlog/                   ← Future work
│   ├── improve_performance.md
│   └── refactor_api_layer.md
└── archive/                   ← Completed/abandoned
    └── 2025-02-05_voice_wav_fixed.md
```

**The rule:** Work on ONE context per session, but maintain ALL active contexts.

---

## 🔄 Context State Transitions

```
NEW PROBLEM/FEATURE
    ↓
Create contexts/active/[type]_[name].md
    ↓
Work on it (multiple sessions)
    ↓
COMPLETE → mv to contexts/archive/
BLOCKED → update with blocker, work on different context
ABANDONED → mv to contexts/archive/ with reason
```

---

## 🎯 The Evidence Pyramid (Non-Negotiable)

```
           🚀 DEPLOYED TO PRODUCTION
          (monitoring shows it works)
                    ↑
          📊 E2E TESTS PASSING
         (whole system works together)
                    ↑
        🔗 INTEGRATION TESTS PASSING
       (components work together)
                    ↑
      ✅ UNIT TESTS PASSING
     (individual functions work)
                    ↑
    📝 TESTS EXIST (even if failing)
   (we know what "working" means)
                    ↑
  📋 REQUIREMENTS DOCUMENTED
 (we know what to build)
```

**NEVER skip a level. If integration tests fail, don't deploy—fix the tests.**

---

## 💬 Chat vs Code vs Plan - Decision Matrix

### Use CLAUDE CHAT when:
- [ ] Designing architecture
- [ ] Analyzing logs/errors/data
- [ ] Making strategic decisions
- [ ] Reviewing code quality
- [ ] Unsticking yourself ("I don't know what to do")
- [ ] Creating specifications

**Output:** Decisions, designs, analysis → Feed to Code

### Use CLAUDE CODE when:
- [ ] Implementing features (with clear spec)
- [ ] Creating/modifying files
- [ ] Running tests
- [ ] Debugging (with hypothesis)
- [ ] Scaffolding projects

**Output:** Working code, tests, evidence

### Use PLAN MODE when:
- [ ] Multi-step tasks with dependencies
- [ ] Complex refactoring across many files
- [ ] "I know what to do, just need execution"

**Workflow:**
```
CHAT (design) → CODE with PLAN (execute) → CHAT (review)
```

---

## 🚨 Emergency Protocols

### "I'm completely stuck" (>30 min no progress)
```bash
# 1. STOP coding
# 2. Switch to Chat:
"I'm stuck on [problem]. Here's what I've tried: [list].
Help me:
1. Identify what I'm missing
2. Generate 3 alternative approaches
3. Create a diagnostic checklist"

# 3. Return to Code with new strategy
```

### "I found a critical bug mid-session"
```bash
# 1. git stash (save current work)
# 2. Create contexts/active/hotfix_[bug].md
# 3. git checkout -b hotfix/[bug]
# 4. Fix with minimal scope
# 5. git checkout previous-branch
# 6. git stash pop
# 7. Update both context files
```

### "Nothing works and I don't know why"
```bash
# 1. Create contexts/active/explore_[mystery].md
# 2. Switch to EXPLORATION mode
# 3. Build minimal reproduction
# 4. Binary search the problem
# 5. Document findings even if unsolved
```

---

## 📚 Read Next

After internalizing this Quick Start, read in order:
1. `01_CONTEXT_MANAGEMENT.md` - Managing multiple active contexts
2. `02_CHAT_CODE_PLAN_GUIDE.md` - When to use which tool
3. `03_EVIDENCE_DRIVEN_DEV.md` - Test-first workflows
4. `04_CONTEXT_PROTOCOLS.md` - Detailed protocols for each context
5. `05_SKILLS_COMMANDS_HOOKS.md` - Advanced features
6. `06_PRACTICE_SCENARIOS.md` - Hands-on exercises

---

## ⚡ The Three Mantras (Memorize These)

1. **"Tests first, code second, evidence always"**
   - Never implement without knowing how to verify

2. **"One context, one session, one outcome"**
   - Focus beats multitasking every time

3. **"Document now or debug forever"**
   - Future-you will thank present-you

---

## 🎯 Your First Week Checklist

### Day 1: Setup
- [ ] Create `contexts/active/`, `contexts/backlog/`, `contexts/archive/`
- [ ] Read this file 3 times
- [ ] Do one practice session (any context)

### Day 2-3: Firefighting Practice
- [ ] Find a small bug
- [ ] Use hotfix protocol
- [ ] Document in context file

### Day 4-5: Building Practice
- [ ] Pick one small feature
- [ ] Write tests first
- [ ] Implement with Code

### Day 6-7: Multi-Context Practice
- [ ] Maintain 2-3 active contexts
- [ ] Switch between them across sessions
- [ ] Keep context files updated

---

**Print this page. Keep it visible. Read it every morning for 7 days.**

Next: Open `01_CONTEXT_MANAGEMENT.md`
