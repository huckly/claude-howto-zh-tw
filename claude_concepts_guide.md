# Complete Guide to Claude Concepts

A comprehensive reference guide covering Slash Commands, Subagents, Memory, MCP Protocol, and Agent Skills with tables, diagrams, and practical examples.

---

## Table of Contents

1. [Slash Commands](#slash-commands)
2. [Subagents](#subagents)
3. [Memory](#memory)
4. [MCP Protocol](#mcp-protocol)
5. [Agent Skills](#agent-skills)
6. [Plugins](#claude-code-plugins)
7. [Hooks](#hooks)
8. [Checkpoints and Rewind](#checkpoints-and-rewind)
9. [Advanced Features](#advanced-features)
10. [Comparison & Integration](#comparison--integration)

---

## Slash Commands

### Overview

斜線命令是用戶啟動的快捷方式，儲存為 Markdown 檔案，Claude Code 可以執行。它們能讓團隊標準化常用的提示詞和工作流程。

### Architecture

```mermaid
graph TD
    A["User Input: /command-name"] -->|Triggers| B["Search .claude/commands/"]
    B -->|Finds| C["command-name.md"]
    C -->|Loads| D["Markdown Content"]
    D -->|Executes| E["Claude Processes Prompt"]
    E -->|Returns| F["Result in Context"]
```

### File Structure

```mermaid
graph LR
    A["Project Root"] -->|contains| B[".claude/commands/"]
    B -->|contains| C["optimize.md"]
    B -->|contains| D["test.md"]
    B -->|contains| E["docs/"]
    E -->|contains| F["generate-api-docs.md"]
    E -->|contains| G["generate-readme.md"]
```

### Command Organization Table

| Location | Scope | Availability | Use Case | Git Tracked |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | Project-specific | Team members | Team workflows, shared standards | ✅ Yes |
| `~/.claude/commands/` | Personal | Individual user | Personal shortcuts across projects | ❌ No |
| Subdirectories | Namespaced | Based on parent | Organize by category | ✅ Yes |

### Features & Capabilities

| Feature | Example | Supported |
|---------|---------|-----------|
| Shell script execution | `bash scripts/deploy.sh` | ✅ Yes |
| File references | `@path/to/file.js` | ✅ Yes |
| Bash integration | `$(git log --oneline)` | ✅ Yes |
| Arguments | `/pr --verbose` | ✅ Yes |
| MCP commands | `/mcp__github__list_prs` | ✅ Yes |

### Practical Examples

#### Example 1: Code Optimization Command

**File:** `.claude/commands/optimize.md`

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

### 使用方法：
```bash
# 用戶輸入 Claude Code
/optimize

# Claude 載入提示詞並等待程式碼輸入
```

#### 範例 2：Pull Request 助手命令

**檔案：** `.claude/commands/pr.md`

```markdown
---
name: 準備 Pull Request
description: 清理程式碼、階段變更、並準備一個 Pull Request
tags: git, workflow
---

# Pull Request 準備清單

在建立 PR 之前，執行以下步驟：

1. 執行 linting: `prettier --write .`
2. 執行測試: `npm test`
3. 檢視 git diff: `git diff HEAD`
4. 階段變更: `git add .`
5. 建立符合 conventional commits 的 commit 訊息：
   - `fix:` 用於修正錯誤
   - `feat:` 用於新增功能
   - `docs:` 用於文件
   - `refactor:` 用於程式碼重構
   - `test:` 用於新增測試
   - `chore:` 用於維護

6. 產生 PR 摘要，包含：
   - 變更了什麼
   - 為什麼變更
   - 執行了哪些測試
   - 潛在影響
```

**使用方法：**
```bash
/pr

# Claude 執行清單並準備 PR
```

#### 範例 3：分層文件產生器

**檔案：** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: 產生 API 文件
description: 從原始程式碼產生全面的 API 文件
tags: documentation, api
---

# API 文件產生器

透過以下方式產生 API 文件：

1. 掃描 `/src/api/` 中的所有檔案
2. 提取函式簽章和 JSDoc 註解
3. 依端點/模組組織
4. 產生 Markdown 文件
5. 包含請求/回應模式
6. 新增錯誤文件

輸出格式：
- Markdown 檔案位於 `/docs/api.md`
- 包含所有端點的 curl 範例
- 新增 TypeScript 類型
```

### 命令生命週期圖

```mermaid
sequenceDiagram
    participant 用戶
    participant Claude as Claude Code
    participant FS as 檔案系統
    participant CLI as Shell/Bash

    用戶->>Claude: 輸入 /optimize
    Claude->>FS: 搜尋 .claude/commands/
    FS-->>Claude: 傳回 optimize.md
    Claude->>Claude: 載入 Markdown 內容
    Claude->>用戶: 顯示提示詞上下文
    用戶->>Claude: 提供程式碼進行分析
    Claude->>CLI: (可能執行腳本)
    CLI-->>Claude: 結果
    Claude->>用戶: 傳回分析
```

### 最佳實踐

| ✅ 做到 | ❌ 避免 |
|------|---------|
| 使用清晰、以行動為導向的名稱 | 為一次性任務建立命令 |
| 在描述中記錄觸發詞 | 在命令中建立複雜邏輯 |
| 保持命令專注於單一任務 | 建立重複的命令 |
| 版本控制專案命令 | 硬編碼敏感資訊 |
| 在子目錄中組織 | 建立冗長命令清單 |
| 使用簡單、易讀的提示詞 | 使用縮寫或神秘的措辭 |

---

## 子代理

### 概述

子代理是具有獨立上下文視窗和自訂系統提示詞的專業 AI 助理。它們能夠委派任務執行，同時保持關注點的清晰分離。

### 架構圖

```mermaid
graph TB
    User["👤 用戶"]
    Main["🎯 主代理<br/>(協調器)"]
    Reviewer["🔍 程式碼審查員<br/>子代理"]
    Tester["✅ 測試工程師<br/>子代理"]
    Docs["📝 文件<br/>子代理"]

    User -->|提出要求| Main
    Main -->|委派| Reviewer
    Main -->|委派| Tester
    Main -->|委派| Docs
    Reviewer -->|返回結果| Main
    Tester -->|返回結果| Main
    Docs -->|返回結果| Main
    Main -->|合成| User
```

### 子代理生命週期

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as 主代理
    participant CodeReviewer as 程式碼審查員<br/>子代理
    participant Context as 獨立<br/>上下文視窗

    User->>MainAgent: "建立新的驗證功能"
    MainAgent->>MainAgent: 分析任務
    MainAgent->>CodeReviewer: "審查此程式碼"
    CodeReviewer->>Context: 初始化乾淨的上下文
    Context->>CodeReviewer: 載入審查員指示
    CodeReviewer->>CodeReviewer: 執行審查
    CodeReviewer-->>MainAgent: 返回結果
    MainAgent->>MainAgent: 納入結果
    MainAgent-->>User: 提供合成結果
```

### 子代理設定表格

| 設定 | 類型 | 目的 | 範例 |
|---------------|------|---------|---------|
| `name` | 字串 | 代理識別碼 | `code-reviewer` |
| `description` | 字串 | 目的和觸發詞 | `全面的程式碼品質和維護性分析` |
| `tools` | 清單/字串 | 允許的權能 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 行為指示 | 自訂指南 |

### 工具存取層級

```mermaid
graph TD
    A["子代理設定"] -->|選項 1| B["繼承主線上的所有工具"]
    A -->|選項 2| C["指定個別工具"]
    B -->|包含| B1["檔案操作"]
    B -->|包含| B2["Shell 命令"]
    B -->|包含| B3["MCP 工具"]
    C -->|明確清單| C1["read, grep, diff"]
    C -->|明確清單| C2["Bash(npm:*), Bash(test:*)"]
```

### 實用範例

#### 範例 1：完整的子代理設定

**檔案:** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: Comprehensive code quality and maintainability analysis
tools: read, grep, diff, lint_runner
---

# 程式碼審查員代理

您是一位專注於以下方面的專業程式碼審查員：
- 效能優化
- 安全漏洞
- 程式碼維護性
- 測試覆蓋率
- 設計模式
```

## 審查優先順序 (依順序)

1. **安全性問題** - 驗證、授權、資料外洩
2. **效能問題** - O(n²) 運作、記憶體洩漏、效率不佳的查詢
3. **程式碼品質** - 可讀性、命名、文件
4. **測試覆蓋率** - 缺少測試、邊緣案例
5. **設計模式** - SOLID 原則、架構

## 審查輸出格式

對於每個問題：
- **嚴重性**: 嚴重 / 高 / 中 / 低
- **類別**: 安全性 / 效能 / 品質 / 測試 / 設計
- **位置**: 檔案路徑和行號
- **問題描述**: 出了什麼問題以及為什麼
- **建議修正**: 程式碼範例
- **影響**: 如何影響系統

## 範例審查

### 問題：N+1 查詢問題
- **嚴重性**: 高
- **類別**: 效能
- **位置**: src/user-service.ts:45
- **問題**: 迴圈在每次迭代中執行資料庫查詢
- **修正**: 使用 JOIN 或批次查詢
```

**File:** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: 測試策略、覆蓋率分析和自動化測試
tools: read, write, bash, grep
---

# 測試工程師代理

您擅長：
- 撰寫全面的測試套件
- 確保高程式碼覆蓋率 (>80%)
- 測試邊緣案例和錯誤情境
- 效能基準測試
- 整合測試

## 測試策略

1. **單元測試** - 獨立的函數/方法
2. **整合測試** - 元件互動
3. **端到端測試** - 完整的流程
4. **邊緣案例** - 邊界條件
5. **錯誤情境** - 失敗處理
```

## 測試輸出需求

- 使用 Jest 進行 JavaScript/TypeScript 測試
- 每個測試包含設定/清理階段
- 模擬外部依賴
- 記錄測試目的
- 包含效能驗證（如果適用）

## 涵蓋率需求

- 最低 80% 的程式碼涵蓋率
- 關鍵路徑需要 100%
- 報告未涵蓋的區域

**檔案:** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: 技術文件、API 文件和使用者指南
tools: read, write, grep
---

# 文件撰寫員代理

您創建：
- 具有範例的 API 文件
- 使用者指南和教學
- 架構文件
- 變更記錄條目
- 程式碼註解改進

## 文件標準

1. **清晰度** - 使用簡單、清晰的語言
2. **範例** - 包含實用的程式碼範例
3. **完整性** - 涵蓋所有參數和回傳值
4. **結構** - 使用一致的格式
5. **準確性** - 根據實際程式碼驗證

## 文件區段

### 對於 API
- 描述
- 參數（包含類型）
- 回傳值（包含類型）
- 可能的錯誤 (Throws)
- 範例（curl、JavaScript、Python）
- 相關端點

### 對於功能
- 總覽
- 預備條件
- 逐步指示
- 預期的結果
- 疑難排解
- 相關主題

#### 範例 2：子代理委派的實際操作

```markdown
# 情境：構建支付功能

## 用戶請求
"構建一個與 Stripe 整合的安全支付處理功能"

## 主要代理流程

1. **規劃階段**
   - 理解需求
   - 確定所需任務
   - 規劃架構

2. **委派給程式碼審查員子代理**
   - 任務：「審查支付處理的安全性實作」
   - 上下文：驗證、API 金鑰、token 處理
   - 審查：SQL 注入、金鑰暴露、HTTPS 強制執行

3. **委派給測試工程師子代理**
   - 任務：「為支付流程創建全面的測試」
   - 上下文：成功情境、失敗、邊緣案例
   - 創建測試：有效支付、拒絕的卡、網路失敗、webhook

4. **委派給文件撰寫員子代理**
   - 任務：「記錄支付 API 端點」
   - 上下文：請求/回覆模式
   - 產生：具有 curl 範例的 API 文件、錯誤碼

5. **綜合**
   - 主要代理收集所有輸出
   - 整合結果
   - 將完整解決方案回傳給使用者
```

#### 範例 3：工具權限範圍

**限制性設定 - 僅限特定命令**

```yaml
---
name: secure-reviewer
description: 專注於安全性的程式碼審查，權限有限
tools: read, grep
---

# 安全程式碼審查員

僅審查程式碼中的安全性漏洞。

此代理：
- ✅ 讀取檔案以進行分析
- ✅ 搜尋模式
- ❌ 無法執行程式碼
- ❌ 無法修改檔案
- ❌ 無法執行測試

這可確保審查員不會意外損壞任何內容。
```

**擴展設定 - 完整實作功能**

```yaml
---
name: implementation-agent
description: 功能開發的完整實作功能

tools: read, write, bash, grep, edit, glob
---

# 實作代理

從規格說明建立功能。

這個代理：
- ✅ 讀取規格說明
- ✅ 寫入新的程式碼檔案
- ✅ 執行建置命令
- ✅ 搜尋程式碼庫
- ✅ 編輯現有檔案
- ✅ 尋找符合模式的檔案

完全的獨立功能開發能力。

```
### 子代理上下文管理

```mermaid
graph TB
    A["主要代理上下文<br/>50,000 tokens"]
    B["子代理 1 上下文<br/>20,000 tokens"]
    C["子代理 2 上下文<br/>20,000 tokens"]
    D["子代理 3 上下文<br/>20,000 tokens"]

    A -->|乾淨的狀態| B
    A -->|乾淨的狀態| C
    A -->|乾淨的狀態| D

    B -->|僅限結果| A
    C -->|僅限結果| A
    D -->|僅限結果| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 何時使用子代理

| 情境 | 使用子代理 | 原因 |
|----------|--------------|-----|
| 具有許多步驟的複雜功能 | ✅ 是 | 分開處理，防止上下文污染 |
| 快速程式碼審查 | ❌ 否 | 無必要額外負擔 |
| 平行任務執行 | ✅ 是 | 每個子代理擁有自己的上下文 |
| 需要專門的專業知識 | ✅ 是 | 自訂系統提示詞 |
| 長時間分析 | ✅ 是 | 防止主要上下文耗盡 |
| 單一任務 | ❌ 否 | 不必要地增加延遲 |

### 代理團隊

代理團隊協調多個代理執行相關任務。 與一次委派給一個子代理不同，代理團隊允許主要代理組織一組協作、共享中間結果並為共同目標而努力的代理。 這對於需要前端代理、後端代理和測試代理並行工作的全棧功能開發等大規模任務非常有用。

---

## 記憶

### 總覽

記憶讓 Claude 能夠跨會話和對話保持上下文。它有兩種形式：在 claude.ai 中自動合成，以及在 Claude Code 中基於檔案系統的 CLAUDE.md。

### 記憶架構

```mermaid
graph TB
    A["Claude 會話"]
    B["使用者輸入"]
    C["記憶系統"]
    D["記憶儲存"]

    B -->|使用者提供資訊| C
    C -->|每 24 小時合成| D
    D -->|自動載入| A
    A -->|使用上下文| C
```

### Claude Code 中的記憶層級 (7 層)

Claude Code 從 7 層載入記憶，從最高到最低優先順序排列：

```mermaid
graph TD
    A["1. 策略管理<br/>企業管理員策略"] --> B["2. 專案記憶<br/>./CLAUDE.md"]
    B --> C["3. 專案規則<br/>.claude/rules/*.md"]
    C --> D["4. 使用者記憶<br/>~/.claude/CLAUDE.md"]
    D --> E["5. 使用者規則<br/>~/.claude/rules/*.md"]
    E --> F["6. 本機記憶<br/>.claude/local/CLAUDE.md"]
    F --> G["7. 自動記憶<br/>自動捕捉的偏好設定"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### 記憶位置表格

| 層級 | 位置 | 範圍 | 優先順序 | 共享 | 適用於 |
|------|----------|-------|----------|--------|----------|
| 1. 策略管理 | 企業管理員 | 組織 | 最高 | 所有組織使用者 | 遵循、安全策略 |
| 2. 專案 | `./CLAUDE.md` | 專案 | 高 | 團隊 (Git) | 團隊標準、架構 |
| 3. 專案規則 | `.claude/rules/*.md` | 專案 | 高 | 團隊 (Git) | 模組化專案慣例 |
| 4. 使用者 | `~/.claude/CLAUDE.md` | 個人 | 中 | 個人 | 個人偏好設定 |
| 5. 使用者規則 | `~/.claude/rules/*.md` | 個人 | 中 | 個人 | 個人規則模組 |
| 6. 本機 | `.claude/local/CLAUDE.md` | 本機 | 低 | 未共享 | 機器特定的設定 |
| 7. 自動記憶 | 自動 | 會話 | 最低 | 個人 | 學習的偏好設定、模式 |

### 自動記憶

自動記憶會自動捕捉使用者在會話期間觀察到的偏好設定和模式。Claude 從您的互動中學習並記住：

- 程式碼風格偏好設定
- 您所做的常見修正
- 框架和工具選擇
- 溝通風格偏好設定

自動記憶在後台運作，不需要手動設定。

### 記憶更新生命週期

```mermaid
sequenceDiagram
    participant 使用者
    participant Claude as Claude Code
    participant Editor as 檔案系統
    participant Memory as CLAUDE.md

    使用者->>Claude: "記住：使用 async/await"
    Claude->>使用者: "哪個記憶檔案？"
    使用者->>Claude: "專案記憶"
    Claude->>Editor: 開啟 ~/.claude/settings.json
    Claude->>Memory: 寫入 ./CLAUDE.md
    Memory-->>Claude: 檔案已儲存
    Claude->>Claude: 載入更新後的記憶
```

### 實際範例

#### 範例 1：專案記憶體結構

**檔案：** `./CLAUDE.md`

```markdown
# 專案設定

## 專案概觀
- **名稱**: 電商平台
- **技術堆疊**: Node.js, PostgreSQL, React 18, Docker
- **團隊規模**: 5 位開發人員
- **截止日期**: 2025 年第四季

## 架構
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 開發標準

### 程式碼樣式
- 使用 Prettier 格式化
- 使用 ESLint 搭配 airbnb 設定
- 最大行長度：100 個字元
- 使用 2 個空格縮排

### 命名慣例
- **檔案**: kebab-case (user-controller.js)
- **類別**: PascalCase (UserService)
- **函式/變數**: camelCase (getUserById)
- **常數**: UPPER_SNAKE_CASE (API_BASE_URL)
- **資料庫表格**: snake_case (user_accounts)

### Git 工作流程
- 分支名稱：`feature/description` 或 `fix/description`
- 提交訊息：遵循 conventional commits
- PR 必須在合併前
- 所有 CI/CD 檢查都必須通過
- 至少需要 1 項批准

### 測試需求
- 至少 80% 的程式碼覆蓋率
- 所有關鍵路徑都必須有測試
- 使用 Jest 進行單元測試
- 使用 Cypress 進行 E2E 測試
- 測試檔案名稱：`*.test.ts` 或 `*.spec.ts`

### API 標準
- 僅限 RESTful 端點
- JSON 請求/回應
- 正確使用 HTTP 狀態碼
- 版本 API 端點：`/api/v1/`
- 使用範例記錄所有端點

### 資料庫
- 對於 schema 變更，使用遷移
- 永遠不要硬編碼憑證
- 使用連線池
- 在開發環境中啟用查詢記錄
- 定期備份

### 部署
- 基於 Docker 的部署
- Kubernetes 協調
- 藍綠部署策略
- 失敗時自動回滾
- 在部署前執行資料庫遷移
```

## 常用命令

| 指令 | 目的 |
|---------|---------|
| `npm run dev` | 啟動開發伺服器 |
| `npm test` | 執行測試套件 |
| `npm run lint` | 檢查程式碼樣式 |
| `npm run build` | 為生產環境建置 |
| `npm run migrate` | 執行資料庫遷移 |

## 團隊聯絡人
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## 已知問題與解決方案
- PostgreSQL 連線池在尖峰時段限制為 20
- 解決方案：實作查詢佇列
- Safari 14 與非同步產生器相容性問題
- 解決方案：使用 Babel 轉換器

## 相關專案
- 分析儀表板：`/projects/analytics`
- 行動應用程式：`/projects/mobile`
- 管理員面板：`/projects/admin`
```

#### 範例 2：特定目錄的記憶

**檔案:** `./src/api/CLAUDE.md`

~~~~markdown
# API 模組標準

此檔案會覆寫根目錄的 CLAUDE.md，適用於 /src/api/ 中的所有內容。

## API 專屬標準

### 請求驗證
- 使用 Zod 進行模式驗證
- 務必驗證輸入
- 傳回 400，並附帶驗證錯誤
- 包含欄位等級的錯誤詳細資料

### 驗證
- 所有端點都需要 JWT token
- Token 在 Authorization 標頭中
- Token 過期後 24 小時
- 實作重新整理 token 機制

### 回應格式

所有回應都必須遵循此結構：

```json
{
  "success": true,
  "data": { /* 實際資料 */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### 錯誤回應:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "使用者訊息",
    "details": { /* 欄位錯誤 */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 分頁
- 使用游標式分頁 (而不是偏移量)
- 包含 `hasMore` 布林值
- 將最大頁面大小限制為 100
- 預設頁面大小：20

### 速率限制
- 經過身份驗證的使用者每小時 1000 請求
- 公共端點每小時 100 請求
- 超出時傳回 429
- 包含重試後標頭

### 快取
- 使用 Redis 進行會話快取
- 快取時長：預設 5 分鐘
- 在寫入操作時失效
- 使用資源類型標記快取金鑰
~~~~

#### 範例 3：個人記憶

**檔案:** `~/.claude/CLAUDE.md`

~~~~markdown
# 我的開發偏好

## 關於我
- **經驗等級**: 8 年全棧開發經驗
- **偏好的語言**: TypeScript, Python
- **溝通風格**: 直接，並提供範例
- **學習方式**: 視覺化圖表與程式碼

## 程式碼偏好

### 錯誤處理
我偏好明確的錯誤處理，使用 try-catch 區塊和有意義的錯誤訊息。
避免使用通用的錯誤。 務必記錄錯誤以利除錯。

### 註解
使用註解說明 WHY，而不是 WHAT。 程式碼應該是自我文件化的。
註解應該解釋業務邏輯或非顯而見的決策。

### 測試
我偏好 TDD (測試驅動開發)。
先寫測試，再進行實作。
專注於行為，而不是實作細節。

### 架構
我偏好模組化、鬆散耦合的設計。
使用依賴注入以提高可測試性。
分離關注點 (Controllers, Services, Repositories)。

## 除錯偏好
- 使用 console.log 並加上前綴：`[DEBUG]`
- 包含上下文：函式名稱、相關變數
- 盡可能使用堆疊追蹤
- 務必在記錄中包含時間戳記

## 溝通
- 使用圖表說明複雜的概念
- 在說明理論之前，先展示具體的範例
- 包含前後的程式碼片段
- 在結尾總結重點

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
- **IDE**: VS Code with vim keybindings
- **Terminal**: Zsh with Oh-My-Zsh
- **格式**: Prettier (100 char line length)
- **Linter**: ESLint with airbnb config
- **測試框架**: Jest with React Testing Library
~~~~

#### 範例 4：會在會話期間更新記憶

**會話互動：**

```markdown
User: Remember that I prefer using React hooks instead of class components
     for all new components.

Claude: I'm adding that to your memory. Which memory file should this go in?
        1. Project memory (./CLAUDE.md)
        2. Personal memory (~/.claude/CLAUDE.md)

User: Project memory

Claude: ✅ Memory saved!

Added to ./CLAUDE.md:
---

### Component Development
- Use functional components with React Hooks
- Prefer hooks over class components
- Custom hooks for reusable logic
- Use useCallback for event handlers
- Use useMemo for expensive computations
```

### 記憶在 Claude Web/Desktop

#### 記憶合成時程表

```mermaid
graph LR
    A["Day 1: User<br/>Conversations"] -->|24 hours| B["Day 2: Memory<br/>Synthesis"]
    B -->|Automatic| C["Memory Updated<br/>Summarized"]
    C -->|Loaded in| D["Day 2-N:<br/>New Conversations"]
    D -->|Add to| E["Memory"]
    E -->|24 hours later| F["Memory Refreshed"]
```

**範例記憶摘要：**

## Claude 的使用者記憶

### 專業背景
- 擁有 8 年經驗的高階全棧開發者
- 專注於 TypeScript/Node.js 後端和 React 前端
- 活躍的開源貢獻者
- 對 AI 和機器學習感興趣

### 專案上下文
- 目前正在構建電商平台
- 技術棧：Node.js, PostgreSQL, React 18, Docker
- 與由 5 名開發者組成的團隊合作
- 使用 CI/CD 和藍綠部署

### 溝通偏好
- 偏好直接、簡潔的解釋
- 喜歡視覺圖表和範例
- 感謝程式碼片段
- 在程式碼中說明商業邏輯

### 目前目標
- 提升 API 效能
- 將測試覆蓋率提高到 90%
- 實作快取策略
- 記錄架構
```

### 記憶功能比較

| 功能 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 自動合成 | ✅ 每 24 小時 | ❌ 手動 |
| 跨專案 | ✅ 共享 | ❌ 專案特定 |
| 團隊存取 | ✅ 共享專案 | ✅ Git 追蹤 |
| 搜尋 | ✅ 內建 | ✅ 透過 `/memory` |
| 編輯 | ✅ 在對話中 | ✅ 直接檔案編輯 |
| 匯入/匯出 | ✅ 是 | ✅ 複製/貼上 |
| 持續性 | ✅ 24 小時+ | ✅ 無期限 |

---

## MCP 協定

### 概述

MCP (Model Context Protocol) 是一種標準化的方式，讓 Claude 存取外部工具、API 和即時資料來源。與記憶不同，MCP 提供對變動資料的即時存取。

### MCP 架構

```mermaid
graph TB
    A["Claude"]
    B["MCP 伺服器"]
    C["外部服務"]

    A -->|請求：list_issues| B
    B -->|查詢| C
    C -->|資料| B
    B -->|回覆| A

    A -->|請求：create_issue| B
    B -->|動作| C
    C -->|結果| B
    B -->|回覆| A
```

### MCP 生態系統

```mermaid
graph TB
    A["Claude"] -->|MCP| B["檔案系統<br/>MCP 伺服器"]
    A -->|MCP| C["GitHub<br/>MCP 伺服器"]
    A -->|MCP| D["資料庫<br/>MCP 伺服器"]
    A -->|MCP| E["Slack<br/>MCP 伺服器"]
    A -->|MCP| F["Google Docs<br/>MCP 伺服器"]

    B -->|檔案 I/O| G["本機檔案"]
    C -->|API| H["GitHub 存放庫"]
    D -->|查詢| I["PostgreSQL/MySQL"]
    E -->|訊息| J["Slack 工作區"]
    F -->|文件| K["Google Drive"]
```

### MCP 設定流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as 設定檔案
    participant Service as 外部服務

    User->>Claude: 類型 /mcp
    Claude->>Claude: 列出可用的 MCP 伺服器
    Claude->>User: 顯示選項
    User->>Claude: 選擇 GitHub MCP
    Claude->>Config: 更新設定
    Config->>Claude: 啟用連線
    Claude->>Service: 測試連線
    Service-->>Claude: 驗證成功
    Claude->>User: ✅ MCP 連線成功！
```

### 可用的 MCP 伺服器表格

| MCP 伺服器 | 目的 | 常用工具 | 驗證 | 即時 |
|------------|---------|--------------|------|-----------|
| **檔案系統** | 檔案操作 | 讀取、寫入、刪除 | 作業系統權限 | ✅ 是 |
| **GitHub** | 存放庫管理 | list_prs, create_issue, push | OAuth | ✅ 是 |

| **Slack** | Team communication | send_message, list_channels | Token | ✅ Yes |
| **Database** | SQL queries | query, insert, update | Credentials | ✅ Yes |
| **Google Docs** | Document access | read, write, share | OAuth | ✅ Yes |
| **Asana** | Project management | create_task, update_status | API Key | ✅ Yes |
| **Stripe** | Payment data | list_charges, create_invoice | API Key | ✅ Yes |
| **Memory** | Persistent memory | store, retrieve, delete | Local | ❌ No |

### Practical Examples

#### Example 1: GitHub MCP Configuration

**File:** `.mcp.json` (project scope) or `~/.claude.json` (user scope)

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

**Available GitHub MCP Tools:**

~~~~markdown
# GitHub MCP Tools

## Pull Request Management
- `list_prs` - List all PRs in repository
- `get_pr` - Get PR details including diff
- `create_pr` - Create new PR
- `update_pr` - Update PR description/title
- `merge_pr` - Merge PR to main branch
- `review_pr` - Add review comments

Example request:
```
/mcp__github__get_pr 456

# Returns:
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

## Issue Management
- `list_issues` - List all issues
- `get_issue` - Get issue details
- `create_issue` - Create new issue
- `close_issue` - Close issue
- `add_comment` - Add comment to issue

## 存放庫資訊
- `get_repo_info` - 存放庫詳細資訊
- `list_files` - 檔案樹狀結構
- `get_file_content` - 讀取檔案內容
- `search_code` - 搜尋程式碼

## 提交操作
- `list_commits` - 提交歷史
- `get_commit` - 特定提交詳細資訊
- `create_commit` - 建立新的提交
~~~~

#### 範例 2：資料庫 MCP 設定

**設定：**

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

**範例用法：**

```markdown
使用者：取得所有訂單數大於 10 的使用者

Claude：我會查詢您的資料庫以取得這些資訊。

# 使用 MCP 資料庫工具：
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 成果：
- Alice: 15 筆訂單
- Bob: 12 筆訂單
- Charlie: 11 筆訂單
```

#### 範例 3：多 MCP 工作流程

**情境：每日報告產生**

```markdown
# 每日報告工作流程，使用多個 MCP

## 設定
1. GitHub MCP - 取得 PR 指標
2. 資料庫 MCP - 查詢銷售資料
3. Slack MCP - 發布報告
4. 檔案系統 MCP - 儲存報告

## 工作流程

### 步驟 1：取得 GitHub 資料
/mcp__github__list_prs completed:true last:7days

輸出：
- 總 PR 數：42
- 平均合併時間：2.3 小時
- 審查時間：1.1 小時

### 步驟 2：查詢資料庫
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

輸出：
- 銷售額：247
- 收入：$12,450

### 步驟 3：產生報告
將資料合併到 HTML 報告

### 步驟 4：儲存到檔案系統
將 report.html 寫入 /reports/

### 步驟 5：發布到 Slack
將摘要發送到 #daily-reports 頻道

最終輸出：
✅ 報告已產生並發布
📊 本週已合併 47 個 PR
💰 每日銷售額 $12,450
```

#### 範例 4：檔案系統 MCP 操作

**設定：**

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

**可用的操作：**

| 操作 | 命令 | 目的 |
|---|---|---|
| 列出檔案 | `ls ~/projects` | 顯示目錄內容 |
| 讀取檔案 | `cat src/main.ts` | 讀取檔案內容 |
| 寫入檔案 | `create docs/api.md` | 建立新檔案 |
| 編輯檔案 | `edit src/app.ts` | 修改檔案 |
| 搜尋 | `grep "async function"` | 在檔案中搜尋 |
| 刪除 | `rm old-file.js` | 刪除檔案 |

### MCP 與記憶體：決策矩陣

```mermaid
graph TD
    A["需要外部資料嗎？"]
    A -->|不| B["使用記憶體"]
    A -->|是| C["資料是否頻繁變更？"]
    C -->|不/很少| B
    C -->|是/經常| D["使用 MCP"]

    B -->|儲存| E["偏好設定<br/>上下文<br/>歷史"]
    D -->|存取| F["串流 API<br/>資料庫<br/>服務"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 請求/回應模式

```mermaid

## 代理技能

### 總覽

代理技能是可重複使用的、模型調用的能力，封裝成包含指令、腳本和資源的資料夾。Claude 會自動偵測並使用相關技能。

### 技能架構

```mermaid
graph TB
    A["技能目錄"]
    B["SKILL.md"]
    C["YAML 元數據"]
    D["指令"]
    E["腳本"]
    F["範本"]

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

    User->>Claude: "建立 Excel 報告"
    Claude->>System: 掃描可用技能
    System->>System: 載入技能元數據
    Claude->>Claude: 將使用者請求與技能比對
    Claude->>Skill: 載入 xlsx 技能 SKILL.md
    Skill-->>Claude: 傳回指令 + 工具
    Claude->>Claude: 執行技能
    Claude->>User: 產生 Excel 檔案
```

### 技能類型與位置表格

| 類型 | 位置 | 範圍 | 共享 | 同步 | 適用於 |
|------|----------|-------|--------|------|----------|
| 內建 | 內建 | 全域 | 所有使用者 | 自動 | 文件建立 |
| 個人 | `~/.claude/skills/` | 個人 | 否 | 手動 | 個人自動化 |
| 專案 | `.claude/skills/` | 團隊 | 是 | Git | 團隊標準 |
| 外掛 | 透過外掛安裝 | 變動 | 視情況而定 | 自動 | 整合功能 |

### 內建技能

```mermaid
graph TB
    A["內建技能"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["建立簡報"]
    B --> B2["編輯投影片"]
    C --> C1["建立試算表"]
    C --> C2["分析數據"]
    D --> D1["建立文件"]
    D --> D2["格式化文字"]
    E --> E1["產生 PDF"]
    E --> E2["填寫表單"]
```

### 內含技能

Claude Code 現在包含 5 個內含技能，可以直接使用：

| 技能 | 指令 | 目的 |
|-------|---------|---------|
| **Simplify** | `/simplify` | 簡化複雜的程式碼或說明 |
| **Batch** | `/batch` | 對多個檔案或項目執行操作 |
| **Debug** | `/debug` | 對問題進行系統除錯，並進行根本原因分析 |
| **Loop** | `/loop` | 根據計時器排定定時任務 |
| **Claude API** | `/claude-api` | 直接與 Anthropic API 互動 |

這些內含技能始終可用，無需安裝或設定。

### 實用範例

#### 範例 1：自訂程式碼審查技能

**目錄結構：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
```

```
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**File:** `~/.claude/skills/code-review/SKILL.md`

```yaml
---
name: Code Review Specialist
description: Comprehensive code review with security, performance, and quality analysis
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: When users ask to review code, analyze code quality, or evaluate pull requests
effort: high
shell: bash
---

# Code Review Skill

This skill provides comprehensive code review capabilities focusing on:

1. **Security Analysis**
   - Authentication/authorization issues
   - Data exposure risks
   - Injection vulnerabilities
   - Cryptographic weaknesses
   - Sensitive data logging

2. **Performance Review**
   - Algorithm efficiency (Big O analysis)
   - Memory optimization
   - Database query optimization
   - Caching opportunities
   - Concurrency issues

3. **Code Quality**
   - SOLID principles
   - Design patterns
   - Naming conventions
   - Documentation
   - Test coverage

4. **Maintainability**
   - Code readability
   - Function size (should be < 50 lines)
   - Cyclomatic complexity
   - Dependency management
   - Type safety

## Review Template

For each piece of code reviewed, provide:

### Summary
- Overall quality assessment (1-5)
- Key findings count
- Recommended priority areas

### Critical Issues (if any)
- **Issue**: Clear description
- **Location**: File and line number
- **Impact**: Why this matters
- **Severity**: Critical/High/Medium
- **Fix**: Code example

### Findings by Category

#### Security (if issues found)
List security vulnerabilities with examples

#### Performance (if issues found)
List performance problems with complexity analysis

#### Quality (if issues found)
List code quality issues with refactoring suggestions

#### Maintainability (if issues found)
List maintainability problems with improvements
```

## Python Script: analyze-metrics.py

```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """分析程式碼以取得常見指標。」」

    # 統計函式
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # 統計類別
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # 平均行長
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # 估算複雜度
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
比較變更前後程式碼的循環複雜度。
有助於判斷重構是否真的簡化了程式碼結構。
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """分析程式碼複雜度指標。」」

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        使用 McCabe 方法計算循環複雜度。
        計算決策點：if、elif、else、for、while、except、and、or
        """
        complexity = 1  # 基礎複雜度

        # 統計決策點
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
        計算認知複雜度 - 難以理解？
        基於巢狀深度和控制流程。
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # 追蹤巢狀深度
            if re.search(r'^\s*(if|for|while|def|class|try)\b', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)\b', line):
                cognitive += nesting_depth

            # 減少巢狀時縮排
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        可維護性指數的範圍是 0-100。
        > 85: 優秀
        > 65: 良好
        > 50: 普通
        < 50: 差勁
        """
```

```python
    def calculate_maintainability_index(self) -> float:
        """Calculate maintainability index."""
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """Generate comprehensive complexity report."""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """Compare complexity metrics between two code versions."""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("程式碼複雜度比較")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  迴圈複雜度:    {before_metrics['cyclomatic_complexity']}")
    print(f"  認知複雜度:     {before_metrics['cognitive_complexity']}")
    print(f"  可維護性指數:    {before_metrics['maintainability_index']}")
    print(f"  程式碼行數:            {before_metrics['lines_of_code']}")
    print(f"  平均行長:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  迴圈複雜度:    {after_metrics['cyclomatic_complexity']}")
    print(f"  認知複雜度:     {after_metrics['cognitive_complexity']}")
    print(f"  可維護性指數:    {after_metrics['maintainability_index']}")
    print(f"  程式碼行數:            {after_metrics['lines_of_code']}")
    print(f"  平均行長:          {after_metrics['avg_line_length']}")

    print("\n變更:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  迴圈複雜度:    {cyclomatic_change:+d}")
    print(f"  認知複雜度:     {cognitive_change:+d}")
    print(f"  可維護性指數:    {mi_change:+.2f}")
    print(f"  程式碼行數:            {loc_change:+d}")

    print("\n評估:")
    if mi_change > 0:
        print("  ✅ 程式碼更具可維護性")
```

```
elif mi_change < 0:
    print("  ⚠️  Code 的可維護性降低")
else:
    print("  ➡️  可維護性未變")

if cyclomatic_change < 0:
    print("  ✅ 複雜度降低")
elif cyclomatic_change > 0:
    print("  ⚠️  複雜度增加")
else:
    print("  ➡️  複雜度未變")

print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```

## 範本: review-checklist.md

```markdown
# 程式碼審查清單

## 安全性清單
- [ ] 沒有硬編碼的憑證或機密資訊
- [ ] 所有使用者輸入的輸入驗證
- [ ] SQL 注入防護 (參數化查詢)
- [ ] 狀態變更操作的 CSRF 保護
- [ ] XSS 防護，使用適當的轉譯
- [ ] 保護端點的驗證檢查
- [ ] 資源的授權檢查
- [ ] 安全的密碼雜湊 (bcrypt, argon2)
- [ ] 日誌中沒有敏感資料
- [ ] 強制 HTTPS

## 效能清單
- [ ] 沒有 N+1 查詢
- [ ] 索引的適當使用
- [ ] 實作有益的快取
- [ ] 主執行緒上沒有阻塞操作
- [ ] 正確使用 Async/await
- [ ] 對大型資料集進行分頁
- [ ] 資料庫連線池化
- [ ] 正規表達式優化
- [ ] 沒有不必要的物件建立
- [ ] 防止記憶體洩漏

## 品質清單
- [ ] 函式 < 50 行
- [ ] 清晰的變數命名
- [ ] 沒有重複程式碼
- [ ] 適當的錯誤處理
- [ ] 註解解釋 WHY，而不是 WHAT
- [ ] 程式碼中沒有 console.logs (在生產環境中)
- [ ] 類型檢查 (TypeScript/JSDoc)
- [ ] 遵循 SOLID 原則
- [ ] 正確應用設計模式
- [ ] 自行記錄程式碼
```

## 測試檢查清單
- [ ] 單元測試已撰寫
- [ ] 邊際案例已涵蓋
- [ ] 錯誤情境已測試
- [ ] 整合測試已存在
- [ ] 涵蓋率 > 80%
- [ ] 沒有不穩定的測試
- [ ] 模擬外部依賴
- [ ] 清晰的測試名稱

```

## 範本: finding-template.md

~~~~markdown
# 程式碼審查發現範本

使用此範本來記錄程式碼審查中發現的每個問題。

---

## 問題：[標題]

### 嚴重程度
- [ ] 嚴重 (阻礙部署)
- [ ] 高 (應在合併前修正)
- [ ] 中等 (應盡快修正)
- [ ] 低 (如果方便可以修正)

### 類別
- [ ] 安全性
- [ ] 效能
- [ ] 程式碼品質
- [ ] 可維護性
- [ ] 測試
- [ ] 設計模式
- [ ] 文件

### 位置
**檔案:** `src/components/UserCard.tsx`

**行號:** 45-52

**函式/方法:** `renderUserDetails()`

### 問題描述

**什麼:** 描述問題是什麼。

**為什麼重要:** 說明影響以及為什麼需要修正。

**目前行為:** 顯示有問題的程式碼或行為。

**預期行為:** 描述應該發生的事情。

### 程式碼範例

#### 目前 (有問題)

```typescript
// 顯示 N+1 查詢問題
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // 每個使用者查詢一次！
  renderUserPosts(posts);
});
```

#### 建議修正

```typescript
// 使用 JOIN 查詢優化
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### 影響分析

| 方面 | 影響 | 嚴重程度 |
|--------|--------|----------|
| 效能 | 對 20 名使用者進行 100+ 次查詢 | 高 |
| 使用者體驗 | 頁面載入速度慢 | 高 |
| 可擴展性 | 在大規模應用中失效 | 嚴重 |
| 可維護性 | 難以除錯 | 中等 |

### 相關問題

- `AdminUserList.tsx` 第 120 行的類似問題
- 相關 PR: #456
- 相關問題: #789

### 額外資源

- [N+1 查詢問題](https://en.wikipedia.org/wiki/N%2B1_problem)
- [資料庫 JOIN 文件](https://docs.example.com/joins)

### 審查者備註

- 這是此程式碼庫中的常見模式
- 考慮將其新增到程式碼樣式指南
- 可能值得創建一個輔助函數

### 作者回覆 (用於回饋)

*由程式碼作者填寫：*

- [ ] 修正已在提交中實施：`abc123`
- [ ] 修正狀態：完成 / 進行中 / 需要討論
- [ ] 問題或疑慮：(描述)

---

## 查找統計資料（供審查者使用）

在審查多個發現結果時，請追蹤：

- **總共發現的問題數：** X
- **嚴重：** X
- **高：** X
- **中：** X
- **低：** X

**建議：** ✅ 批准 / ⚠️ 請求修改 / 🔄 需要討論

**整體程式碼品質：** 1-5 星

~~~~

## 使用範例

~~~~markdown
使用者：審查此 React 元件程式碼

Claude：我將使用我的程式碼審查技能進行全面的程式碼審查。

[載入 SKILL.md]
[根據檢查清單分析程式碼]
[執行 Python 指標腳本]
[套用範本]

## 程式碼審查結果

### 摘要
品質分數：3.5/5
主要發現：7 項
優先區域：效能優化、錯誤處理

### 嚴重問題：1 項

**問題：** N+1 查詢問題
**位置：** UserComponent.tsx, 45-52 行
**影響：** 對 20 名使用者的列表渲染 100+ 個資料庫查詢
**嚴重性：** 高
**修正方法：**
```javascript
// 之前：N+1 查詢
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20+ 查詢

// 之後：使用 JOIN 的單一查詢
const users = fetchUsersWithPosts(); // 1 查詢
```

### 效能發現

- 大型列表缺少分頁
- 建議：使用 React.memo() 對項目進行處理
- 資料庫查詢：可以使用索引進行優化

### 品質發現

- 第 20 行的函式長度為 127 行（最大：50）
- 缺少錯誤邊界
- Props 應該具有 TypeScript 類型

~~~~

#### 範例 2：品牌聲音技能

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

## 總覽

這個技能確保所有溝通都保持一致的品牌聲音、語氣和訊息傳遞。

## 品牌形象

### 使命
幫助團隊使用 AI 自動化其開發工作流程。

### 價值觀
- **簡潔性**: 將複雜的事情變得簡單
- **可靠性**: 堅實可靠的執行
- **賦能**: 激發人類的創造力

### 語氣
- **親切但專業** - 容易接近但不隨意
- **清晰且簡潔** - 避免行話，簡單地解釋技術概念
- **自信** - 我們知道自己在做什麼
- **富有同理心** - 了解使用者需求和痛點

## 書寫指南

### 應該做 ✅
- 在與讀者溝通時使用 "你"
- 使用主動語態：例如 "Claude 產生報告"，而不是 "報告由 Claude 產生"
- 從價值主張開始
- 使用具體的例子
- 將句子控制在 20 個字以內
- 使用列表以提高清晰度
- 包含行動呼籲

### 不應該做 ❌
- 避免使用企業行話
- 避免居高臨下或過度簡化
- 避免使用 "我們相信" 或 "我們認為"
- 除非用於強調，否則避免使用全大寫字母
- 避免創建大段文字
- 避免假設使用者具有技術知識

## 詞彙

### ✅ 偏好的術語
- Claude (不使用 "the Claude AI")
- 程式碼生成 (不使用 "auto-coding")
- 代理 (不使用 "bot")
- 簡化 (不使用 "revolutionize")
- 整合 (不使用 "synergize")

### ❌ 避免使用的術語
- "Cutting-edge" (過度使用)
- "Game-changer" (模糊)
- "Leverage" (企業術語)
- "Utilize" (使用 "use")
- "Paradigm shift" (不明確)

## 範例

### ✅ 好的範例
"Claude 自動化您的程式碼審查流程。與手動檢查每個 PR 相比，Claude 審查安全性、效能和品質，每週為您的團隊節省數小時。"

為什麼有效：清晰的價值主張，具體的優點，以行動為導向

### ❌ 壞的範例
"Claude 運用尖端的 AI 來提供全面的軟體開發解決方案。"

為什麼無效：模糊，企業術語，沒有具體的價值

## 範本：電子郵件

```
主旨：[清晰、以效益為導向的主旨]

您好 [姓名]，

[開頭：對他們的價值]

[內文：如何運作 / 他們將獲得什麼]

[具體的例子或好處]

[行動呼籲：明確的下一步]

此致，
[姓名]
```

## 範本：社群媒體

```
[鉤子：在第一行吸引注意力]
[2-3 行：價值或有趣的事實]
[行動呼籲：連結、問題或互動]
[表情符號：最多 1-2 個以增加視覺效果]
```

## 檔案：tone-examples.md
```
令人興奮的公告：
"每週節省 8 小時的程式碼審查時間。Claude 自動審查您的 PR。"

同理心的支援：
"我們知道部署可能壓力很大。Claude 處理測試，所以您不必擔心。"

自信的產品功能：
"Claude 不僅僅是提出程式碼建議。它了解您的架構並維持一致性。"

教育性的部落格文章：
"讓我們來探索代理如何改善程式碼審查工作流程。以下是我們學到的..."
```

#### 範例 3：文件產生器技能

**檔案：** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
name: API 文件產生器
description: 從原始程式碼產生全面的、精確的 API 文件
version: "1.0.0"
tags:
  - 文件
  - api
  - 自動化
when_to_use: 在建立或更新 API 文件時
---

# API 文件產生器技能

## 產生

- OpenAPI/Swagger 規格
- API 終點文件
- SDK 使用範例
- 整合指南
- 錯誤碼參考
- 驗證指南

## 文件結構

### 每個終點

```markdown

## GET /api/v1/users/:id

### Description
簡要說明此端點的功能

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| id | string | Yes | 用戶 ID |

### Response

**200 成功**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 找不到**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### Examples

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

### Skill Discovery & Invocation

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

### 技能 vs 其他功能

```mermaid
graph TB
    A["擴展 Claude"]
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

    B -->|使用者觸發| G["快速捷徑"]
    C -->|自動委派| H["隔離上下文"]
    D -->|持續性| I["跨會話上下文"]
    E -->|即時| J["外部資料存取"]
    F -->|自動觸發| K["自主執行"]
```

---

## Claude Code 外掛

### 總覽

Claude Code 外掛是捆綁的自訂設定集合（斜線命令、子代理、MCP 伺服器和鉤子），它們可以使用單一命令安裝。它們代表最高層級的擴展機制，將多個功能合併到協同、可分享的套件中。

### 架構

```mermaid
graph TB
    A["外掛"]
    B["斜線命令"]
    C["子代理"]
    D["MCP 伺服器"]
    E["鉤子"]
    F["設定"]

    A -->|捆綁| B
    A -->|捆綁| C
    A -->|捆綁| D
    A -->|捆綁| E
    A -->|捆綁| F
```

### 外掛載入流程

```mermaid
sequenceDiagram
    participant 使用者
    participant Claude as Claude Code
    participant 外掛 as 外掛市集
    participant 安裝 as 安裝
    participant SlashCmds as 斜線命令
    participant Subagents
    participant MCPServers as MCP 伺服器
    participant Hooks
    participant 工具 as 設定工具

    使用者->>Claude: /plugin install pr-review
    Claude->>外掛: 下載外掛清單
    外掛-->>Claude: 傳回外掛定義
    Claude->>安裝: 提取元件
    安裝->>SlashCmds: 設定
    安裝->>Subagents: 設定
    安裝->>MCPServers: 設定
    安裝->>Hooks: 設定
    SlashCmds-->>工具: 準備就緒
    Subagents-->>工具: 準備就緒
    MCPServers-->>工具: 準備就緒
    Hooks-->>工具: 準備就緒
    工具-->>Claude: 外掛已安裝 ✅
```

### 外掛類型與發布

| 類型 | 範圍 | 共享 | 權限 | 範例 |
|------|-------|--------|-----------|----------|
| 官方 | 全域 | 所有使用者 | Anthropic | PR 審查、安全性指導 |
| 社群 | 公開 | 所有使用者 | 社群 | DevOps、資料科學 |
| 組織 | 內部 | 團隊成員 | 公司 | 內部標準、工具 |
| 個人 | 個人 | 單一使用者 | 開發者 | 自訂工作流程 |

### 外掛定義結構

```yaml
---
name: plugin-name
version: "1.0.0"
description: "這個外掛的作用"
author: "您的姓名"
license: MIT

# 外掛元數據
tags:
  - category
  - use-case

# 需求
requires:
  - claude-code: ">=1.0.0"

# 捆綁的元件
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# 設定
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

### 實用範例

#### 範例 1：PR 審查外掛

**檔案:** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "完成 PR 審查工作流程，包含安全性、測試和文件",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**檔案:** `commands/review-pr.md`

```markdown
---
name: 審查 PR
description: 啟動全面的 PR 審查，包含安全性及測試檢查
---

# PR 審查

這個命令啟動完整的拉取請求審查，包含：

1. 安全性分析
2. 測試覆蓋率驗證
3. 文件更新
4. 程式碼品質檢查
5. 效能影響評估
```

**檔案:** `agents/security-reviewer.md`

```yaml
---
name: security-reviewer
description: 專注於安全性的程式碼審查
tools: read, grep, diff
---

# 安全性審查員

專門找出安全性漏洞：
- 驗證/授權問題
- 資料外洩
- 注入攻擊
- 安全配置
```

**安裝:**

```bash
/plugin install pr-review

# 结果：
# ✅ 3 斜線命令安裝
# ✅ 3 子代理配置
# ✅ 2 MCP 伺服器連接
# ✅ 4 鉤子註冊
# ✅ 準備就緒！
```

#### 範例 2：DevOps 外掛

**組件:**

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

**內建組件:**

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

### 外掛程式市集

```mermaid
graph TB
    A["外掛程式市集"]
    B["官方<br/>Anthropic"]
    C["社群<br/>市集"]
    D["企業<br/>登錄檔"]

    A --> B
    A --> C
    A --> D

    B -->|類別| B1["開發"]
    B -->|類別| B2["DevOps"]
    B -->|類別| B3["文件"]

    C -->|搜尋| C1["DevOps 自動化"]
    C -->|搜尋| C2["行動開發"]
```

### 外掛程式安裝與生命週期

```mermaid
graph LR
    A["探索"] -->|瀏覽| B["市集"]
    B -->|選擇| C["外掛程式頁面"]
    C -->|檢視| D["元件"]
    D -->|安裝| E["/plugin install"]
    E -->|提取| F["設定"]
    F -->|啟用| G["使用"]
    G -->|檢查| H["更新"]
    H -->|可用| G
    G -->|完成| I["停用"]
    I -->|稍後| J["啟用"]
    J -->|返回| G
```

### 外掛程式功能比較

| 功能 | 斜線命令 | 技能 | 子代理 | 外掛程式 |
|---------|---------------|-------|----------|--------|
| **安裝** | 手動複製 | 手動複製 | 手動設定 | 一個命令 |
| **設定時間** | 5 分鐘 | 10 分鐘 | 15 分鐘 | 2 分鐘 |
| **打包** | 單一檔案 | 單一檔案 | 單一檔案 | 多個 |
| **版本控制** | 手動 | 手動 | 手動 | 自動 |
| **團隊分享** | 複製檔案 | 複製檔案 | 複製檔案 | 安裝 ID |
| **更新** | 手動 | 手動 | 手動 | 自動可用 |
| **相依性** | 無 | 無 | 無 | 可能包含 |
| **市集** | 否 | 否 | 否 | 是 |
| **發布** | 儲存庫 | 儲存庫 | 儲存庫 | 市集 |

### 外掛程式使用案例

| 使用案例 | 建議 | 原因 |
|----------|-----------------|-----|
| **團隊導入** | ✅ 使用外掛程式 | 即時設定，所有設定 |
| **框架設定** | ✅ 使用外掛程式 | 打包框架特定的命令 |
| **企業標準** | ✅ 使用外掛程式 | 集中發布，版本控制 |
| **快速任務自動化** | ❌ 使用命令 | 過於複雜 |
| **單一領域專業知識** | ❌ 使用技能 | 太過重量，使用技能代替 |
| **專業分析** | ❌ 使用子代理 | 手動建立或使用技能 |
| **即時數據存取** | ❌ 使用 MCP | 獨立運作，不要打包 |

### 何時建立外掛程式

```mermaid
graph TD
    A["我應該建立外掛程式嗎？"]
    A -->|需要多個元件| B{"多個命令<br/>或子代理<br/>或 MCPs?"}
    B -->|是| C["✅ 建立外掛程式"]
    B -->|否| D["使用個別功能"]
    A -->|團隊工作流程| E{"與團隊分享?"}
    E -->|是| C
    E -->|否| F["保留為本地設定"]
    A -->|複雜設定| G{"需要自動<br/>設定?"}
    G -->|是| C
    G -->|否| D
```

### 發布外掛程式

**發布步驟：**

1. 建立包含所有元件的外掛程式結構
2. 撰寫 `.claude-plugin/plugin.json` 清單
3. 建立 `README.md` 包含文件
4. 使用 `/plugin install ./my-plugin` 本地測試
5. 提交至外掛程式市集
6. 進行審查並批准
7. 在市集上發布
8. 用戶可以使用一個命令安裝

**範例提交：**

~~~~markdown
# PR 審查外掛程式

## 說明
完整 PR 審查工作流程，包含安全性、測試和文件檢查。

## 內含內容
- 3 個斜線命令，用於不同審查類型
- 3 個專業子代理
- GitHub 和 CodeQL MCP 整合
- 自動化的安全性掃描鉤子

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

## 使用方法
```bash
/review-pr
/check-security
/check-tests
```

## 需求
- Claude Code 1.0+
- GitHub 存取權
- CodeQL (可選)
~~~~

### 外掛程式與手動設定

**手動設定 (2+ 小時):**
- 逐一安裝斜線命令
- 獨立建立子代理
- 獨立設定 MCP
- 手動設定鉤子
- 記錄所有內容
- 與團隊分享 (希望他們能正確設定)

**使用外掛程式 (2 分鐘):**
```bash
/plugin install pr-review
# ✅ 所有內容已安裝並設定
# ✅ 立即使用
# ✅ 團隊可以重現相同的設定
```

---

## 比較與整合

### 功能比較矩陣

| 功能 | 觸發方式 | 持續性 | 範圍 | 使用案例 |
|---------|-----------|------------|-------|----------|
| **斜線命令** | 手動 (`/cmd`) | 會話僅限 | 單一命令 | 快速捷徑 |
| **子代理** | 自動委派 | 隔離上下文 | 專業任務 | 任務分配 |
| **記憶** | 自動載入 | 跨會話 | 用戶/團隊上下文 | 長期學習 |
| **MCP 協定** | 自動查詢 | 即時外部 | 即時數據存取 | 動態資訊 |
| **技能** | 自動調用 | 檔案系統基礎 | 可重複使用的專業知識 | 自動化工作流程 |

### 互動時序

```mermaid
graph LR
    A["會話開始"] -->|載入| B["記憶 (CLAUDE.md)"]
    B -->|發現| C["可用的技能"]
    C -->|註冊| D["斜線命令"]
    D -->|連接| E["MCP 伺服器"]
    E -->|就緒| F["使用者互動"]

    F -->|輸入 /cmd| G["斜線命令"]
    F -->|請求| H["技能自動調用"]
    F -->|查詢| I["MCP 數據"]
    F -->|複雜任務| J["委派給子代理"]

    G -->|使用| B
    H -->|使用| B
    I -->|使用| B
    J -->|使用| B
```

### 實際整合範例：客戶支援自動化

#### 架構

```mermaid
graph TB
    User["客戶電子郵件"] -->|接收| Router["支援路由器"]

    Router -->|分析| Memory["記憶<br/>客戶歷史"]
    Router -->|查詢| MCP1["MCP: 客戶資料庫<br/>先前票證"]
    Router -->|檢查| MCP2["MCP: Slack<br/>團隊狀態"]

    Router -->|路由複雜| Sub1["子代理：技術支援<br/>上下文：技術問題"]
    Router -->|路由簡單| Sub2["子代理：帳單<br/>上下文：付款問題"]
    Router -->|路由緊急| Sub3["子代理：升級<br/>上下文：優先處理"]

    Sub1 -->|格式化| Skill1["技能：回覆產生器<br/>品牌語氣維護"]
    Sub2 -->|格式化| Skill2["技能：回覆產生器"]
    Sub3 -->|格式化| Skill3["技能：回覆產生器"]

    Skill1 -->|產生| Output["格式化的回覆"]

### Skill2 -->|Generate| Output
### Skill3 -->|Generate| Output

### Output -->|Post| MCP3["MCP: Slack<br/>Notify team"]
### Output -->|Send| Reply["Customer Reply"]

#### Request Flow

```markdown
## 客戶支援請求流程

### 1. 收到電子郵件
"我試著上傳檔案時出現 500 錯誤。這正在阻礙我的工作流程！"

### 2. 記憶體查詢
- 載入 CLAUDE.md 支援標準
- 檢查客戶歷史記錄：VIP 客戶，本月第 3 次發生事件

### 3. MCP 查詢
- GitHub MCP：列出開放問題（找到相關的錯誤報告）
- 資料庫 MCP：檢查系統狀態（未報告任何中斷）
- Slack MCP：檢查工程團隊是否知情

### 4. 技能偵測與載入
- 請求符合 "技術支援" 技能
- 從技能載入支援回覆範本

### 5. 子代理委派
- 路由到技術支援子代理
- 提供上下文：客戶歷史記錄、錯誤詳細資料、已知問題
- 子代理擁有以下工具的完整存取權：讀取、bash、grep 工具

### 6. 子代理處理
技術支援子代理：
- 搜尋程式碼庫中檔案上傳的 500 錯誤
- 找到 commit 8f4a2c 中的最近變更
- 建立 workaround 文件

### 7. 技能執行
回覆產生器技能：
- 使用品牌聲音指南
- 使用同理心格式回覆
- 包含 workaround 步驟
- 連結到相關文件

### 8. MCP 輸出
- 貼文更新到 #support Slack 頻道
- 標記工程團隊
- 在 Jira MCP 中更新票證

### 9. 回覆
客戶收到：
- 同理心的確認
- 原因說明
- 立即 workaround
- 永久修正的時間表
- 連結到相關問題
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

    User->>Claude: 請求： "建立 auth 系統"
    Claude->>Memory: 載入專案標準
    Memory-->>Claude: Auth 標準、團隊實務
    Claude->>MCP: 查詢 GitHub 以尋找類似的實作
    MCP-->>Claude: 程式碼範例、最佳實務
    Claude->>Skills: 偵測符合的技能
    Skills-->>Claude: 安全性審查技能 + 測試技能
    Claude->>SubAgent: 委派實作
    SubAgent->>SubAgent: 建立功能
    Claude->>Skills: 應用安全性審查技能
    Skills-->>Claude: 安全性檢查清單結果
    Claude->>SubAgent: 委派測試
    SubAgent-->>Claude: 測試結果
    Claude->>User: 完整系統交付
```

### When to Use Each Feature

```mermaid
graph TD
    A["新任務"] --> B{任務類型？}

    B -->|重複的工作流程| C["斜線命令"]
    B -->|需要即時資料| D["MCP 協定"]
    B -->|下次需要記住| E["記憶體"]
    B -->|專門的子任務| F["子代理"]
    B -->|特定領域的工作| G["技能"]

    C --> C1["✅ 團隊快捷方式"]
    D --> D1["✅ 即時 API 存取"]
    E --> E1["✅ 持續的上下文"]
    F --> F1["✅ 並行執行"]
    G --> G1["✅ 自動調用的專業知識"]
```

### Selection Decision Tree

```mermaid
graph TD
    Start["Need to extend Claude?"]

    Start -->|Quick repeated task| A{"Manual or Auto?"}
    A -->|Manual| B["Slash Command"]
    A -->|Auto| C["Skill"]

    Start -->|Need external data| D{"Real-time?"}
    D -->|Yes| E["MCP Protocol"]
    D -->|No/Cross-session| F["Memory"]

    Start -->|Complex project| G{"Multiple roles?"}
    G -->|Yes| H["Subagents"]
    G -->|No| I["Skills + Memory"]

    Start -->|Long-term context| J["Memory"]
    Start -->|Team workflow| K["Slash Command +<br/>Memory"]
    Start -->|Full automation| L["Skills +<br/>Subagents +<br/>MCP"]
```

---

## Summary Table

| Aspect | Slash Commands | Subagents | Memory | MCP | Skills | Plugins |
|--------|---|---|---|---|---|---|
| **Setup Difficulty** | Easy | Medium | Easy | Medium | Medium | Easy |
| **Learning Curve** | Low | Medium | Low | Medium | Medium | Low |
| **Team Benefit** | High | High | Medium | High | High | Very High |
| **Automation Level** | Low | High | Medium | High | High | Very High |
| **Context Management** | Single-session | Isolated | Persistent | Real-time | Persistent | All features |
| **Maintenance Burden** | Low | Medium | Low | Medium | Medium | Low |
| **Scalability** | Good | Excellent | Good | Excellent | Excellent | Excellent |
| **Shareability** | Fair | Fair | Good | Good | Good | Excellent |
| **Versioning** | Manual | Manual | Manual | Manual | Manual | Automatic |
| **Installation** | Manual copy | Manual config | N/A | Manual config | Manual copy | One command |

---

## 快速上手指南

### 第一週：從小處著手
- 建立 2-3 個斜線命令以處理常見任務
- 在設定中啟用記憶
- 在 CLAUDE.md 中記錄團隊標準

### 第二週：新增即時存取
- 設定 1 個 MCP (GitHub 或資料庫)
- 使用 `/mcp` 進行設定
- 在您的工作流程中查詢即時資料

### 第三週：分散工作
- 建立第一個子代理以處理特定角色
- 使用 `/agents` 命令
- 使用簡單任務測試委派

### 第四週：自動化所有內容
- 建立第一個技能以處理重複的自動化
- 使用技能市集或自訂建立
- 結合所有功能以實現完整的流程

### 持續進行
- 每月審查和更新記憶
- 隨著模式出現，新增新的技能
- 優化 MCP 查詢
- 完善子代理的提示詞

---

## 鉤子

### 總覽

鉤子是事件驅動的 shell 命令，會在 Claude Code 事件發生時自動執行。 透過鉤子可以實現自動化、驗證和自訂工作流程，而無需手動介入。

### 鉤子事件

Claude Code 支援 **25 個鉤子事件**，跨四種類型的鉤子（命令、HTTP、提示詞、代理）：

| 鉤子事件 | 觸發 | 用途 |
|------------|---------|-----------|
| **SessionStart** | 工作階段開始/恢復/清除/壓縮 | 環境設定、初始化 |
| **InstructionsLoaded** | 加載 CLAUDE.md 或規則檔案 | 驗證、轉換、增強 |
| **UserPromptSubmit** | 用戶提交提示詞 | 輸入驗證、提示詞過濾 |
| **PreToolUse** | 任何工具執行前 | 驗證、批准閘道、記錄 |
| **PermissionRequest** | 顯示權限對話框 | 自動批准/拒絕流程 |
| **PostToolUse** | 工具成功執行後 | 自動格式化、通知、清理 |
| **PostToolUseFailure** | 工具執行失敗 | 錯誤處理、記錄 |
| **Notification** | 發送通知 | 警報、外部整合 |
| **SubagentStart** | 產生子代理 | 上下文注入、初始化 |
| **SubagentStop** | 子代理完成 | 結果驗證、記錄 |
| **Stop** | Claude 完成回應 | 摘要生成、清理任務 |
| **StopFailure** | API 錯誤結束回合 | 錯誤恢復、記錄 |
| **TeammateIdle** | 代理團隊成員閒置 | 工作分配、協調 |
| **TaskCompleted** | 任務標記完成 | 任務後處理 |
| **TaskCreated** | 透過 TaskCreate 建立任務 | 任務追蹤、記錄 |
| **ConfigChange** | 設定檔案變更 | 驗證、傳播 |
| **CwdChanged** | 工作目錄變更 | 目錄特定的設定 |
| **FileChanged** | 監控檔案變更 | 檔案監控、重建觸發 |
| **PreCompact** | 上下文壓縮前 | 狀態保存 |
| **PostCompact** | 壓縮完成後 | 壓縮後動作 |
| **WorktreeCreate** | 建立工作樹 | 環境設定、安裝相依性 |
| **WorktreeRemove** | 移除工作樹 | 清理、資源釋放 |
| **Elicitation** | MCP 伺服器請求使用者輸入 | 輸入驗證 |
| **ElicitationResult** | 使用者回應請求 | 回應處理 |
| **SessionEnd** | 工作階段終止 | 清理、最終記錄 |

### 常用鉤子

鉤子設定在 `~/.claude/settings.json` (使用者層級) 或 `.claude/settings.json` (專案層級)：

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
- `$CLAUDE_SESSION_ID` - 目前的會話識別碼
- `$CLAUDE_PROJECT_DIR` - 專案目錄路徑

### 最佳實踐

✅ **應該：**
- 保持鉤子快速 (< 1 秒)
- 使用鉤子進行驗證和自動化
- 妥善處理錯誤
- 使用絕對路徑

❌ **不應該：**
- 使鉤子具有互動性
- 使用鉤子進行長時間運作的任務
- 硬編碼憑證

**參閱**: [06-hooks/](06-hooks/) 以獲得詳細範例

---

## 檢查點與回溯

### 概述

檢查點允許您保存會話狀態並回溯到之前的點，從而可以安全地進行實驗和探索多種方法。

### 關鍵概念

| 概念 | 描述 |
|---------|-------------|
| **檢查點** | 包含訊息、檔案和上下文的會話狀態快照 |
| **回溯** | 返回到之前的檢查點，捨棄後續的變更 |
| **分支點** | 從此檢查點探索多種方法 |

### 存取檢查點

檢查點會隨著每個使用者提示自動建立。 要回溯：

```bash
# 雙擊 Esc 鍵以開啟檢查點瀏覽器
Esc + Esc

# 或使用斜線命令 /rewind
/rewind
```

當您選擇一個檢查點時，您可以從以下五個選項中選擇：
1. **恢復程式碼和對話** -- 將其恢復到該點
2. **恢復對話** -- 回溯訊息，保留目前程式碼
3. **恢復程式碼** -- 恢復檔案，保留對話
4. **總結從這裡開始** -- 將對話壓縮成摘要
5. **取消** -- 關閉

### 使用案例

| 情境 | 工作流程 |
|----------|----------|
| **探索方法** | 保存 → 嘗試 A → 保存 → 回溯 → 嘗試 B → 比較 |
| **安全重構** | 保存 → 重構 → 測試 → 如果失敗：回溯 |
| **A/B 測試** | 保存 → 設計 A → 保存 → 回溯 → 設計 B → 比較 |
| **錯誤恢復** | 發現問題 → 回溯到最後一個良好狀態 |

### 設定

```json
{
  "autoCheckpoint": true
}
```

**參閱**: [08-checkpoints/](08-checkpoints/) 以獲得詳細範例

## 進階功能

### 計畫模式

在編碼前建立詳細的實施計畫。

**啟用:**
```bash
/plan 實作使用者驗證系統
```

**優點:**
- 清晰的路線圖，包含時間估計
- 風險評估
- 有系統地將任務分解
- 檢閱和修改的機會

### 擴展思考

針對複雜問題進行深入推理。

**啟用:**
- 在會話期間使用 `Alt+T` (或 macOS 上的 `Option+T`) 切換
- 設置 `MAX_THINKING_TOKENS` 環境變數以進行程式化控制

```bash
# 透過環境變數啟用擴展思考
export MAX_THINKING_TOKENS=50000
claude -p "我們應該使用微服務還是單一應用程式？"
```

**優點:**
- 徹底分析權衡取捨
- 更好的架構決策
- 考慮邊緣案例
- 有系統地進行評估

### 背景任務

在不影響對話的情況下執行長時間操作。

**用法:**
```bash
使用者：執行測試於背景

Claude：已啟動任務 bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 檢查進度
/task show bg-1234   # 檢視輸出
/task cancel bg-1234 # 停止任務
```

### 權限模式

控制 Claude 可以做什麼。

| 模式 | 描述 | 使用案例 |
|------|-------------|----------|
| **default** | 標準權限，對於敏感動作會提示 | 一般開發 |
| **acceptEdits** | 自動接受檔案編輯，無需確認 | 值得信賴的編輯工作流程 |
| **plan** | 僅進行分析和規劃，不進行任何檔案修改 | 程式碼檢閱、架構規劃 |
| **auto** | 自動批准安全動作，僅對有風險的動作提示 | 平衡自主性和安全性 |
| **dontAsk** | 不進行任何確認提示，直接執行所有動作 | 經驗豐富的使用者、自動化 |
| **bypassPermissions** | 完全無限制的存取權，不進行任何安全檢查 | CI/CD 管道、受信任的腳本 |

**用法:**
```bash
claude --permission-mode plan          # 僅讀取分析
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動批准安全動作
claude --permission-mode dontAsk       # 不進行任何確認提示
```

### 無頭模式 (列印模式)

使用 `-p` (列印) 標誌，在沒有互動輸入的情況下執行 Claude Code，以進行自動化和 CI/CD。

**用法:**
```bash
# 執行特定任務
claude -p "執行所有測試"

# 傳輸輸入以進行分析
cat error.log | claude -p "解釋這個錯誤"

# CI/CD 整合 (GitHub Actions)
- name: AI 程式碼檢閱
  run: claude -p "檢閱 PR 變更並回報問題"

# JSON 輸出以進行腳本編寫
claude -p --output-format json "列出 src/ 中的所有函數"
```

### 排程任務

使用 `/loop` 命令，以重複的排程執行任務。

**用法:**
```bash
/loop every 30m "執行測試並回報失敗"
/loop every 2h "檢查相依性更新"
/loop every 1d "產生程式碼變更的每日摘要"
```

排程的任務會在背景中執行，並在完成時回報結果。 這些對於持續監控、定期檢查和自動化維護工作流程非常有用。

### Chrome Integration

Claude Code 可以與 Chrome 瀏覽器整合，用於網頁自動化任務。這使得能夠執行例如瀏覽網頁、填寫表單、截取螢幕截圖以及直接在您的開發工作流程中從網站中提取資料等功能。

### Session Management

管理多個工作階段。

**命令：**
```bash
/resume                # 繼續先前的對話
/rename "Feature"      # 命名目前的階段
/fork                  # 分支到一個新的階段
claude -c              # 繼續最近的對話
claude -r "Feature"    # 根據名稱/ID 繼續階段
```

### Interactive Features

**快捷鍵：**
- `Ctrl + R` - 搜尋命令歷史記錄
- `Tab` - 自動完成
- `↑ / ↓` - 命令歷史記錄
- `Ctrl + L` - 清除螢幕

**多行輸入：**
```bash
User: \
> 長且複雜的提示詞
> 跨越多行
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

**參閱**: [09-advanced-features/](09-advanced-features/) 以獲得完整的指南

---

## Resources

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [Anthropic Documentation](https://docs.anthropic.com)
- [MCP GitHub Servers](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---
*上次更新：2026 年 4 月 11 日*
*適用於 Claude Haiku 4.5、Sonnet 4.6 和 Opus 4.6*
*現在包含：鉤子、檢查點、規劃模式、擴展思考、背景任務、權限模式（6 種模式）、無頭模式、階段管理、自動記憶、代理團隊、排定任務、Chrome 整合、頻道、語音錄音和內建技能*

---
**上次更新：** 2026 年 4 月 11 日
**Claude Code 版本：** 2.1.101
**來源：**
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/skills
