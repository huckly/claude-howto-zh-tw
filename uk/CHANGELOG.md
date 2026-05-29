# Changelog

## v2.3.0 — 2026-04-07

### Features

- build and publish EPUB artifacts per language (90e9c30) @Thiên Toán
- add missing pre-tool-check.sh hook to 06-hooks (b511ed1) @JiayuWang
- add Chinese translations in zh/ directory (89e89d4) @Luong NGUYEN
- Add performance-optimizer subagent and dependency-check hook (f53d080) @qk

### Bug Fixes

- Windows Git Bash compatibility + stdin JSON protocol (2cbb10c) @Luong NGUYEN
- correct autoCheckpoint config documentation in 08-checkpoints (749c79f) @JiayuWang
- embed SVG images instead of replacing with placeholders (1b16709) @Thiên Toán
- nested code fence rendering in memory README (ce24423) @Zhaoshan Duan
- apply review fixes dropped by squash merge (34259ca) @Luong NGUYEN
- make hook scripts compatible with Windows Git Bash and use stdin JSON protocol (107153d) @binyu li

### Documentation

- sync all tutorials with latest Claude Code docs (April 2026) (72d3b01) @Luong NGUYEN
- add Chinese language link to language switcher (6cbaa4d) @Luong NGUYEN
- add language switcher between English and Vietnamese (100c45e) @Luong NGUYEN
- add GitHub #1 Trending badge (0ca8c37) @Luong NGUYEN
- Introduce cc-context-stats for context zone monitoring (d41b335) @Luong NGUYEN
- Introduce luongnv89/skills collection and luongnv89/asm skill manager (7e3c0b6) @Luong NGUYEN
- Update README stats to reflect current GitHub metrics (5,900+ stars, 690+ forks) (5001525) @Luong NGUYEN
- Update README stats to reflect current GitHub metrics (3,900+ stars, 460+ forks) (9cb92d6) @Luong NGUYEN

### Refactoring

- replace Kroki HTTP dependency with local mmdc rendering (e76bbe4) @Luong NGUYEN
- shift quality checks to pre-commit, CI as 2nd pass (6d1e0ae) @Luong NGUYEN
- narrow auto-mode permissions baseline (2790fb2) @Luong NGUYEN
- Replace auto-adapt hook with one-time permissions setup script (995a5d6) @Luong NGUYEN

### Other

- shift-left quality gates — add mypy to pre-commit, fix CI failures (699fb39) @Luong NGUYEN
- Add Vietnamese (Tiếng Việt) Localization (a70777e) @Thiên Toán

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### Documentation

- Sync all tutorials and references with Claude Code v2.1.84 (f78c094) @luongnv89
  - Update slash commands to 55+ built-in + 5 bundled skills, mark 3 deprecated
  - Expand hook events from 18 to 25, add `agent` hook type (now 4 types)
  - Add Auto Mode, Channels, Voice Dictation to advanced features
  - Add `effort`, `shell` skill frontmatter fields; `initialPrompt`, `disallowedTools` agent fields
  - Add WebSocket MCP transport, elicitation, 2KB tool cap
  - Add plugin LSP support, `userConfig`, `${CLAUDE_PLUGIN_DATA}`
  - Update all reference docs (CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX)
- Rewrite README as landing-page-structured guide (32a0776) @luongnv89

### Bug Fixes

- Add missing cSpell words and README sections for CI compliance (93f9d51) @luongnv89
- Add `Sandboxing` to cSpell dictionary (b80ce6f) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug Fixes

- Remove dead marketplace link failing CI link checks (3fdf0d6) @luongnv89
- Add `sandboxed` and `pycache` to cSpell dictionary (dc64618) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### Features

- Add adaptive learning path with self-assessment and lesson quiz skills (1ef46cd) @luongnv89
  - `/self-assessment` — interactive proficiency quiz across 10 feature areas with personalized learning path
  - `/lesson-quiz [lesson]` — per-lesson knowledge check with 8-10 targeted questions

### Bug Fixes

- Update broken URLs, deprecations, and outdated references (8fe4520) @luongnv89
- Fix broken links in resources and self-assessment skill (7a05863) @luongnv89
- Use tilde fences for nested code blocks in concepts guide (5f82719) @VikalpP
- Add missing words to cSpell dictionary (8df7572) @luongnv89

### Documentation

- Phase 5 QA — fix consistency, URLs, and terminology across docs (00bbe4c) @luongnv89
- Complete Phases 3-4 — new feature coverage and reference doc updates (132de29) @luongnv89
- Add MCPorter runtime to MCP context bloat section (ef52705) @luongnv89
- Add missing commands, features, and settings across 6 guides (4bc8f15) @luongnv89
- Add style guide based on existing repo conventions (84141d0) @luongnv89
- Add self-assessment row to guide comparison table (8fe0c96) @luongnv89
- Add VikalpP to contributors list for PR #7 (d5b4350) @luongnv89
- Add self-assessment and lesson-quiz skill references to README and roadmap (d5a6106) @luongnv89

### New Contributors

- @VikalpP made their first contribution in #7

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### Features

- Sync all documentation with Claude Code February 2026 features (487c96d)
  - Update 26 files across all 10 tutorial directories and 7 reference documents
  - Add documentation for **Auto Memory** — persistent learnings per project
  - Add documentation for **Remote Control**, **Web Sessions**, and **Desktop App**
  - Add documentation for **Agent Teams** (experimental multi-agent collaboration)
  - Add documentation for **MCP OAuth 2.0**, **Tool Search**, and **Claude.ai Connectors**
  - Add documentation for **Persistent Memory** and **Worktree Isolation** for subagents
  - Add documentation for **Background Subagents**, **Task List**, **Prompt Suggestions**
  - Add documentation for **Sandboxing** and **Managed Settings** (Enterprise)
  - Add documentation for **HTTP Hooks** and 7 new hook events
  - Add documentation for **Plugin Settings**, **LSP Servers**, and Marketplace updates
  - Add documentation for **Summarize from Checkpoint** rewind option
  - Document 17 new slash commands (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast`, etc.)
  - Document new CLI flags (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode`, etc.)
  - Document new environment variables for auto memory, effort levels, agent teams, and more

### Design

- Redesign logo to compass-bracket mark with minimal palette (20779db)

### Bug Fixes / Corrections

- Update model names: Sonnet 4.5 → **Sonnet 4.6**, Opus 4.5 → **Opus 4.6**
- Fix permission mode names: replace fictional "Unrestricted/Confirm/Read-only" with actual `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- Fix hook events: remove fictional `PreCommit`/`PostCommit`/`PrePush`, add real events (`SubagentStart`, `WorktreeCreate`, `ConfigChange`, etc.)
- Fix CLI syntax: replace `claude-code --headless` with `claude -p` (print mode)
- Fix checkpoint commands: replace fictional `/checkpoint save/list/rewind/diff` with actual `Esc+Esc` / `/rewind` interface
- Fix session management: replace fictional `/session list/new/switch/save` with real `/resume`/`/rename`/`/fork`
- Fix plugin manifest format: migrate `plugin.yaml` → `.claude-plugin/plugin.json`
- Fix MCP config paths: `~/.claude/mcp.json` → `.mcp.json` (project) / `~/.claude.json` (user)
- Fix documentation URLs: `docs.claude.com` → `docs.anthropic.com`; remove fictional `plugins.claude.com`
- Remove fictional configuration fields across multiple files
- Update all "Last Updated" dates to February 2026

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
