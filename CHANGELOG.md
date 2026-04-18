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
