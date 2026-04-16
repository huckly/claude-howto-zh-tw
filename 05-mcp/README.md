<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP (Model Context Protocol)

此資料夾包含關於 MCP 伺服器配置和與 Claude Code 搭配使用的全面文件和範例。

## 概述

MCP (Model Context Protocol) 是一種標準化方式，讓 Claude 能夠存取外部工具、API 和即時資料來源。與記憶體 (Memory) 相比，MCP 提供對變動資料的即時存取。

主要特點：
- 即時存取外部服務
- 即時資料同步
- 可擴展的架構
- 安全的驗證
- 基於工具的互動

## MCP 架構

```mermaid
graph TB
    A["Claude"]
    B["MCP 伺服器"]
    C["外部服務"]

    A -->|Request: list_issues| B
    B -->|Query| C
    C -->|Data| B
    B -->|Response| A

    A -->|Request: create_issue| B
    B -->|Action| C
    C -->|Result| B
    B -->|Response| A

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#e8f5e9,stroke:#333,color:#333
```

## MCP 生態系統

```mermaid
graph TB
    A["Claude"] -->|MCP| B["檔案系統<br/>MCP 伺服器"]
    A -->|MCP| C["GitHub<br/>MCP 伺服器"]
    A -->|MCP| D["資料庫<br/>MCP 伺服器"]
    A -->|MCP| E["Slack<br/>MCP 伺服器"]
    A -->|MCP| F["Google Docs<br/>MCP 伺服器"]

    B -->|File I/O| G["本機檔案"]
    C -->|API| H["GitHub 儲存庫"]
    D -->|Query| I["PostgreSQL/MySQL"]
    E -->|Messages| J["Slack 工作區"]
    F -->|Docs| K["Google Drive"]

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#f3e5f5,stroke:#333,color:#333
    style G fill:#e8f5e9,stroke:#333,color:#333
    style H fill:#e8f5e9,stroke:#333,color:#333
    style I fill:#e8f5e9,stroke:#333,color:#333
    style J fill:#e8f5e9,stroke:#333,color:#333
    style K fill:#e8f5e9,stroke:#333,color:#333
```

## MCP 安裝方法

Claude Code 支援多種傳輸協定，用於與 MCP 伺服器建立連線：

### HTTP 傳輸 (推薦)

```bash
# 基本 HTTP 連線
claude mcp add --transport http notion https://mcp.notion.com/mcp

# 帶有驗證標頭的 HTTP
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"
```

### Stdio 傳輸 (本機)

用於本機執行的 MCP 伺服器：

```bash
# 本機 Node.js 伺服器
claude mcp add --transport stdio myserver -- npx @myorg/mcp-server

# 帶有環境變數
claude mcp add --transport stdio myserver --env KEY=value -- npx server
```

### SSE 傳輸 (已過時)

Server-Sent Events 傳輸已過時，但仍支援，建議使用 `http`：

```bash
claude mcp add --transport sse legacy-server https://example.com/sse
```

### Windows 專屬注意事項

在原生 Windows (非 WSL) 上，請使用 `cmd /c` 對 npx 命令：

```bash
claude mcp add --transport stdio my-server -- cmd /c npx -y @some/package
```

### OAuth 2.0 驗證

Claude Code 支援 OAuth 2.0，用於需要驗證的 MCP 伺服器。當連接到 OAuth 啟用的伺服器時，Claude Code 會處理整個驗證流程：

```bash
# 連接到 OAuth 啟用的 MCP 伺服器 (互動式流程)
claude mcp add --transport http my-service https://my-service.example.com/mcp

# 預先設定 OAuth 憑證以進行非互動式設定
claude mcp add --transport http my-service https://my-service.example.com/mcp \
  --client-id "your-client-id" \
  --client-secret "your-client-secret" \
  --callback-port 8080
```

| 功能 | 描述 |
|---------|-------------|
| **互動式 OAuth** | 使用 `/mcp` 觸發瀏覽器基礎的 OAuth 流程 |
| **預先設定的 OAuth 用戶端** | 內建 OAuth 用戶端，用於常見服務，例如 Notion、Stripe 等 (v2.1.30+) |
| **預先設定的憑證** | `--client-id`、`--client-secret`、`--callback-port` 標誌，用於自動設定 |
| **Token 儲存** | Token 安全地儲存在您的系統金鑰串中 |
| **Step-up 驗證** | 支援特權操作的 step-up 驗證 |
| **Discovery 快取** | OAuth discovery 資訊快取，以加快重新連接速度 |
| **資訊超載** | 在伺服器設定的 `oauth` 物件中，使用 `oauth.authServerMetadataUrl` 來覆寫預設的 OAuth 資訊 discovery |

#### 覆寫 OAuth 資訊 Discovery

如果您的 MCP 伺服器在標準 OAuth 資訊端點 (`/.well-known/oauth-authorization-server`) 上傳回錯誤，但公開了可用的 OIDC 端點，您可以告知 Claude Code 從特定 URL 抓取 OAuth 資訊。在伺服器設定的 `oauth` 物件中設定 `authServerMetadataUrl`：

```json
{
  "mcpServers": {
    "my-server": {
      "type": "http",
      "url": "https://mcp.example.com/mcp",
      "oauth": {
        "authServerMetadataUrl": "https://auth.example.com/.well-known/openid-configuration"
      }
    }
  }
}
```

URL 必須使用 `https://`。此選項需要 Claude Code v2.1.64 或更高版本。

### Claude.ai MCP Connectors

在您的 Claude.ai 帳戶中配置的 MCP 伺服器，將自動在 Claude Code 中可用。這表示您透過 Claude.ai 網頁介面設定的任何 MCP 連線，都無需額外配置即可存取。

Claude.ai MCP 連接器也可用於 `--print` 模式 (v2.1.83+)，支援非互動式和腳本化使用。

若要停用 Claude.ai MCP 伺服器在 Claude Code 中，請將 `ENABLE_CLAUDEAI_MCP_SERVERS` 環境變數設定為 `false`：

```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude
```

> **注意:** 此功能僅適用於使用 Claude.ai 帳戶登入的使用者。

## MCP 設定流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as 設定檔案
    participant Service as 外部服務

    User->>Claude: 輸入 /mcp
    Claude->>Claude: 列出可用的 MCP 伺服器
    Claude->>User: 顯示選項
    User->>Claude: 選擇 GitHub MCP
    Claude->>Config: 更新設定
    Config->>Claude: 啟用連線
    Claude->>Service: 測試連線
    Service-->>Claude: 驗證成功
    Claude->>User: ✅ MCP 連線成功！
```

## MCP 工具搜尋

當 MCP 工具描述超過上下文中 10% 時，Claude Code 會自動啟用工具搜尋，以有效選擇正確的工具，而不會過度填滿模型上下文。

| 設定 | 值 | 描述 |
|---------|-------|-------------|
| `ENABLE_TOOL_SEARCH` | `auto` (預設) | 當工具描述超過上下文中 10% 時，自動啟用 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` | 以自訂的 `N` 個工具閾值自動啟用 |
| `ENABLE_TOOL_SEARCH` | `true` | 永遠啟用，不論工具數量 |
| `ENABLE_TOOL_SEARCH` | `false` | 停用；所有工具描述完整發送 |

> **注意:** 工具搜尋需要 Sonnet 4 或更高版本，或 Opus 4 或更高版本。 Haiku 模型不支援工具搜尋。

## 動態工具更新

Claude Code 支援 MCP 的 `list_changed` 通知。當 MCP 伺服器動態新增、移除或修改其可用的工具時，Claude Code 會收到更新並自動調整其工具清單——無需重新連接或重新啟動。

## MCP Apps

MCP Apps 是第一個官方 MCP 擴充功能，允許 MCP 工具呼叫傳回直接在聊天介面中呈現的互動式 UI 元件。與純文字回應不同，MCP 伺服器可以傳遞豐富的儀表板、表單、資料視覺化和多步驟工作流程——所有內容都直接顯示在對話中，無需離開對話。

## MCP 提示

MCP 伺服器可以使用互動式對話 (v2.1.49+) 請求使用者提供結構化輸入。這允許 MCP 伺服器在工作流程中請求額外資訊——例如，提示使用者確認、從選項清單中選擇或填寫必要欄位——為 MCP 伺服器互動增加互動性。

## 工具描述和指令上限

從 v2.1.84 開始，Claude Code 對每個 MCP 伺服器的工具描述和指令強制執行 **2 KB 的上限**。這可防止個別伺服器使用過於冗長的工具定義來消耗過多的上下文，從而減少上下文膨脹並保持互動效率。

## MCP 提示作為斜線命令

MCP 伺服器可以將提示公開為 Claude Code 中的斜線命令。提示可以使用以下命名慣例訪問：

```
/mcp__<server>__<prompt>
```

例如，如果名為 `github` 的伺服器公開了一個名為 `review` 的提示，您可以使用 `/mcp__github__review` 呼叫它。

## 伺服器去重

當相同的 MCP 伺服器在多個範圍 (本機、專案、使用者) 中被定義時，本機設定具有優先權。 這允許您使用本機自訂設定來覆寫專案級別或使用者級別的 MCP 設定，而不會發生衝突。

## 使用 @ 提及符號的 MCP 資源

您可以使用 `@` 提及符號語法在提示詞中直接引用 MCP 資源：

```
@伺服器名稱:protocol://resource/path
```

例如，要引用特定的資料庫資源：

```
@database:postgres://mydb/users
```

這允許 Claude 擷取並將 MCP 資源內容內嵌到會話上下文中。

## MCP 範圍

MCP 設定可以儲存在不同的範圍中，具有不同的共享程度：

| 範圍 | 位置 | 描述 | 共享對象 | 需要批准 |
|-------|----------|-------------|-------------|------------------|
| **本機** (預設) | `~/.claude.json` (在專案路徑下) | 僅限目前使用者和目前專案 (舊版本中稱為 `project`) | 僅限您 | 否 |
| **專案** | `.mcp.json` | 檢查到 git 儲存庫 | 團隊成員 | 是 (首次使用) |
| **使用者** | `~/.claude.json` | 跨所有專案可用 (舊版本中稱為 `global`) | 僅限您 | 否 |

### 使用專案範圍

將專案特定的 MCP 設定儲存在 `.mcp.json` 中：

```json
{
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.github.com/mcp"
    }
  }
}
```

團隊成員將在首次使用專案 MCP 時看到批准提示詞。

## MCP 配置管理

### 增加 MCP 伺服器

```bash
# 增加基於 HTTP 的伺服器
claude mcp add --transport http github https://api.github.com/mcp

# 增加本機 stdio 伺服器
claude mcp add --transport stdio database -- npx @company/db-server

# 列出所有 MCP 伺服器
claude mcp list

# 取得特定伺服器的詳細資訊
claude mcp get github

# 移除 MCP 伺服器
claude mcp remove github

# 重新設定專案特定的核准選項
claude mcp reset-project-choices

# 從 Claude Desktop 匯入
claude mcp add-from-claude-desktop
```

## 可用的 MCP 伺服器表格

| MCP 伺服器 | 目的 | 常用工具 | 認證 | 即時 |
|------------|---------|--------------|------|-----------|
| **檔案系統** | 檔案操作 | 讀取、寫入、刪除 | 作業系統權限 | ✅ 是 |
| **GitHub** | 儲存庫管理 | list_prs, create_issue, push | OAuth | ✅ 是 |
| **Slack** | 團隊溝通 | send_message, list_channels | Token | ✅ 是 |
| **資料庫** | SQL 查詢 | query, insert, update | 憑證 | ✅ 是 |
| **Google 文件** | 文件存取 | 讀取、寫入、分享 | OAuth | ✅ 是 |
| **Asana** | 專案管理 | create_task, update_status | API 金鑰 | ✅ 是 |
| **Stripe** | 支付資料 | list_charges, create_invoice | API 金鑰 | ✅ 是 |
| **記憶** | 持續性記憶 | 儲存、檢索、刪除 | 本機 | ❌ 否 |

## 實用範例

### 範例 1：GitHub MCP 配置

**檔案：** `.mcp.json` (專案根目錄)

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

#### 提取請求管理
- `list_prs` - 列出儲存庫中的所有 PR
- `get_pr` - 取得 PR 詳細資訊，包括差異
- `create_pr` - 建立新的 PR
- `update_pr` - 更新 PR 描述/標題
- `merge_pr` - 將 PR 合併到主要分支
- `review_pr` - 增加審查評論

**範例請求：**
```
/mcp__github__get_pr 456

# 傳回：
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

#### 問題管理
- `list_issues` - 列出所有問題
- `get_issue` - 取得問題詳細資訊
- `create_issue` - 建立新的問題
- `close_issue` - 關閉問題
- `add_comment` - 增加對問題的評論

#### 儲存庫資訊
- `get_repo_info` - 儲存庫詳細資訊
- `list_files` - 檔案樹狀結構
- `get_file_content` - 讀取檔案內容
- `search_code` - 搜尋程式碼

#### 提交操作
- `list_commits` - 提交歷史記錄
- `get_commit` - 特定提交的詳細資訊
- `create_commit` - 建立新的提交

**設定：**
```bash
export GITHUB_TOKEN="your_github_token"
# 或者使用 CLI 直接新增：
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

### 配置中的環境變數擴充

MCP 配置支援環境變數擴充，並提供預設值。 `${VAR}` 和 `${VAR:-default}` 語法適用於以下欄位：`command`、`args`、`env`、`url` 和 `headers`。

```json
{
  "mcpServers": {
    "api-server": {
      "type": "http",
      "url": "${API_BASE_URL:-https://api.example.com}/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}",
        "X-Custom-Header": "${CUSTOM_HEADER:-default-value}"
      }
    },
    "local-server": {
      "command": "${MCP_BIN_PATH:-npx}",
      "args": ["${MCP_PACKAGE:-@company/mcp-server}"],
      "env": {
        "DB_URL": "${DATABASE_URL:-postgresql://localhost/dev}"
      }
    }
  }
}
```

變數會在執行時擴展：
- `${VAR}` - 使用環境變數，如果未設定則會出錯
- `${VAR:-default}` - 使用環境變數，如果未設定則會回退到預設值

### 範例 2：資料庫 MCP 設定

**設定:**

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

**範例使用:**

```markdown
User: 取得所有訂單數大於 10 的使用者

Claude: 我會查詢您的資料庫以取得這些資訊。

# 使用 MCP 資料庫工具：
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 成果：
- Alice: 15 訂單
- Bob: 12 訂單
- Charlie: 11 訂單
```

**設定**:
```bash
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
# 或使用 CLI 直接新增：
claude mcp add --transport stdio database -- npx @modelcontextprotocol/server-database
```

### 範例 3：多 MCP 工作流程

**情境：每日報告產生**

```markdown
# 每日報告工作流程，使用多個 MCP
```

## 設定

1. GitHub MCP - 抓取 PR 指標
2. Database MCP - 查詢銷售數據
3. Slack MCP - 發布報告
4. Filesystem MCP - 儲存報告

## 工作流程

### 步驟 1：抓取 GitHub 數據
/mcp__github__list_prs completed:true last:7days

輸出：
- 總共 PR 數量：42
- 平均合併時間：2.3 小時
- 審查時間：1.1 小時

### 步驟 2：查詢資料庫
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

輸出：
- 銷售額：247
- 營收：$12,450

### 步驟 3：產生報告
將數據合併到 HTML 報告

### 步驟 4：儲存到檔案系統
將 report.html 寫入 /reports/

### 步驟 5：發布到 Slack
將摘要發送到 #daily-reports 頻道

最終輸出：
✅ 報告已產生並發布
📊 本週合併了 47 個 PR
💰 每日銷售額 $12,450

**設定**:
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="your_slack_token"
# Add each MCP server via the CLI or configure them in .mcp.json
```

### 範例 4：檔案系統 MCP 運作

**設定**:

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

**可用的運作**:

| 運作 | 命令 | 目的 |
|-----------|---------|---------|
| 列出檔案 | `ls ~/projects` | 顯示目錄內容 |
| 讀取檔案 | `cat src/main.ts` | 讀取檔案內容 |
| 寫入檔案 | `create docs/api.md` | 建立新檔案 |
| 編輯檔案 | `edit src/app.ts` | 修改檔案 |
| 搜尋 | `grep "async function"` | 在檔案中搜尋 |
| 刪除 | `rm old-file.js` | 刪除檔案 |

**設定**:
```bash
# Use the CLI to add directly:
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /home/user/projects
```

## MCP 與記憶體：決策矩陣

```mermaid
graph TD
    A["需要外部資料嗎？"]
    A -->|不| B["使用記憶體"]
    A -->|是| C["資料是否經常變動？"]
    C -->|不/很少| B
    C -->|是/經常| D["使用 MCP"]

    B -->|儲存| E["偏好設定<br/>上下文<br/>歷史記錄"]
    D -->|存取| F["即時 API<br/>資料庫<br/>服務"]

    style A fill:#fff3e0,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#fff3e0,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#e8f5e9,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
```

## 請求/回應模式

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP 伺服器
    participant DB as 資料庫

    App->>MCP: 請求: "SELECT * FROM users WHERE id=1"
    MCP->>DB: 執行查詢
    DB-->>MCP: 結果集
    MCP-->>App: 回傳解析後的資料
    App->>App: 處理結果
    App->>App: 繼續任務

    Note over MCP,DB: 即時存取<br/>不快取
```

## 環境變數

將敏感憑證儲存在環境變數中：

```bash
# ~/.bashrc or ~/.zshrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="xoxb-xxxxxxxxxxxxx"
```

然後在 MCP 設定檔中參考它們：

```json
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

## 將 Claude 作為 MCP 伺服器 (`claude mcp serve`)

Claude Code 本身可以作為其他應用程式的 MCP 伺服器。 這讓外部工具、編輯器和自動化系統能夠透過標準 MCP 協定使用 Claude 的功能。

```bash
# 以 stdio 作為 MCP 伺服器啟動 Claude Code
claude mcp serve
```

其他應用程式現在可以像連接到任何 stdio 基礎的 MCP 伺服器一樣連接到此伺服器。 例如，要在另一個 Claude Code 實例中將 Claude Code 作為 MCP 伺服器新增：

```bash
claude mcp add --transport stdio claude-agent -- claude mcp serve
```

這對於構建多代理工作流程非常有用，其中一個 Claude 實例協調另一個。

## 企業版管理的 MCP 設定

對於企業部署，IT 管理員可以透過 `managed-mcp.json` 設定檔案來強制執行 MCP 伺服器策略。這個檔案提供對哪些 MCP 伺服器可以組織範圍內許可或封鎖的獨家控制權。

**位置：**
- macOS: `/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux: `~/.config/ClaudeCode/managed-mcp.json`
- Windows: `%APPDATA%\ClaudeCode\managed-mcp.json`

**功能：**
- `allowedMcpServers` -- 許可伺服器的白名單
- `deniedMcpServers` -- 封鎖伺服器的黑名單
- 支援透過伺服器名稱、命令和 URL 模式進行比對
- 組織範圍內的 MCP 策略在使用者設定之前強制執行
- 防止未經授權的伺服器連線

**範例設定：**

```json
{
  "allowedMcpServers": [
    {
      "serverName": "github",
      "serverUrl": "https://api.github.com/mcp"
    },
    {
      "serverName": "company-internal",
      "serverCommand": "company-mcp-server"
    }
  ],
  "deniedMcpServers": [
    {
      "serverName": "untrusted-*"
    },
    {
      "serverUrl": "http://*"
    }
  ]
}
```

> **注意：** 當 `allowedMcpServers` 和 `deniedMcpServers` 都與伺服器比對時，封鎖規則優先。

## 外掛程式提供的 MCP 伺服器

外掛程式可以將其自己的 MCP 伺服器封裝，使其在安裝外掛程式時自動可用。外掛程式提供的 MCP 伺服器可以透過兩種方式定義：

1. **獨立的 `.mcp.json`** -- 將 `.mcp.json` 檔案放在外掛程式的根目錄中
2. **內嵌在 `plugin.json` 中** -- 在外掛程式的資訊清單中直接定義 MCP 伺服器

使用 `${CLAUDE_PLUGIN_ROOT}` 變數來參考相對於外掛程式安裝目錄的路徑：

```json
{
  "mcpServers": {
    "plugin-tools": {
      "command": "node",
      "args": ["${CLAUDE_PLUGIN_ROOT}/dist/mcp-server.js"],
      "env": {
        "CONFIG_PATH": "${CLAUDE_PLUGIN_ROOT}/config.json"
      }
    }
  }
}
```

## 子代理範圍的 MCP

MCP 伺服器可以使用 `mcpServers:` 鍵在代理 frontmatter 中內嵌定義，將其範圍限定在特定的子代理，而不是整個專案。 當代理需要存取特定的 MCP 伺服器，而其他代理在工作流程中不需要時，這非常有用。

```yaml
---
mcpServers:
  my-tool:
    type: http
    url: https://my-tool.example.com/mcp
---

您是一個具有存取 my-tool 以進行專用操作的代理。
```

子代理範圍的 MCP 伺服器僅在該代理的執行上下文中可用，並且不與父代理或兄弟代理共享。

## MCP 輸出限制

Claude Code 對 MCP 工具的輸出強制執行限制，以防止上下文溢出：

| 限制 | 閾值 | 行為 |
|-------|-----------|----------|
| **警告** | 10,000 token | 顯示警告，指出輸出過大 |
| **預設最大值** | 25,000 token | 超出此限制時會截斷輸出 |
| **磁碟持久性** | 50,000 字元 | 超出 50K 字元的工具結果會儲存到磁碟 |

最大輸出限制可以透過 `MAX_MCP_OUTPUT_TOKENS` 環境變數進行配置：

```bash
# 將最大輸出增加到 50,000 token
export MAX_MCP_OUTPUT_TOKENS=50000
```

## 使用程式碼執行解決上下文膨脹

隨著 MCP 的採用規模擴大，連接到數十個伺服器和數千個工具會造成一個顯著的挑戰：**上下文膨脹**。 這毫無疑問是 MCP 規模化時面臨的最大問題，而 Anthropic 的工程團隊提出了一種優雅的解決方案：使用程式碼執行，而不是直接調用工具。

> **來源**: [使用 MCP 的程式碼執行：構建更高效的代理](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic 工程部落格

### 問題：兩個來源的 token 浪費

**1. 工具定義過載上下文視窗**

大多數 MCP 客戶端會預先載入所有工具定義。 當連接到數千個工具時，模型必須處理數十萬個 token，才能讀取使用者的請求。

**2. 中間結果消耗額外的 token**

每個中間工具結果都會經過模型的上下文。 考慮將 Google Drive 中的會議記錄傳輸到 Salesforce — 完整的記錄會經過上下文 **兩次**：一次在讀取時，再次在寫入目的地時。 2 小時的會議記錄可能意味著 50,000+ 個額外的 token。

```mermaid
graph LR
    A["模型"] -->|"工具調用：getDocument"| B["MCP 伺服器"]
    B -->|"完整記錄 (50K token)"| A
    A -->|"工具調用：updateRecord<br/>(重新發送完整記錄)"| B
    B -->|"確認"| A

    style A fill:#ffcdd2,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
```

### 解決方案：MCP 工具作為程式碼 API

與將工具定義和結果傳遞到上下文視窗不同，代理 **編寫程式碼**，該程式碼將調用 MCP 工具作為 API。 程式碼在沙盒執行環境中執行，只有最終結果才會返回到模型。

```mermaid
graph LR
    A["模型"] -->|"編寫程式碼"| B["程式碼執行環境"]
    B -->|"直接呼叫工具"| C["MCP 伺服器"]
    C -->|"資料保留在執行環境"| B
    B -->|"僅最終結果 (最少 token)"| A

    style A fill:#c8e6c9,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
```

#### 運作方式

MCP 工具以類型函數的檔案樹呈現：

```
servers/
├── google-drive/
│   ├── getDocument.ts
│   └── index.ts
├── salesforce/
│   ├── updateRecord.ts
│   └── index.ts
└── ...
```

每個工具檔案都包含一個類型封裝器：

```typescript
// ./servers/google-drive/getDocument.ts
import { callMCPTool } from "../../../client.js";

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

export async function getDocument(
  input: GetDocumentInput
): Promise<GetDocumentResponse> {
  return callMCPTool<GetDocumentResponse>(
    'google_drive__get_document', input
  );
}
```

代理程式接著編寫程式碼來協調工具：

```typescript
import * as gdrive from './servers/google-drive';
import * as salesforce from './servers/salesforce';

// 資料直接在工具之間流動 — 從未通過模型
const transcript = (
  await gdrive.getDocument({ documentId: 'abc123' })
).content;

await salesforce.updateRecord({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  data: { Notes: transcript }
});
```

**結果：token 使用量從 ~150,000 降低到 ~2,000 — 減少了 98.7%。**

### 關鍵優點

| 優點 | 描述 |
|---------|-------------|
| **逐步揭露** | 代理程式瀏覽檔案系統以載入其需要的工具定義，而不是一次性載入所有工具 |
| **高效能上下文** | 資料在將其傳回模型之前，在執行環境中進行過濾/轉換 |
| **強大的流程控制** | 迴圈、條件和錯誤處理在程式碼中執行，無需通過模型來回傳遞 |
| **保護隱私** | 中間資料 (PII、敏感記錄) 保留在執行環境中；從未進入模型上下文 |
| **狀態持久性** | 代理程式可以將中間結果儲存到檔案中，並建立可重複使用的技能函數 |

#### 範例：過濾大型資料集

```typescript
// 沒有程式碼執行 — 所有 10,000 列都流經上下文
// 工具呼叫：gdrive.getSheet(sheetId: 'abc123')
//   -> 傳回 10,000 列到上下文中

// 具有程式碼執行 — 在執行環境中進行過濾
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(
  row => row["Status"] === 'pending'
);
console.log(`找到 ${pendingOrders.length} 待處理訂單`);
console.log(pendingOrders.slice(0, 5)); // 只有 5 列到達模型
```

#### 範例：迴圈而無需來回傳遞

```typescript
// 輪詢部署通知 — 完全在程式碼中執行
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({
```

```
channel: 'C123456'
  });
  found = messages.some(
    m => m.text.includes('deployment complete')
  );
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('Deployment notification received');
```

### 權衡考量

程式碼執行會引入自身的複雜性。執行代理生成的程式碼需要：

- **安全的沙箱執行環境**，具有適當的資源限制
- 執行中程式碼的 **監控和記錄**
- 與直接工具呼叫相比，額外的 **基礎設施開銷**

降低 token 成本、降低延遲、改進工具組成等優點，應與這些實施成本相權衡。對於只有少量 MCP 伺服器的代理，直接工具呼叫可能更簡單。對於大規模代理（數十台伺服器、數百種工具），程式碼執行是顯著的改進。

### MCPorter：用於 MCP 工具組成的執行時間

[MCPorter](https://github.com/steipete/mcporter) 是一個 TypeScript 執行時間和 CLI 工具包，可以實用地呼叫 MCP 伺服器，並通過選擇性工具暴露和類型化封裝來減少上下文膨脹。

**解決方案：** 與預先從所有 MCP 伺服器載入所有工具定義不同，MCPorter 讓您能夠按需發現、檢查和呼叫特定工具，從而保持上下文簡潔。

**主要功能：**

| 功能 | 描述 |
|---------|-------------|
| **零配置發現** | 自動從 Cursor、Claude、Codex 或本地配置中發現 MCP 伺服器 |
| **類型化工具客戶端** | `mcporter emit-ts` 生成 `.d.ts` 介面和可立即使用的封裝 |
| **可組合 API** | `createServerProxy()` 將工具暴露為駝峰式方法，並具有 `.text()`、`.json()`、`.markdown()` 輔助工具 |
| **CLI 生成** | `mcporter generate-cli` 將任何 MCP 伺服器轉換為獨立的 CLI，具有 `--include-tools` / `--exclude-tools` 過濾器 |
| **參數隱藏** | 可選參數預設保持隱藏，減少模式冗餘 |

**安裝：**

```bash
npx mcporter list          # 無需安裝 — 立即發現伺服器
pnpm add mcporter          # 增加到專案
brew install steipete/tap/mcporter  # macOS 透過 Homebrew
```

**範例 — 在 TypeScript 中組合工具：**

```typescript
import { createRuntime, createServerProxy } from "mcporter";

const runtime = await createRuntime();
const gdrive = createServerProxy(runtime, "google-drive");
const salesforce = createServerProxy(runtime, "salesforce");

// 資料在工具之間流動，無需經過模型上下文
const doc = await gdrive.getDocument({ documentId: "abc123" });
await salesforce.updateRecord({
  objectType: "SalesMeeting",
  recordId: "00Q5f000001abcXYZ",
  data: { Notes: doc.text() }
});
```

**範例 — CLI 工具呼叫：**

```bash
# 呼叫特定工具
npx mcporter call linear.create_comment issueId:ENG-123 body:'Looks good!'

# 列出可用的伺服器和工具
npx mcporter list
```

MCPorter 通過提供用於將 MCP 工具作為類型化 API 呼叫的執行時間基礎設施，補充了上述程式碼執行方法，使將中間資料保持在模型上下文之外變得容易。
```

## 最佳實務

### 安全考量

#### 應該做 ✅
- 使用環境變數來儲存所有憑證
- 定期更換 token 和 API 金鑰（建議每月）
- 盡可能使用唯讀 token
- 限制 MCP 伺服器存取範圍到最低所需
- 監控 MCP 伺服器使用量和存取記錄
- 對於外部服務，如果有的話，使用 OAuth
- 實作對 MCP 請求的速率限制
- 在生產環境中使用之前測試 MCP 連線
- 記錄所有活躍的 MCP 連線
- 保持 MCP 伺服器套件更新

#### 不應該做 ❌
- 不要將憑證硬編碼到設定檔中
- 不要將 token 或機密資訊提交到 git
- 不要將 token 在團隊聊天或電子郵件中分享
- 不要使用個人 token 進行團隊專案
- 不要授予不必要的權限
- 不要忽略驗證錯誤
- 不要公開 MCP 端點
- 不要以 root/admin 權限執行 MCP 伺服器
- 不要將敏感資料快取在記錄中
- 不要停用驗證機制

### 設定最佳實務

1. **版本控制**: 將 `.mcp.json` 放在 git 中，但使用環境變數來儲存機密資訊
2. **最小權限**: 為每個 MCP 伺服器授予所需的最低權限
3. **隔離**: 盡可能在不同的處理程序中執行不同的 MCP 伺服器
4. **監控**: 記錄所有 MCP 請求和錯誤，以用於審計記錄
5. **測試**: 在部署到生產環境之前測試所有 MCP 設定

### 效能提示

- 在應用程式層級快取經常存取的資料
- 使用針對性更強的 MCP 查詢以減少資料傳輸
- 監控 MCP 運作的回應時間
- 考慮對外部 API 進行速率限制
- 在執行多個運作時使用批次處理

## 安裝說明

### 預備條件
- 已安裝 Node.js 和 npm
- 已安裝 Claude Code CLI
- 對於外部服務的 API token/憑證

### 逐步設定

1. **新增你的第一個 MCP 伺服器** 使用 CLI (例如：GitHub):
```bash
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

   或在你的專案根目錄中建立一個 `.mcp.json` 檔案:
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

2. **設定環境變數:**
```bash
export GITHUB_TOKEN="your_github_personal_access_token"
```

3. **測試連線:**
```bash
claude /mcp
```

4. **使用 MCP 工具:**
```bash
/mcp__github__list_prs
/mcp__github__create_issue "Title" "Description"
```

### 針對特定服務的安裝

**GitHub MCP:**
```bash
npm install -g @modelcontextprotocol/server-github
```

**Database MCP:**
```bash
npm install -g @modelcontextprotocol/server-database
```

**Filesystem MCP:**
```bash
npm install -g @modelcontextprotocol/server-filesystem
```

**Slack MCP:**
```bash
npm install -g @modelcontextprotocol/server-slack
```

## 疑難排解

### MCP 伺服器未找到
```bash
# 驗證 MCP 伺服器是否已安裝
npm list -g @modelcontextprotocol/server-github

# 若未安裝，請安裝
npm install -g @modelcontextprotocol/server-github
```

### 驗證失敗
```bash
# 驗證環境變數是否已設定
echo $GITHUB_TOKEN

# 若需要，請重新匯出
export GITHUB_TOKEN="your_token"

# 驗證 token 是否具有正確的權限
# 在這裡檢查 GitHub token 權限範圍：https://github.com/settings/tokens
```

### 連線逾時
- 檢查網路連線：`ping api.github.com`
- 驗證 API 端點是否可存取
- 檢查 API 的速率限制
- 嘗試在設定中增加逾時時間
- 檢查是否有防火牆或代理程式問題

### MCP 伺服器崩潰
- 檢查 MCP 伺服器記錄檔：`~/.claude/logs/`
- 驗證所有環境變數是否已設定
- 確保檔案權限正確
- 嘗試重新安裝 MCP 伺服器套件
- 檢查是否有相同埠上的衝突處理程序

## 相關概念

### 記憶體 vs MCP

- **記憶體**: 儲存持續、不變動的資料（偏好設定、上下文、歷史記錄）
- **MCP**: 存取即時、變動的資料（API、資料庫、即時服務）

### 何時使用哪一種

- **使用記憶體** 時：使用者偏好設定、對話歷史記錄、學習到的上下文
- **使用 MCP** 時：目前的 GitHub issue、即時資料庫查詢、即時資料

### 與其他 Claude 功能的整合

- 將 MCP 與記憶體結合以獲得豐富的上下文
- 在提示詞中使用 MCP 工具以獲得更好的推理
- 利用多個 MCP 進行複雜的工作流程

## 額外資源

- [官方 MCP 文件](https://code.claude.com/docs/en/mcp)
- [MCP 協定規格](https://modelcontextprotocol.io/specification)
- [MCP GitHub 存放庫](https://github.com/modelcontextprotocol/servers)
- [可用的 MCP 伺服器](https://github.com/modelcontextprotocol/servers)
- [MCPorter](https://github.com/steipete/mcporter) — 用於呼叫 MCP 伺服器的 TypeScript 執行時間和 CLI，無需額外程式碼
- [使用 MCP 進行程式碼執行](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic 的工程部落格，說明如何解決上下文膨脹問題
- [Claude Code CLI 參考](https://code.claude.com/docs/en/cli-reference)
- [Claude API 文件](https://docs.anthropic.com)

---
**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/mcp
**相容模型**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
