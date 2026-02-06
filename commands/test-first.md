Guide the user through test-first implementation of a feature or fix.

Read the skill at `skills/test-first/SKILL.md` for the methodology.

Follow the three-phase process with explicit pauses between phases:

## PHASE 1 — TEST SPECIFICATION

1. **Clarify** what's being built/fixed (from user description or $ARGUMENTS)
2. **Define test scenarios**:
   - Happy path: expected behavior with valid input
   - Edge cases: boundary conditions, empty inputs, large inputs
   - Error cases: invalid input, missing data, auth failures
3. **Create test file** at `tests/test_[name].py` (or appropriate subdirectory):
   - Use pytest with descriptive names: `test_should_[action]_when_[condition]`
   - GIVEN/WHEN/THEN structure for each test
   - Fixtures for test data
4. **Run tests** — they should ALL FAIL (feature not implemented)
5. **Show results** — confirm failures prove tests are meaningful

**PAUSE**: Show test results and ask user to confirm before proceeding.

## PHASE 2 — IMPLEMENTATION

1. **Implement ONLY what tests require** — no extras
2. **Follow existing code patterns** in the codebase
3. **Run tests** — they should ALL PASS
4. **Show results** — confirm all tests green

**PAUSE**: Show test results and ask user to confirm before proceeding.

## PHASE 3 — VERIFICATION

1. **Run full test suite**: `pytest tests/ -v`
2. **Check coverage**: `pytest tests/ --cov=[module] --cov-report=term-missing`
3. **Report**:
   - Total tests passing
   - Coverage percentage (target: >80%)
   - Any regressions
   - Uncovered code paths
4. **Verdict**: Feature is DONE or needs more work

Feature/fix description: $ARGUMENTS
