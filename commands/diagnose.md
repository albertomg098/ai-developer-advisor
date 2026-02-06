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

## Relevant Command

Run `/ai-dev-advisor:[command]` to proceed.
```

User's situation: $ARGUMENTS
