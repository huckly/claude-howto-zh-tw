# 深度評估 — 各輪題目

深度評估的 5 個輪次。每一輪呈現一次 AskUserQuestion 呼叫（多選，最多 4 個選項）。每輪涵蓋 2 個功能領域，各 2 個選項。每輪的計分對照表在 SKILL.md 的 Step 2B 中重複列出，以便無需重讀本檔即可計算結果。

---

**第 1 輪 — Slash Commands 與 Memory**（標題："Commands"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「建立過自訂 slash 指令或 skill」— 寫過帶 frontmatter 的 SKILL.md 檔，或建立過 .claude/commands/ 檔案
2. 「在指令中使用過動態情境」— 在 skill/指令檔中用過 `$ARGUMENTS`、`$0`/`$1`、反引號 `!command` 語法，或 `@file` 引用
3. 「設定過專案 + 個人記憶」— 同時建立了專案 CLAUDE.md 與個人 ~/.claude/CLAUDE.md（或 CLAUDE.local.md）
4. 「使用過記憶階層功能」— 理解 7 個記憶位置如何串接進情境（由根目錄往下載入、每一層的 CLAUDE.local.md 附加在 CLAUDE.md 之後、子目錄檔案按需載入）、用過 .claude/rules/ 目錄、路徑專屬規則，或 @import 語法

**計分：** 選項 1-2 → **Slash Commands**（0-2）；選項 3-4 → **Memory**（0-2）

---

**第 2 輪 — Skills 與 Hooks**（標題："Automation"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「安裝並使用過自動觸發的 skill」— 一個根據 description 自動觸發、無需手動 /command 呼叫的 skill
2. 「控制過 skill 的觸發行為」— 在 SKILL.md frontmatter 中用過 `disable-model-invocation`、`user-invocable`，或帶 agent 欄位的 `context: fork`
3. 「設定過 PreToolUse 或 PostToolUse hook」— 設定過在工具執行前/後執行的 hook（例如指令驗證器、自動格式化）
4. 「使用過進階 hook 功能」— 設定過 prompt 型 hook、SKILL.md 中的元件範圍 hook、HTTP hook，或帶自訂 JSON 輸出（updatedInput、systemMessage）的 hook

**計分：** 選項 1-2 → **Skills**（0-2）；選項 3-4 → **Hooks**（0-2）

---

**第 3 輪 — MCP 與 Subagents**（標題："Integration"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「連接過 MCP 伺服器並使用其工具」— 例如用 GitHub MCP 處理 PR/issue、用資料庫 MCP 查詢，或任何外部資料來源
2. 「使用過進階 MCP 功能」— 專案範圍 .mcp.json、OAuth 驗證、以 @提及 的 MCP 資源、Tool Search，或 `claude mcp serve`
3. 「建立或設定過自訂子代理」— 在 .claude/agents/ 中定義帶自訂 tools、model 或權限的代理
4. 「使用過進階子代理功能」— Worktree 隔離、持久化代理記憶、以 Ctrl+B 執行背景任務、以 `Agent(agent_type)` 設定代理允許清單（`Task(...)` 形式是為了向後相容而保留的別名），或代理團隊

**計分：** 選項 1-2 → **MCP**（0-2）；選項 3-4 → **Subagents**（0-2）

---

**第 4 輪 — Checkpoints 與 Advanced Features**（標題："Power User"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「用 checkpoints 做安全實驗」— 建立過 checkpoint、用過 Esc+Esc 或 /rewind、還原過程式碼與/或對話，或用過任一種 Summarize 選項（summarize from here / summarize up to here）
2. 「使用過規劃模式或延伸思考」— 透過 /plan、Shift+Tab 或 --permission-mode plan 啟用規劃；用 Alt+T/Option+T 切換延伸思考
3. 「設定過權限模式」— 透過 CLI 旗標、鍵盤快捷鍵或設定使用過六種模式中的任一種 — manual（於 v2.1.200 由 default 更名）、acceptEdits、plan、auto、dontAsk 或 bypassPermissions
4. 「使用過遠端/桌面/網頁功能」— 用過 `claude --remote-control`、`claude --cloud`、`/teleport`、`/desktop`，或以 `claude -w` 使用 worktrees

**計分：** 選項 1 → **Checkpoints**（0-1）；選項 2-4 → **Advanced Features**（0-3）

---

**第 5 輪 — Plugins 與 CLI**（標題："Mastery"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「安裝或建立過 plugin」— 用過市集捆綁的 plugin，或建立過含 plugin.json 資訊清單的 .claude-plugin/ 目錄
2. 「使用過 plugin 進階功能」— Plugin skills（`skills/` — 建議的形式；命名空間的 `commands/` 屬舊式做法但仍可運作）、plugin hooks、plugin MCP 伺服器、LSP 設定，或用於測試的 --plugin-dir 旗標
3. 「在腳本或 CI/CD 中使用過 print 模式」— 用過 `claude -p` 搭配 --output-format json、--max-turns、管線輸入，或整合進 GitHub Actions / CI 流程
4. 「使用過進階 CLI 功能」— 工作階段續接（-c/-r）、--agents 旗標、供結構化輸出的 --json-schema、--fallback-model、--from-pr，或批次處理迴圈

**計分：** 選項 1-2 → **Plugins**（0-2）；選項 3-4 → **CLI**（0-2）

---

**最後更新日期**：2026 年 9 月 2 日
**Claude Code 版本**：2.1.257
**來源**：
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/sub-agents
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/plugins-reference
