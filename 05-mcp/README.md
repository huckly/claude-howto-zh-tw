<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# MCP (Model Context Protocol)

此資料夾包含關於 MCP 伺服器配置以及在 Claude Code 中使用的完整文件與範例。

## 概觀

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

Claude Code 支援多種傳輸協定用於 MCP server 連結

### HTTP 傳輸協定 (建議使用

```bash
# 基本 HTTP 連線
claude mcp add --transport http notion https://mcp.example.com/mcp

# 使用環境變數
claude mcp add --transport http myserver --env KEY=value
```

### Stdio 傳輸協定 (本地端

對於在本地執行中的 MCP server

```bash
# 本地 Node.js server
claude mcp add --transport stdio myserver -- npx @myorg/mcp-server

# 使用環境變數
claude mcp add --transport stdio myserver --env KEY=value
```

### SSE 傳輸協定 (已棄用

SSE 傳輸協定已棄用，建議改用 `http`

```bash
claude mcp add --transport sse legacy-server https://example.com/mcp
```

### Windows 特有注意事項

在原生 Windows (非 WSL

```bash
claude mcp add --transport stdio my-server -- npx @some/package
```

### OAuth 2.0 驗證

claude mcp add --transport stdio my-server --env KEY=value

```bash
# 透過
claude mcp add --transport http myserver -- npx server
```

### OAuth 2.0 驗證

對於需要 OAuth 2.0

```bash
# 透過
claude mcp add --transport http myserver --env KEY=value
```

| 功能 | 說明 |
|---------|-------------|
| **互動式 OAuth** | 使用 `/mcp

| 透過 |
| oauth-configuration |
| 透過-client-id |
| --client-secret |
| --callback-port 8cd-client-id |
| --client-secret |
| --callback-port 8080

| 透過

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callbackly-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret |
| --callback-port 8080

| oauth-client-id |
| --client-secret

### Claude.ai MCP Connectors

在您的 Claude.ai 帳戶中設定的 MCP servers 會自動在 Claude Code 中可用。這意味著您透過 Claude.ai 網頁介面建立的任何 MCP 連線都可以在無需額外設定的情況下直接存取。

Claude.ai MCP connectors 同時也支援 `--print` 模式 (v2.1.83+)，可實現非互動式與腳本化使用。

若要在 Claude Code 中停用 Claude.ai MCP servers，請將 `ENABLE_CLAUDEAI_MCP_SERVERS` 環境變數設定為 `false`：

```bash
ENABLE_CLAUDEAI_MCP_SERVERS=false claude
```

> **注意：** 此功能僅供使用 Claude.ai 帳戶登入的使用者使用。

## MCP Setup Process

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

## MCP Tool Search

當 MCP tool 描述超過 context 視窗的 10% 時，Claude Code 會自動啟用 tool search，以便在不造成模型 context 過載的情況下，高效地選擇正確的 tools。

| 設定 | 值 | 說明 |
|---------|-------|-------------|
| `ENABLE_TOOL_SEARCH` | `auto` (預設) | 當 tool 描述超過 context 的 10% 時自動啟用 |
| `ENABLE_TOOL_SEARCH` | `auto:<N>` | 當 tool 數量達到自定義門檻 `N` 時自動啟用 |
| `ENABLE_TOOL_SEARCH` | `true` | 無論 tool 數量多少皆始終啟用 |
| `ENABLE_TOOL_SEARCH` | `false` | 已停用；所有 tool 描述將完整傳送 |

> **注意：** Tool search 需要 Sonnet 4 或更高版本，或 Opus 4 或更高版本。Haiku 模型不支援 tool search。

## 動態工具更新

Claude Code 支援 MCP `list_changed` 通知。當 MCP server 動態新增、移除或修改其可用工具時，Claude Code 會接收更新並自動調整其工具列表 —— 無需重新連線或重啟。

## MCP Apps

MCP Apps 是第一個官方 MCP 擴充功能，它讓 MCP 工具呼叫可以回傳互動式 UI 元件，並直接在聊天介面中渲染。MCP server 不再僅限於純文字回應，而是可以提供豐富的儀表板、表單、數據視覺化和多步驟工作流程 —— 所有內容皆以行內（inline）方式顯示，無需離開對話。

## MCP 誘導（Elicitation）

MCP server 可以透過互動式對話向使用者請求結構化輸入（v2.1.49+）。這允許 MCP server 在工作流程中途要求額外資訊 —— 例如，提示進行確認、從選項列表中進行選擇或填寫必填欄位 —— 為 MCP server 的互動增添了互動性。

## 工具描述與指令限制

自 v2.1.84 起，Claude Code 對每個 MCP server 的工具描述與指令實施 **2 KB 的上限限制**。這可以防止單一 server 因過於冗長的工具定義而消耗過多的上下文（context），從而減少上下文膨脹並保持互動效率。

## 作為斜線命令的 MCP Prompts

MCP server 可以公開提示詞（prompts），使其在 Claude Code 中以斜線命令的形式出現。提示詞可以使用以下命名慣例進行存取：

```
/mcp__<server>__<prompt>
```

例如，如果一個名為 `github` 的 server 公開了一個名為 `review` 的提示詞，您可以透過 `/mcp__github__review` 來呼叫它。

## Server 重複定義處理

當同一個 MCP server 在多個範圍（local、project、user）中被定義時，local 設定將具有最高優先權。這讓您能夠使用 local 自定義設定來覆蓋 project 層級或 user 層級的 MCP 設定，而不會產生衝突。

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

| 範圍 | 位置 | 說明 | 共享對象 | 是否需要核准 |
|-------|----------|-------------|-------------|------------------|
| **Local** (預設) | `~/.claude.json` (位於專案路徑下) | 僅限當前使用者與當前專案私有（舊版本稱為 `project`） | 僅限您自己 | 否 |
| **Project** | `.mcp.json` | 已提交至 git 儲存庫 | 團隊成員 | 是（首次使用時） |
| **User** | `~/.claude.json` | 可用於所有專案（舊版本稱為 `global`） | 僅限您自己 | 否 |

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

團隊成員在首次使用專案 MCP 時，會看到核准提示。

## MCP 設定管理

### 新增 MCP 伺服器

```bash
# 新增基於 HTTP-based server
claude mcp add --transport http github https://api.github.com/mcp

# 新增 local stdio server
claude mcp add --transport stdio database -- npx @company/db-server

# 列出所有 MCP servers
claude mcp list

# 取得特定伺服器詳細資訊
clauts_details

# 移除一個 MCP server
claude mcp remove github

# 重設專案特定核准選擇
claude mcp reset-project-choices

# 從 Claude Desktop 匯入
claude mcp add-from-claude-desktop
```

## 可用 MCP 伺服器表格

| MCP 伺服器 | 用途 | 常用的工具 | 驗證方式 | 即時性 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 檔案操作 | 權限 | 讀取檔案內容 | 檔案路徑 | ✅ 是 |
| **GitHub** | 儲存庫管理 | 提交紀錄 | 提交

| 目的 | 提交

| 
|------------|---------|--------------|------|-----------|
| **Slack** | 團隊溝通 | 
| **Database** | SQL 查詢 | 
| **Google Docs | 文件的存取 | 
| **Asana | 專案管理 | 
| **Strabes | 支付數據 | 
| **Memory | 持續性記憶 | 
| **Stripe | 支付數據 | 
| **Memory | 存儲

| 目的 | 
|------------|---------|
| **Google Docs | 
| **Asana | 
| **Stripe | 
| **Memory | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Memory | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Memory | 
| **Asana | 
| **Stripes | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripes | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe | 
| **Asana | 
| **Stripe

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
- `${VAR:-default}` - 使用環境變數，若未設定則回退至預設值

### 範例 2：Database MCP 設定

**配置：**

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
# 或者使用 CLI 直接新增：
claude mcp add --transport stdio database -- npx @modelcontextprotocol/server-database
```

### 範例 3：多重 MCP 工作流程

**情境：每日報告生成**

```markdown
# 使用多個 MCP 的每日報告工作流程
```

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

**Setup**:
```bash
export GITHUB_TOKEN="your_github_token"
export DATABASE_URL="postgresql://user:pass@localhost/mydb"
export SLACK_TOKEN="your_slack_token"
# 透過 CLI 新增每個 MCP 伺服器或在 .mcp.json 中進行配置
```

### 範例 4：Filesystem MCP 操作

**配置：**

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

**Setup**:
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

## Claude 作為 MCP Server (`claude mcp serve`)

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

## 管理式 MCP 設定 (企業版)

對於企業級部署，IT 管理員可以透過 `managed-mcp.json` 設定檔來強制執行 MCP 伺服器政策。此檔案提供了對全組織範圍內允許或封鎖哪些 MCP 伺服器的排他性控制。

**位置：**
- macOS: `/Library/Application Support/ClaudeCode/managed-mcp.json`
- Linux: `~/.config/ClaudeCode/managed-mcp.json`
- Windows: `%APPDATA%\ClaudeCode\managed-mcp.json`

**功能：**
- `allowedMcpServers` -- 允許伺服器的白名單
- `deniedMcpServers` -- 禁止伺服器的黑名單
- 支援透過伺服器名稱、指令與 URL 模式進行比對
- 在使用者設定之前強制執行全組織範圍的 MCP 政策
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

> **注意：** 當 `allowedMcpServers` 與 `deniedMcpServers` 同時符合某個伺服器時，封鎖規則具有優先權。

## 外掛提供的 MCP 伺服器

外掛可以封裝自己的 MCP 伺服器，使其在安裝外掛時自動可用。外掛提供的 MCP 伺服器可以透過兩種方式定義：

1. **獨立的 `.mcp.json`** -- 將 `.mcp.json` 檔案放置在外掛的根目錄中
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

最大輸出限制可以透過 `MAX_MCP_OUTPUT_TOKENS` 環境變數進行配置：

```bash
# 將最大輸出增加到 50,000 tokens
export MAX_MCP_OUTPUT_TOKENS=50000
```

## 透過程式碼執行解決上下文膨脹

隨著 MCP 的採用規模擴大，連接到包含數百或數千個工具的數十個 servers 會帶來一個重大挑戰：**上下文膨脹（context bloat）**。這可以說是 MCP 在大規模應用時最大的問題，而 Anthropic 的工程團隊提出了一個優雅的解決方案——使用程式碼執行而非直接的工具呼叫。

> **來源**：[Code Execution with MCP: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic Engineering Blog

### 問題所在：兩個導致 token 浪費的來源

**1. 工具定義過載了上下文視窗**

大多數 MCP clients 會預先載入所有工具定義。當連接到數千個工具時，模型在讀取使用者的請求之前，就必須處理數十萬個 tokens。

**2. 中間結果消耗額外的 tokens**

每個中間工具結果都會經過模型的上下文。假設要將會議逐字稿從 Google Drive 轉移到 Salesforce——完整的逐字稿會在上下文中流動**兩次**：一次是在讀取時，另一次是在寫入目的地時。一份 2 小時的會議逐字稿可能意味著超過 50,000 個額外的 tokens。

```mermaid
graph LR
    A["Model"] -->|"Tool Call: getDocument"| B["MCP Server"]
    B -->|"Full transcript (50K tokens)"| A
    A -->|"Tool Call: updateRecord<br/>(re-sends full transcript)"| B
    B -->|"Confirmation"| A

    style A fill:#ffcdd2,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
```

### 解決方案：將 MCP tools 作為程式碼 API

與其透過上下文視窗傳遞工具定義和結果，agent 會**撰寫程式碼**，將 MCP tools 作為 API 進行呼叫。程式碼在沙盒化的執行環境中執行，且只有最終結果會返回給模型。

```mermaid
graph LR
    A["Model"] -->|"Writes code"| B["Code Execution<br/>Environment"]
    B -->|"Calls tools directly"| C["MCP Servers"]
    C -->|"Data stays in<br/>execution env
import GetDocument-CD way
    C -->|"Data stays in<br/>execution env
    B -->|"Only final result
    A -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data staysin
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data unique
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays in
    B -->|"Data stays

```javascript
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

程式碼執行會引入自身的複雜性。執行由 agent 產生的程式碼需要：

- 一個具有適當資源限制的**安全沙盒執行環境**
- 對執行程式碼進行**監控與記錄**
- 與直接呼叫工具相比，需要額外的**基礎設施開銷**

其優點（降低 token 成本、降低延遲、改善工具組合）應與這些實作成本進行權衡。對於僅擁有少數 MCP servers 的 agent 來說，直接呼叫工具可能更簡單。但對於大規模的 agent（擁有數十個 servers、數百個 tools）而言，程式碼執行是一項顯著的改進。

### MCPorter：用於 MCP 工具組合的執行環境

[MCPorter](https://github.com/steipete/mcporter) 是一個 TypeScript 執行環境與 CLI 工具包，它讓呼叫 MCP servers 變得實際且無需樣板程式碼，並透過選擇性工具暴露與型別包裝器來幫助減少 context 膨脹。

**解決的問題：** MCPorter 不需預先從所有 MCP servers 載入所有工具定義，而是讓您可以根據需求發現、檢查並呼叫特定工具，從而保持您的 context 精簡。

**關鍵特性：**

| 特性 | 描述 |
|---------|-------------|
| **零配置發現** | 自動從 Cursor、Claude、Codex 或本地配置中發現 MCP servers |
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

MCPorter 透過提供將 MCP tools 作為型別化 API 呼叫的執行環境基礎設施，對上述的程式碼執行方法形成了互補——這使得將中間資料排除在模型 context 之外變得非常簡單。

## 最佳實務

### 安全考量

#### 應執行事項 ✅
- 使用環境變數來管理所有憑證
- 定期輪換 token 與 API keys（建議每月一次）
- 盡可能使用唯讀 token
- 將 MCP server 的存取範圍限制在最小需求範圍內
- 監控 MCP server 的使用情況與存取日誌
- 若可用，請對外部服務使用 OAuth
- 對 MCP 請求實施速率限制（rate limiting）
- 在正式環境使用前測試 MCP 連線
- 記錄所有啟用的 MCP 連線
- 保持 MCP server 套件為最新版本

#### 不應執行事項 ❌
- 不要將憑證寫死（hardcode）在設定檔中
- 不要將 token 或秘密資訊提交（commit）至 git
- 不要透過團隊聊天或電子郵件分享 token
- 不要將個人 token 用於團隊專案
- 不要授予不必要的權限
- 不要忽略身分驗證錯誤
- 不要公開暴露 MCP 端點
- 不要以 root/admin 權限執行 MCP servers
- 不要將敏感資料快取在日誌中
- 不要停用身分驗證機制

### 設定最佳實務

1. **版本控制**：將 `.mcp.json` 保存在 git 中，但使用環境變數來處理秘密資訊
2. **最小權限原則**：為每個 MCP server 授予所需的最小權限
3. **隔離**：盡可能在獨立的程序中執行不同的 MCP servers
4. **監控**：記錄所有 MCP 請求與錯誤，以建立稽核軌跡
5. **測試**：在部署到正式環境之前，測試所有 MCP 設定

### 效能技巧

- 在應用程式層級快取頻繁存取的資料
- 使用具備特定性的 MCP 查詢以減少資料傳輸量
- 監控 MCP 操作的響應時間
- 考慮對外部 API 實施速率限制
- 執行多個操作時使用批次處理（batching）

## 安裝說明

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

### 找不到 MCP server
```bash
# 確認 MCP server 已安裝
npm list -g @modelcontextprotocol/server-github

# 若缺失則進行安裝
npm install -g @modelcontextprotocol/server-github
```

### 身分驗證失敗
```bash
# 確認環境變數已設定
echo $GITHUB_TOKEN

# 若有需要，重新執行 export
export GITHUB_TOKEN="your_token"

# 確認 token 具有正確的權限
# 可在以下網址檢查 GitHub token scopes: https://github.com/settings/tokens
```

### 連線逾時
- 檢查網路連線狀況：`ping api.github.com`
- 確認 API 端點是否可存取
- 檢查 API 的速率限制 (rate limits)
- 嘗試在設定中增加 timeout 時間
- 檢查防火牆或代理伺服器 (proxy) 問題

### MCP server 當機
- 檢查 MCP server 日誌：`~/.claude/logs/`
- 確認所有環境變數皆已設定
- 確保檔案權限正確
- 嘗試重新安裝 MCP server 套件
- 檢查是否有衝突的程序佔用了相同的連接埠 (port)

## 相關概念

### Memory vs MCP
- **Memory**：儲存持久且不變的資料（偏好、上下文、歷史紀錄）
- **MCP**：存取即時且變動的資料（API、資料庫、即時服務）

### 何時使用各自的功能
- **使用 Memory**：用於使用者偏好、對話歷史、學習到的上下文
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

**最後更新日期**：2026 年 4 月 16 日
**Claude Code 版本**：2.1.112
**來源**：
- https://docs.anthropic.com/en/docs/claude-code/mcp
- https://www.anthropic.com/news/claude-opus-4-7
- https://support.claude.com/en/articles/12138966-release-notes
**相容模型**：Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
