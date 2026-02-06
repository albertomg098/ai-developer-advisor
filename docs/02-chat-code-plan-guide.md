# Chat vs Code vs Plan Mode - The Complete Decision Guide
## When to use which tool (and when to use both)

---

## 🎯 The Core Distinction

**Think of it like cooking:**
- **Chat** = Recipe creation (what to cook, how to cook it)
- **Code** = Executing the recipe (actually cooking)
- **Plan Mode** = Meal prep service (executing complex multi-step recipes)

---

## 📊 Quick Decision Matrix

| Situation | Use Chat | Use Code | Use Plan Mode |
|-----------|----------|----------|---------------|
| "I don't know what's wrong" | ✅ YES | ❌ No | ❌ No |
| "I need to design an API" | ✅ YES | ❌ No | ❌ No |
| "I have 100 log files to analyze" | ✅ YES | ❌ No | ❌ No |
| "Review my code for bugs" | ✅ YES | ❌ No | ❌ No |
| "Create 5 related test files" | ❌ No | ❌ No | ✅ YES |
| "Implement a feature (clear spec)" | ❌ No | ✅ YES | Maybe |
| "Fix a bug (know the fix)" | ❌ No | ✅ YES | ❌ No |
| "Refactor 20 files consistently" | ❌ No | ❌ No | ✅ YES |
| "I'm stuck and confused" | ✅ YES | ❌ No | ❌ No |
| "Build complex multi-file feature" | Use both | Use both | ✅ YES |

---

## 🗣️ CLAUDE CHAT - The Strategic Brain

### Chat is your:
- 🧠 Architect and designer
- 🔍 Data analyst and debugger
- 🎓 Teacher and explainer
- 👨‍⚖️ Code reviewer and critic
- 🗺️ Navigation system when lost

### Use Chat When You Need To:

#### 1. **THINK** (Before Acting)
```
[CHAT] "I need to add authentication to my API.
Options I'm considering:
1. JWT with Clerk
2. OAuth with Auth0
3. Roll my own

Help me:
- Compare these approaches
- Identify risks
- Recommend best for my use case (FastAPI + React)"
```

**Why Chat:** Strategic decisions need evaluation, not implementation.

---

#### 2. **UNDERSTAND** (Confusion or Blockage)
```
[CHAT] "My FastAPI app is returning 500 errors but logs show nothing.
I've checked:
- Database connection (works)
- All endpoints (some work, some don't)
- Recent changes (only added logging)

Help me:
1. What am I NOT checking?
2. What debugging steps should I take?
3. Generate a diagnostic checklist"
```

**Why Chat:** You need hypothesis generation, not code execution.

---

#### 3. **ANALYZE** (Data, Logs, Errors)
```
[CHAT] Upload: error_logs_last_24h.txt (2MB of logs)

"Analyze these logs:
1. What are the top 3 error patterns?
2. When did each pattern start?
3. What's the likely root cause?
4. What should I investigate next?"
```

**Why Chat:** Pattern recognition and data synthesis is Chat's strength.

---

#### 4. **DESIGN** (Architecture, APIs, Data Models)
```
[CHAT] "I'm building a voice AI system with:
- FastRTC for voice
- LangGraph for conversation flow
- Supabase for persistence
- Multi-tenant with org isolation

Design:
1. Database schema for conversation history
2. State machine for conversation flow
3. API endpoints needed
4. Error handling strategy

Output as specs I can give to Claude Code"
```

**Why Chat:** Design requires trade-off evaluation and creative thinking.

---

#### 5. **REVIEW** (Code Quality, Security, Performance)
```
[CHAT] Upload: src/api/auth.py

"Review this authentication code for:
1. Security vulnerabilities
2. Edge cases not handled
3. Performance issues
4. Best practice violations

Be specific about what to change"
```

**Why Chat:** Critical analysis without executing code.

---

#### 6. **LEARN** (How Things Work)
```
[CHAT] "Explain FastAPI dependency injection:
1. How it works under the hood
2. When to use vs not use
3. Common mistakes
4. Example with database session management"
```

**Why Chat:** Educational explanations with context.

---

### Chat Workflow Patterns

#### Pattern A: Design → Code Pipeline
```
Step 1 [CHAT]: Design the solution
"I need to add bulk CSV import. Design:
- API endpoint spec
- Validation requirements
- Error handling
- Test scenarios"

Output: Detailed spec document

Step 2 [CODE]: Implement the spec
claude "Implement bulk CSV import following the spec in
[paste chat output or upload spec file]"
```

---

#### Pattern B: Analyze → Fix Pipeline
```
Step 1 [CHAT]: Upload logs/errors
"Analyze these errors and create a hypothesis-ranked list"

Output: 
1. Most likely: DB connection timeout (70% confidence)
2. Less likely: Memory leak (20% confidence)
3. Unlikely: Config issue (10% confidence)

Step 2 [CODE]: Test hypothesis #1
claude "Create tests/debug/test_db_timeout.py that:
- Simulates high connection load
- Measures connection acquisition time
- Confirms if timeouts occur"
```

---

#### Pattern C: Code → Review → Refine Pipeline
```
Step 1 [CODE]: Implement feature
claude "Implement user authentication with JWT"

Step 2 [CHAT]: Review what Code produced
Upload: src/api/auth.py
"Review this auth implementation. What's missing or wrong?"

Step 3 [CODE]: Apply Chat's recommendations
claude "Fix the issues identified in review:
1. Add token refresh
2. Handle expired tokens properly
3. Add rate limiting"
```

---

## 💻 CLAUDE CODE - The Builder

### Code is your:
- ⚙️ Implementation engine
- 🧪 Test executor
- 📝 File manipulator
- 🔨 Rapid scaffolder

### Use Code When You Need To:

#### 1. **IMPLEMENT** (Clear Specifications)
```bash
# You already know WHAT and HOW
# Just need execution

claude "Implement user registration endpoint:
- POST /api/users/register
- Input: email, password, name
- Validation: email format, password strength
- Creates user in database
- Returns JWT token
- Tests in tests/test_registration.py

Follow existing patterns in src/api/auth.py"
```

**Why Code:** Clear task, just needs implementation.

---

#### 2. **TEST** (Create or Run Tests)
```bash
# Create tests
claude "Create tests/test_voice_detection.py:
- Test WAV file processing
- Test MP3 file processing
- Test invalid format handling
- Use fixtures from tests/fixtures/audio/"

# Run tests
claude "Run pytest tests/test_voice_detection.py -v
Show me results and any failures"
```

**Why Code:** Direct file manipulation and command execution.

---

#### 3. **DEBUG** (With Known Hypothesis)
```bash
# After Chat generated hypothesis
claude "The bug is likely in src/audio_processor.py line 45
where we're not handling None returns.

Fix:
1. Add None check before calling .get()
2. Add test that reproduces the bug
3. Verify test passes after fix"
```

**Why Code:** Executing a specific fix.

---

#### 4. **SCAFFOLD** (Project Setup, Boilerplate)
```bash
claude "Create a new FastAPI microservice with:
- src/api/ (routes)
- src/services/ (business logic)
- src/models/ (data models)
- tests/ (pytest structure)
- Dockerfile
- requirements.txt
- README.md

Follow clean architecture pattern"
```

**Why Code:** Bulk file creation.

---

#### 5. **REFACTOR** (Simple, Localized Changes)
```bash
# Single file refactoring
claude "Refactor src/utils/helpers.py:
- Extract duplicate validation logic into validate_email()
- Add docstrings to all functions
- Add type hints
- Keep tests passing"
```

**Why Code:** Concrete changes with clear scope.

---

### Code Workflow Patterns

#### Pattern A: Test-First Development
```bash
# Step 1: Create failing test
claude "Create tests/test_csv_import.py:
- Test valid CSV import (should import 100 users)
- Test invalid format (should raise ValidationError)
- Test duplicate emails (should skip duplicates)

Tests should FAIL initially"

# Step 2: Run tests (confirm they fail)
claude "Run pytest tests/test_csv_import.py -v"

# Step 3: Implement to pass tests
claude "Implement CSV import to make all tests pass.
File: src/services/import_service.py"

# Step 4: Verify
claude "Run pytest tests/test_csv_import.py -v
Confirm all tests pass"
```

---

#### Pattern B: Iterative Building
```bash
# Build complex feature in stages

# Stage 1: Data model
claude "Create src/models/invoice.py with Invoice model
and tests/test_invoice_model.py"

# Verify Stage 1
pytest tests/test_invoice_model.py

# Stage 2: Repository layer
claude "Create src/repositories/invoice_repo.py
and tests/test_invoice_repo.py"

# Verify Stage 2
pytest tests/test_invoice_repo.py

# Stage 3: API endpoints
claude "Create src/api/invoices.py with CRUD endpoints
and tests/integration/test_invoice_api.py"

# Verify Stage 3
pytest tests/integration/

# Each stage builds on verified previous stages
```

---

#### Pattern C: Bug Fix Protocol
```bash
# Step 1: Reproduce bug in test
claude "Create tests/hotfix/test_auth_bug.py that:
- Reproduces the exact bug we're seeing
- Shows expected vs actual behavior
- Fails currently"

# Step 2: Verify reproduction
claude "Run the new test, confirm it fails"

# Step 3: Fix
claude "Fix src/api/auth.py to make test pass.
Change ONLY what's needed, no refactoring"

# Step 4: Verify fix
claude "Run full test suite to ensure no regressions"
```

---

## 🎯 PLAN MODE - The Orchestrator

### Plan Mode is your:
- 🎼 Multi-step orchestrator
- 🏗️ Complex builder
- 🔄 Bulk operation executor
- 📋 Task decomposer

### Use Plan Mode When:

#### 1. **Multi-File Coordinated Changes**
```bash
# Example: Rename "user" to "member" across entire codebase

claude --plan "Refactor: rename 'user' to 'member' throughout codebase:
1. Update src/models/user.py → src/models/member.py
2. Update all imports in src/api/*.py
3. Update all references in src/services/*.py
4. Update database migrations
5. Update tests to use 'member'
6. Update API documentation

Ensure tests pass after each step"
```

**Why Plan:** Coordinated changes across many files with dependencies.

---

#### 2. **Complex Feature with Many Components**
```bash
claude --plan "Implement complete authentication system:
1. Create database models (User, Session)
2. Create repository layer (UserRepo, SessionRepo)
3. Create authentication service (login, logout, verify)
4. Create API endpoints (/login, /logout, /verify)
5. Add middleware for protected routes
6. Create comprehensive tests for each layer
7. Update API documentation

Follow test-first approach for each component"
```

**Why Plan:** Multi-layer implementation with dependencies.

---

#### 3. **Bulk Consistent Operations**
```bash
claude --plan "Add type hints to entire codebase:
1. Add type hints to all functions in src/models/*.py
2. Add type hints to all functions in src/services/*.py
3. Add type hints to all functions in src/api/*.py
4. Run mypy to verify type correctness
5. Fix any type errors
6. Update CI to include mypy checks"
```

**Why Plan:** Consistent operation across many files.

---

#### 4. **Migration or Major Refactoring**
```bash
claude --plan "Migrate from SQLAlchemy to Supabase:
1. Install supabase-py, remove SQLAlchemy
2. Create new connection module src/database.py
3. Update all models to use Supabase queries
4. Update all repositories to use new connection
5. Update tests to use Supabase test fixtures
6. Verify all tests pass
7. Update deployment configuration

Proceed step-by-step, verify each before next"
```

**Why Plan:** High-risk changes requiring careful progression.

---

### When NOT to Use Plan Mode

❌ **Don't use Plan for:**
- Single file changes
- Simple bug fixes
- Exploratory work
- Investigating issues

❌ **Plan Mode Problems:**
- Can get "stuck" on one step
- Harder to course-correct mid-way
- Less interactive than regular Code mode

✅ **Use regular Code mode instead:**
- You can break the task down yourself
- You want to verify each step interactively
- The task is exploratory

---

## 🔄 The Chat → Code → Plan Pipeline

### Full Workflow for Complex Features

```
Phase 1: DESIGN (Chat)
├─ "I need to build [feature]. Design the architecture"
├─ Output: Detailed spec document
└─ Decision: Is this simple or complex?

Phase 2a: SIMPLE IMPLEMENTATION (Code)
├─ If <5 files, straightforward
├─ claude "Implement [feature] following [spec]"
└─ Test and verify

Phase 2b: COMPLEX IMPLEMENTATION (Plan)
├─ If >5 files, many dependencies
├─ claude --plan "Implement [feature] step-by-step: [steps]"
└─ Monitor and verify each step

Phase 3: REVIEW (Chat)
├─ Upload: All new code
├─ "Review implementation for issues"
└─ Output: List of improvements

Phase 4: REFINEMENT (Code)
├─ claude "Apply review feedback: [list]"
└─ Final verification
```

---

## 🎯 Context-Specific Tool Selection

### For EXPLORATION Contexts:
```
Primary: CHAT (analyze, generate hypotheses)
Secondary: CODE (test hypotheses, gather evidence)
Rarely: PLAN (not helpful when exploring)

Typical flow:
1. [CHAT] Analyze the problem, create investigation plan
2. [CODE] Execute diagnostic tests
3. [CHAT] Analyze test results
4. [CODE] Test next hypothesis
... repeat until root cause found
```

---

### For FIREFIGHTING Contexts:
```
Primary: CODE (fast execution)
Secondary: CHAT (if stuck or need review)
Rarely: PLAN (too slow for emergencies)

Typical flow:
1. [CODE] Create reproduction test
2. [CODE] Implement minimal fix
3. [CODE] Verify no regressions
4. [CHAT] Post-fix: review for prevention
```

---

### For BUILDING Contexts:
```
Primary: CODE (implementation)
Often: CHAT (design before building)
Sometimes: PLAN (if complex)

Typical flow:
1. [CHAT] Design the feature
2. [CODE] Create tests first
3. [CODE or PLAN] Implement feature
4. [CHAT] Review implementation
5. [CODE] Apply improvements
```

---

### For IMPROVING Contexts:
```
Primary: CHAT (analysis and strategy)
Secondary: CODE (apply improvements)
Sometimes: PLAN (bulk refactoring)

Typical flow:
1. [CHAT] Analyze codebase, identify improvements
2. [CHAT] Prioritize improvements by impact
3. [CODE or PLAN] Apply improvements
4. [CHAT] Measure improvement
```

---

## 💡 Pro Tips

### Tip 1: Use Chat as Your "Rubber Duck"
Before implementing anything complex:
```
[CHAT] "I'm about to implement [X] by doing [Y].
What am I not considering?
What will break?
What's a simpler approach?"

Then take that feedback to Code
```

---

### Tip 2: Upload Artifacts to Chat for Review
```
# After Code session
git diff > /tmp/changes.diff

[CHAT] Upload: /tmp/changes.diff
"Review these changes:
- Security issues?
- Edge cases missed?
- Test coverage gaps?"
```

---

### Tip 3: Use Code for Quick Prototypes, Chat for Decisions
```
[CODE] "Create 3 versions of the login form:
1. Simple email/password
2. With OAuth buttons
3. With magic link

Put in prototypes/login_*.py"

[CHAT] Upload: prototypes/*
"Which login approach fits our use case best?
Consider: security, UX, maintenance"
```

---

### Tip 4: Context Switching = Tool Switching
```
When you feel lost → Switch to CHAT
When you know what to do → Switch to CODE
When you have clear multi-step plan → Use PLAN
```

---

### Tip 5: The "Stuck for 20 Minutes" Rule
```
If stuck in CODE for >20 minutes without progress:
STOP → Switch to CHAT → Get unstuck → Return to CODE

Don't waste 2 hours in Code when Chat can unstick you in 5 minutes
```

---

## 🎓 Practice Exercises

### Exercise 1: Chat → Code Pipeline
```
1. [CHAT] "Design a rate limiting system for my API"
2. Receive design from Chat
3. [CODE] Implement following the design
4. [CHAT] Review implementation
5. [CODE] Apply feedback
```

### Exercise 2: Investigation with Both Tools
```
1. [CHAT] "Mystery bug: users randomly logged out. Analyze possible causes"
2. [CODE] Create diagnostic tests for top hypothesis
3. [CODE] Run tests, gather evidence
4. [CHAT] Upload test results, "What does this tell us?"
5. Repeat until solved
```

### Exercise 3: Complex Feature with Plan
```
1. [CHAT] "Design complete user notification system"
2. Receive multi-component design
3. [PLAN] Implement step-by-step
4. [CHAT] Review completed implementation
5. [CODE] Fix any issues identified
```

---

## 📋 Decision Checklist

Before starting ANY work, ask:

**Do I need to THINK or UNDERSTAND?**
→ Use CHAT first

**Do I know exactly what to build and how?**
→ Use CODE

**Is this >5 files with dependencies?**
→ Consider PLAN mode

**Am I stuck or confused?**
→ Switch to CHAT (even if in middle of Code session)

**Do I need to review or analyze something?**
→ Use CHAT

---

**Next:** Read `03_EVIDENCE_DRIVEN_DEV.md` to learn how to ensure everything you build is testable and verifiable.
