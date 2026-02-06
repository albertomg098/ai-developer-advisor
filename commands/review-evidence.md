Review the current evidence state for the project or a specific feature.

Read the skill at `skills/test-first/SKILL.md` for evidence requirements.

Follow this process:

1. **Run the test suite**:
   - `pytest tests/ -v` — show all test results
   - `pytest tests/ --cov=src --cov-report=term-missing` — show coverage

2. **Assess against the Evidence Pyramid**:
   ```
   □ Requirements documented
   □ Tests exist (and are meaningful)
   □ Unit tests pass
   □ Integration tests pass
   □ E2E tests pass (if applicable)
   □ Coverage >80%
   □ No regressions
   ```

3. **Identify gaps**:
   - Files with <80% coverage
   - Missing test types (unit/integration/e2e)
   - Untested error paths
   - Missing edge cases

4. **Report readiness**:
   ```
   ## Evidence Report

   **Tests:** X passing, Y failing, Z skipped
   **Coverage:** XX% (target: 80%)
   **Pyramid Status:**
   - ✅/❌ Unit tests
   - ✅/❌ Integration tests
   - ✅/❌ E2E tests
   - ✅/❌ No regressions

   **Gaps:**
   - [List uncovered areas]

   **Verdict:** READY TO MERGE / NEEDS WORK
   ```

5. **Suggest improvements**: If not ready, list specific tests to add.

If a specific feature is specified: $ARGUMENTS — focus the review on that feature's tests.
