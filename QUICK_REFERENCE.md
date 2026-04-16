<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code Examples - Quick Reference Card

## 🚀 Installation Quick Commands

### 斜線命令
```bash
# Install all
cp 01-slash-commands/*.md .claude/commands/

# Install specific
cp 01-slash-commands/optimize.md .claude/commands/
```

### 記憶
```bash
# Project memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Personal memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### 技能
```bash
# Personal skills
cp -r 03-skills/code-review ~/.claude/skills/

# Project skills
cp -r 03-skills/code-review .claude/skills/
```

### 子代理
```bash
# Install all
cp 04-subagents/*.md .claude/agents/

# Install specific
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# Set credentials
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# Install config (project scope)
cp 05-mcp/github-mcp.json .mcp.json

# Or user scope: add to ~/.claude.json
```

### 鉤子
```bash
# Install hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Configure in settings (~/.claude/settings.json)
```

### 外掛
```bash
# Install from examples (if published)
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### 檢查點
```bash
# Checkpoints are created automatically with every user prompt
# To rewind, press Esc twice or use:
/rewind

# Then choose: Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, or Never mind
```

### Advanced Features

```bash
# Configure in settings (.claude/settings.json)
# See 09-advanced-features/config-examples.json

# Planning mode
/plan Task description

# Permission modes (use --permission-mode flag)
# default        - Ask for approval on risky actions
# acceptEdits    - Auto-accept file edits, ask for others
# plan           - Read-only analysis, no modifications
# dontAsk        - Accept all actions except risky ones
# auto           - Background classifier decides permissions automatically
# bypassPermissions - Accept all actions (requires --dangerously-skip-permissions)

# Session management
/resume                # Resume a previous conversation
/rename "name"         # Name the current session
/fork                  # Fork the current session
claude -c              # Continue most recent conversation
claude -r "session"    # Resume session by name/ID
```

---

## 📋 功能快速參考

| 功能 | 安裝路徑 | 使用方法 |
|---------|-------------|-------|
| **斜線命令 (55+)** | `.claude/commands/*.md` | `/command-name` |
| **記憶** | `./CLAUDE.md` | 自動載入 |
| **技能** | `.claude/skills/*/SKILL.md` | 自動觸發 |
| **子代理** | `.claude/agents/*.md` | 自動委派 |
| **MCP** | `.mcp.json` (專案) 或 `~/.claude.json` (使用者) | `/mcp__server__action` |
| **鉤子 (25 個事件)** | `~/.claude/hooks/*.sh` | 事件觸發 (4 種類型) |
| **外掛** | `/plugin install` | 整合所有 |
| **檢查點** | 內建 | `Esc+Esc` 或 `/rewind` |
| **規劃模式** | 內建 | `/plan <task>` |
| **權限模式 (6 種)** | 內建 | `--allowedTools`, `--permission-mode` |
| **會話** | 內建 | `/session <command>` |
| **背景任務** | 內建 | 在背景執行 |
| **遠端控制** | 內建 | WebSocket API |
| **網頁會話** | 內建 | `claude web` |
| **Git 工作樹** | 內建 | `/worktree` |
| **自動記憶** | 內建 | 自動儲存到 CLAUDE.md |
| **任務清單** | 內建 | `/task list` |
| **內建技能 (5 種)** | 內建 | `/simplify`, `/loop`, `/claude-api`, `/voice`, `/browse` |

---

## 🎯 常用使用案例

### 程式碼審查
```bash
# 方法 1：斜線命令
cp 01-slash-commands/optimize.md .claude/commands/
# 使用方法：/optimize

# 方法 2：子代理
cp 04-subagents/code-reviewer.md .claude/agents/
# 使用方法：自動委派

# 方法 3：技能
cp -r 03-skills/code-review ~/.claude/skills/
# 使用方法：自動觸發

# 方法 4：外掛 (最佳)
/plugin install pr-review
# 使用方法：/review-pr
```

### 文件撰寫
```bash
# 斜線命令
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 子代理
cp 04-subagents/documentation-writer.md .claude/agents/

# 技能
cp -r 03-skills/doc-generator ~/.claude/skills/

# 外掛 (完整解決方案)
/plugin install documentation
```

### DevOps
```bash
# 完整外掛
/plugin install devops-automation

# 命令：/deploy, /rollback, /status, /incident
```

### 團隊標準
```bash
# 專案記憶體
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 編輯以符合您的團隊
vim CLAUDE.md
```

### 自動化與鉤子
```bash
# 安裝鉤子 (25 個事件，4 種類型：命令、HTTP、提示詞、代理)
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 範例：
# - 提交前測試：pre-commit.sh
# - 自動格式化程式碼：format-code.sh
# - 安全性掃描：security-scan.sh

# 自動模式用於完全自主的工作流程
claude --enable-auto-mode -p "重構並測試身份驗證模組"
# 或使用 Shift+Tab 互動式循環模式
```

### 安全重構
```bash
# 每個提示詞之前都會自動建立檢查點
# 嘗試重構
# 如果成功：繼續
# 如果失敗：按下 Esc+Esc 或使用 /rewind 回溯
```

### 複雜實作
```bash
# 使用規劃模式
/plan 實作使用者身份驗證系統

# Claude 建立詳細計畫
# 審查並批准
# Claude 系統性地實作
```

### CI/CD 整合
```bash
# 使用規劃模式
/plan 實作使用者身份驗證系統

# Claude 建立詳細計畫
# 審查並批准
# Claude 系統性地實作
```

# 在無頭模式下執行 (非互動式)
claude -p "執行所有測試並產生報告"

# 搭配權限模式用於 CI
claude -p "執行測試" --permission-mode dontAsk

# 搭配自動模式用於完全自主的 CI 任務
claude --enable-auto-mode -p "執行測試並修復失敗"

# 搭配鉤子進行自動化
# 參見 09-advanced-features/README.md

### 學習與實驗

```bash
# 使用計劃模式進行安全分析
claude --permission-mode plan

# 安全地進行實驗 - 檢查點會自動建立
# 如果需要回溯：按下 Esc+Esc 或使用 /rewind
```

### 代理團隊

```bash
# 啟用代理團隊
export CLAUDE_AGENT_TEAMS=1

# 或在 settings.json 中
{ "agentTeams": { "enabled": true } }

# 開始： "使用團隊方法實施功能 X"
```

### 排程任務

```bash
# 執行命令每 5 分鐘
/loop 5m /check-status

# 一次性提醒
/loop 30m "提醒我檢查部署"
```

---

## 📁 檔案位置參考

```
Your Project/
├── .claude/
│   ├── commands/              # 斜線命令放在這裡
│   ├── agents/                # 子代理放在這裡
│   ├── skills/                # 專案技能放在這裡
│   └── settings.json          # 專案設定 (鉤子等)
├── .mcp.json                  # MCP 設定 (專案範圍)
├── CLAUDE.md                  # 專案記憶
└── src/
    └── api/
        └── CLAUDE.md          # 目錄特定記憶

User Home/
├── .claude/
│   ├── commands/              # 個人斜線命令
│   ├── agents/                # 個人代理
│   ├── skills/                # 個人技能
│   ├── hooks/                 # 鉤子腳本
│   ├── settings.json          # 用戶設定
│   ├── managed-settings.d/    # 企業/組織管理的設定
│   └── CLAUDE.md              # 個人記憶
└── .claude.json               # 個人 MCP 設定 (用戶範圍)
```

## 🔍 範例查找

### 依類別
- **斜線命令**: `01-slash-commands/`
- **記憶**: `02-memory/`
- **技能**: `03-skills/`
- **子代理**: `04-subagents/`
- **MCP**: `05-mcp/`
- **鉤子**: `06-hooks/`
- **外掛**: `07-plugins/`
- **檢查點**: `08-checkpoints/`
- **進階功能**: `09-advanced-features/`
- **CLI**: `10-cli/`

### 依使用案例
- **效能**: `01-slash-commands/optimize.md`
- **安全性**: `04-subagents/secure-reviewer.md`
- **測試**: `04-subagents/test-engineer.md`
- **文件**: `03-skills/doc-generator/`
- **DevOps**: `07-plugins/devops-automation/`

### 依複雜度
- **簡單**: 斜線命令
- **中等**: 子代理, 記憶
- **進階**: 技能, 鉤子
- **完整**: 外掛

---

## 🎓 學習路徑

### 第一天
```bash
# 閱讀總覽
cat README.md

# 安裝命令
cp 01-slash-commands/optimize.md .claude/commands/

# 試用
/optimize
```

### 第二天-第三天
```bash
# 設定記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# 安裝子代理
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 第四天-第五天
```bash
# 設定 MCP
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# 試用 MCP 命令
/mcp__github__list_prs
```

### 第二週
```bash
# 安裝技能
cp -r 03-skills/code-review ~/.claude/skills/

# 讓其自動觸發
# 只要說：「檢查這段程式碼是否有問題」
```

### 第三週+
```bash
# 安裝完整外掛
/plugin install pr-review

# 使用內建功能
/review-pr
/check-security
/check-tests
```

## 新功能 (2026 年 3 月)

| 功能 | 描述 | 使用方法 |
|---------|-------------|-------|
| **自動模式** | 完全自主運作，帶有背景分類器 | `--enable-auto-mode` 參數，`Shift+Tab` 循環切換模式 |
| **頻道** | Discord 和 Telegram 整合 | `--channels` 參數，Discord/Telegram bot |
| **語音輸入** | 對 Claude 說話輸入命令和上下文 | `/voice` 命令 |
| **鉤子 (26 種事件)** | 擴展的鉤子系統，包含 4 種類型 | 命令、HTTP、提示詞、代理鉤子類型 |
| **MCP 提示** | MCP 伺服器可以在執行時請求使用者輸入 | 當伺服器需要澄清時自動提示 |
| **外掛 LSP** | 對外掛支援語言伺服器協定 | `userConfig`，`${CLAUDE_PLUGIN_DATA}` 變數 |
| **遠端控制** | 透過 WebSocket API 控制 Claude Code | `claude --remote` 用於外部整合 |
| **網頁會話** | 基於瀏覽器的 Claude Code 介面 | `claude web` 啟動 |
| **桌面應用程式** | 原生的桌面應用程式 | 從 claude.ai/download 下載 |
| **任務清單** | 管理背景任務 | `/task list`，`/task status <id>` |
| **自動記憶** | 自動從對話中儲存記憶 | Claude 自動將重要上下文儲存到 CLAUDE.md |
| **Git 工作樹** | 平行開發的隔離工作區 | `/worktree` 建立隔離工作區 |
| **模型選擇** | 在 Sonnet 4.6 和 Opus 4.6 之間切換 | `/model` 或 `--model` 參數 |
| **代理團隊** | 協調代理執行任務 | 使用 `CLAUDE_AGENT_TEAMS=1` 環境變數啟用 |
| **排程任務** | 使用 `/loop` 的重複任務 | `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 整合** | 瀏覽器自動化 | `--chrome` 參數或 `/chrome` 命令 |
| **鍵盤自訂** | 自訂按鍵綁定 | `/keybindings` 命令 |

---

## 小技巧與訣竅

### 自訂

- 從範例開始使用
- 修改以符合您的需求
- 在分享給團隊之前進行測試
- 對您的設定進行版本控制

### 最佳實務

- 使用記憶體來記錄團隊標準
- 使用外掛程式來完成工作流程
- 使用子代理來處理複雜任務
- 使用斜線命令來快速處理任務

### 疑難排解

```bash
# 檢查檔案位置
ls -la .claude/commands/
ls -la .claude/agents/

# 驗證 YAML 語法
head -20 .claude/agents/code-reviewer.md

# 測試 MCP 連線
echo $GITHUB_TOKEN
```

---

## 📊 功能矩陣

| 需求 | 使用此功能 | 範例 |
|------|----------|---------|
| 快速捷徑 | 斜線命令 (55+) | `01-slash-commands/optimize.md` |
| 團隊標準 | 記憶 | `02-memory/project-CLAUDE.md` |
| 自動工作流程 | 技能 | `03-skills/code-review/` |
| 專業任務 | 子代理 | `04-subagents/code-reviewer.md` |
| 外部資料 | MCP (+ 提取) | `05-mcp/github-mcp.json` |
| 事件自動化 | 鉤子 (26 個事件，4 個類型) | `06-hooks/pre-commit.sh` |
| 完整解決方案 | 外掛程式 (+ LSP 支援) | `07-plugins/pr-review/` |
| 安全實驗 | 檢查點 | `08-checkpoints/checkpoint-examples.md` |
| 完全自主 | 自動模式 | `--enable-auto-mode` 或 `Shift+Tab` |
| 聊天整合 | 頻道 | `--channels` (Discord, Telegram) |
| CI/CD 流水線 | CLI | `10-cli/README.md` |

---

## 🔗 快速連結

- **主要指南**: `README.md`
- **完整索引**: `INDEX.md`
- **摘要**: `EXAMPLES_SUMMARY.md`
- **原始指南**: `claude_concepts_guide.md`

## 📞 常見問題

**Q: 應該使用哪個？**
A: 從斜線命令開始，然後根據需要新增功能。

**Q: 可以混合使用功能嗎？**
A: 可以！它們可以協同運作。記憶 + 命令 + MCP = 強大。

**Q: 如何與團隊分享？**
A: 將 `.claude/` 目錄提交到 git。

**Q: 關於機密資訊呢？**
A: 使用環境變數，切勿硬編碼。

**Q: 可以修改範例嗎？**
A: 絕對可以！它們是可自訂的範本。

---

## ✅ 檢查清單

開始檢查清單：

- [ ] 閱讀 `README.md`
- [ ] 安裝 1 個斜線命令
- [ ] 嘗試該命令
- [ ] 建立專案 `CLAUDE.md`
- [ ] 安裝 1 個子代理
- [ ] 設定 1 個 MCP 整合
- [ ] 安裝 1 個技能
- [ ] 嘗試完整的外掛
- [ ] 根據您的需求進行自訂
- [ ] 與團隊分享

---

**快速入門**: `cat README.md`

**完整索引**: `cat INDEX.md`

**此卡片**: 隨時備查，方便快速參考！

---
**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/cli-reference
