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
- **Hooks**：11 個範例鉤子
- **就緒狀態**：所有範例皆可直接使用

---

## 01. 斜線命令 (10 個檔案)

用於常見工作流程的使用者觸發捷徑。

| 檔案 | 描述 | 使用情境 |
|------|-------------|----------|
| `optimize.md` | 程式碼最佳化分析器 | 尋找效能問題 |
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

**AGENTS.md**（v2.1.277）：當工作目錄及其上層都不存在 `CLAUDE.md`、`.claude/CLAUDE.md` 或 `CLAUDE.local.md` 時，會被當作專案指示讀取——請參閱 `02-memory/README.md#agentsmd`

---

## 03. 技能 (23 個檔案)

透過腳本與範本自動觸發的能力。

### Code Review 技能 (5 個檔案)
```text
code-review-specialist/
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
```text
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
```text
doc-generator/
├── SKILL.md                          # 技能定義
└── generate-docs.py                  # Python 文件提取器
```

**目的**：從原始碼生成全面的 API 文件

**自動觸發**：建立或更新 API 文件時

---

### Refactor 技能 (5 個檔案)
```text
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
```text
claude-md/
└── SKILL.md                          # 技能定義
```

**目的**：管理與最佳化 CLAUDE.md 檔案

---

### Blog Draft 技能 (3 個檔案)
```text
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

## 04. Subagents (10 個檔案)

具有自訂能力的專業化 AI 助手。

| 檔案 | 描述 | 工具 | 使用情境 |
|------|-------------|-------|----------|
| `code-reviewer.md` | 程式碼品質分析 | Read, Grep, Glob, Bash | 全面性審查 |
| `test-engineer.md` | 測試覆蓋率分析 | Read, Write, Bash, Grep | 測試自動化 |
| `documentation-writer.md` | 文件建立 | Read, Write, Grep | 文件生成 |
| `secure-reviewer.md` | 安全性審查 (唯讀) | Read, Grep | 安全稽核 |
| `implementation-agent.md` | 全功能實作 | Read, Write, Edit, Bash, Grep, Glob | 功能開發 |
| `debugger.md` | 除錯專家 | Read, Edit, Bash, Grep, Glob | Bug 調查 |
| `data-scientist.md` | 資料分析專家 | Bash, Read, Write | 資料工作流程 |
| `clean-code-reviewer.md` | Clean code 標準 | Read, Grep, Glob, Bash | 程式碼品質 |
| `performance-optimizer.md` | 效能瓶頸分析 | Read, Edit, Bash, Grep, Glob | 最佳化工作 |
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

## 06. Hooks (12 個檔案)

事件驅動的自動化腳本，會自動執行。

| 檔案 | 描述 | 事件 | 使用情境 |
|------|-------------|-------|----------|
| `format-code.sh` | 自動格式化程式碼 | PostToolUse (matcher: Write) | 程式碼格式化 |
| `pre-commit.sh` | 在 commit 前執行測試 | PreToolUse (matcher: Bash) | 測試自動化 |
| `pre-tool-check.sh` | 在指令執行前驗證並稽核 | PreToolUse (matcher: Bash) | 防護機制、稽核紀錄 |
| `security-scan.sh` | 安全掃描 | PostToolUse (matcher: Write) | 安全檢查 |
| `dependency-check.sh` | 掃描相依套件清單中的漏洞 | PostToolUse (matcher: Write) | 供應鏈檢查 |
| `log-bash.sh` | 記錄 bash 指令 | PostToolUse (matcher: Bash) | 指令記錄 |
| `notify-team.sh` | 發送通知 | PostToolUse (matcher: Bash) | 團隊通知 |
| `validate-prompt.sh` | 驗證提示詞 | UserPromptSubmit | 輸入驗證 |
| `session-end.sh` | 工作階段結束時擷取進度 | SessionEnd | 進度追蹤 |
| `context-tracker.py` | 追蹤 context window 使用量 | UserPromptSubmit, Stop | Context 監控 |
| `context-tracker-tiktoken.py` | 基於 token 的 context 追蹤 | UserPromptSubmit, Stop | 精確的 token 計數 |
| `README.md` | 文件 | - | 安裝與使用指南 |

**安裝路徑**：在 `~/.claude/settings.json` 中設定

**用法**：在設定檔中設定，並自動執行

**Hook 類型**（5 種）：`command`、`http`、`prompt`、`mcp_tool`、`agent`——決定 hook 如何執行。

**Hook 事件**（33 個，分為 4 類）——決定 hook 何時執行：
- Tool Hooks: PreToolUse, PostToolUse, PostToolUseFailure, PostToolBatch, PermissionRequest, PermissionDenied
- Session Hooks: SessionStart, Setup, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop
- Task Hooks: UserPromptSubmit, UserPromptExpansion, MessageDisplay, TaskCompleted, TaskCreated, TeammateIdle（TaskCompleted/TaskCreated 只有在啟用 todo 工具時才會觸發——todo 工具預設僅在 Claude 3.x 模型、Opus 4 至 4.7、Sonnet 4 至 4.6 以及 Haiku 4.5 上提供）
- Lifecycle Hooks: ConfigChange, CwdChanged, DirectoryAdded, FileChanged, PreCompact, PostCompact, PreModelSwitch, PostModelSwitch, WorktreeCreate, WorktreeRemove, Notification, InstructionsLoaded, Elicitation, ElicitationResult

---

## 07. Plugins (3 個完整外掛，39 個檔案)

功能組合包。

### PR Review Plugin (10 個檔案)
```text
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
```text
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
│   └── kubernetes-config.json        # Kubernetes 整合
├── hooks/
│   ├── pre-deploy.js                 # 部署前檢查
│   └── post-deploy.js                # 部署後任務
├── scripts/
│   ├── deploy.sh                     # 部署自動化
│   ├── rollback.sh                   # 復原自動化
│   └── health-check.sh               # 健康檢查
└── README.md                         # 外掛文件
```

**功能**: Kubernetes 部署、復原、監控、事件回應

**斜線命令**: `/deploy`、`/rollback`、`/status`、`/incident`

**安裝**: `/plugin install devops-automation`

---

### Documentation Plugin (14 個檔案)
```text
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

**功能**: API 文件、README 生成、文件同步、驗證

**斜線命令**: `/generate-api-docs`、`/generate-readme`、`/sync-docs`、`/validate-docs`

**安裝**: `/plugin install documentation`

**Plus**: `README.md` - 外掛概覽與使用指南

---

## 08. Checkpoints and Rewind (2 個檔案)

儲存對話狀態並探索替代方案。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | 文件 | 全面的檢查點指南 |
| `checkpoint-examples.md` | 真實案例 | 資料庫遷移、效能最佳化、UI 迭代、除錯 |
| | | |

**核心概念**：
- **Checkpoint**: 對話狀態的快照
- **Rewind**: 回到先前的檢查點
- **Branch Point**: 探索多種方法

**用法**：
```text
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

## 09. Advanced Features (4 個檔案)

用於複雜工作流程的高階功能。

| 檔案 | 說明 | 功能 |
|------|-------------|----------|
| `README.md` | 完整指南 | 所有高階功能文件 |
| `config-examples.json` | 設定範例 | 10 個以上特定使用情境的設定 |
| `planning-mode-examples.md` | 規劃範例 | REST API、資料庫遷移、重構 |
| `setup-auto-mode-permissions.py` | 為 auto 模式預先填入 `permissions.allow` | 冪等，支援 `--dry-run` 與選擇性啟用旗標 |
| Dynamic Workflows | 透過 `/workflows` 進行確定性的多代理協調 (v2.1.154) | 全面稽核、遷移、橫向擴展 |
| Scheduled Tasks | 使用 `/loop` 與 cron 工具進行週期性任務 | 自動化週期性工作流程 |
| Chrome Integration | 透過 headless Chromium 進行瀏覽器自動化 | 網頁測試與爬蟲 |
| Remote Control (expanded) | 連線方式、安全性、比較表、裝置卡片 | 遠端工作階段管理（已不再是研究預覽） |
| Cross-Session Messaging | `SendMessage` / `ListAgents`，包括 `notify_when_idle` (v2.1.236) | 協調同一台機器上的工作階段 |
| Keyboard Customization | 自訂按鍵綁定、和弦支援、上下文 | 個人化快捷鍵 |
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
- 無須阻塞的長時間執行操作
- 並行開發工作流程
- 任務管理與監控

### Dynamic Workflows (v2.1.154)
- 以確定性方式協調數十到數百個背景 subagent
- 扇出／管線／平行階段，達到全面涵蓋
- 使用 `/workflows` 檢視執行紀錄；`ultracode` `/effort` 可為工作階段開啟此功能
- 自 v2.1.219 起預設規模準則為 medium（目標少於 10 個代理）——可在 `/config` 的 **Dynamic workflow size** 中變更

### Permission Modes
- **manual**: 對於風險行為請求核准（於 v2.1.200 從 `default` 更名；仍接受 `default`）
- **acceptEdits**: 自動接受檔案編輯，其他行為則請求核准
- **plan**: 唯讀分析，不進行修改
- **auto**: 全部允許，但有背景安全檢查——由分類器審查指令與受保護目錄的寫入（透過 `autoMode` 設定物件設定）
- **dontAsk**: 僅限預先核准的工具——自動拒絕所有原本會提示的呼叫。Claude 只會執行符合 `permissions.allow` 的項目、唯讀 Bash 指令，以及經 `PreToolUse` hook 核准的呼叫
- **bypassPermissions**: 接受所有操作（需要 `--dangerously-skip-permissions`）

### Headless Mode (`claude -p`)
- CI/CD 整合
- 自動化任務執行
- 批次處理

### Session Management
- 多個工作階段
- 工作階段切換與儲存
- 工作階段持久化

### Interactive Features
- 鍵盤快捷鍵
- 指令歷史紀錄
- Tab 補全
- 多行輸入

### Configuration
- 全面的設定管理
- 特定環境的設定
- 每個專案的自訂

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
- 已結束研究預覽——執行 `claude remote-control` 的機器會以裝置卡片形式出現在 Claude app 的 Code 分頁中

### 跨工作階段訊息
- 在同一台機器上的工作階段之間使用 `SendMessage`
- `notify_when_idle`——當另一個工作階段下次閒置時，傳送一次需選擇啟用的通知 (v2.1.236)
- `ListAgents` 會回報工作階段自己的名稱並列出存活中的隊友 (v2.1.239)

### 鍵盤自訂
- 自訂按鍵綁定設定
- 支援多鍵組合快捷鍵的 Chord 功能
- 具備上下文感知能力的按鍵綁定啟動

### Desktop App (擴充)
- 用於 IDE 整合的連接器
- `launch.json` 設定
- 企業級功能與部署

---

## 10. CLI 使用方法 (1 個檔案)

命令列介面使用模式與參考。

| 檔案 | 說明 | 內容 |
|------|-------------|---------|
| `README.md` | CLI 文件 | 旗標 (Flags)、選項與使用模式 |

**關鍵 CLI 功能**：
- `claude` - 啟動互動式工作階段
- `claude -p "prompt"` - Headless/非互動模式
- `claude web` - 啟動網頁工作階段
- `claude --model` - 選擇模型 (Opus 5, Sonnet 5, Sonnet 4.6, Opus 4.8, Haiku 4.5)
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

```text
claude-howto/
├── README.md                                    # 主要概覽
├── INDEX.md                                     # 本檔案
├── QUICK_REFERENCE.md                           # 快速參考卡
├── claude_concepts_guide.md                     # 原始概念指南
│
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
│
├── 02-memory/                                   # 記憶
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   ├── memory-saved.png
│   ├── memory-ask-claude.png
│   └── README.md
│
├── 03-skills/                                   # 技能
│   ├── code-review-specialist/
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
│
├── 05-mcp/                                      # MCP 協定
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
│
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
│
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

---

## 快速入門（依使用情境）

### 程式碼品質與審查
```bash
# 安裝斜線命令
cp 01-slash-commands/optimize.md .claude/commands/

# 安裝子代理
cp 04-subagents/code-reviewer.md .claude/agents/

# 安裝技能
cp -r 03-skills/code-review-specialist ~/.claude/skills/

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

# 在設定檔中設定鉤子 (~/.claude/settings.json)
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
# 設定進階功能
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

# 請參閱 09-advanced-features/README.md 以取得完整指南
```

---

## 功能覆蓋矩陣

| 類別 | 斜線命令 | 代理 | MCP | 鉤子 | 腳本 | 範本 | 文件 | 圖片 | 總計 |
|----------|----------|--------|-----|-------|---------|-----------|------|--------|-------|
| **01 Slash Commands** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 Memory** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 Skills** | - | - | - | - | 5 | 7 | 11 | - | **23** |
| **04 Subagents** | - | 9 | - | - | - | - | 1 | - | **10** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 Hooks** | - | - | - | 11 | - | - | 1 | - | **12** |
| **07 Plugins** | 11 | 9 | 3 | 3 | 3 | 3 | 7 | - | **39** |
| **08 Checkpoints** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 Advanced** | - | - | - | - | 1 | 1 | 2 | - | **4** |
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
2. ✅ 建立自訂斜線命令
3. ✅ 建立自訂子代理
4. ✅ 建立自訂技能
5. ✅ 開發你自己的外掛

### 專家 (第 5 週+)
1. ✅ 設定自動化鉤子
2. ✅ 使用檢查點進行實驗
3. ✅ 設定規劃模式
4. ✅ 有效使用權限模式
5. ✅ 為 CI/CD 設定無頭模式 (headless mode)
6. ✅ 精通工作階段管理

---

## 關鍵字搜尋

### 效能
- `01-slash-commands/optimize.md` - 效能分析
- `04-subagents/code-reviewer.md` - 效能審查
- `03-skills/code-review-specialist/` - 效能指標
- `07-plugins/pr-review/agents/performance-analyzer.md` - 效能專家

### 安全性
- `04-subagents/secure-reviewer.md` - 安全審查
- `03-skills/code-review-specialist/` - 安全分析
- `07-plugins/pr-review/` - 安全檢查

### 測試
- `04-subagents/test-engineer.md` - 測試工程師
- `07-plugins/pr-review/commands/check-tests.md` - 測試覆蓋率

### 文件
- `01-slash-commands/generate-api-docs.md` - API 文件命令
- `04-subagents/documentation-writer.md` - 文件撰寫代理
- `03-skills/doc-generator/` - 文件生成技能
- `07-plugins/documentation/` - 完整文件外掛

### 部署
- `07-plugins/devops-automation/` - 完整 DevOps 解決方案

### 自動化
- `06-hooks/` - 事件驅動自動化
- `06-hooks/pre-commit.sh` - Pre-commit 自動化
- `06-hooks/format-code.sh` - 自動格式化
- `09-advanced-features/` - 用於 CI/CD 的無頭模式

### 驗證
- `06-hooks/security-scan.sh` - 安全驗證
- `06-hooks/validate-prompt.sh` - 提示詞驗證

### 實驗
- `08-checkpoints/` - 使用回溯進行安全實驗
- `08-checkpoints/checkpoint-examples.md` - 真實案例

### 規劃
- `09-advanced-features/planning-mode-examples.md` - 規劃模式範例
- `09-advanced-features/README.md` - 延伸思考

### 設定
- `09-advanced-features/config-examples.json` - 設定範例

---

## Notes

- 所有範例皆可直接使用
- 可根據您的特定需求進行修改
- 範例遵循 Claude Code 最佳實務
- 每個類別都有其專屬的 README，包含詳細說明
- 腳本包含適當的錯誤處理
- 範本可自訂

---

## Contributing

想要增加更多範例嗎？請遵循以下結構：
1. 建立適當的子目錄
2. 包含包含使用說明的 README.md
3. 遵循命名慣例
4. 進行徹底測試
5. 更新此索引

---

**最後更新日期**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
- https://code.claude.com/docs/en/workflows#set-a-size-guideline
- https://code.claude.com/docs/en/memory#agents-md
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/permission-modes
- https://github.com/anthropics/claude-code/releases/tag/v2.1.153
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/model-config
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
**範例總數**：100+ 個檔案
**分類**：10 項功能
**Hooks**：11 個自動化腳本
**設定範例**：10+ 種情境
**就緒狀態**：所有範例皆可直接使用
