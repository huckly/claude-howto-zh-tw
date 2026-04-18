<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code Examples - 完整索引

本文件提供了按功能類型分類的所有範例檔案完整索引。

## 統計摘要

- **檔案總數**：100+ 個檔案
- **分類**：10 個功能分類
- **Plugins**：3 個完整外掛
- **Skills**：6 個完整技能
- **Hooks**：8 個範例鉤子
- **就緒狀態**：所有範例皆可直接使用

---

## 01. 斜線命令 (10 個檔案)

用於常見工作流程的使用者觸發捷徑。

| 檔案 | 描述 | 使用情境 |
|------|-------------|----------|
| `optimize.md` | 程式碼優化分析器 | 尋找效能問題 |
| `pr.md` | Pull request 準備 | PR 工作流程自動化 |
| `generate-api-docs.md` | API 文件產生器 | 產生 API 文件 |
| `commit.md` | Commit 訊息助手 | 標準化 commit |
| `setup-ci-cd.md` | CI/CD 流水線設定 | DevOps 自動化 |
| `push-all.md` | 推送所有變更 | 快速推送工作流程 |
| `unit-test-expand.md` | 擴充單元測試覆蓋率 | 測試自動化 |
| `doc-refactor.md` | 文件重構 | 文件改進 |
| `pr-slash-command.png` | 螢幕截圖範例 | 視覺參考 |
| `README.md` | 文件 | 設定與使用指南 |

**安裝路徑**：`.claude/commands/`

**用法**：`/optimize`, `/pr`, `/generate-api-docs`, `/commit`, `/setup-ci-cd`, `/push-all`, `/unit-test-expand`, `/doc-refactor`

---

## 02. 記憶 (6 個檔案)

持久化的上下文與專案標準。

| 檔案 | 描述 | 範圍 | 位置 |
|------|-------------|-------|----------|
| `project-CLAUDE.md` | 團隊專案標準 | 全域專案 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 特定規則 | 目錄 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 個人偏好 | 使用者 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | 螢幕截圖：記憶已儲存 | - | 視覺參考 |
| `memory-ask-claude.png` | 螢幕截圖：詢問 Claude | - | 視覺參考 |
| `README.md` | 文件 | - | 參考 |

**安裝**：複製到適當的位置

**用法**：由 Claude 自動載入

---

## 03. 技能 (28 個檔案)

透過腳本與範本自動觸發的能力。

### Code Review 技能 (5 個檔案)
```
code-review/
├── SKILL.md                          # 技能定義
├── scripts/
│   ├── analyze-metrics.py            # 程式碼指標分析器
│   └── compare-complexity.py         # 複雜度比較
└── templates/
    ├── review-checklist.md           # 審查檢查清單
    └── finding-template.md           # 問題紀錄範本
```

**目的**：包含安全性、效能與品質分析的全面性程式碼審查

**自動觸發**：進行程式碼審查時

---

### Brand Voice 技能 (4 個檔案)
```
brand-voice/
├── SKILL.md                          # 技能定義
├── templates/
│   ├── email-template.txt            # Email 格式
│   └── social-post-template.txt      # 社群媒體格式
└── tone-examples.md                  # 範例訊息
```

**目的**：確保溝通中品牌語調的一致性

**自動觸發**：建立行銷文案時

---

### Documentation Generator 技能 (2 個檔案)
```
doc-generator/
├── SKILL.md                          # 技能定義
└── generate-docs.py                  # Python 文件提取器
```

**目的**：從原始碼生成全面的 API 文件

**自動觸發**：建立或更新 API 文件時

---

### Refactor 技能 (5 個檔案)
```
refactor/
├── SKILL.md                          # 技能定義
├── scripts/
│   ├── analyze-complexity.py         # 複雜度分析器
│   └── detect-smells.py              # 程式碼壞味道偵測器
├── references/
│   ├── code-smells.md                # 程式碼壞味道目錄
│   └── refactoring-catalog.md        # 重構模式目錄
└── templates/
    └── refactoring-plan.md           # 重構計畫範本
```

**目的**：結合複雜度分析的系統性程式碼重構

**自動觸發**：進行程式碼重構時

---

### Claude MD 技能 (1 個檔案)
```
claude-md/
└── SKILL.md                          # 技能定義
```

**目的**：管理與優化 CLAUDE.md 檔案

---

### Blog Draft 技能 (3 個檔案)
```
blog-draft/
├── SKILL.md                          # 技能定義
└── templates/
    ├── draft-template.md             # 部落格草稿範本
    └── outline-template.md           # 部落格大綱範本
```

**目的**：撰寫具有一致結構的部落格文章草稿

**Plus**: `README.md` - 技能概覽與使用指南

**安裝路徑**: `~/.claude/skills/` 或 `.claude/skills/`

---

## 04. Subagents (9 個檔案)

具有自定義能力的專業化 AI 助手。

| 檔案 | 描述 | 工具 | 使用情境 |
|------|-------------|-------|----------|
| `code-reviewer.md` | 程式碼品質分析 | read, grep, diff, lint_runner | 全面性審查 |
| `test-engineer.md` | 測試覆蓋率分析 | read, write, bash, grep | 測試自動化 |
| `documentation-writer.md` | 文件建立 | read, write, grep | 文件生成 |
| `secure-reviewer.md` | 安全性審查 (唯讀) | read, grep | 安全稽核 |
| `implementation-agent.md` | 全功能實作 | read, write, bash, grep, edit, glob | 功能開發 |
| `debugger.md` | 除錯專家 | read, bash, grep | Bug 調查 |
| `data-scientist.md` | 資料分析專家 | read, write, bash | 資料工作流程 |
| `clean-code-reviewer.md` | Clean code 標準 | read, grep | 程式碼品質 |
| `README.md` | 文件 | - | 設定與使用指南 |

**安裝路徑**: `.claude/agents/`

**用法**: 由主代理自動委派

---

## 05. MCP Protocol (5 個檔案)

外部工具與 API 整合。

| 檔案 | 描述 | 整合對象 | 使用情境 |
|------|-------------|-----------------|----------|
| `github-mcp.json` | GitHub 整合 | GitHub API | PR/issue 管理 |
| `database-mcp.json` | 資料庫查詢 | PostgreSQL/MySQL | 即時資料查詢 |
| `filesystem-mcp.json` | 檔案操作 | 本地檔案系統 | 檔案管理 |
| `multi-mcp.json` | 多重伺服器 | GitHub + DB + Slack | 完全整合 |
| `README.md` | 文件 | - | 設定與使用指南 |

**安裝路徑**: `.mcp.json` (專案範圍) 或 `~/.claude.json` (使用者範圍)

**用法**: `/mcp__github__list_prs` 等

---

## 06. Hooks (9 個檔案)

事件驅動的自動化腳本，會自動執行。

| 檔案 | 描述 | 事件 | 使用情境 |
|------|-------------|-------|----------|
| `format-code.sh` | 自動格式化程式碼 | PreToolUse:Write | 程式碼格式化 |
| `pre-commit.sh` | 在 commit 前執行測試 | PreToolUse:Bash | 測試自動化 |
| `security-scan.sh` | 安全掃描 | PostToolUse:Write | 安全檢查 |
| `log-bash.sh` | 記錄 bash 指令 | PostToolUse:Bash | 指令記錄 |
| `validate-prompt.sh` | 驗證提示詞 | PreToolUse | 輸入驗證 |
| `notify-team.sh` | 發送通知 | Notification | 團隊通知 |
| `context-tracker.py` | 追蹤 context window 使用量 | PostToolUse | Context 監控 |
| `context-tracker-tiktoken.py` | 基於 token 的 context 追蹤 | PostToolUse | 精確的 token 計數 |
| `README.md` | 文件 | - | 安裝與使用指南 |

**安裝路徑**：在 `~/.claude/settings.json` 中進行配置

**用法**：在設定中配置，並自動執行

**Hook 類型**（4 種類型，25 個事件）：
- Tool Hooks: PreToolUse, PostToolUse, PostToolUseFailure, PermissionRequest
- Session Hooks: SessionStart, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop
- Task Hooks: UserPromptSubmit, TaskCompleted, TaskCreated, TeammateIdle
- Lifecycle Hooks: ConfigChange, CwdChanged, FileChanged, PreCompact, PostCompact, WorktreeCreate, WorktreeRemove, Notification, InstructionsLoaded, Elicitation, ElicitationResult

---

## 07. Plugins (3 個完整外掛，40 個檔案)

功能組合包。

### PR Review Plugin (10 個檔案)
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

**斜線命令**：`/review-pr`, `/check-security`, `/check-tests`

**安裝**：`/plugin install pr-review`

---

### DevOps Automation Plugin (15 個檔案)
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
```

```
├── mcp/
│   └── kubernetes-config.json        # Kubernetes 整合
├── hooks/
│   ├── pre-deploy.js                 # 部署前檢查
│   └── post-deploy.js                # 部署後任務
├── scripts/
│   ├── deploy.sh                     # 部署自動化
│   ├── rollback.sh                   # 回滾自動化
│   └── health-check.sh               # 健康檢查
└── README.md                         # 外掛文件
```

**Features**: Kubernetes 部署、回滾、監控、事件響應

**Commands**: `/deploy`、`/rollback`、`/status`、`/incident`

**Installation**: `/plugin install devops-automation`

---

### Documentation Plugin (14 files)
```
documentation/
├── .claude-plugin/
│   └── plugin.json                   # 外掛清單
├── commands/
│   ├── generate-api-docs.md          # API 文件生成
│   ├── generate-readme.md            # README 建立
│   ├── sync-docs.md                  # 文件同步
│   └── validate-docs.md              # 文件驗證
├── agents/
│   ├── api-documenter.md             # API 文件專家
│   ├── code-commentator.md           # 程式碼註解專家
│   └── example-generator.md          # 範例建立者
├── mcp/
│   └── github-docs-config.json       # GitHub 整合
├── templates/
│   ├── api-endpoint.md               # API 端點範本
│   ├── function-docs.md              # 函式文件範本
│   └── adr-template.md               # ADR 範本
└── README.md                         # 外掛文件
```

**Features**: API 文件、README 生成、文件同步、驗證

**Commands**: `/generate-api-docs`、`/generate-readme`、`/sync-docs`、`/validate-docs`

**Installation**: `/plugin install documentation`

**Plus**: `README.md` - 外掛概覽與使用指南

---

## 08. Checkpoints and Rewind (2 個檔案)

儲存對話狀態並探索替代方案。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | 文件 | 全面的檢查點指南 |
| `checkpoint-examples.md` | 真實案例 | 資料庫遷移、效能優化、UI 迭代、除錯 |
| | | |

**核心概念**：
- **Checkpoint**: 對話狀態的快照
- **Rewind**: 回到先前的檢查點
- **Branch Point**: 探索多種方法

**用法**：
```
# Checkpoints 會隨著每個使用者提示詞自動建立
# 若要 rewind，請按兩次 Esc 或使用：
/rewind
# 然後選擇：Restore code and conversation, Restore conversation,
# Restore code, Summarize from here, 或 Never mind
```

**使用情境**：
- 嘗試不同的實作方式
- 從錯誤中恢復
- 安全的實驗
- 比較解決方案
- A/B 測試

---

## 09. Advanced Features (3 個檔案)

用於複雜工作流程的高階功能。

| 檔案 | 說明 | 功能 |
|------|-------------|----------|
| `README.md` | 完整指南 | 所有高階功能文件 |
| `config-examples.json` | 設定範例 | 10 個以上特定使用情境的設定 |
| `planning-mode-examples.md` | 規劃範例 | REST API、資料庫遷移、重構 |
| Scheduled Tasks | 使用 `/loop` 與 cron 工具進行週期性任務 | 自動化週期性工作流程 |
| Chrome Integration | 透過 headless Chromium 進行瀏覽器自動化 | 網頁測試與爬蟲 |
| Remote Control (expanded) | 連線方式、安全性、比較表 | 遠端會話管理 |
| Keyboard Customization | 自定義按鍵綁定、和弦支援、上下文 | 個人化快捷鍵 |
| Desktop App (expanded) | 連接器、launch.json、企業級功能 | 桌面整合 |
| | | |

**涵蓋的高階功能**：

### Planning Mode
- 建立詳細的實作計畫
- 時間估算與風險評估
- 系統化的任務拆解

### Extended Thinking
- 針對複雜問題的深度推理
- 架構決策分析
- 權衡評估

### Background Tasks
- 無須阻塞的長時間運行操作
- 並行開發工作流程
- 任務管理與監控

### Permission Modes
- **default**: 對於風險行為請求核准
- **acceptEdits**: 自動接受檔案編輯，其他行為則請求核准
- **plan**: 唯讀分析，不進行修改
- **auto**: 自動核准安全行為，對風險行為發出提示
- **dontAsk**: 除了風險行為外，接受所有操作
- **bypassPermissions**: 接受所有操作（需要 `--dangerously-skip-permissions`）

### Headless Mode (`claude -p`)
- CI/CD 整合
- 自動化任務執行
- 批次處理

### Session Management
- 多個工作會話
- 會話切換與儲存
- 會話持久化

### Interactive Features
- 鍵盤快捷鍵
- 指令歷史紀錄
- Tab 補全
- 多行輸入

### Configuration
- 全面的設定管理
- 特定環境的設定
- 每個專案的自定義

### 排程任務
- 使用 `/loop` 命令的循環任務
- Cron 工具：CronCreate、CronList、CronDelete
- 自動化循環工作流程

### Chrome 整合
- 透過 headless Chromium 進行瀏覽器自動化
- 網頁測試與爬蟲功能
- 頁面互動與資料擷取

### 遠端控制 (擴充)
- 連線方式與協定
- 安全性考量與最佳實踐
- 遠端存取選項的比較表

### 鍵盤自定義
- 自定義按鍵綁定配置
- 支援多鍵組合快捷鍵的 Chord 功能
- 具備上下文感知能力的按鍵綁定啟動

### Desktop App (擴充)
- 用於 IDE 整合的連接器
- `launch.json` 配置
- 企業級功能與部署

---

## 10. CLI 使用方法 (1 個檔案)

命令列介面使用模式與參考。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | CLI 文件 | 旗標 (Flags)、選項與使用模式 |

**關鍵 CLI 功能**：
- `claude` - 啟動互動式會話
- `claude -p "prompt"` - Headless/非互動模式
- `claude web` - 啟動網頁會話
- `claude --model` - 選擇模型 (Sonnet 4.6, Opus 4.7, Haiku 4.5)
- `claude --permission-mode` - 設定權限模式
- `claude --remote` - 透過 WebSocket 啟用遠端控制

---

## 文件檔案 (13 個檔案)

| 檔案 | 位置 | 說明 |
|------|----------|-------------|
| `README.md` | `/` | 主要範例概覽 |
| `INDEX.md` | `/` | 此完整索引 |
| `QUICK_REFERENCE.md` | `/` | 快速參考卡 |
| `README.md` | `/01-slash-commands/` | 斜線命令指南 |
| `README.md` | `/02-memory/` | 記憶指南 |
| `README.md` | `/03-skills/` | 技能指南 |
| `README.md` | `/04-subagents/` | 子代理指南 |
| `README.md` | `/05-mcp/` | MCP 指南 |
| `README.md` | `/06-hooks/` | 鉤子指南 |
| `README.md` | `/07-plugins/` | 外掛指南 |
| `README.md` | `/08-checkpoints/` | 檢查點指南 |
| `README.md` | `/09-advanced-features/` | 進階功能指南 |
| `README.md` | `/10-cli/` | CLI 指南 |

---

## 完整檔案樹

```
claude-howto/
├── README.md                                    # 主要概覽
├── INDEX.md                                     # 本檔案
├── QUICK_REFERENCE.md                           # 快速參考卡

├── 01-slash-commands/                           # 斜線命令
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

├── 02-memory/                                   # 記憶
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md

├── 03-skills/                                   # 技能
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

├── 04-subagents/                                # 子代理
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   ├── debugger.md
│   ├── data-scientist.md
│   ├── clean-code-reviewer.md
│   └── README.md

├── 05-mcp/                                      # MCP 協定
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md

├── 06-hooks/                                    # 鉤子
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   ├── context-tracker.py
│   ├── context-tracker-tiktoken.py
│   └── README.md

├── 07-plugins/                                  # 外掛
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
```

```text
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
├── 08-checkpoints/                              # 檢查點
│   ├── checkpoint-examples.md
│   └── README.md
│
├── 09-advanced-features/                        # 進階功能
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
│
└── 10-cli/                                      # CLI 使用
    └── README.md
```

## 快速入門（依使用情境）

### 程式碼品質與審查
```bash
# 安裝斜線命令
cp 01-slash-commands/optimize.md .claude/commands/

# 安裝子代理
cp 04-subagents/code-reviewer.md .claude/agents/

# 安裝技能
cp -r 03-skills/code-review ~/.claude/skills/

# 或者安裝完整的插件
/plugin install pr-review
```

### DevOps 與部署
```bash
# 安裝插件（包含所有功能）
/plugin install devops-automation
```

### 文件撰寫
```bash
# 安裝斜線命令
cp 01-slash-commands/generate-api-docs.md .claude/commands/

# 安裝子代理
cp 04-subagents/documentation-writer.md .claude/agents/

# 安裝技能
cp -r 03-skills/doc-generator ~/.claude/skills/

# 或者安裝完整的插件
/plugin install documentation
```

### 團隊標準
```bash
# 設定專案記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 編輯以符合您團隊的標準
```

### 外部整合
```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安裝 MCP 設定（專案範圍）
cp 05-mcp/multi-mcp.json .mcp.json
```

### 自動化與驗證
```bash
# 安裝鉤子
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在設定中配置鉤子 (~/.claude/settings.json)
# 請參閱 06-hooks/README.md
```

### 安全實驗
```bash
# 檢查點會在每次使用者輸入提示詞時自動建立
# 若要回溯：按下 Esc+Esc 或使用 /rewind
# 然後從回溯選單中選擇要還原的內容

# 請參閱 08-checkpoints/README.md 以查看範例
```

### 進階工作流程
```bash
# 配置進階功能
# 請參閱 09-advanced-features/config-examples.json

# 使用規劃模式
/plan Implement feature X

# 使用權限模式
claude --permission-mode plan          # 用於程式碼審查（唯讀）
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動核准安全操作

# 在 CI/CD 中以無介面模式執行
claude -p "Run tests and report results"

# 執行背景任務
Run tests in background

# 請參閱 09-advanced-features/README.md 以獲取完整指南
```

---

## 功能覆蓋矩陣

| 類別 | 斜線命令 | 代理 | MCP | 鉤子 | 腳本 | 範本 | 文件 | 圖片 | 總計 |
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

### 初學者 (第 1 週)
1. ✅ 閱讀 `README.md`
2. ✅ 安裝 1-2 個斜線命令
3. ✅ 建立專案記憶檔案
4. ✅ 嘗試基礎命令

### 進階者 (第 2-3 週)
1. ✅ 設定 GitHub MCP
2. ✅ 安裝一個子代理
3. ✅ 嘗試委派任務
4. ✅ 安裝一個技能

### 高階者 (第 4 週+)
1. ✅ 安裝完整外掛
2. ✅ 建立自定義斜線命令
3. ✅ 建立自定義子代理
4. ✅ 建立自定義技能
5. ✅ 開發你自己的外掛

### 專家 (第 5 週+)
1. ✅ 設定自動化鉤子
2. ✅ 使用檢查點進行實驗
3. ✅ 配置規劃模式
4. ✅ 有效使用權限模式
5. ✅ 為 CI/CD 設定無頭模式 (headless mode)
6. ✅ 精通會話管理

---

| 關鍵字搜尋 | |
|---|---|
| **效能** | - `01-slash-commands/optimize.md` - 效能分析<br>- `04-subagents/code-reviewer.md` - 效能審查<br>- `03-skills/code-review/` - 效能指標<br>- `07-plugins/pr-review/agents/performance-analyzer.md` - 效能專家 |
| **安全性** | - `04-subagents/secure-reviewer.md` - 安全審查<br>- `03-skills/code-review/` - 安全分析<br>- `07-plugins/pr-review/` - 安全檢查 |
| **測試** | - `04-subagents/test-engineer.md` - 測試工程師<br>- `07-plugins/pr-review/commands/check-tests.md` - 測試覆蓋率 |
| **文件** | - `01-slash-commands/generate-api-docs.md` - API 文件命令<br>- `04-subagents/documentation-writer.md` - 文件撰寫代理<br>- `03-skills/doc-generator/` - 文件生成技能<br>- `07-plugins/documentation/` - 完整文件外掛 |
| **部署** | - `07-plugins/devops-automation/` - 完整 DevOps 解決方案 |
| **自動化** | - `06-hooks/` - 事件驅動自動化<br>- `06-hooks/pre-commit.sh` - Pre-commit 自動化<br>- `06-hooks/format-code.sh` - 自動格式化<br>- `09-advanced-features/` - 用於 CI/CD 的無頭模式 |
| **驗證** | - `06-hooks/security-scan.sh` - 安全驗證<br>- `06-hooks/validate-prompt.sh` - 提示詞驗證 |
| **實驗** | - `08-checkpoints/` - 使用回溯進行安全實驗<br>- `08-checkpoints/checkpoint-examples.md` - 真實案例 |
| **規劃** | - `09-advanced-features/planning-mode-examples.md` - 規劃模式範例<br>- `09-advanced-features/README.md` - 延伸思考 |
| **配置** | |

- `09-advanced-features/config-examples.json` - 設定範例

---

## Notes

- 所有範例皆可直接使用
- 可根據您的特定需求進行修改
- 範例遵循 Claude Code 最佳實務
- 每個類別都有其專屬的 README，包含詳細說明
- 腳本包含適當的錯誤處理
- 範本可自定義

---

## Contributing

想要增加更多範例嗎？請遵循以下結構：
1. 建立適當的子目錄
2. 包含包含使用說明的 README.md
3. 遵循命名慣例
4. 進行徹底測試
5. 更新此索引

---

**Last Updated**: April 16, 2026
**Claude Code Version**: 2.1.112
**Sources**:
- https://docs.anthropic.com/en/docs/claude-code
- https://www.anthropic.com/news/claude-opus-4-7
- https://support.claude.com/en/articles/12138966-release-notes
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
**Total Examples**: 100+ files
**Categories**: 10 features
**Hooks**: 8 automation scripts
**Configuration Examples**: 10+ scenarios
**Ready to Use**: All examples
