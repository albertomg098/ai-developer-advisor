# Context Protocols - Step-by-Step Workflows
## Detailed protocols for each context type with real examples

---

## 🔍 EXPLORATION Context Protocol

**When to use:** "Something's broken/weird and I don't know why"

### Step-by-Step Protocol:

```bash
# ===== SESSION START =====

# Step 1: Create investigation file (30 seconds)
claude "Create investigations/$(date +%Y%m%d)_[short_description].md
using the universal context template for EXPLORATION"

# Example output: investigations/20250205_voice_mp3_failure.md

# Step 2: Document what you know (2 minutes)
claude "Update investigations/[file].md with:
## What's Broken
[Symptoms you're seeing]

## What Works (for comparison)
[Similar things that DO work]

## Context
[What changed recently, if anything]"

# Step 3: Generate diagnostic plan (5 minutes - use CHAT)
[CHAT] "I have this problem: [describe].

What I know:
- [what works]
- [what doesn't]
- [recent changes]

Create a diagnostic plan:
1. List possible root causes (ranked by likelihood)
2. Create tests to isolate each cause
3. Suggest what evidence would confirm each cause"

# Step 4: Test hypotheses systematically (15-30 min each)
claude "For Hypothesis #1: [hypothesis from chat]

Create tests/debug/test_hypothesis_1.py that:
- Sets up the exact failing scenario
- Tests if hypothesis is true
- Prints detailed diagnostic output

Run it and show results"

# Step 5: Update investigation log after EACH test
claude "Append to investigations/[file].md:
## Hypothesis #1: [hypothesis]
Test: tests/debug/test_hypothesis_1.py
Result: [CONFIRMED/REJECTED/UNCLEAR]
Evidence: [what we learned]
Conclusion: [what this tells us]"

# Step 6: Continue until root cause found OR blocked
# If found: Move to FIREFIGHTING or BUILDING mode
# If blocked: Document and ask for help

# ===== SESSION END =====
git add investigations/
git commit -m "investigation: [issue] - [current status]"
```

---

### EXPLORATION Example: Voice MP3 Detection Failure

```bash
# Session 1: Discovery
claude "Create investigations/20250205_voice_mp3_failure.md"

claude "Update with:
## What's Broken
MP3 files >10MB fail voice detection with no error

## What Works
- WAV files of any size work perfectly
- MP3 files <10MB work fine
- All other audio processing works

## Context
- No code changes recently
- Happening in production since yesterday
- No errors in logs"

# Move to CHAT for analysis
[CHAT] "Help me create diagnostic plan for: [paste above]"

# CHAT returns hypotheses:
# 1. File size limit somewhere (70% likely)
# 2. Memory issue with large MP3 decoding (20% likely)
# 3. Timeout on processing (10% likely)

# Test Hypothesis #1
claude "Create tests/debug/test_file_size_limits.py:
- Test 5MB MP3 (should work based on symptoms)
- Test 15MB MP3 (should fail)
- Test 15MB WAV (should work)
- Check for any size-related config
Show results"

# Results: 5MB MP3 ALSO fails! Not a size limit.

claude "Update investigations/20250205_voice_mp3_failure.md:
## Hypothesis #1: File size limit
Test: tests/debug/test_file_size_limits.py
Result: REJECTED
Evidence: 5MB MP3 also fails, so not size-related
Conclusion: The issue is MP3 format itself, not file size"

# New insight: ALL MP3s fail, not just large ones
# Move to Hypothesis #2 (now #1)

claude "Create tests/debug/test_mp3_decoder.py:
- Try decoding MP3 with pydub
- Try decoding MP3 with librosa
- Try decoding MP3 with audioread
- Print which libraries are available
Show results"

# Results: pydub fails with "ffmpeg not found"!

claude "Update investigations/20250205_voice_mp3_failure.md:
## Root Cause FOUND
pydub requires ffmpeg, but it's not installed in production

## Fix Required
Install ffmpeg in production environment

## Verification
After ffmpeg installed, run tests/debug/test_mp3_decoder.py
Should pass"

# TRANSITION to FIREFIGHTING mode (critical fix needed)
```

---

## 🔥 FIREFIGHTING Context Protocol

**When to use:** "Critical bug, must fix NOW"

### Step-by-Step Protocol:

```bash
# ===== EMERGENCY START =====

# Step 1: Assess severity (1 minute)
# How many users affected?
# What's the business impact?
# Can it wait or not?

# Step 2: Create hotfix context (1 minute)
claude "Create contexts/active/hotfix_[bug].md with:
Status: 🔴 CRITICAL
Impact: [users affected, services down]
First detected: [time]
Outcome: [bug fixed and deployed within X hours]"

# Step 3: Stop feature work if needed (1 minute)
git stash  # Save any uncommitted work

# Step 4: Create hotfix branch
git checkout -b hotfix/[bug-name]

# Step 5: Reproduce bug in test (10 minutes)
claude "Create tests/hotfix/test_[bug].py that:
- Reproduces the EXACT bug we're seeing
- Should FAIL currently (proves bug exists)
- Clear expected vs actual behavior
- Uses minimal setup"

pytest tests/hotfix/test_[bug].py -v
# Should FAIL - this is proof of bug

# Step 6: Fix with MINIMAL scope (20-40 minutes)
claude "Fix [file] to make tests/hotfix/test_[bug].py PASS

CONSTRAINTS:
- Change ONLY what's needed
- No refactoring
- No additional features
- No cleanup of unrelated code

Just make the test pass."

# Step 7: Verify fix (5 minutes)
pytest tests/hotfix/test_[bug].py -v  # Should PASS
pytest tests/ -v  # No regressions

# Step 8: Deploy immediately
git add .
git commit -m "hotfix: [brief description]"
git push origin hotfix/[bug-name]

# Step 9: Monitor in production (30 minutes)
# Watch logs, metrics, user reports

# Step 10: Update hotfix context
claude "Update contexts/active/hotfix_[bug].md:
Status: ✅ FIXED
Fix: [what we changed]
Deployed: [time]
Verification: [how we confirmed it works]"

# Step 11: Archive and return
mv contexts/active/hotfix_[bug].md \
   contexts/archive/$(date +%Y%m%d)_[bug]_fixed.md

git checkout [previous-branch]
git stash pop  # Resume previous work

# ===== FIREFIGHTING COMPLETE =====
```

---

### FIREFIGHTING Example: Production 500 Errors

```bash
# CRITICAL: API returning 500 errors, production down

# Step 1: Immediate triage
# Impact: 100% of API users (10,000 users)
# First detected: 10:15 AM
# Severity: CRITICAL - immediate action needed

# Step 2: Create hotfix
claude "Create contexts/active/hotfix_prod_500_errors.md"
git checkout -b hotfix/prod-500-errors

# Step 3: Gather error info quickly
tail -f logs/api.log | grep "500"
# Error: "AttributeError: 'NoneType' object has no attribute 'get'"
# File: src/api/auth.py line 45

# Step 4: Reproduce in test
claude "Create tests/hotfix/test_prod_500.py:
- Call the failing endpoint: POST /api/users
- With valid auth token
- Should return 500 (proves bug)
- Capture exact error message"

pytest tests/hotfix/test_prod_500.py -v
# FAILED: AttributeError at line 45

# Step 5: Quick investigation (use CHAT)
[CHAT] Upload: src/api/auth.py
"Line 45 is causing AttributeError on None.
What's the most likely cause and quick fix?"

# CHAT: "Line 45 assumes user object exists,
# but database query might return None if session expired"

# Step 6: Minimal fix
claude "Fix src/api/auth.py line 45:
Add None check before calling .get()
If None, raise 401 Unauthorized (not 500)

Make tests/hotfix/test_prod_500.py pass"

# Step 7: Verify
pytest tests/hotfix/test_prod_500.py -v  # PASSED
pytest tests/ -v  # All pass

# Step 8: Emergency deploy
git add .
git commit -m "hotfix: handle None user in auth (fixes 500 errors)"
git push
# Deploy script runs automatically

# Step 9: Monitor
# 10:45 AM: Error rate dropped from 40% to <1%
# 11:00 AM: Confirmed stable

# Step 10: Document
claude "Update contexts/active/hotfix_prod_500_errors.md:
Status: ✅ RESOLVED
Root cause: Expired sessions not handled, returned None
Fix: Added None check and return 401 instead of crashing
Deployed: 10:45 AM
Impact: Error rate <1%, production stable"

# Step 11: Post-mortem (later)
[CHAT] "Create post-mortem for prod 500 errors.
Include:
- Timeline
- Root cause
- Fix
- Prevention (what should we change?)
- Monitoring (what alerts to add?)"

# Archive
mv contexts/active/hotfix_prod_500_errors.md \
   contexts/archive/20250205_prod_500_fixed.md

# CRISIS OVER - back to normal work
```

---

## 🏗️ BUILDING Context Protocol

**When to use:** "I know what to build and how to build it"

### Step-by-Step Protocol:

```bash
# ===== SESSION START =====

# Step 1: Load context (2 minutes)
claude "Read contexts/active/feature_[name].md
Summarize:
- What we're building
- What's done so far
- What's next"

# If starting new feature:
claude "Create contexts/active/feature_[name].md with:
Status: 🟢 Active
Branch: feature/[name]
Outcome: [What does 'done' look like?]
Success Criteria:
- [ ] Tests pass
- [ ] Feature works as specified
- [ ] Documentation updated"

# Step 2: Create feature branch
git checkout -b feature/[name]

# Step 3: Design phase (CHAT - 10 minutes)
[CHAT] "Design [feature] with:
- API specification
- Data models
- Test scenarios
- Integration points

Output as structured spec"

# Step 4: Write test specifications (CODE - 10 minutes)
claude "Based on design, create tests/specs/[feature]_spec.md:
## Unit Tests
[list scenarios]

## Integration Tests
[list scenarios]

## E2E Tests
[list workflows]"

# Step 5: Unit tests FIRST (CODE - 20 minutes)
claude "Create tests/unit/test_[feature].py
Based on: tests/specs/[feature]_spec.md

Requirements:
- Tests should FAIL initially
- Clear GIVEN/WHEN/THEN
- Use fixtures for test data"

pytest tests/unit/test_[feature].py -v
# Should see: X failed tests

# Step 6: Unit implementation (CODE - 30-60 minutes)
claude "Implement src/[feature].py
to make tests/unit/test_[feature].py PASS"

pytest tests/unit/test_[feature].py -v
# Should see: X passed tests

# Step 7: Commit unit layer
git add .
git commit -m "feat([feature]): unit layer complete"

# Step 8: Integration tests (CODE - 20 minutes)
claude "Create tests/integration/test_[feature].py
Test interaction with [database/API/other components]"

pytest tests/integration/test_[feature].py -v
# Should fail initially

# Step 9: Integration implementation (CODE - 30-60 minutes)
claude "Complete integration layer for [feature]"

pytest tests/integration/test_[feature].py -v
# Should pass

# Step 10: Commit integration layer
git add .
git commit -m "feat([feature]): integration layer complete"

# Step 11: E2E test (CODE - 20 minutes)
claude "Create tests/e2e/test_[feature]_flow.py
Test complete user workflow: [describe]"

pytest tests/e2e/test_[feature]_flow.py -v
# Should initially fail

# Step 12: E2E completion (CODE - 20 minutes)
claude "Complete any wiring needed for E2E test"

pytest tests/e2e/test_[feature]_flow.py -v
# Should pass

# Step 13: Full validation gate
# This runs tests, validates real inputs, writes evidence,
# updates context, and offers to commit/PR — all in one command
/ai-dev-advisor:review-evidence

# If PASS: review-evidence offers to commit and create PR
# If FAIL: shows gaps and suggests fixes — fix them, then re-run

# Step 14: Review (CHAT - optional, if you want extra eyes)
[CHAT] Upload: evidence/[context]-validation.md
"Review this evidence report for issues"

# Step 15: Ship
# When review-evidence says PASS and you approve,
# it commits, creates PR, and archives the context

# ===== SESSION END =====
```

---

### BUILDING Example: Bulk CSV Export Feature

```bash
# Starting new feature from scratch

# Step 1: Create context
claude "Create contexts/active/feature_bulk_csv_export.md:
Status: 🟢 Active
Outcome: Users can export user data as CSV
Success Criteria:
- [ ] Unit tests pass (CSV generation)
- [ ] Integration tests pass (API endpoint)
- [ ] E2E test passes (full export flow)
- [ ] Coverage >80%"

git checkout -b feature/bulk-csv-export

# Step 2: Design in CHAT
[CHAT] "Design CSV export feature:
Input: list of user IDs
Output: CSV file with user data
Fields: id, email, name, created_at

Design:
- API endpoint specification
- CSV generation logic
- Error handling
- Test scenarios"

# CHAT outputs detailed design

# Step 3: Test specs
claude "Create tests/specs/csv_export_spec.md
from the design above"

# Step 4: Unit tests
claude "Create tests/unit/test_csv_generator.py:
- test_generates_csv_with_headers
- test_formats_dates_correctly
- test_handles_empty_list
- test_handles_special_characters_in_names
- test_handles_missing_fields

Tests should FAIL"

pytest tests/unit/test_csv_generator.py -v
# 5 failed

# Step 5: Implement CSV generator
claude "Implement src/services/csv_generator.py
to make all unit tests pass"

pytest tests/unit/test_csv_generator.py -v
# 5 passed

git commit -m "feat(export): CSV generator with unit tests"

# Step 6: Integration tests
claude "Create tests/integration/test_export_api.py:
- test_export_endpoint_returns_csv
- test_export_requires_authentication
- test_export_filters_by_org_id (multi-tenant)
- test_export_handles_large_datasets

Tests should FAIL"

pytest tests/integration/test_export_api.py -v
# 4 failed

# Step 7: Implement API endpoint
claude "Create src/api/export.py with:
- GET /api/users/export endpoint
- Uses CSVGenerator service
- Requires auth
- Filters by org_id
- Streams large files

Make integration tests pass"

pytest tests/integration/test_export_api.py -v
# 4 passed

git commit -m "feat(export): API endpoint with integration tests"

# Step 8: E2E test
claude "Create tests/e2e/test_export_flow.py:
Complete workflow:
1. User logs in
2. Requests CSV export
3. Receives download link
4. Downloads CSV file
5. Verifies data correctness

Should FAIL initially"

pytest tests/e2e/test_export_flow.py -v
# 1 failed (minor wiring issue)

# Step 9: Complete E2E
claude "Fix the wiring issue to make E2E pass"

pytest tests/e2e/test_export_flow.py -v
# 1 passed

git commit -m "feat(export): complete E2E workflow"

# Step 10: Full validation gate
/ai-dev-advisor:review-evidence
# Runs tests, validates fixtures, writes evidence/feature_csv_export-validation.md,
# updates context, asks to commit/PR

# If review identifies gaps (e.g., "Add rate limiting"):
claude "Add rate limiting to export endpoint:
- Max 10 exports per hour per user
- Return 429 if exceeded
- Add test for rate limiting"

# Re-run validation gate
/ai-dev-advisor:review-evidence
# PASS → "Commit and create PR?" → yes → done

# FEATURE DONE
```

---

## ✨ IMPROVING Context Protocol

**When to use:** "It works, but could be better"

### Step-by-Step Protocol:

```bash
# ===== SESSION START =====

# Step 1: Identify improvements (CHAT - 10 minutes)
[CHAT] "Analyze codebase for improvements:
- Low test coverage areas
- Performance bottlenecks
- Code duplication
- Missing documentation
- Tech debt

Prioritize by impact vs effort"

# Step 2: Create improvement backlog
claude "Create contexts/backlog/improvements.md:
## Quick Wins (<1 hour)
- [ ] [item] - Impact: [low/med/high]

## Medium Tasks (<1 day)
- [ ] [item] - Impact: [low/med/high]

## Large Efforts (>1 day)
- [ ] [item] - Impact: [low/med/high]"

# Step 3: Pick ONE quick win
claude "Move [improvement] to contexts/active/improve_[name].md
Status: 🟢 Active
Outcome: [specific improvement]
Measurement: [how we'll measure success]"

# Step 4: Baseline measurement (CODE - 5 minutes)
claude "Measure current state:
- If performance: benchmark current performance
- If coverage: show current coverage percentage
- If refactoring: measure code complexity

Save to contexts/active/improve_[name].md under '## Before'"

# Step 5: Make improvement (CODE - variable time)
claude "Implement [improvement]
CONSTRAINT: All existing tests must still pass"

# Step 6: After measurement (CODE - 5 minutes)
claude "Measure after improvement:
- Same metrics as before
- Show improvement

Save to contexts/active/improve_[name].md under '## After'"

# Step 7: Full validation gate
/ai-dev-advisor:review-evidence
# Runs tests, compares before/after baselines, writes evidence,
# updates context to ✅ Done, offers to commit/PR

# ===== SESSION END =====
```

---

### IMPROVING Example: Increase Test Coverage

```bash
# Current coverage: 65% (below our 80% target)

# Step 1: Identify gaps
[CHAT] Upload: coverage report
"Identify files with <80% coverage.
Prioritize by importance and ease of testing"

# CHAT returns:
# 1. src/services/auth_service.py (45% coverage) - HIGH priority
# 2. src/api/webhooks.py (30% coverage) - MED priority
# 3. src/utils/formatters.py (0% coverage) - LOW priority

# Step 2: Create improvement context
claude "Create contexts/active/improve_auth_coverage.md:
Status: 🟢 Active
Outcome: auth_service.py coverage >80%
Current: 45%
Target: 80%"

# Step 3: Baseline
pytest tests/ --cov=src.services.auth_service --cov-report=term-missing
# 45% coverage, missing: lines 23-45, 67-89

# Step 4: Add missing tests
claude "Analyze src/services/auth_service.py
Add tests for uncovered code paths:
- Lines 23-45: Token refresh logic
- Lines 67-89: Password reset logic

Create tests in tests/unit/test_auth_service_coverage.py"

# Step 5: Verify improvement
pytest tests/ --cov=src.services.auth_service --cov-report=term-missing
# 82% coverage (improvement: +37%)

# Step 6: Update context
claude "Update contexts/active/improve_auth_coverage.md:
Status: ✅ COMPLETE
Before: 45% coverage
After: 82% coverage
Improvement: +37 percentage points
Tests added: 8 new tests"

# Archive
mv contexts/active/improve_auth_coverage.md \
   contexts/archive/20250205_auth_coverage_improved.md

# IMPROVEMENT COMPLETE
# Move to next item in backlog
```

---

## 🔄 Context Switching Protocol

**What to do when switching between contexts mid-session:**

```bash
# You're in BUILDING mode, working on feature_export.md
# Suddenly: Critical bug discovered!

# === PAUSE CURRENT CONTEXT ===

# Step 1: Checkpoint current work (1 minute)
git add .
git commit -m "WIP: export feature - pausing for hotfix"

# Step 2: Update current context
claude "Update contexts/active/feature_export.md:
Status: 🟡 PAUSED (critical bug)
Paused at: [timestamp]
State: Completed unit tests, starting integration layer
Resume point: Create tests/integration/test_export_api.py"

# === SWITCH TO FIREFIGHTING ===

# Step 3: Create hotfix context
claude "Create contexts/active/hotfix_[bug].md
Note under 'Context': Paused feature_export.md for this"

# Step 4: Switch branch
git checkout -b hotfix/[bug]

# Step 5: Work on hotfix (follow FIREFIGHTING protocol)
# ... fix the bug ...

# === COMPLETE FIREFIGHTING ===

# Step 6: Merge hotfix
git checkout main
git merge hotfix/[bug]

# Step 7: Archive hotfix context
mv contexts/active/hotfix_[bug].md \
   contexts/archive/$(date +%Y%m%d)_[bug]_fixed.md

# === RESUME BUILDING ===

# Step 8: Return to feature branch
git checkout feature/export

# Step 9: Update feature context
claude "Update contexts/active/feature_export.md:
Status: 🟢 Active (resumed)
Resumed at: [timestamp]
Read 'Resume point' and continue"

# Step 10: Continue where you left off
claude "Read contexts/active/feature_export.md
Continue from 'Resume point': Create integration tests"

# BACK IN BUILDING MODE
```

---

**Next:** Read `05_SKILLS_COMMANDS_HOOKS.md` to learn advanced Claude Code features.
