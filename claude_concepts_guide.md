<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 概念完整指南

一份全面的參考指南，透過表格、圖表與實際範例，涵蓋斜線命令、子代理、記憶、MCP Protocol 以及代理技能。

---

## 目錄

1. [斜線命令](#slash-commands)
2. [子代理](#subagents)
3. [記憶](#memory)
4. [MCP Protocol](#mcp-protocol)
5. [代理技能](#agent-skills)
6. [外掛](#claude-code-plugins)
7. [鉤子](#hooks)
8. [檢查點與回溯](#checkpoints-and-rewind)
9. [進階功能](#advanced-features)
10. [比較與整合](#comparison--integration)

---

## 斜線命令

### 概述

斜線命令是使用者觸發的捷徑，以 Markdown 檔案形式儲存，並可由 Claude Code 執行。它們能讓團隊將常用的提示詞與工作流程標準化。

### 架構

```mermaid
graph TD
    A["User Input: /command-name"] -->|Triggers| B["Search .claude/commands/"]
    B -->|Finds| C["command-name.md"]
    C -->|Loads| D["Markdown Content"]
    D -->|Executes| E["Claude Processes Prompt"]
    E -->|Returns| F["Result in Context"]
```

### 檔案結構

```mermaid
graph LR
    A["Project Root"] -->|contains| B[".claude/commands/"]
    B -->|contains| C["optimize.md"]
    B -->|contains| D["test.md"]
    B -->|contains| E["docs/"]
    E -->|contains| F["generate-api-docs.md"]
    E -->|contains| G["generate-readme.md"]
```

### 命令組織表格

| 位置 | 範圍 | 可用性 | 使用案例 | Git 追蹤 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 專案特定 | 團隊成員 | 團隊工作流程、共享標準 | ✅ 是 |
| `~/.claude/commands/` | 個人 | 個人使用者 | 跨專案的個人捷徑 | ❌ 否 |
| 子目錄 | 命名空間 | 依據父目錄 | 按類別進行組織 | ✅ 是 |

### 功能與能力

| 功能 | 範例 | 支援 |
|---------|---------|-----------|
| Shell 腳本執行 | `bash scripts/deploy.sh` | ✅ 是 |
| 檔案引用 | `@path/to/file.js` | ✅ 是 |
| Bash 整合 | `$(git log --oneline)` | ✅ 是 |
| 參數 | `/pr --verbose` | ✅ 是 |
| MCP 命令 | `/mcp__github__list_prs` | ✅ 是 |

### 實際範例

#### 範例 1：程式碼優化命令

**檔案：** `.claude/commands/optimize.md`

```markdown
---
name: Code Optimization
description: Analyze code for performance issues and suggest optimizations
tags: performance, analysis
---

# Code Optimization

Review the provided code for the following issues in order of priority:

1. **Performance bottlenecks** - identify O(n²) operations, inefficient loops
2. **Memory leaks** - find unreleased resources, circular references
3. **Algorithm improvements** - suggest better algorithms or data structures
4. **Caching opportunities** - identify repeated computations
5. **Concurrency issues** - find race conditions or threading problems
```

請以以下格式回應：
- 問題嚴重性 (Critical/High/Medium/Low)
- 程式碼位置
- 解釋
- 建議修復方案與程式碼範例
```

**用法：**
```bash
# 使用者在 Claude Code 中輸入
/optimize

# Claude 載入提示詞並等待程式碼輸入
```

#### 範例 2：Pull Request 輔助命令

**檔案：** `.claude/commands/pr.md`

```markdown
---
name: Prepare Pull Request
description: Clean up code, stage changes, and prepare a pull request
tags: git, workflow
---

# Pull Request 準備檢查清單

在建立 PR 之前，請執行以下步驟：

1. 執行 linting：`prettier --write .`
2. 執行測試：`npm test`
3. 檢查 git diff：`git diff HEAD`
4. 暫存變更：`git add .`
5. 依照 conventional commits 建立 commit 訊息：
   - `fix:` 用於修復 bug
   - `feat:` 用於新功能
   - `docs:` 用於文件
   - `refactor:` 用於程式碼重構
   - `test:` 用於新增測試
   - `chore:` 用於維護工作

6. 生成 PR 摘要，包含：
   - 變更內容
   - 變更原因
   - 已執行的測試
   - 潛在影響
```

**用法：**
```bash
/pr

# Claude 執行檢查清單並準備 PR
```

#### 範例 3：層級式文件生成器

**檔案：** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: Generate API Documentation
description: Create comprehensive API documentation from source code
tags: documentation, api
---

# API 文件生成器

透過以下步驟生成 API 文件：

1. 掃描 `/src/api/` 下的所有檔案
2. 提取函式簽章與 JSDoc 註解
3. 依據 endpoint/module 進行整理
4. 建立包含範例的 markdown 文件
5. 包含 request/response schema
6. 加入錯誤處理文件

輸出格式：
- 位於 `/docs/api.md` 的 Markdown 檔案
- 包含所有 endpoint 的 curl 範例
- 加入 TypeScript types
```

### 命令生命週期圖

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/commands/
    FS-->>Claude: Returns optimize.md
    Claude->>Claude: Loads Markdown content
    Claude->>User: Displays prompt context
    User->>Claude: Provides code to analyze
    Claude->>CLI: (May execute scripts)
    CLI-->>Claude: Results
    Claude->>User: Returns analysis
```

### 最佳實務

| ✅ 應該 | ❌ 不該 |
|------|---------|
| 使用清晰且具行動導向的名稱 | 為一次性任務建立命令 |
| 在 description 中記錄觸發詞 | 在命令中構建複雜邏輯 |
| 保持命令專注於單一任務 | 建立冗餘的命令 |
| 將專案命令納入版本控制 | 將敏感資訊寫死 (Hardcode) |
| 整理在子目錄中 | 建立冗長的命令列表 |
| 使用簡單、易讀的提示詞 | 使用縮寫或晦澀的措辭 |

---

## 子代理

### 概述

子代理是具有隔離上下文視窗與自定義系統提示詞的專業化 AI 助手。它們能夠在維持清晰的關注點分離（separation of concerns）之餘，實現任務委派執行。

### 架構圖

```mermaid
graph TB
    User["👤 User"]
    Main["🎯 Main Agent<br/>(Coordinator)"]
    Reviewer["🔍 Code Reviewer<br/>Subagent"]
    Tester["✅ Test Engineer<br/>Subagent"]
    Docs["📝 Documentation<br/>Subagent"]

    User -->|asks| Main
    Main -->|delegates| Reviewer
    Main -->|delegates| Tester
    Main -->|delegates| Docs
    Reviewer -->|returns result| Main
    Tester -->|returns result| Main
    Docs -->|returns result| Main
    Main -->|synthesizes| User
```

### 子代理生命週期

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as Main Agent
    participant CodeReviewer as Code Reviewer<br/>Subagent
    participant Context as Separate<br/>Context Window

    User->>MainAgent: "Build new auth feature"
    MainAgent->>MainAgent: Analyze task
    MainAgent->>CodeReviewer: "Review this code"
    CodeReviewer->>Context: Initialize clean context
    Context->>CodeReviewer: Load reviewer instructions
    CodeReviewer->>CodeReviewer: Perform review
    CodeReviewer-->>MainAgent: Return findings
    MainAgent->>MainAgent: Incorporate results
    MainAgent-->>User: Provide synthesis
```

### 子代理配置表

| 配置 | 類型 | 用途 | 範例 |
|---------------|------|---------|---------|
| `name` | String | 代理識別碼 | `code-reviewer` |
| `description` | String | 用途與觸發詞 | `Comprehensive code quality analysis` |
| `tools` | List/String | 允許的能力 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 行為指令 | 自定義指南 |

### 工具存取層級

```mermaid
graph TD
    A["Subagent Configuration"] -->|Option 1| B["Inherit All Tools<br/>from Main Thread"]
    A -->|Option 2| C["Specify Individual Tools"]
    B -->|Includes| B1["File Operations"]
    B -->|Includes| B2["Shell Commands"]
    B -->|Includes| B3["MCP Tools"]
    C -->|Explicit List| C1["read, grep, diff"]
    C -->|Explicit List| C2["Bash(npm:*), Bash(test:*)"]
```

### 實際範例

#### 範例 1：完整的子代理設定

**檔案：** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: Comprehensive code quality and maintainability analysis
tools: read, grep, diff, lint_runner
---

# Code Reviewer Agent

You are an expert code reviewer specializing in:
- Performance optimization
- Security vulnerabilities
- Code maintainability
- Testing coverage
- Design patterns
```

## 審查優先順序（依序）

1. **安全性問題** - 身分驗證、授權、資料外洩
2. **效能問題** - O(n²) 操作、memory leaks、低效率查詢
3. **程式碼品質** - 可讀性、命名、文件
4. **測試覆蓋率** - 缺失的測試、邊界情況
5. **設計模式** - SOLID 原則、架構

## 審查輸出格式

針對每個問題：
- **Severity**: Critical / High / Medium / Low
- **Category**: Security / Performance / Quality / Testing / Design
- **Location**: 檔案路徑與行號
- **Issue Description**: 問題所在及其原因
- **Suggested Fix**: 程式碼範例
- **Impact**: 對系統的影響

## 審查範例

### Issue: N+1 Query Problem
- **Severity**: High
- **Category**: Performance
- **Location**: src/user-service.ts:45
- **Issue**: 迴圈在每次迭代中執行資料庫查詢
- **Fix**: 使用 JOIN 或批次查詢
```

**File:** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: Test strategy, coverage analysis, and automated testing
tools: read, write, bash, grep
---

# Test Engineer Agent

你是以下領域的專家：
- 編寫全面的測試套件
- 確保高程式碼覆蓋率 (>80%)
- 測試邊界情況與錯誤情境
- 效能基準測試
- 整合測試

## 測試策略

1. **單元測試** - 個別函式/方法
2. **整合測試** - 組件間的互動
3. **端到端測試** - 完整的工作流程
4. **邊界情況** - 邊界條件
5. **錯誤情境** - 錯誤處理

## 測試輸出需求

- JavaScript/TypeScript 使用 Jest
- 為每個測試包含 setup/teardown
- 模擬（Mock）外部依賴
- 記錄測試目的
- 在相關時包含效能斷言（assertions）

## 覆蓋率需求

- 最低 80% 程式碼覆蓋率
- 關鍵路徑需達 100%
- 回報缺失的覆蓋範圍

```

**檔案：** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: 技術文件、API 文件與使用者指南
tools: read, write, grep
---

# Documentation Writer Agent

你負責建立：
- 包含範例的 API 文件
- 使用者指南與教學
- 架構文件
- 更新日誌（Changelog）條目
- 程式碼註解改進

## 文件標準

1. **清晰度** - 使用簡單、明瞭的語言
2. **範例** - 包含實際的程式碼範例
3. **完整性** - 涵蓋所有參數與回傳值
4. **結構** - 使用一致的格式
5. **準確性** - 對照實際程式碼進行驗證

## 文件章節

### 用於 API
- 描述
- 參數（含類型）
- 回傳值（含類型）
- 拋出異常（可能的錯誤）
- 範例（curl, JavaScript, Python）
- 相關端點

### 用於功能特性
- 概觀
- 前置條件
- 逐步操作說明
- 預期結果
- 疑難排解
- 相關主題
```

#### 範例 2：子代理委派實例

```markdown
# 場景：建立支付功能

## 使用者請求
「建立一個與 Stripe 整合的安全支付處理功能」

## 主代理工作流程

1. **規劃階段**
   - 理解需求
   - 確定所需任務
   - 規劃架構

2. **委派給 Code Reviewer 子代理**
   - 任務：「審查支付處理實作的安全性」
   - 上下文：驗證、API keys、token 處理
   - 審查重點：SQL 注入、金鑰外洩、強制執行 HTTPS

3. **委派給 Test Engineer 子代理**
   - 任務：「為支付流程建立全面的測試」
   - 上下文：成功情境、失敗情境、邊界案例
   - 建立測試：有效支付、信用卡遭拒、網路故障、webhooks

4. **委派給 Documentation Writer 子代理**
   - 任務：「記錄支付 API 端點」
   - 上下文：請求/回應架構（schemas）
   - 產出：包含 curl 範例與錯誤代碼的 API 文件

5. **綜合整理**
   - 主代理收集所有輸出
   - 整合發現結果
   - 向使用者回傳完整的解決方案
```

#### 範例 3：工具權限範圍限制

**限制性設定 - 僅限特定命令**

```yaml
---
name: secure-reviewer
description: 以安全性為核心且具備最小權限的程式碼審查
tools: read, grep
---

# Secure Code Reviewer

僅針對安全性漏洞進行程式碼審查。

此代理：
- ✅ 讀取檔案進行分析
- ✅ 搜尋模式
- ❌ 無法執行程式碼
- ❌ 無法修改檔案
- ❌ 無法執行測試

這確保了審查者不會意外破壞任何內容。
```

**擴充性設定 - 具備實作所需的所有工具**

```yaml
---
name: implementation-agent
description: 用於功能開發的完整實作能力
```

tools: read, write, bash, grep, edit, glob
---

# Implementation Agent

根據規格說明開發功能。

此代理：
- ✅ 讀取規格說明
- ✅ 撰寫新的程式碼檔案
- ✅ 執行建置指令
- ✅ 搜尋程式碼庫
- ✅ 編輯現有檔案
- ✅ 尋找符合模式的檔案

具備獨立開發功能的完整能力。
```

### Subagent Context Management

```mermaid
graph TB
    A["Main Agent Context<br/>50,000 tokens"]
    B["Subagent 1 Context<br/>20,000 tokens"]
    C["Subagent 2 Context<br/>20,000 tokens"]
    D["Subagent 3 Context<br/>20,000 tokens"]

    A -->|Clean slate| B
    A -->|Clean slate| C
    A -->|Clean slate| D

    B -->|Results only| A
    C -->|Results only| A
    D -->|Results only| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 何時使用 Subagents

| 場景 | 使用 Subagent | 原因 |
|----------|--------------|-----|
| 包含多個步驟的複雜功能 | ✅ 是 | 分離關注點，防止上下文污染 |
| 快速程式碼審查 | ❌ 否 | 不需要額外的開銷 |
| 並行任務執行 | ✅ 是 | 每個 subagent 擁有各自的上下文 |
| 需要專業知識時 | ✅ 是 | 使用自定義系統提示詞 |
| 長時間運行的分析 | ✅ 是 | 防止主上下文耗盡 |
| 單一任務 | ❌ 否 | 不必要地增加延遲 |

### Agent Teams

Agent Teams 協調多個處理相關任務的代理。與其一次只委派給一個 subagent，Agent Teams 允許主代理編排一群代理，讓它們進行協作、共享中間結果，並朝著共同目標努力。這對於大規模任務非常有用，例如全端功能開發，其中前端代理、後端代理與測試代理可以並行工作。

---

## 記憶

### 概觀

記憶功能讓 Claude 能夠在不同的會話與對話之間保留上下文。它以兩種形式存在：claude.ai 中的自動合成，以及 Claude Code 中基於檔案系統的 CLAUDE.md。

### 記憶架構

```mermaid
graph TB
    A["Claude Session"]
    B["User Input"]
    C["Memory System"]
    D["Memory Storage"]

    B -->|User provides info| C
    C -->|Synthesizes every 24h| D
    D -->|Loads automatically| A
    A -->|Uses context| C
```

### Claude Code 中的記憶層級 (7 個層級)

Claude Code 從 7 個層級載入記憶，按優先順序從高到低排列：

```mermaid
graph TD
    A["1. Managed Policy<br/>Enterprise admin policies"] --> B["2. Project Memory<br/>./CLAUDE.md"]
    B --> C["3. Project Rules<br/>.claude/rules/*.md"]
    C --> D["4. User Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. User Rules<br/>~/.claude/rules/*.md"]
    E --> F["6. Local Memory<br/>.claude/local/CLAUDE.md"]
    F --> G["7. Auto Memory<br/>Automatically captured preferences"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### 記憶位置對照表

| 層級 | 位置 | 範圍 | 優先順序 | 共享 | 最適合用於 |
|------|----------|-------|----------|--------|----------|
| 1. Managed Policy | Enterprise admin | Organization | 最高 | 所有組織使用者 | 合規性、安全政策 |
| 2. Project | `./CLAUDE.md` | Project | 高 | 團隊 (Git) | 團隊標準、架構 |
| 3. Project Rules | `.claude/rules/*.md` | Project | 高 | 團隊 (Git) | 模組化專案慣例 |
| 4. User | `~/.claude/CLAUDE.md` | Personal | 中 | 個人 | 個人偏好 |
| 5. User Rules | `~/.claude/rules/*.md` | Personal | 中 | 個人 | 個人規則模組 |
| 6. Local | `.claude/local/CLAUDE.md` | Local | 低 | 不共享 | 特定機器設定 |
| 7. Auto Memory | Automatic | Session | 最低 | 個人 | 學習到的偏好、模式 |

### 自動記憶 (Auto Memory)

自動記憶會自動擷取在會話期間觀察到的使用者偏好與模式。Claude 會從您的互動中學習並記住：

- 程式碼風格偏好
- 您常做的修正
- 框架與工具選擇
- 溝通風格偏好

自動記憶在背景運作，不需要手動配置。

### 記憶更新生命週期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: Open ~/.claude/settings.json
    Claude->>Memory: Write to ./CLAUDE.md
    Memory-->>Claude: File saved
    Claude->>Claude: Load updated memory
```

Claude-->>User: "Memory saved!"
```

### 實際範例

#### 範例 1：專案記憶結構

**檔案：** `./CLAUDE.md`

```markdown
# 專案配置

## 專案概觀
- **名稱**: E-commerce Platform
- **技術棧**: Node.js, PostgreSQL, React 18, Docker
- **團隊規模**: 5 位開發人員
- **截止日期**: 2025 年 Q4

## 架構
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 開發標準

### 程式碼風格
- 使用 Prettier 進行格式化
- 使用帶有 airbnb 配置的 ESLint
- 最大行長：100 字元
- 使用 2 個空格縮排

### 命名慣例
- **檔案**: kebab-case (user-controller.js)
- **類別**: PascalCase (UserService)
- **函式/變數**: camelCase (getUserById)
- **常數**: UPPER_SNAKE_CASE (API_BASE_URL)
- **資料庫資料表**: snake_case (user_accounts)

### Git 工作流程
- 分支名稱：`feature/description` 或 `fix/description`
- Commit 訊息：遵循 conventional commits
- 合併前須提交 PR
- 所有 CI/CD 檢查必須通過
- 最少需 1 個審核通過

### 測試需求
- 最低 80% 程式碼覆蓋率
- 所有關鍵路徑必須包含測試
- 使用 Jest 進行單元測試
- 使用 Cypress 進行 E2E 測試
- 測試檔案名稱：`*.test.ts` 或 `*.spec.ts`

### API 標準
- 僅限 RESTful 端點
- JSON 請求/回應
- 正確使用 HTTP 狀態碼
- API 端點版本化：`/api/v1/`
- 為所有端點提供範例文件

### 資料庫
- 使用 migrations 進行結構變更
- 絕不將憑證寫死在程式碼中
- 使用連線池 (connection pooling)
- 在開發環境中啟用查詢日誌
- 需要定期備份

### 部署
- 基於 Docker 的部署
- Kubernetes 編排
- 藍綠部署策略
- 失敗時自動回滾
- 部署前執行資料庫 migrations
```

## 常用命令

| 命令 | 用途 |
|---------|---------|
| `npm run dev` | 啟動開發伺服器 |
| `npm test` | 執行測試套件 |
| `npm run lint` | 檢查程式碼風格 |
| `npm run build` | 建置生產版本 |
| `npm run migrate` | 執行資料庫遷移 |

## 團隊聯絡人
- 技術負責人 (Tech Lead): Sarah Chen (@sarah.chen)
- 產品經理 (Product Manager): Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## 已知問題與解決方案
- PostgreSQL 連線池在尖峰時段限制為 20
- 解決方案：實作查詢佇列
- Safari 14 與 async generators 的相容性問題
- 解決方案：使用 Babel transpiler

## 相關專案
- 分析儀表板 (Analytics Dashboard): `/projects/analytics`
- 行動應用程式 (Mobile App): `/projects/mobile`
- 管理後台 (Admin Panel): `/projects/admin`

#### 範例 2：特定目錄的記憶

**檔案：** `./src/api/CLAUDE.md`

~~~~markdown
# API 模組標準

此檔案會覆蓋根目錄的 CLAUDE.md，適用於 /src/api/ 中的所有內容

## API 特定標準

### 請求驗證
- 使用 Zod 進行 schema 驗證
- 務必驗證輸入內容
- 若驗證失敗，回傳 400 錯誤
- 包含欄位層級的錯誤詳情

### 身分驗證
- 所有端點皆需要 JWT token
- Token 放置於 Authorization header
- Token 在 24 小時後過期
- 實作 refresh token 機制

### 回應格式

所有回應必須遵循此結構：

```json
{
  "success": true,
  "data": { /* actual data */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### 錯誤回應：
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 分頁
- 使用基於游標 (cursor-based) 的分頁（而非 offset）
- 包含 `hasMore` 布林值
- 最大分頁大小限制為 100
- 預設分頁大小：20

### 速率限制 (Rate Limiting)
- 已驗證使用者每小時 1000 次請求
- 公開端點每小時 100 次請求
- 超過限制時回傳 429
- 包含 retry-after header

### 快取
- 使用 Redis 進行 session 快取
- 快取時長：預設 5 分鐘
- 執行寫入操作時失效快取
- 使用資源類型為快取鍵 (cache keys) 加上標籤
~~~~

#### 範例 3：個人記憶

**檔案：** `~/.claude/CLAUDE.md`

~~~~markdown
# 我的開發偏好
~~~~

## 關於我
- **經驗程度**：8 年全端開發經驗
- **偏好語言**：TypeScript, Python
- **溝通風格**：直接，並附帶範例
- **學習風格**：視覺化圖表搭配程式碼

## 程式碼偏好

### 錯誤處理
我偏好使用 try-catch 區塊進行明確的錯誤處理，並提供具備意義的錯誤訊息。
避免使用通用的錯誤訊息。務必記錄錯誤以供除錯。

### 註解
註解應說明「為什麼（WHY）」，而非「做了什麼（WHAT）」。程式碼應具備自我文件化特性。
註解應解釋業務邏輯或非顯而易見的決策。

### 測試
我偏好 TDD（測試驅動開發）。
先撰寫測試，再進行實作。
專注於行為，而非實作細節。

### 架構
我偏好模組化、鬆散耦合的設計。
使用相依性注入（dependency injection）以提升可測試性。
關注點分離（Controllers, Services, Repositories）。

## 除錯偏好
- 使用帶有前綴的 `console.log`：`[DEBUG]`
- 包含上下文：函式名稱、相關變數
- 在可用時使用堆疊追蹤（stack traces）
- 務必在記錄中包含時間戳記

## 溝通
- 使用圖表解釋複雜概念
- 在解釋理論之前，先展示具體的範例
- 包含修改前後的程式碼片段
- 在最後總結重點

## 專案組織
我將專案組織如下：
```
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```

## 工具
- **IDE**：VS Code 搭配 vim 快捷鍵
- **Terminal**：Zsh 搭配 Oh-My-Zsh
- **Format**：Prettier (100 char line length)
- **Linter**：ESLint 搭配 airbnb config
- **Test Framework**：Jest 搭配 React Testing Library
~~~~

#### 範例 4：會話期間的記憶更新

**會話互動：**

```markdown
User: 記住我偏好在所有新組件中使用 React hooks 而不是 class components。

Claude: 我正在將此資訊加入您的記憶。這應該存放在哪個記憶檔案中？
        1. 專案記憶 (./CLAUDE.md)
        2. 個人記憶 (~/.claude/CLAUDE.md)

User: 專案記憶

Claude: ✅ 記憶已儲存！

已新增至 ./CLAUDE.md:
---

### 組件開發
- 使用帶有 React Hooks 的函式式組件
- 偏好使用 hooks 而非 class components
- 使用自定義 hooks 來處理可重複使用的邏輯
- 使用 useCallback 處理事件處理常式
- 使用 useMemo 處理耗時的運算
```

### Claude Web/Desktop 中的記憶

#### 記憶綜合時間軸

```mermaid
graph LR
    A["第 1 天：使用者<br/>對話"] -->|24 小時| B["第 2 天：記憶<br/>綜合"]
    B -->|自動| C["記憶已更新<br/>已摘要"]
    C -->|載入於| D["第 2 天-N 天：<br/>新對話"]
    D -->|新增至| E["記憶"]
    E -->|24 小時後| F["記憶已更新"]
```

**範例記憶摘要：**

```markdown

## Claude 對使用者的記憶

### 專業背景
- 資深全端開發人員，擁有 8 年經驗
- 專注於 TypeScript/Node.js 後端與 React 前端
- 活躍的開源貢獻者
- 對 AI 與機器學習感興趣

### 專案上下文
- 目前正在開發電商平台
- 技術棧：Node.js, PostgreSQL, React 18, Docker
- 與 5 名開發人員組成的團隊合作
- 使用 CI/CD 與藍綠部署

### 溝通偏好
- 偏好直接、簡潔的解釋
- 喜歡視覺化圖表與範例
- 欣賞程式碼片段
- 在註解中說明業務邏輯

### 目前目標
- 提升 API 效能
- 將測試覆蓋率提高至 90%
- 實作快取策略
- 撰寫架構文件

```

### 記憶功能比較

| 功能 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 自動綜合 | ✅ 每 24 小時 | ❌ 手動 |
| 跨專案 | ✅ 共享 | ❌ 專案特定 |
| 團隊存取 | ✅ 共享專案 | ✅ Git 追蹤 |
| 可搜尋性 | ✅ 內建 | ✅ 透過 `/memory` |
| 可編輯性 | ✅ 在對話中 | ✅ 直接編輯檔案 |
| 匯入/匯出 | ✅ 是 | ✅ 複製/貼上 |
| 持久性 | ✅ 24 小時以上 | ✅ 無限期 |

---

## MCP Protocol

### 概述

MCP (Model Context Protocol) 是 Claude 用於存取外部工具、API 與即時資料來源的標準化方式。與「記憶」不同，MCP 提供對變動資料的即時存取。

### MCP 架構

```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["External Service"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A
```

### MCP 生態系統

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|File I/O| G["Local Files"]
    C -->|API| H["GitHub Repos"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack Workspace"]
    F -->|Docs| K["Google Drive"]
```

### MCP 設定流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: 輸入 /mcp
    Claude->>Claude: 列出可用 MCP servers
    Claude->>User: 顯示選項
    User->>Claude: 選擇 GitHub MCP
    Claude->>Config: 更新設定檔
    Config->>Claude: 啟動連線
    Claude->>Service: 測試連線
    Service-->>Claude: 身分驗證成功
    Claude->>User: ✅ MCP 已連線！
```

### 可用 MCP Servers 表格

| MCP Server | 用途 | 常用工具 | 驗證方式 | 即時性 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 檔案操作 | read, write, delete | 作業系統權限 | ✅ 是 |
| **GitHub** | 儲存庫管理 | list_prs, create_issue, push | OAuth | ✅ 是 |

| **Slack** | 團隊溝通 | send_message, list_channels | Token | ✅ Yes |
| **Database** | SQL 查詢 | query, insert, update | Credentials | ✅ Yes |
| **Google Docs** | 文件存取 | read, write, share | OAuth | ✅ Yes |
| **Asana** | 專案管理 | create_task, update_status | API Key | ✅ Yes |
| **Stripe** | 付款數據 | list_charges, create_invoice | API Key | ✅ Yes |
| **Memory** | 持久化記憶 | store, retrieve, delete | Local | ❌ No |

### 實際範例

#### 範例 1：GitHub MCP 配置

**檔案：** `.mcp.json` (專案範圍) 或 `~/.claude.json` (使用者範圍)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**可用的 GitHub MCP 工具：**

~~~~markdown
# GitHub MCP 工具

## Pull Request 管理
- `list_prs` - 列出儲存庫中的所有 PR
- `get_pr` - 獲取 PR 詳細資訊（包含 diff）
- `create_pr` - 建立新的 PR
- `update_pr` - 更新 PR 描述/標題
- `merge_pr` - 將 PR 合併至主分支
- `review_pr` - 新增審查評論

範例請求：
```
/mcp__github__get_pr 456

# 回傳結果：
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

## Issue 管理
- `list_issues` - 列出所有 issue
- `get_issue` - 獲取 issue 詳細資訊
- `create_issue` - 建立新的 issue
- `close_issue` - 關閉 issue
- `add_comment` - 在 issue 中新增評論

## Repository Information
- `get_repo_info` - Repository 詳細資訊
- `list_files` - 檔案樹結構
- `get_file_content` - 讀取檔案內容
- `search_code` - 跨程式碼庫搜尋

## Commit Operations
- `list_commits` - Commit 歷史紀錄
- `get_commit` - 特定 commit 詳細資訊
- `create_commit` - 建立新 commit
~~~~

#### 範例 2：Database MCP 設定

**Configuration:**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**Example Usage:**

```markdown
User: Fetch all users with more than 10 orders

Claude: I'll query your database to find that information.

# Using MCP database tool:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# Results:
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```

#### 範例 3：Multi-MCP 工作流程

**Scenario: Daily Report Generation**

```markdown
# Daily Report Workflow using Multiple MCPs

## Setup
1. GitHub MCP - fetch PR metrics
2. Database MCP - query sales data
3. Slack MCP - post report
4. Filesystem MCP - save report

## Workflow

### Step 1: Fetch GitHub Data
/mcp__github__list_prs completed:true last:7days

Output:
- Total PRs: 42
- Average merge time: 2.3 hours
- Review turnaround: 1.1 hours

### Step 2: Query Database
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

Output:
- Sales: 247
- Revenue: $12,450

### Step 3: Generate Report
Combine data into HTML report

### Step 4: Save to Filesystem
Write report.html to /reports/

### Step 5: Post to Slack
Send summary to #daily-reports channel

Final Output:
✅ Report generated and posted
📊 47 PRs merged this week
💰 $12,450 in daily sales
```

#### 範例 4：Filesystem MCP 操作

**Configuration:**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**Available Operations:**

| Operation | Command | Purpose |
|-----------|---------|---------|
| List files | `ls ~/projects` | 顯示目錄內容 |
| Read file | `cat src/main.ts` | 讀取檔案內容 |
| Write file | `create docs/api.md` | 建立新檔案 |
| Edit file | `edit src/app.ts` | 修改檔案 |
| Search | `grep "async function"` | 在檔案中搜尋 |
| Delete | `rm old-file.js` | 刪除檔案 |

### MCP vs Memory: Decision Matrix

```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### Request/Response Pattern

```mermaid

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as Database

    App->>MCP: Request: "SELECT * FROM users WHERE id=1"
    MCP->>DB: Execute query
    DB-->>MCP: Result set
    MCP-->>App: Return parsed data
    App->>App: Process result
    App->>App: Continue task

    Note over MCP,DB: Real-time access<br/>No caching
```

---

## Agent 技能

### 概述

Agent 技能是可重複使用的、由模型觸發的能力，封裝為包含指令、腳本與資源的資料夾。Claude 會自動偵測並使用相關的技能。

### 技能架構

```mermaid
graph TB
    A["Skill Directory"]
    B["SKILL.md"]
    C["YAML Metadata"]
    D["Instructions"]
    E["Scripts"]
    F["Templates"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### 技能載入流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as System
    participant Skill as Skill

    User->>Claude: "Create Excel report"
    Claude->>System: Scan available skills
    System->>System: Load skill metadata
    Claude->>Claude: Match user request to skills
    Claude->>Skill: Load xlsx skill SKILL.md
    Skill-->>Claude: Return instructions + tools
    Claude->>Claude: Execute skill
    Claude->>User: Generate Excel file
```

### 技能類型與位置對照表

| Type | Location | Scope | Shared | Sync | Best For |
|------|----------|-------|--------|------|----------|
| Pre-built | Built-in | Global | All users | Auto | Document creation |
| Personal | `~/.claud/skills/` | Individual | No | Manual | Personal automation |
| Project | `.claude/skills/` | Team | Yes | Git | Team standards |
| Plugin | Via plugin install | Varies | Depends | Auto | Integrated features |

### 內建技能 (Pre-built Skills)

```mermaid
graph TB
    A["Pre-built Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["Create presentations"]
    B --> B2["Edit slides"]
    C --> C1["Create spreadsheets"]
    C --> C2["Analyze data"]
    D --> D1["Create documents"]
    D --> D2["Format text"]
    E --> E1["Generate PDFs"]
    E --> E2["Fill forms"]
```

### 內建組合技能 (Bundled Skills)

Claude Code 現在內建了 5 個開箱即用的組合技能：

| Skill | Command | Purpose |
|-------|---------|---------|
| **Simplify** | `/simplify` | 簡化複雜的程式碼或解釋 |
| **Batch** | `/batch` | 在多個檔案或項目中執行操作 |
| **Debug** | `/debug` | 系統化地進行問題除錯與根本原因分析 |
| **Loop** | `/loop` | 設定定時循環任務 |
| **Claude API** | `/claude-api` | 直接與 Anthropic API 進行互動 |

這些組合技能隨時可用，無需安裝或配置。

### 實際範例

#### 範例 1：自定義程式碼審查技能

**目錄結構：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
```

```yaml
---
name: Code Review Specialist
description: 包含安全性、效能與品質分析的全面性程式碼審查
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: 當使用者要求審查程式碼、分析程式碼品質或評估 pull requests 時使用
effort: high
shell: bash
---

# Code Review Skill

此技能提供全面的程式碼審查能力，重點在於：

1. **安全性分析**
   - 身分驗證/授權問題
   - 資料外洩風險
   - 注入漏洞
   - 加密弱點
   - 敏感資料紀錄

2. **效能審查**
   - 演算法效率 (Big O 分析)
   - 記憶體優化
   - 資料庫查詢優化
   - 快取機會
   - 並行問題

3. **程式碼品質**
   - SOLID 原則
   - 設計模式
   - 命名規範
   - 文件化
   - 測試覆蓋率

4. **可維護性**
   - 程式碼可讀性
   - 函式大小 (應 < 50 行)
   - 圈複雜度 (Cyclomatic complexity)
   - 相依性管理
   - 型別安全

## Review Template

針對每一段審查過的程式碼，請提供：

### Summary
- 整體品質評估 (1-5)
- 關鍵發現數量
- 建議優先處理的領域

### Critical Issues (if any)
- **Issue**: 清晰的描述
- **Location**: 檔案與行號
- **Impact**: 為什麼這很重要
- **Severity**: Critical/High/Medium
- **Fix**: 程式碼範例

### Findings by Category

#### Security (if issues found)
列出安全性漏洞及其範例

#### Performance (if issues found)
列出效能問題及其複雜度分析

#### Quality (if issues found)
列出程式碼品質問題及其重構建議

#### Maintainability (if issues found)
列出可維護性問題及其改進建議
```

## Python Script: analyze-metrics.py

```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """Analyze code for common metrics."""

    # Count functions
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # Count classes
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # Average line length
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # Estimate complexity
    complexity = len(re.findall(r'\b(if|elif|else|for|while|and|or)\b', code))

    return {
        'functions': functions,
        'classes': classes,
        'avg_line_length': avg_length,
        'complexity_score': complexity
    }

if __name__ == '__main__':
    with open(sys.argv[1], 'r') as f:
        code = f.read()
    metrics = analyze_code_metrics(code)
    for key, value in metrics.items():
        print(f"{key}: {value:.2f}")
```

## Python Script: compare-complexity.py

```python
#!/usr/bin/env python3
"""
Compare cyclomatic complexity of code before and after changes.
Helps identify if refactoring actually simplifies code structure.
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """Analyze code complexity metrics."""

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        Calculate cyclomatic complexity using McCabe's method.
        Count decision points: if, elif, else, for, while, except, and, or
        """
        complexity = 1  # Base complexity

        # Count decision points
        decision_patterns = [
            r'\bif\b',
            r'\belif\b',
            r'\bfor\b',
            r'\bwhile\b',
            r'\bexcept\b',
            r'\band\b(?!$)',
            r'\bor\b(?!$)'
        ]

        for pattern in decision_patterns:
            matches = re.findall(pattern, self.code)
            complexity += len(matches)

        return complexity

    def calculate_cognitive_complexity(self) -> int:
        """
        Calculate cognitive complexity - how hard is it to understand?
        Based on nesting depth and control flow.
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # Track nesting depth
            if re.search(r'^\s*(if|for|while|def|class|try)\b', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)\b', line):
                cognitive += nesting_depth

            # Reduce nesting when unindenting
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        Maintainability Index ranges from 0-100.
        > 85: Excellent
        > 65: Good
        > 50: Fair
        < 50: Poor
        """
```

```python
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """產生全面的複雜度報告。"""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """比較兩個程式碼版本之間的複雜度指標。"""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("CODE COMPLEXITY COMPARISON")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  Cyclomatic Complexity:    {before_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {before_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {before_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {before_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  Cyclomatic Complexity:    {after_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {after_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {after_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {after_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {after_metrics['avg_line_length']}")

    print("\nCHANGES:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  Cyclomatic Complexity:    {cyclomatic_change:+d}")
    print(f"  Cognitive Complexity:     {cognitive_change:+d}")
    print(f"  Maintainability Index:    {mi_change:+.2f}")
    print(f"  Lines of Code:            {loc_change:+d}")

    print("\nASSESSMENT:")
    if mi_change > 0:
        print("  ✅ Code is MORE maintainable")
```

```python
    elif mi_change < 0:
        print("  ⚠️  Code is LESS maintainable")
    else:
        print("  ➡️  Maintainability unchanged")

    if cyclomatic_change < 0:
        print("  ✅ Complexity DECREASED")
    elif cyclomatic_change > 0:
        print("  ⚠️  Complexity INCREASED")
    else:
        print("  ➡️  Complexity unchanged")

    print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```

## Template: review-checklist.md

```markdown
# Code Review Checklist

## Security Checklist
- [ ] 無寫死的憑證或金鑰
- [ ] 所有使用者輸入皆經過輸入驗證
- [ ] 防止 SQL 注入（使用參數化查詢）
- [ ] 對於會改變狀態的操作進行 CSRF 防護
- [ ] 使用適當的轉義機制防止 XSS
- [ ] 對受保護的端點進行身分驗證檢查
- [ ] 對資源進行授權檢查
- [ ] 安全的密碼雜湊（bcrypt, argon2）
- [ ] 日誌中不含敏感資料
- [ ] 強制使用 HTTPS

## Performance Checklist
- [ ] 無 N+1 查詢問題
- [ ] 適當使用索引
- [ ] 在有益處的地方實作快取
- [ ] 主執行緒上無阻塞操作
- [ ] 正確使用 Async/await
- [ ] 大型資料集已進行分頁處理
- [ ] 資料庫連線已使用連線池
- [ ] 正規表示式已優化
- [ ] 無不必要的物件建立
- [ ] 防止記憶體洩漏

## Quality Checklist
- [ ] 函式行數 < 50 行
- [ ] 變數命名清晰
- [ ] 無重複程式碼
- [ ] 適當的錯誤處理
- [ ] 註解說明的是「為什麼」而非「是什麼」
- [ ] 生產環境中無 console.logs
- [ ] 型別檢查 (TypeScript/JSDoc)
- [ ] 遵循 SOLID 原則
- [ ] 正確應用設計模式
- [ ] 自我文件化程式碼 (Self-documenting code)
```

## Testing Checklist
- [ ] 已撰寫單元測試
- [ ] 已涵蓋邊界情況
- [ ] 已測試錯誤情境
- [ ] 已包含整合測試
- [ ] 測試覆蓋率 > 80%
- [ ] 無不穩定的測試 (flaky tests)
- [ ] 已模擬外部依賴

```

## Template: finding-template.md

~~~~markdown
# Code Review 發現事項範本

當記錄程式碼審查過程中發現的每個問題時，請使用此範本。

---

## 問題：[標題]

### 嚴重程度
- [ ] Critical (阻礙部署)
- [ ] High (應在合併前修復)
- [ ] Medium (應儘快修復)
- [ ] Low (建議修復)

### 類別
- [ ] Security
- [ ] Performance
- [ ] Code Quality
- [ ] Maintainability
- [ ] Testing
- [ ] Design Pattern
- [ ] Documentation

### 位置
**檔案：** `src/components/UserCard.tsx`

**行號：** 45-52

**函式/方法：** `renderUserDetails()`

### 問題描述

**內容：** 描述問題是什麼。

**重要性：** 解釋其影響以及為什麼需要修復。

**目前行為：** 顯示有問題的程式碼或行為。

**預期行為：** 描述應該發生的行為。

### 程式碼範例

#### 目前（有問題的）

```typescript
// 顯示 N+1 查詢問題
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // 每個使用者進行一次查詢！
  renderUserPosts(posts);
});
```

#### 建議修復方式

```typescript
// 使用 JOIN 查詢進行優化
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### 影響分析

| 維度 | 影響 | 嚴重程度 |
|--------|--------|----------|
| Performance | 20 個使用者會產生 100+ 次查詢 | High |
| User Experience | 頁面載入緩慢 | High |
| Scalability | 規模擴大時會失效 | Critical |
| Maintainability | 難以除錯 | Medium |

### 相關問題

- `AdminUserList.tsx` 第 120 行有類似問題
- 相關 PR: #456
- 相關 issue: #789

### 額外資源

- [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_problem)
- [Database Join Documentation](https://docs.example.com/joins)

### 審查者筆記

- 這是此程式碼庫中的常見模式
- 考慮將此加入程式碼風格指南
- 可能值得建立一個輔助函式

### 作者回覆（用於回饋）

*由程式碼作者填寫：*

- [ ] 已在 commit 中實作修復：`abc123`
- [ ] 修復狀態：已完成 / 進行中 / 需要討論
- [ ] 問題或疑慮：(描述)

---
~~~~

## 發現統計（供審查者使用）

在審查多個發現時，請追蹤：

- **發現問題總數：** X
- **緊急 (Critical)：** X
- **高 (High)：** X
- **中 (Medium)：** X
- **低 (Low)：** X

**建議：** ✅ 核准 / ⚠️ 要求修改 / 🔄 需要討論

**整體程式碼品質：** 1-5 顆星
~~~~

## 使用範例

~~~~markdown
User: 審查這個 React 組件程式碼

Claude: 我將使用我的 Code Review Skill 進行全面的程式碼審查。

[載入 SKILL.md]
[根據檢查清單分析程式碼]
[執行 Python 指標腳本]
[套用範本]

## 程式碼審查結果

### 摘要
品質分數：3.5/5
關鍵發現：7 個項目
優先處理領域：效能優化、錯誤處理

### 緊急問題：1

**問題**：N+1 查詢問題
**位置**：UserComponent.tsx, lines 45-52
**影響**：針對 20 個使用者的列表產生了 100+ 次資料庫查詢
**嚴重程度**：高
**修復方法**：
```javascript
// Before: N+1 queries
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20+ queries

// After: Single query with JOIN
const users = fetchUsersWithPosts(); // 1 query
```

### 效能發現
- 大型列表缺少分頁功能
- 建議：對項目使用 React.memo()
- 資料庫查詢：可以透過索引進行優化

### 品質發現
- 第 20 行的函式長達 127 行（最大限制：50 行）
- 缺少錯誤邊界 (error boundary)
- Props 應該具備 TypeScript 型別
~~~~

#### 範例 2：品牌語氣技能 (Brand Voice Skill)

**目錄結構：**

```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```

**檔案：** `.claude/skills/brand-voice/SKILL.md`

```yaml
---
name: Brand Voice Consistency
description: Ensure all communication matches brand voice and tone guidelines
tags:
  - brand
  - writing
  - consistency
when_to_use: When creating marketing copy, customer communications, or public-facing content
---

# Brand Voice Skill
```

## 概觀
此技能確保所有溝通內容都能維持一致的品牌語氣、調性與訊息傳遞。

## 品牌識別

### 使命
協助團隊透過 AI 自動化其開發工作流程

### 價值觀
- **簡潔 (Simplicity)**：化繁為簡
- **可靠 (Reliability)**：穩如磐石的執行力
- **賦能 (Empowerment)**：啟發人類的創造力

### 語氣
- **友善且專業** - 平易近人但不失莊重
- **清晰且簡潔** - 避免術語，以簡單的方式解釋技術概念
- **自信** - 我們深知自己在做什麼
- **同理心** - 理解使用者的需求與痛點

## 寫作指南

### 應做事項 ✅
- 對讀者使用「你」
- 使用主動語態：例如「Claude 生成報告」而非「報告是由 Claude 生成的」
- 以價值主張作為開頭
- 使用具體的範例
- 將句子控制在 20 個單字以內
- 使用列表以增加清晰度
- 包含行動呼籲 (calls-to-action)

### 避免事項 ❌
- 不要使用企業術語
- 不要以居高臨下的態度或過度簡化內容
- 不要使用「我們相信」或「我們認為」
- 除了強調用途外，不要使用全大寫
- 不要堆砌大段文字
- 不要預設讀者具備技術知識

## 詞彙

### ✅ 偏好術語
- Claude (而非 "the Claude AI")
- Code generation (而非 "auto-coding")
- Agent (而非 "bot")
- Streamline (而非 "revolutionize")
- Integrate (而非 "synergize")

### ❌ 應避免的術語
- "Cutting-edge" (過度使用)
- "Game-changer" (含義模糊)
- "Leverage" (企業官腔)
- "Utilize" (請使用 "use")
- "Paradigm shift" (不明確)

```
## 範例

### ✅ 正面範例
"Claude 自動化您的程式碼審查流程。與其手動檢查每個 PR，Claude 會審查安全性、效能與品質——每週為您的團隊節省數小時的時間。"

為什麼有效：價值明確、具體效益、導向行動

### ❌ 反面範例
"Claude 利用尖端 AI 技術提供全面的軟體開發解決方案。"

為什麼無效：含義模糊、使用企業術語、缺乏具體價值
```

## 範本：Email

```
Subject: [清晰且以利益為導向的主旨]

Hi [Name],

[開場白：對他們的價值為何]

[正文：運作方式 / 他們將獲得什麼]

[具體範例或利益]

[Call to action：明確的下一步]

Best regards,
[Name]
```

## 範本：社群媒體

```
[Hook：在第一行吸引注意力]
[2-3 行：價值或有趣的資訊]
[Call to action：連結、問題或互動]
[Emoji：最多 1-2 個以增加視覺趣味]
```

## File: tone-examples.md
```
Exciting announcement:
"Save 8 hours per week on code reviews. Claude reviews your PRs automatically."

Empathetic support:
"We know deployments can be stressful. Claude handles testing so you don't have to worry."

Confident product feature:
"Claude doesn't just suggest code. It understands your architecture and maintains consistency."

Educational blog post:
"Let's explore how agents improve code review workflows. Here's what we learned..."
```

#### 範例 3：Documentation Generator Skill

**File:** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
name: API Documentation Generator
description: Generate comprehensive, accurate API documentation from source code
version: "1.0.0"
tags:
  - documentation
  - api
  - automation
when_to_use: When creating or updating API documentation
---

# API Documentation Generator Skill

## Generates

- OpenAPI/Swagger specifications
- API endpoint documentation
- SDK usage examples
- Integration guides
- Error code references
- Authentication guides

## Documentation Structure

### For Each Endpoint

```markdown

## GET /api/v1/users/:id

### 說明
簡要說明此端點的功能

### 參數

| 名稱 | 類型 | 必要 | 說明 |
|------|------|----------|-------------|
| id | string | 是 | 使用者 ID |

### 回應

**200 Success**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 Not Found**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### 範例

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```

## Python Script: generate-docs.py

```python
#!/usr/bin/env python3
import ast
import json
from typing import Dict, List

class APIDocExtractor(ast.NodeVisitor):
    """Extract API documentation from Python source code."""

    def __init__(self):
        self.endpoints = []

    def visit_FunctionDef(self, node):
        """Extract function documentation."""
        if node.name.startswith('get_') or node.name.startswith('post_'):
            doc = ast.get_docstring(node)
            endpoint = {
                'name': node.name,
                'docstring': doc,
                'params': [arg.arg for arg in node.args.args],
                'returns': self._extract_return_type(node)
            }
            self.endpoints.append(endpoint)
        self.generic_visit(node)

    def _extract_return_type(self, node):
        """Extract return type from function annotation."""
        if node.returns:
            return ast.unparse(node.returns)
        return "Any"

def generate_markdown_docs(endpoints: List[Dict]) -> str:
    """Generate markdown documentation from endpoints."""
    docs = "# API Documentation\n\n"

    for endpoint in endpoints:
        docs += f"## {endpoint['name']}\n\n"
        docs += f"{endpoint['docstring']}\n\n"
        docs += f"**Parameters**: {', '.join(endpoint['params'])}\n\n"
        docs += f"**Returns**: {endpoint['returns']}\n\n"
        docs += "---\n\n"

    return docs

if __name__ == '__main__':
    import sys
    with open(sys.argv[1], 'r') as f:
        tree = ast.parse(f.read())

    extractor = APIDocExtractor()
    extractor.visit(tree)

    markdown = generate_markdown_docs(extractor.endpoints)
    print(markdown)
```

### 技能發現與調用

```mermaid
graph TD
    A["User Request"] --> B["Claude Analyzes"]
    B -->|Scans| C["Available Skills"]
    C -->|Metadata check| D["Skill Description Match?"]
    D -->|Yes| E["Load SKILL.md"]
    D -->|No| F["Try next skill"]
    F -->|More skills?| D
    F -->|No more| G["Use general knowledge"]
    E --> H["Extract Instructions"]
```

```mermaid
    H --> I["執行技能"]
    I --> J["回傳結果"]
```

### 技能 vs 其他功能

```mermaid
graph TB
    A["擴充 Claude"]
    B["斜線命令"]
    C["子代理"]
    D["記憶"]
    E["MCP"]
    F["技能"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|使用者呼叫| G["快速捷徑"]
    C -->|自動委派| H["隔離的上下文"]
    D -->|持久化| I["跨會話上下文"]
    E -->|即時| J["外部數據存取"]
    F -->|自動呼叫| K["自主執行"]
```

---

## Claude Code 外掛

### 概述

Claude Code 外掛是綑綁在一起的自定義集合（包含斜線命令、子代理、MCP 伺服器與鉤子），只需透過單一指令即可完成安裝。它們代表了最高層級的擴充機制——將多種功能整合為凝聚且可共用的套件。

### 架構

```mermaid
graph TB
    A["外掛"]
    B["斜線命令"]
    C["子代理"]
    D["MCP 伺服器"]
    E["鉤子"]
    F["配置"]

    A -->|綑綁| B
    A -->|綑綁| C
    A -->|綑綁| D
    A -->|綑綁| E
    A -->|綑綁| F
```

### 外掛載入流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as 外掛市場
    participant Install as 安裝程序
    participant SlashCmds as 斜線命令
    participant Subagents
    participant MCPServers as MCP 伺服器
    participant Hooks
    participant Tools as 已配置的工具

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: 下載外掛清單
    Plugin-->>Claude: 回傳外掛定義
    Claude->>Install: 解壓縮組件
    Install->>SlashCmds: 配置
    Install->>Subagents: 配置
    Install->>MCPServers: 配置
    Install->>Hooks: 配置
    SlashCmds-->>Tools: 可供使用
    Subagents-->>Tools: 可供使用
    MCPServers-->>Tools: 可供使用
    Hooks-->>Tools: 可供使用
    Tools-->>Claude: 外掛安裝成功 ✅
```

### 外掛類型與分發

| 類型 | 範圍 | 共用對象 | 權限 | 範例 |
|------|-------|--------|-----------|----------|
| 官方 | 全域 | 所有使用者 | Anthropic | PR Review, 安全指南 |
| 社群 | 公開 | 所有使用者 | 社群 | DevOps, 資料科學 |
| 組織 | 內部 | 團隊成員 | 公司 | 內部標準、工具 |
| 個人 | 個人 | 單一使用者 | 開發者 | 自定義工作流程 |

### 外掛定義結構

```yaml
---
name: plugin-name
version: "1.0.0"
description: "此外掛的功能說明"
author: "您的姓名"
license: MIT

# 外掛元數據
tags:
  - category
  - use-case

# 需求
requires:
  - claude-code: ">=1.0.0"

# 綑綁的組件
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# 配置
config:
  auto_load: true
  enabled_by_default: true
---
```

### 外掛結構

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
```

```
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 實際範例

#### 範例 1：PR Review 外掛

**檔案：** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "Complete PR review workflow with security, testing, and docs",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**檔案：** `commands/review-pr.md`

```markdown
---
name: Review PR
description: Start comprehensive PR review with security and testing checks
---

# PR Review

此命令會啟動完整的 pull request 審查，包含：

1. 安全性分析
2. 測試覆蓋率驗證
3. 文件更新
4. 程式碼品質檢查
5. 效能影響評估
```

**檔案：** `agents/security-reviewer.md`

```yaml
---
name: security-reviewer
description: Security-focused code review
tools: read, grep, diff
---

# Security Reviewer

專精於發現安全性漏洞：
- 身分驗證/授權問題
- 資料外洩
- 注入攻擊
- 安全配置
```

**安裝：**

```bash
/plugin install pr-review

# 結果：
# ✅ 3 個斜線命令已安裝
# ✅ 3 個子代理已配置
# ✅ 2 個 MCP servers 已連接
# ✅ 4 個鉤子已註冊
# ✅ 已準備就緒！
```

#### 範例 2：DevOps 外掛

**組件：**

```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```

#### 範例 3：文件外掛

**內含組件：**

```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```

### 外掛市場

```mermaid
graph TB
    A["外掛市場"]
    B["官方<br/>Anthropic"]
    C["社群<br/>市場"]
    D["企業<br/>註冊表"]

    A --> B
    A --> C
    A --> D

    B -->|分類| B1["開發"]
    B -->|分類| B2["DevOps"]
    B -->|分類| B3["文件"]

    C -->|搜尋| C1["DevOps 自動化"]
    C -->|搜尋| C2["行動裝置開發"]
```

```mermaid
    C -->|Search| C3["Data Science"]

    D -->|Internal| D1["Company Standards"]
    D -->|Internal| D2["Legacy Systems"]
    D -->|Internal| D3["Compliance"]
```

### 外掛安裝與生命週期

```mermaid
graph LR
    A["Discover"] -->|Browse| B["Marketplace"]
    B -->|Select| C["Plugin Page"]
    C -->|View| D["Components"]
    D -->|Install| E["/plugin install"]
    E -->|Extract| F["Configure"]
    F -->|Activate| G["Use"]
    G -->|Check| H["Update"]
    H -->|Available| G
    G -->|Done| I["Disable"]
    I -->|Later| J["Enable"]
    J -->|Back| G
```

### 外掛功能比較

| 功能 | 斜線命令 | 技能 | 子代理 | 外掛 |
|---------|---------------|-------|----------|--------|
| **安裝** | 手動複製 | 手動複製 | 手komfig | 單一指令 |
| **設定時間** | 5 分鐘 | 10 分鐘 | 15 分鐘 | 2 分鐘 |
| **打包** | 單一檔案 | 單一檔案 | 單一檔案 | 多個檔案 |
| **版本控制** | 手動 | 手動 | 手動 | 自動 |
| **團隊共享** | 複製檔案 | 複製檔案 | 複製檔案 | 安裝 ID |
| **更新** | 手動 | 手動 | 手動 | 自動可用 |
| **依賴關係** | 無 | 無 | 無 | 可能包含 |
| **Marketplace** | 否 | 否 | 否 | 是 |
| **分發** | 儲存庫 | 儲存庫 | 儲存庫 | Marketplace |

### 外掛使用情境

| 使用情境 | 建議 | 原因 |
|----------|-----------------|-----|
| **團隊入職培訓** | ✅ 使用外掛 | 即時設定，包含所有配置 |
| **框架設定** | ✅ 使用外掛 | 打包特定框架的指令 |
| **企業標準** | ✅ 使用外掛 | 中央分發，版本控制 |
| **快速任務自動化** | ❌ 使用指令 | 過於複雜 |
| **單一領域專業知識** | ❌ 使用技能 | 太過笨重，改用技能 |
| **專業分析** | ❌ 使用子代理 | 手動建立或使用技能 |
| **即時數據存取** | ❌ 使用 MCP | 獨立運行，不要打包 |

### 何時建立外掛

```mermaid
graph TD
    A["我應該建立外掛嗎？"]
    A -->|需要多個組件| B{"多個指令<br/>或子代理<br/>或 MCPs？"}
    B -->|是| C["✅ 建立外掛"]
    B -->|否| D["使用個別功能"]
    A -->|團隊工作流程| E{"與團隊<br/>共享？"}
    E -->|是| C
    E -->|否| F["保留為本地設定"]
    A -->|複雜設定| G{"需要自動<br/>配置？"}
    G -->|是| C
    G -->|否| D
```

### 發布外掛

**發布步驟：**

1. 建立包含所有組件的外掛結構
2. 撰寫 `.claude-plugin/plugin.json` 清單
3. 建立包含文件的 `README.md`
4. 使用 `/plugin install ./my-plugin` 進行本地測試
5. 提交至外掛 Marketplace
6. 經過審核與批准
7. 在 Marketplace 上發布
8. 使用者可以透過單一指令進行安裝

**提交範例：**

~~~~markdown
# PR Review Plugin

## 描述
完整的 PR 審查工作流程，包含安全性、測試與文件檢查。

## 內容包含
- 3 個用於不同審查類型的斜線命令
- 3 個專業的子代理
- GitHub 與 CodeQL MCP 整合
- 自動化安全性掃描鉤子

## 安裝
```bash
/plugin install pr-review
```

## 功能
✅ 安全性分析
✅ 測試覆蓋率檢查
✅ 文件驗證
✅ 程式碼品質評估
✅ 效能影響分析

## 使用方式
```bash
/review-pr
/check-security
/check-tests
```

## 需求
- Claude Code 1.0+
- GitHub 存取權限
- CodeQL (選填)
~~~~

### 外掛 vs 手動配置

**手動設定 (2 小時以上)：**
- 一個一個安裝斜線命令
- 個別建立子代理
- 分開配置 MCP
- 手動設定鉤子
- 記錄所有內容
- 與團隊分享（希望他們能正確配置）

**使用外掛 (2 分鐘)：**
```bash
/plugin install pr-review
# ✅ 所有內容皆已安裝並配置完成
# ✅ 可立即使用
# ✅ 團隊可以重現完全相同的設定
```

---

## 比較與整合

### 功能比較矩陣

| 功能 | 呼叫方式 | 持久性 | 範圍 | 使用情境 |
|---------|-----------|------------|-------|----------|
| **斜線命令** | 手動 (`/cmd`) | 僅限會話 | 單一命令 | 快速捷徑 |
| **子代理** | 自動委派 | 隔離的上下文 | 專業任務 | 任務分配 |
| **記憶** | 自動載入 | 跨會話 | 使用者/團隊上下文 | 長期學習 |
| **MCP 協定** | 自動查詢 | 即時外部 | 即時數據存取 | 動態資訊 |
| **技能** | 自動呼叫 | 基於檔案系統 | 可重複使用的專業知識 | 自動化工作流程 |

### 互動時間軸

```mermaid
graph LR
    A["Session Start"] -->|Load| B["Memory (CLAUDE.md)"]
    B -->|Discover| C["Available Skills"]
    C -->|Register| D["Slash Commands"]
    D -->|Connect| E["MCP Servers"]
    E -->|Ready| F["User Interaction"]

    F -->|Type /cmd| G["Slash Command"]
    F -->|Request| H["Skill Auto-Invoke"]
    F -->|Query| I["MCP Data"]
    F -->|Complex task| J["Delegate to Subagent"]

    G -->|Uses| B
    H -->|Uses| B
    I -->|Uses| B
    J -->|Uses| B
```

### 實際整合範例：客戶支援自動化

#### 架構

```mermaid
graph TB
    User["Customer Email"] -->|Receives| Router["Support Router"]

    Router -->|Analyze| Memory["Memory<br/>Customer history"]
    Router -->|Lookup| MCP1["MCP: Customer DB<br/>Previous tickets"]
    Router -->|Check| MCP2["MCP: Slack<br/>Team status"]

    Router -->|Route Complex| Sub1["Subagent: Tech Support<br/>Context: Technical issues"]
    Router -->|Route Simple| Sub2["Subagent: Billing<br/>Context: Payment issues"]
    Router -->|Route Urgent| Sub3["Subagent: Escalation<br/>Context: Priority handling"]

    Sub1 -->|Format| Skill1["Skill: Response Generator<br/>Brand voice maintained"]
    Sub2 -->|Format| Skill2["Skill: Response Generator"]
    Sub3 -->|Format| Skill3["Skill: Response Generator"]

    Skill1 -->|Generate| Output["Formatted Response"]
```

```mermaid
    Skill2 -->|Generate| Output
    Skill3 -->|Generate| Output

    Output -->|Post| MCP3["MCP: Slack<br/>Notify team"]
    Output -->|Send| Reply["Customer Reply"]
```

#### Request Flow

```markdown
## 客戶支援請求工作流程

### 1. 入站郵件
「當我嘗試上傳檔案時，一直出現 500 錯誤。這阻礙了我的工作流程！」

### 2. 記憶查詢
- 載入包含支援標準的 CLAUDE.md
- 檢查客戶歷史紀錄：VIP 客戶，本月第 3 次事件

### 3. MCP 查詢
- GitHub MCP：列出開啟中的問題（找到相關的 bug 報告）
- Database MCP：檢查系統狀態（未回報停機）
- Slack MCP：檢查工程團隊是否已知情

### 4. 技能偵測與載入
- 請求符合「技術支援」技能
- 從 Skill 載入支援回應範本

### 5. 子代理委派
- 路由至技術支援子代理
- 提供上下文：客戶歷史紀錄、錯誤細節、已知問題
- 子代理擁有完整存取權限：read、bash、grep 工具

### 6. 子代理處理
技術支援子代理：
- 在程式碼庫中搜尋檔案上傳中的 500 錯誤
- 在 commit 8f4a2c 中發現最近的變更
- 建立暫時解決方案文件

### 7. 技能執行
回應產生器技能：
- 使用品牌語調指南
- 以同理心格式化回應
- 包含暫時解決方案步驟
- 連結至相關文件

### 8. MCP 輸出
- 在 #support Slack 頻道發布更新
- 標記工程團隊
- 在 Jira MCP 中更新工單

### 9. 回應
客戶收到：
- 同理心的確認
- 原因說明
- 即時的暫時解決方案
- 永久修復的時間表
- 相關問題的連結
```

### Complete Feature Orchestration

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: Request: "Build auth system"
    Claude->>Memory: Load project standards
    Memory-->>Claude: Auth standards, team practices
    Claude->>MCP: Query GitHub for similar implementations
    MCP-->>Claude: Code examples, best practices
    Claude->>Skills: Detect matching Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: Delegate implementation
    SubAgent->>SubAgent: Build feature
    Claude->>Skills: Apply Security Review Skill
    Skills-->>Claude: Security checklist results
    Claude->>SubAgent: Delegate testing
    SubAgent-->>Claude: Test results
    Claude->>User: Complete system delivered
```

### When to Use Each Feature

```mermaid
graph TD
    A["New Task"] --> B{Type of Task?}

    B -->|Repeated workflow| C["Slash Command"]
    B -->|Need real-time data| D["MCP Protocol"]
    B -->|Remember for next time| E["Memory"]
    B -->|Specialized subtask| F["Subagent"]
    B -->|Domain-specific work| G["Skill"]

    C --> C1["✅ Team shortcut"]
    D --> D1["✅ Live API access"]
    E --> E1["✅ Persistent context"]
    F --> F1["✅ Parallel execution"]
    G --> G1["✅ Auto-invoked expertise"]
```

### 選擇決策樹

```mermaid
graph TD
    Start["需要擴充 Claude？"]

    Start -->|快速重複性任務| A{"手動或自動？"}
    A -->|手動| B["Slash Command"]
    A -->|自動| C["Skill"]

    Start -->|需要外部數據| D{"即時性？"}
    D -->|是| E["MCP Protocol"]
    D -->|否/跨會話| F["Memory"]

    Start -->|複雜專案| G{"多重角色？"}
    G -->|是| H["Subagents"]
    G -->|否| I["Skills + Memory"]

    Start -->|長期上下文| J["Memory"]
    Start -->|團隊工作流程| K["Slash Command +<br/>Memory"]
    Start -->|完全自動化| L["Skills +<br/>Subagents +<br/>MCP"]
```

---

## 摘要表格

| 維度 | Slash Commands | Subagents | Memory | MCP | Skills | Plugins |
|--------|---|---|---|---|---|---|
| **設定難度** | 簡單 | 中等 | 簡單 | 中等 | 中等 | 簡單 |
| **學習曲線** | 低 | 中等 | 低 | 中等 | 中等 | 低 |
| **團隊效益** | 高 | 高 | 中等 | 高 | 高 | 極高 |
| **自動化程度** | 低 | 高 | 中等 | 高 | 高 | 極高 |
| **上下文管理** | 單一會話 | 隔離 | 持續性 | 即時 | 持續性 | 所有功能 |
| **維護負擔** | 低 | 中等 | 低 | 中等 | 中等 | 低 |
| **擴充性** | 良好 | 極佳 | 良好 | 極佳 | 極佳 | 極佳 |
| **可分享性** | 普通 | 普通 | 良好 | 良好 | 良好 | 極佳 |
| **版本控制** | 手動 | 手動 | 手動 | 手動 | 手動 | 自動 |
| **安裝方式** | 手動複製 | 手動配置 | N/A | 手動配置 | 手動複製 | 單一指令 |

---

## 快速入門指南

### 第一週：從簡單開始
- 為常見任務建立 2-3 個斜線命令
- 在設定中啟用 Memory
- 在 CLAUDE.md 中記錄團隊標準

### 第二週：加入即時存取
- 設定 1 個 MCP（GitHub 或資料庫）
- 使用 `/mcp` 進行配置
- 在你的工作流程中查詢即時數據

### 第三週：分配工作
- 為特定角色建立第一個 Subagent
- 使用 `/agents` 命令
- 使用簡單任務測試委派功能

### 第四週：全面自動化
- 建立第一個用於重複自動化的 Skill
- 使用 Skill 市集或建立自定義內容
- 結合所有功能以實現完整工作流程

### 持續進行
- 每月審查並更新 Memory
- 隨著模式出現，增加新的 Skills
- 優化 MCP 查詢
- 精煉 Subagent 提示詞

---

## Hooks

### 概述

Hooks 是事件驅動的 shell 命令，會根據 Claude Code 事件自動執行。它們可以在無需人工干預的情況下，實現自動化、驗證與自定義工作流程。

### Hook 事件

Claude Code 在四種 hook 類型（command、http、prompt、agent）中支援 **25 個 hook 事件**：

| Hook 事件 | 觸發條件 | 使用案例 |
|------------|---------|-----------|
| **SessionStart** | 會話開始/恢復/清除/壓縮 | 環境設定、初始化 |
| **InstructionsLoaded** | CLAUDE.md 或規則檔案載入 | 驗證、轉換、增強 |
| **UserPromptSubmit** | 使用者提交提示詞 | 輸入驗證、提示詞過濾 |
| **PreToolUse** | 在任何工具執行前 | 驗證、審核閘門、記錄 |
| **PermissionRequest** | 顯示權限對話框時 | 自動核准/拒絕流程 |
| **PostToolUse** | 工具執行成功後 | 自動格式化、通知、清理 |
| **PostToolUseFailure** | 工具執行失敗時 | 錯誤處理、記錄 |
| **Notification** | 發送通知時 | 警示、外部整合 |
| **SubagentStart** | Subagent 被啟動時 | 上下文注入、初始化 |
| **SubagentStop** | Subagent 完成時 | 結果驗證、記錄 |
| **Stop** | Claude 完成回應時 | 摘要生成、清理任務 |
| **StopFailure** | API 錯誤導致回合結束時 | 錯誤恢復、記錄 |
| **TeammateIdle** | Agent 團隊成員閒置時 | 工作分配、協調 |
| **TaskCompleted** | 任務被標記為完成時 | 任務後處理 |
| **TaskCreated** | 透過 TaskCreate 建立任務時 | 任務追蹤、記錄 |
| **ConfigChange** | 設定檔變更時 | 驗證、傳播 |
| **CwdChanged** | 工作目錄變更時 | 特定目錄的設定 |
| **FileChanged** | 被監控的檔案變更時 | 檔案監控、重新構建觸發 |
| **PreCompact** | 在上下文壓縮前 | 狀態保存 |
| **PostCompact** | 壓縮完成後 | 壓縮後動作 |
| **WorktreeCreate** | 正在建立 Worktree 時 | 環境設定、依賴安裝 |
| **WorktreeRemove** | 正在移除 Worktree 時 | 清理、資源釋放 |
| **Elicitation** | MCP 伺服器請求使用者輸入時 | 輸入驗證 |
| **ElicitationResult** | 使用者回應詢問時 | 回應處理 |
| **SessionEnd** | 會話終止時 | 清理、最終記錄 |

### 常用鉤子 (Hooks)

鉤子可以在 `~/.claude/settings.json`（使用者層級）或 `.claude/settings.json`（專案層級）中進行配置：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### 鉤子環境變數

- `$CLAUDE_FILE_PATH` - 正在編輯/寫入的檔案路徑
- `$CLAUDE_TOOL_NAME` - 正在使用的工具名稱
- `$CLAUDE_SESSION_ID` - 當前會話識別碼
- `$CLAUDE_PROJECT_DIR` - 專案目錄路徑

### 最佳實踐

✅ **應該：**
- 保持鉤子執行快速（< 1 秒）
- 將鉤子用於驗證與自動化
- 優雅地處理錯誤
- 使用絕對路徑

❌ **不應該：**
- 使鉤子具備互動性
- 將鉤子用於長時間運行的任務
- 將憑證寫死在程式碼中

**參閱**：[06-hooks/](06-hooks/) 以獲取詳細範例

---

## 檢查點 (Checkpoints) 與回溯 (Rewind)

### 概述

檢查點允許您儲存對話狀態並回溯到先前的點，從而實現安全的實驗以及對多種方法的探索。

### 核心概念

| 概念 | 描述 |
|---------|-------------|
| **Checkpoint** | 對話狀態的快照，包含訊息、檔案與上下文 |
| **Rewind** | 回到先前的檢查點，捨棄隨後的變更 |
| **Branch Point** | 用於探索多種方法的檢查點 |

### 存取檢查點

每次使用者輸入提示詞時都會自動建立檢查點。若要進行回溯：

```bash
# 按兩次 Esc 開啟檢查點瀏覽器
Esc + Esc

# 或使用 /rewind 斜線命令
/rewind
```

當您選擇一個檢查點時，會有五個選項可供選擇：
1. **Restore code and conversation** -- 將程式碼與對話同時還原到該點
2. **Restore conversation** -- 回溯訊息，保留目前的程式碼
3. **Restore code** -- 還原檔案，保留對話
4. **Summarize from here** -- 將對話壓縮成摘要
5. **Never mind** -- 取消

### 使用情境

| 情境 | 工作流程 |
|----------|----------|
| **探索方法** | 儲存 → 嘗試 A → 儲存 → 回溯 → 嘗試 B → 比較 |
| **安全重構** | 儲存 → 重構 → 測試 → 若失敗：回溯 |
| **A/B 測試** | 儲存 → 設計 A → 儲存 → 回溯 → 設計 B → 比較 |
| **錯誤恢復** | 發現問題 → 回溯到上一個良好的狀態 |

### 配置

```json
{
  "autoCheckpoint": true
}
```

**參閱**：[08-checkpoints/](08-checkpoints/) 以獲取詳細範例

---

## 進階功能

### Planning Mode

在編寫程式碼之前建立詳細的實作計畫。

**啟動方式：**
```bash
/plan Implement user authentication system
```

**優點：**
- 包含時間估計的清晰路線圖
- 風險評估
- 系統化的任務拆解
- 提供審查與修改的機會

### Extended Thinking

針對複雜問題進行深度推理。

**啟動方式：**
- 在會話期間使用 `Alt+T`（macOS 為 `Option+T`）進行切換
- 設定 `MAX_THINKING_TOKENS` 環境變數以進行程式化控制

```bash
# 透過環境變數啟用 extended thinking
export MAX_THINKING_TOKENS=50000
claude -p "Should we use microservices or monolith?"
```

**優點：**
- 對權衡取捨進行徹底分析
- 更佳的架構決策
- 考慮邊緣案例
- 系統化的評估

### Background Tasks

執行長時間運行的操作而不阻塞對話。

**用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 檢查進度
/task show bg-1234   # 查看輸出
/task cancel bg-1234 # 取消任務
```

### Permission Modes

控制 Claude 的操作權限。

| 模式 | 描述 | 使用情境 |
|------|-------------|----------|
| **default** | 標準權限，敏感操作會發出提示 | 一般開發 |
| **acceptEdits** | 自動接受檔案編輯，無需確認 | 受信任的編輯工作流程 |
| **plan** | 僅限分析與規劃，不進行檔案修改 | 程式碼審查、架構規劃 |
| **auto** | 自動核准安全操作，僅針對風險操作發出提示 | 在安全性與自主性之間取得平衡 |
| **dontAsk** | 執行所有操作，不發出確認提示 | 資深使用者、自動化 |
| **bypassPermissions** | 完全不受限的存取權限，無安全性檢查 | CI/CD 流水線、受信任的腳本 |

**用法：**
```bash
claude --permission-mode plan          # 唯讀分析
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動核准安全操作
claude --permission-mode dontAsk       # 無確認提示
```

### Headless Mode (Print Mode)

使用 `-p` (print) 旗標在沒有互動式輸入的情況下執行 Claude Code，適用於自動化與 CI/CD。

**用法：**
```bash
# 執行特定任務
claude -p "Run all tests"

# 將輸入透過 pipe 傳送進行分析
cat error.log | claude -p "explain this error"

# CI/CD 整合 (GitHub Actions)
- name: AI Code Review
  run: claude -p "Review PR changes and report issues"

# 用於腳本的 JSON 輸出
claude -p --output-format json "list all functions in src/"
```

### Scheduled Tasks

使用 `/loop` 命令按重複排程執行任務。

**用法：**
```bash
/loop every 30m "Run tests and report failures"
/loop every 2h "Check for dependency updates"
/loop every 1d "Generate daily summary of code changes"
```

排程任務會在背景執行，並在完成時回報結果。它們對於持續監控、定期檢查與自動化維護工作流程非常有用。

### Chrome Integration

Claude Code 可以與 Chrome 瀏覽器整合以進行網頁自動化任務。這讓您能夠在開發工作流程中直接執行導覽網頁、填寫表單、擷取螢幕截圖以及從網站提取數據等功能。

### Session Management

管理多個工作會話。

**Commands:**
```bash
/resume                # 恢復先前的對話
/rename "Feature"      # 為目前的會話命名
/fork                  # 分叉（Fork）成一個新的會話
claude -c              # 繼續最近一次的對話
claude -r "Feature"    # 透過名稱/ID 恢復會話
```

### Interactive Features

**Keyboard Shortcuts:**
- `Ctrl + R` - 搜尋指令歷史紀錄
- `Tab` - 自動補完
- `↑ / ↓` - 指令歷史紀錄
- `Ctrl + L` - 清除螢幕

**Multi-line Input:**
```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

### Configuration

完整的配置範例：

```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```

**請參閱**：[09-advanced-features/](09-advanced-features/) 以獲取完整指南

---

## Resources

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [Anthropic Documentation](https://docs.anthropic.com)
- [MCP GitHub Servers](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---

*最後更新日期：2026 年 4 月 16 日*
*適用於 Claude Haiku 4.5, Sonnet 4.6, 與 Opus 4.7*
*現在包含：Hooks、Checkpoints、Planning Mode、Extended Thinking、Background Tasks、Permission Modes (6 modes)、Headless Mode、Session Management、Auto Memory、Agent Teams、Scheduled Tasks、Chrome Integration、Channels、Voice Dictation 以及 Bundled Skills*

---
**Last Updated**: April 16, 2026
**Claude Code Version**: 2.1.112
**Sources**:
- https://docs.anthropic.com/en/docs/claude-code
- https://www.anthropic.com/news/claude-opus-4-7
- https://support.claude.com/en/articles/12138966-release-notes
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
