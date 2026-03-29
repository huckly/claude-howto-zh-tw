# 更新日誌

## v2.2.0 — 2026-03-26

### 文件

- 將所有教學與參考資料同步至 Claude Code v2.1.84 (f78c094) @luongnv89
  - 將 slash commands 更新為 55+ 個內建指令 + 5 個捆綁 skills，標記 3 個已棄用
  - 將 hook 事件從 18 個擴充至 25 個，新增 `agent` hook 類型（現共 4 種類型）
  - 在進階功能中新增 Auto Mode、Channels、語音聽寫
  - 新增 skill frontmatter 欄位 `effort`、`shell`；agent 欄位 `initialPrompt`、`disallowedTools`
  - 新增 WebSocket MCP transport、elicitation、2KB 工具上限
  - 新增 plugin LSP 支援、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有參考文件（CATALOG、QUICK_REFERENCE、LEARNING-ROADMAP、INDEX）
- 以登陸頁結構重寫 README (32a0776) @luongnv89

### 錯誤修復

- 新增遺漏的 cSpell 詞彙與 README 章節以符合 CI 規範 (93f9d51) @luongnv89
- 將 `Sandboxing` 加入 cSpell 字典 (b80ce6f) @luongnv89

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### 錯誤修復

- 移除導致 CI 連結檢查失敗的失效 marketplace 連結 (3fdf0d6) @luongnv89
- 將 `sandboxed` 與 `pycache` 加入 cSpell 字典 (dc64618) @luongnv89

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 新功能

- 新增自適應學習路徑，包含自我評估與課程測驗 skills (1ef46cd) @luongnv89
  - `/self-assessment` — 跨 10 個功能領域的互動式熟練度測驗，並提供個人化學習路徑
  - `/lesson-quiz [lesson]` — 每堂課的知識測驗，包含 8-10 道針對性題目

### 錯誤修復

- 更新失效 URL、已棄用項目及過時參考資料 (8fe4520) @luongnv89
- 修復 resources 與 self-assessment skill 中的失效連結 (7a05863) @luongnv89
- 在 concepts guide 的巢狀程式碼區塊中使用波浪號圍欄 (5f82719) @VikalpP
- 將遺漏詞彙加入 cSpell 字典 (8df7572) @luongnv89

### 文件

- 第 5 階段品質保證 — 修復文件間的一致性、URL 與術語問題 (00bbe4c) @luongnv89
- 完成第 3-4 階段 — 新功能說明與參考文件更新 (132de29) @luongnv89
- 在 MCP 情境膨脹章節中加入 MCPorter runtime (ef52705) @luongnv89
- 在 6 份指南中補充缺失的指令、功能與設定 (4bc8f15) @luongnv89
- 根據現有儲存庫慣例新增樣式指南 (84141d0) @luongnv89
- 在指南比較表中新增 self-assessment 列 (8fe0c96) @luongnv89
- 在貢獻者名單中加入 VikalpP（PR #7）(d5b4350) @luongnv89
- 在 README 與 roadmap 中新增 self-assessment 與 lesson-quiz skill 參考 (d5a6106) @luongnv89

### 新貢獻者

- @VikalpP 在 #7 中完成首次貢獻

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 新功能

- 將所有文件同步至 Claude Code 2026 年 2 月功能 (487c96d)
  - 更新 10 個教學目錄與 7 份參考文件，共 26 個檔案
  - 新增 **Auto Memory** 文件 — 每個專案的持久性學習記錄
  - 新增 **Remote Control**、**Web Sessions** 與 **Desktop App** 文件
  - 新增 **Agent Teams**（實驗性多 agent 協作）文件
  - 新增 **MCP OAuth 2.0**、**Tool Search** 與 **Claude.ai Connectors** 文件
  - 新增 subagents 的 **Persistent Memory** 與 **Worktree Isolation** 文件
  - 新增 **Background Subagents**、**Task List**、**Prompt Suggestions** 文件
  - 新增 **Sandboxing** 與 **Managed Settings**（企業版）文件
  - 新增 **HTTP Hooks** 與 7 個新 hook 事件文件
  - 新增 **Plugin Settings**、**LSP Servers** 與 Marketplace 更新文件
  - 記錄 Checkpoint 的 **Summarize from Checkpoint** 倒帶選項
  - 記錄 17 個新 slash commands（`/fork`、`/desktop`、`/teleport`、`/tasks`、`/fast` 等）
  - 記錄新 CLI 旗標（`--worktree`、`--from-pr`、`--remote`、`--teleport`、`--teammate-mode` 等）
  - 記錄 auto memory、effort levels、agent teams 等的新環境變數

### 設計

- 重新設計 logo，採用羅盤括號標誌與簡約色盤 (20779db)

### 錯誤修復 / 更正

- 更新模型名稱：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修正 permission mode 名稱：以實際的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions` 取代虛構的「Unrestricted/Confirm/Read-only」
- 修正 hook 事件：移除虛構的 `PreCommit`/`PostCommit`/`PrePush`，新增真實事件（`SubagentStart`、`WorktreeCreate`、`ConfigChange` 等）
- 修正 CLI 語法：以 `claude -p`（print 模式）取代 `claude-code --headless`
- 修正 checkpoint 指令：以實際的 `Esc+Esc` / `/rewind` 介面取代虛構的 `/checkpoint save/list/rewind/diff`
- 修正 session 管理：以真實的 `/resume`/`/rename`/`/fork` 取代虛構的 `/session list/new/switch/save`
- 修正 plugin manifest 格式：從 `plugin.yaml` 遷移至 `.claude-plugin/plugin.json`
- 修正 MCP 設定路徑：`~/.claude/mcp.json` → `.mcp.json`（專案）/ `~/.claude.json`（使用者）
- 修正文件 URL：`docs.claude.com` → `docs.anthropic.com`；移除虛構的 `plugins.claude.com`
- 移除多個檔案中的虛構設定欄位
- 將所有「最後更新」日期更新至 2026 年 2 月

**完整更新日誌**：https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0
