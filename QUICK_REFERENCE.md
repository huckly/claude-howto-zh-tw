<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code Examples - 快速參考卡

## 🚀 安裝快速指令

### 斜線命令
```bash
# 安裝全部
cp 01-slash-commands/*.md .claude/commands/

# 安裝特定項目
cp 01-slash-commands/optimize.md .claude/commands/
```

### 記憶
```bash
# 專案記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 個人記憶
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

### 技能
```bash
# 個人技能
cp -r 03-skills/code-review ~/.claude/skills/

# 專案技能
cp -r 03-skills/code-review .claude/skills/
```

### 子代理
```bash
# 安裝全部
cp 04-subagents/*.md .claude/agents/

# 安裝特定項目
cp 04-subagents/code-reviewer.md .claude/agents/
```

### MCP
```bash
# 設定憑證
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安裝設定 (專案範圍)
cp 05-mcp/github-mcp.json .mcp.json

# 或使用者範圍：新增至 ~/.claude.json
```

### 鉤子
```bash
# 安裝鉤子
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在設定中進行配置 (~/.claude/settings.json)
```

### 外掛
```bash
# 從範例安裝 (若已發佈)
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

### 檢查點
```bash
# 檢查點會在每次使用者輸入提示詞時自動建立
# 若要回溯，請按兩次 Esc 或使用：
/rewind

# 然後選擇：Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, 或 Never mind
```

### 進階功能
```bash
# 在設定中進行配置 (.claude/settings.json)
# 請參閱 09-advanced-features/config-examples.json

# 規劃模式
/plan Task description

# 權限模式 (使用 --permission-mode 旗標)
# default        - 對於風險行為請求核准
# acceptEdits    - 自動接受檔案編輯，其他行為則請求核准
# plan           - 唯讀分析，不進行修改
# dontAsk        - 除了風險行為外，接受所有動作
# auto           - 由背景分類器自動決定權限
# bypassPermissions - 接受所有動作 (需要 --dangerously-skip-permissions)

# 會話管理
/resume                # 恢復先前的會話
/rename "name"         # 為當前會話命名
/fork                  # 分叉當前會話
claude -c              # 繼續最近一次的會話
claude -r "session"    # 透過名稱/ID 恢復會話
```

---

## 📋 功能速查表

| 功能 | 安裝路徑 | 用法 |
|---------|-------------|-------|
| **斜線命令 (55+)** | `.claude/commands/*.md` | `/command-name` |
| **記憶** | `./CLAUDE.md` | 自動載入 |
| **技能** | `.claude/skills/*/SKILL.md` | 自動調用 |
| **子代理** | `.claude/agents/*.md` | 自動委派 |
| **MCP** | `.mcp.json` (專案) 或 `~/.claude.json` (使用者) | `/mcp__server__action` |
| **鉤子 (25 個事件)** | `~/.claude/hooks/*.sh` | 事件觸發 (4 種類型) |
| **外掛** | 透過 `/plugin install` | 整合所有功能 |
| **檢查點** | 內建 | `Esc+Esc` 或 `/rewint` |
| **規劃模式** | 內建 | `/plan <task>` |
| **權限模式 (6 種)** | 內建 | `--allowedTools`, `--permission-mode` |
| **會話** | 內建 | `/session <command>` |
| **背景任務** | 內建 | 在背景執行 |
| **遠端控制** | 內建 | WebSocket API |
| **網頁會話** | 內建 | `claude web` |
| **Git Worktrees** | 內建 | `/worktree` |
| **自動記憶** | 內建 | 自動儲存至 CLAUDE.md |
| **任務列表** | 內建 | `/task list` |
| **內建技能 (5 種)** | 內建 | `/simplify`, `/loop`, `/claude-api`, `/voice`, `/browse` |

---

## 🎯 常見使用情境

### Code Review
```bash
# 方法 1：斜線命令
cp 01-slash-commands/optimize.md .claude/commands/
# 用法：/optimize

# 方法 2：子代理
cp 04-subagents/code-reviewer.md .claude/agents/
# 用法：自動委派

# 方法 3：技能
cp -r 03-skills/code-review ~/.claude/skills/
# 用法：自動調用

# 方法 4：外掛 (最佳方案)
/plugin install pr-review
# 用法：/review-pr
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
# 專案記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 為您的團隊進行編輯
vim CLAUDE.md
```

### 自動化與鉤子
```bash
# 安裝鉤子 (25 個事件，4 種類型：command, http, prompt, agent)
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 範例：
# - Pre-commit 測試：pre-commit.sh
# - 自動格式化程式碼：format-code.sh
# - 安全掃描：security-scan.sh

# 用於完全自主工作流程的自動模式
claude --enable-auto-mode -p "Refactor and test the auth module"
# 或者使用 Shift+Tab 互動式切換模式
```

### 安全重構
```bash
# 在每個提示詞之前會自動建立檢查點
# 嘗試進行重構
# 如果成功：繼續
# 如果失敗：按下 Esc+Esc 或使用 /rewind 回溯
```

### 複雜實作
```bash
# 使用規劃模式
/plan Implement user authentication system

# Claude 會建立詳細的計畫
# 審查並核准
# Claude 系統性地進行實作
```

### CI/CD 整合

```bash
# 以無頭模式（非互動式）執行
claude -p "Run all tests and generate report"

# 使用 CI 專用的權限模式
claude -p "Run tests" --permission-mode dontAsk

# 使用 Auto Mode 進行完全自主的 CI 任務
claude --enable-auto-mode -p "Run tests and fix failures"

# 使用 hooks 進行自動化
# 請參閱 09-advanced-features/README.md
```

### 學習與實驗
```bash
# 使用 plan mode 進行安全分析
claude --permission-mode plan

# 安全地進行實驗 - 會自動建立 checkpoints
# 如果需要回溯：請按 Esc+Esc 或使用 /rewind
```

### Agent Teams
```bash
# 啟用 agent teams
export CLAUDE_AGENT_TEAMS=1

# 或在 settings.json 中設定
{ "agentTeams": { "enabled": true } }

# 開始指令範例："Implement feature X using a team approach"
```

### 排程任務
```bash
# 每 5 分鐘執行一次指令
/loop 5m /check-status

# 單次提醒
/loop 30m "remind me to check the deploy"
```

---

## 📁 檔案位置參考

```
Your Project/
├── .claude/
│   ├── commands/              # 斜線命令存放於此
│   ├── agents/                # 子代理存放於此
│   ├── skills/                # 專案技能存放於此
│   └── settings.json          # 專案設定 (hooks 等)
├── .mcp.json                  # MCP 配置 (專案範圍)
├── CLAUDE.md                  # 專案記憶
└── src/
    └── api/
        └── CLAUDE.md          # 特定目錄的記憶

User Home/
├── .claude/
│   ├── commands/              # 個人指令
│   ├── agents/                # 個人代理
│   ├── skills/                # 個人技能
│   ├── hooks/                 # Hook 腳本
│   ├── settings.json          # 使用者設定
│   ├── managed-settings.d/    # 受管制的設定 (企業/組織)
│   └── CLAUDE.md              # 個人記憶
└── .claude.json               # 個人 MCP 配置 (使用者範圍)
```

---

## 🔍 尋找範例

### 按類別
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

### 按使用情境
- **效能**: `01-slash-commands/optimize.md`
- **安全性**: `04-subagents/secure-reviewer.md`
- **測試**: `04-subagents/test-engineer.md`
- **文件**: `03-skills/doc-generator/`
- **DevOps**: `07-plugins/devops-automation/`

### 按複雜度
- **簡單**: 斜線命令
- **中等**: 子代理、記憶
- **進階**: 技能、鉤子
- **完整**: 外掛

---

## 🎓 學習路徑

### 第 1 天
```bash
# 閱讀概觀
cat README.md

# 安裝指令
cp 01-slash-commands/optimize.md .claude/commands/

# 嘗試使用
/optimize
```

### 第 2-3 天
```bash
# 設定記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
vim CLAUDE.md

# 安裝子代理
cp 04-subagents/code-reviewer.md .claude/agents/
```

### 第 4-5 天
```bash
# 設定 MCP
export GITHUB_TOKEN="your_token"
cp 05-mcp/github-mcp.json .mcp.json

# 嘗試 MCP 指令
/mcp__github__list_prs
```

### 第二週
```bash
# 安裝技能
cp -r 03-skills/code-review ~/.claude/skills/

# 讓它自動觸發
# 只要說：「Review this code for issues」
```

### 第三週以上
```bash
# 安裝完整外掛
/plugin install pr-review

# 使用內建功能
/review-pr
/check-security
/check-tests
```

---

## 新功能 (2026 年 3 月)

| 功能 | 描述 | 使用方式 |
|---------|-------------|-------|
| **Auto Mode** | 具備背景分類器的全自動運作模式 | 使用 `--enable-auto-mode` 旗標，按 `Shift+Tab` 切換模式 |
| **Channels** | Discord 與 Telegram 整合 | 使用 `--channels` 旗標，透過 Discord/Telegram bots |
| **Voice Dictation** | 向 Claude 說出指令與上下文 | 使用 `/voice` 命令 |
| **Hooks (26 events)** | 擴展後的鉤子系統，包含 4 種類型 | 包含 command、http、prompt、agent 鉤子類型 |
| **MCP Elicitation** | MCP servers 可在執行時請求使用者輸入 | 當伺服器需要釐清資訊時會自動提示 |
| **Plugin LSP** | 為外掛提供 Language Server Protocol 支援 | 使用 `userConfig` 與 `${CLAUDE_PLUGIN_DATA}` 變數 |
| **Remote Control** | 透過 WebSocket API 控制 Claude Code | 使用 `claude --remote` 進行外部整合 |
| **Web Sessions** | 基於瀏覽器的 Claude Code 介面 | 使用 `claude web` 啟動 |
| **Desktop App** | 原生桌面應用程式 | 從 claude.ai/download 下載 |
| **Task List** | 管理背景任務 | `/task list`、`/task status <id>` |
| **Auto Memory** | 從對話中自動儲存記憶 | Claude 會將關鍵上下文自動儲存至 CLAUDE.md |
| **Git Worktrees** | 用於平行開發的隔離工作空間 | 使用 `/worktree` 建立隔離的工作空間 |
| **Model Selection** | 在 Sonnet 4.6、Opus 4.7 與 Haiku 4.5 之間切換 | 使用 `/model` 或 `--model` 旗標 |
| **Agent Teams** | 在任務中協調多個代理 | 透過設定環境變數 `CLAUDE_AGENT_TEAMS=1` 啟用 |
| **Scheduled Tasks** | 使用 `/loop` 進行週期性任務 | `/loop 5m /command` 或使用 CronCreate 工具 |
| **Chrome Integration** | 瀏覽器自動化 | 使用 `--chrome` 旗標或 `/chrome` 命令 |
| **Keyboard Customization** | 自定義鍵位綁定 | 使用 `/keybindings` 命令 |

---

## 技巧與訣竅

### 自定義
- 從範例開始直接使用
- 根據您的需求進行修改
- 與團隊分享前先進行測試
- 對您的配置進行版本控制

### 最佳實踐
- 使用 memory 來建立團隊標準
- 使用 plugins 來實現完整的工作流程
- 使用 subagents 來處理複雜任務
- 使用斜線命令 來執行快速任務

### 除錯
```bash
# Check file locations
ls -la .claude/commands/
ls -la .claude/agents/

# Verify YAML syntax
head -20 .claude/agents/code-reviewer.md

# Test MCP connection
echo $GITHUB_TOKEN
```

---

## 📊 功能矩陣

| 需求 | 使用此功能 | 範例 |
|------|----------|---------|
| 快速捷徑 | 斜線命令 (55+) | `01-slash-commands/optimize.md` |
| 團隊標準 | memory | `02-memory/project-CLAUDE.md` |
| 自動工作流程 | 技能 | `03-skills/code-review/` |
| 特殊任務 | 子代理 | `04-subagents/code-reviewer.md` |
| 外部數據 | MCP (+ Elicitation) | `05-mcp/github-mcp.json` |
| 事件自動化 | 鉤子 (26 個事件, 4 種類型) | `06-hooks/pre-commit.sh` |
| 完整解決方案 | 外掛 (+ LSP 支援) | `07-plugins/pr-review/` |
| 安全實驗 | 檢查點 | `08-checkpoints/checkpoint-examples.md` |
| 完全自主 | 自動模式 | `--enable-auto-mode` 或 `Shift+Tab` |
| 聊天整合 | 頻道 | `--channels` (Discord, Telegram) |
| CI/CD 流水線 | CLI | `10-cli/README.md` |

---

## 🔗 快速連結

- **主指南**: `README.md`
- **完整索引**: `INDEX.md`
- **摘要**: `EXAMPLES_SUMMARY.md`
- **原始指南**: `claude_concepts_guide.md`

---

## 📞 常見問題

**Q: 我應該使用哪一個？**
A: 從斜線命令開始，並根據需要增加功能。

**Q: 我可以混合使用不同功能嗎？**
A: 可以！它們可以協同工作。記憶 + 命令 + MCP = 強大的組合。

**Q: 如何與團隊分享？**
A: 將 `.claude/` 目錄提交至 git。

**Q: 關於敏感資訊（secrets）該怎麼辦？**
A: 使用環境變數，絕不要將其寫死在程式碼中。

**Q: 我可以修改範例嗎？**
A: 當然可以！它們是用來客製化的範本。

---

## ✅ 檢查清單

入門檢查清單：

- [ ] 閱讀 `README.md`
- [ ] 安裝 1 個斜線命令
- [ ] 嘗試該命令
- [ ] 建立專案 `CLAUDE.md`
- [ ] 安裝 1 個子代理
- [ ] 設定 1 個 MCP 整合
- [ ] 安裝 1 個技能
- [ ] 嘗試一個完整的外掛
- [ ] 根據您的需求進行客製化
- [ ] 與團隊分享

---

**快速開始**: `cat README.md`

**完整索引**: `cat INDEX.md`

**此卡片**: 請妥善保存以便快速查閱！

---
**最後更新日期**: 2026 年 4 月 16 日
**Claude Code 版本**: 2.1.112
**來源**:
- https://docs.anthropic.com/en/docs/claude-code
- https://www.anthropic.com/news/claude-opus-4-7
- https://support.claude.com/en/articles/12138966-release-notes
**相容模型**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
