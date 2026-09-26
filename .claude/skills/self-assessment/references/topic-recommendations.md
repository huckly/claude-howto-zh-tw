# 主題專屬建議

當某個主題是能力缺口時，使用以下的具體建議。路徑相對於此參考檔（`.claude/skills/self-assessment/references/`）。

**Slash Commands（分數 0）**：
- 教學：[01-slash-commands/](../../../../01-slash-commands/)
- 重點：內建指令參考、建立你的第一個 SKILL.md、`$ARGUMENTS` 語法
- 關鍵練習：建立一個 `/optimize` 指令並測試它
- 完成標準：你能建立一個帶有參數與動態情境的自訂 skill

**Slash Commands（分數 1 — 複習）**：
- 重點：以 `!`反引號`` 語法帶入動態情境、`@file` 引用、`disable-model-invocation` 與 `user-invocable` 的控制差異
- 完成標準：你能建立一個會注入即時指令輸出、並自行控制觸發行為的 skill

**Memory（分數 0）**：
- 教學：[02-memory/](../../../../02-memory/)
- 重點：建立 CLAUDE.md、`/init` 與 `/memory` 指令、以 `#` 前綴快速更新
- 關鍵練習：建立一個包含你程式碼規範的專案 CLAUDE.md
- 完成標準：Claude 能跨工作階段記住你的偏好

**Memory（分數 1 — 複習）**：
- 重點：7 個記憶位置，以及它們如何串接進情境（由根目錄往下載入，而非互相覆蓋）、含路徑專屬規則的 .claude/rules/ 目錄、`@import` 語法（最大深度 4）、Auto Memory MEMORY.md（Claude 會載入其前 200 行或前 25 KB，以先達到者為準 — 這是載入上限，而非檔案大小限制）
- 完成標準：你為不同目錄建立了模組化規則，並理解每個記憶檔案如何串接進情境

**Skills（分數 0）**：
- 教學：[03-skills/](../../../../03-skills/)
- 重點：SKILL.md 格式、透過 description 欄位自動觸發、漸進式揭露（3 個載入層級）
- 關鍵練習：安裝 code-review skill 並確認它會自動觸發
- 完成標準：某個 skill 能根據對話情境自動啟用

**Skills（分數 1 — 複習）**：
- 重點：以 `context: fork` 搭配 `agent` 欄位在子代理中執行、`disable-model-invocation` 與 `user-invocable`、skill 清單預算（情境視窗的 1%，備用值 8,000 字元，每個項目 250 字元）、隨附資源（scripts/、references/、assets/）
- 完成標準：你能建立一個在子代理中以 forked 情境執行的 skill

**Hooks（分數 0）**：
- 教學：[06-hooks/](../../../../06-hooks/)
- 重點：設定結構（matcher + hooks 陣列）、PreToolUse/PostToolUse 事件、離開碼（0=成功、2=阻擋）、JSON 輸入/輸出格式
- 關鍵練習：建立一個驗證 Bash 指令的 PreToolUse hook
- 完成標準：某個 hook 能在執行前阻擋危險指令

**Hooks（分數 1 — 複習）**：
- 重點：全部 33 種 hook 事件（含 PostToolUseFailure、StopFailure、TaskCreated、CwdChanged、FileChanged、PostCompact、Elicitation、ElicitationResult、Setup、UserPromptExpansion、MessageDisplay、PreModelSwitch、PostModelSwitch — 最後兩個於 v2.1.251 新增）、5 種 hook 類型（command、http、mcp_tool、prompt、agent — agent hooks 仍屬實驗性質，未來可能變動）、SKILL.md frontmatter 中的元件範圍 hook、帶 allowedEnvVars 的 HTTP hook、供 SessionStart/CwdChanged/FileChanged 使用的 `CLAUDE_ENV_FILE`
- 完成標準：你能建立一個 prompt 型的 Stop hook，以及一個 skill 內的元件範圍 hook

**MCP（分數 0）**：
- 教學：[05-mcp/](../../../../05-mcp/)
- 重點：`claude mcp add` 指令、傳輸類型（建議用 `http`，另有 `stdio`、適用於推送型伺服器的 `ws`，以及已棄用的 `sse` — 注意 `--transport` 不接受 `ws`，因此 WebSocket 伺服器要用 `claude mcp add-json` 新增）、GitHub MCP 設定、環境變數展開
- 關鍵練習：新增 GitHub MCP 伺服器並查詢 PR
- 完成標準：你能透過 MCP 從外部服務查詢即時資料

**MCP（分數 1 — 複習）**：
- 重點：專案範圍 .mcp.json（需團隊核准）、OAuth 2.0 驗證、以 `@server:resource` 提及的 MCP 資源、Tool Search（ENABLE_TOOL_SEARCH）、`claude mcp serve`、輸出上限（10,000 tokens 時警告；透過 `MAX_MCP_OUTPUT_TOKENS` 設定的預設上限 25,000 tokens；50,000 字元的磁碟持久化門檻）
- 完成標準：你有一份專案 .mcp.json，並理解 Tool Search 的 auto 模式

**Subagents（分數 0）**：
- 教學：[04-subagents/](../../../../04-subagents/)
- 重點：Agent 檔案格式（.claude/agents/*.md）、內建代理（Explore、Plan、general-purpose、claude、statusline-setup、claude-code-guide）、tools/model/permissionMode 設定、產生數量限制（自 v2.1.219 起預設深度為 3、預設並行數為 20，每個工作階段 200 個的產生上限已於 v2.1.224 移除）
- 關鍵練習：建立一個 code-reviewer 子代理並測試委派
- 完成標準：Claude 會把程式碼審查委派給你的自訂代理

**Subagents（分數 1 — 複習）**：
- 重點：Worktree 隔離（`isolation: worktree`）、持久化代理記憶（帶範圍的 `memory` 欄位）、背景代理（Ctrl+B/Ctrl+F）、以 `Agent(agent_type)` 設定代理允許清單（`Task(...)` 仍保留為向後相容的別名）、代理團隊（`--teammate-mode`）
- 完成標準：你有一個在 worktree 隔離中執行、且具持久化記憶的子代理

**Checkpoints（分數 0）**：
- 教學：[08-checkpoints/](../../../../08-checkpoints/)
- 重點：以 Esc+Esc 與 /rewind 存取、6 種 rewind 選項（還原程式碼與對話、還原對話、還原程式碼、從此處摘要、摘要至此處、取消）、限制 — bash 檔案系統操作、子代理的編輯（前景執行的 `context: fork` skill 除外）、在 Claude Code 之外所做的編輯，以及符號連結/硬連結的路徑，都不會被追蹤
- 關鍵練習：做一些實驗性變更，再 rewind 還原
- 完成標準：你能放心實驗，因為知道隨時可以 rewind

**Advanced Features（分數 0）**：
- 教學：[09-advanced-features/](../../../../09-advanced-features/)
- 重點：規劃模式（/plan 或 Shift+Tab）、權限模式（6 種：manual — 於 v2.1.200 由 default 更名 — acceptEdits、plan、auto、dontAsk、bypassPermissions）、延伸思考（Alt+T 切換）
- 關鍵練習：用規劃模式設計一個功能，再實作它
- 完成標準：你能在規劃與實作模式間流暢切換

**Advanced Features（分數 1 — 複習）**：
- 重點：遠端控制（`claude --remote-control`，別名 `--rc`）、網頁工作階段（`claude --cloud`；`--remote` 是已棄用的別名）、桌面交接（`/desktop`）、worktrees（`claude -w`）、任務清單（Ctrl+T）、企業用託管設定
- 完成標準：你能在 CLI、網頁與桌面之間交接工作階段

**Plugins（分數 0）**：
- 教學：[07-plugins/](../../../../07-plugins/)
- 重點：Plugin 結構（.claude-plugin/plugin.json）、plugin 能捆綁什麼（skills、agents、MCP、hooks、settings — 另有舊式的 `commands/` 目錄，仍可運作，但新的 plugins 建議使用 `skills/`）、從市集安裝
- 關鍵練習：安裝一個 plugin 並探索它的元件
- 完成標準：你理解何時該用 plugin、何時該用獨立元件

**Plugins（分數 1 — 複習）**：
- 重點：建立 plugin.json 資訊清單、plugin hooks（hooks/hooks.json）、LSP 設定（.lsp.json）、`${CLAUDE_PLUGIN_ROOT}` 變數、以 --plugin-dir 測試、發佈到市集
- 完成標準：你能為團隊建立並測試一個 plugin

**CLI（分數 0）**：
- 教學：[10-cli/](../../../../10-cli/)
- 重點：互動模式 vs print 模式、`claude -p` 搭配管線、`--output-format json`、工作階段管理（-c/-r）
- 關鍵練習：把一個檔案管線傳給 `claude -p` 並取得 JSON 輸出
- 完成標準：你能在腳本中非互動地使用 Claude

**CLI（分數 1 — 複習）**：
- 重點：帶 JSON 設定的 --agents 旗標、供結構化輸出的 --json-schema、--fallback-model、--from-pr、--strict-mcp-config、以 for 迴圈批次處理、`claude mcp serve`
- 完成標準：你有一個使用 Claude 並產出結構化 JSON 輸出的 CI/CD 腳本

---

**最後更新日期**：2026 年 9 月 2 日
**Claude Code 版本**：2.1.257
**來源**：
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/mcp
- https://code.claude.com/docs/en/sub-agents
- https://code.claude.com/docs/en/checkpointing
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/plugins-reference
