# 更新日誌

## v2.1.112 — 2026-04-16

### 重點摘要

- 將所有英文教學與 Claude Code v2.1.112 及全新的 Opus 4.7 模型 (`claude-opus-4-7`) 同步，包含全新的 `xhigh` 努力層級（在 Opus 4.7 上為預設值，介於 `high` 與 `max` 之間）、兩個全新的內建斜線命令 (`/ultrareview`, `/less-permission-prompts`)、針對 Opus 4.7 的 Max 訂閱者不再需要 `--enable-auto-mode` 的自動模式、Windows 上的 PowerShell 工具、「Auto (match terminal)」主題，以及以提示詞命名的計畫檔案。所有 18 個英文文件頁尾皆已更新至 Claude Code v2.1.112。 @Luong NGUYEN

### 新功能

- 在所有模組、根目錄文件、範例與參考資料中加入完整的烏克蘭語 (uk) 本地化 (039dde2) @Evgenij I

### Bug 修復

- 修正 `pre-tool-check.sh` 鉤子協定錯誤 (bce7cf8) @yarlinghe
- 將錯誤的 mermaid 範例更改為文字區塊以通過 CI (b8a7b1f) @Evgenij I
- 修正烏克蘭語 `claude_concepts_guide.md` 目錄中的 CP1251 編碼問題 (d970cc6) @Evgenij I
- 將暫存的烏克蘭語 README 替換為完整翻譯，並修復損壞的錨點 (f6d73e2) @Evgenij I
- 將所有頁尾的 Claude Code 版本更正為 2.1.97 (63a1416) @Luong NGUYEN
- 套用 2026-04-09 的文件準確性更新 (e015f39) @Luong NGUYEN

### 文件

- 同步至 Claude Code v2.1.112 (Opus 4.7, `xhigh` 努力層級, `/ultrareview`, `/less-permission-prompts`, PowerShell 工具, Auto-match-terminal 主題) @Luong NGUYEN
- 同步至 Claude Code v2.1.110 (TUI, 推送通知, 會話摘要) (15f0085) @Luong NGUYEN
- 同步至 Claude Code v2.1.101，包含 `/team-onboarding`, `/ultraplan`, Monitor 工具 (2deba3a) @Luong NGUYEN
- 將越南文文件與英文原始碼同步 (561c6cb) @Thiên Toán
- 更新所有檔案的「最後更新日期」與 Claude Code 版本 (7f2e773) @Luong NGUYEN
- 在語言切換器中加入烏克蘭語連結 (9c224ff) @Luong NGUYEN
- 移除貢獻者區塊 (f07313d) @Luong NGUYEN
- 更新 GitHub 指標至 21,800+ 顆星、2,585+ 次 fork (4f55374) @Luong NGUYEN

**完整更新日誌**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### Features

- 依語言建置並發佈 EPUB 產出物 (90e9c30) @Thiên Toán
- 在 06-hooks 中新增缺失的 pre-tool-check.sh 鉤子 (b511ed1) @JiayuWang
- 在 zh/ 目錄中新增中文翻譯 (89e89d4) @Luong NGUYEN
- 新增 performance-optimizer 子代理與 dependency-check 鉤子 (f53d080) @qk

### Bug Fixes

- Windows Git Bash 相容性 + stdin JSON 協定 (2cbb10c) @Luong NGUYEN
- 修正 08-checkpoints 中的 autoCheckpoint 設定文件 (749c79f) @JiayuWang
- 嵌入 SVG 圖片而非使用佔位符取代 (1b16709) @Thiên Toán
- 修正 memory README 中的巢狀程式碼區塊渲染問題 (ce24423) @Zhaoshan Duan
- 補回因 squash merge 而遺失的審查修復 (34259ca) @Luong NGUYEN
- 使鉤子腳本相容於 Windows Git Bash 並使用 stdin JSON 協定 (107153d) @binyu li

### Documentation

- 將所有教學與最新的 Claude Code 文件同步 (2026 年 4 月) (72d3b01) @Luong NGUYEN
- 在語言切換器中新增中文連結 (6cbaa4d) @Luong NGUYEN
- 在英文與越南文之間新增語言切換器 (100c45e) @Luong NGUYEN
- 新增 GitHub #1 Trending 徽章 (0ca8c37) @Luong NGUYEN
- 引入 cc-context-stats 用於 context 區域監控 (d41b335) @Luong NGUYEN
- 引入 luongnv89/skills 集合與 luongnv89/asm 技能管理器 (7e3c06) @Luong NGUYEN
- 更新 README 數據以反映目前的 GitHub 指標 (5,900+ stars, 690+ forks) (5001525) @Luong NGUYEN
- 更新 README 數據以反映目前的 GitHub 指標 (3,900+ stars, 460+ forks) (9cb92d6) @Luong NGUYEN

### Refactoring

- 將 Kroki HTTP 依賴替換為本地 mmdc 渲染 (e76bbe4) @Luong NGUYEN
- 將品質檢查移至 pre-commit，將 CI 作為第二階段 (6d1e0ae) @Luong NGUYEN
- 縮減 auto-mode 權限基準 (2790fb2) @Luong NGUYEN
- 將 auto-adapt 鉤子替換為一次性權限設定腳本 (995a5d6) @Luong NGUYEN

### Other

- 左移品質閘門 (shift-left quality gates) — 在 pre-commit 中加入 mypy，修復 CI 失敗問題 (699fb39) @Luong NGUYEN
- 新增越南文 (Tiếng Việt) 本地化 (a70777e) @Thiên Toán

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## [v2.1.150] — 2026-05-25

### Synced to Claude Code v2.1.150

Bumps tutorial coverage from Claude Code v2.1.145 → v2.1.150 (May 23, 2026
release). Anthropic shipped five patches (v2.1.146 through v2.1.150) since the
last sync. The headline change is the **rename of the bundled `/simplify`
skill to `/code-review`** (v2.1.146) — a pure rename with **no alias**, so the
old name no longer works. Because this repo also ships its own local
code-review skill, the directory was renamed to `code-review-specialist` to
avoid shadowing the new built-in. Other highlights: `/usage` now breaks costs
down per category, background sessions can be pinned with `Ctrl+T`, the
markdown renderer supports GFM task-list checkboxes, and a new
`allowAllClaudeAiMcps` managed setting was added. This sync also catches up
four module READMEs (`04-subagents`, `05-mcp`, `07-plugins`,
`09-advanced-features`) that were still pinned to v2.1.143.

### Behavior changes

- **`/simplify` renamed to `/code-review` (v2.1.146)**: the bundled review
  skill is now invoked as `/code-review` and takes an optional effort level
  (e.g. `/code-review high`); pass `--comment` to post findings as inline
  GitHub PR comments (v2.1.147). The old `/simplify` name no longer works —
  there is no alias. Updated in `01-slash-commands/README.md`,
  `03-skills/README.md`, `CATALOG.md`, `QUICK_REFERENCE.md`, and
  `claude_concepts_guide.md`.

### Changed

- **Renamed the repo's local `code-review` skill to `code-review-specialist`**
  to avoid colliding with the new built-in `/code-review`. The directory
  `03-skills/code-review/` → `03-skills/code-review-specialist/`, and all
  install commands, directory trees, and cross-references were updated in
  `README.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `CATALOG.md`,
  `LEARNING-ROADMAP.md`, `claude_concepts_guide.md`, and `03-skills/README.md`.
  Added a note explaining the collision and how to avoid shadowing the
  built-in.

### Added

- **`/usage` per-category cost breakdown (v2.1.149)** — the cost view now
  breaks spending down by category (skills, subagents, plugins, and
  per-MCP-server costs). Documented in `CATALOG.md` and
  `claude_concepts_guide.md`.
- **Pinned background sessions — `Ctrl+T` (v2.1.147)** — pinning a session in
  `claude agents` keeps it alive when idle, restarts it in place to apply
  Claude Code updates, and sheds it under memory pressure only after
  non-pinned sessions. Documented in `10-cli/README.md`.
- **GFM task-list checkbox rendering (v2.1.149)** — the markdown renderer now
  renders `- [ ]` / `- [x]` as checkboxes. Documented in
  `09-advanced-features/README.md`.
- **`allowAllClaudeAiMcps` managed setting (v2.1.149)** — permits loading
  claude.ai cloud MCP connectors organization-wide. Documented in
  `05-mcp/README.md`.

### Removed

- **Stop/SubagentStop hook input fields `background_tasks` and `session_crons`**
  — removed from `06-hooks/README.md` and `resources.md`. These were added from
  the v2.1.145 release notes but are not enumerated in the official hooks
  reference page; removed to keep the docs aligned with the published
  reference.

### Documentation

- Caught up four module READMEs from v2.1.143 to v2.1.150:
  `04-subagents/README.md`, `05-mcp/README.md`, `07-plugins/README.md`,
  `09-advanced-features/README.md`.
- Bumped every English doc's metadata footer to **v2.1.150 / May 25, 2026** for
  a consistent sync.

## [v2.1.145] — 2026-05-20

### Synced to Claude Code v2.1.145

Bumps tutorial coverage from Claude Code v2.1.143 → v2.1.145 (May 19, 2026
release). Anthropic shipped two patches (v2.1.144 and v2.1.145) since the last
sync. Highlights: the `/extra-usage` rename to `/usage-credits`, `/model`
becoming session-only by default, three new bundled skills (`/run`, `/verify`,
`/run-skill-generator`), Stop/SubagentStop hook input fields
`background_tasks` and `session_crons`, `claude agents --json` for scripting,
and a security fix that closes the bare-env-var Bash auto-approve loophole.
This sync also catches up six root-level reference docs
(`LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`,
`claude_concepts_guide.md`, `STYLE_GUIDE.md`) that were missed in the
v2.1.143 sync and were still pinned to v2.1.138.

### Added

- `/usage-credits` slash command (v2.1.144) — replaces `/extra-usage` as the
  primary name; `/extra-usage` still works as an alias. Documented in
  `01-slash-commands/README.md` and `CATALOG.md`.
- Three new bundled skills (v2.1.145) — `/run` (launches the project's app to
  see a change running), `/verify` (builds, runs, and observes the app to
  confirm a fix works), `/run-skill-generator` (teaches `/run`/`/verify` how
  to handle a specific project by generating a per-project skill). Documented
  in `03-skills/README.md`, `CATALOG.md`, and `QUICK_REFERENCE.md`. Brings
  the canonical bundled-skill count to **9**.
- Stop/SubagentStop hook input fields `background_tasks` and `session_crons`
  (v2.1.145) — hook authors can read these to decide whether to block stop
  while background work or scheduled tasks are still pending. Documented in
  `06-hooks/README.md`.
- `claude agents --json` (v2.1.145) — print the agent list as machine-readable
  JSON for scripting (status bars, session pickers, tmux-resurrect).
  Documented in `10-cli/README.md`.
- Five hook-event rows that were missing from summary tables — `Setup`,
  `UserPromptExpansion`, `PermissionDenied`, `PostToolBatch` (the narrative
  already claimed "29 events"; the summary tables in `CATALOG.md`,
  `claude_concepts_guide.md`, and `INDEX.md` only listed 25).

### Behavior changes

- **`/model` is session-only by default (v2.1.144)**: selecting a model now
  applies only to the current session; press `d` after selecting to set the
  choice as the new default for future sessions. Documented in
  `01-slash-commands/README.md`.
- **Bash bare env-var auto-approve closed (v2.1.145 security fix)**: a
  command of the form `FOO=bar somecommand` is no longer auto-approved when
  only `FOO=bar` was on the allowlist. Re-allow such commands explicitly via
  `Bash(...)` permission rules that cover the full command. Documented in
  `06-hooks/README.md`.
- **`context: fork` infinite-loop fix (v2.1.145)**: a skill using
  `context: fork` could previously trigger an infinite re-invocation loop in
  rare cases. Documented as a note in `03-skills/README.md`.

### Documentation

- Caught up six root-level reference docs from v2.1.138 to v2.1.145:
  `LEARNING-ROADMAP.md`, `QUICK_REFERENCE.md`, `INDEX.md`, `resources.md`,
  `claude_concepts_guide.md`, `STYLE_GUIDE.md`.
- Fixed the bundled-skills mismatch — `CATALOG.md`, `QUICK_REFERENCE.md`, and
  `03-skills/README.md` previously listed three different 5-item lists;
  reconciled to the canonical 9 (`/batch`, `/claude-api`, `/debug`,
  `/fewer-permission-prompts`, `/loop`, `/run`, `/run-skill-generator`,
  `/simplify`, `/verify`). The `QUICK_REFERENCE.md` cell had also incorrectly
  listed `/voice` and `/browse` as bundled skills — neither is bundled.
- Renamed "New Features (March 2026)" → "New Features (May 2026)" in
  `QUICK_REFERENCE.md` and `resources.md` to match the rest of the repo.
- Bumped the version badge in `README.md` from `2.1.138` to `2.1.145` and
  updated the two "latest: v2.1.138" claims in the body.
- Updated the STYLE_GUIDE sample metadata footer from `2.1.97` to `2.1.145`
  so contributors copy the current version.

## [v2.1.143] — 2026-05-19

### Synced to Claude Code v2.1.143

Bumps tutorial coverage from Claude Code v2.1.138 → v2.1.143 (May 15, 2026
release). Anthropic shipped five patches (v2.1.139–v2.1.143) since the last
sync. Highlights: `/goal` and `/scroll-speed` slash commands, the `claude
agents` Agent View (Research Preview) with a full dispatch flag set, a Stop
hook safety cap, hook exec form (`args`), `continueOnBlock` for PostToolUse,
hook `terminalSequence` output, Fast Mode defaulting to Opus 4.7,
PowerShell-by-default on Windows for Bedrock/Vertex/Foundry, and the
`worktree.bgIsolation` setting.

### Added

- `/goal <statement>` slash command (v2.1.139) — registers a session-level
  completion condition with a live overlay panel showing elapsed time, turn
  count, and token usage. Documented in `01-slash-commands/README.md` and
  cross-linked from `10-cli/README.md`.
- `/scroll-speed <±N>` slash command (v2.1.139) — tunes TUI live-preview
  scroll speed; persists per-machine. Documented in
  `01-slash-commands/README.md`.
- `claude agents` Agent View (Research Preview, v2.1.139) with dispatch flags
  `--cwd` (v2.1.141), `--add-dir`, `--settings`, `--mcp-config`,
  `--plugin-dir`, `--permission-mode`, `--model`, `--effort`,
  `--dangerously-skip-permissions` (v2.1.142). Documented in
  `10-cli/README.md`.
- `claude plugin details <name>` (v2.1.139) — full plugin inventory plus
  projected per-turn/per-invocation token-cost estimate. LSP servers added
  to the details pane in v2.1.142. Documented in `07-plugins/README.md`.
- Marketplace context-cost projection in the `/plugin` browse pane (v2.1.143).
  Documented in `07-plugins/README.md`.
- Hook **exec form** (`args: string[]`, v2.1.139) — direct `execve()` spawn
  with no shell parsing; mutually exclusive with the shell-form `command`
  field. Documented in `06-hooks/README.md`.
- Hook `continueOnBlock: true` field on PostToolUse (v2.1.139) — surfaces a
  blocked tool result back to Claude as a `tool_result` instead of aborting
  the turn. Documented in `06-hooks/README.md`.
- Hook `terminalSequence` JSON output field (v2.1.141) — emits raw OSC escape
  sequences for desktop notifications, window titles, and bells. Documented
  in `06-hooks/README.md`.
- `worktree.bgIsolation: "none"` setting (v2.1.143) — background sessions
  edit the current working copy directly instead of an isolated worktree.
  Documented in `09-advanced-features/README.md`.
- `CLAUDE_PROJECT_DIR` is now passed to every MCP stdio server's environment
  (v2.1.139), and `${CLAUDE_PROJECT_DIR}` substitution is supported in plugin
  and project `.mcp.json` `command`/`args`/`env` fields. Documented in
  `05-mcp/README.md`.
- Subagent OTEL headers `x-claude-code-agent-id` and
  `x-claude-code-parent-agent-id` (v2.1.139), exposed as
  `agent_id` / `parent_agent_id` attributes on `claude_code.llm_request`
  OTEL spans. Documented in `04-subagents/README.md`.
- `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1` (v2.1.142) — pin Fast Mode back
  to Opus 4.6 after the v2.1.142 default flipped to Opus 4.7. Documented in
  `10-cli/README.md`.
- `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` and
  `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1` (v2.1.143) — opt out of
  the default-on PowerShell tool, or make it honor the system execution
  policy instead of `-ExecutionPolicy Bypass`. Documented in
  `09-advanced-features/README.md`.
- `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` (v2.1.143) — override the 8-consecutive-
  blocks safety cap for Stop hooks (set `0` to disable). Documented in
  `06-hooks/README.md` and `09-advanced-features/README.md`.
- `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` (v2.1.141) — force plugin installs to
  clone GitHub plugin sources over HTTPS for CI runners without SSH keys.
  Documented in `07-plugins/README.md`.
- `ANTHROPIC_WORKSPACE_ID` (v2.1.141) — scopes a federated workload-identity
  token to a specific workspace. Documented in `09-advanced-features/README.md`.
- Root-level `SKILL.md` plugin pattern (v2.1.142) — a plugin with only a
  top-level `SKILL.md` (no `skills/` subdirectory) is surfaced as a single
  skill. Documented in `07-plugins/README.md`.
- Plugins marketing name **Routines** for `/schedule` (Anthropic blog,
  2026-05-14) — surfaced as a one-line note in
  `09-advanced-features/README.md`; the CLI surface remains `/schedule`.

### Behavior changes

- **Fast Mode default flipped to Opus 4.7 (v2.1.142)**: `/fast` now runs Opus
  4.7 by default (was Opus 4.6). Set `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`
  to opt back in.
- **PowerShell tool enabled by default on Windows for Bedrock/Vertex/Foundry
  (v2.1.143)**: Claude Code invokes PowerShell with `-ExecutionPolicy Bypass`.
  Opt out with `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1` (honor
  system policy) or `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` (disable the tool).
- **Remote Control, `/schedule`, claude.ai MCP connectors, and notification
  preferences auto-disabled when API-key auth is set (v2.1.139)**: setting
  `ANTHROPIC_API_KEY`, `ANTHROPIC_AUTH_TOKEN`, or `apiKeyHelper` disables all
  four claude.ai-bridged surfaces, even if a claude.ai login is also active.
- **Stop hook block loops capped at 8 consecutive blocks (v2.1.143)**: the
  session ends with a warning after 8 in a row, preventing buggy Stop hooks
  from looping the session forever. Override with
  `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`.
- **`subagent_type` matching is now case- and separator-insensitive (v2.1.140)**:
  `code-reviewer`, `Code Reviewer`, and `code_reviewer` all resolve to the
  same agent. Documented in `04-subagents/README.md`.

### Changed

- Root reference docs (`README.md`, `CATALOG.md`) updated from `28 hook
  events` to `29 hook events` — matches `06-hooks/README.md` and
  `LEARNING-ROADMAP.md` after the `Setup` hook landed in v2.1.138.

### Notes for translators

- Tutorial translations (`vi/`, `ja/`, `uk/`, `zh/`) follow English; sync
  this round's deltas in module READMEs and the CHANGELOG above. Footers
  must reflect `Last Updated: May 19, 2026` and `Claude Code Version: 2.1.143`.

## [v2.1.138] — 2026-05-09

### Synced to Claude Code v2.1.138

Bumps tutorial coverage from Claude Code v2.1.131 → v2.1.138 (May 9, 2026
release). Anthropic shipped seven patches between v2.1.132 and v2.1.138 since
the last sync.

### Added (English docs)

- `worktree.baseRef` setting (v2.1.133) — controls whether `claude --worktree`
  branches from `origin/<default>` (`"fresh"`, default) or local `HEAD`
  (`"head"`). **Behavior change**: the `"fresh"` default reverts the v2.1.128
  behavior, so users who relied on local-`HEAD` branching after v2.1.128 must
  opt back in. Documented in `09-advanced-features/README.md`.
- `autoMode.hard_deny` admin key (v2.1.136) — array of classifier rules that
  block a class of actions regardless of inferred user intent. Use for
  actions that must never run in auto mode (e.g., `rm -rf /`, force-push to
  protected branches). Unlike `soft_deny`, hard-deny rules are not negotiable
  by the classifier. Documented in `09-advanced-features/README.md`.
- `parentSettingsBehavior` admin key (v2.1.133+, admin-tier) — controls how
  the SDK's `managedSettings` merges with parent-process settings.
  `"first-wins"` keeps existing precedence; `"merge"` deep-merges values.
  Documented in `09-advanced-features/README.md`.
- `Setup` hook event — initial environment setup (one-time per session); use
  to provision tooling or install deps. Brings the documented hook-events
  total from 28 to 29. Documented in `06-hooks/README.md`.
- `effort.level` field in hook input JSON (v2.1.133) — exposes the active
  effort level (`low`/`medium`/`high`/`xhigh`/`max`) to hooks. Documented in
  `06-hooks/README.md`.
- `CLAUDE_CODE_SESSION_ID` environment variable in Bash subprocesses
  (v2.1.132) — session UUID matching the `session_id` field in hook input
  JSON, for correlating bash logs with hook telemetry. Documented in
  `06-hooks/README.md`.
- `CLAUDE_EFFORT` environment variable in Bash subprocesses (v2.1.133) —
  active effort level, matching `effort.level` in hook input JSON. Documented
  in `06-hooks/README.md`.
- `sandbox.bwrapPath` and `sandbox.socatPath` settings (v2.1.133+, Linux/WSL)
  — point Claude Code at non-standard install locations for `bubblewrap` and
  `socat`. Default to `$PATH` lookup. Documented in
  `09-advanced-features/README.md`.
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` environment variable (v2.1.132).
  Documented in `09-advanced-features/README.md`.
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` environment variable
  (v2.1.136) — re-enables the session-quality survey for organizations
  capturing OpenTelemetry data; off by default in OTEL deployments.
  Documented in `09-advanced-features/README.md`.

### Changed

- **Behavior change**: Plan mode now blocks all file writes unconditionally
  (v2.1.136), including when a matching `Edit(...)` rule exists in
  `permissions.allow`. Previously a permissive `Edit(...)` rule could let
  writes through in plan mode; that bypass is closed. Workflows that
  depended on the older behavior must exit plan mode (`Shift+Tab`) before
  editing. Documented in `09-advanced-features/README.md`.
- Plugin spaced slash commands (e.g., `/myplugin review`) now resolve to
  `/myplugin:review`. Plugin `skills` config entries no longer hide the
  default `skills/` directory — both are merged. Documented in
  `07-plugins/README.md`.
- MCP servers now persist across `/clear` (v2.1.132+). Documented in
  `05-mcp/README.md`.
- Subagents discover project, user, and plugin skills via the Skill tool
  (v2.1.133). Documented in `04-subagents/README.md`.
- `--permission-mode` is now honored when resuming plan-mode sessions
  (v2.1.132). Documented in `09-advanced-features/README.md`.
- `CronList` output now includes the qualifier(s) and the scheduled prompt
  body (v2.1.136), so you can audit what each cron will run without opening
  it. Documented in `09-advanced-features/README.md`.

### Fixed

- OAuth refresh-token concurrent-refresh race condition.
- INDEX.md count drift: Skills 28 → 16, Plugins 40 → 27, Hooks scripts
  8 → 9 (recounted from the markdown content tree). The new totals reflect a
  `.md`-only methodology that scopes counts to tutorial content rather than
  build artifacts and config.
- Stale source URLs in `CATALOG.md` (v2.1.118 → v2.1.138) and
  `claude_concepts_guide.md` (v2.1.117 → v2.1.138). Removed a duplicate
  legacy footer in the concepts guide.

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.1.131] — 2026-05-06

### Synced to Claude Code v2.1.131

Bumps tutorial coverage from Claude Code v2.1.126 → v2.1.131 (May 6, 2026
release). Anthropic shipped v2.1.128, v2.1.129, and v2.1.131 since the last
sync; v2.1.127 and v2.1.130 were skipped and never released publicly.

### Added (English docs)

- `--plugin-url <url>` flag (v2.1.129) — fetches a plugin `.zip` archive from
  a URL for the current session. Repeatable. Documented in
  `07-plugins/README.md`.
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` env var (v2.1.129) — forces synchronous
  output for terminals where auto-detection misses (e.g., Emacs `eat`).
  Documented in `10-cli/README.md` and `09-advanced-features/README.md`.
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` env var (v2.1.129) — enables
  background upgrades for Homebrew/WinGet installs (which normally do not
  auto-update). Documented in `10-cli/README.md` and
  `09-advanced-features/README.md`.
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` env var (v2.1.129) — required
  to opt in to `/v1/models` gateway discovery (see Changed). Documented in
  `10-cli/README.md`.
- `disableRemoteControl` setting (v2.1.128) — admins can block
  `claude remote-control` and `/remote-control` via managed/policy scope.
  Documented in `09-advanced-features/README.md`.
- `--plugin-dir` accepts `.zip` archives (v2.1.128) — alongside directory
  inputs. Documented in `07-plugins/README.md`.
- `skillOverrides` accepts `"name-only"` and `"user-invocable-only"`
  (v2.1.129) — in addition to the previous `"on"`/`"off"`. Documented in
  `03-skills/README.md`.

### Changed

- **Behavior change**: Gateway `/v1/models` discovery is now **opt-in**
  (v2.1.129). Previously (v2.1.126), setting `ANTHROPIC_BASE_URL` automatically
  populated `/model` from the gateway's `/v1/models` endpoint. From v2.1.129,
  users must additionally set `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`;
  without the env var, `/model` falls back to the built-in static list.
  Documented in `10-cli/README.md`.
- `/mcp` shows tool count per server and visually flags servers reporting 0
  tools (v2.1.128). Documented in `05-mcp/README.md`.
- Bare `/color` (no args) picks a random session color (v2.1.128); explicit
  `/color <name|hex>` continues to set a specific color. Documented in
  `01-slash-commands/README.md`.
- `--channels` flag now works with API-key (console) authentication
  (v2.1.128). Earlier releases required Pro/Max OAuth. Documented in
  `09-advanced-features/README.md`.
- Ctrl+R history picker defaults to **all prompts across all projects**
  (v2.1.129). Press Ctrl+S inside the picker to narrow scope to the current
  project. Documented in `09-advanced-features/README.md`.
- `/context` no longer dumps its ASCII visualization into the conversation
  (v2.1.129). The viz is shown in-UI only; no more ~1.6k token cost per
  invocation. Documented in `09-advanced-features/README.md`.
- Oversized images in drag-and-drop are auto-downscaled (v2.1.128) — earlier
  versions rejected images outright.

### Fixed

- VS Code extension activation on Windows (v2.1.131).
- Mantle endpoint authentication (v2.1.131).
- 1-hour prompt-cache TTL no longer truncated to 5 minutes (v2.1.129).
- Crash on stdin payloads larger than 10 MB (v2.1.128).

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.1.126] — 2026-05-02

### Synced to Claude Code v2.1.126

Bumps tutorial coverage from Claude Code v2.1.119 → v2.1.126 (May 1, 2026 release).
v2.1.120 was rolled back on its first release day (2026-04-24) but re-released
successfully on 2026-04-28 with the originally-reported regressions fixed.
v2.1.124 and v2.1.125 were skipped by Anthropic and never released.

### Added (English docs)

- `claude project purge [path]` subcommand (v2.1.126) — deletes all Claude Code
  state for a project (transcripts, tasks, debug logs, file-edit history,
  prompt history, `~/.claude.json` entry). Supports `--dry-run`, `-y/--yes`,
  `-i/--interactive`, `--all`. Documented in `10-cli/README.md`.
- `claude plugin prune` subcommand (v2.1.121) — removes orphaned auto-installed
  plugin dependencies; `plugin uninstall --prune` cascades. Documented in
  `07-plugins/README.md`.
- `claude ultrareview [target]` subcommand (v2.1.120) — runs `/ultrareview`
  non-interactively from CI/scripts, prints findings to stdout, exits 0/1 on
  success/failure; supports `--json` and `--timeout <minutes>`. Documented in
  `10-cli/README.md`.
- `${CLAUDE_EFFORT}` placeholder available inside skill content (v2.1.120) —
  resolves to the current effort level. Documented in `03-skills/README.md`.
- `alwaysLoad` MCP server config option (v2.1.121) — when `true`, all tools
  from that server skip tool-search deferral. Documented in `05-mcp/README.md`.
- `PostToolUse.hookSpecificOutput.updatedToolOutput` now works for all tools
  (v2.1.121), previously MCP-only. Documented in `06-hooks/README.md`.
- `ANTHROPIC_BEDROCK_SERVICE_TIER` environment variable (v2.1.122) — selects
  Bedrock service tier (`default`, `flex`, `priority`). Documented in
  `10-cli/README.md` env-var table.
- `--dangerously-skip-permissions` extended-path coverage (v2.1.121, v2.1.126)
  — now bypasses prompts for writes to `.claude/skills/`, `.claude/agents/`,
  `.claude/commands/`, `.claude/`, `.git/`, `.vscode/`, shell config files.
  Catastrophic removal commands (`rm -rf /` etc.) still prompt. Documented in
  `09-advanced-features/README.md` permission-modes section.
- OAuth code paste fallback (v2.1.126) — `claude auth login` accepts the OAuth
  code pasted into the terminal when the browser callback can't reach
  localhost (WSL2, SSH, containers). Documented in `10-cli/README.md`.
- Type-to-filter `/skills` menu (v2.1.121). Documented in `03-skills/README.md`.
- `AI_AGENT` environment variable (v2.1.120) — set on subprocesses so `gh` can
  attribute traffic to Claude Code. Documented in `10-cli/README.md` env-var
  table.

### Changed

- `--from-pr` (v2.1.119) and `/resume` PR-URL search (v2.1.122) now both
  support GitHub, GitHub Enterprise, GitLab, and Bitbucket URLs.
- Windows: Git for Windows / Git Bash no longer required (v2.1.120) — Claude
  Code uses PowerShell as the shell tool when Git Bash is absent. From v2.1.126,
  PowerShell is the primary shell when the PowerShell tool is enabled. Detection
  extended to PowerShell 7 installed via Microsoft Store, MSI without PATH, or
  `.NET global tool`. Documented in `09-advanced-features/README.md` platform
  notes.
- `/model` picker now lists models from your gateway's `/v1/models` endpoint
  when `ANTHROPIC_BASE_URL` points at an Anthropic-compatible gateway
  (v2.1.126). Documented in `10-cli/README.md`.
- `--dangerously-skip-permissions` no longer prompts for writes to a much
  broader allowlist (see Added). Catastrophic removals still prompt.
- Image paste auto-downscale (v2.1.126) — images larger than 2000px are
  downscaled on paste; oversized images in history are auto-removed and the
  request retried. (Tutorial-relevant only as a safety/UX note.)

### Security

- Fixed `allowManagedDomainsOnly` / `allowManagedReadPathsOnly` being ignored
  when a higher-priority managed-settings source lacked a `sandbox` block
  (v2.1.126).

### Notes for translation maintainers

The `vi/`, `zh/`, `uk/`, and `ja/` localized trees are community-maintained
and may lag the English source. Contributors syncing translations should diff
against the English files updated in this release.

## [v2.4.0] — 2026-04-27

### Synced to Claude Code v2.1.119

Bumps tutorial coverage from Claude Code v2.1.112 → v2.1.119 (April 23, 2026 release).
v2.1.120 was published April 24, briefly rolled back the same day due to regressions,
and re-released on April 28 with fixes — it is now part of the normal release line.
A subsequent v2.1.126 (May 1, 2026) is the next stable target and is covered in the
v2.1.126 entry above.

### Added (English docs)

- Native binary packaging note (v2.1.113) — CLI now ships per-platform native binaries
- `bfs`/`ugrep` Glob/Grep substitution footnote on native macOS/Linux builds (v2.1.117)
- `mcp_tool` hook type with example (v2.1.118)
- `duration_ms` field on PostToolUse / PostToolUseFailure inputs (v2.1.119)
- `prUrlTemplate` setting (v2.1.119) and expanded `--from-pr` provider list (GitLab, Bitbucket)
- `cleanupPeriodDays` extended scope (checkpoints + tasks + shell-snapshots + backups, v2.1.117)
- Plugin marketplace enforcement on every lifecycle event (v2.1.117) and `hostPattern`/`pathPattern` regex (v2.1.119)
- New env vars: `DISABLE_UPDATES`, `CLAUDE_CODE_HIDE_CWD`, `CLAUDE_CODE_FORK_SUBAGENT`, `OTEL_LOG_TOOL_DETAILS`, `ENABLE_TOOL_SEARCH` Vertex opt-in
- New slash commands: `/btw`, `/theme` with custom themes
- `/usage` canonical command (merges `/cost` + `/stats`, v2.1.118)
- Forked subagents (`CLAUDE_CODE_FORK_SUBAGENT=1`, v2.1.117)
- Auto mode `"$defaults"` token (v2.1.118)
- `wslInheritsWindowsSettings` managed policy (v2.1.118)
- Vim visual / visual-line modes (v2.1.118)
- `claude install [version]` and `claude plugin tag` subcommands

### Changed

- Documentation host migrated: `docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 effort levels: `xhigh` is now the Claude Code default since the 2026-04-16 launch; Opus 4.7 native context window confirmed at 1M (v2.1.117 fixed `/context` miscounting it as 200K)
- Default effort raised from `medium` to `high` for Pro/Max subscribers on Opus 4.6 / Sonnet 4.6 (v2.1.117)
- `STYLE_GUIDE.md` Source URL updated from Claude Apps article to `code.claude.com/docs/en/changelog`

### Deprecated (tracked, not removed)

- `includeCoAuthoredBy` setting → use `attribution.commit` / `attribution.pr`
- `voiceEnabled` setting → use `voice.enabled`

### Notes for translation maintainers

The `vi/`, `zh/`, and `uk/` localized trees are community-maintained and may lag the English source. Contributors syncing translations should diff against the English files updated in this release.

## v2.1.112 — 2026-04-16

### Highlights

- Sync all English tutorials with Claude Code v2.1.112 and the new Opus 4.7 model (`claude-opus-4-7`), including the new `xhigh` effort level (default on Opus 4.7, between `high` and `max`), two new built-in slash commands (`/ultrareview`, `/less-permission-prompts`), auto-mode no longer requiring `--enable-auto-mode` for Max subscribers on Opus 4.7, the PowerShell tool on Windows, the "Auto (match terminal)" theme, and plan files named after prompts. All 18 EN doc footers bumped to Claude Code v2.1.112. @Luong NGUYEN

### Features

- Add complete Ukrainian (uk) localization across all modules, root docs, examples, and references (039dde2) @Evgenij I

### Bug Fixes

- Correct pre-tool-check.sh hook protocol bugs (bce7cf8) @yarlinghe
- Change bad mermaid example to text block to pass CI (b8a7b1f) @Evgenij I
- Fix CP1251 encoding in Ukrainian claude_concepts_guide.md ToC (d970cc6) @Evgenij I
- Replace stub Ukrainian README with full translation, fix broken anchors (f6d73e2) @Evgenij I
- Correct Claude Code version to 2.1.97 across all footers (63a1416) @Luong NGUYEN
- Apply 2026-04-09 documentation accuracy updates (e015f39) @Luong NGUYEN

### Documentation

- Sync to Claude Code v2.1.112 (Opus 4.7, `xhigh` effort, `/ultrareview`, `/less-permission-prompts`, PowerShell tool, Auto-match-terminal theme) @Luong NGUYEN
- Sync to Claude Code v2.1.110 (TUI, push notifications, session recap) (15f0085) @Luong NGUYEN
- Sync to Claude Code v2.1.101 with `/team-onboarding`, `/ultraplan`, Monitor tool (2deba3a) @Luong NGUYEN
- Sync Vietnamese documentation with English source (561c6cb) @Thiên Toán
- Update Last Updated date and Claude Code version across all files (7f2e773) @Luong NGUYEN
- Add Ukrainian language link to language switcher (9c224ff) @Luong NGUYEN
- Remove contributors section (f07313d) @Luong NGUYEN
- Update GitHub metrics to 21,800+ stars, 2,585+ forks (4f55374) @Luong NGUYEN

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

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

- 將所有教學與參考文件與 Claude Code v2.1.84 (f78c094) 同步 @luongnv89
  - 更新斜線命令至 55 個以上內建功能 + 5 個綑綁技能，並將 3 個棄用功能標記為棄用
  - 將 hook 事件從 18 個擴展至 25 個，新增 `agent` hook 類型（現在共有 4 種類型）
  - 在進階功能中新增 Auto Mode、Channels、Voice Dictation
  - 新增 `effort`、`shell` 技能 frontmatter 欄位；新增 `initialPrompt`、`disallowedTools` agent 欄位
  - 新增 WebSocket MCP transport、elicitation、2KB tool cap
  - 新增 plugin LSP 支援、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有參考文件 (CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX)
- 將 README 重寫為著陸頁結構的指南 (32a0776) @luongnv89

### Bug Fixes

- 補上缺失的 cSpell 單字與 README 章節以符合 CI 規範 (93f9d51) @luongnv89
- 將 `Sandboxing` 加入 cSpell 字典 (b80ce6f) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug Fixes

- 移除導致 CI 連結檢查失敗的失效 marketplace 連結 (3dfd0d6) @luongnv89
- 將 `sandboxed` 與 `pycache` 加入 cSpell 字典 (dc64618) @luongnv89

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### Features

- 新增包含自我評估與課程測驗技能的適應性學習路徑 (1ef46cd) @luongnv89
  - `/self-assessment` — 橫跨 10 個功能領域的互動式熟練度測驗，並提供個人化學習路徑
  - `/lesson-quiz [lesson]` — 針對每堂課的知識檢查，包含 8-10 個目標問題

### Bug Fixes

- 更新失效的 URL、棄用功能以及過時的參考內容 (8fe4520) @luongnv89
- 修復資源與自我評估技能中的失效連結 (7a05863) @luongnv89
- 在概念指南中使用 tilde fences 處理巢狀程式碼區塊 (5f82719) @VikalpP
- 將缺失的單字加入 cSpell 字典 (8df7572) @luongnv89

### Documentation

- 第 5 階段 QA — 修復文件中的一致性、URL 與術語 (00bbe4c) @luongnv89
- 完成第 3-4 階段 — 新功能覆蓋與參考文件更新 (132de29) @luongnv89
- 在 MCP context bloat 章節中新增 MCPorter runtime (ef52705) @luongnv89
- 在 6 個指南中補上缺失的命令、功能與設定 (4bc8f15) @luongnv89
- 根據現有 repo 約定新增風格指南 (84141d0) @luongnv89
- 在指南比較表中新增自我評估列 (8fe0c96) @luongnv89
- 在貢獻者名單中為 PR #7 新增 VikalpP (d5b4350) @luongnv89
- 在 README 與 roadmap 中新增自我評估與 lesson-quiz 技能的參考 (d5a6106) @luongnv89

### New Contributors

- @VikalpP 在 #7 中完成了首次貢獻

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### Features

- 將所有文件與 Claude Code 2026 年 2 月的新功能同步 (487c96d)
  - 更新所有 10 個教學目錄與 7 個參考文件中的 26 個檔案
  - 新增 **Auto Memory** 文件 — 每個專案的持久化學習內容
  - 新增 **Remote Control**、**Web Sessions** 與 **Desktop App** 文件
  - 新增 **Agent Teams**（實驗性多代理協作）文件
  - 新增 **MCP OAuth 2.0**、**Tool Search** 與 **Claude.ai Connectors** 文件
  - 新增針對子代理的 **Persistent Memory** 與 **Worktree Isolation** 文件
  - 新增 **Background Subagents**、**Task List**、**Prompt Suggestions** 文件
  - 新增 **Sandboxing** 與 **Managed Settings**（企業版）文件
  - 新增 **HTTP Hooks** 與 7 個新的 hook 事件文件
  - 新增 **Plugin Settings**、**LSP Servers** 與 Marketplace 更新文件
  - 新增 **Summarize from Checkpoint** 回溯選項文件
  - 記錄 17 個新的斜線命令 (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast` 等)
  - 記錄新的 CLI 參數 (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode` 等)
  - 記錄用於 auto memory、effort levels、agent teams 等功能的新環境變數

### Design

- 重新設計 Logo 為指南針括號標誌，並採用極簡色調 (20779db)

### Bug Fixes / Corrections

- 更新模型名稱：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修復權限模式名稱：將虛構的 "Unrestricted/Confirm/Read-only" 替換為實際的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- 修復 hook 事件：移除虛構的 `PreCommit`/`PostCommit`/`PrePush`，新增真實事件 (`SubagentStart`, `WorktreeCreate`, `ConfigChange` 等)
- 修復 CLI 語法：將 `claude-code --headless` 替換為 `claude -p` (print mode)
- 修復 checkpoint 命令：將虛構的 `/checkpoint save/list/rewind/diff` 替換為實際的 `Esc+Esc` / `/rewind` 介面
- 修復會話管理：將虛構的 `/session list/new/switch/save` 替換為真實的 `/resume`/`/rename`/`/fork`
- 修復 plugin manifest 格式：將 `plugin.yaml` 遷移至 `.claude-plugin/plugin.json`
- 修復 MCP 設定路徑：`~/.claude/mcp.json` → `.mcp.json` (專案) / `~/.claude.json` (使用者)
- 修復文件 URL：`docs.claude.com` → `docs.anthropic.com`；移除虛構的 `plugins.claude.com`
- 移除多個檔案中的虛構設定欄位
- 將所有「最後更新」日期更新為 2026 年 2 月

**Full Changelog**: https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
