# Universal Test-First Prompt Template

Copy and customize this prompt for ANY implementation task:

```bash
claude "Feature/Fix: [NAME]

PHASE 1 - TEST SPECIFICATION:
Create tests/test_[name].py with comprehensive test cases:
- Happy path: [describe expected behavior]
- Edge cases: [list boundary conditions]
- Error cases: [list failure scenarios]

Requirements:
- Tests should FAIL initially (feature not implemented)
- Clear GIVEN/WHEN/THEN structure
- Use fixtures for test data

PHASE 2 - IMPLEMENTATION:
Implement [name] in [file path] to make all tests PASS.

Requirements:
- Follow existing code patterns
- Add type hints to all functions
- Add docstrings
- Implement ONLY what tests require

PHASE 3 - VERIFICATION:
Run full test suite: pytest tests/ -v --cov=[module]

Show me:
- Test coverage percentage
- Any failing tests
- Any uncovered code paths

Evidence required at each phase."
```

## Customization Guide

### For a Feature
```
Feature: User CSV Export

PHASE 1:
- Happy path: Valid CSV with 100 users → all imported
- Edge cases: Empty CSV, headers only, special characters
- Error cases: Invalid format, duplicate emails, missing fields
```

### For a Bug Fix
```
Bug Fix: Users can't login with special characters in password

PHASE 1:
- Create tests/hotfix/test_login_special_chars.py
- Should FAIL (reproduces the bug)
- Test passwords with: !@#$%^&*

PHASE 2:
- Fix password validation in src/auth/validator.py
- Tests should PASS

PHASE 3:
- Ensure no other auth tests broke
```

### For a Refactoring
```
Refactor: Extract duplicate validation logic

PHASE 1:
- Run current tests (establish baseline)
- All should PASS

PHASE 2:
- Extract common validation to src/utils/validators.py
- Update all callers to use new function
- Tests should still PASS

PHASE 3:
- Coverage should stay same or improve
- No new failures
```
