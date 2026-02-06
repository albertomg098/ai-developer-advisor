Complete validation gate — runs all checks, writes evidence, updates context, and offers to ship.

Read the skill at `skills/test-first/SKILL.md` for evidence requirements.

---

## Step 1: DETECT CONTEXT

Identify the active context for this validation:

1. Check the current git branch: `git branch --show-current`
2. Look for a matching context file in `contexts/active/` where the `Branch:` field matches
3. If no match found, list all `contexts/active/*.md` files and ask which one this validation is for
4. Read the full context file — you'll need the outcome goal, success criteria, and session log

Announce:
```
📋 Validating context: [context name]
🌿 Branch: [branch]
🎯 Goal: [outcome goal from context file]
```

---

## Step 2: RUN TESTS

Run the project's full test suite. **STOP here if any test fails.**

1. **Discover the test runner:**
   - Python with `src/` layout: `PYTHONPATH=src pytest tests/ -v --tb=long`
   - Python standard: `pytest tests/ -v --tb=long`
   - Node: `npm test` or `npx jest --verbose`
   - Other: check `package.json`, `Makefile`, or project README

2. **Run with verbose output** — show all test names and results

3. **Run with coverage:**
   - Python: `pytest tests/ -v --cov=[source-dir] --cov-report=term-missing`
   - Node: `npx jest --coverage`

4. **If ANY test fails** — show full output, suggest specific fixes, and STOP. Do NOT proceed.

Report:
```
## 🧪 Test Results
- Total: X tests
- ✅ Passing: X
- ❌ Failing: X
- ⏭️ Skipped: X
- Coverage: XX% (target: 80%)
- Uncovered files: [list files below 80%]
```

---

## Step 3: VALIDATE WITH REAL INPUTS

Check if the project has real test data for integration validation:

1. **Look for fixture/data directories:**
   - `tests/fixtures/`, `tests/data/`, `tests/samples/`
   - `tests/fixtures/documents/`, `tests/fixtures/inputs/`
   - Any directory with sample inputs mentioned in the context file

2. **If fixtures exist:**
   - Identify which adapter/pipeline/module the feature branch modified (from `git diff` against base branch)
   - Process each relevant fixture through the modified code path
   - Capture full output — show COMPLETE results, never summarize

3. **If no fixtures exist:**
   - Note: "⚠️ No integration fixtures found — unit tests only"
   - Suggest creating a fixtures directory for future validation
   - Proceed to Step 4

Report:
```
## 📄 Real Input Validation
- Fixtures directory: [path]
- Files processed: X
| File | Status | Key Output |
|------|--------|------------|
| [name] | ✅/❌ | [summary of extraction/result] |
```

---

## Step 4: BEFORE/AFTER COMPARISON

Compare current results against baselines:

1. **Look for expected outputs:**
   - `tests/fixtures/expected_outputs/`, `tests/expected/`
   - Baseline measurements recorded in the context file
   - Previous evidence files in `evidence/`

2. **If baselines exist:**
   - Compare field by field (structured output) or line by line (text output)
   - Flag regressions (❌ worse than baseline)
   - Flag improvements (✅ better than baseline)
   - Flag unchanged (➖ same as baseline)

3. **If no baselines exist:**
   - Note: "📌 No baselines found — establishing current output as first baseline"
   - Save current outputs as the baseline for future comparison
   - Proceed to Step 5

Report:
```
## 📊 Before/After Comparison
| Input | Field | Before | After | Status |
|-------|-------|--------|-------|--------|
| [file] | [field] | [value] | [value] | ✅/➖/❌ |

Regressions: X | Improvements: X | Unchanged: X
```

---

## Step 5: WRITE EVIDENCE FILE

Create the `evidence/` directory if it doesn't exist.

Write `evidence/[context-name]-validation.md` with the COMPLETE report:

```markdown
# Validation Evidence: [Context Name]

**Date:** YYYY-MM-DD HH:MM
**Branch:** [branch]
**Context:** [path to context file]
**Validator:** review-evidence

---

## Test Results

[Full verbose test output — NEVER summarize, show COMPLETE output]

## Coverage Report

[Full coverage report with missing lines — NEVER summarize]

## Real Input Validation

[Full results from Step 3 — NEVER summarize]

## Before/After Comparison

[Full comparison table from Step 4 — NEVER summarize]

## Evidence Pyramid Assessment

- [x/blank] Requirements documented
- [x/blank] Tests exist and are meaningful
- [x/blank] Unit tests pass
- [x/blank] Integration tests pass
- [x/blank] E2E tests pass (if applicable)
- [x/blank] Coverage >80%
- [x/blank] No regressions
- [x/blank] Real inputs validated

## Verdict: PASS / FAIL

**Reason:** [If FAIL — list every failing gate. If PASS — confirm all gates met.]
```

**IMPORTANT:** Show COMPLETE output in every section. Never summarize, truncate, or abbreviate. The evidence file is the proof — it must contain everything.

Tell the user: `📄 Evidence written to evidence/[context-name]-validation.md`

---

## Step 6: UPDATE CONTEXT

Update the context file in `contexts/active/`:

1. **Check off completed items** — mark Phase 3 / verification checkboxes as `[x]`

2. **Append to Session Log:**
   ```
   ### Session YYYY-MM-DD — Evidence Review
   **Action:** Full validation gate (review-evidence)
   **Results:** X tests passing, XX% coverage, X fixtures validated, X regressions
   **Evidence:** evidence/[context-name]-validation.md
   **Verdict:** PASS/FAIL
   ```

3. **If PASS:**
   - Set Status to `✅ Done`
   - Update Diagnosis to `COMPLETED`

4. **If FAIL:**
   - Keep current status
   - List remaining gaps under `Next Session`

---

## Step 7: NEXT STEPS

### If PASS:

Ask the user:
```
✅ All evidence gates passed.

📄 Evidence: evidence/[context-name]-validation.md
🧪 Tests: X passing, XX% coverage
📄 Fixtures: X validated, 0 regressions

Ready to:
1. Commit all changes and create PR to [base branch]?
2. Review the evidence file first?
3. Add more tests before shipping?
```

If the user says yes to commit/PR:
- Stage all relevant files (source, tests, evidence, context)
- Commit with a descriptive message referencing the evidence
- Create PR with evidence summary in the description

After commit/PR is done:
```
📦 Archive & Next

1. Archive the context: move `contexts/active/[file]` → `contexts/archive/[file]`
2. Pick your next task:
   → Run `/ai-dev-advisor:start-session` to review remaining work and pick the next context.
   → Run `/ai-dev-advisor:advisor` if you have something new to work on.
```

### If FAIL:

Show what's missing with specific fixes for each gate:
```
❌ Evidence gates not met:

[For each failing gate:]
- ❌ [Gate name]: [What's wrong] → [Specific action to fix]
```

Then immediately ask:
```
🔧 Want me to fix these now?
1. Yes — fix all gaps and re-validate automatically
2. Fix tests only — add missing tests, then re-validate
3. No — I'll fix manually and re-run /ai-dev-advisor:review-evidence later
```

#### If the user says YES (fix and re-validate):

1. **Fix each failing gate in order:**
   - ❌ Tests failing → read the failure output, fix the code or test
   - ❌ Coverage <80% → identify uncovered paths, write tests for them
   - ❌ Missing integration tests → create them based on the context file's success criteria
   - ❌ Regressions found → fix the regression, verify original behavior restored
   - ❌ Real inputs failing → debug the pipeline against the failing fixture

2. **After all fixes applied**, re-run the full validation automatically:
   - Go back to **Step 2** and run the complete gate again
   - Write a NEW evidence file (overwrite the previous one)
   - Update the context file with the new results

3. **Repeat until PASS or user stops:**
   - Maximum 3 automatic fix cycles — if still failing after 3 rounds, stop and ask the user for guidance
   - Each cycle appends to the session log: "Fix cycle N: fixed [X], [Y] still failing"

---

If a specific feature is specified: $ARGUMENTS — focus the review on that feature's tests and context.
