# QA Validation Plan: [Feature/Context Name]

**Date:** YYYY-MM-DD
**Context:** [path to context file]
**Branch:** [branch name]

---

## App Type & Start Command

**Detected type:** 🌐 API / 🖥️ Frontend / ⌨️ CLI / 🐳 Docker / 📦 Library (skip QA)
**Start command:** `[command to start the app]`
**Ready signal:** `[how to know the app is ready — e.g., HTTP 200 on /health, specific stdout message]`
**Port:** [port number]

---

## Environment Requirements

- [ ] Environment variables set (list: `VAR_NAME=description`)
- [ ] Database running (type, connection string)
- [ ] Docker available (if needed)
- [ ] External services mocked or available
- [ ] Seed data loaded (if needed)

---

## QA Scenarios

### Happy Path
| # | Action | Expected Result |
|---|--------|-----------------|
| 1 | [e.g., GET /api/users] | [200, returns user list] |
| 2 | [e.g., POST /api/users with valid data] | [201, user created] |

### Error Path
| # | Action | Expected Result |
|---|--------|-----------------|
| 1 | [e.g., POST /api/users with empty body] | [400/422, validation error] |
| 2 | [e.g., GET /api/users/999 (nonexistent)] | [404, not found] |

### Edge Cases
| # | Action | Expected Result |
|---|--------|-----------------|
| 1 | [e.g., concurrent requests] | [no race conditions] |
| 2 | [e.g., large payload] | [handled gracefully] |

---

## Tools

- **curl** (always) — HTTP checks, status codes, response bodies
- **Playwright MCP** (if available) — interactive frontend testing, screenshots, JS console errors
- **Direct execution** (CLI) — stdout/stderr, exit codes

---

## Notes

[Any additional context — auth tokens needed, specific test data, order of operations]
