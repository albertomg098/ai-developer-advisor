Diagnose the development context for the user's situation.

Read the skill at `skills/dev-advisor/SKILL.md` for the diagnosis decision tree.

Walk through the decision tree:

1. **Is production down or users affected RIGHT NOW?**
   - YES → 🔥 FIREFIGHTING

2. **Do you know what's wrong and how to fix/build it?**
   - YES, new → 🏗️ BUILDING
   - YES, broken → 🔥 FIREFIGHTING

3. **Is something broken/weird but not urgent?**
   - YES → 🔍 EXPLORATION

4. **Everything working, just want it better?**
   - YES → ✨ IMPROVING

For the diagnosed context, output:

```
## Diagnosis

**Context:** [emoji] [CONTEXT NAME]
**Confidence:** [High/Medium/Low]
**Reasoning:** [Why this context fits]

## Recommended Protocol

1. [First concrete step]
2. [Second concrete step]
3. [Third concrete step]

## Files to Create

- Context: `contexts/active/[type]_[name].md`
- Branch: `[type]/[name]` (if applicable)
- Tests: `tests/[dir]/test_[name].py`
```

## Next Step

Based on the diagnosed context, route explicitly to ONE of these:

- **🔍 EXPLORATION** → See the investigation workflow below, then run `/ai-dev-advisor:create-context` to create the investigation file.

- **🔥 FIREFIGHTING** → Run `/ai-dev-advisor:create-context` to create the hotfix context, then reproduce the bug in a test and fix with minimal scope.

- **🏗️ BUILDING (familiar tech)** → Run `/ai-dev-advisor:test-first` to start test-driven implementation.

- **🏗️ BUILDING (needs research)** → Run `/ai-dev-advisor:research` to research the technology before building.

- **✨ IMPROVING** → Run `/ai-dev-advisor:review-evidence` to baseline the current state, then improve and re-validate.

- **Multiple tasks** → Run `/ai-dev-advisor:start-session` to prioritize your work items, or `/ai-dev-advisor:setup-parallel` for parallel worktrees.

---

## 🔍 EXPLORATION: Investigation Workflow

If the diagnosis is EXPLORATION, guide the user through the investigation process inline (since there is no dedicated exploration command):

1. **Create investigation file** → `investigations/YYYYMMDD_[issue].md`
   - Run `/ai-dev-advisor:create-context` to scaffold the context file too

2. **Document what's known:**
   - Symptoms observed
   - What works vs. what doesn't
   - Recent changes that might be related
   - Environment details

3. **Generate hypotheses** (rank by likelihood):
   ```
   | # | Hypothesis | Likelihood | Test |
   |---|-----------|-----------|------|
   | 1 | [Most likely cause] | High | [How to test it] |
   | 2 | [Second candidate] | Medium | [How to test it] |
   | 3 | [Long shot] | Low | [How to test it] |
   ```

4. **Test systematically** — one hypothesis at a time:
   - Write a diagnostic test in `tests/debug/test_[hypothesis].py`
   - Run it → CONFIRMED / REJECTED / UNCLEAR
   - Update investigation file with results

5. **After 3 rejected hypotheses** without new leads → ask for help, pair with someone, or re-examine assumptions.

6. **Root cause found** → Transition to the appropriate context:
   - Needs a fix → 🔥 FIREFIGHTING: run `/ai-dev-advisor:test-first` to implement the fix
   - Needs a new feature → 🏗️ BUILDING: run `/ai-dev-advisor:test-first` to build it

---

User's situation: $ARGUMENTS
