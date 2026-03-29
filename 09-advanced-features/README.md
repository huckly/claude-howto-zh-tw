<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 進階功能

Claude Code 進階功能的全面指南，涵蓋規劃模式、延伸思考、auto 模式、背景任務、權限模式、print 模式（非互動式）、會話管理、互動功能、channels、語音輸入、遠端控制、web 會話、桌面應用程式、任務列表、提示建議、git worktrees、沙盒隔離、受管設定，以及配置。

## 目錄

1. [概覽](#概覽)
2. [規劃模式](#規劃模式)
3. [延伸思考](#延伸思考)
4. [Auto 模式](#auto-模式)
5. [背景任務](#背景任務)
6. [排程任務](#排程任務)
7. [權限模式](#權限模式)
8. [Headless 模式](#headless-模式)
9. [會話管理](#會話管理)
10. [互動功能](#互動功能)
11. [語音輸入](#語音輸入)
12. [Channels](#channels)
13. [Chrome 整合](#chrome-整合)
14. [遠端控制](#遠端控制)
15. [Web 會話](#web-會話)
16. [桌面應用程式](#桌面應用程式)
17. [任務列表](#任務列表)
18. [提示建議](#提示建議)
19. [Git Worktrees](#git-worktrees)
20. [沙盒隔離](#沙盒隔離)
21. [受管設定（企業版）](#受管設定企業版)
22. [配置與設定](#配置與設定)
23. [最佳實踐](#最佳實踐)
24. [相關概念](#相關概念)

---

## 概覽

Claude Code 的進階功能透過規劃、推理、自動化和控制機制擴展核心能力。這些功能支援複雜的工作流程，適用於複雜的開發任務、程式碼審查、自動化和多會話管理。

**主要進階功能包括：**
- **規劃模式**：在撰寫程式碼前建立詳細的實作計畫
- **延伸思考**：針對複雜問題的深度推理
- **Auto 模式**：背景安全分類器在執行前審查每個動作（研究預覽版）
- **背景任務**：執行長時間操作而不阻塞對話
- **權限模式**：控制 Claude 可以執行的操作（`default`、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`）
- **Print 模式**：以非互動方式執行 Claude Code 用於自動化和 CI/CD（`claude -p`）
- **會話管理**：管理多個工作會話
- **互動功能**：鍵盤快捷鍵、多行輸入和指令歷史
- **語音輸入**：支援 20 種語言 STT 的按住即錄語音輸入
- **Channels**：MCP servers 將訊息推送到執行中的會話（研究預覽版）
- **遠端控制**：從 Claude.ai 或 Claude 應用程式控制 Claude Code
- **Web 會話**：在 claude.ai/code 的瀏覽器中執行 Claude Code
- **桌面應用程式**：用於視覺化差異審查和多會話的獨立應用程式
- **任務列表**：跨上下文壓縮的持久化任務追蹤
- **提示建議**：基於上下文的智慧指令建議
- **Git Worktrees**：用於平行工作的獨立 worktree 分支
- **沙盒隔離**：OS 層級的檔案系統和網路隔離
- **受管設定**：透過 plist、Registry 或受管檔案進行企業部署
- **配置**：使用 JSON 配置檔案自訂行為

---

## 規劃模式

規劃模式讓 Claude 在實作複雜任務前先進行思考，建立詳細計畫供你審查和批准。

### 什麼是規劃模式？

規劃模式是兩階段方法：
1. **規劃階段**：Claude 分析任務並建立詳細的實作計畫
2. **實作階段**：批准後，Claude 執行計畫

### 何時使用規劃模式

✅ 在以下情況使用規劃模式：
- 複雜的多檔案重構
- 新功能實作
- 架構變更
- 資料庫遷移
- 重大 API 重設計

❌ 不要在以下情況使用規劃模式：
- 簡單的錯誤修正
- 格式調整
- 單一檔案編輯
- 快速查詢

### 啟用規劃模式

**Slash command**：
```bash
/plan Implement user authentication system
```

**CLI 旗標**：
```bash
claude --permission-mode plan
```

**設為預設值**：
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**鍵盤快捷鍵**：
- `Shift + Tab` - 循環切換權限模式（包括 plan）
- `Alt + M` - 循環切換權限模式（Windows/Linux）

### 規劃模式的優點

- **結構化實作方法**：實作步驟的詳細分解
- **審查與批准**：在執行前批准或調整計畫
- **風險識別**：在實作前識別潛在問題
- **清晰的階段**：有組織的實作階段和里程碑

### 範例：功能實作

```
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.

## Implementation Plan

### Phase 1: Infrastructure Setup
1. Install WebSocket libraries (socket.io)
2. Set up WebSocket server
3. Configure connection management

### Phase 2: Backend Implementation
4. Create notification service
5. Implement notification queue
6. Add database schema for notifications
7. Create API endpoints

### Phase 3: Frontend Integration
8. Set up WebSocket client
9. Create notification UI components
10. Implement notification state management
11. Add notification permissions

### Phase 4: Testing & Deployment
12. Write unit tests
13. Write integration tests
14. Update documentation
15. Deploy to staging

Estimated time: 3-4 hours
Files to modify: 12
New files to create: 8

Ready to proceed? (yes/no/modify plan)

User: yes

Claude: [Begins implementation following the plan]
```

### 計畫修改

```
User: Modify the plan - skip the queue for now, we'll add it later

Claude: Updated plan:
[Shows modified plan with queue removed]

User: Looks good, proceed

Claude: [Implements modified plan]
```

### 規劃模式配置

規劃模式透過 CLI 旗標或 slash command 啟用：

```bash
# 透過 CLI 啟用 plan 模式
claude --permission-mode plan

# 或在 REPL 內使用 /plan slash command
/plan Implement user authentication system
```

**規劃用模型別名**：使用 `opusplan` 作為模型別名，讓 Opus 負責規劃、Sonnet 負責執行：

```bash
claude --model opusplan "design and implement the new API"
```

**外部編輯計畫**：按 `Ctrl+G` 在外部編輯器中開啟目前計畫以進行詳細修改。

---

## 延伸思考

延伸思考讓 Claude 在提供解決方案前，花更多時間推理複雜問題。

### 什麼是延伸思考？

延伸思考是一個深思熟慮、逐步推理的過程，Claude 會：
- 分解複雜問題
- 考慮多種方法
- 評估權衡取捨
- 推理邊界情況

### 啟用延伸思考

**鍵盤快捷鍵**：
- `Option + T`（macOS）/ `Alt + T`（Windows/Linux）- 切換延伸思考

**自動啟用**：
- 所有模型預設啟用（Opus 4.6、Sonnet 4.6、Haiku 4.5）
- Opus 4.6：自適應推理，投入程度：`low`（○）、`medium`（◐）、`high`（●）、`max`（僅限 Opus 4.6）
- 其他模型：固定預算，最多 31,999 tokens

**配置方法**：
- 切換：`Alt+T` / `Option+T`，或透過 `/config`
- 查看推理過程：`Ctrl+O`（詳細模式）
- 設定投入程度：`/effort` 指令或 `--effort` 旗標

**自訂預算**：
```bash
export MAX_THINKING_TOKENS=1024
```

**投入程度**（僅限 Opus 4.6）：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low（○）、medium（◐）、high（●）或 max（僅限 Opus 4.6）
```

**CLI 旗標**：
```bash
claude --effort high "complex architectural review"
```

**Slash command**：
```
/effort high
```

> **注意：** 在提示中使用關鍵字「ultrathink」可啟動深度推理模式。投入程度 `low`、`medium`、`high` 和 `max`（僅限 Opus 4.6）控制 Claude 執行多少推理。

### 延伸思考的優點

- **深度推理**：花更多時間思考複雜問題
- **更好的架構決策**：透過全面評估做出明智選擇
- **考慮邊界情況**：思考潛在問題和情境
- **徹底分析**：複雜問題解決的結構化方法

### 範例：架構決策

```
User: /think Should we use microservices or a monolith for our e-commerce platform?

Claude: Let me think through this architectural decision carefully...

<extended_thinking>
## Context Analysis
- Team size: 5 developers
- Expected traffic: 10k daily users initially
- Need to scale to 100k+ users
- Limited DevOps resources
- 6-month timeline to MVP

## Approach 1: Microservices
Pros:
+ Independent scaling
+ Technology flexibility
+ Team independence
+ Fault isolation

Cons:
- Operational complexity
- Network overhead
- Distributed tracing needed
- Requires DevOps expertise
- Slower initial development

## Approach 2: Monolith
Pros:
+ Simpler deployment
+ Faster initial development
+ Easier debugging
+ Lower operational overhead
+ Single codebase

Cons:
- Scaling challenges later
- All-or-nothing deployments
- Technology lock-in
- Potential for tight coupling

## Decision Factors
1. Team size (5 devs) → Favors monolith
2. DevOps resources (limited) → Favors monolith
3. Timeline (6 months) → Favors monolith
4. Future scale (100k users) → Achievable with monolith + caching
5. Complexity budget → Limited

## Recommendation
Start with a **modular monolith**:
- Single deployment unit
- Clear module boundaries
- Clean interfaces between modules
- Prepare for future extraction to microservices if needed

This gives you:
✓ Fast initial development
✓ Low operational complexity
✓ Option to extract services later
✓ Clear architecture

Migration path: When you reach 50k+ users or specific modules need independent scaling, extract those modules into microservices.
</extended_thinking>

**Recommendation**: Implement a modular monolith with clean module boundaries.

This approach balances your current constraints (team size, timeline, DevOps resources) with future scalability needs. You can migrate specific modules to microservices later when there's a clear business need.
```

### 延伸思考配置

延伸思考透過環境變數、鍵盤快捷鍵和 CLI 旗標控制：

```bash
# 設定思考 token 預算
export MAX_THINKING_TOKENS=16000

# 設定投入程度（僅限 Opus 4.6）：low（○）、medium（◐）、high（●）或 max（僅限 Opus 4.6）
export CLAUDE_CODE_EFFORT_LEVEL=high
```

在會話期間用 `Alt+T` / `Option+T` 切換，用 `/effort` 設定投入程度，或透過 `/config` 配置。

---

## Auto 模式

Auto 模式是研究預覽版的權限模式（2026 年 3 月），使用背景安全分類器在執行前審查每個動作。它允許 Claude 自主工作，同時封鎖危險操作。

### 需求

- **方案**：Team 方案（Enterprise 和 API 陸續推出）
- **模型**：Claude Sonnet 4.6 或 Opus 4.6
- **分類器**：在 Claude Sonnet 4.6 上執行（增加額外的 token 成本）

### 啟用 Auto 模式

```bash
# 以 CLI 旗標解鎖 auto 模式
claude --enable-auto-mode

# 然後在 REPL 中用 Shift+Tab 循環切換至它
```

或設為預設權限模式：

```bash
claude --permission-mode auto
```

透過配置設定：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 分類器運作方式

背景分類器按以下決策順序評估每個動作：

1. **允許/拒絕規則** - 首先檢查明確的權限規則
2. **唯讀/編輯自動批准** - 檔案讀取和編輯自動通過
3. **分類器** - 背景分類器審查動作
4. **備用** - 連續 3 次或共 20 次封鎖後，退回到提示使用者

### 預設封鎖的動作

Auto 模式預設封鎖以下動作：

| 封鎖動作 | 範例 |
|----------------|---------|
| 管道安裝 | `curl \| bash` |
| 外傳敏感資料 | 透過網路傳送 API 金鑰、憑證 |
| 生產環境部署 | 指向生產環境的部署指令 |
| 大量刪除 | 在大型目錄上執行 `rm -rf` |
| IAM 變更 | 權限和角色修改 |
| 強制推送到 main | `git push --force origin main` |

### 預設允許的動作

| 允許動作 | 範例 |
|----------------|---------|
| 本地檔案操作 | 讀取、寫入、編輯專案檔案 |
| 已宣告的依賴安裝 | 從清單執行 `npm install`、`pip install` |
| 唯讀 HTTP | `curl` 取得文件 |
| 推送到目前分支 | `git push origin feature-branch` |

### 配置 Auto 模式

**以 JSON 格式列印預設規則**：
```bash
claude auto-mode defaults
```

**透過 `autoMode.environment` 受管設定配置可信任的基礎設施**，用於企業部署。這讓管理員可以定義可信任的 CI/CD 環境、部署目標和基礎設施模式。

### 備用行為

當分類器不確定時，auto 模式會退回到提示使用者：
- 連續 **3 次**分類器封鎖後
- 會話中共 **20 次**分類器封鎖後

這確保當分類器無法自信地批准動作時，使用者始終保持控制。

---

## 背景任務

背景任務允許長時間執行的操作在不阻塞對話的情況下執行。

### 什麼是背景任務？

背景任務在你繼續工作的同時非同步執行：
- 長時間的測試套件
- 建置流程
- 資料庫遷移
- 部署腳本
- 分析工具

**基本用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 查看進度
/task show bg-1234   # 查看輸出
/task cancel bg-1234 # 取消任務
```

### 啟動背景任務

```
User: Run the full test suite in the background

Claude: Starting tests in background (task-id: bg-1234)
You can continue working while tests run.

[You can continue the conversation]

User: Meanwhile, let's refactor the auth module

Claude: [Works on auth module while tests run in background]

[Later, when tests complete]

Claude: 📢 Background task bg-1234 completed:
✅ 245 tests passed
❌ 3 tests failed
View results: /task show bg-1234
```

### 管理背景任務

**列出執行中的任務**：
```
User: /task list

Active background tasks:
1. [bg-1234] Running tests (50% complete, 2min remaining)
2. [bg-1235] Building Docker image (25% complete, 8min remaining)
3. [bg-1236] Deploying to staging (90% complete, 30sec remaining)
```

**查看任務狀態**：
```
User: /task status bg-1234

Task bg-1234: Running tests
Status: In progress
Progress: 120/245 tests (49%)
Started: 2025-11-08 10:30:15
Estimated completion: 2025-11-08 10:34:22
```

**查看任務輸出**：
```
User: /task show bg-1234

[Shows live output from the test run]
```

**取消任務**：
```
User: /task cancel bg-1234

Cancelled background task bg-1234
```

### 範例：平行開發

```
User: Run the build in the background

Claude: Starting build... (task-id: bg-5001)

User: Also run the linter in background

Claude: Starting linter... (task-id: bg-5002)

User: While those run, let's implement the new API endpoint

Claude: [Implements API endpoint while build and linter run]

[10 minutes later]

Claude: 📢 Build completed successfully (bg-5001)
📢 Linter found 12 issues (bg-5002)

User: Show me the linter issues

Claude: [Shows linter output from bg-5002]
```

### 配置

```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```

---

## 排程任務

排程任務讓你可以按重複排程自動執行提示，或設定一次性提醒。任務以會話為範圍——它們在 Claude Code 執行期間執行，會話結束時清除。自 v2.1.72+ 起可用。

### `/loop` 指令

```bash
# 明確的時間間隔
/loop 5m check if the deployment finished

# 自然語言
/loop check build status every 30 minutes
```

也支援標準的 5 欄位 cron 表達式用於精確排程。

### 一次性提醒

設定在特定時間觸發一次的提醒：

```
remind me at 3pm to push the release branch
in 45 minutes, run the integration tests
```

### 管理排程任務

| 工具 | 描述 |
|------|-------------|
| `CronCreate` | 建立新的排程任務 |
| `CronList` | 列出所有執行中的排程任務 |
| `CronDelete` | 刪除排程任務 |

**限制和行為**：
- 每個會話最多 **50 個排程任務**
- 以會話為範圍——會話結束時清除
- 重複任務在 **3 天**後自動到期
- 任務只在 Claude Code 執行時觸發——不會補執行錯過的觸發

### 行為細節

| 面向 | 詳細說明 |
|--------|--------|
| **重複抖動** | 最多間隔的 10%（最多 15 分鐘）|
| **一次性抖動** | 在 :00/:30 邊界上最多 90 秒 |
| **錯過的觸發** | 不補執行——若 Claude Code 未執行則跳過 |
| **持久性** | 重啟後不持久 |

### 雲端排程任務

使用 `/schedule` 建立在 Anthropic 基礎設施上執行的雲端排程任務：

```
/schedule daily at 9am run the test suite and report failures
```

雲端排程任務在重啟後持久，不需要 Claude Code 在本地執行。

### 停用排程任務

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

### 範例：監控部署

```
/loop 5m check the deployment status of the staging environment.
        If the deploy succeeded, notify me and stop looping.
        If it failed, show the error logs.
```

> **提示**：排程任務以會話為範圍。對於在重啟後仍可存活的持久自動化，請改用 CI/CD 流水線、GitHub Actions 或桌面應用程式排程任務。

---

## 權限模式

權限模式控制 Claude 在不需要明確批准的情況下可以採取哪些動作。

### 可用的權限模式

| 模式 | 行為 |
|---|---|
| `default` | 僅讀取檔案；所有其他動作都需要提示 |
| `acceptEdits` | 讀取和編輯檔案；指令需要提示 |
| `plan` | 僅讀取檔案（研究模式，不編輯）|
| `auto` | 所有動作都有背景安全分類器檢查（研究預覽版）|
| `bypassPermissions` | 所有動作，不檢查權限（危險）|
| `dontAsk` | 只執行預先批准的工具；所有其他工具被拒絕 |

在 CLI 中用 `Shift+Tab` 循環切換模式。用 `--permission-mode` 旗標或 `permissions.defaultMode` 設定來設定預設值。

### 啟用方法

**鍵盤快捷鍵**：
```bash
Shift + Tab  # 循環切換所有 6 個模式
```

**Slash command**：
```bash
/plan                  # 進入 plan 模式
```

**CLI 旗標**：
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**設定**：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 權限模式範例

#### 預設模式
Claude 對重要動作要求確認：

```
User: Fix the bug in auth.ts

Claude: I need to modify src/auth.ts to fix the bug.
The change will update the password validation logic.

Approve this change? (yes/no/show)
```

#### Plan 模式
在執行前審查實作計畫：

```
User: /plan Implement user authentication system

Claude: I'll create a plan for implementing authentication.

## Implementation Plan
[Detailed plan with phases and steps]

Ready to proceed? (yes/no/modify)
```

#### 接受編輯模式
自動接受檔案修改：

```
User: acceptEdits
User: Fix the bug in auth.ts

Claude: [Makes changes without asking]
```

### 使用場景

**程式碼審查**：
```
User: claude --permission-mode plan
User: Review this PR and suggest improvements

Claude: [Reads code, provides feedback, but cannot modify]
```

**配對程式設計**：
```
User: claude --permission-mode default
User: Let's implement the feature together

Claude: [Asks for approval before each change]
```

**自動化任務**：
```
User: claude --permission-mode acceptEdits
User: Fix all linting issues in the codebase

Claude: [Auto-accepts file edits without asking]
```

---

## Headless 模式

Print 模式（`claude -p`）讓 Claude Code 在沒有互動輸入的情況下執行，非常適合自動化和 CI/CD。這是非互動模式，取代了舊版的 `--headless` 旗標。

### 什麼是 Print 模式？

Print 模式支援：
- 自動化腳本執行
- CI/CD 整合
- 批次處理
- 排程任務

### 以 Print 模式執行（非互動式）

```bash
# 執行特定任務
claude -p "Run all tests"

# 處理管道內容
cat error.log | claude -p "Analyze these errors"

# CI/CD 整合（GitHub Actions）
- name: AI Code Review
  run: claude -p "Review PR"
```

### 其他 Print 模式使用範例

```bash
# 執行特定任務並擷取輸出
claude -p "Run all tests and generate coverage report"

# 使用結構化輸出
claude -p --output-format json "Analyze code quality"

# 從 stdin 輸入
echo "Analyze code quality" | claude -p "explain this"
```

### 範例：CI/CD 整合

**GitHub Actions**：
```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "Review this PR for:
            - Code quality issues
            - Security vulnerabilities
            - Performance concerns
            - Test coverage
            Output results as JSON" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### Print 模式配置

Print 模式（`claude -p`）支援幾個用於自動化的旗標：

```bash
# 限制自主輪次
claude -p --max-turns 5 "refactor this module"

# 結構化 JSON 輸出
claude -p --output-format json "analyze this codebase"

# 附 schema 驗證
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "find bugs in this code"

# 停用會話持久化
claude -p --no-session-persistence "one-off analysis"
```

---

## 會話管理

有效管理多個 Claude Code 會話。

### 會話管理指令

| 指令 | 描述 |
|---------|-------------|
| `/resume` | 以 ID 或名稱繼續對話 |
| `/rename` | 為目前會話命名 |
| `/fork` | 將目前會話 Fork 成新分支 |
| `claude -c` | 繼續最近的對話 |
| `claude -r "session"` | 以名稱或 ID 繼續會話 |

### 繼續會話

**繼續上次對話**：
```bash
claude -c
```

**繼續命名會話**：
```bash
claude -r "auth-refactor" "finish this PR"
```

**重命名目前會話**（在 REPL 內）：
```
/rename auth-refactor
```

### Fork 會話

Fork 會話以嘗試替代方法而不失去原始：

```
/fork
```

或從 CLI：
```bash
claude --resume auth-refactor --fork-session "try OAuth instead"
```

### 會話持久化

會話自動儲存並可繼續：

```bash
# 繼續上次對話
claude -c

# 以名稱或 ID 繼續特定會話
claude -r "auth-refactor"

# 繼續並 Fork 進行實驗
claude --resume auth-refactor --fork-session "alternative approach"
```

---

## 互動功能

### 鍵盤快捷鍵

Claude Code 支援鍵盤快捷鍵以提高效率。以下是官方文件的完整參考：

| 快捷鍵 | 描述 |
|----------|-------------|
| `Ctrl+C` | 取消目前輸入/生成 |
| `Ctrl+D` | 退出 Claude Code |
| `Ctrl+G` | 在外部編輯器中編輯計畫 |
| `Ctrl+L` | 清除終端機畫面 |
| `Ctrl+O` | 切換詳細輸出（查看推理過程）|
| `Ctrl+R` | 反向搜尋歷史 |
| `Ctrl+T` | 切換任務列表視圖 |
| `Ctrl+B` | 背景執行中的任務 |
| `Esc+Esc` | 回溯程式碼/對話 |
| `Shift+Tab` / `Alt+M` | 切換權限模式 |
| `Option+P` / `Alt+P` | 切換模型 |
| `Option+T` / `Alt+T` | 切換延伸思考 |

**行編輯（標準 readline 快捷鍵）：**

| 快捷鍵 | 動作 |
|----------|--------|
| `Ctrl + A` | 移至行首 |
| `Ctrl + E` | 移至行尾 |
| `Ctrl + K` | 剪切至行尾 |
| `Ctrl + U` | 剪切至行首 |
| `Ctrl + W` | 向後刪除單字 |
| `Ctrl + Y` | 貼上（yank）|
| `Tab` | 自動補全 |
| `↑ / ↓` | 指令歷史 |

### 自訂按鍵綁定

執行 `/keybindings` 建立自訂鍵盤快捷鍵，這會開啟 `~/.claude/keybindings.json` 進行編輯（v2.1.18+）。

**配置格式**：

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

將綁定設為 `null` 以解除預設快捷鍵。

### 可用的上下文

按鍵綁定限定在特定 UI 上下文中：

| 上下文 | 按鍵動作 |
|---------|-------------|
| **Chat** | `submit`、`cancel`、`cycleMode`、`modelPicker`、`thinkingToggle`、`undo`、`externalEditor`、`stash`、`imagePaste` |
| **Confirmation** | `yes`、`no`、`previous`、`next`、`nextField`、`cycleMode`、`toggleExplanation` |
| **Global** | `interrupt`、`exit`、`toggleTodos`、`toggleTranscript` |
| **Autocomplete** | `accept`、`dismiss`、`next`、`previous` |
| **HistorySearch** | `search`、`previous`、`next` |
| **Settings** | 特定於上下文的設定導覽 |
| **Tabs** | 標籤頁切換和管理 |
| **Help** | 說明面板導覽 |

共有 18 個上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 和 `Select`。

### 和弦支援

按鍵綁定支援和弦序列（多鍵組合）：

```
"ctrl+k ctrl+s"   → 兩鍵序列：先按 ctrl+k，再按 ctrl+s
"ctrl+shift+p"    → 同時按下修飾鍵
```

**按鍵語法**：
- **修飾鍵**：`ctrl`、`alt`（或 `opt`）、`shift`、`meta`（或 `cmd`）
- **大寫字母暗示 Shift**：`K` 等同於 `shift+k`
- **特殊鍵**：`escape`、`enter`、`return`、`tab`、`space`、`backspace`、`delete`、方向鍵

### 保留和衝突的按鍵

| 按鍵 | 狀態 | 備註 |
|-----|--------|-------|
| `Ctrl+C` | 保留 | 無法重新綁定（中斷）|
| `Ctrl+D` | 保留 | 無法重新綁定（退出）|
| `Ctrl+B` | 終端機衝突 | tmux 前綴鍵 |
| `Ctrl+A` | 終端機衝突 | GNU Screen 前綴鍵 |
| `Ctrl+Z` | 終端機衝突 | 程序暫停 |

> **提示**：如果快捷鍵不起作用，請檢查與你的終端機模擬器或多路復用器的衝突。

### Tab 補全

Claude Code 提供智慧 tab 補全：

```
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 指令歷史

存取之前的指令：

```
User: <↑>  # 上一個指令
User: <↓>  # 下一個指令
User: Ctrl+R  # 搜尋歷史

(reverse-i-search)`test': run all tests
```

### 多行輸入

對於複雜查詢，使用多行模式：

```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

**範例：**

```
User: \
> Implement a user authentication system
> with the following requirements:
> - JWT tokens
> - Email verification
> - Password reset
> - 2FA support
> \end

Claude: [Processes the multi-line request]
```

### 行內編輯

在傳送前編輯指令：

```
User: Deploy to prodcution<Backspace><Backspace>uction

[Edit in-place before sending]
```

### Vim 模式

為文字編輯啟用 Vi/Vim 按鍵綁定：

**啟用**：
- 使用 `/vim` 指令或 `/config` 啟用
- 用 `Esc` 切換到 NORMAL 模式，用 `i/a/o` 切換到 INSERT 模式

**導覽鍵**：
- `h` / `l` - 向左/右移動
- `j` / `k` - 向下/上移動
- `w` / `b` / `e` - 按單字移動
- `0` / `$` - 移至行首/行尾
- `gg` / `G` - 跳至文字開頭/結尾

**文字物件**：
- `iw` / `aw` - 內部/周圍單字
- `i"` / `a"` - 內部/周圍引號字串
- `i(` / `a(` - 內部/周圍括號

### Bash 模式

使用 `!` 前綴直接執行 shell 指令：

```bash
! npm test
! git status
! cat src/index.js
```

在不切換上下文的情況下快速執行指令。

---

## 語音輸入

語音輸入為 Claude Code 提供按住即錄的語音輸入，讓你可以說出提示而不是打字。

### 啟用語音輸入

```
/voice
```

### 功能

| 功能 | 描述 |
|---------|-------------|
| **按住即錄** | 按住一個鍵錄音，放開即傳送 |
| **20 種語言** | 語音轉文字支援 20 種語言 |
| **自訂按鍵綁定** | 透過 `/keybindings` 配置按住即錄的鍵 |
| **帳號要求** | 需要 Claude.ai 帳號進行 STT 處理 |

### 配置

在你的按鍵綁定檔案中自訂按住即錄的按鍵綁定（`/keybindings`）。語音輸入使用你的 Claude.ai 帳號進行語音轉文字處理。

---

## Channels

Channels（研究預覽版）允許 MCP servers 將訊息推送到執行中的 Claude Code 會話，與外部服務實現即時整合。

### 訂閱 Channels

```bash
# 在啟動時訂閱 channel plugins
claude --channels discord,telegram
```

### 支援的整合

| 整合 | 描述 |
|-------------|-------------|
| **Discord** | 在你的會話中接收並回應 Discord 訊息 |
| **Telegram** | 在你的會話中接收並回應 Telegram 訊息 |

### 配置

企業部署的**受管設定**：

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 受管設定控制整個組織允許哪些 channel plugins。

### 運作方式

1. MCP servers 作為連接外部服務的 channel plugins
2. 傳入訊息被推送到活躍的 Claude Code 會話
3. Claude 可以在會話上下文中讀取和回應訊息
4. Channel plugins 必須透過 `allowedChannelPlugins` 受管設定批准

---

## Chrome 整合

Chrome 整合將 Claude Code 連接到你的 Chrome 或 Microsoft Edge 瀏覽器，用於即時 web 自動化和除錯。這是自 v2.0.73+ 起可用的測試版功能（Edge 支援在 v1.0.36+ 中新增）。

### 啟用 Chrome 整合

**啟動時**：

```bash
claude --chrome      # 啟用 Chrome 連線
claude --no-chrome   # 停用 Chrome 連線
```

**在會話中**：

```
/chrome
```

選擇「預設啟用」以在所有未來會話中啟用 Chrome 整合。Claude Code 共享你的瀏覽器登入狀態，因此它可以與已驗證的 web 應用程式互動。

### 能力

| 能力 | 描述 |
|------------|-------------|
| **即時除錯** | 讀取主控台日誌、檢查 DOM 元素、即時除錯 JavaScript |
| **設計驗證** | 比較渲染後的頁面與設計稿 |
| **表單驗證** | 測試表單提交、輸入驗證和錯誤處理 |
| **Web 應用程式測試** | 與已驗證的應用程式互動（Gmail、Google Docs、Notion 等）|
| **資料提取** | 從網頁抓取並處理內容 |
| **會話錄製** | 將瀏覽器互動錄製為 GIF 檔案 |

### 網站層級權限

Chrome 擴充功能管理每個網站的存取。隨時透過擴充功能彈出視窗為特定網站授予或撤銷存取權。Claude Code 只與你明確允許的網站互動。

### 運作方式

Claude Code 在可見視窗中控制瀏覽器——你可以即時觀看動作發生。當瀏覽器遇到登入頁面或 CAPTCHA 時，Claude 會暫停並等待你手動處理後再繼續。

### 已知限制

- **瀏覽器支援**：僅限 Chrome 和 Edge——不支援 Brave、Arc 和其他 Chromium 瀏覽器
- **WSL**：Windows Subsystem for Linux 中不可用
- **第三方提供者**：Bedrock、Vertex 或 Foundry API 提供者不支援
- **Service worker 閒置**：在長時間會話中，Chrome 擴充功能的 service worker 可能進入閒置狀態

> **提示**：Chrome 整合是測試版功能。未來版本可能會擴展瀏覽器支援。

---

## 遠端控制

遠端控制讓你可以從手機、平板或任何瀏覽器繼續本地執行中的 Claude Code 會話。你的本地會話繼續在你的機器上執行——沒有任何東西移到雲端。在 Pro、Max、Team 和 Enterprise 方案上可用（v2.1.51+）。

### 啟動遠端控制

**從 CLI**：

```bash
# 以預設會話名稱啟動
claude remote-control

# 以自訂名稱啟動
claude remote-control --name "Auth Refactor"
```

**在會話中**：

```
/remote-control
/remote-control "Auth Refactor"
```

**可用旗標**：

| 旗標 | 描述 |
|------|-------------|
| `--name "title"` | 便於識別的自訂會話標題 |
| `--verbose` | 顯示詳細連線日誌 |
| `--sandbox` | 啟用檔案系統和網路隔離 |
| `--no-sandbox` | 停用沙盒隔離（預設）|

### 連線到會話

從其他裝置連線的三種方式：

1. **會話 URL** — 會話啟動時列印到終端機；在任何瀏覽器中開啟
2. **QR 碼** — 啟動後按 `空白鍵` 顯示可掃描的 QR 碼
3. **按名稱尋找** — 在 claude.ai/code 或 Claude 行動應用程式（iOS/Android）中瀏覽你的會話

### 安全性

- 在你的機器上不開放**任何入站連接埠**
- 僅限透過 TLS 的**出站 HTTPS**
- **限定範圍的憑證** — 多個短期、範圍限定的 tokens
- **會話隔離** — 每個遠端會話都是獨立的

### 遠端控制與 Web 上的 Claude Code 比較

| 面向 | 遠端控制 | Web 上的 Claude Code |
|--------|---------------|-------------------|
| **執行位置** | 在你的機器上執行 | 在 Anthropic 雲端上執行 |
| **本地工具** | 完整存取本地 MCP servers、檔案和 CLI | 無本地依賴 |
| **使用場景** | 從其他裝置繼續本地工作 | 從任何瀏覽器重新開始 |

### 限制

- 每個 Claude Code 執行實例一個遠端會話
- 主機機器上的終端機必須保持開啟
- 若網路無法連接，會話在約 10 分鐘後逾時

### 使用場景

- 離開桌子時從行動裝置或平板控制 Claude Code
- 使用更豐富的 claude.ai UI 同時保持本地工具執行
- 使用完整的本地開發環境在外隨時進行快速程式碼審查

---

## Web 會話

Web 會話讓你可以直接在 claude.ai/code 的瀏覽器中執行 Claude Code，或從 CLI 建立 web 會話。

### 建立 Web 會話

```bash
# 從 CLI 建立新的 web 會話
claude --remote "implement the new API endpoints"
```

這會在 claude.ai 啟動一個你可以從任何瀏覽器存取的 Claude Code 會話。

### 在本地繼續 Web 會話

如果你在 web 上開始了一個會話並想在本地繼續：

```bash
# 在本地終端機中繼續 web 會話
claude --teleport
```

或在互動式 REPL 中：
```
/teleport
```

### 使用場景

- 在一台機器上開始工作，在另一台繼續
- 與團隊成員分享會話 URL
- 使用 web UI 進行視覺化差異審查，然後切換到終端機執行

---

## 桌面應用程式

Claude Code 桌面應用程式提供附視覺化差異審查、平行會話和整合連接器的獨立應用程式。適用於 macOS 和 Windows（Pro、Max、Team 和 Enterprise 方案）。

### 安裝

從 [claude.ai](https://claude.ai) 下載適合你平台的版本：
- **macOS**：通用版本（Apple Silicon 和 Intel）
- **Windows**：x64 和 ARM64 安裝程式可用

設定說明請參閱[桌面快速開始](https://code.claude.com/docs/en/desktop-quickstart)。

### 從 CLI 移交

將目前的 CLI 會話傳送到桌面應用程式：

```
/desktop
```

### 核心功能

| 功能 | 描述 |
|---------|-------------|
| **差異視圖** | 逐檔案視覺化審查附行內評論；Claude 讀取評論並修改 |
| **應用程式預覽** | 自動啟動開發 servers 並附嵌入式瀏覽器進行即時驗證 |
| **PR 監控** | GitHub CLI 整合，自動修正 CI 失敗，檢查通過後自動合併 |
| **平行會話** | 側邊欄中的多個會話，附自動 Git worktree 隔離 |
| **排程任務** | 應用程式開啟時執行的重複任務（每小時、每天、工作日、每週）|
| **豐富渲染** | 附語法高亮的程式碼、Markdown 和圖表渲染 |

### 應用程式預覽配置

在 `.claude/launch.json` 中配置開發 server 行為：

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### 連接器

連接外部服務以獲得更豐富的上下文：

| 連接器 | 能力 |
|-----------|------------|
| **GitHub** | PR 監控、issue 追蹤、程式碼審查 |
| **Slack** | 通知、頻道上下文 |
| **Linear** | Issue 追蹤、sprint 管理 |
| **Notion** | 文件、知識庫存取 |
| **Asana** | 任務管理、專案追蹤 |
| **Calendar** | 排程感知、會議上下文 |

> **注意**：連接器不適用於遠端（雲端）會話。

### 遠端和 SSH 會話

- **遠端會話**：在 Anthropic 雲端基礎設施上執行；即使應用程式關閉也繼續執行。可從 claude.ai/code 或 Claude 行動應用程式存取
- **SSH 會話**：透過 SSH 連接遠端機器，完整存取遠端檔案系統和工具。Claude Code 必須安裝在遠端機器上

### 桌面應用程式的權限模式

桌面應用程式支援與 CLI 相同的 4 種權限模式：

| 模式 | 行為 |
|------|----------|
| **詢問權限**（預設）| 審查並批准每個編輯和指令 |
| **自動接受編輯** | 檔案編輯自動批准；指令需要手動批准 |
| **Plan 模式** | 在進行任何變更前審查方法 |
| **繞過權限** | 自動執行（僅限沙盒，管理員控制）|

### 企業功能

- **管理員主控台**：控制組織的 Code 標籤頁存取和權限設定
- **MDM 部署**：在 macOS 上透過 MDM 或在 Windows 上透過 MSIX 部署
- **SSO 整合**：要求組織成員使用單一登入
- **受管設定**：集中管理團隊配置和模型可用性

---

## 任務列表

任務列表功能提供跨上下文壓縮（對話歷史被截斷以符合上下文視窗時）的持久化任務追蹤。

### 切換任務列表

在會話期間按 `Ctrl+T` 切換任務列表視圖。

### 持久化任務

任務在上下文壓縮後仍然持久，確保長時間執行的工作項目不會在對話上下文被截斷時丟失。這對於複雜的多步驟實作特別有用。

### 命名任務目錄

使用 `CLAUDE_CODE_TASK_LIST_ID` 環境變數建立跨會話共享的命名任務目錄：

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

這允許多個會話共享同一個任務列表，對於團隊工作流程或多會話專案很有用。

---

## 提示建議

提示建議根據你的 git 歷史和目前對話上下文顯示灰色的範例指令。

### 運作方式

- 建議以灰色文字顯示在你的輸入提示下方
- 按 `Tab` 接受建議
- 按 `Enter` 接受並立即提交
- 建議具有上下文感知能力，從 git 歷史和對話狀態中提取

### 停用提示建議

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git Worktrees

Git Worktrees 讓你可以在獨立的 worktree 中啟動 Claude Code，實現在不同分支上的平行工作，無需暫存或切換。

### 在 Worktree 中啟動

```bash
# 在獨立的 worktree 中啟動 Claude Code
claude --worktree
# 或
claude -w
```

### Worktree 位置

Worktrees 建立於：
```
<repo>/.claude/worktrees/<name>
```

### Monorepo 的稀疏檢出

使用 `worktree.sparsePaths` 設定在 monorepo 中執行稀疏檢出，減少磁碟使用量和複製時間：

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### Worktree 工具和 Hooks

| 項目 | 描述 |
|------|-------------|
| `ExitWorktree` | 退出並清理目前 worktree 的工具 |
| `WorktreeCreate` | 建立 worktree 時觸發的 hook 事件 |
| `WorktreeRemove` | 移除 worktree 時觸發的 hook 事件 |

### 自動清理

如果在 worktree 中沒有進行任何變更，會話結束時會自動清理。

### 使用場景

- 在功能分支上工作，同時保持主分支不受影響
- 在隔離環境中執行測試，不影響工作目錄
- 在可丟棄的環境中嘗試實驗性變更
- 在 monorepo 中稀疏檢出特定套件以加快啟動速度

---

## 沙盒隔離

沙盒隔離為 Claude Code 執行的 Bash 指令提供 OS 層級的檔案系統和網路隔離。這與權限規則互補，提供額外的安全層。

### 啟用沙盒隔離

**Slash command**：
```
/sandbox
```

**CLI 旗標**：
```bash
claude --sandbox       # 啟用沙盒隔離
claude --no-sandbox    # 停用沙盒隔離
```

### 配置設定

| 設定 | 描述 |
|---------|-------------|
| `sandbox.enabled` | 啟用或停用沙盒隔離 |
| `sandbox.failIfUnavailable` | 若無法啟用沙盒隔離則失敗 |
| `sandbox.filesystem.allowWrite` | 允許寫入存取的路徑 |
| `sandbox.filesystem.allowRead` | 允許讀取存取的路徑 |
| `sandbox.filesystem.denyRead` | 拒絕讀取存取的路徑 |
| `sandbox.enableWeakerNetworkIsolation` | 在 macOS 上啟用較弱的網路隔離 |

### 配置範例

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```

### 運作方式

- Bash 指令在受限檔案系統存取的沙盒環境中執行
- 網路存取可以被隔離以防止意外的外部連線
- 與權限規則一起工作以實現深度防禦
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 進行網路限制（macOS 上不支援完整的網路隔離）

### 使用場景

- 安全執行不受信任或生成的程式碼
- 防止意外修改專案外的檔案
- 在自動化任務期間限制網路存取

---

## 受管設定（企業版）

受管設定讓企業管理員可以使用平台原生管理工具在整個組織部署 Claude Code 配置。

### 部署方法

| 平台 | 方法 | 自版本 |
|----------|--------|-------|
| macOS | 受管 plist 檔案（MDM）| v2.1.51+ |
| Windows | Windows 登錄 | v2.1.51+ |
| 跨平台 | 受管配置檔案 | v2.1.51+ |
| 跨平台 | 受管 drop-ins（`managed-settings.d/` 目錄）| v2.1.83+ |

### 受管 Drop-ins

自 v2.1.83 起，管理員可以將多個受管設定檔案部署到 `managed-settings.d/` 目錄。檔案按字母順序合併，允許跨團隊的模組化配置：

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 可用的受管設定

| 設定 | 描述 |
|---------|-------------|
| `disableBypassPermissionsMode` | 防止使用者啟用繞過權限 |
| `availableModels` | 限制使用者可以選擇的模型 |
| `allowedChannelPlugins` | 控制允許哪些 channel plugins |
| `autoMode.environment` | 為 auto 模式配置可信任的基礎設施 |
| 自訂政策 | 組織特定的權限和工具政策 |

### 範例：macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 配置與設定

### 配置檔案位置

1. **全域配置**：`~/.claude/config.json`
2. **專案配置**：`./.claude/config.json`
3. **使用者配置**：`~/.config/claude-code/settings.json`

### 完整配置範例

**核心進階功能配置：**

```json
{
  "permissions": {
    "mode": "default"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```

**延伸配置範例：**

```json
{
  "permissions": {
    "mode": "default",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```

### 環境變數

使用環境變數覆蓋配置：

```bash
# 模型選擇
export ANTHROPIC_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 配置
export ANTHROPIC_API_KEY=sk-ant-...

# 思考配置
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high

# 功能切換
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # 由 --bare 旗標設定

# MCP 配置
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# 任務管理
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# Agent 團隊（實驗性）
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=true

# Subagent 和 plugin 配置
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=true

# 子程序和串流
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000
```

### 配置管理指令

```
User: /config
[Opens interactive configuration menu]
```

`/config` 指令提供互動式選單以切換設定，例如：
- 延伸思考開/關
- 詳細輸出
- 權限模式
- 模型選擇

### 每個專案的配置

在你的專案中建立 `.claude/config.json`：

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```

---

## 最佳實踐

### 規劃模式
- ✅ 用於複雜的多步驟任務
- ✅ 批准前審查計畫
- ✅ 需要時修改計畫
- ❌ 不要用於簡單任務

### 延伸思考
- ✅ 用於架構決策
- ✅ 用於複雜問題解決
- ✅ 審查思考過程
- ❌ 不要用於簡單查詢

### 背景任務
- ✅ 用於長時間執行的操作
- ✅ 監控任務進度
- ✅ 優雅地處理任務失敗
- ❌ 不要啟動太多並發任務

### 權限
- ✅ 程式碼審查使用 `plan`（唯讀）
- ✅ 互動式開發使用 `default`
- ✅ 自動化工作流程使用 `acceptEdits`
- ✅ 附安全防護的自主工作使用 `auto`
- ❌ 除非絕對必要，不要使用 `bypassPermissions`

### 會話
- ✅ 為不同任務使用獨立會話
- ✅ 儲存重要的會話狀態
- ✅ 清理舊會話
- ❌ 不要在一個會話中混合不相關的工作

---

## 相關概念

有關 Claude Code 和相關功能的更多資訊：

- [官方互動模式文件](https://code.claude.com/docs/en/interactive-mode)
- [官方 Headless 模式文件](https://code.claude.com/docs/en/headless)
- [CLI 參考](https://code.claude.com/docs/en/cli-reference)
- [Checkpoints 指南](../08-checkpoints/) - 會話管理和回溯
- [Slash Commands](../01-slash-commands/) - 指令參考
- [Memory 指南](../02-memory/) - 持久化上下文
- [Skills 指南](../03-skills/) - 自主能力
- [Subagents 指南](../04-subagents/) - 委派任務執行
- [MCP 指南](../05-mcp/) - 外部資料存取
- [Hooks 指南](../06-hooks/) - 事件驅動自動化
- [Plugins 指南](../07-plugins/) - 套件擴充功能
- [官方排程任務文件](https://code.claude.com/docs/en/scheduled-tasks)
- [官方 Chrome 整合文件](https://code.claude.com/docs/en/chrome)
- [官方遠端控制文件](https://code.claude.com/docs/en/remote-control)
- [官方按鍵綁定文件](https://code.claude.com/docs/en/keybindings)
