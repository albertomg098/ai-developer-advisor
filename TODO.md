# TODO: Make Plugin Installable from Other Repos

## Context
The repo is structured as a Claude Code plugin (`ai-dev-advisor`), but we haven't verified the actual install flow works end-to-end from another project. These are the steps to make it real.

---

## 1. Verify Plugin Discovery

- [ ] Confirm Claude Code's plugin install mechanism — does `/install-plugin <path>` exist, or is it a different command?
- [ ] Check if Claude Code reads `.claude-plugin/plugin.json` automatically or needs registration
- [ ] Test: from a fresh project, try installing this repo as a plugin and see what happens
- [ ] Document the exact install command that works

## 2. Test the Full Install Flow

- [ ] Create a throwaway test project (`mkdir /tmp/test-project && cd /tmp/test-project`)
- [ ] Try installing the plugin from local path
- [ ] Verify skills are discoverable (do they show up in Claude's context?)
- [ ] Verify commands are available (does `/ai-dev-advisor:advisor` work?)
- [ ] Run `/ai-dev-advisor:setup-advisor` — does it append to CLAUDE.md and create dirs?
- [ ] Try a full advisory flow: describe a task, get diagnosed, follow the protocol

## 3. Fix Whatever Breaks

- [ ] If plugin install doesn't work via path, research the correct mechanism
- [ ] If skills aren't auto-discovered, check if `plugin.json` needs a `skills` field listing them
- [ ] If commands don't get namespaced correctly, adjust naming
- [ ] If `setup-advisor` fails, fix the paths it references (they assume plugin root)

## 4. Make It Installable from GitHub

- [ ] Push the repo to GitHub (`github.com/albertomartin/claude-code-mental-model`)
- [ ] Verify the repo is public (or figure out auth for private repos)
- [ ] Test install from GitHub URL: `/install-plugin https://github.com/albertomartin/claude-code-mental-model`
- [ ] If GitHub install doesn't work, check if there's a plugin registry/marketplace to publish to

## 5. Polish the README Install Section

- [ ] Replace placeholder install commands with the ones that actually work
- [ ] Add troubleshooting section (common install issues)
- [ ] Add an uninstall instruction
- [ ] Add version/compatibility note (which Claude Code versions support plugins)

## 6. Optional: Auto-Setup on Install

- [ ] Research if `plugin.json` supports a `postInstall` hook or `setup` script
- [ ] If yes, wire it to run `setup-advisor` automatically after install
- [ ] If no, make sure the first-run experience clearly tells users to run `/ai-dev-advisor:setup-advisor`

## 7. Optional: Add a .gitignore

- [ ] Add `.gitignore` to exclude `contexts/`, `investigations/` (user-specific data) from the plugin repo
- [ ] Keep templates and skills tracked in git

---

## Priority Order
Start with **1 and 2** — everything else depends on knowing what actually works.
