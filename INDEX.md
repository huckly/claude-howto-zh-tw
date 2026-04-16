# Claude Code Examples - 完整索引

這份文件提供所有範例檔案的完整索引，依功能類型組織。

## 摘要統計

- **總檔案數**: 100+ 檔案
- **類別**: 10 個功能類別
- **外掛**: 3 個完整的外掛
- **技能**: 6 個完整的技能
- **鉤子**: 8 個範例鉤子
- **即刻可用**: 所有範例

---

## 01. 斜線命令 (10 檔案)

使用者啟動的常用工作流程捷徑。

| 檔案 | 描述 | 使用案例 |
|------|-------------|----------|
| `optimize.md` | 程式碼優化分析器 | 找出效能問題 |
| `pr.md` | 提取請求準備 | PR 工作流程自動化 |
| `generate-api-docs.md` | API 文件產生器 | 產生 API 文件 |
| `commit.md` | 提交訊息輔助工具 | 標準化提交 |
| `setup-ci-cd.md` | CI/CD 流水線設定 | DevOps 自動化 |
| `push-all.md` | 推送所有變更 | 快速推送工作流程 |
| `unit-test-expand.md` | 擴展單元測試覆蓋率 | 測試自動化 |
| `doc-refactor.md` | 文件重構 | 文件改進 |
| `pr-slash-command.png` | 螢幕截圖範例 | 視覺參考 |
| `README.md` | 文件 | 設定與使用指南 |

**安裝路徑**: `.claude/commands/`

**用法**: `/optimize`, `/pr`, `/generate-api-docs`, `/commit`, `/setup-ci-cd`, `/push-all`, `/unit-test-expand`, `/doc-refactor`

## 02. 記憶 (6 files)

持續的上下文和專案標準。

| 檔案 | 描述 | 範圍 | 位置 |
|------|-------------|-------|----------|
| `project-CLAUDE.md` | 團隊專案標準 | 專案範圍 | `./CLAUDE.md` |
| `directory-api-CLAUDE.md` | API 相關規則 | 目錄 | `./src/api/CLAUDE.md` |
| `personal-CLAUDE.md` | 個人偏好 | 用戶 | `~/.claude/CLAUDE.md` |
| `memory-saved.png` | 截圖：記憶儲存 | - | 視覺參考 |
| `memory-ask-claude.png` | 截圖：詢問 Claude | - | 視覺參考 |
| `README.md` | 文件 | - | 參考 |

**安裝**: 複製到適當位置

**使用**: 由 Claude 自動載入

---

## 03. 技能 (28 files)

自動觸發的內建功能，使用腳本和範本。

### 程式碼審查技能 (5 files)
```
code-review/
├── SKILL.md                          # 技能定義
├── scripts/
│   ├── analyze-metrics.py            # 程式碼指標分析器
│   └── compare-complexity.py         # 複雜度比較
└── templates/
    ├── review-checklist.md           # 審查清單
    └── finding-template.md           # 發現文件
```

**目的**: 進行全面的程式碼審查，包含安全性、效能和品質分析

**自動觸發**: 在審查程式碼時

---

### 品牌語氣技能 (4 files)
```
brand-voice/
├── SKILL.md                          # 技能定義
├── templates/
│   ├── email-template.txt            # 電子郵件格式
│   └── social-post-template.txt      # 社群媒體格式
└── tone-examples.md                  # 範例訊息
```

**目的**: 確保溝通內容的品牌語氣一致

**自動觸發**: 在創建行銷文案時

---

### 文件產生器技能 (2 files)
```
doc-generator/
├── SKILL.md                          # 技能定義
└── generate-docs.py                  # Python 文件提取器
```

**目的**: 從原始程式碼產生全面的 API 文件

**自動觸發**: 在創建/更新 API 文件時

---

### 重構技能 (5 files)
```
refactor/
├── SKILL.md                          # 技能定義
├── scripts/
│   ├── analyze-complexity.py         # 複雜度分析器
│   └── detect-smells.py              # 程式碼氣味偵測器
├── references/
│   ├── code-smells.md                # 程式碼氣味目錄
│   └── refactoring-catalog.md        # 重構模式
└── templates/
    └── refactoring-plan.md           # 重構計畫範本
```

**目的**: 透過複雜度分析進行系統性的程式碼重構

**自動觸發**: 在重構程式碼時

---

### Claude MD 技能 (1 file)
```
claude-md/
└── SKILL.md                          # 技能定義
```

**目的**: 管理和優化 CLAUDE.md 檔案

---

### 部落格草稿技能 (3 files)
```
blog-draft/
├── SKILL.md                          # 技能定義
└── templates/
    ├── draft-template.md             # 部落格草稿範本
    └── outline-template.md           # 部落格大綱範本
```

**目的**: 以一致的結構撰寫部落格文章

## 04. 子代理 (9 檔案)

專門的 AI 助理，具有自訂功能。

| 檔案 | 描述 | 工具 | 使用案例 |
|------|-------------|-------|----------|
| `code-reviewer.md` | 程式碼品質分析 | read, grep, diff, lint_runner | 全面審查 |
| `test-engineer.md` | 測試覆蓋率分析 | read, write, bash, grep | 測試自動化 |
| `documentation-writer.md` | 文件撰寫 | read, write, grep | 文件生成 |
| `secure-reviewer.md` | 安全性審查 (唯讀) | read, grep | 安全稽核 |
| `implementation-agent.md` | 完整實作 | read, write, bash, grep, edit, glob | 功能開發 |
| `debugger.md` | 除錯專家 | read, bash, grep | Bug 調查 |
| `data-scientist.md` | 資料分析專家 | read, write, bash | 資料流程 |
| `clean-code-reviewer.md` | 清潔程式碼標準 | read, grep | 程式碼品質 |
| `README.md` | 文件 | - | 設定與使用指南 |

**安裝路徑**: `.claude/agents/`

**使用方式**: 由主要代理自動委派

---

## 05. MCP 協定 (5 檔案)

外部工具和 API 整合。

| 檔案 | 描述 | 整合對象 | 使用案例 |
|------|-------------|-----------------|----------|
| `github-mcp.json` | GitHub 整合 | GitHub API | PR/issue 管理 |
| `database-mcp.json` | 資料庫查詢 | PostgreSQL/MySQL | 即時資料查詢 |
| `filesystem-mcp.json` | 檔案操作 | 本機檔案系統 | 檔案管理 |
| `multi-mcp.json` | 伺服器多個 | GitHub + DB + Slack | 完整整合 |
| `README.md` | 文件 | - | 設定與使用指南 |

**安裝路徑**: `.mcp.json` (專案範圍) 或 `~/.claude.json` (使用者範圍)

**使用方式**: `/mcp__github__list_prs` 等。

## 06. 鉤子 (9 files)

Event-driven automation scripts that execute automatically。

| File | Description | Event | Use Case |
|------|-------------|-------|----------|
| `format-code.sh` | Auto-format code | PreToolUse:Write | Code formatting |
| `pre-commit.sh` | Run tests before commit | PreToolUse:Bash | Test automation |
| `security-scan.sh` | Security scanning | PostToolUse:Write | Security checks |
| `log-bash.sh` | Log bash commands | PostToolUse:Bash | Command logging |
| `validate-prompt.sh` | Validate prompts | PreToolUse | Input validation |
| `notify-team.sh` | Send notifications | Notification | Team notifications |
| `context-tracker.py` | Track context window usage | PostToolUse | Context monitoring |
| `context-tracker-tiktoken.py` | Token-based context tracking | PostToolUse | Precise token counting |
| `README.md` | Documentation | - | Setup and usage guide |

**Installation Path**: Configure in `~/.claude/settings.json`

**Usage**: Configured in settings, executed automatically

**Hook Types** (4 types, 25 events):
- Tool Hooks: PreToolUse, PostToolUse, PostToolUseFailure, PermissionRequest
- Session Hooks: SessionStart, SessionEnd, Stop, StopFailure, SubagentStart, SubagentStop
- Task Hooks: UserPromptSubmit, TaskCompleted, TaskCreated, TeammateIdle
- Lifecycle Hooks: ConfigChange, CwdChanged, FileChanged, PreCompact, PostCompact, WorktreeCreate, WorktreeRemove, Notification, InstructionsLoaded, Elicitation, ElicitationResult

---

## 07. 外掛 (3 complete plugins, 40 files)

Bundled collections of features。

### PR Review Plugin (10 files)
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

**Features**: Security analysis, test coverage, performance impact

**Commands**: `/review-pr`, `/check-security`, `/check-tests`

**Installation**: `/plugin install pr-review`

---

### DevOps Automation Plugin (15 files)
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
│   ├── rollback.sh                   # 回復自動化
│   └── health-check.sh               # 健康檢查
└── README.md                         # 外掛說明文件
```

**功能**: Kubernetes 部署、回溯、監控、事件處理

**命令**: `/deploy`, `/rollback`, `/status`, `/incident`

**安裝**: `/plugin install devops-automation`

---

### 文件說明外掛 (14 個檔案)
```
documentation/
├── .claude-plugin/
│   └── plugin.json                   # 外掛資訊清單
├── commands/
│   ├── generate-api-docs.md          # API 文件產生
│   ├── generate-readme.md            # README 建立
│   ├── sync-docs.md                  # 文件同步
│   └── validate-docs.md              # 文件驗證
├── agents/
│   ├── api-documenter.md             # API 文件專家
│   ├── code-commentator.md           # 程式碼註解專家
│   └── example-generator.md          # 範例產生器
├── mcp/
│   └── github-docs-config.json       # GitHub 整合
├── templates/
│   ├── api-endpoint.md               # API 端點範本
│   ├── function-docs.md              # 函數文件範本
│   └── adr-template.md               # ADR 範本
└── README.md                         # 外掛說明文件
```

**功能**: API 文件、README 產生、文件同步、驗證

**命令**: `/generate-api-docs`, `/generate-readme`, `/sync-docs`, `/validate-docs`

**安裝**: `/plugin install documentation`

**加分項**: `README.md` - 外掛總覽和使用指南
```

## 08. 檢查點與回溯 (2 檔案)

儲存會話狀態並探索替代方案。

| 檔案 | 描述 | 內容 |
|------|-------------|---------|
| `README.md` | 文件 | 完整的檢查點指南 |
| `checkpoint-examples.md` | 實際案例 | 資料庫遷移、效能優化、UI 迭代、除錯 |
| | | |

**關鍵概念**:
- **檢查點**: 會話狀態的快照
- **回溯**: 回到先前的檢查點
- **分支點**: 探索多種方法

**使用方法**:
```
# 檢查點會隨著每個使用者提示詞自動建立
# 要回溯，請雙擊 Esc 鍵或使用：
/rewind
# 然後選擇：還原程式碼和會話、還原會話、
# 還原程式碼、從這裡摘要說明或取消
```

**使用案例**:
- 嘗試不同的實作方式
- 從錯誤中恢復
- 安全的實驗
- 比較解決方案
- A/B 測試

---

## 09. 進階功能 (3 檔案)

用於複雜工作流程的進階功能。

| 檔案 | 描述 | 功能 |
|------|-------------|----------|
| `README.md` | 完整指南 | 所有進階功能的說明 |
| `config-examples.json` | 配置範例 | 10+ 種特定使用案例的配置 |
| `planning-mode-examples.md` | 規劃範例 | REST API、資料庫遷移、重構 |
| 排程任務 | 使用 `/loop` 和 cron 工具進行重複性任務 | 自動重複性工作流程 |
| Chrome 整合 | 透過無頭 Chromium 進行瀏覽器自動化 | 網頁測試和抓取 |
| 遠端控制 (擴展) | 連線方法、安全性、比較表格 | 遠端會話管理 |
| 鍵盤自訂 | 自訂按鍵綁定、和弦支援、上下文 | 個人化捷徑 |
| 桌面應用程式 (擴展) | 連接器、launch.json、企業功能 | 桌面整合 |
| | | |

**涵蓋的進階功能**:

### 規劃模式
- 建立詳細的實作計畫
- 時間估計和風險評估
- 有系統地將任務分解

### 擴展思考
- 對於複雜問題進行深入推理
- 架構決策分析
- 權衡評估

### 背景任務
- 在不封鎖的情況下執行長時間運作
- 平行開發工作流程
- 任務管理和監控

### 權限模式
- **default**: 詢問有風險動作的批准
- **acceptEdits**: 自動接受檔案編輯，並詢問其他人
- **plan**: 唯讀分析，不進行任何修改
- **auto**: 自動批准安全動作，提示有風險的動作
- **dontAsk**: 接受所有動作，除了有風險的動作
- **bypassPermissions**: 接受所有 (需要 `--dangerously-skip-permissions`)

### 無頭模式 (`claude -p`)
- CI/CD 整合
- 自動化任務執行
- 批次處理

### 會話管理
- 複数の作業會話
- 會話切換和儲存
- 會話持久性

### 互動功能
- 鍵盤捷徑
- 命令歷史記錄
- Tab 補全
- 多行輸入

### 配置
- 全面的設定管理
- 針對特定環境的配置
- 每個專案的自訂設定

### 排程任務
- 透過 `/loop` 指令進行重複性任務
- Cron 工具：CronCreate、CronList、CronDelete
- 自動化的重複性工作流程

### Chrome 整合
- 透過無頭 Chromium 進行瀏覽器自動化
- 網頁測試和抓取資料的能力
- 頁面互動和資料提取

### 遠端控制 (擴展)
- 連線方法和協定
- 安全考量和最佳實務
- 遠端存取選項比較表格

### 鍵盤自訂
- 自訂按鍵綁定設定
- 和弦支援，用於多鍵快捷鍵
- 根據上下文啟動按鍵綁定

### 桌面應用程式 (擴展)
- IDE 整合的連接器
- launch.json 設定
- 企業功能和部署

---

## 10. CLI 使用方法 (1 檔案)

命令列介面使用模式和參考。

| 檔案 | 描述 | 內容 |
|------|-------------|---------|
| `README.md` | CLI 文件 | 標誌、選項和使用模式 |

**主要 CLI 功能**:
- `claude` - 啟動互動式會話
- `claude -p "prompt"` - 無頭/非互動模式
- `claude web` - 啟動網頁會話
- `claude --model` - 選擇模型 (Sonnet 4.6, Opus 4.6)
- `claude --permission-mode` - 設置權限模式
- `claude --remote` - 透過 WebSocket 啟用遠端控制

---

## 文件 (13 檔案)

| 檔案 | 位置 | 描述 |
|------|----------|-------------|
| `README.md` | `/` | 主要範例概觀 |
| `INDEX.md` | `/` | 完整的索引 |
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

## 完整檔案樹

```
claude-howto/
├── README.md                                    # 主要概觀
├── INDEX.md                                     # 此檔案
├── QUICK_REFERENCE.md                           # 快速參考卡
├── claude_concepts_guide.md                     # 原始指南
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
```

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
└── 10-cli/                                      # CLI 使用法
    └── README.md

## 快速入門指南 (按使用案例)

### 程式碼品質與審查

```bash
# 安裝斜線命令
cp 01-slash-commands/optimize.md .claude/commands/

# 安裝子代理
cp 04-subagents/code-reviewer.md .claude/agents/

# 安裝技能
cp -r 03-skills/code-review ~/.claude/skills/

# 或安裝完整的外掛
/plugin install pr-review
```

### DevOps 與部署

```bash
# 安裝外掛 (包含所有內容)
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

# 或安裝完整的外掛
/plugin install documentation
```

### 團隊標準

```bash
# 設定專案記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 編輯以符合團隊的標準
```

### 外部整合

```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 安裝 MCP 設定 (專案範圍)
cp 05-mcp/multi-mcp.json .mcp.json
```

### 自動化與驗證

```bash
# 安裝鉤子
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 在設定中設定鉤子 (~/.claude/settings.json)
# 參閱 06-hooks/README.md
```

### 安全實驗

```bash
# 每次使用者提示詞都會自動建立檢查點
# 要回溯：按下 Esc+Esc 或使用 /rewind
# 然後從回溯選單中選擇要還原的內容

# 參閱 08-checkpoints/README.md 以取得範例
```

### 進階工作流程

```bash
# 設定進階功能
# 參閱 09-advanced-features/config-examples.json

# 使用規劃模式
/plan 實作功能 X

# 使用權限模式
claude --permission-mode plan          # 程式碼審查 (唯讀)
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動批准安全動作

# 在無頭模式下執行以用於 CI/CD
claude -p "執行測試並報告結果"

# 執行背景任務
在背景中執行測試

# 參閱 09-advanced-features/README.md 以取得完整指南
```

## 功能涵蓋矩陣

| 類別 | 斜線命令 | 代理 | MCP | 鉤子 | 腳本 | 範本 | 文件 | 圖片 | 總計 |
|----------|----------|--------|-----|-------|---------|-----------|------|--------|-------|
| **01 斜線命令** | 8 | - | - | - | - | - | 1 | 1 | **10** |
| **02 記憶** | - | - | - | - | - | 3 | 1 | 2 | **6** |
| **03 技能** | - | - | - | - | 5 | 9 | 1 | - | **28** |
| **04 子代理** | - | 8 | - | - | - | - | 1 | - | **9** |
| **05 MCP** | - | - | 4 | - | - | - | 1 | - | **5** |
| **06 鉤子** | - | - | - | 8 | - | - | 1 | - | **9** |
| **07 外掛** | 11 | 9 | 3 | 3 | 3 | 3 | 4 | - | **40** |
| **08 檢查點** | - | - | - | - | - | - | 1 | 1 | **2** |
| **09 進階** | - | - | - | - | - | - | 1 | 2 | **3** |
| **10 CLI** | - | - | - | - | - | - | 1 | - | **1** |

---

## 學習路徑

### 初學者 (第 1 週)
1. ✅ 閱讀 `README.md`
2. ✅ 安裝 1-2 個斜線命令
3. ✅ 建立專案記憶檔案
4. ✅ 嘗試基本命令

### 中級 (第 2-3 週)
1. ✅ 設定 GitHub MCP
2. ✅ 安裝一個子代理
3. ✅ 嘗試委派任務
4. ✅ 安裝一個技能

### 進階 (第 4 週+)
1. ✅ 安裝完整的外掛
2. ✅ 建立自訂斜線命令
3. ✅ 建立自訂子代理
4. ✅ 建立自訂技能
5. ✅ 建立您自己的外掛

### 專家 (第 5 週+)
1. ✅ 設定鉤子以進行自動化
2. ✅ 使用檢查點進行實驗
3. ✅ 設定規劃模式
4. ✅ 有效使用權限模式
5. ✅ 設定無頭模式以進行 CI/CD
6. ✅ 掌握會話管理

---

## 關鍵字搜尋

### 效能
- `01-slash-commands/optimize.md` - 效能分析
- `04-subagents/code-reviewer.md` - 效能審查
- `03-skills/code-review/` - 效能指標
- `07-plugins/pr-review/agents/performance-analyzer.md` - 效能專家

### 安全性
- `04-subagents/secure-reviewer.md` - 安全性審查
- `03-skills/code-review/` - 安全性分析
- `07-plugins/pr-review/` - 安全性檢查

### 測試
- `04-subagents/test-engineer.md` - 測試工程師
- `07-plugins/pr-review/commands/check-tests.md` - 測試覆蓋率

### 文件
- `01-slash-commands/generate-api-docs.md` - API 文件命令
- `04-subagents/documentation-writer.md` - 文件撰寫者代理
- `03-skills/doc-generator/` - 文件產生器技能
- `07-plugins/documentation/` - 完整的文檔外掛

### 部署
- `07-plugins/devops-automation/` - 完整的 DevOps 解決方案

### 自動化
- `06-hooks/` - 事件驅動的自動化
- `06-hooks/pre-commit.sh` - 預提交自動化
- `06-hooks/format-code.sh` - 自動格式化
- `09-advanced-features/` - 無頭模式用於 CI/CD

### 驗證
- `06-hooks/security-scan.sh` - 安全性驗證
- `06-hooks/validate-prompt.sh` - 提示詞驗證

### 實驗
- `08-checkpoints/` - 透過回溯進行安全實驗
- `08-checkpoints/checkpoint-examples.md` - 實際案例

### 規劃
- `09-advanced-features/planning-mode-examples.md` - 規劃模式案例
- `09-advanced-features/README.md` - 擴展思考

### 設定

---

`09-advanced-features/config-examples.json` - Configuration examples

---

## Notes

- 所有範例都已準備好使用
- 根據您的特定需求進行修改
- 範例遵循 Claude Code 最佳實踐
- 每個類別都有自己的 README，其中包含詳細的說明
- 腳本包含適當的錯誤處理
- 範本是可客製化的

---

## Contributing

想新增更多範例嗎？請遵循以下結構：
1. 建立適當的子目錄
2. 包含 README.md，說明使用方法
3. 遵循命名慣例
4. 徹底測試
5. 更新此索引

---

**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/overview
**範例總數**: 100+ 檔案
**類別**: 10 項功能
**鉤子**: 8 個自動化腳本
**配置範例**: 10+ 情境
**已準備好使用**: 所有範例
