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
   - Proceed to Step 5 (QA) or Step 6 (Evidence)

Report:
```
## 📊 Before/After Comparison
| Input | Field | Before | After | Status |
|-------|-------|--------|-------|--------|
| [file] | [field] | [value] | [value] | ✅/➖/❌ |

Regressions: X | Improvements: X | Unchanged: X
```

---

## Step 5: QA VALIDATION

Validate the application works live — not just in test harnesses, but as a real running deployment artifact.

### 5a. Determine if QA applies

Detect the app type using this decision tree:

1. **Check `docker-compose.yml` / `Dockerfile` with EXPOSE**
   → FOUND: 🐳 DOCKER (`docker compose up -d`, test via curl, `docker compose down`)

2. **Check `package.json` "workspaces" / `lerna.json` / `pnpm-workspace.yaml`**
   → FOUND: 📦 MONOREPO → ask user: "Which package should I QA? [list packages with start scripts]"

3. **Check `package.json` scripts + dependencies:**
   → `dev`/`start` with next/vite/react-scripts/angular → 🖥️ FRONTEND
   → `start` with express/fastify/nest/hono/koa → 🌐 API
   → `bin` field present → ⌨️ CLI
   → Multiple matches → QA each type sequentially

4. **Check Python project:**
   → uvicorn/gunicorn/flask/django in deps → 🌐 API
   → console_scripts / argparse / click / typer → ⌨️ CLI

5. **Check `Makefile` for run/serve/start/dev targets**
   → FOUND: use that target

6. **Nothing detected:**
   → Only `src/` + `tests/` with no entry point → 📦 LIBRARY (skip QA)
   → Otherwise: ask user "How do you start this app? [provide the start command]"

- If LIBRARY: `📦 Library detected — QA not applicable. Skipping to Step 6.` → go to Step 6.
- If app type found: proceed to 5b.

### 5b. Start the application

Execute via bash commands:

1. Find start command from detection above
2. Start in background, capture PID: `<start-command> & APP_PID=$!`
3. Poll for readiness (max 30s, 1s intervals):
   ```bash
   for i in {1..30}; do
     curl -s -o /dev/null -w "%{http_code}" http://localhost:PORT/ | grep -q "200" && break
     sleep 1
   done
   ```
4. If server fails to start after 30s: record as QA FAIL with the startup error

### 5c. Test via bash — protocol per app type

**🌐 API Protocol:**
1. Identify which endpoints to test:
   - Run `git diff [base-branch]...HEAD --name-only` to get changed files
   - Read each changed file looking for route definitions (decorators like `@app.route`, `@router.get`, or registrations like `app.get('/path', handler)`)
   - If no routes found in changed files, check if changed files are imported by route files (grep for the module name in route/controller files)
   - Also always test: `GET /` or `GET /health` as a baseline
2. For each identified endpoint, run 2-3 curl checks:
   ```bash
   # Happy path — valid request
   curl -s -w "\n%{http_code}" -X POST http://localhost:PORT/api/resource \
     -H "Content-Type: application/json" -d '{"field": "value"}'
   # → Check: status code matches expected (200/201/etc), response body is valid JSON

   # Error path — invalid input
   curl -s -w "\n%{http_code}" -X POST http://localhost:PORT/api/resource \
     -H "Content-Type: application/json" -d '{}'
   # → Check: status code is 400/422, response contains error message

   # Auth path (if endpoint requires auth) — missing token
   curl -s -w "\n%{http_code}" -X GET http://localhost:PORT/api/protected
   # → Check: status code is 401/403
   ```
3. For each check: compare actual status code and response shape against what the route handler's code says it should return

**🖥️ Frontend Protocol (3 tiers — use the highest available):**

| Tier | Tool | What It Tests |
|------|------|---------------|
| **Full** | Playwright MCP | Interactive flows, visual state, JS errors, screenshots |
| **Standard** | `curl` + bash | Server starts, responds with HTML, contains expected elements |
| **Minimal** | Build check only | `npm run build` succeeds, output files exist, no errors in build log |

1. **Minimal (always):** Run `npm run build` (or equivalent). Verify exit code 0, output directory exists, no error strings in build log.
2. **Standard (if dev server available):** `curl -s http://localhost:PORT/` — verify HTTP 200 and response body contains expected HTML markers (`<div id="root">`, `<title>`, no "Error" or "Cannot GET"). `curl` each route that corresponds to changed page components (e.g., changed `pages/settings.tsx` → curl `/settings`).
3. **Full (if Playwright MCP available):** Navigate to affected pages, interact with changed UI elements, verify visual state, check browser console for JS errors, take screenshots.

**⌨️ CLI Protocol:**
```bash
# Help text works
./cli --help  # → exits 0, outputs usage text

# Happy path
./cli process input.txt  # → exits 0, outputs expected result

# Error path
./cli process nonexistent.txt  # → exits non-zero, outputs helpful error message
```

**🐳 Docker Protocol:**
```bash
docker compose up -d
# Wait for health check
for i in {1..30}; do
  curl -s http://localhost:PORT/health > /dev/null && break
  sleep 1
done
# Then run API or Frontend protocol above
# Cleanup
docker compose down
```

### 5d. Report + cleanup

1. Kill background processes: `kill $APP_PID`, `docker compose down` if used
2. Generate QA report:

```
## 🌐 QA Validation

**App type:** [detected type]
**Start command:** [command used]
**QA status:** ✅ PASS / ❌ FAIL / ⚠️ PARTIAL

| Check | Result | Notes |
|-------|--------|-------|
| [check name] | ✅/❌ | [details] |
```

---

## Step 6: WRITE EVIDENCE FILE

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

## QA Validation

[Full QA results from Step 5. Write "N/A — library" if QA was skipped. Use this format:]

**App type:** [detected type]
**QA status:** ✅ PASS / ❌ FAIL / ⚠️ PARTIAL (X/Y checks passing after N fix cycles)

| Check | Result | Notes |
|-------|--------|-------|
| [check name] | ✅/❌ | [details] |

**QA-fix cycles:** [N] | **Bugs found:** [X] | **Fixed:** [Y] | **Remaining:** [Z]

## Evidence Pyramid Assessment

- [x/blank] Requirements documented
- [x/blank] Tests exist and are meaningful
- [x/blank] Unit tests pass
- [x/blank] Integration tests pass
- [x/blank] E2E tests pass (if applicable)
- [x/blank] Coverage >80%
- [x/blank] No regressions
- [x/blank] Real inputs validated
- [x/blank] QA validation passes (if applicable)

## Verdict: PASS / FAIL / PASS WITH CAVEATS

**Reason:** [If FAIL — list every failing gate. If PASS — confirm all gates met. If PASS WITH CAVEATS — list known issues documented for follow-up.]
```

**IMPORTANT:** Show COMPLETE output in every section. Never summarize, truncate, or abbreviate. The evidence file is the proof — it must contain everything.

Tell the user: `📄 Evidence written to evidence/[context-name]-validation.md`

---

## Step 7: UPDATE CONTEXT

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

## Step 8: NEXT STEPS

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

1. **Archive the context:** move `contexts/active/[file]` → `contexts/archive/[date]_[file]`

2. **Check backlog:** Read `contexts/backlog/` for pending items.
   - If backlog has items, show them:
     ```
     📋 Backlog has X items waiting:
     - [filename] — [outcome goal]
     - [filename] — [outcome goal]

     Promote one to active? (Pick a number, or "no" to skip)
     ```
   - If user picks one: move it from `contexts/backlog/` → `contexts/active/`, set Status to 🟢 Active, and load it
   - If backlog is empty: "Backlog is clear. Run `/ai-dev-advisor:advisor` when you have something new."

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
   - ❌ QA validation failing → for each QA bug: write a failing test first (test-first discipline), then fix the code

2. **After all fixes applied**, re-run the full validation automatically:
   - Go back to **Step 2** and run the complete gate again
   - Write a NEW evidence file (overwrite the previous one)
   - Update the context file with the new results

3. **Repeat until PASS or user stops:**
   - Each cycle appends to the session log: "Fix cycle N: fixed [X], [Y] still failing"
   - Maximum 3 automatic fix cycles — if still failing after 3 rounds, present options:
     ```
     ⚠️ QA validation partially passing (X/Y) after 3 fix cycles.

     Options:
     1. Keep fixing — I'll try 3 more fix cycles
     2. Ship with known issues — document in evidence, create follow-up context in backlog
     3. Stop — I'll investigate manually
     ```
   - If option 2: create `contexts/backlog/qa_followup_[name].md` with remaining bugs, set evidence verdict to `PASS WITH CAVEATS`

---

If a specific feature is specified: $ARGUMENTS — focus the review on that feature's tests and context.
