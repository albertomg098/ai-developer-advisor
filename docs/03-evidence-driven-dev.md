# Evidence-Driven Development - Test-First, Always Verifiable
## How to make Claude Code produce testable, reliable, end-to-end implementations

---

## 🎯 The Core Philosophy

**"If you can't prove it works, it doesn't work."**

Every line of code should answer: **"How do I know this works?"**

---

## 📊 The Evidence Pyramid (Non-Negotiable)

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
                      ↑
  🤔 PROBLEM UNDERSTOOD
 (we know WHY we're building)
```

**NEVER skip a level.**

---

## 🔨 The Test-First Discipline

### The Golden Rule
**"Test before code, always. No exceptions."**

This isn't optional. This is THE framework.

### Why Test-First?
1. **Clarity:** Forces you to define "done" before starting
2. **Safety:** Prevents regressions
3. **Speed:** Faster to test than to manually verify
4. **Documentation:** Tests show how code should be used
5. **Confidence:** Deploy without fear

---

## 🎯 How to Make Claude Code Do Test-First

### ❌ The Wrong Way (What You Might Be Doing)

```bash
# This is BAD
claude "Implement user authentication with JWT"

# What happens:
# - Claude writes 300 lines of code
# - You have no idea if it works
# - No tests
# - You manually test it (maybe)
# - Ships with bugs
```

### ✅ The Right Way (Test-First Discipline)

```bash
# STEP 1: Define success criteria
claude "For user authentication feature, create a test specification:

## Test Scenarios
1. Valid login → JWT token returned
2. Invalid password → 401 error
3. Non-existent user → 404 error
4. Expired token → 401 error
5. Token refresh → New token returned

Output as a checklist in tests/specs/auth_tests.md"

# STEP 2: Create failing tests FIRST
claude "Create tests/test_auth.py with tests for all scenarios in
tests/specs/auth_tests.md

Requirements:
- Use pytest
- Tests should FAIL initially (features not implemented yet)
- Clear assertions for each scenario
- Use fixtures for test data

DO NOT implement the actual auth code yet, ONLY the tests."

# STEP 3: Verify tests fail (proof we haven't cheated)
pytest tests/test_auth.py -v
# Output should show: 5 failed tests

# STEP 4: Implement ONLY to make tests pass
claude "Now implement user authentication in src/api/auth.py
to make ALL tests in tests/test_auth.py pass.

Requirements:
- Use JWT for tokens
- Follow existing patterns in codebase
- Implement ONLY what's needed to pass tests
- No extra features not covered by tests"

# STEP 5: Verify tests pass
pytest tests/test_auth.py -v
# Output should show: 5 passed tests

# STEP 6: Integration verification
pytest tests/ -v  # All tests including old ones
```

---

## 📝 The Universal Test-First Prompt Template

Use this for EVERY feature request to Claude Code:

```bash
claude "Feature: [FEATURE NAME]

PHASE 1 - TEST SPECIFICATION (Do this first):
Create tests/test_[feature].py with comprehensive test cases covering:
- Happy path: [describe expected behavior]
- Edge cases: [list edge cases]
- Error cases: [list error scenarios]
- Integration: [if applicable, how it integrates]

Requirements for tests:
- Use pytest with clear test names (test_should_[action]_when_[condition])
- Use fixtures for common test data
- Tests should FAIL initially (feature not yet implemented)
- Each test should have:
  * GIVEN: Setup/preconditions
  * WHEN: Action being tested
  * THEN: Expected outcome with assertions

PHASE 2 - IMPLEMENTATION (Only after Phase 1):
Implement [FEATURE NAME] in [file path] to make all tests pass.

Requirements for implementation:
- Follow existing code patterns
- Add type hints
- Add docstrings
- Implement ONLY what tests require
- No additional features beyond test coverage

PHASE 3 - VERIFICATION:
Run full test suite to ensure no regressions:
pytest tests/ -v --cov=[module]

Show me:
- Test coverage percentage
- Any failing tests
- Any uncovered code paths"
```

### Example Usage:

```bash
claude "Feature: Bulk CSV User Import

PHASE 1 - TEST SPECIFICATION:
Create tests/test_bulk_import.py with comprehensive test cases covering:
- Happy path: CSV with 100 valid users → all imported
- Edge cases: Empty CSV, CSV with headers only
- Error cases: Invalid email format, duplicate emails, missing fields
- Integration: Database constraints enforced

Requirements for tests:
- Use pytest with fixtures/sample_valid_csv, sample_invalid_csv
- Tests should FAIL initially
- Each test should have GIVEN/WHEN/THEN structure

PHASE 2 - IMPLEMENTATION:
Implement bulk CSV import in src/services/import_service.py
to make all tests pass.

Requirements:
- Use pandas for CSV parsing
- Validate emails with regex
- Skip duplicates with warning log
- Return ImportResult with success/failure counts

PHASE 3 - VERIFICATION:
Run pytest tests/ -v --cov=src.services.import_service
Show coverage and any failures"
```

---

## 🏗️ Test Levels - When to Use Each

### Level 1: Unit Tests
**What:** Test individual functions/methods in isolation
**When:** Always, for every function with logic
**Speed:** Milliseconds per test

```python
# tests/unit/test_validators.py
def test_should_validate_email_when_format_correct():
    # GIVEN: a valid email
    email = "user@example.com"
    
    # WHEN: validating the email
    result = validate_email(email)
    
    # THEN: validation passes
    assert result is True

def test_should_reject_email_when_missing_at_symbol():
    # GIVEN: invalid email without @
    email = "userexample.com"
    
    # WHEN: validating
    result = validate_email(email)
    
    # THEN: validation fails
    assert result is False
```

**Claude Code Prompt:**
```bash
claude "Create unit tests for all functions in src/utils/validators.py

Requirements:
- Test each function independently
- Mock any external dependencies
- Cover all branches (if/else)
- Test boundary conditions
- Tests in tests/unit/test_validators.py"
```

---

### Level 2: Integration Tests
**What:** Test components working together (e.g., API + Database)
**When:** For features involving multiple components
**Speed:** Seconds per test

```python
# tests/integration/test_user_api.py
def test_should_create_user_in_database_when_api_called():
    # GIVEN: test database and API client
    client = TestClient(app)
    
    # WHEN: calling POST /users
    response = client.post("/users", json={
        "email": "test@example.com",
        "name": "Test User"
    })
    
    # THEN: user created in database
    assert response.status_code == 201
    user = db.query(User).filter_by(email="test@example.com").first()
    assert user is not None
    assert user.name == "Test User"
```

**Claude Code Prompt:**
```bash
claude "Create integration tests for user creation API

Requirements:
- Test API endpoint + database interaction
- Use test database (not production)
- Setup/teardown for each test
- Verify database state after API calls
- Tests in tests/integration/test_user_api.py"
```

---

### Level 3: End-to-End Tests
**What:** Test complete user workflows (like a user would)
**When:** For critical user journeys
**Speed:** Seconds to minutes per test

```python
# tests/e2e/test_user_registration_flow.py
def test_complete_user_registration_and_login_flow():
    # GIVEN: a new user wants to register
    client = TestClient(app)
    
    # WHEN: user registers
    reg_response = client.post("/register", json={
        "email": "newuser@example.com",
        "password": "SecurePass123!"
    })
    assert reg_response.status_code == 201
    
    # AND: receives confirmation email
    email = get_last_email()  # Helper function
    assert "Confirm your registration" in email.body
    
    # AND: clicks confirmation link
    confirm_token = extract_token_from_email(email)
    confirm_response = client.get(f"/confirm/{confirm_token}")
    assert confirm_response.status_code == 200
    
    # AND: logs in with credentials
    login_response = client.post("/login", json={
        "email": "newuser@example.com",
        "password": "SecurePass123!"
    })
    
    # THEN: receives valid JWT token
    assert login_response.status_code == 200
    token = login_response.json()["token"]
    assert jwt.decode(token, verify=True)
    
    # AND: can access protected endpoints
    profile_response = client.get(
        "/profile",
        headers={"Authorization": f"Bearer {token}"}
    )
    assert profile_response.status_code == 200
```

**Claude Code Prompt:**
```bash
claude "Create end-to-end test for complete user registration flow

Test flow:
1. User registers → receives confirmation email
2. User clicks confirmation link → account activated
3. User logs in → receives JWT token
4. User accesses protected endpoint → authorized

Requirements:
- Mock email sending
- Use test database
- Test complete happy path
- Verify each step
- File: tests/e2e/test_registration_flow.py"
```

---

## 🎯 The Prompt Patterns That Enforce Evidence

### Pattern 1: "Test-First" Prompt
```bash
claude "Feature: [X]

IMPORTANT: Follow test-first development:
1. First, create ONLY tests in tests/test_[x].py
2. Run tests - they should FAIL
3. Then, implement feature to make tests PASS
4. Run tests again - they should all PASS

Do NOT implement the feature until tests exist and fail."
```

---

### Pattern 2: "Evidence Required" Prompt
```bash
claude "Implement [X]

Requirements:
- Tests must exist BEFORE implementation
- Test coverage must be >80%
- All tests must pass
- Must include integration test

Show me:
1. Test file created
2. Test results (failed initially)
3. Implementation
4. Test results (passed after implementation)
5. Coverage report"
```

---

### Pattern 3: "Prove It Works" Prompt
```bash
claude "Implement [X]

After implementation, provide EVIDENCE that it works:
1. Unit tests passing (screenshot/output)
2. Integration tests passing
3. Manual verification script/commands
4. Example usage with expected output

I will not accept 'implemented' without proof."
```

---

### Pattern 4: "Red-Green-Refactor" Prompt
```bash
claude "Feature: [X]

Follow Red-Green-Refactor cycle:

RED: Create tests that fail
- File: tests/test_[x].py
- Run: pytest tests/test_[x].py
- Show me: Test failures

GREEN: Make tests pass (minimal implementation)
- File: src/[x].py
- Run: pytest tests/test_[x].py
- Show me: Tests passing

REFACTOR: Clean up code (tests still pass)
- Improve code quality
- Add docstrings
- Run: pytest tests/test_[x].py
- Show me: Tests still passing

Do these three phases explicitly."
```

---

## 🔍 The Verification Checklist

After Claude Code completes ANY implementation, verify:

```bash
# 1. Tests exist
ls tests/test_[feature].py  # File exists?

# 2. Tests are comprehensive
cat tests/test_[feature].py  # Covers scenarios?

# 3. Tests pass
pytest tests/test_[feature].py -v  # All green?

# 4. Coverage is adequate
pytest tests/ --cov=[module] --cov-report=term-missing
# >80% coverage?

# 5. No regressions
pytest tests/ -v  # All old tests still pass?

# 6. Integration works
pytest tests/integration/ -v  # Components work together?

# IF ANY FAIL → Feature is NOT done
```

---

## 🚨 The "No Test, No Merge" Policy

**Add this to your project's README:**

```markdown
## Testing Policy

**No code merges to main without:**
1. ✅ Tests written BEFORE implementation
2. ✅ All tests passing (pytest tests/ -v)
3. ✅ Coverage >80% for new code
4. ✅ Integration tests for multi-component features
5. ✅ No regressions (all existing tests pass)

**To merge a feature:**
1. Create feature branch
2. Write tests (should fail)
3. Implement feature (tests pass)
4. Run full test suite
5. Create PR with test evidence
```

**Enforce this in pre-commit hook:**
```bash
# .git/hooks/pre-commit
#!/bin/bash
pytest tests/ -v --cov=src --cov-fail-under=80
if [ $? -ne 0 ]; then
    echo "❌ Tests failed or coverage <80%. Commit blocked."
    exit 1
fi
```

---

## 🎓 Training Claude Code to Be Test-First

### Create a `.clauderules` file in your project:

```markdown
# .clauderules

## Test-First Development (Non-Negotiable)

**For EVERY feature or bug fix:**

1. **Tests First:** Always create tests BEFORE implementation
   - Create tests/test_[feature].py
   - Tests should initially FAIL
   - Show test failure output

2. **Implementation:** Only after tests exist
   - Implement ONLY what tests require
   - No extra features beyond test coverage

3. **Verification:** Always prove it works
   - Show test passing output
   - Show coverage report
   - Verify no regressions

**Example workflow:**
```bash
# Step 1: Tests (should fail)
Create tests/test_new_feature.py
Run: pytest tests/test_new_feature.py -v
Output: 5 failed

# Step 2: Implement
Implement src/new_feature.py
Run: pytest tests/test_new_feature.py -v
Output: 5 passed

# Step 3: Verify no regressions
Run: pytest tests/ -v
Output: All tests passed
```

**This workflow is MANDATORY for all code changes.**
```

---

## 🏗️ End-to-End Feature Implementation Template

### The Complete Evidence-Driven Workflow:

```bash
# PHASE 1: REQUIREMENTS & DESIGN (Chat)
[CHAT] "Design a [feature] with:
- User requirements
- API specification
- Data models
- Test scenarios
- Success criteria

Output as a structured document"

# PHASE 2: TEST SPECIFICATION (Code)
claude "Based on the design, create test specifications:

File: tests/specs/[feature]_spec.md

Include:
- Unit test scenarios
- Integration test scenarios
- E2E test scenarios
- Test data requirements
- Expected outcomes for each test"

# PHASE 3: UNIT TESTS (Code - Test First)
claude "Create unit tests in tests/unit/test_[feature].py

Based on: tests/specs/[feature]_spec.md

Requirements:
- Test each function independently
- Use mocks for dependencies
- Tests should FAIL initially
- Clear GIVEN/WHEN/THEN structure"

# Verify tests fail
pytest tests/unit/test_[feature].py -v
# Expected: All tests FAIL (feature not implemented)

# PHASE 4: UNIT IMPLEMENTATION (Code)
claude "Implement [feature] in src/[feature].py
to make tests/unit/test_[feature].py PASS

Requirements:
- Follow existing code patterns
- Type hints on all functions
- Docstrings
- Implement ONLY what tests require"

# Verify tests pass
pytest tests/unit/test_[feature].py -v
# Expected: All tests PASS

# PHASE 5: INTEGRATION TESTS (Code - Test First)
claude "Create integration tests in tests/integration/test_[feature].py

Based on: tests/specs/[feature]_spec.md

Test:
- API endpoints + database
- Component interactions
- Error handling across layers

Tests should FAIL initially"

# Verify integration tests fail
pytest tests/integration/test_[feature].py -v
# Expected: Tests FAIL

# PHASE 6: INTEGRATION IMPLEMENTATION (Code)
claude "Complete integration layer for [feature]
to make tests/integration/test_[feature].py PASS

Requirements:
- Wire components together
- Add error handling
- Add logging
- Implement ONLY what tests require"

# Verify integration tests pass
pytest tests/integration/test_[feature].py -v
# Expected: All tests PASS

# PHASE 7: E2E TESTS (Code - Test First)
claude "Create end-to-end test in tests/e2e/test_[feature]_flow.py

Test complete user workflow:
[describe full workflow]

Should FAIL initially"

# PHASE 8: E2E COMPLETION (Code)
claude "Complete any missing pieces for E2E test to pass
(usually just wiring, all components already implemented)"

# Verify E2E passes
pytest tests/e2e/test_[feature]_flow.py -v

# PHASE 9: FULL VERIFICATION (Code)
claude "Run complete test suite with coverage:

Commands to run:
1. pytest tests/ -v (all tests)
2. pytest tests/ --cov=src --cov-report=html
3. Show me coverage report
4. Identify any gaps"

# PHASE 10: REVIEW (Chat)
[CHAT] Upload: Coverage report, test results, new code

"Review this implementation:
1. Is test coverage adequate?
2. Are there edge cases not tested?
3. Are there security concerns?
4. Are there performance issues?

Provide specific recommendations"

# PHASE 11: HARDENING (Code)
claude "Based on review feedback:
[list specific improvements]

Add tests for any new scenarios identified"

# PHASE 12: DOCUMENTATION (Code)
claude "Update documentation:
1. Add docstrings to public functions
2. Update API_SPEC.yaml if API changed
3. Update README.md with new feature
4. Add examples to docs/EXAMPLES.md"

# PHASE 13: FINAL VERIFICATION (Code)
pytest tests/ -v --cov=src --cov-report=term
# Everything passes, >80% coverage

git add .
git commit -m "feat: [feature] with comprehensive tests"
```

---

## 🎯 Context-Specific Evidence Requirements

### EXPLORATION Context
**Evidence needed:** Investigation findings, not code tests
```bash
claude "Create investigation evidence in investigations/[date]_[issue].md:
- Reproduction steps
- Minimal test case
- Hypotheses tested (with results)
- Root cause (if found)
- Fix verification (if applied)"
```

### FIREFIGHTING Context
**Evidence needed:** Bug reproduction + fix verification
```bash
claude "For hotfix:
1. Create tests/hotfix/test_[bug].py that reproduces bug
2. Run test - should FAIL (proves bug exists)
3. Fix the bug
4. Run test - should PASS (proves bug fixed)
5. Run full suite - should PASS (proves no regressions)"
```

### BUILDING Context
**Evidence needed:** Full test pyramid
```bash
claude "For new feature:
- Unit tests (must pass)
- Integration tests (must pass)
- E2E test (must pass)
- Coverage >80%
- All existing tests still pass"
```

### IMPROVING Context
**Evidence needed:** Before/after measurements
```bash
claude "For improvement:
1. Benchmark BEFORE (performance/coverage/complexity)
2. Make improvement
3. Benchmark AFTER
4. All tests still pass
5. Show measurable improvement"
```

---

## 📊 Evidence Dashboard

Create a simple script to see test health:

```bash
# scripts/test_dashboard.sh
#!/bin/bash

echo "========================================="
echo "  TEST EVIDENCE DASHBOARD"
echo "========================================="
echo ""

echo "🧪 Test Counts:"
echo "Unit: $(find tests/unit -name '*.py' -exec grep -l 'def test_' {} \; | wc -l) files"
echo "Integration: $(find tests/integration -name '*.py' -exec grep -l 'def test_' {} \; | wc -l) files"
echo "E2E: $(find tests/e2e -name '*.py' -exec grep -l 'def test_' {} \; | wc -l) files"
echo ""

echo "📊 Test Results (last run):"
pytest tests/ -v --tb=no | tail -1
echo ""

echo "📈 Coverage:"
pytest tests/ --cov=src --cov-report=term | grep "TOTAL"
echo ""

echo "⚠️ Uncovered Files:"
pytest tests/ --cov=src --cov-report=term-missing | grep "0%"
echo ""

echo "========================================="
```

**Run daily:**
```bash
bash scripts/test_dashboard.sh
```

---

## 🎓 Practice Exercise: Evidence-Driven Feature

**Build a complete feature test-first:**

```bash
# Day 1: Tests only (no implementation)
1. Create tests/unit/test_csv_validator.py (should fail)
2. Create tests/integration/test_csv_import_api.py (should fail)
3. Create tests/e2e/test_complete_import_flow.py (should fail)
4. Commit: "tests: CSV import test suite (failing)"

# Day 2: Implementation
5. Implement to make unit tests pass
6. Commit: "feat: CSV validator"
7. Implement to make integration tests pass
8. Commit: "feat: CSV import API"
9. Complete E2E test
10. Commit: "feat: CSV import complete"

# Day 3: Verification and hardening
11. Run full test suite (all should pass)
12. Check coverage (should be >80%)
13. Add any missing edge case tests
14. Commit: "test: additional edge cases"
```

**Evidence required at each commit:**
- Test output (pass/fail)
- Coverage report
- No regressions

---

**Next:** Read `04_CONTEXT_PROTOCOLS.md` for detailed step-by-step protocols for each context type.
