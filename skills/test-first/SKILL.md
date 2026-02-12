---
name: test-first
description: >
  Evidence-driven test-first development methodology. Covers the evidence pyramid,
  red-green-refactor workflow, test-level hierarchy, and universal test-first prompt
  templates. Use when implementing features, fixing bugs, or any task that produces code.
---

# Test-First — Evidence-Driven Development

## 🎯 Core Philosophy

**"If you can't prove it works, it doesn't work."**

Every line of code must answer: **"How do I know this works?"**

---

## 📊 The Evidence Pyramid

```
             🚀 PRODUCTION
            (monitoring data)
                  ↑
          📊 E2E TESTS PASS
         (user flows work)
                  ↑
      🔗 INTEGRATION TESTS PASS
     (components work together)
                  ↑
    ✅ UNIT TESTS PASS
   (functions work correctly)
                  ↑
  📝 TESTS EXIST & FAIL
 (we know what "working" means)
                  ↑
📋 REQUIREMENTS CLEAR
(we know what to build)
```

**NEVER skip a level. Always work bottom-up.**

---

## 🔨 The Test-First Discipline

### The Golden Rule
**"Test before code, always. No exceptions."**

### Why Test-First
1. **Clarity** — forces you to define "done" before starting
2. **Safety** — prevents regressions
3. **Speed** — faster to test than to manually verify
4. **Documentation** — tests show how code should be used
5. **Confidence** — deploy without fear

---

## 🎯 Red-Green-Refactor Cycle

### 🔴 RED: Create Tests That Fail
```bash
# Create tests FIRST — feature not implemented yet
# File: tests/test_[feature].py
# Run: pytest tests/test_[feature].py -v
# Expected: All tests FAIL
```

### 🟢 GREEN: Make Tests Pass (Minimal Implementation)
```bash
# Implement ONLY what's needed to pass tests
# File: src/[feature].py
# Run: pytest tests/test_[feature].py -v
# Expected: All tests PASS
```

### 🔄 REFACTOR: Clean Up (Tests Still Pass)
```bash
# Improve code quality — tests must still pass
# Run: pytest tests/test_[feature].py -v
# Expected: All tests STILL PASS
```

---

## 📝 Universal Test-First Prompt Template

Use this for EVERY implementation task:

```
Feature/Fix: [NAME]

PHASE 1 — TEST SPECIFICATION (do this first):
Create tests/test_[name].py with comprehensive test cases:
- Happy path: [describe expected behavior]
- Edge cases: [list boundary conditions]
- Error cases: [list failure scenarios]
- Integration: [if applicable, how it connects]

Requirements for tests:
- Use pytest with clear names: test_should_[action]_when_[condition]
- Use fixtures for common test data
- Tests should FAIL initially (feature not yet implemented)
- Each test should have GIVEN/WHEN/THEN structure

PHASE 2 — IMPLEMENTATION (only after Phase 1):
Implement [name] in [file path] to make all tests pass.
- Follow existing code patterns
- Implement ONLY what tests require
- No features beyond test coverage

PHASE 3 — VERIFICATION:
Run full test suite: pytest tests/ -v --cov=[module]
Show: coverage percentage, failing tests, uncovered paths
```

---

## 🏗️ Test Levels — When to Use Each

### Level 1: Unit Tests
**What:** Individual functions/methods in isolation
**When:** Always, for every function with logic
**Speed:** Milliseconds

```python
# tests/unit/test_validators.py
def test_should_validate_email_when_format_correct():
    # GIVEN: a valid email
    email = "user@example.com"
    # WHEN: validating
    result = validate_email(email)
    # THEN: validation passes
    assert result is True

def test_should_reject_email_when_missing_at_symbol():
    # GIVEN: invalid email
    email = "userexample.com"
    # WHEN: validating
    result = validate_email(email)
    # THEN: validation fails
    assert result is False
```

### Level 2: Integration Tests
**What:** Components working together (API + database)
**When:** Features involving multiple components
**Speed:** Seconds

```python
# tests/integration/test_user_api.py
def test_should_create_user_in_database_when_api_called():
    # GIVEN: test database and API client
    client = TestClient(app)
    # WHEN: calling POST /users
    response = client.post("/users", json={"email": "test@example.com", "name": "Test"})
    # THEN: user created
    assert response.status_code == 201
    user = db.query(User).filter_by(email="test@example.com").first()
    assert user is not None
```

### Level 3: E2E Tests
**What:** Complete user workflows
**When:** Critical user journeys
**Speed:** Seconds to minutes

```python
# tests/e2e/test_registration_flow.py
def test_complete_registration_and_login():
    # GIVEN: new user
    client = TestClient(app)
    # WHEN: register → confirm → login
    reg = client.post("/register", json={"email": "new@test.com", "password": "Pass123!"})
    assert reg.status_code == 201
    # AND: login with credentials
    login = client.post("/login", json={"email": "new@test.com", "password": "Pass123!"})
    # THEN: receives valid token
    assert login.status_code == 200
    assert "token" in login.json()
```

---

## 🎯 Prompt Patterns That Enforce Evidence

### Pattern 1: "Test-First"
```
Feature: [X]

IMPORTANT: Follow test-first development:
1. First, create ONLY tests in tests/test_[x].py
2. Run tests — they should FAIL
3. Then, implement feature to make tests PASS
4. Run tests again — all should PASS

Do NOT implement the feature until tests exist and fail.
```

### Pattern 2: "Evidence Required"
```
Implement [X]

Requirements:
- Tests must exist BEFORE implementation
- Test coverage must be >80%
- All tests must pass
- Must include integration test

Show me:
1. Test file created
2. Test results (failed initially)
3. Implementation
4. Test results (passed after)
5. Coverage report
```

### Pattern 3: "Red-Green-Refactor"
```
Feature: [X]

RED: Create tests that fail → show failures
GREEN: Make tests pass (minimal) → show passing
REFACTOR: Clean up → show tests still pass

Do these three phases explicitly.
```

---

## 🔍 Verification Checklist

After ANY implementation, verify:

```
□ Tests exist for the feature/fix
□ Tests are comprehensive (happy path + edge + error cases)
□ Tests pass: pytest tests/test_[feature].py -v
□ Coverage adequate: pytest tests/ --cov=[module] --cov-report=term-missing (>80%)
□ No regressions: pytest tests/ -v (all old tests still pass)
□ Integration works: pytest tests/integration/ -v
□ QA validation: app runs and works live (if applicable — N/A for libraries)

IF ANY FAIL → Feature is NOT done.
```

---

## 🎯 Context-Specific Evidence

### 🔍 EXPLORATION
Evidence needed: investigation findings, not code tests
- Reproduction steps
- Minimal test case
- Hypotheses tested (with results)
- Root cause (if found)

### 🔥 FIREFIGHTING
Evidence needed: bug reproduction + fix verification
1. Test that reproduces bug → FAILS
2. Fix applied
3. Test now → PASSES
4. Full suite → no regressions

### 🏗️ BUILDING
Evidence needed: full test pyramid
- Unit tests pass
- Integration tests pass
- E2E test passes
- Coverage >80%
- All existing tests pass
- QA validation passes (for runnable apps)

### ✨ IMPROVING
Evidence needed: before/after measurements
1. Benchmark BEFORE
2. Make improvement
3. Benchmark AFTER
4. All tests still pass
5. Show measurable improvement

---

## 🚨 The "No Test, No Merge" Policy

**No code merges to main without:**
1. ✅ Tests written BEFORE implementation
2. ✅ All tests passing
3. ✅ Coverage >80% for new code
4. ✅ Integration tests for multi-component features
5. ✅ No regressions
6. ✅ QA validation for runnable apps (API/frontend/CLI)

---

## 🏗️ End-to-End Feature Implementation Flow

```
Phase 1:  REQUIREMENTS     → Chat: design feature, define test scenarios
Phase 2:  TEST SPEC        → Code: create tests/specs/[feature]_spec.md
Phase 3:  UNIT TESTS       → Code: tests should FAIL
Phase 4:  UNIT IMPL        → Code: tests should PASS → commit
Phase 5:  INTEGRATION TESTS → Code: tests should FAIL
Phase 6:  INTEGRATION IMPL → Code: tests should PASS → commit
Phase 7:  E2E TESTS        → Code: tests should FAIL
Phase 8:  E2E COMPLETION   → Code: tests should PASS → commit
Phase 9:  FULL VERIFY      → Code: pytest tests/ -v --cov=src
Phase 9.5: QA VALIDATE    → Code: start app, curl/test live, fix QA bugs
Phase 10: REVIEW           → Chat: review code + coverage
Phase 11: HARDEN           → Code: apply feedback, add tests
Phase 12: DOCUMENTATION    → Code: docstrings, API docs
Phase 13: FINAL VERIFY     → Code: all green, >80% coverage → merge
```
