<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# CLI 命令列介面

## 概觀

Claude Code CLI（命令列介面）是與 Claude Code 互動的主要方式。它提供了強大的選項，用於執行查詢、管理會話、配置模型，以及將 Claude 整合到您的開發工作流程中。

## 架構

```mermaid
graph TD
    A["User Terminal"] -->|"claude [options] [query]"| B["Claude Code CLI"]
    B -->|Interactive| C["REPL Mode"]
    B -->|"--print"| D["Print Mode (SDK)"]
    B -->|"--resume"| E["Session Resume"]
    C -->|Conversation| F["Claude API"]
    D -->|Single Query| F
    E -->|Load Context| F
    F -->|Response| G["Output"]
    G -->|text/json/stream-json| H["Terminal/Pipe"]
```

## 執行環境與封裝

自 **v2.1.113** 起，Claude Code CLI 透過可選的 npm 相依套件啟動**各平台的原生二進位執行檔**（macOS、Linux、Windows）。二進位執行檔在安裝時會依據您的作業系統與架構自動配對——舊版的 JavaScript 打包執行環境在 macOS 或 Linux 上已不再是預設選項。

**使用者端的安裝方式不變**：`npm install -g @anthropic-ai/claude-code` 依然有效，且仍是推薦的安裝路徑。在背後，npm 會為您的平台取得正確的原生二進位執行檔。

**下載來源**（v2.1.116+）：原生二進位執行檔的產出物由 `https://downloads.claude.ai/claude-code-releases` 提供。

> **企業/Proxy 使用者**：若您的網路需要明確的允許清單，請將 `downloads.claude.ai`（以及 `https://downloads.claude.ai/claude-code-releases`）加入您的 proxy 出口規則。先前僅允許 `storage.googleapis.com` 或 npm registry 的環境需要更新規則，否則 `claude update` 及初始安裝將會失敗。

舊版 JavaScript 套件仍會為 Windows 及固定使用該版本的環境產出；這些安裝版本繼續將 Glob 和 Grep 作為一級工具提供（請參閱[工具](#工具與權限管理)下方的 Glob/Grep 附註）。

## CLI 命令

| 命令 | 描述 | 範例 |
|---------|-------------|---------|
| `claude` | 啟動互動式 REPL | `claude` |
| `claude "query"` | 啟動帶有初始提示詞的 REPL | `claude "explain this project"` |
| `claude -p "query"` | 列印模式 - 執行查詢後退出 | `claude -p "explain this function"` |
| `cat file \| claude -p "query"` | 處理透過管線傳遞的內容 | `cat logs.txt \| claude -p "explain"` |
| `claude -c` | 繼續最近一次的會話 | `claude -c` |
| `claude -c -p "query"` | 在列印模式下繼續會話 | `claude -c -p "check for type errors"` |
| `claude -r "<session>" "query"` | 透過 ID 或名稱恢復會話 | `claude -r "auth-refactor" "finish this PR"` |
| `claude update` | 更新至最新版本 | `claude update` |
| `/doctor`（斜線命令） | 診斷安裝、設定與外掛健康狀態。自 v2.1.116 起可在 **Claude 回應過程中**開啟，以內嵌方式顯示狀態圖示，並接受 `f` 鍵自動修復偵測到的問題 | 在 REPL 中執行 `/doctor` |
| `claude mcp` | 配置 MCP 伺服器 | 請參閱 [MCP documentation](../05-mcp/) |
| `claude mcp serve` | 將 Claude Code 作為 MCP 伺服器執行 | `claude mcp serve` |
| `claude agents` | 開啟 **Agent View**（研究預覽版，v2.1.139+）— 多會話管理器，列出每個 Claude Code 會話及其狀態。詳見下方 [Agent View](#agent-view-claude-agents-v21139)。 | `claude agents` |
| `claude auto-mode defaults` | 以 JSON 格式列印自動模式預設規則 | `claude auto-mode defaults` |
| `claude remote-control` | 啟動遠端控制伺服器 | `claude remote-control` |
| `claude plugin` | 管理外掛（安裝、啟用、停用） | `claude plugin install my-plugin` |
| `claude plugin tag <version>` | 為外掛建立含版本驗證的發行 git 標籤（v2.1.118+） | `claude plugin tag v0.3.0` |
| `claude install [version]` | 安裝指定的原生二進位版本。接受 `stable`、`latest` 或明確的版本字串 | `claude install 2.1.131` |
| `claude project purge [path]` | 刪除專案的所有本地 Claude Code 狀態（逐字記錄、任務、除錯日誌、檔案編輯歷史、提示詞歷史紀錄，以及 `~/.claude.json` 項目）。省略 `[path]` 則顯示互動式選擇器。旗標：`--dry-run` 預覽、`-y/--yes` 跳過確認、`-i/--interactive` 逐項確認、`--all` 清除所有專案（v2.1.126+） | `claude project purge ~/work/repo --dry-run` |
| `claude plugin prune` | 移除孤立的自動安裝外掛相依套件（父外掛已移除）。`plugin uninstall --prune` 在卸載目標後執行相同的串聯清除（v2.1.121+） | `claude plugin prune` |
| `claude ultrareview [target]` | 以非互動方式執行 `/ultrareview`。將結果輸出至 stdout，成功時退出代碼為 0，失敗時為 1。使用 `--json` 取得原始資料，`--timeout <minutes>` 覆蓋預設的 30 分鐘限制（v2.1.120+） | `claude ultrareview 1234 --json` |
| `claude auth login` | 登入（支援 `--email`、`--sso`）。自 v2.1.126 起，當瀏覽器回調無法連到 localhost 時（WSL2、SSH、容器），支援將 OAuth 驗證碼貼到終端機作為備援方式 | `claude auth login --email user@example.com` |
| `claude auth logout` | 登出目前帳戶 | `claude auth logout` |
| `claude auth status` | 檢查驗證狀態（已登入則退出碼為 0，未登入則為 1） | `claude auth status` |

## 核心 Flags

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `-p, --print` | 印出回應而不進入互動模式 | `claude -p "query"` |
| `-c, --continue` | 載入最近一次的對話 | `claude --continue` |
| `-r, --resume` | 透過 ID 或名稱恢復特定會話 | `claude --resume auth-refactor` |
| `-v, --version` | 輸出版本號碼 | `claude -v` |
| `-w, --worktree` | 在隔離的 git worktree 中啟動 | `claude -w` |
| `-n, --name` | 會話顯示名稱 | `claude -n "auth-refactor"` |
| `--from-pr <url-or-number>` | 恢復與 Pull/Merge Request 關聯的會話。自 v2.1.119 起接受 GitHub（雲端 + Enterprise）、GitLab MR 和 Bitbucket PR URL；先前僅支援 GitHub.com | `claude --from-pr 42` 或 `claude --from-pr https://gitlab.example.com/org/repo/-/merge_requests/17` |
| `--remote "task"` | 在 claude.ai 上建立網頁會話 | `claude --remote "implement API"` |
| `--remote-control, --rc` | 使用 Remote Control 的互動式會話 | `claude --rc` |
| `--teleport` | 在本地恢復網頁會話 | `claude --teleport` |
| `--teammate-mode` | 代理團隊顯示模式 | `claude --teammate-mode tmux` |
| `--bare` | 最小模式（跳過 hooks、skills、plugins、MCP、自動記憶、CLAUDE.md） | `claude --bare` |
| `--enable-auto-mode` | 解鎖自動權限模式（Max 訂閱者使用 Opus 4.7 時已不再需要） | `claude --enable-auto-mode` |
| `--channels` | 訂閱 MCP channel plugins | `claude --channels discord,telegram` |
| `--chrome` / `--no-chrome` | 啟用/停用 Chrome 瀏覽器整合 | `claude --chrome` |
| `--effort` | 設定思考努力程度 | `claude --effort high` |
| `--init` / `--init-only` | 執行初始化 hooks | `claude --init` |
| `--maintenance` | 執行維護 hooks 並退出 | `claude --maintenance` |
| `--disable-slash-commands` | 停用所有 skills 與斜線命令 | `claude --disable-slash-commands` |
| `--no-session-persistence` | 停用會話儲存（印出模式） | `claude -p --no-session-persistence "query"` |
| `--exclude-dynamic-system-prompt-sections` | 從 system prompt 中排除動態區段，以獲得更好的 prompt 快取命中率 | `claude -p --exclude-dynamic-system-prompt-sections "query"` |

### 互動模式 vs 印出模式

```mermaid
graph LR
    A["claude"] -->|Default| B["Interactive REPL"]
    A -->|"-p flag"| C["Print Mode"]
    B -->|Features| D["多輪對話<br>Tab 補全<br>歷史紀錄<br>斜線命令"]
    C -->|Features| E["單次查詢<br>可腳本化<br>可透過 pipe 傳遞<br>JSON 輸出"]
```

**互動模式** (預設):
```bash
# 啟動互動式會話
claude

# 帶有初始提示詞啟動
claude "explain the authentication flow"
```

**印出模式** (非互動式):
```bash
# 單次查詢後退出
claude -p "what does this function do?"

# 處理檔案內容
cat error.log | claude -p "explain this error"

# 與其他工具串接
claude -p "list todos" | grep "URGENT"
```

## 模型與組態

| 參數 | 說明 | 範例 |
|------|-------------|---------|
| `--model` | 設定模型 (sonnet, opus, haiku 或完整名稱) | `claude --model opus` |
| `--fallback-model` | 當負載過重時自動切換備用模型 | `claude -p --fallback-model sonnet "query"` |
| `--agent` | 指定該會話使用的代理 | `claude --agent my-custom-agent` |
| `--agents` | 透過 JSON 定義自定義子代理 | 請參閱 [Agents Configuration](#agents-configuration) |
| `--effort` | 設定投入程度 (low, medium, high, xhigh, max) | `claude --effort high` |

### 模型選擇範例

```bash
# 使用 Opus 4.7 處理複雜任務
claude --model opus "design a caching strategy"

# 使用 Haiku 4.5 處理快速任務
claude --model haiku -p "format this JSON"

# 使用完整模型名稱
claude --model claude-sonnet-4-6-20250929 "review this code"

# 使用備用模型以確保可靠性
claude -p --model opus --fallback-model sonnet "analyze architecture"

# 使用 opusplan (Opus 規劃，Sonnet 執行)
claude --model opusplan "design and implement the caching layer"
```

> **閘道模型探索（v2.1.129+，需手動開啟）**：當 `ANTHROPIC_BASE_URL` 指向相容 Anthropic 的閘道時，設定 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1` 可從閘道的 `/v1/models` 端點填充 `/model` 清單。若未設定此環境變數，`/model` 將退回使用內建靜態清單。此旗標為選擇性開啟（v2.1.129 的變更），因為探索呼叫可能會顯示使用者無權使用的模型；v2.1.126 曾將其設為隱式啟用，後來已還原。

## System Prompt 自定義

| 參數 | 說明 | 範例 |
|------|-------------|---------|
| `--system-prompt` | 取代整個預設提示詞 | `claude --system-prompt "You are a Python expert"` |
| `--system-prompt-file` | 從檔案載入提示詞 (僅限 print 模式) | `claude -p --system-prompt-file ./prompt.txt "query"` |
| `--append-system-prompt` | 附加至預設提示詞 | `claude --append-system-prompt "Always use TypeScript"` |

### System Prompt 範例

```bash
# 完全自定義人格
claude --system-prompt "You are a senior security engineer. Focus on vulnerabilities."

# 附加特定指令
claude --append-system-prompt "Always include unit tests with code examples"

# 從檔案載入複雜的提示詞
claude -p --system-prompt-file ./prompts/code-reviewer.txt "review main.py"
```

### System Prompt 參數比較

| 參數 | 行為 | 互動模式 | Print 模式 |
|------|----------|-------------|-------|
| `--system-prompt` | 取代整個預設 system prompt | ✅ | ✅ |
| `--system-prompt-file` | 以檔案中的提示詞取代 | ❌ | ✅ |
| `--append-system-prompt` | 附加至預設 system prompt | ✅ | ✅ |

**請僅在 print 模式下使用 `--system-prompt-file`。在互動模式下，請使用 `--system-prompt` 或 `--append-system-prompt`。**

## 工具與權限管理

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `--tools` | 限制可用的內建工具 | `claude -p --tools "Bash,Edit,Read" "query"` |
| `--allowedTools` | 無須提示即可執行的工具 | `"Bash(git log:*)" "Read"` |
| `--disallowedTools` | 從上下文（context）中移除的工具 | `"Bash(rm:*)" "Edit"` |
| `--dangerously-skip-permissions` | 跳過所有權限提示 | `claude --dangerously-skip-permissions` |
| `--permission-mode` | 以指定的權限模式啟動 | `claude --permission-mode auto` |
| `--permission-prompt-tool` | 用於處理權限的 MCP 工具 | `claude -p --permission-prompt-tool mcp_auth "query"` |
| `--enable-auto-mode` | 解鎖自動權限模式 | `claude --enable-auto-mode` |

> **Glob / Grep 附註（v2.1.113+）**：在原生 macOS/Linux 版本上，`Glob` 和 `Grep` 是透過 Bash 工具呼叫內嵌的 `bfs` 和 `ugrep` 二進位執行檔提供，而非作為獨立的一級工具。Windows 及 npm 打包（JS）安裝版仍以獨立工具的形式提供。對於子代理的 `allowedTools` / `disallowedTools` 清單，後端的替換是透明的——在各平台的設定中均可繼續使用 `Glob` / `Grep`。

> **PowerShell 自動核准（v2.1.119）**：PowerShell 工具指令可在權限模式下以與 Bash 指令完全相同的方式自動核准。使用與 `Bash(...)` 規則相同的比對語法來限定 PowerShell 權限範圍，例如 `PowerShell(Get-ChildItem:*)`。

> **`--permission-mode` 在恢復時生效（v2.1.132+）**：`claude -p --continue --permission-mode plan`（以及 `--resume`）現在會正確套用此旗標。先前的版本在恢復會話時會靜默忽略 `--permission-mode`，導致 plan 模式的會話在不重新傳入旗標的情況下恢復時，會靜默降級——此問題已修正。

### 權限範例

```bash
# 用於程式碼審查的唯讀模式
claude --permission-mode plan "review this codebase"

# 僅限制於安全工具
claude --tools "Read,Grep,Glob" -p "find all TODO comments"

# 允許特定的 git 指令而無需提示
claude --allowedTools "Bash(git status:*)" "Bash(git log:*)"

# 封鎖危險操作
claude --disallowedTools "Bash(rm -rf:*)" "Bash(git push --force:*)"
```

## 輸出與格式

| Flag | 說明 | 選項 | 範例 |
|------|-------------|---------|---------|
| `--output-format` | 指定輸出格式（列印模式） | `text`, `json`, `stream-json` | `claude -p --output-format json "query"` |
| `--input-format` | 指定輸入格式（列印模式） | `text`, `stream-json` | `claude -p --input-format stream-json` |
| `--verbose` | 啟用詳細日誌 | | `claude --verbose` |
| `--include-partial-messages` | 包含串流事件 | 需要 `stream-json` | `claude -p --output-format stream-json --include-partial-messages "query"` |
| `--json-schema` | 取得符合 schema 的驗證 JSON | | `claude -p --json-schema '{"type":"object"}' "query"` |
| `--max-budget-usd` | 列印模式的最大支出金額 | | `claude -p --max-budget-usd 5.00 "query"` |

### 輸出格式範例

```bash
# 純文字（預設）
claude -p "explain this code"

# 用於程式化使用的 JSON
claude -p --output-format json "list all functions in main.py"

# 用於即時處理的串流 JSON
claude -p --output-format stream-json "generate a long report"

# 帶有 schema 驗證的結構化輸出
claude -p --json-schema '{"type":"object","properties":{"bugs":{"type":"array"}}}' \
  "find bugs in this code and return as JSON"
```

## Workspace 與目錄

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `--add-dir` | 新增額外的作業目錄 | `claude --add-dir ../apps ../lib` |
| `--setting-sources` | 以逗號分隔的設定來源 | `claude --setting-sources user,project` |

> **`/config` 持久化（v2.1.119）**：透過 `/config` 指令互動式進行的變更，現在會寫入 `~/.claude/settings.json` 並參與正常的優先順序鏈（project → local → policy → user）。v2.1.119 之前，部分 `/config` 變更僅對目前會話有效。完整優先順序請參閱 [Memory & Settings](../02-memory/README.md)。
| `--settings` | 從檔案或 JSON 載入設定 | `claude --settings ./settings.json` |
| `--plugin-dir` | 從目錄載入外掛（可重複使用） | `claude --plugin-dir ./my-plugin` |

### 多目錄範例

```bash
# 在多個專案目錄中進行工作
claude --add-dir ../frontend ../backend ../shared "find all API endpoints"

# 載入自定義設定
claude --settings '{"model":"opus","verbose":true}' "complex task"
```

## MCP 配置

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `--mcp-config` | 從 JSON 載入 MCP servers | `claude --mcp-config ./mcp.json` |
| `--strict-mcp-config` | 僅使用指定的 MCP config | `claude --strict-mcp-config --mcp-config ./mcp.json` |
| `--channels` | 訂閱 MCP channel 外掛 | `claude --channels discord,telegram` |

### MCP 範例

```bash
# 載入 GitHub MCP server
claude --mcp-config ./github-mcp.json "list open PRs"

# 嚴格模式 - 僅使用指定的 servers
claude --strict-mcp-config --mcp-config ./production-mcp.json "deploy to staging"
```

## Session 管理

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `--session-id` | 使用特定的 session ID (UUID) | `claude --session-id "550e8400-..."` |
| `--fork-session` | 恢復時建立新的 session | `claude --resume abc123 --fork-session` |

### Session 範例

```bash
# 繼續最後一次對話
claude -c

# 恢復具名的 session
claude -r "feature-auth" "continue implementing login"

# 分叉 session 以進行實驗
claude --resume feature-auth --fork-session "try alternative approach"

# 使用特定的 session ID
claude --session-id "550e8400-e29b-41d4-a716-446655440000" "continue"
```

### Session Fork

從現有的 session 建立一個分支以進行實驗：

```bash
# 分叉一個 session 以嘗試不同的方法
claude --resume abc123 --fork-session "try alternative implementation"

# 帶有自定義訊息的分叉
claude -r "feature-auth" --fork-session "test with different architecture"
```

**使用案例：**
- 在不遺失原始 session 的情況下嘗試替代實作方式
- 並行實驗不同的方法
- 從成功的成果建立分支以進行變體開發
- 在不影響主 session 的情況下測試破壞性變更

原始 session 將保持不變，而分叉出的內容會成為一個新的獨立 session。

### 專案狀態清除（v2.1.126+）

`claude project purge` 會刪除專案的所有本地 Claude Code 狀態——逐字記錄、任務列表、除錯日誌、檔案編輯歷史、提示詞歷史紀錄，以及 `~/.claude.json` 項目。先使用 `--dry-run` 預覽刪除內容；`--all` 則會遍歷機器上的每個專案。

```bash
# 預覽將被刪除的內容（安全）
claude project purge ~/work/repo --dry-run

# 刪除特定專案的狀態，不提示確認
claude project purge ~/work/repo --yes

# 以互動方式遍歷每個專案
claude project purge --all --interactive
```

## 進階功能

| Flag | 說明 | 範例 |
|------|-------------|---------|
| `--chrome` | 啟用 Chrome 瀏覽器整合 | `claude --chrome` |
| `--no-chrome` | 停用 Chrome 瀏覽器整合 | `claude --no-chrome` |
| `--ide` | 若可用則自動連接至 IDE | `claude --ide` |
| `--max-turns` | 限制代理回合數（非互動式） | `claude -p --max-turns 3 "query"` |
| `--debug` | 啟用帶有篩選功能的除錯模式 | `claude --debug "api,mcp"` |
| `--enable-lsp-logging` | 啟用詳細的 LSP 紀錄 | `claude --enable-lsp-logging` |
| `--betas` | 用於 API 請求的 Beta 標頭 | `claude --betas interleaved-thinking` |
| `--plugin-dir` | 從目錄載入外掛（可重複使用） | `claude --plugin-dir ./my-plugin` |
| `--enable-auto-mode` | 解鎖自動權限模式 | `claude --enable-auto-mode` |
| `--effort` | 設定思考努力程度 | `claude --effort high` |
| `--bare` | 極簡模式（跳過 hooks、skills、plugins、MCP、自動 memory、CLAUDE.md） | `claude --bare` |
| `--channels` | 訂閱 MCP channel 外掛 | `claude --channels discord` |
| `--tmux` | 為 worktree 建立 tmux session | `claude --tmux` |
| `--fork-session` | 恢復時建立新的 session ID | `claude --resume abc --fork-session` |
| `--max-budget-usd` | 最大支出限制（列印模式） | `claude -p --max-budget-usd 5.00 "query"` |
| `--json-schema` | 驗證 JSON 輸出 | `claude -p --json-schema '{"type":"object"}' "q"` |

### 平台與佈景主題備註（v2.1.112）

- **Windows 上的 PowerShell 工具**：Windows 上正在推出專屬的 PowerShell 工具，可透過環境變數控制。
- **自動（配合終端機）佈景主題**：新的「自動（配合終端機）」佈景主題會同步 Claude Code 的明暗外觀與您的終端機設定。
- **更安靜的權限提示**：唯讀的 `Bash` 呼叫與 `Glob` 樣式不再觸發權限提示。

### 進階範例

```bash
# 限制自主行為
claude -p --max-turns 5 "refactor this module"

# 除錯 API 呼叫
claude --debug "api" "test query"

# 啟用 IDE 整合
claude --ide "help me with this file"
```

## Agents 配置

`--agents` 旗標接受一個 JSON 物件，用於定義該會話的自定義子代理。

### Agents JSON 格式

```json
{
  "agent-name": {
    "description": "必填：何時要呼叫此代理",
    "prompt": "必填：此代理的系統提示詞",
    "tools": ["選填", "工具", "陣列"],
    "model": "選填：sonnet|opus|haiku"
  }
}
```

**必填欄位：**
- `description` - 以自然語言描述何時使用此代理
- `prompt` - 定義代理角色與行為的系統提示詞

**選填欄位：**
- `tools` - 可用工具的陣列（若省略則繼承所有工具）
  - 格式：`["Read", "Grep", "Glob", "Bash"]`
- `model` - 使用的模型：`sonnet`、`opus` 或 `haiku`

### 完整 Agents 範例

```json
{
  "code-reviewer": {
    "description": "專家級程式碼審查員。在程式碼變更後主動使用。",
    "prompt": "你是一位資深程式碼審查員。專注於程式碼品質、安全性與最佳實務。",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "針對錯誤與測試失敗的除錯專家。",
    "prompt": "你是一位專家級除錯員。分析錯誤、識別根本原因並提供修復方案。",
    "tools": ["Read", "Edit", "Bash", "Grep"],
    "model": "opus"
  },
  "documenter": {
    "description": "用於生成指南的文件的專家。",
    "prompt": "你是一位技術作家。建立清晰且全面的文件。",
    "tools": ["Read", "Write"],
    "model": "haiku"
  }
}
```

### Agents 命令範例

```bash
# 行內定義自定義代理
claude --agents '{
  "security-auditor": {
    "description": "用於漏洞分析的安全專家",
    "prompt": "你是一位安全專家。找出漏洞並建議修復方法。",
    "tools": ["Read", "Grep", "Glob"],
    "model": "opus"
  }
}' "audit this codebase for security issues"

# 從檔案載入代理
claude --agents "$(cat ~/.claude/agents.json)" "review the auth module"

# 與其他旗標結合使用
claude -p --agents "$(cat agents.json)" --model sonnet "analyze performance"
```

### Agent 優先順序

當存在多個代理定義時，它們將依據以下優先順序載入：
1. **CLI 定義** (`--agents` 旗標) - 僅限該會話
2. **專案層級** (`.claude/agents/`) - 目前專案
3. **使用者層級** (`~/.claude/agents/`) - 所有專案

CLI 定義的代理會覆蓋該會話中的專案與使用者代理。專案層級的代理在名稱衝突時會覆蓋使用者層級的代理。完整的優先順序表（含外掛層級代理）請參閱 [Lesson 04 — Subagents](../04-subagents/README.md#file-locations)。

### Agent View（`claude agents`，v2.1.139+）

> **研究預覽版** — 此功能已穩定到足以日常使用，但可能仍有所變動。

`claude agents` 會開啟 **Agent View**——一個列出機器上所有 Claude Code 會話及其目前狀態（`running`、`blocked on you`、`done`）的單一清單。當您執行背景代理、排程任務或透過 `--bg` 啟動的會話時，它是取代多個終端機分頁的替代方案。

```bash
# 開啟 Agent View
claude agents
```

從 Agent View 派發會話（或透過 `claude --bg <prompt>`）時，可以傳入與 `claude` 本身相同的配置旗標。為 Agent View 派發路徑引入的旗標：

| Flag | 版本 | 說明 |
|------|-------|-------------|
| `--cwd <path>` | v2.1.141 | 將會話清單（或新會話）限定於特定工作目錄 |
| `--add-dir <path>` | v2.1.142 | 為派發的會話新增工作區目錄 |
| `--settings <path>` | v2.1.142 | 為派發的會話使用特定的 `settings.json` |
| `--mcp-config <path>` | v2.1.142 | 為派發的會話使用特定的 MCP 配置 |
| `--plugin-dir <path>` | v2.1.142 | 為派發的會話使用特定的外掛目錄 |
| `--permission-mode <mode>` | v2.1.142 | 為派發的會話設定權限模式（`plan`、`acceptEdits`、`auto` 等） |
| `--model <model>` | v2.1.142 | 為派發的會話固定模型 |
| `--effort <level>` | v2.1.142 | 為派發的會話固定努力層級（`low`/`medium`/`high`/`xhigh`/`max`） |
| `--dangerously-skip-permissions` | v2.1.142 | 以不提示權限的方式執行派發的會話（僅在沙箱中使用） |
| `--json` | v2.1.145 | 以機器可讀的 JSON 格式輸出代理清單，供腳本使用（狀態列、會話選擇器、tmux-resurrect 整合） |

完成工作但仍有背景 shell 開啟的會話，會從「Working」移至「Completed」（v2.1.141 修正）。在已附加的代理會話中，`Shift+Tab` 可循環切換權限模式，包括自動模式（v2.1.143）。

**固定會話** — 在 `claude agents` 中對會話按下 `Ctrl+T` 可固定它（v2.1.147）。被固定的背景會話在閒置時保持存活，並在 Claude Code 更新時就地重啟，且只有在記憶體壓力下，才會在非固定會話之後被釋放。（此 `Ctrl+T` 僅在 Agent View 中有效；在主會話中則是切換任務列表檢視。）

---

## 高價值使用案例

### 1. CI/CD 整合

在您的 CI/CD 流水線中使用 Claude Code 進行自動化程式碼審查、測試與文件撰寫。

**GitHub Actions 範例：**

```yaml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 1 \
            "Review the changes in this PR for:
            - Security vulnerabilities
            - Performance issues
            - Code quality
            Output as JSON with 'issues' array" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            // Process and post review comments
```

**Jenkins Pipeline：**

```groovy
pipeline {
    agent any
    stages {
        stage('AI Review') {
            steps {
                sh '''
                    claude -p --output-format json \
                      --max-turns 3 \
                      "Analyze test coverage and suggest missing tests" \
                      > coverage-analysis.json
                '''
            }
        }
    }
}
```

**無介面 `ultrareview`（v2.1.120+）：**

```yaml
# .github/workflows/ultrareview.yml
- name: Claude ultrareview
  run: claude ultrareview ${{ github.event.pull_request.number }} --json > review.json
```

`claude ultrareview` 在審查結果乾淨時退出碼為 0，有發現問題時為 1，因此可作為 PR 的直接把關工具。使用 `--timeout <minutes>` 可覆蓋預設的 30 分鐘限制。

### 2. 腳本管線 (Script Piping)

透過 Claude 處理檔案、日誌與數據進行分析。

**日誌分析：**

```bash
# 分析錯誤日誌
tail -1000 /var/log/app/error.log | claude -p "summarize these errors and suggest fixes"

# 在存取日誌中尋找模式
cat access.log | claude -p "identify suspicious access patterns"

# 分析 git 歷史紀錄
git log --oneline -50 | claude -p "summarize recent development activity"
```

**程式碼處理：**

```bash
# 審查特定檔案
cat src/auth.ts | claude -p "review this authentication code for security issues"

# 生成文件
cat src/api/*.ts | claude -p "generate API documentation in markdown"

# 尋找 TODO 並排列優先順序
grep -r "TODO" src/ | claude -p "prioritize these TODOs by importance"
```

### 3. 多會話工作流程

透過多個對話執行緒管理複雜專案。

```bash
# 開始一個功能分支會話
claude -r "feature-auth" "let's implement user authentication"

# 稍後，繼續該會話
claude -r "feature-auth" "add password reset functionality"

# 分叉 (Fork) 以嘗試另一種方法
claude --resume feature-auth --fork-session "try OAuth instead"

# 在不同的功能會話之間切換
claude -r "feature-payments" "continue with Stripe integration"
```

### 4. 自定義代理配置

為您團隊的工作流程定義專門的代理。

```bash
# 將代理配置儲存至檔案
cat > ~/.claude/agents.json << 'EOF'
{
  "reviewer": {
    "description": "Code reviewer for PR reviews",
    "prompt": "Review code for quality, security, and maintainability.",
    "model": "opus"
  },
  "documenter": {
    "description": "文件專家",
    "prompt": "生成清晰且全面的文件。",
    "model": "sonnet"
  },
  "refactorer": {
    "description": "程式碼重構專家",
    "prompt": "建議並實作乾淨的程式碼重構。",
    "tools": ["Read", "Edit", "Glob"]
  }
}
EOF

# 在會話中使用代理
claude --agents "$(cat ~/.claude/agents.json)" "review the auth module"
```

### 5. 批次處理

使用一致的設定處理多個查詢。

```bash
# 處理多個檔案
for file in src/*.ts; do
  echo "Processing $file..."
  claude -p --model haiku "summarize this file: $(cat $file)" >> summaries.md
done

# 批次程式碼審查
find src -name "*.py" -exec sh -c '
  echo "## $1" >> review.md
  cat "$1" | claude -p "brief code review" >> review.md
' _ {} \;

# 為所有模組生成測試
for module in $(ls src/modules/); do
  claude -p "generate unit tests for src/modules/$module" > "tests/$module.test.ts"
done
```

### 6. 安全意識開發

使用權限控制以確保安全操作。

```bash
# 唯讀安全審查
claude --permission-mode plan \
  --tools "Read,Grep,Glob" \
  "audit this codebase for security vulnerabilities"

# 封鎖危險指令
claude --disallowedTools "Bash(rm:*)" "Bash(curl:*)" "Bash(wget:*)" \
  "help me clean up this project"

# 受限自動化
claude -p --max-turns 2 \
  --allowedTools "Read" "Glob" \
  "find all hardcoded credentials"
```

### 7. JSON API 整合

將 Claude 作為可程式化的 API，並搭配 `jq` 進行解析。

```bash
# 獲取結構化分析
claude -p --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array"},"complexity":{"type":"string"}}}' \
  "analyze main.py and return function list with complexity rating"

# 整合 jq 進行處理
claude -p --output-format json "list all API endpoints" | jq '.endpoints[]'

# 在腳本中使用
RESULT=$(claude -p --output-format json "is this code secure? answer with {secure: boolean, issues: []}" < code.py)
if echo "$RESULT" | jq -e '.secure == false' > /dev/null; then
  echo "Security issues found!"
  echo "$RESULT" | jq '.issues[]'
fi
```

### jq 解析範例

使用 `jq` 解析並處理 Claude 的 JSON 輸出：

```bash
# 提取特定欄位
claude -p --output-format json "analyze this code" | jq '.result'

# 過濾陣列元素
claude -p --output-format json "list issues" | jq -r '.issues[] | select(.severity=="high")'

# 提取多個欄位
claude -p --output-format json "describe the project" | jq -r '.{name, version, description}'

# 轉換為 CSV
claude -p --output-format json "list functions" | jq -r '.functions[] | [.name, .lineCount] | @csv'

# 條件式處理
claude -p --output-format json "check security" | jq 'if .vulnerabilities | length > 0 then "UNSAFE" else "SAFE" end'

# 提取巢狀值
claude -p --output-format json "analyze performance" | jq '.metrics.cpu.usage'

# 處理整個陣列
claude -p --output-format json "find todos" | jq '.todos | length'

# 轉換輸出
claude -p --output-format json "list improvements" | jq 'map({title: .title, priority: .priority})'
```

---

## 模型

Claude Code 支援具有不同能力的複數模型：

| 模型 | ID | 上下文視窗 | 備註 |
|-------|-----|----------------|-------|
| Opus 4.7 | `claude-opus-4-7` | 1M tokens（1M 上下文修正於 v2.1.117 落地） | 能力最強，具備適應性努力層級；`xhigh` 為 Claude Code 自 Opus 4.7 發布（2026-04-16）以來的預設努力層級 |
| Sonnet 4.6 | `claude-sonnet-4-6` | 1M tokens | 速度與能力的平衡；Pro/Max 訂閱者的預設努力層級在 v2.1.117 從 `medium` 提升至 `high` |
| Haiku 4.5 | `claude-haiku-4-5` | 1M tokens | 最快，適合快速任務 |

### 模型選擇

```bash
# 使用簡短名稱
claude --model opus "complex architectural review"
claude --model sonnet "implement this feature"
claude --model haiku -p "format this JSON"

# 使用 opusplan 別名 (Opus 規劃，Sonnet 執行)
claude --model opusplan "design and implement the API"

# 在會話期間切換快速模式
/fast
```

> **Fast Mode 預設切換至 Opus 4.7（v2.1.142）**：自 v2.1.142 起，`/fast` 預設使用 Opus 4.7（先前為 Opus 4.6）。若要將 Fast Mode 切回 Opus 4.6，請匯出 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`。

### 努力層級（Opus 4.7）

Opus 4.7 支援具備努力層級的適應性推理，從輕到重依序為：`low`（○）、`medium`（◐）、`high`（●）、`xhigh`（自 Opus 4.7 發布 2026-04-16 起為 Claude Code 的預設值）以及 `max`（僅限 Opus 4.7）。在 Opus 4.6 / Sonnet 4.6 上，Pro/Max 訂閱者的預設努力層級在 v2.1.117 從 `medium` 提升至 `high`。

```bash
# 透過 CLI 旗標設定努力層級
claude --effort xhigh "complex review"

# 透過斜線命令設定努力層級
/effort xhigh

# 透過環境變數設定努力層級
export CLAUDE_CODE_EFFORT_LEVEL=xhigh   # low, medium, high, xhigh（Opus 4.7 的預設值），或 max（僅限 Opus 4.7）
```

提示詞中的 "ultrathink" 關鍵字會啟動深度推理。`max` 努力層級為 Opus 4.7 專屬。

---

## 關鍵環境變數

| 變數 | 描述 |
|----------|-------------|
| `ANTHROPIC_API_KEY` | 用於身分驗證的 API key |
| `ANTHROPIC_MODEL` | 覆蓋預設模型 |
| `ANTHROPIC_CUSTOM_MODEL_OPTION` | 用於 API 的自定義模型選項 |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | 覆蓋預設 Opus 模型 ID |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 覆蓋預設 Sonnet 模型 ID |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | 覆蓋預設 Haiku 模型 ID |
| `MAX_THINKING_TOKENS` | 設定擴展思考的 token 預算 |
| `CLAUDE_CODE_EFFORT_LEVEL` | 設定努力層級 (`low`/`medium`/`high`/`xhigh`/`max`) — `xhigh` 為 Opus 4.7 的預設值；`max` 僅限 Opus 4.7 |
| `CLAUDE_CODE_SIMPLE` | 極簡模式，由 `--bare` 旗標設定 |
| `CLAUDE_CODE_DISABLE_AUTO_MEMORY` | 停用自動 CLAUDE.md 更新 |
| `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` | 停用背景任務執行 |
| `CLAUDE_CODE_DISABLE_CRON` | 停用排程/cron 任務 |
| `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` | 停用 git 相關指令 |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` | 停用終端機標題更新 |
| `CLAUDE_CODE_DISABLE_1M_CONTEXT` | 停用 1M token 上下文視窗 |
| `CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK` | 停用非串流回退機制 |
| `CLAUDE_CODE_ENABLE_TASKS` | 啟用任務列表功能 |
| `CLAUDE_CODE_TASK_LIST_ID` | 跨會話共用的具名任務目錄 |
| `CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION` | 切換提示詞建議 (`true`/`false`) |
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` | 啟用實驗性代理團隊 |
| `CLAUDE_CODE_NEW_INIT` | 使用新的初始化流程 |
| `CLAUDE_CODE_SUBAGENT_MODEL` | 子代理執行的模型 |
| `CLAUDE_CODE_PLUGIN_SEED_DIR` | 外掛種子檔案目錄 |
| `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` | 從子程序中清除的環境變數 |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | 覆蓋自動壓縮百分比 |
| `CLAUDE_STREAM_IDLE_TIMEOUT_MS` | 串流閒置逾時（毫秒） |
| `SLASH_COMMAND_TOOL_CHAR_BUDGET` | 斜線命令工具的字元預算 |
| `ENABLE_TOOL_SEARCH` | 啟用工具搜尋功能 |
| `MAX_MCP_OUTPUT_TOKENS` | MCP 工具輸出的最大 token 數 |
| `CLAUDE_CODE_PERFORCE_MODE` | 設定為 `1` 以啟用 Perforce 模式 — 預設將檔案視為唯讀（適用於 Perforce/P4 版本控制工作流程）（新增於 v2.1.98） |
| `DISABLE_UPDATES` | 封鎖所有更新路徑，包含手動 `claude update`。比 `DISABLE_AUTOUPDATER` 更嚴格，後者僅封鎖背景自動更新器（v2.1.118+） |
| `CLAUDE_CODE_HIDE_CWD` | 設定為 `1` 時，在啟動 logo 中隱藏目前工作目錄（隱私/螢幕分享使用）（v2.1.119+） |
| `CLAUDE_CODE_FORK_SUBAGENT` | 設定為 `1` 以在外部建置版本（Bedrock、Vertex、Foundry）上啟用分叉子代理。對直接使用 Anthropic API 的版本無效，因為分叉子代理已正式發布（v2.1.117+） |
| `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` | 設定為 `1` 以停用全螢幕替代畫面渲染器；會話將保留在正常的終端機捲動區域中。在將逐字記錄透過管線輸出至日誌或與 `script(1)` 配合使用時很有用（v2.1.132+） |
| `CLAUDE_CODE_SESSION_ID` | 在 Claude Code 啟動的每個 Bash 工具子程序中設定；等於 hook 輸入 JSON 中的 `session_id`。可用於將 bash 日誌與 hook 遙測資料關聯（v2.1.132+） |
| `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` | 設定為 `1` 以為擷取 OpenTelemetry 資料的組織重新啟用 Anthropic 的會話品質調查。在 OTEL 部署中預設關閉（v2.1.136+） |
| `OTEL_LOG_TOOL_DETAILS` | 設定為 `1` 以在 OpenTelemetry 事件中取消遮蔽自定義和 MCP 指令名稱（v2.1.117+）。預設仍會遮蔽。 |
| `ANTHROPIC_BEDROCK_SERVICE_TIER` | 選擇 Bedrock 服務層級：`default`、`flex` 或 `priority`（v2.1.122+） |
| `AI_AGENT` | 在子程序中自動設定，讓外部 CLI（例如 `gh`）能將流量歸因於 Claude Code（v2.1.120+） |
| `CLAUDE_CODE_FORCE_SYNC_OUTPUT` | 設定為 `1` 以在自動偵測失誤的終端機（例如 Emacs `eat`）上強制同步輸出（v2.1.129+） |
| `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` | 設定為 `1` 以為 Homebrew/WinGet 安裝版本啟用背景升級（這些安裝版本通常不會自動更新）（v2.1.129+） |
| `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` | 設定為 `1` 以在 `ANTHROPIC_BASE_URL` 已設定時選擇性開啟閘道 `/v1/models` 探索。若未設定，`/model` 顯示內建靜態清單（v2.1.129+） |
| `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` | 設定為 `1` 以將 Fast Mode（`/fast`）固定回 Opus 4.6。預設值已在 v2.1.142 切換為 Opus 4.7。 |

> **`ENABLE_TOOL_SEARCH` 在 Vertex AI 上（v2.1.119+）**：工具搜尋在 **Google Cloud Vertex AI** 部署上**預設停用**。想在 Vertex 上使用工具搜尋功能的使用者，必須使用 `export ENABLE_TOOL_SEARCH=true` 明確選擇開啟。在直接使用 Anthropic API 時，工具搜尋仍預設啟用。

---

## 快速參考

### 最常用的命令

```bash
# 互動式會話
claude

# 快速提問
claude -p "how do I..."

# 繼續對話
claude -c

# 處理檔案
cat file.py | claude -p "review this"

# 用於腳本的 JSON 輸出
claude -p --output-format json "query"
```

### 參數組合

| 使用情境 | 命令 |
|----------|---------|
| 快速程式碼審查 | `cat file \| claude -p "review"` |
| 結構化輸出 | `claude -p --output-format json "query"` |
| 安全探索 | `claude --permission-mode plan` |
| 具備安全性的自主模式 | `claude --enable-auto-mode --permission-mode auto` |
| CI/CD 整合 | `claude -p --max-turns 3 --output-format json` |
| 恢復工作 | `claude -r "session-name"` |
| 自定義模型 | `claude --model opus "complex task"` |
| 精簡模式 | `claude --bare "quick query"` |
| 預算限制執行 | `claude -p --max-budget-usd 2.00 "analyze code"` |

---

## 疑難排解

### Command Not Found

**問題：** `claude: command not found`

**解決方案：**
- 安裝 Claude Code：`npm install -g @anthropic-ai/claude-code`
- 檢查 PATH 是否包含 npm 全域 bin 目錄
- 嘗試使用完整路徑執行：`npx claude`

### API Key 問題

**問題：** 身分驗證失敗

**解決方案：**
- 設定 API key：`export ANTHROPIC_API_KEY=your-key`
- 檢查 key 是否有效且有足夠的額度
- 確認該 key 對於所請求模型的權限

### Session Not Found

**問題：** 無法恢復會話

**解決方案：**
- 列出可用會話以尋找正確的名稱/ID
- 會話可能會在一段時間不活動後過期
- 使用 `-c` 來繼續最近的會話

### 輸出格式問題

**問題：** JSON 輸出格式錯誤

**解決方案：**
- 使用 `--json-schema` 來強制執行結構
- 在提示詞中加入明確的 JSON 指令
- 使用 `--output-format json`（而不僅僅是在提示詞中要求 JSON）

### Permission Denied

**問題：** 工具執行被阻擋

**解決方案：**
- 檢查 `--permission-mode` 設定
- 檢查 `--allowedTools` 與 `--disallowedTools` 旗標
- 使用 `--dangerously-skip-permissions` 進行自動化（請謹慎使用）

---

## 延伸資源

- **[Official CLI Reference](https://code.claude.com/docs/en/cli-reference)** - 完整的指令參考
- **[Headless Mode Documentation](https://code.claude.com/docs/en/headless)** - 自動化執行
- **[Slash Commands](../01-slash-commands/)** - Claude 內部的自定義快捷鍵
- **[Memory Guide](../02-memory/)** - 透過 CLAUDE.md 實現持久化上下文
- **[MCP Protocol](../05-mcp/)** - 外部工具整合
- **[Advanced Features](../09-advanced-features/)** - 規劃模式、延伸思考
- **[Subagents Guide](../04-subagents/)** - 委派任務執行

---

*屬於 [Claude How To](../) 指南系列的一部分*

---
**最後更新日期**：2026 年 5 月 25 日
**Claude Code 版本**：2.1.150
**來源**：
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/agent-view
- https://www.anthropic.com/news/claude-opus-4-7
- https://github.com/anthropics/claude-code/releases/tag/v2.1.113
- https://github.com/anthropics/claude-code/releases/tag/v2.1.116
- https://github.com/anthropics/claude-code/releases/tag/v2.1.117
- https://github.com/anthropics/claude-code/releases/tag/v2.1.118
- https://github.com/anthropics/claude-code/releases/tag/v2.1.131
- https://github.com/anthropics/claude-code/releases/tag/v2.1.138
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/releases/tag/v2.1.141
- https://github.com/anthropics/claude-code/releases/tag/v2.1.142
- https://github.com/anthropics/claude-code/releases/tag/v2.1.143
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
**相容模型**：Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
