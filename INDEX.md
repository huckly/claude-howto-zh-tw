<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 範例 — 完整索引

本文件提供所有範例檔案的完整索引，依功能類型組織。

## 統計摘要

- **檔案總數**：100+ 個檔案
- **類別**：10 個功能類別
- **外掛**：3 個完整外掛
- **Skills**：6 個完整 skills
- **Hooks**：8 個範例 hooks
- **可立即使用**：所有範例

---

## 01. 斜線指令（10 個檔案）

使用者呼叫的常用工作流程快捷方式。

| 檔案 | 說明 | 使用情境 |
|------|-------------|----------|
| `optimize.md` | 程式碼最佳化分析器 | 找出效能問題 |
| `pr.md` | Pull request 準備 | PR 工作流程自動化 |
| `generate-api-docs.md` | API 文件生成器 | 生成 API 文件 |
| `commit.md` | Commit 訊息助手 | 標準化提交 |
| `setup-ci-cd.md` | CI/CD 流程設置 | DevOps 自動化 |
| `push-all.md` | 推送所有變更 | 快速推送工作流程 |
| `unit-test-expand.md` | 擴展單元測試覆蓋率 | 測試自動化 |
| `doc-refactor.md` | 文件重構 | 文件改善 |
| `pr-slash-command.png` | 截圖範例 | 視覺參考 |
| `README.md` | 文件 | 設置和使用指南 |

**安裝路徑**：`.claude/commands/`

**使用方式**：`/optimize`, `/pr`, `/generate-api-docs`, `/commit`, `/setup-ci-cd`, `/push-all`, `/unit-test-expand`, `/doc-refactor`

---

## 02. Memory（6 個檔案）

持久化上下文和專案標準。

| 檔案 | 說明 | 範圍 | 位置 |
|------|-------------|-------|----------|
| `project-CLAUDE.md` | 團隊專案標準 | 整個專案 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 特定規則 | 目錄 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 個人偏好 | 使用者 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | 截圖：memory 已儲存 | - | 視覺參考 |
| `memory-ask-claude.png` | 截圖：詢問 Claude | - | 視覺參考 |
| `README.md` | 文件 | - | 參考 |

**安裝**：複製至適當位置

**使用方式**：由 Claude 自動載入

---

## 03. Skills（28 個檔案）

自動呼叫的能力，包含腳本和模板。

### Code Review Skill（5 個檔案）
```
code-review/
├── SKILL.md                          # Skill definition
├── scripts/
│   ├── analyze-metrics.py            # Code metrics analyzer
│   └── compare-complexity.py         # Complexity comparison
└── templates/
    ├── review-checklist.md           # Review checklist
    └── finding-template.md           # Finding documentation
```

**用途**：包含安全性、效能和品質分析的全面程式碼審查

**自動呼叫時機**：審查程式碼時

---

### Brand Voice Skill（4 個檔案）
```
brand-voice/
├── SKILL.md                          # Skill definition
├── templates/
│   ├── email-template.txt            # Email format
│   └── social-post-template.txt      # Social media format
└── tone-examples.md                  # Example messages
```

**用途**：確保溝通中品牌聲音的一致性

**自動呼叫時機**：建立行銷文案時

---

### Documentation Generator Skill（2 個檔案）
```
doc-generator/
├── SKILL.md                          # Skill definition
└── generate-docs.py                  # Python doc extractor
```

**用途**：從原始碼生成完整的 API 文件

**自動呼叫時機**：建立／更新 API 文件時

---

### Refactor Skill（5 個檔案）
```
refactor/
├── SKILL.md                          # Skill definition
├── scripts/
│   ├── analyze-complexity.py         # Complexity analyzer
│   └── detect-smells.py              # Code smell detector
├── references/
│   ├── code-smells.md                # Code smells catalog
│   └── refactoring-catalog.md        # Refactoring patterns
└── templates/
    └── refactoring-plan.md           # Refactoring plan template
```

**用途**：包含複雜度分析的系統性程式碼重構

**自動呼叫時機**：重構程式碼時

---

### Claude MD Skill（1 個檔案）
```
claude-md/
└── SKILL.md                          # Skill definition
```

**用途**：管理和最佳化 CLAUDE.md 檔案

---

### Blog Draft Skill（3 個檔案）
```
blog-draft/
├── SKILL.md                          # Skill definition
└── templates/
    ├── draft-template.md             # Blog draft template
    └── outline-template.md           # Blog outline template
```

**用途**：以一致的結構起草部落格文章

**另外**：`README.md` - Skills 概覽和使用指南

**安裝路徑**：`~/.claude/skills/` 或 `.claude/skills/`

---

## 04. 子代理（9 個檔案）

具有自訂能力的專業 AI 助手。

| 檔案 | 說明 | 工具 | 使用情境 |
|------|-------------|-------|----------|
| `code-reviewer.md` | 程式碼品質分析 | read, grep, diff, lint_runner | 全面審查 |
| `test-engineer.md` | 測試覆蓋率分析 | read, write, bash, grep | 測試自動化 |
| `documentation-writer.md` | 文件建立 | read, write, grep | 文件生成 |
| `secure-reviewer.md` | 安全性審查（唯讀） | read, grep | 安全性稽核 |
| `implementation-agent.md` | 完整實作 | read, write, bash, grep, edit, glob | 功能開發 |
| `debugger.md` | 除錯專家 | read, bash, grep | 錯誤調查 |
| `data-scientist.md` | 資料分析專家 | read, write, bash | 資料工作流程 |
| `clean-code-reviewer.md` | Clean code 標準 | read, grep | 程式碼品質 |
| `README.md` | 文件 | - | 設置和使用指南 |

**安裝路徑**：`.claude/agents/`

**使用方式**：由主代理自動委派

---

## 05. MCP 協定（5 個檔案）

外部工具和 API 整合。

| 檔案 | 說明 | 整合對象 | 使用情境 |
|------|-------------|-----------------|----------|
| `github-mcp.json` | GitHub 整合 | GitHub API | PR／issue 管理 |
| `database-mcp.json` | 資料庫查詢 | PostgreSQL/MySQL | 即時資料查詢 |
| `filesystem-mcp.json` | 檔案操作 | 本地檔案系統 | 檔案管理 |
| `multi-mcp.json` | 多個伺服器 | GitHub + DB + Slack | 完整整合 |
| `README.md` | 文件 | - | 設置和使用指南 |

**安裝路徑**：`.mcp.json`（專案範圍）或 `~/.claude.json`（使用者範圍）

**使用方式**：`/mcp__github__list_prs` 等

---

## 06. Hooks（9 個檔案）

自動執行的事件驅動自動化腳本。

| 檔案 | 說明 | 事件 | 使用情境 |
|------|-------------|-------|----------|
| `format-code.sh` | 自動格式化程式碼 | PreToolUse:Write | 程式碼格式化 |
| `pre-commit.sh` | 提交前執行測試 | PreToolUse:Bash | 測試自動化 |
| `security-scan.sh` | 資安掃描 | PostToolUse:Write | 安全性確認 |
| `log-bash.sh` | 記錄 bash 指令 | PostToolUse:Bash | 指令記錄 |
| `validate-prompt.sh` | 驗證提示 | PreToolUse | 輸入驗證 |
| `notify-team.sh` | 發送通知 | Notification | 團隊通知 |
| `context-tracker.py` | 追蹤上下文視窗使用量 | PostToolUse | 上下文監控 |
| `context-tracker-tiktoken.py` | 基於 token 的上下文追蹤 | PostToolUse | 精確 token 計數 |
| `README.md` | 文件 | - | 設置和使用指南 |

**安裝路徑**：在 `~/.claude/settings.json` 中配置

**使用方式**：在設定中配置，自動執行

**Hook 類型**（4 種類型，25 個事件）：
- 工具 Hooks：PreToolUse、PostToolUse、PostToolUseFailure、PermissionRequest
- 工作階段 Hooks：SessionStart、SessionEnd、Stop、StopFailure、SubagentStart、SubagentStop
- 任務 Hooks：UserPromptSubmit、TaskCompleted、TaskCreated、TeammateIdle
- 生命週期 Hooks：ConfigChange、CwdChanged、FileChanged、PreCompact、PostCompact、WorktreeCreate、WorktreeRemove、Notification、InstructionsLoaded、Elicitation、ElicitationResult

---

## 07. 外掛（3 個完整外掛，40 個檔案）

功能的捆綁集合。

### PR Review 外掛（10 個檔案）
```
pr-review/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── review-pr.md                  # Comprehensive review
│   ├── check-security.md             # Security check
│   └── check-tests.md                # Test coverage check
├── agents/
│   ├── security-reviewer.md          # Security specialist
│   ├── test-checker.md               # Test specialist
│   └── performance-analyzer.md       # Performance specialist
├── mcp/
│   └── github-config.json            # GitHub integration
├── hooks/
│   └── pre-review.js                 # Pre-review validation
└── README.md                         # Plugin documentation
```

**功能**：安全性分析、測試覆蓋率、效能影響

**指令**：`/review-pr`, `/check-security`, `/check-tests`

**安裝**：`/plugin install pr-review`

---

### DevOps Automation 外掛（15 個檔案）
```
devops-automation/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── deploy.md                     # Deployment
│   ├── rollback.md                   # Rollback
│   ├── status.md                     # System status
│   └── incident.md                   # Incident response
├── agents/
│   ├── deployment-specialist.md      # Deployment expert
│   ├── incident-commander.md         # Incident coordinator
│   └── alert-analyzer.md             # Alert analyzer
├── mcp/
│   └── kubernetes-config.json        # Kubernetes integration
├── hooks/
│   ├── pre-deploy.js                 # Pre-deployment checks
│   └── post-deploy.js                # Post-deployment tasks
├── scripts/
│   ├── deploy.sh                     # Deployment automation
│   ├── rollback.sh                   # Rollback automation
│   └── health-check.sh               # Health checks
└── README.md                         # Plugin documentation
```

**功能**：Kubernetes 部署、回滾、監控、事故應對

**指令**：`/deploy`, `/rollback`, `/status`, `/incident`

**安裝**：`/plugin install devops-automation`

---

### Documentation 外掛（14 個檔案）
```
documentation/
├── .claude-plugin/
│   └── plugin.json                   # Plugin manifest
├── commands/
│   ├── generate-api-docs.md          # API docs generation
│   ├── generate-readme.md            # README creation
│   ├── sync-docs.md                  # Doc synchronization
│   └── validate-docs.md              # Doc validation
├── agents/
│   ├── api-documenter.md             # API doc specialist
│   ├── code-commentator.md           # Code comment specialist
│   └── example-generator.md          # Example creator
├── mcp/
│   └── github-docs-config.json       # GitHub integration
├── templates/
│   ├── api-endpoint.md               # API endpoint template
│   ├── function-docs.md              # Function doc template
│   └── adr-template.md               # ADR template
└── README.md                         # Plugin documentation
```

**功能**：API 文件、README 生成、文件同步、驗證

**指令**：`/generate-api-docs`, `/generate-readme`, `/sync-docs`, `/validate-docs`

**安裝**：`/plugin install documentation`

**另外**：`README.md` - 外掛概覽和使用指南

---

## 08. 檢查點與倒回（2 個檔案）

儲存對話狀態並探索替代方法。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | 文件 | 完整的檢查點指南 |
| `checkpoint-examples.md` | 實際範例 | 資料庫遷移、效能最佳化、UI 迭代、除錯 |
| | | |

**核心概念**：
- **Checkpoint**：對話狀態的快照
- **Rewind**：回到上一個檢查點
- **Branch Point**：探索多種方法

**使用方式**：
```
# Checkpoints are created automatically with every user prompt
# To rewind, press Esc twice or use:
/rewind
# Then choose: Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, or Never mind
```

**使用情境**：
- 嘗試不同的實作方式
- 從錯誤中恢復
- 安全實驗
- 比較解決方案
- A/B 測試

---

## 09. 進階功能（3 個檔案）

複雜工作流程的進階能力。

| 檔案 | 說明 | 功能 |
|------|-------------|----------|
| `README.md` | 完整指南 | 所有進階功能文件 |
| `config-examples.json` | 設定範例 | 10+ 個使用情境設定 |
| `planning-mode-examples.md` | 規劃範例 | REST API、資料庫遷移、重構 |
| Scheduled Tasks | 使用 `/loop` 和 cron 工具的重複性任務 | 自動化重複工作流程 |
| Chrome Integration | 透過無頭 Chromium 進行瀏覽器自動化 | 網路測試和爬蟲 |
| Remote Control（擴展） | 連接方法、安全性、比較表 | 遠端工作階段管理 |
| Keyboard Customization | 自訂鍵盤綁定、和弦支援、上下文 | 個人化快捷鍵 |
| Desktop App（擴展） | 連接器、launch.json、企業功能 | 桌面整合 |
| | | |

**涵蓋的進階功能**：

### 規劃模式
- 建立詳細的實作計劃
- 時間估算和風險評估
- 系統性任務分解

### Extended Thinking
- 複雜問題的深度推理
- 架構決策分析
- 權衡評估

### 背景任務
- 不阻塞的長時間執行操作
- 並行開發工作流程
- 任務管理與監控

### 權限模式
- **default**：對風險動作請求核准
- **acceptEdits**：自動接受檔案編輯，其他操作仍詢問
- **plan**：唯讀分析，不進行修改
- **auto**：自動核准安全動作，對風險動作提示
- **dontAsk**：接受除風險動作以外的所有動作
- **bypassPermissions**：接受所有動作（需要 `--dangerously-skip-permissions`）

### 無頭模式（`claude -p`）
- CI/CD 整合
- 自動化任務執行
- 批次處理

### 工作階段管理
- 多個工作工作階段
- 工作階段切換和儲存
- 工作階段持久化

### 互動功能
- 鍵盤快捷鍵
- 指令歷史
- Tab 補全
- 多行輸入

### 設定
- 完整的設定管理
- 環境特定設定
- 每個專案的自訂

### 排程任務
- 使用 `/loop` 指令的重複性任務
- Cron 工具：CronCreate、CronList、CronDelete
- 自動化重複工作流程

### Chrome 整合
- 透過無頭 Chromium 進行瀏覽器自動化
- 網路測試和爬蟲能力
- 頁面互動和資料擷取

### 遠端控制（擴展）
- 連接方法和協定
- 安全考量和最佳實踐
- 遠端存取選項比較表

### 鍵盤自訂
- 自訂鍵盤綁定設定
- 多鍵快捷鍵的和弦支援
- 上下文感知的鍵盤綁定啟動

### Desktop App（擴展）
- IDE 整合連接器
- launch.json 設定
- 企業功能和部署

---

## 10. CLI 使用（1 個檔案）

命令列介面使用模式和參考。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | CLI 文件 | 旗標、選項和使用模式 |

**主要 CLI 功能**：
- `claude` - 開始互動工作階段
- `claude -p "prompt"` - 無頭／非互動模式
- `claude web` - 啟動網頁工作階段
- `claude --model` - 選擇模型（Sonnet 4.6、Opus 4.6）
- `claude --permission-mode` - 設定權限模式
- `claude --remote` - 透過 WebSocket 啟用遠端控制

---

## 文件檔案（13 個檔案）

| 檔案 | 位置 | 說明 |
|------|----------|-------------|
| `README.md` | `/` | 主要範例概覽 |
| `INDEX.md` | `/` | 本完整索引 |
| `QUICK_REFERENCE.md` | `/` | 快速參考卡 |
| `README.md` | `/01-slash-commands/` | 斜線指令指南 |
| `README.md` | `/02-memory/` | Memory 指南 |
| `README.md` | `/03-skills/` | Skills 指南 |
| `README.md` | `/04-subagents/` | 子代理指南 |
| `README.md` | `/05-mcp/` | MCP 指南 |
| `README.md` | `/06-hooks/` | Hooks 指南 |
| `README.md` | `/07-plugins/` | 外掛指南 |
| `README.md` | `/08-checkpoints/` | 檢查點指南 |
| `README.md` | `/09-advanced-features/` | 進階功能指南 |
| `README.md` | `/10-cli/` | CLI 指南 |

---

## 完整檔案樹

```
claude-howto/
├── README.md                                    # Main overview
├── INDEX.md                                     # This file
├── QUICK_REFERENCE.md                           # Quick reference card
├── claude_concepts_guide.md                     # Original guide
│
├── 01-slash-commands/                           # Slash Commands
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   ├── commit.md
│   ├── setup-ci-cd.md
│   ├── push-all.md
│   ├── unit-test-expand.md
│   ├── doc-refactor.md
│   ├── pr-slash-command.png
│   └── README.md
│
├── 02-memory/                                   # Memory
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                   # Skills
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-metrics.py
│   │   │   └── compare-complexity.py
│   │   └── templates/
│   │       ├── review-checklist.md
│   │       └── finding-template.md
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   ├── templates/
│   │   │   ├── email-template.txt
│   │   │   └── social-post-template.txt
│   │   └── tone-examples.md
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   ├── refactor/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   │   ├── analyze-complexity.py
│   │   │   └── detect-smells.py
│   │   ├── references/
│   │   │   ├── code-smells.md
│   │   │   └── refactoring-catalog.md
│   │   └── templates/
│   │       └── refactoring-plan.md
│   ├── claude-md/
│   │   └── SKILL.md
│   ├── blog-draft/
│   │   ├── SKILL.md
│   │   └── templates/
│   │       ├── draft-template.md
│   │       └── outline-template.md
│   └── README.md
│
├── 04-subagents/                                # Subagents
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md
│
├── 05-mcp/                                      # MCP Protocol
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
├── 06-hooks/                                    # Hooks
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md
│
├── 07-plugins/                                  # Plugins
│   ├── pr-review/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── review-pr.md
│   │   │   ├── check-security.md
│   │   │   └── check-tests.md
│   │   ├── agents/
│   │   │   ├── security-reviewer.md
│   │   │   ├── test-checker.md
│   │   │   └── performance-analyzer.md
│   │   ├── mcp/
│   │   │   └── github-config.json
│   │   ├── hooks/
│   │   │   └── pre-review.js
│   │   └── README.md
│   ├── devops-automation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── deploy.md
│   │   │   ├── rollback.md
│   │   │   ├── status.md
│   │   │   └── incident.md
│   │   ├── agents/
│   │   │   ├── deployment-specialist.md
│   │   │   ├── incident-commander.md
│   │   │   └── alert-analyzer.md
│   │   ├── mcp/
│   │   │   └── kubernetes-config.json
│   │   ├── hooks/
│   │   │   ├── pre-deploy.js
│   │   │   └── post-deploy.js
│   │   ├── scripts/
│   │   │   ├── deploy.sh
│   │   │   ├── rollback.sh
│   │   │   └── health-check.sh
│   │   └── README.md
│   ├── documentation/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── commands/
│   │   │   ├── generate-api-docs.md
│   │   │   ├── generate-readme.md
│   │   │   ├── sync-docs.md
│   │   │   └── validate-docs.md
│   │   ├── agents/
│   │   │   ├── api-documenter.md
│   │   │   ├── code-commentator.md
│   │   │   └── example-generator.md
│   │   ├── mcp/
│   │   │   └── github-docs-config.json
│   │   ├── templates/
│   │   │   ├── api-endpoint.md
│   │   │   ├── function-docs.md
│   │   │   └── adr-template.md
│   │   └── README.md
│   └── README.md
│
├── 08-checkpoints/                              # Checkpoints
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                        # Advanced Features
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                      # CLI Usage
    └── README.md
```

---

## 依使用情境快速入門

### 程式碼品質與審查
```bash
# Install slash command
cp 01-slash-commands/optimize.md .claude/commands/

# Install subagent
cp 04-subagents/code-reviewer.md .claude/agents/

# Install skill
cp -r 03-skills/code-review ~/.claude/skills/

# Or install complete plugin
/plugin install pr-review
```

### DevOps 與部署
```bash
# Install plugin (includes everything)
/plugin install devops-automation
```

### 文件
```bash
# Install slash command
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# Install subagent
cp 04-subagents/documentation-writer.md .claude/agents/

# Install skill
cp -r 03-skills/doc-generator ~/.claude/skills/

# Or install complete plugin
/plugin install documentation
```

### 團隊標準
```bash
# Set up project memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Edit to match your team's standards
```

### 外部整合
```bash
# Set environment variables
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# Install MCP config (project scope)
cp 05-mcp/multi-mcp.json .mcp.json
```

### 自動化與驗證
```bash
# Install hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Configure hooks in settings (~/.claude/settings.json)
# See 06-hooks/README.md
```

### 安全實驗
```bash
# Checkpoints are created automatically with every user prompt
# To rewind: press Esc+Esc or use /rewind
# Then choose what to restore from the rewind menu

# See 08-checkpoints/README.md for examples
```

### 進階工作流程
```bash
# Configure advanced features
# See 09-advanced-features/config-examples.json

# Use planning mode
/plan Implement feature X

# Use permission modes
claude --permission-mode plan          # For code review (read-only)
claude --permission-mode acceptEdits   # Auto-accept edits
claude --permission-mode auto          # Auto-approve safe actions

# Run in headless mode for CI/CD
claude -p "Run tests and report results"

# Run background tasks
Run tests in background

# See 09-advanced-features/README.md for complete guide
```

---

## 功能涵蓋矩陣

| 類別 | 指令 | 代理 | MCP | Hooks | 腳本 | 模板 | 文件 | 圖片 | 總計 |
|----------|----------|--------|-----|-------|---------|-----------|------|--------|-------|
| **01 Slash Commands** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 Memory** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 Skills** | - | - | - | - | 5 | 9 | 1 | - | **28** |
| **04 Subagents** | - | 8 | - | - | - | - | 1 | - | **9** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 Hooks** | - | - | - | 8 | - | - | 1 | - | **9** |
| **07 Plugins** | 11 | 9 | 3 | 3 | 3 | 3 | 4 | - | **40** |
| **08 Checkpoints** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 Advanced** | - | - | - | - | - | - | 1 | 2 | **3** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 學習路徑

### 初學者（第 1 週）
1. 閱讀 `README.md`
2. 安裝 1-2 個斜線指令
3. 建立專案 memory 檔案
4. 嘗試基本指令

### 中階（第 2-3 週）
1. 設置 GitHub MCP
2. 安裝子代理
3. 嘗試委派任務
4. 安裝 skill

### 進階（第 4 週以後）
1. 安裝完整外掛
2. 建立自訂斜線指令
3. 建立自訂子代理
4. 建立自訂 skill
5. 建立自己的外掛

### 專家（第 5 週以後）
1. 設置 hooks 進行自動化
2. 使用檢查點進行實驗
3. 配置規劃模式
4. 有效使用權限模式
5. 設置 CI/CD 無頭模式
6. 精通工作階段管理

---

## 依關鍵字搜尋

### 效能
- `01-slash-commands/optimize.md` - 效能分析
- `04-subagents/code-reviewer.md` - 效能審查
- `03-skills/code-review/` - 效能指標
- `07-plugins/pr-review/agents/performance-analyzer.md` - 效能專家

### 安全性
- `04-subagents/secure-reviewer.md` - 安全性審查
- `03-skills/code-review/` - 安全性分析
- `07-plugins/pr-review/` - 安全性確認

### 測試
- `04-subagents/test-engineer.md` - 測試工程師
- `07-plugins/pr-review/commands/check-tests.md` - 測試覆蓋率

### 文件
- `01-slash-commands/generate-api-docs.md` - API 文件指令
- `04-subagents/documentation-writer.md` - 文件撰寫代理
- `03-skills/doc-generator/` - 文件生成 skill
- `07-plugins/documentation/` - 完整文件外掛

### 部署
- `07-plugins/devops-automation/` - 完整 DevOps 解決方案

### 自動化
- `06-hooks/` - 事件驅動自動化
- `06-hooks/pre-commit.sh` - 提交前自動化
- `06-hooks/format-code.sh` - 自動格式化
- `09-advanced-features/` - CI/CD 無頭模式

### 驗證
- `06-hooks/security-scan.sh` - 安全性驗證
- `06-hooks/validate-prompt.sh` - 提示驗證

### 實驗
- `08-checkpoints/` - 使用倒回功能進行安全實驗
- `08-checkpoints/checkpoint-examples.md` - 實際範例

### 規劃
- `09-advanced-features/planning-mode-examples.md` - 規劃模式範例
- `09-advanced-features/README.md` - Extended Thinking

### 設定
- `09-advanced-features/config-examples.json` - 設定範例

---

## 注意事項

- 所有範例均可立即使用
- 請依您的特定需求進行修改
- 範例遵循 Claude Code 最佳實踐
- 每個類別都有自己的 README，包含詳細說明
- 腳本包含適當的錯誤處理
- 模板可自訂

---

## 貢獻

想新增更多範例？請遵循以下結構：
1. 建立適當的子目錄
2. 包含帶有使用說明的 README.md
3. 遵循命名慣例
4. 徹底測試
5. 更新此索引

---

**最後更新**：2026 年 3 月
**範例總數**：100+ 個檔案
**類別**：10 個功能
**Hooks**：8 個自動化腳本
**設定範例**：10+ 個情境
**可立即使用**：所有範例
