# Claude Code Mastery - Complete Knowledge Package
## Your 7-Day Path to Evidence-Driven AI Development

---

## 📚 What's Inside

This package contains everything you need to master Claude Code and become a highly productive, evidence-driven developer.

### Core Guides (Read in Order)

1. **00_QUICK_START.md** ⭐ START HERE
   - Daily rituals
   - The 4 contexts
   - Emergency protocols
   - 5-minute reference guide

2. **01_CONTEXT_MANAGEMENT.md**
   - Managing multiple projects simultaneously
   - Context file system
   - Switching between contexts
   - Real-world examples

3. **02_CHAT_CODE_PLAN_GUIDE.md**
   - When to use Chat vs Code vs Plan
   - Decision matrices
   - Tool workflows
   - Context-specific guidance

4. **03_EVIDENCE_DRIVEN_DEV.md** ⭐ CORE PRINCIPLE
   - Test-first development
   - The evidence pyramid
   - Making Claude Code produce testable code
   - Verification protocols

5. **04_CONTEXT_PROTOCOLS.md**
   - Step-by-step workflows for each context
   - Real examples with actual code
   - Context switching protocols
   - Session management

6. **05_SKILLS_COMMANDS_HOOKS.md**
   - Leveraging Claude Code's advanced features
   - Creating custom skills
   - Building command libraries
   - Automating quality gates with hooks

---

## 🎯 Your Learning Path

### Week 1: Fundamentals

**Day 1 - Monday: Setup & First Session**
- Read: `00_QUICK_START.md` (print it, keep it visible)
- Read: `01_CONTEXT_MANAGEMENT.md`
- Action: Create contexts/ directory structure
- Practice: One complete session (any small task)

**Day 2 - Tuesday: Evidence-Driven Development**
- Read: `03_EVIDENCE_DRIVEN_DEV.md` (this is THE core)
- Action: Pick one small feature
- Practice: Build it test-first (tests → implement → verify)

**Day 3 - Wednesday: Tool Mastery**
- Read: `02_CHAT_CODE_PLAN_GUIDE.md`
- Practice: Use Chat for design, Code for implementation
- Practice: One investigation using Chat → Code pipeline

**Day 4 - Thursday: Context Protocols**
- Read: `04_CONTEXT_PROTOCOLS.md`
- Practice: Deliberately practice each context type
  - Morning: EXPLORATION (find something confusing)
  - Afternoon: BUILDING (implement small feature)
  - Evening: Review progress

**Day 5 - Friday: Advanced Features**
- Read: `05_SKILLS_COMMANDS_HOOKS.md`
- Action: Create your first custom skill
- Action: Set up basic git hooks
- Practice: Use skills explicitly in prompts

**Day 6-7 - Weekend: Integration Practice**
- Practice: Work on real project using full framework
- Maintain 2-3 active contexts
- Use test-first for everything
- Use hooks to enforce quality

---

## 💎 The Core Principles (Memorize These)

### 1. Evidence-Driven Development
**"If you can't prove it works, it doesn't work."**

Never ship code without:
- ✅ Tests written FIRST
- ✅ Tests passing
- ✅ Coverage >80%
- ✅ No regressions

### 2. Context Management
**"One context, one session, one outcome."**

You'll have multiple contexts active, but work on ONE at a time:
- 🔍 EXPLORATION: "WTF is broken?"
- 🔥 FIREFIGHTING: "Fix it NOW"
- 🏗️ BUILDING: "I know what to build"
- ✨ IMPROVING: "Make it better"

### 3. Tool Selection
**"Think in Chat, Build in Code, Orchestrate with Plan."**

- Chat = Strategy, design, analysis
- Code = Implementation, testing, files
- Plan = Multi-step orchestration

### 4. Test-First Always
**"Tests before code, always. No exceptions."**

Workflow:
1. Write tests (should FAIL)
2. Implement (tests PASS)
3. Verify (no regressions)
4. Commit

---

## 🚀 Quick Reference Cards

### Session Starter Checklist
```
□ Read contexts/active/*.md
□ Choose ONE to work on
□ Know what test proves success
□ Have evidence from last session
```

### Session Ender Checklist
```
□ Update context file with progress
□ Run tests (all passing or documented why not)
□ Commit or document why not
□ Write "next step" for tomorrow
```

### "I'm Stuck" Protocol
```
1. STOP coding (>30 min no progress)
2. Switch to Chat: analyze the problem
3. Generate hypotheses
4. Test hypotheses in Code
5. If still stuck: document and ask for help
```

### Universal Test-First Prompt
```bash
claude "Feature: [NAME]

PHASE 1: Create tests (should FAIL initially)
PHASE 2: Implement to make tests PASS
PHASE 3: Verify no regressions

Show evidence at each phase."
```

---

## 📋 Project Setup Template

**Copy this to every new project:**

```bash
# Directory structure
mkdir -p contexts/{active,backlog,archive}
mkdir -p investigations
mkdir -p tests/{unit,integration,e2e,fixtures}

# Create context template
cat > contexts/TEMPLATE.md << 'TEMPLATE'
# [Context Type]: [Short Title]

**Status:** 🟢 Active / 🟡 Blocked / 🔴 Urgent / ✅ Done
**Started:** YYYY-MM-DD
**Branch:** [if applicable]

## 🎯 Outcome Goal
[One sentence: what does "done" look like?]

## ✅ Success Criteria
- [ ] [Specific, testable criterion]

## 📊 Current State
[What works, what doesn't, what's in progress]

## 📝 Session Log
[Add entry after each session]

## 🧪 Evidence/Tests
[Link to tests, coverage, results]
TEMPLATE

# Create commands file
cat > .claude/commands.md << 'COMMANDS'
## Session Management
### @start
Read contexts/active/*.md and summarize

### @end  
Update context, run tests, commit

## Testing
### @test-quick
Run tests for modified files

### @test-all
Full suite with coverage

## Evidence
### @prove-it-works
Run tests + coverage + create evidence summary
COMMANDS

# Create pre-commit hook
cat > .git/hooks/pre-commit << 'HOOK'
#!/bin/bash
pytest tests/ -v --cov=src --cov-fail-under=80
if [ $? -ne 0 ]; then
    echo "❌ Tests failed or coverage <80%"
    exit 1
fi
HOOK

chmod +x .git/hooks/pre-commit

# Create README with testing policy
cat >> README.md << 'POLICY'

## Testing Policy

**No merges without:**
1. ✅ Tests written FIRST
2. ✅ All tests passing
3. ✅ Coverage >80%
4. ✅ No regressions
POLICY
```

---

## 🎓 Success Metrics

**After 1 week, you should:**
- ✅ Have 2-3 active contexts you're managing
- ✅ Be writing tests BEFORE code automatically
- ✅ Know whether to use Chat or Code without thinking
- ✅ Have git hooks enforcing quality
- ✅ Feel confident switching between contexts

**After 1 month, you should:**
- ✅ Ship features 40-50% faster
- ✅ Have 70% fewer regressions
- ✅ Context files show clear progress
- ✅ Have created 2-3 custom skills
- ✅ Rarely get "stuck" for more than 30 minutes

---

## 💡 Common Pitfalls to Avoid

### ❌ Pitfall 1: Implementing Without Tests
**Symptom:** "I'll add tests later"
**Fix:** Force yourself - tests FIRST, always

### ❌ Pitfall 2: Working on Too Many Contexts
**Symptom:** Nothing gets done
**Fix:** One context per session, maintain all

### ❌ Pitfall 3: Not Updating Context Files
**Symptom:** Lost context between sessions
**Fix:** Update after EVERY session (make it a habit)

### ❌ Pitfall 4: Using Code for Design
**Symptom:** Lots of rewrites, unclear direction
**Fix:** Design in Chat, implement in Code

### ❌ Pitfall 5: Ignoring Test Failures
**Symptom:** "I'll fix tests later"
**Fix:** Tests are non-negotiable - fix or don't commit

---

## 📞 When You Need Help

### Quick Answers
- Reread `00_QUICK_START.md`
- Check the relevant protocol in `04_CONTEXT_PROTOCOLS.md`

### Stuck on Concepts
- Reread `02_CHAT_CODE_PLAN_GUIDE.md` for tool selection
- Reread `03_EVIDENCE_DRIVEN_DEV.md` for testing approach

### Example Needed
- All protocol files have complete real-world examples
- Context files include actual commands and outputs

---

## 🎯 Your First Action (Right Now)

1. **Print `00_QUICK_START.md`** - Put it next to your monitor
2. **Create contexts/active/** - Your command center
3. **Do one practice session:**
   - Pick the smallest task in your project
   - Create a context file
   - Write tests first
   - Implement
   - Update context file
   - Commit

4. **Come back tomorrow** - Read `01_CONTEXT_MANAGEMENT.md`

---

## 📚 File Reference

| File | Purpose | When to Read |
|------|---------|--------------|
| `00_QUICK_START.md` | Daily reference, quick lookup | Every morning first week |
| `01_CONTEXT_MANAGEMENT.md` | Multi-project juggling | When overwhelmed |
| `02_CHAT_CODE_PLAN_GUIDE.md` | Tool selection | When unsure which tool |
| `03_EVIDENCE_DRIVEN_DEV.md` | Testing discipline | Daily until automatic |
| `04_CONTEXT_PROTOCOLS.md` | Step-by-step workflows | When starting any task |
| `05_SKILLS_COMMANDS_HOOKS.md` | Power features | After basics mastered |

---

## 🎉 You've Got This!

This framework is based on real usage patterns (your own!) and battle-tested protocols.

**Remember:**
- 🎯 One context at a time
- 🧪 Tests before code
- 📝 Document as you go
- 🔄 Small, verified steps

**Start small. Practice daily. Build the habit.**

The investment this week will pay dividends for years.

---

**Ready? Open `00_QUICK_START.md` and begin.**
