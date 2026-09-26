<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP 伺服器（Model Context Protocol）

此資料夾包含關於 MCP 伺服器設定以及在 Claude Code 中使用的完整文件與範例。

## 概述

MCP (Model Context Protocol) 是一種標準化的方式，讓 Claude 能夠存取外部工具、API 與即時資料來源。與記憶（Memory）不同，MCP 提供對變動資料的即時存取。

關鍵特性：
- 即時存取外部服務
- 即時資料同步
- 可擴展架構
- 安全的身分驗證
- 基於工具的互動

## MCP 架構

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

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#e8f5e9,stroke:#333,color:#333
```

## MCP 生態系統

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

Claude Code 支援多種用於 MCP 伺服器連線的傳輸協定：

### HTTP 傳輸 (推薦)

```bash
# 基本 HTTP 連線
claude mcp add --transport http notion https://mcp.notion.com/mcp

# 帶有驗證標頭的 HTTP
claude mcp add --transport http secure-api https://api.example.com/mcp \
  --header "Authorization: Bearer your-token"
```

### Stdio 傳輸 (本地)

用於在本地執行的 MCP 伺服器：

```bash
# 本地 Node.js 伺服器
claude mcp add --transport stdio myserver -- npx @myorg/mcp-server

# 使用環境變數
claude mcp add --transport stdio myserver --env KEY=value -- npx server
```

#### stdio 伺服器的 `CLAUDE_PROJECT_DIR`（v2.1.139+）

每個 MCP stdio 伺服器在啟動時，其環境中都已設定 `CLAUDE_PROJECT_DIR=<repo 根目錄的絕對路徑>`——與 hooks 使用的慣例相同。外掛及專案的 `.mcp.json` 檔案可在 `command`、`args` 和 `env` 的值中引用 `${CLAUDE_PROJECT_DIR}`，並在 `execve()` 之前完成替換：

```json
{
  "mcpServers": {
    "repo-tools": {
      "type": "stdio",
      "command": "node",
      "args": ["${CLAUDE_PROJECT_DIR}/.claude/mcp/repo-tools.js"],
      "env": {
        "REPO_ROOT": "${CLAUDE_PROJECT_DIR}"
      }
    }
  }
}
```

當您的 stdio 伺服器需要相對於專案根目錄讀取檔案，且無論 Claude Code 從哪個位置啟動皆適用時，請使用此設定。

stdio MCP 伺服器也會收到 `CLAUDE_CODE_SESSION_ID`（與傳給 hooks 和 Bash 的值相同），包括使用 `--resume` 恢復工作階段時 (v2.1.163+)。

### SSE 傳輸 (已棄用)

Server-Sent Events 傳輸已棄用並改由 `http` 取代，但目前仍受支援：

```bash
claude mcp add --transport sse legacy-server https://example.com/sse
```

### WebSocket 傳輸 (`ws`)

WebSocket 伺服器會維持持久的雙向連線，適合會主動向 Claude 推送事件的遠端 MCP 伺服器。若您的伺服器只回應請求，請改用 HTTP，因為 HTTP 支援 OAuth 與 `claude mcp add --transport` 旗標，而 WebSocket 兩者皆不支援。

由於 `--transport` 不接受 `ws`，請在 `.mcp.json` 中或透過 `claude mcp add-json` 進行設定：

```json
{
  "type": "ws",
  "url": "wss://mcp.example.com/socket",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

`type: "ws"` 項目接受與 `http` 相同的 `url`、`headers`、`headersHelper`、`timeout` 與 `alwaysLoad` 欄位。驗證方式**僅限標頭** — WebSocket 伺服器沒有 OAuth 流程。

> **注意**：WebSocket 伺服器不會出現在 `claude mcp list` 的輸出中。請使用 `claude mcp get <name>` 或 `/mcp` 面板來檢查它們。

與 HTTP 和 SSE 一樣，WebSocket 連線使用 5 分鐘的閒置時間窗；stdio 與 WebSocket 沒有逐請求的計時器。沒有 `type` 的 `url` 項目會產生錯誤，並指出 `"http"`、`"sse"` 與 `"ws"` 為有效值。

### 工作階段工作目錄（roots/list）

MCP 伺服器可以探索工作階段的工作目錄：啟動目錄以及所有 `--add-dir`/`additionalDirectories` 項目會透過 MCP `roots/list` 請求回傳，且每當該集合變更時，會送出 `notifications/roots/list_changed` 通知 (v2.1.203)。閒置逾時現在也適用於 stdio 伺服器（30 分鐘），而每個伺服器的 `timeout` 會作為閒置時間的下限 (v2.1.203)。

### Windows 特定注意事項

在原生 Windows (非 WSL) 上，請針對 npx 指令使用 `cmd /c`：

```bash
claude mcp add --transport stdio my-server -- cmd /c npx -y @some/package
```

### OAuth 2.0 驗證

Claude Code 支援需要 OAuth 2.0 的 MCP 伺服器。當連線至啟用了 OAuth 的伺服器時，Claude Code 會處理整個驗證流程：

```bash
# 連線至啟用了 OAuth 的 MCP 伺服器 (互動式流程)
claude mcp add --transport http my-service https://my-service.example.com/mcp

# 為非互動式設定預先配置 OAuth 憑證
claude mcp add --transport http my-service https://my-service.example.com/mcp \
  --client-id "your-client-id" \
  --client-secret "your-client-secret" \
  --callback-port 8080
```

| 功能 | 說明 |
|---------|-------------|
| **互動式 OAuth** | 使用 `/mcp` 觸發基於瀏覽器的 OAuth 流程 |
| **預先設定的 OAuth 用戶端** | 內建用於 Notion、Stripe 等常見服務的 OAuth 用戶端 (v2.1.30+) |
| **預先設定的憑證** | 提供 `--client-id`、`--client-secret`、`--callback-port` 旗標以進行自動化設定 |
| **token 儲存** | token 會安全地儲存在您的系統鑰匙圈 (keychain) 中 |
| **Step-up 驗證** | 支援針對特權操作的 step-up 驗證 |
| **探索快取** | OAuth 探索中繼資料 (discovery metadata) 會被快取以實現更快的重新連線 |
| **中繼資料覆寫** | 在 `.mcp.json` 中使用 `oauth.authServerMetadataUrl` 來覆寫預設的 OAuth 中繼資料探索 |

#### 覆寫 OAuth 中繼資料探索

如果您的 MCP 伺服器在標準 OAuth 中繼資料端點 (`/.well-known/oauth-authorization-server`) 回傳錯誤，但卻提供了一個可用的 OIDC 端點，您可以指示 Claude Code 從特定 URL 獲取 OAuth 中繼資料。請在伺服器設定的 `oauth` 物件中設定 `authServerMetadataUrl`：

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

該 URL 必須使用 `https://`。此選項需要 Claude Code v2.1.64 或更高版本。

#### 驗證啟動通知與動態標頭重新整理 (v2.1.193)

- **啟動驗證通知 (v2.1.193+)**：啟動時，Claude Code 會顯示一則通知，列出仍需驗證的 MCP 伺服器，避免需要登入的伺服器在無聲無息中無法運作。
- **`headersHelper` 自動重新整理 (v2.1.193+)**：若您透過 `headersHelper` 提供自訂驗證，當伺服器回傳 HTTP 401 或 403 時，helper 會自動重新呼叫。憑證會即時重新整理，無需手動重新連線。請參閱 [Use dynamic headers for custom authentication](https://code.claude.com/docs/en/mcp)。

> **警告** (v2.1.238)：專案 `.mcp.json` 中的 `headersHelper`，以及專案或 `--add-dir` 代理檔案中的內嵌 MCP 伺服器，現在要求該資料夾的信任對話框已被接受 — 包括在 `claude -p` 下。這類 helper 也會在**不繼承憑證環境變數**的情況下執行；使用者、受管與 claude.ai 範圍的 helper 則改為從 Claude 設定目錄執行。依賴繼承憑證或在未受信任狀態下執行的專案設定將會停止運作，直到您接受信任對話框並以其他方式提供憑證。

### Claude.ai MCP Connectors

在您的 Claude.ai 帳戶中設定的 MCP servers 會自動在 Claude Code 中可用。這意味著您透過 Claude.ai 網頁介面建立的任何 MCP 連線都無需額外設定即可直接存取。

Claude.ai MCP connectors 也可用於 `--print` 模式（v2.1.83+），使其能夠進行非互動式與腳本化使用。

> **啟動注意（v2.1.117+）：** 當本地與 claude.ai MCP 伺服器同時設定時，預設改為並行連線（先前為序列），可在多個伺服器同時使用時降低啟動延遲。

若要在 Claude Code 中停用 Claude.ai MCP servers，請將 `ENABLE_CLAUDEAI_MCP_SERVERS` 環境變數設定為 `false`：

```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude
```

> **注意：** 此功能僅適用於使用 Claude.ai 帳戶登入的使用者。

## MCP 設定流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as Config File
    participant Service as External Service

    User->>Claude: Type /mcp
    Claude->>Claude: List available MCP servers
    Claude->>User: Show options
    User->>Claude: Select GitHub MCP
    Claude->>Config: Update configuration
    Config->>Claude: Activate connection
    Claude->>Service: Test connection
    Service-->>Claude: Authentication successful
    Claude->>User: ✅ MCP connected!
```

### `/mcp` 指令

在工作階段中輸入 `/mcp` 可列出已連線的伺服器、觸發 OAuth 流程，並檢查連線狀態。

- 自 **v2.1.121** 起，MCP 在發生短暫錯誤時，初始連線最多重試 3 次。
- 自 **v2.1.128** 起，`/mcp` 會顯示每個已連線伺服器的**工具數量**，並以視覺方式標記回報 **0 個工具**的伺服器，讓設定錯誤的伺服器一眼就能發現。

## MCP Tool Search

當 MCP tool 描述超過 context window 的 10% 時，Claude Code 會自動啟用 tool search，以便在不造成模型 context 過載的情況下，高效地選擇正確的 tools。

| 設定 | 值 | 說明 |
|---------|-------|-------------|
| `ENABLE_TOOL_SEARCH` | `auto` (預設) | 當 tool 描述超過 context 的 10% 時自動啟用 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` | 在自訂的 `N` 個 tools 門檻時自動啟用 |
| `ENABLE_TOOL_SEARCH` | `true` | 無論 tool 數量多少皆始終啟用 |
| `ENABLE_TOOL_SEARCH` | `false` | 已停用；所有 tool 描述將完整傳送 |

> **注意：** Tool search 需要 Sonnet 4 或更高版本，或 Opus 4 或更高版本。Haiku 模型不支援 tool search。

### 對特定伺服器略過 Tool Search（v2.1.121+）

若特定 MCP 伺服器的工具在每次對話都需要用到，可在其設定中標記 `"alwaysLoad": true`，略過 tool search 的延遲載入，讓其工具始終保持可用：

```json
{
  "mcpServers": {
    "always-on-tool": {
      "command": "node",
      "args": ["./tools/always.js"],
      "alwaysLoad": true
    }
  }
}
```

請謹慎使用——每個始終載入的工具都會消耗上下文，而這些上下文原本可用於 tool search 以找出更相關的工具。

## 動態工具更新

Claude Code 支援 MCP `list_changed` 通知。當 MCP server 動態新增、移除或修改其可用工具時，Claude Code 會接收更新並自動調整其工具列表——無需重新連線或重啟。

## MCP Apps

MCP Apps 是第一個官方 MCP 擴充功能，它讓 MCP 工具呼叫可以回傳互動式 UI 元件，並直接在聊天介面中渲染。MCP server 不再僅限於純文字回應，而是可以提供豐富的儀表板、表單、資料視覺化以及多步驟工作流程——所有內容皆以行內（inline）方式顯示，無需離開對話介面。

## MCP 誘導（Elicitation）

MCP server 可以透過互動式對話向使用者請求結構化輸入（v2.1.49+）。這讓 MCP server 能夠在工作流程中途要求額外資訊——例如，提示進行確認、從選項列表中進行選擇，或填寫必填欄位——為 MCP server 的互動增添了互動性。

## 工具描述與指令限制

自 v2.1.84 起，Claude Code 對每個 MCP server 的工具描述與指令實施 **2 KB 的上限限制**。這可以防止單一 server 因過於冗長的工具定義而消耗過多的上下文（context），從而減少上下文膨脹並保持互動效率。

## 將 MCP Prompts 作為斜線命令

MCP server 可以公開提示詞（prompts），使其在 Claude Code 中以斜線命令的形式出現。提示詞可透過以下命名慣例進行存取：

```
/mcp__<server>__<prompt>
```

例如，如果一個名為 `github` 的 server 公開了一個名為 `review` 的提示詞，您可以透過 `/mcp__github__review` 來呼叫它。

## Server 重複定義處理

當同一個 MCP server 在多個範圍（local、project、user）中被定義時，local 設定將具有最高優先權。這讓您能夠使用 local 自訂設定來覆蓋 project 層級或 user 層級的 MCP 設定，而不會產生衝突。

## 近期生命週期修復（v2.1.136）

v2.1.136 修復了兩個長期存在的 MCP 生命週期錯誤——若您使用多伺服器設定，建議升級：

- **MCP 伺服器在 `/clear` 後持續存在**：透過 `.mcp.json`、外掛或 claude.ai connectors 設定的伺服器，在 VS Code、JetBrains 或 Agent SDK 中執行 `/clear` 後不再消失。舊版本會靜默地將其移除，需要重新啟動才能恢復。
- **OAuth refresh token 並行刷新修復**：多伺服器 OAuth 設定在多個伺服器同時競相刷新時，不再遺失 refresh token。這消除了影響多個 OAuth 保護 MCP 伺服器設定的「每天早上都要重新驗證」現象。

## 透過 @ 提及功能使用 MCP Resources

您可以使用 `@` 提及語法在提示詞中直接引用 MCP resources：

```
@server-name:protocol://resource/path
```

例如，要引用特定的資料庫資源：

```
@database:postgres://mydb/users
```

這讓 Claude 能夠擷取並將 MCP resource 內容以行內方式包含在對話的上下文（context）中。

## MCP 範圍 (Scopes)

MCP 設定可以儲存在不同的範圍中，具有不同的共享層級：

| 範圍 | 旗標 | 位置 | 說明 | 共享對象 | 是否需要核准 |
|-------|------|----------|-------------|-------------|------------------|
| **Local** (預設) | `--scope local` | `~/.claude.json` (位於專案路徑下) | 僅限目前使用者與目前專案私有（舊版本稱為 `project`） | 僅限您自己 | 否 |
| **Project** | `--scope project` | `.mcp.json` | 已提交至 git 儲存庫 | 團隊成員 | 是 (首次使用時) |
| **User** | `--scope user` | `~/.claude.json` | 可用於所有專案（舊版本稱為 `global`） | 僅限您自己 | 否 |

新增伺服器時使用 `--scope`（簡寫 `-s`）選擇範圍。若省略，
Claude Code 會使用 `local`：

```bash
# Project 範圍 — 寫入 .mcp.json，讓團隊共享
claude mcp add --scope project --transport http github https://api.github.com/mcp

# User 範圍 — 在每個專案中皆可用
claude mcp add --scope user --transport stdio memory -- npx @modelcontextprotocol/server-memory
```

### 使用 Project 範圍

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

團隊成員在首次使用專案 MCP 時，會看到核准提示。在未受信任的工作區中，儲存庫透過已提交的 `.claude/settings.json` 自行核准的伺服器，**不會**被 `claude mcp list`/`get` 自動啟動 — 它們會顯示 `⏸ Pending approval`，直到您接受信任對話框為止；且在未受信任的資料夾中，`enableAllProjectMcpServers` 會被忽略 (v2.1.196)。

## MCP 設定管理

### 新增 MCP Servers

```bash
# 新增基於 HTTP 的 server
claude mcp add --transport http github https://api.github.com/mcp

# 新增本地 stdio server
claude mcp add --transport stdio database -- npx @company/db-server

# 列出所有 MCP servers
claude mcp list

# 取得特定 server 的詳細資訊
claude mcp get github

# 移除一個 MCP server
claude mcp remove github

# 重設專案特定的核准選項
claude mcp reset-project-choices

# 從 CLI 驗證 MCP server (v2.1.186+)
claude mcp login github

# 登出 MCP server (v2.1.186+)
claude mcp logout github

# 從 Claude Desktop 匯入
claude mcp add-from-claude-desktop

# 從 JSON 字串新增 server（適用於腳本化設定）
claude mcp add-json events-server '{"type":"stdio","command":"npx","args":["@modelcontextprotocol/server-events"]}'
```

> **注意**：在 JSON 設定中 — `.mcp.json`、`~/.claude.json` 或 `claude mcp add-json` — `type` 欄位接受 `streamable-http` 作為 `http` 的別名。MCP 規格將此傳輸命名為 `streamable-http`，因此從伺服器自身文件複製而來的設定無需修改即可使用。

自 v2.1.238 起，`claude mcp list` 與 `claude mcp get` 會將已停用的伺服器顯示為 `⊘ Disabled`，且不會為了健康檢查而連線到它們。

`claude mcp login <name>` / `claude mcp logout <name>` 是 `/mcp` 選單中 OAuth 流程的非互動式對應方式 — 無需開啟選單即可驗證或登出。在 `login` 加上 `--no-browser`，即可透過 SSH 或在 headless 工作階段中完成 OAuth（它會將流程導向 stdin）。

## 可用 MCP Servers 表格

| MCP Server | 用途 | 常用工具 | 驗證方式 | 即時性 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 檔案操作 | read, write, delete | OS 權限 | ✅ 是 |
| **GitHub** | 儲存庫管理 | list_prs, create_issue, push | OAuth | ✅ 是 |
| **Slack** | 團隊溝通 | send_message, list_channels | Token | ✅ 是 |
| **Database** | SQL 查詢 | query, insert, update | 憑證 | ✅ 是 |
| **Google Docs** | 文件存取 | read, write, share | OAuth | ✅ 是 |
| **Asana** | 專案管理 | create_task, update_status | API Key | ✅ 是 |
| **Stripe** | 付款資料 | list_charges, create_invoice | API Key | ✅ 是 |
| **Memory** | 持久化記憶 | store, retrieve, delete | 本地 | ❌ 否 |

## 實際範例

### 範例 1：GitHub MCP 設定

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

#### Pull Request 管理
- `list_prs` - 列出儲存庫中所有的 PR
- `get_pr` - 取得 PR 詳細資訊（包含 diff）
- `create_pr` - 建立新的 PR
- `update_pr` - 更新 PR 描述/標題
- `merge_pr` - 將 PR 合併至主分支
- `review_pr` - 新增審核評論

**範例請求：**
```
/mcp__github__get_pr 456

# 回傳結果：
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

#### Issue 管理
- `list_issues` - 列出所有 issues
- `get_issue` - 取得 issue 詳細資訊
- `create_issue` - 建立新的 issue
- `close_issue` - 關閉 issue
- `add_comment` - 為 issue 新增評論

#### 儲存庫資訊
- `get_repo_info` - 儲存庫詳細資訊
- `list_files` - 檔案樹結構
- `get_file_content` - 讀取檔案內容
- `search_code` - 在程式碼庫中搜尋

#### Commit 操作
- `list_commits` - Commit 歷史紀錄
- `get_commit` - 特定 commit 詳細資訊
- `create_commit` - 建立新的 commit

**設定步驟**：
```bash
export GITHUB_TOKEN="your_github_token"
# 或者使用 CLI 直接新增：
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

### 設定中的環境變數擴充

MCP 設定支援環境變數擴充以及預設值回退。`${VAR}` 和 `${VAR:-default}` 語法可用於以下欄位：`command`、`args`、`env`、`url` 以及 `headers`。

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

變數會在執行時展開：
- `${VAR}` - 使用環境變數，若未設定則報錯
- `${VAR:-default}` - 使用環境變數，若未設定則使用預設值

### 範例 2：Database MCP 設定

**設定：**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    }
  }
}
```

**範例用法：**

```markdown
User: 獲取所有訂單數量超過 10 筆的使用者

Claude: 我將查詢您的資料庫以尋找該資訊。

# 使用 MCP database 工具：
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# 結果：
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```

**設定**：
```bash
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
# 或使用 CLI 直接新增：
claude mcp add --transport stdio database -- npx @modelcontextprotocol/server-database
```

### 範例 3：Multi-MCP 工作流程

**情境：每日報告生成**

```markdown
# 使用多個 MCP 的每日報告工作流程

## 設定
1. GitHub MCP - 獲取 PR 指標
2. Database MCP - 查詢銷售數據
3. Slack MCP - 發佈報告
4. Filesystem MCP - 儲存報告

## 工作流程

### 步驟 1：獲取 GitHub 數據
/mcp__github__list_prs completed:true last:7days

輸出：
- 總 PR 數：42
- 平均合併時間：2.3 小時
- 審查週轉時間：1.1 小時

### 步驟 2：查詢資料庫
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

輸出：
- 銷售量：247
- 營收：$12,450

### 步驟 3：生成報告
將數據整合至 HTML 報告

### 步驟 4：儲存至檔案系統
將 report.html 寫入至 /reports/

### 步驟 5：發佈至 Slack
將摘要傳送至 #daily-reports 頻道

最終輸出：
✅ 報告已生成並發佈
📊 本週已合併 47 個 PR
💰 日銷售額 $12,450
```

**設定**：
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="your_slack_token"
# 透過 CLI 新增每個 MCP 伺服器或在 .mcp.json 中進行配置
```

### 範例 4：Filesystem MCP 操作

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

**可用操作：**

| 操作 | 指令 | 用途 |
|-----------|---------|---------|
| 列出檔案 | `ls ~/projects` | 顯示目錄內容 |
| 讀取檔案 | `cat src/main.ts` | 讀取檔案內容 |
| 寫入檔案 | `create docs/api.md` | 建立新檔案 |
| 編輯檔案 | `edit src/app.ts` | 修改檔案 |
| 搜尋 | `grep "async function"` | 在檔案中搜尋 |
| 刪除 | `rm old-file.js` | 刪除檔案 |

**設定**：
```bash
# 使用 CLI 直接新增：
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /home/user/projects
```

## MCP vs Memory：決策矩陣

```mermaid
graph TD
    A["Need external data?"]
    A -->|No| B["Use Memory"]
    A -->|Yes| C["Does it change frequently?"]
    C -->|No/Rarely| B
    C -->|Yes/Often| D["Use MCP"]

    B -->|Stores| E["Preferences<br/>Context<br/>History"]
    D -->|Accesses| F["Live APIs<br/>Databases<br/>Services"]

    style A fill:#fff3e0,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#fff3e0,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#e8f5e9,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
```

## Request/Response 模式

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

## 環境變數

將敏感的憑證儲存在環境變數中：

```bash
# ~/.bashrc or ~/.zshrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxx"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="xoxb-xxxxxxxxxxxxx"
```

然後在 MCP 設定中引用它們：

```json
{
  "env": {
    "GITHUB_TOKEN": "${GITHUB_TOKEN}"
  }
}
```

## Claude 作為 MCP Server（`claude mcp serve`）

Claude Code 本身可以作為其他應用程式的 MCP server。這使得外部工具、編輯器和自動化系統能夠透過標準的 MCP 協定來利用 Claude 的能力。

```bash
# Start Claude Code as an MCP server on stdio
claude mcp serve
```

其他應用程式接著可以像連接任何基於 stdio 的 MCP server 一樣連接到此伺服器。例如，要在另一個 Claude Code 實例中將 Claude Code 增加為一個 MCP server：

```bash
claude mcp add --transport stdio claude-agent -- claude mcp serve
```

這對於構建多代理工作流程非常有用，其中一個 Claude 實例負責編排另一個實例。

## 管理式 MCP 設定（企業版）

對於企業級部署，IT 管理員透過兩種不同的機制來強制執行 MCP 伺服器政策：一個是 `managed-mcp.json` 檔案，用來部署一組具有排他性控制的固定伺服器；另一個是 `allowedMcpServers` / `deniedMcpServers` 設定鍵，用來過濾哪些已設定的伺服器可以載入。

**位置：**
- macOS: `/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux 與 WSL: `/etc/claude-code/managed-mcp.json`
- Windows: `C:\Program Files\ClaudeCode\managed-mcp.json`

`managed-mcp.json` 使用與專案 `.mcp.json` 相同的格式 — 一個頂層的 `mcpServers` 對應表。它負責部署伺服器，而不是過濾伺服器：

```json
{
  "mcpServers": {
    "example-remote": {
      "type": "http",
      "url": "https://mcp.example.com/mcp"
    },
    "company-internal": {
      "type": "stdio",
      "command": "/usr/local/bin/company-mcp-server",
      "args": ["--config", "/etc/company/mcp-config.json"]
    }
  }
}
```

機器上的任何使用者都能讀取此檔案，因此切勿在 `env` 區塊中放入憑證。請改用 `${VAR}` 展開、OAuth 或 `headersHelper`。

**過濾：允許清單與拒絕清單**

`allowedMcpServers`、`deniedMcpServers` 與 `allowAllClaudeAiMcps` 是**設定鍵，而非 `managed-mcp.json` 的欄位**。請將它們放在受管設定來源中 — 伺服器端受管設定、`managed-settings.json`、MDM 設定檔或登錄檔 — 才能強制執行：

- `allowedMcpServers` -- 允許伺服器的允許清單。請在同一個受管來源中同時設定 `allowManagedMcpServersOnly: true`，否則允許清單會從所有範圍合併，使用者可以擴大您的清單。
- `deniedMcpServers` -- 封鎖伺服器的拒絕清單。無論如何都會從所有範圍合併。
- `allowAllClaudeAiMcps` -- 在已部署的 `managed-mcp.json` 之外，同時載入 claude.ai 雲端 connectors（v2.1.149+）。僅從管理員控制的政策層級讀取。

每個項目都是一個只有**單一**鍵的物件：

| 鍵 | 比對對象 |
|-----|---------|
| `serverUrl` | 遠端伺服器 URL，可完全相符或使用 `*` 萬用字元 |
| `serverCommand` | 啟動 stdio 伺服器的確切指令與參數，以陣列表示 — 包含每個參數，且順序一致 |
| `serverName` | 使用者指定的標籤。**僅限完全相符；不會展開萬用字元** |

**範例設定：**

```json
{
  "allowedMcpServers": [
    { "serverUrl": "https://mcp.example.com/*" },
    { "serverCommand": ["/usr/local/bin/company-mcp-server", "--config", "/etc/company/mcp-config.json"] }
  ],
  "deniedMcpServers": [
    { "serverName": "untrusted-server" },
    { "serverUrl": "http://*" }
  ],
  "allowManagedMcpServersOnly": true
}
```

第三個受管設定 `managedMcpServers`（v2.1.259+）讓組織可以為每位使用者提供 HTTP/SSE MCP 伺服器。項目使用與 `.mcp.json` 相同的結構；指定要執行指令的項目會被略過。

> **注意：** 當 `allowedMcpServers` 與 `deniedMcpServers` 同時符合某個伺服器時，封鎖規則具有優先權。

## 外掛提供的 MCP 伺服器

外掛可以封裝自己的 MCP 伺服器，使其在安裝外掛時自動可用。外掛提供的 MCP 伺服器可以透過兩種方式定義：

1. **獨立的 `.mcp.json`** -- 將 `.mcp.json` 檔案放置在外掛根目錄中
2. **內嵌於 `plugin.json`** -- 直接在外掛清單中定義 MCP 伺服器

使用 `${CLAUDE_PLUGIN_ROOT}` 變數來引用相對於外掛安裝目錄的路徑：

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

## 子代理範圍內的 MCP

MCP servers 可以透過 `mcpServers:` 鍵值直接定義在 agent 的 frontmatter 中，將其範圍限制在特定的子代理，而非整個專案。當某個 agent 需要存取特定的 MCP server，而工作流程中的其他 agent 不需要時，這非常有用。

```yaml
---
mcpServers:
  my-tool:
    type: http
    url: https://my-tool.example.com/mcp
---

You are an agent with access to my-tool for specialized operations.
```

子代理範圍內的 MCP servers 僅在該 agent 的執行上下文（execution context）中可用，不會與父代理或兄弟代理共享。

## MCP 輸出限制

Claude Code 會對 MCP tool 的輸出進行限制，以防止上下文溢出：

| 限制 | 閾值 | 行為 |
|-------|-----------|----------|
| **警告** | 10,000 tokens | 顯示輸出內容過大的警告 |
| **預設最大值** | 25,000 tokens | 超過此限制的輸出將被截斷 |
| **磁碟持久化** | 50,000 字元 | 超過 50K 字元的工具結果將被持久化到磁碟 |

最大輸出限制可以透過 `MAX_MCP_OUTPUT_TOKENS` 環境變數進行設定：

```bash
# 將最大輸出增加到 50,000 tokens
export MAX_MCP_OUTPUT_TOKENS=50000
```

## 自動將長時間執行的工具呼叫轉至背景 (v2.1.212)

執行超過 2 分鐘的 MCP 工具呼叫現在會自動移至背景，讓工作階段保持可用，而不會因緩慢的工具而阻塞。此門檻可以設定，行為也可透過 `CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS` 調整或停用：

```bash
# 將自動轉背景的門檻改為 5 分鐘（300,000ms）
export CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS=300000
```

## 透過程式碼執行解決上下文膨脹

隨著 MCP 的採用規模擴大，連接到包含數百或數千個工具的數十個 servers 會帶來一個重大挑戰：**上下文膨脹（context bloat）**。這可以說是 MCP 在大規模應用時最大的問題，而 Anthropic 的工程團隊提出了一個優雅的解決方案——使用程式碼執行而非直接進行工具呼叫。

> **來源**：[Code Execution with MCP: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic Engineering Blog

### 問題所在：兩個導致 token 浪費的來源

**1. 工具定義過載了上下文視窗**

大多數 MCP 客戶端會在啟動時載入所有工具定義。當連接到數千個工具時，模型在讀取使用者的請求之前，就必須先處理數十萬個 tokens。

**2. 中間結果消耗額外的 tokens**

每個中間工具結果都會經過模型的上下文。假設要將會議紀錄從 Google Drive 轉移到 Salesforce——完整的會議紀錄會在上下文中流動**兩次**：一次是在讀取時，另一次是在寫入目的地時。一份 2 小時的會議紀錄可能意味著額外消耗 50,000+ tokens。

```mermaid
graph LR
    A["Model"] -->|"Tool Call: getDocument"| B["MCP Server"]
    B -->|"Full transcript (50K tokens)"| A
    A -->|"Tool Call: updateRecord<br/>(re-sends full transcript)"| B
    B -->|"Confirmation"| A

    style A fill:#ffcdd2,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
```

### 解決方案：將 MCP Tools 作為程式碼 API

與其透過上下文視窗傳遞工具定義和結果，agent 會**編寫程式碼**，將 MCP tools 作為 API 進行呼叫。該程式碼在沙盒化的執行環境中執行，且只有最終結果會返回給模型。

```mermaid
graph LR
    A["Model"] -->|"Writes code"| B["Code Execution<br/>Environment"]
    B -->|"Calls tools directly"| C["MCP Servers"]
    C -->|"Data stays in<br/>execution env"| B
    B -->|"Only final result<br/>(minimal tokens)"| A

    style A fill:#c8e6c9,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#f3e5f5,stroke:#333,color:#333
```

#### 工作原理

MCP 工具以帶有型別函式的檔案樹形式呈現：

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

每個工具檔案都包含一個帶有型別的封裝（wrapper）：

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

接著，代理會撰寫程式碼來編排這些工具：

```typescript
import * as gdrive from './servers/google-drive';
import * as salesforce from './servers/salesforce';

// 資料直接在工具之間流動 — 從不經過模型
const transcript = (
  await gdrive.getDocument({ documentId: 'abc123' })
).content;

await salesforce.updateRecord({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  data: { Notes: transcript }
});
```

**結果：token 使用量從 ~150,000 降至 ~2,000 — 減少了 98.7%。**

### 核心優勢

| 優勢 | 描述 |
|---------|-------------|
| **漸進式揭露 (Progressive Disclosure)** | 代理瀏覽檔案系統以僅載入所需的工具定義，而非一次載入所有工具 |
| **高效率的上下文結果** | 資料在返回模型之前，已在執行環境中完成過濾/轉換 |
| **強大的控制流** | 迴圈、條件判斷與錯誤處理直接在程式碼中執行，無需透過模型進行往返（round-tripping） |
| **隱私保護** | 中間資料（PII、敏感紀錄）保留在執行環境中；絕不會進入模型上下文 |
| **狀態持久化** | 代理可以將中間結果儲存至檔案，並建立可重複使用的技能函式 |

#### 範例：過濾大型資料集

```typescript
// 若無程式碼執行 — 所有 10,000 列資料都會流經上下文
// TOOL CALL: gdrive.getSheet(sheetId: 'abc123')
//   -> 在上下文中回傳 10,000 列

// 使用程式碼執行 — 在執行環境中進行過濾
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(
  row => row["Status"] === 'pending'
);
console.log(`Found ${pendingOrders.length} pending orders`);
console.log(pendingOrders.slice(0, 5)); // 只有 5 列會到達模型
```

#### 範例：無需往返的迴圈

```typescript
// 輪詢部署通知 — 完全在程式碼中執行
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({
    channel: 'C123456'
  });
  found = messages.some(
    m => m.text.includes('deployment complete')
  );
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('Deployment notification received');
```

### 需要考慮的權衡

程式碼執行會引入其自身的複雜性。執行由 agent 產生的程式碼需要：

- 一個具有適當資源限制的**安全沙箱執行環境**
- 對執行程式碼進行**監控與記錄**
- 與直接進行工具呼叫相比，會產生額外的**基礎設施開銷**

其優點（降低 token 成本、降低延遲、改善工具組合）應與這些實作成本進行權衡。對於僅擁有少數 MCP servers 的 agent 來說，直接進行工具呼叫可能更簡單。而對於大規模的 agent（擁有數十個 servers、數百個 tools）來說，程式碼執行則是顯著的改進。

### MCPorter：用於 MCP 工具組合的執行環境

[MCPorter](https://github.com/steipete/mcporter) 是一個 TypeScript 執行環境與 CLI 工具包，它讓呼叫 MCP servers 變得實際且無需樣板程式碼，並透過選擇性工具暴露與型別包裝器來幫助減少 context 膨脹。

**解決的問題：** MCPorter 不需預先從所有 MCP servers 載入所有工具定義，而是讓您可以根據需求發現、檢查並呼叫特定工具，從而保持您的 context 精簡。

**核心功能：**

| 功能 | 描述 |
|---------|-------------|
| **零設定發現** | 自動從 Cursor、Claude、Codex 或本地設定中發現 MCP servers |
| **型別化工具用戶端** | `mcporter emit-ts` 會生成 `.d.ts` 介面與即插即用的包裝器 |
| **可組合的 API** | `createServerProxy()` 將工具暴露為 camelCase 方法，並附帶 `.text()`、`.json()`、`.markdown()` 等輔助工具 |
| **CLI 生成** | `mcporter generate-cli` 將任何 MCP server 轉換為獨立的 CLI，並支援 `--include-tools` / `--exclude-tools` 過濾 |
| **參數隱藏** | 選用參數預設保持隱藏，減少 schema 的冗長性 |

**安裝：**

```bash
npx mcporter list          # 無需安裝 — 立即發現 servers
pnpm add mcporter          # 新增至專案
brew install steipete/tap/mcporter  # macOS 透過 Homebrew 安裝
```

**範例 — 在 TypeScript 中組合工具：**

```typescript
import { createRuntime, createServerProxy } from "mcporter";

const runtime = await createRuntime();
const gdrive = createServerProxy(runtime, "google-drive");
const salesforce = createServerProxy(runtime, "salesforce");

// 資料在工具之間流動，而無需經過模型 context
const doc = await gdrive.getDocument({ documentId: "abc123" });
await salesforce.updateRecord({
  objectType: "SalesMeeting",
  recordId: "00Q5f000001abcXYZ",
  data: { Notes: doc.text() }
});
```

**範例 — CLI 工具呼叫：**

```bash
# 直接呼叫特定工具
npx mcporter call linear.create_comment issueId:ENG-123 body:'Looks good!'

# 列出可用的 servers 與 tools
npx mcporter list
```

MCPorter 透過提供將 MCP tools 作為型別化 API 進行呼叫的執行環境基礎設施，對上述的程式碼執行方法形成了互補——這使得將中間資料排除在模型 context 之外變得非常簡單。

## 最佳實務

### 安全考量

#### 應執行事項 ✅
- 所有憑證皆使用環境變數
- 定期輪換 token 與 API keys（建議每月一次）
- 盡可能使用唯讀 token
- 將 MCP server 的存取範圍限制在最小需求範圍內
- 監控 MCP server 的使用情況與存取紀錄
- 若可用，請對外部服務使用 OAuth
- 對 MCP 請求實施速率限制（rate limiting）
- 在正式環境使用前測試 MCP 連線
- 記錄所有啟用的 MCP 連線
- 保持 MCP server 套件為最新版本

#### 不應執行事項 ❌
- 不要將憑證寫死在設定檔中
- 不要將 token 或秘密資訊提交至 git
- 不要透過團隊聊天或電子郵件分享 token
- 不要將個人 token 用於團隊專案
- 不要授予不必要的權限
- 不要忽略身分驗證錯誤
- 不要公開暴露 MCP 端點
- 不要以 root/admin 權限執行 MCP servers
- 不要將敏感資料快取在紀錄檔中
- 不要停用身分驗證機制

### 設定最佳實務

1. **版本控制**：將 `.mcp.json` 保存在 git 中，但使用環境變數來處理秘密資訊
2. **最小權限原則**：為每個 MCP server 授予所需的最小權限
3. **隔離**：盡可能在獨立的行程中執行不同的 MCP servers
4. **監控**：記錄所有 MCP 請求與錯誤，以建立稽核軌跡
5. **測試**：在部署到正式環境之前，測試所有 MCP 設定

### 效能技巧

- 在應用程式層級快取頻繁存取的資料
- 使用具備特定性的 MCP 查詢以減少資料傳輸量
- 監控 MCP 操作的響應時間
- 考慮對外部 API 實施速率限制
- 執行多個操作時使用批次處理（batching）

## 安裝指南

### 前置條件
- 已安裝 Node.js 與 npm
- 已安裝 Claude Code CLI
- 已準備好外部服務的 API token/憑證

### 逐步設定步驟

1. **使用 CLI 新增您的第一個 MCP server**（範例：GitHub）：
```bash
claude mcp add --transport stdio github -- npx @modelcontextprotocol/server-github
```

   或者在專案根目錄建立一個 `.mcp.json` 檔案：
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

2. **設定環境變數：**
```bash
export GITHUB_TOKEN="your_github_personal_access_token"
```

3. **測試連線：**
```bash
claude /mcp
```

4. **使用 MCP 工具：**
```bash
/mcp__github__list_prs
/mcp__github__create_issue "Title" "Description"
```

### 特定服務的安裝方式

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

### 從錯誤輸出開始 (v2.1.219+)

若伺服器無法連線，請在變更任何設定之前先執行 `claude mcp list`（或在工作階段中使用 `/mcp`）。Claude Code 現在會在失敗的伺服器旁印出 **HTTP 狀態碼與伺服器的錯誤文字**，因此您會看到 `401 Unauthorized` 或 `404 Not Found`，而不是籠統的「failed to connect」：

```bash
# 顯示連線狀態，並為失敗項目附上 HTTP 狀態與錯誤文字
claude mcp list
```

先閱讀狀態 — 它會告訴您該套用哪種修正：

- `401` / `403` → 憑證錯誤或已過期；使用 `claude mcp login <name>` 重新驗證
- `404` → URL 錯誤（通常是缺少 `/mcp` 或 `/sse` 路徑後綴）
- `5xx` / 逾時 → 遠端伺服器已停止運作；請參閱[連線逾時](#連線逾時)

### 設定值中的隱藏空白字元 (v2.1.219+)

當 MCP 設定值有**開頭或結尾空白字元**時，Claude Code 會發出警告。這是驗證失敗常見且難以察覺的原因：從瀏覽器或聊天訊息複製貼上的 token 常帶有結尾空格或換行，它會被原封不動地放進 `Authorization` 標頭中送出，並因憑證無效而失敗。

若看到此警告，請重新檢查 `.mcp.json` 中的值（或其展開來源的環境變數）並去除空白：

```bash
# 在分隔符號之間顯示隱藏的開頭/結尾空白字元
printf '[%s]\n' "$GITHUB_TOKEN"
```

### Headless 執行中被略過的伺服器 (v2.1.219+)

透過 `--mcp-config` 傳入但未通過設定驗證的伺服器會被**略過**，而不是中止執行，因此 headless 腳本可能看似正常運作，實際上卻少了一半的工具。Claude Code 現在會回報哪些伺服器被捨棄：

- **Headless / `-p` 執行**：stream-json 的 `init` 事件帶有 `mcp_server_errors` 欄位，列出每個被略過的項目。在信任執行結果之前請先檢查它。
- **互動式終端機執行**：相同的問題會在工作階段開始時以啟動警告的形式印出。

```bash
# 檢查 headless 執行中被略過的 --mcp-config 項目
claude -p "list my tools" --mcp-config ./servers.json \
  --output-format stream-json --verbose \
  | jq -r 'select(.type == "system" and .subtype == "init") | .mcp_server_errors'
```

### 找不到 MCP server
```bash
# 確認 MCP server 是否已安裝
npm list -g @modelcontextprotocol/server-github

# 若缺失則進行安裝
npm install -g @modelcontextprotocol/server-github
```

### 身分驗證失敗
```bash
# 確認環境變數是否已設定
echo $GITHUB_TOKEN

# 若有需要，重新執行 export
export GITHUB_TOKEN="your_token"

# 確認 token 具有正確的權限
# 可至此處檢查 GitHub token 範圍：https://github.com/settings/tokens
```

### 連線逾時
- 檢查網路連線狀況：`ping api.github.com`
- 確認 API 端點是否可存取
- 檢查 API 的速率限制 (rate limits)
- 嘗試在設定中增加逾時時間
- 檢查防火牆或代理伺服器 (proxy) 問題

### MCP server 當機
- 檢查 MCP server 紀錄檔：`~/.claude/logs/`
- 確認所有環境變數皆已設定
- 確保檔案權限正確
- 嘗試重新安裝 MCP server 套件
- 檢查是否有衝突的行程佔用了相同連接埠 (port)

## 相關概念

### Memory vs MCP
- **Memory**：儲存持久且不變的資料（偏好、上下文、歷史紀錄）
- **MCP**：存取即時且變動的資料（API、資料庫、即時服務）

### 何時使用各自的功能
- **使用 Memory**：用於使用者偏好、對話歷史、已學習的上下文
- **使用 MCP**：用於目前的 GitHub issues、即時資料庫查詢、即時資料

### 與其他 Claude 功能的整合
- 將 MCP 與 Memory 結合以獲得豐富的上下文
- 在提示詞中使用 MCP 工具以獲得更好的推理能力
- 利用多個 MCP 來處理複雜的工作流程

## 其他資源

- [Official MCP Documentation](https://code.claude.com/docs/en/mcp)
- [MCP Protocol Specification](https://modelcontextprotocol.io/specification)
- [MCP GitHub Repository](https://github.com/modelcontextprotocol/servers)
- [Available MCP Servers](https://github.com/modelcontextprotocol/servers)
- [MCPorter](https://github.com/steipete/mcporter) — TypeScript runtime & CLI，用於無需樣板程式碼即可呼叫 MCP servers
- [Code Execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic 工程部落格，關於解決上下文膨脹的問題
- [Claude Code CLI Reference](https://code.claude.com/docs/en/cli-reference)
- [Claude API Documentation](https://docs.anthropic.com)

---
**最後更新日期**：2026 年 9 月 6 日
**Claude Code 版本**：2.1.263
**來源**：
- https://code.claude.com/docs/en/mcp
- https://code.claude.com/docs/en/managed-mcp
- https://code.claude.com/docs/en/changelog
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/model-config
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
