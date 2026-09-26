<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hooks 鉤子

Hooks 是自動化腳本，會在 Claude Code 工作階段期間針對特定事件執行。它們可以實現自動化、驗證、權限管理以及自訂工作流程。

## 概觀

Hooks 是自動化動作（shell 命令、HTTP webhooks、LLM 提示詞、MCP 工具呼叫或子代理評估），當 Claude Code 中發生特定事件時會自動執行。它們接收 JSON 輸入，並透過結束代碼（exit codes）和 JSON 輸出進行結果通訊。

**關鍵特性：**
- 事件驅動的自動化
- 基於 JSON 的輸入/輸出
- 支援 `command`、`http`、`mcp_tool`、`prompt` 和 `agent` 類型的 hook
- 針對特定工具的 pattern matching

## 設定

Hooks 在設定檔中以特定結構進行設定：

- `~/.claude/settings.json` - 使用者設定（所有專案）
- `.claude/settings.json` - 專案設定（可共用、已提交）
- `.claude/settings.local.json` - 本地專案設定（未提交）
- Managed policy - 組織範圍的設定
- Plugin `hooks/hooks.json` - Plugin 範圍的 hooks
- Skill/Agent frontmatter - 元件生命週期 hooks

### 基本設定結構

```json
{
  "hooks": {
    "EventName": [
      {
        "matcher": "ToolPattern",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here",
            "timeout": 60
          }
        ]
      }
    ]
  }
}
```

**關鍵欄位：**

| 欄位 | 說明 | 範例 |
|-------|-------------|---------|
| `matcher` | 用於比對工具名稱的模式（區分大小寫） | `"Write"`, `"Edit\|Write"`, `"*"` |
| `hooks` | hook 定義的陣列 | `[{ "type": "command", ... }]` |
| `type` | Hook 類型：`"command"` (bash)、`"prompt"` (LLM)、`"http"` (webhook)、`"mcp_tool"` (MCP 工具呼叫，v2.1.118+) 或 `"agent"` (subagent) | `"command"` |
| `command` | 要執行的 shell 命令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 選填的逾時秒數。預設值：command/http/mcp_tool 為 600，prompt 為 30，agent 為 60。 | `30` |
| `once` | 若為 `true`，則每個工作階段僅執行一次 hook | `true` |
| `async` | 若為 `true`，則在背景執行而不阻斷 | `true` |
| `asyncRewake` | 若為 `true`，則在背景執行，並在結束代碼為 2 時喚醒 Claude。隱含 `async`。 | `true` |
| `shell` | 接受 `"bash"` 或 `"powershell"`。預設為 `"bash"`；在未安裝 Git Bash 的 Windows 上則預設為 `"powershell"`。 | `"bash"` |
| `statusMessage` | hook 執行期間顯示的自訂 spinner 訊息 | `"Formatting…"` |

> **注意**：部分事件會降低預設逾時。`UserPromptSubmit` 會將 `command`、`http` 與 `mcp_tool` 的預設值降為 30 秒，`MessageDisplay` 則降為 10 秒。`SessionEnd` hooks 共用 1.5 秒的預算；若您的設定為個別 hook 指定了更長的 `timeout`，Claude Code 會將該預算提高至相同值，最多 60 秒。

### Matcher 模式

| 模式 | 說明 | 範例 |
|---------|-------------|---------|
| 精確字串 | 比對特定工具 | `"Write"` |
| Regex 模式 | 比對多個工具 | `"Edit\|Write"` |
| 以逗號分隔 | 比對清單中的任一工具（v2.1.191+） | `"Write,Edit"` |
| 萬用字元 | 比對所有工具 | `"*"` 或 `""` |
| MCP 工具 | 伺服器與工具模式 | `"mcp__memory__.*"` |

> **Matcher 採精確比對（v2.1.195+）。** 含連字號的識別碼（例如名稱中含有連字號的 MCP 工具）不會再意外以子字串比對到其他工具。像 `"Write,Edit"` 這樣以逗號分隔的 matcher 會在清單中任一工具上觸發——較早的版本會靜默地永遠不觸發它們。

**InstructionsLoaded matcher 值：**

| Matcher 值 | 說明 |
|---------------|-------------|
| `session_start` | 在工作階段啟動時載入的指令 |
| `nested_traversal` | 在巢狀目錄遍歷期間載入的指令 |
| `path_glob_match` | 透過路徑 glob 模式比對載入的指令 |

### 使用 `if` 條件縮小範圍（工具引數路徑）

`matcher` 欄位依**工具名稱**（`"Write"`、`"Edit|Write"`、`"*"`）選擇 hook。若要依工具的**引數**進一步篩選——例如只在編輯涉及 `src/` 時執行 hook，或防護機密檔案的讀取——請在個別的 hook handler 上加入 `if` 條件。這與工具名稱 matcher 不同：`matcher` 決定*哪個工具*，`if` 決定*哪一次呼叫*。

`if` 使用[權限規則語法](https://code.claude.com/docs/en/permissions)（`ToolName(pattern)`），會同時針對工具名稱**與**其引數進行評估。對於 `Read`/`Edit`/`Write`，路徑模式遵循 gitignore 語意，錨點與權限規則相同：像 `.env` 這樣的單純名稱會比對任何深度，`src/**` 相對於目前目錄，`/src/**` 相對於專案根目錄，`~/...` 相對於您的家目錄，而 `//...` 則是絕對檔案系統路徑。

`if` 欄位位於 **hook handler 層級**——與 `type` 和 `command` 同層、位於 `hooks` 陣列內——而不是放在 `matcher` 上：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "if": "Edit(src/**)",
            "command": "./hooks/lint-src.sh"
          }
        ]
      },
      {
        "matcher": "Read",
        "hooks": [
          {
            "type": "command",
            "if": "Read(.env)",
            "command": "./hooks/block-secret-read.sh"
          }
        ]
      }
    ]
  }
}
```

有效的 `if` 模式範例：`Edit(src/**)`（`src/` 下的編輯）、`Read(~/.ssh/**)`（讀取任何 SSH 金鑰）、`Read(.env)`（目前目錄或其下任何 `.env`）、`Bash(git push *)`（僅限 `git push` 子命令）。

> **v2.1.214 更新**：hook `if` 條件中的單一區段 `dir/**` 模式（例如 `Edit(src/**)`）現在只會比對 `<cwd>/dir`——而不是樹狀結構中任何深度的該目錄。先前 `src/**` 也會比對 `foo/src/**`。若需要任意深度的比對，請使用 `**/dir/**`。**重要**：此範圍縮小僅適用於 hook 的 `if:` 條件與允許規則的自動核准——deny/ask 權限規則仍會在任何深度比對 `dir/**`。

## Hook 類型

Claude Code 支援五種 hook 類型：

### Command Hooks

預設的 hook 類型。執行 shell 命令，並透過 JSON stdin/stdout 與結束代碼（exit codes）進行通訊。

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

#### Exec 形式（`args`）

> 於 v2.1.139 版本新增。

除了 shell 形式的 `"command": "..."` 之外，command hook 也可以透過 `args` 陣列以 `execve()` 直接啟動二進位檔。這種方式不經過 shell 解析，因此路徑中的佔位符永遠不需要加引號，且設定不受 shell 注入漏洞影響。

```json
{
  "type": "command",
  "args": ["python3", "$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py", "--strict"],
  "timeout": 60
}
```

這兩種形式**互斥** — 同時設定 `command` 和 `args` 的 hook 在設定載入時會被拒絕。當您需要管道、重新導向、`&&` 串接或 shell 展開時使用 `command`；當您只是呼叫一個帶有引數的二進位檔時使用 `args`。

### HTTP Hooks

> 於 v2.1.63 版本新增。

遠端 webhook 端點，接收與 command hooks 相同的 JSON 輸入。HTTP hooks 會向該 URL 發送 POST JSON 並接收 JSON 回應。當啟用沙盒（sandboxing）時，HTTP hooks 會透過沙盒進行路由。為了安全性，URL 中的環境變數插值需要明確的 `allowedEnvVars` 列表。

```json
{
  "hooks": {
    "PostToolUse": [{
      "type": "http",
      "url": "https://my-webhook.example.com/hook",
      "matcher": "Write"
    }]
  }
}
```

**關鍵屬性：**
- `"type": "http"` -- 將此識別為 HTTP hook
- `"url"` -- webhook 端點 URL
- 當啟用沙盒時，會透過沙盒進行路由
- URL 中的任何環境變數插值都需要明確的 `allowedEnvVars` 列表

### Prompt Hooks

由 LLM 評估的提示詞，其 hook 內容為 Claude 會進行評估的提示詞。主要用於 `Stop` 與 `SubagentStop` 事件，以進行智慧型的任務完成檢查。

```json
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```

LLM 會評估該提示詞並回傳結構化的決策（詳情請參閱[基於提示詞的 hooks](#基於提示詞的-hooks)）。

### MCP Tool Hooks

> 於 v2.1.118 版本新增。

`mcp_tool` 類型直接呼叫已設定的 MCP 工具；設定中引用 MCP 伺服器和工具名稱，而非 shell 命令或 URL。當驗證或反應邏輯已存在於您所設定的 MCP 伺服器中時，此方式非常實用。

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "mcp_tool",
    "server": "my-mcp-server",
    "tool": "validate_edit"
  }]
}
```

**關鍵屬性：**
- `"type": "mcp_tool"` -- 將此識別為 MCP tool hook
- `"server"` -- 已設定的 MCP 伺服器名稱
- `"tool"` -- 該伺服器上要呼叫的工具名稱

hook 的輸入（工具名稱、工具輸入、工作階段上下文）會作為 MCP 工具的引數傳入。請參閱 [MCP server 設定](../05-mcp/README.md) 了解如何設定 MCP 伺服器。

### Agent Hooks

基於子代理（subagent）的驗證 hook，會啟動一個專用的代理來評估條件或執行複雜的檢查。與 prompt hooks（單輪 LLM 評估）不同，agent hooks 可以使用工具並進行多步驟推理。

> **注意**：Agent hooks 仍屬實驗性功能，可能會有所變動。

```json
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```

**關鍵屬性：**
- `"type": "agent"` -- 將此識別為 agent hook
- `"prompt"` -- 給予子代理的任務描述
- 代理可以使用工具（Read, Grep, Bash 等）來執行其評估
- 回傳與 prompt hooks 類似的結構化決策

## Hook Events

Claude Code 支援 **33 個 hook events**：

| Event | 觸發時機 | Matcher 輸入 | 是否可阻斷 | 常見用途 |
|-------|---------------|---------------|-----------|------------|
| **SessionStart** | 工作階段開始/恢復/清除/壓縮 | startup/resume/clear/compact/fork | 否 | 環境設定 |
| **Setup** | 初始環境設定（每個工作階段執行一次） | (無) | 否 | 佈建工具、安裝相依套件 |
| **InstructionsLoaded** | CLAUDE.md 或規則檔案載入後 | (無) | 否 | 修改/過濾指令 |
| **UserPromptSubmit** | 使用者提交 prompt | (無) | 是 | 驗證 prompt |
| **UserPromptExpansion** | 使用者提示詞被展開時（例如 `@` 提及、斜線命令解析） | (無) | 是 | 轉換或檢視展開後的提示詞 |
| **PreToolUse** | 工具執行前 | 工具名稱 | 是 (允許/拒絕/詢問/延後) | 驗證、修改輸入 |
| **PermissionRequest** | 顯示權限對話框時 | 工具名稱 | 是 | 自動核准/拒絕 |
| **PermissionDenied** | 使用者拒絕權限提示時 | 工具名稱 | 否 | 紀錄、分析、政策執行 |
| **PostToolUse** | 工具執行成功後 | 工具名稱 | 否 | 新增上下文、回饋 |
| **PostToolUseFailure** | 工具執行失敗時 | 工具名稱 | 否 | 錯誤處理、紀錄 |
| **PostToolBatch** | 一批工具使用完成後 | (無) | 否 | 彙總報告、批次驗證 |
| **Notification** | 發送通知時 | 通知類型 | 否 | 自訂通知 |
| **MessageDisplay** | 顯示助理訊息文字時 | (無) | 否 | 轉換或隱藏顯示的訊息文字（v2.1.152） |
| **SubagentStart** | 子代理啟動時 | 代理類型名稱 | 否 | 子代理設定 |
| **SubagentStop** | 子代理結束時 | 代理類型名稱 | 是 | 子代理驗證 |
| **Stop** | Claude 完成回應時 | (無) | 是 | 任務完成檢查 |
| **StopFailure** | API 錯誤導致回合結束時 | (無) | 否 | 錯誤恢復、紀錄 |
| **TeammateIdle** | 代理團隊中的隊友閒置時 | (無) | 是 | 隊友協調 |
| **TaskCompleted** | 任務被標記為完成時 | (無) | 是 | 任務後續動作 |
| **TaskCreated** | 透過 TaskCreate 建立任務時 | (無) | 否 | 任務追蹤、紀錄 |
| **ConfigChange** | 設定檔變更時 | (無) | 是 (政策除外) | 對設定更新做出反應 |
| **CwdChanged** | 工作目錄變更時 | (無) | 否 | 特定目錄的設定 |
| **DirectoryAdded** | 在工作階段中透過 `/add-dir` 或 SDK 的 `register_repo_root` 控制請求註冊新的工作目錄時（v2.1.219） | (無) | 否 | 為新加入的目錄設定工具 |
| **FileChanged** | 被監控的檔案變更時 | (無) | 否 | 檔案監控、重新建置 |
| **PreCompact** | 上下文壓縮前 | manual/auto | 否 | 壓縮前動作 |
| **PostCompact** | 壓縮完成後 | (無) | 否 | 壓縮後動作 |
| **PreModelSwitch** | Claude Code 套用所要求的模型切換之前 | 要切換到的模型標準名稱（取自 `to_model`） | 是 | 把關或否決模型變更 |
| **PostModelSwitch** | 工作階段的模型變更之後，包括 Claude Code 自行做出的變更（例如恢復工作階段時還原模型） | 切換後的模型標準名稱（取自 `to_model`） | 否 | 記錄模型變更或對其做出反應 |
| **WorktreeCreate** | 正在建立 Worktree 時 | (無) | 是 (路徑回傳) | Worktree 初始化 |
| **WorktreeRemove** | 正在移除 Worktree 時 | (無) | 否 | Worktree 清理 |
| **Elicitation** | MCP server 要求使用者輸入時 | (無) | 是 | 輸入驗證 |
| **ElicitationResult** | 使用者回應 elicitation 時 | (無) | 是 | 回應處理 |
| **SessionEnd** | 工作階段終止時 | (無) | 否 | 清理、最終紀錄 |

`PreModelSwitch` 與 `PostModelSwitch` 需要 v2.1.251 或更新版本。兩者都會收到 `from_model` 與 `to_model`；matcher 會針對由 `to_model` 推導出的標準名稱進行評估（例如 `claude-opus-5`、`.*opus.*`）。它們的 `command`、`http` 與 `mcp_tool` 預設逾時會降為 30 秒。

> **`TaskCreated` 與 `TaskCompleted` 需要啟用待辦工具（v2.1.233）。** 這兩個
> 事件由待辦/任務追蹤工具（`TaskCreate`/`Get`/`Update`/`List`、
> `TodoWrite`）觸發，而這些工具**預設僅在 Claude 3.x 模型、Opus 4 至
> 4.7、Sonnet 4 至 4.6 以及 Haiku 4.5 上提供**。在其他模型上，這些 hooks 仍是有效的
> 設定，只是永遠不會觸發——既沒有輸出也沒有錯誤。設定
> `CLAUDE_CODE_ENABLE_TODO_TOOLS=1` 即可恢復這些工具，進而恢復這些事件。

> **PostToolUse 執行時長（v2.1.119）：** `PostToolUse` 和 `PostToolUseFailure` 的 hook 輸入現在包含 `duration_ms` — 詳情請參閱 [PostToolUse](#posttooluse) 章節。

### PreToolUse

在 Claude 建立工具參數之後且在處理之前執行。使用此功能來驗證或修改工具輸入。

**設定：**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py"
          }
        ]
      }
    ]
  }
}
```

**常見 matchers:** `Task`, `Bash`, `Glob`, `Grep`, `Read`, `Edit`, `Write`, `WebFetch`, `WebSearch`

**輸出控制：**
- `permissionDecision`: `"allow"`、`"deny"`、`"ask"` 或 `"defer"`
  - `"allow"` 會略過權限提示（需要使用者互動的工具，以及您的組織設為 `ask` 的連接器工具除外）
  - `"deny"` 會阻止該工具呼叫
  - `"ask"` 會提示使用者確認
  - `"defer"` 會正常結束，讓工具稍後可以繼續；此值會忽略 `permissionDecisionReason`、`updatedInput` 與 `additionalContext`
  - 無論 hook 回傳什麼，deny 與 ask 規則仍會被評估。當多個 `PreToolUse` hooks 意見不一致時，優先順序為 `deny` > `defer` > `ask` > `allow`
- `permissionDecisionReason`: 決策原因說明。`"allow"` 與 `"ask"` 時顯示給使用者（而非 Claude）；`"deny"` 時顯示給 Claude；`"defer"` 時忽略
- `updatedInput`: 修改後的工具輸入參數

### PostToolUse

在工具執行完成後立即執行。用於驗證、記錄日誌或將上下文回傳給 Claude。

**設定：**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py"
          }
        ]
      }
    ]
  }
}
```

**輸出控制：**
- `"block"` 決策會透過回饋提示 Claude
- `additionalContext`: 為 Claude 增加的上下文

**額外輸入欄位（v2.1.119）：**

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `duration_ms` | number | 工具執行時間（毫秒）。不含在權限提示和 PreToolUse hook 執行期間花費的時間。`PostToolUse` 和 `PostToolUseFailure` hooks 均可使用。 |

#### 可恢復的阻斷（`continueOnBlock`，v2.1.139）

預設情況下，回傳 `"decision": "block"` 的 `PostToolUse` hook 會中止目前的回合。在 hook 上設定 `"continueOnBlock": true`，可改為將拒絕作為 `tool_result` 回傳給 Claude，使模型能讀取回饋並重試或調整。

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/policy-check.py",
            "continueOnBlock": true
          }
        ]
      }
    ]
  }
}
```

當 hook 的 `reason` 是 Claude 可以採取行動的內容時（例如「此檔案為唯讀；請寫入其他位置」），使用此功能；若阻斷必須完全終止回合，則不要設定此選項。

### UserPromptSubmit

當使用者提交提示詞時執行，會在 Claude 處理之前執行。

**設定：**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py"
          }
        ]
      }
    ]
  }
}
```

**輸出控制：**
- `decision`: `"block"` 可防止進行處理
- `reason`: 若被阻擋時的說明
- `additionalContext`: 加入到提示詞中的上下文

### Stop and SubagentStop

當 Claude 完成回應 (Stop) 或子代理完成任務 (SubagentStop) 時執行。支援基於提示詞的評估，用於智慧化任務完成檢查。

**額外輸入欄位：** `Stop` 和 `SubagentStop` 鉤子在 JSON 輸入中都會收到一個 `last_assistant_message` 欄位，其中包含 Claude 或子代理在停止前的最後一則訊息。這對於評估任務完成情況非常有用。

**設定：**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate if Claude completed all requested tasks.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

> **連續阻斷安全上限（v2.1.143）：** 若 `Stop` hook 在同一回合中連續 **8 次** 回傳 `"decision": "block"`（或設定 `continue: false`），Claude Code 會短路迴圈並以警告結束工作階段。可透過環境變數 `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP=<整數>` 覆蓋此門檻（設為 `0` 可完全停用上限）。這可防止有缺陷的 Stop hook 使工作階段無限迴圈。

**回傳欄位（v2.1.163）：** `Stop` 或 `SubagentStop` hook 可以回傳 `hookSpecificOutput.additionalContext`，將回饋提供給 Claude 並**繼續該回合，而不會顯示錯誤標籤**。先前要從 Stop hook 影響模型相當不便；現在 hook 可以乾淨地注入上下文，避免舊有回饋途徑（例如 `"decision": "block"`）的錯誤標籤行為。

```json
{
  "hookSpecificOutput": {
    "hookEventName": "Stop",
    "additionalContext": "Reminder: run the test suite before declaring done."
  }
}
```

### SubagentStart

當子代理開始執行時執行。matcher 輸入為代理類型名稱，允許鉤子針對特定的子代理類型。

**設定：**
```json
{
  "hooks": {
    "SubagentStart": [
      {
        "matcher": "code-review",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/subagent-init.sh"
          }
        ]
      }
    ]
  }
}
```

### SessionStart

當工作階段開始或恢復時執行。可以用於持久化環境變數。

**Matchers:** `startup`、`resume`、`clear`、`compact`、`fork`

> **v2.1.214 更新**：分叉（fork）出的工作階段現在會回報來源 `"fork"`——先前回報的是 `"resume"`。

**特殊功能：** 使用 `CLAUDE_ENV_FILE` 來持久化環境變數（也可用於 `CwdChanged` 和 `FileChanged` 鉤子）：

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

**工作階段範圍的輸出（v2.1.152）：** `SessionStart` hook 可以回傳 JSON 來重新掃描 skills 並設定工作階段標題：

```json
{
  "reloadSkills": true,
  "hookSpecificOutput": {
    "sessionTitle": "Payments migration"
  }
}
```

頂層的 `reloadSkills: true` 會在同一個工作階段中觸發 skill 重新掃描（與 `/reload-skills` 命令的動作相同），讓 hook 剛安裝的 skills 立即可用。`hookSpecificOutput.sessionTitle` 會在啟動與恢復時設定工作階段的顯示標題。

### SessionEnd

當工作階段結束時執行，用於執行清理或最終日誌記錄。無法阻斷終止。

**Reason 欄位值：**
- `clear` - 使用者清除工作階段
- `logout` - 使用者登出
- `prompt_input_exit` - 使用者透過提示詞輸入退出
- `other` - 其他原因

**設定：**
```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-cleanup.sh\""
          }
        ]
      }
    ]
  }
}
```

### Notification 事件

通知事件的 matchers：
- `permission_prompt` - 權限請求通知
- `idle_prompt` - 閒置狀態通知
- `auth_success` - 身分驗證成功
- `elicitation_dialog` - 向使用者顯示的對話框
- `agent_needs_input` - 背景代理需要輸入（v2.1.198）
- `agent_completed` - 背景代理已完成（v2.1.198）

### PreModelSwitch

在 Claude Code 套用所要求的模型切換**之前**執行——例如當您執行 `/model`，或某個元件要求使用不同的模型時。需要 v2.1.251 或更新版本。

**Matchers：** 要切換到的模型標準名稱，由 `to_model` 推導而來。可比對精確的模型（`claude-opus-5`），或以 regex 比對一個模型系列（`.*opus.*`）。

**輸入欄位：** 除了共通欄位之外，hook 還會收到 `from_model`（切換前使用的模型）與 `to_model`（要求的模型）。

**可否阻斷：** 可以。結束代碼 `2` 會阻斷切換並將 stderr 顯示為錯誤，讓工作階段維持目前的模型。可用來把關或否決模型變更——例如讓對成本敏感的專案避開最昂貴的模型。

**逾時：** 此事件會將 `command`、`http` 與 `mcp_tool` 的預設逾時降為 30 秒。

**設定：**
```json
{
  "hooks": {
    "PreModelSwitch": [
      {
        "matcher": ".*opus.*",
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/gate-model-switch.sh"
          }
        ]
      }
    ]
  }
}
```

```bash
#!/bin/bash
# gate-model-switch.sh - refuse a switch to Opus on this project
input=$(cat)
to_model=$(echo "$input" | jq -r '.to_model')

if [[ "$to_model" == *opus* ]]; then
  echo "This project is budgeted for Sonnet; staying on the current model." >&2
  exit 2
fi

exit 0
```

### PostModelSwitch

在工作階段的模型變更**之後**執行。它也會在 Claude Code 自行做出的變更時觸發——例如在您恢復工作階段時還原先前選擇的模型——而不僅限於您所要求的切換。需要 v2.1.251 或更新版本。

**Matchers：** 與 `PreModelSwitch` 相同——由 `to_model` 推導出的標準名稱。

**輸入欄位：** `from_model` 與 `to_model`，以及共通欄位。

**可否阻斷：** 不可以。切換已經發生；hook 只能觀察並做出反應。

**逾時：** 此事件會將 `command`、`http` 與 `mcp_tool` 的預設逾時降為 30 秒。

**設定：**
```json
{
  "hooks": {
    "PostModelSwitch": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "$CLAUDE_PROJECT_DIR/.claude/hooks/log-model-switch.sh"
          }
        ]
      }
    ]
  }
}
```

```bash
#!/bin/bash
# log-model-switch.sh - append every model change to a session log
input=$(cat)
from=$(echo "$input" | jq -r '.from_model')
to=$(echo "$input" | jq -r '.to_model')

echo "$(date -Iseconds) $from -> $to" >> ~/.claude/model-switches.log
exit 0
```

## 元件範圍的 Hooks

鉤子可以附加到特定元件（skills、agents、commands）的 frontmatter 中：

**在 SKILL.md、agent.md 或 command.md 中：**

```yaml
---
name: secure-operations
description: Perform operations with security checks
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # Only run once per session
---
```

**元件鉤子支援的事件：** `PreToolUse`、`PostToolUse`、`Stop`

這允許直接在使用的元件中定義鉤子，將相關程式碼集中在一起。

### Subagent Frontmatter 中的 Hooks

當在子代理（subagent）的 frontmatter 中定義 `Stop` 鉤子時，它會自動轉換為範圍限定於該子代理的 `SubagentStop` 鉤子。這確保了停止鉤子僅在該特定子代理完成時觸發，而不是在主工作階段（session）停止時觸發。

```yaml
---
name: code-review-agent
description: Automated code review subagent
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "Verify the code review is thorough and complete."
  # The above Stop hook auto-converts to SubagentStop for this subagent
---
```

**需要工作區信任（v2.1.218）：** **專案** subagent 中的 frontmatter hooks，現在需要先對該代理檔案所在的資料夾接受工作區信任後才會執行。在 v2.1.218 之前，這些 hooks 可能會從您未信任的資料夾執行。哪些範圍可以豁免，請參閱 [subagents 文件](https://code.claude.com/docs/en/sub-agents#hooks-in-subagent-frontmatter)。

## PermissionRequest 事件

處理具有自訂輸出格式的權限請求：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "Custom message",
      "interrupt": false
    }
  }
}
```

## Hook 輸入與輸出

### JSON 輸入（透過 stdin）

所有 hooks 都透過 stdin 接收 JSON 輸入：

```json
{
  "session_id": "abc123",
  "transcript_path": "/path/to/transcript.jsonl",
  "cwd": "/current/working/directory",
  "permission_mode": "default",
  "hook_event_name": "PreToolUse",
  "tool_name": "Write",
  "tool_input": {
    "file_path": "/path/to/file.js",
    "content": "..."
  },
  "tool_use_id": "toolu_01ABC123...",
  "agent_id": "agent-abc123",
  "agent_type": "main",
  "worktree": "/path/to/worktree",
  "effort": { "level": "medium" }
}
```

**常見欄位：**

| 欄位 | 說明 |
|-------|-------------|
| `session_id` | 唯一的工作階段識別碼 |
| `transcript_path` | 對話紀錄檔案的路徑 |
| `cwd` | 目前工作目錄 |
| `prompt_id` | 正在處理之提示詞的 UUID；與 OpenTelemetry 的 `prompt.id` 屬性相對應（v2.1.196） |
| `hook_event_name` | 觸發該 hook 的事件名稱 |
| `agent_id` | 執行此 hook 的 agent 識別碼 |
| `agent_type` | agent 類型 (`"main"`、subagent 類型名稱等) |
| `worktree` | git worktree 的路徑（若 agent 在其中執行） |
| `effort.level` | （v2.1.133+）目前的努力程度：`low`、`medium`、`high`、`xhigh` 或 `max` |

### 結束代碼（Exit Codes）

| Exit Code | 意義 | 行為 |
|-----------|---------|----------|
| **0** | 成功 | 繼續執行，解析 JSON stdout |
| **2** | 阻斷性錯誤 | 阻斷操作，stderr 會顯示為錯誤 |
| **其他** | 非阻斷性錯誤 | 繼續執行，stderr 會在詳細模式下顯示 |

### JSON 輸出（stdout，exit code 0）

```json
{
  "continue": true,
  "stopReason": "Optional message if stopping",
  "suppressOutput": false,
  "systemMessage": "Optional warning message",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "File is in allowed directory",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

> **適用範圍（v2.1.121+）：** `hookSpecificOutput.updatedToolOutput` 現在適用於**所有**工具，而不只是 MCP 工具。`Bash`、`Edit`、`Read` 等工具的 `PostToolUse` hook 可在 Claude 看到之前改寫工具的輸出 — 適用於遮蔽機密、標準化 diff，或過濾雜訊命令輸出。範例（從 `Bash` 輸出中移除 ANSI 色彩碼）：
>
> ```json
> {
>   "hookSpecificOutput": {
>     "hookEventName": "PostToolUse",
>     "updatedToolOutput": "<plain-text output with ANSI escapes removed>"
>   }
> }
> ```

> **`retry`（PermissionDenied）**：使用 JSON `hookSpecificOutput.retry: true` 告知模型可以重試被拒絕的工具呼叫。

> **已棄用的 `PreToolUse` 決策形式**：對於 `PreToolUse`，頂層的 `decision` 與 `reason` 欄位已**棄用**——請改用 `hookSpecificOutput.permissionDecision`（`allow` / `deny` / `ask` / `defer`）與 `permissionDecisionReason`。決策間的優先順序為 `deny` > `defer` > `ask` > `allow`。另請注意，`suppressOutput` 雖然會被接受，但**沒有任何作用**。

#### `terminalSequence`（v2.1.141）

Hook 可以透過在 JSON 輸出中設定 `terminalSequence` 來發送原始 OSC（operating system command）跳脫序列。當 hook 回傳時，host 會將序列寫入其控制終端機 — 適用於桌面通知、視窗標題更新及終端提示音，無需擁有自己的 TTY。

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `terminalSequence` | string | 原始跳脫序列（通常為 OSC 9 / OSC 0 / OSC 777）。以原文寫入 host 終端機。 |

範例 — 當長時間任務完成時發送 OSC 9 桌面通知：

```json
{
  "terminalSequence": "]9;Task complete"
}
```

在 `Stop` hook 上設定此功能，使通知在 Claude 完成一個回合時觸發。序列支援取決於終端機；Kitty/iTerm2/Windows Terminal 支援 OSC 9。

## 環境變數

| 變數 | 可用性 | 描述 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` | 所有 hooks | 專案根目錄的絕對路徑 |
| `CLAUDE_ENV_FILE` | SessionStart, CwdChanged, FileChanged | 用於持久化環境變數的檔案路徑 |
| `CLAUDE_CODE_REMOTE` | 所有 hooks | 若在遠端環境執行，則為 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hooks | 外掛目錄的路徑 |
| `${CLAUDE_PLUGIN_DATA}` | Plugin hooks | 外掛資料目錄的路徑 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | 可設定的 SessionEnd hooks 逾時時間（以毫秒為單位，會覆蓋預設值） |
| `CLAUDE_CODE_SESSION_ID` | Bash 工具子行程（v2.1.132+） | 工作階段 UUID；與 hook 輸入 JSON 中的 `session_id` 欄位相符。用於將 bash 日誌與 hook 遙測關聯。 |
| `CLAUDE_EFFORT` | Bash 工具子行程（v2.1.133+） | 目前的努力程度（`low`/`medium`/`high`/`xhigh`/`max`）；與 hook 輸入 JSON 中的 `effort.level` 相符。 |
| `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` | 行程範圍（v2.1.143+） | Stop hook 連續阻斷導致工作階段結束前的最大次數（預設 `8`）。設為 `0` 可停用上限。 |

## 基於提示詞的 Hooks

對於 `Stop` 和 `SubagentStop` 事件，您可以使用基於 LLM 的評估：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if all tasks are complete. Return your decision.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

**LLM 回應結構：**
```json
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```

## 範例

### 範例 1：Bash 指令驗證器 (PreToolUse)

**檔案：** `.claude/hooks/validate-bash.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"\brm\s+-rf\s+/", "Blocking dangerous rm -rf / command"),
    (r"\bsudo\s+rm", "Blocking sudo rm command"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name != "Bash":
        sys.exit(0)

    command = input_data.get("tool_input", {}).get("command", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, command):
            print(message, file=sys.stderr)
            sys.exit(2)  # Exit 2 = blocking error

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**設定：**
```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\""
          }
        ]
      }
    ]
  }
}
```

### 範例 2：安全性掃描器 (PostToolUse)

**檔案：** `.claude/hooks/security-scan.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

SECRET_PATTERNS = [
    (r"password\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded password"),
    (r"api[_-]?key\s*=\s*['\"][^'\"]+['\"]", "Potential hardcoded API key"),
]

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    if tool_name not in ["Write", "Edit"]:
        sys.exit(0)

    tool_input = input_data.get("tool_input", {})
    content = tool_input.get("content", "") or tool_input.get("new_string", "")
    file_path = tool_input.get("file_path", "")

    warnings = []
    for pattern, message in SECRET_PATTERNS:
        if re.search(pattern, content, re.IGNORECASE):
            warnings.append(message)

    if warnings:
        output = {
            "hookSpecificOutput": {
                "hookEventName": "PostToolUse",
                "additionalContext": f"Security warnings for {file_path}: " + "; ".join(warnings)
            }
        }
        print(json.dumps(output))

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 範例 3：自動格式化程式碼 (PostToolUse)

**檔案：** `.claude/hooks/format-code.sh`

```bash
#!/bin/bash

# Read JSON from stdin
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# Format based on file extension
case "$FILE_PATH" in
    *.js|*.jsx|*.ts|*.tsx|*.json)
        command -v prettier &>/dev/null && prettier --write "$FILE_PATH" 2>/dev/null
        ;;
    *.py)
        command -v black &>/dev/null && black "$FILE_PATH" 2>/dev/null
        ;;
    *.go)
        command -v gofmt &>/dev/null && gofmt -w "$FILE_PATH" 2>/dev/null
        ;;
esac

exit 0
```

### 範例 4：提示詞驗證器 (UserPromptSubmit)

**檔案：** `.claude/hooks/validate-prompt.py`

```python
#!/usr/bin/env python3
import json
import sys
import re

BLOCKED_PATTERNS = [
    (r"delete\s+(all\s+)?database", "Dangerous: database deletion"),
    (r"rm\s+-rf\s+/", "Dangerous: root deletion"),
]

def main():
    input_data = json.load(sys.stdin)
    prompt = input_data.get("user_prompt", "") or input_data.get("prompt", "")

    for pattern, message in BLOCKED_PATTERNS:
        if re.search(pattern, prompt, re.IGNORECASE):
            output = {
                "decision": "block",
                "reason": f"Blocked: {message}"
            }
            print(json.dumps(output))
            sys.exit(0)

    sys.exit(0)

if __name__ == "__main__":
    main()
```

### 範例 5：智慧停止鉤子（基於提示詞）

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review if Claude completed all requested tasks. Check: 1) Were all files created/modified? 2) Were there unresolved errors? If incomplete, explain what's missing.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 範例 6：上下文使用追蹤器（鉤子組合）

結合使用 `UserPromptSubmit`（訊息前）與 `Stop`（回應後）鉤子來追蹤每次請求的 token 消耗量。

**檔案：** `.claude/hooks/context-tracker.py`

```python
#!/usr/bin/env python3
"""
Context Usage Tracker - Tracks token consumption per request.

Uses UserPromptSubmit as "pre-message" hook and Stop as "post-response" hook
to calculate the delta in token usage for each request.

Token Counting Methods:
1. Character estimation (default): ~4 chars per token, no dependencies
2. tiktoken (optional): More accurate (~90-95%), requires: pip install tiktoken
"""
import json
import os
import sys
import tempfile

# Configuration
CONTEXT_LIMIT = 128000  # Claude 的上下文視窗 (請根據您的模型進行調整)
USE_TIKTOKEN = False    # 如果已安裝 tiktoken 以獲得更高的準確度，請設為 True


def get_state_file(session_id: str) -> str:
    """取得用於儲存訊息前 token 計數的暫存檔路徑，並依工作階段進行隔離。"""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")


def count_tokens(text: str) -> int:
    """
    計算文字中的 token 數量。

    如果可用，將使用具有 p50k_base 編碼的 tiktoken（準確度約 90-95%），
    否則將退回使用字元估算法（準確度約 80-90%）。
    """
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # 退回使用估算法

    # 基於字元的估算法：英文約每 4 個字元為 1 個 token
    return len(text) // 4


def read_transcript(transcript_path: str) -> str:
    """讀取並串接 transcript 檔案中的所有內容。"""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                # 從各種訊息格式中擷取文字內容
                if "message" in entry:
                    msg = entry["message"]
                    if isinstance(msg.get("content"), str):
                        content.append(msg["content"])
                    elif isinstance(msg.get("content"), list):
                        for block in msg["content"]:
                            if isinstance(block, dict) and block.get("type") == "text":
                                content.append(block.get("text", ""))
            except json.JSONDecodeError:
                continue

    return "\n".join(content)


def handle_user_prompt_submit(data: dict) -> None:
    """訊息前鉤子 (Pre-message hook)：在請求前儲存目前的 token 計數。"""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # 儲存至暫存檔以供稍後比較
    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)


def handle_stop(data: dict) -> None:
    """回應後鉤子 (Post-response hook)：計算並回報 token 差值。"""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # 載入訊息前的計數
    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    # Calculate delta
    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    # Report usage
    method = "tiktoken" if USE_TIKTOKEN else "estimated"
    print(f"Context ({method}): ~{current_tokens:,} tokens ({percentage:.1f}% used, ~{remaining:,} remaining)", file=sys.stderr)
    if delta_tokens > 0:
        print(f"This request: ~{delta_tokens:,} tokens", file=sys.stderr)


def main():
    data = json.load(sys.stdin)
    event = data.get("hook_event_name", "")

    if event == "UserPromptSubmit":
        handle_user_prompt_submit(data)
    elif event == "Stop":
        handle_stop(data)

    sys.exit(0)


if __name__ == "__main__":
    main()
```

**設定：**
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/context-tracker.py\""
          }
        ]
      }
    ]
  }
}
```

**工作原理：**
1. `UserPromptSubmit` 在您的提示詞被處理前觸發 — 儲存目前的 token 數量
2. `Stop` 在 Claude 回應後觸發 — 計算差值並回報使用量
3. 每個工作階段透過暫存檔名中的 `session_id` 進行隔離

**Token 計數方法：**

| 方法 | 準確度 | 相依套件 | 速度 |
|--------|----------|--------------|-------|
| 字元估算 | ~80-90% | 無 | <1ms |
| tiktoken (p50k_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **注意：** Anthropic 尚未發布官方的離線 tokenizer。這兩種方法都是近似值。對話紀錄包含使用者提示詞、Claude 的回應以及工具輸出，但不包含系統提示詞或內部上下文。

### 範例 7：植入自動模式權限（一次性設定腳本）

一個一次性的設定腳本，用於在 `~/.claude/settings.json` 中植入等同於 Claude Code 自動模式基準線的安全權限規則——預設 37 個，啟用所有類別旗標時最多 63 個——不含任何鉤子，也不會記住未來的選擇。執行一次即可；可重複執行（會跳過已存在的規則）。

**檔案：** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 預覽將會新增的內容
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 套用設定
python3 09-advanced-features/setup-auto-mode-permissions.py
```

**新增內容：**

| 類別 | 範例 |
|----------|---------|
| 內建工具 | `Read(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)` |
| 選用的編輯（--include-edits） | `Edit(*)` |
| Git 讀取 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)` |
| Git 寫入（本地） | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| 套件管理工具 | `Bash(npm ci:*)`, `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(pip3 install:*)` |
| 建置與測試 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 常用 shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

**刻意排除的內容**（此腳本絕不會新增）：
- `rm -rf`, `sudo`, force push, `git reset --hard`
- `DROP TABLE`, `kubectl delete`, `terraform destroy`
- `npm publish`, `curl | bash`, production deploys

### 範例 8：學習進度記錄器（SessionEnd）

在每個 Claude Code 工作階段結束時記錄您學習了哪些模組。進度儲存在 `~/.claude-howto-progress.json` — 在 repo 外部，因此在 `git pull` 後不會被覆蓋。

**為何使用 `SessionEnd` 而非 `Stop`？**
`Stop` 在*每次* Claude 回應後觸發。`SessionEnd` 在工作階段終止時觸發一次 — 正是您想要在工作階段結束時記日誌的情況。

**為何使用 `/dev/tty` 接收輸入？**
Hook 腳本透過 `stdin` 接收 hook JSON 資料，因此互動式的 `read` 必須直接使用 `/dev/tty` 來連接終端。

**檔案：** `06-hooks/session-end.sh`

```bash
#!/usr/bin/env bash
# SessionEnd hook: prompts for modules worked on, then appends a session record
# to ~/.claude-howto-progress.json for persistent learning progress tracking.

PROGRESS_FILE="$HOME/.claude-howto-progress.json"

# Guard: only run inside this repo
if [[ "$CLAUDE_PROJECT_DIR" != *"claude-howto"* ]] && [[ "$PWD" != *"claude-howto"* ]]; then
  exit 0
fi

if [ ! -f "$PROGRESS_FILE" ]; then
  echo '{"sessions":[]}' > "$PROGRESS_FILE"
fi

DATE=$(date +"%Y-%m-%d")
TIME=$(date +"%H:%M")

echo ""
echo " Which modules did you work on? (e.g. 06,07 or press Enter to skip)"
echo " 01=Slash  02=Memory  03=Skills  04=Subagents  05=MCP"
echo " 06=Hooks  07=Plugins 08=Checkpoints 09=Advanced 10=CLI"
printf " > "
read -r INPUT </dev/tty

if [ -z "$INPUT" ] || [ "$INPUT" = "skip" ]; then
  exit 0
fi

MODULES_JSON=$(echo "$INPUT" | tr ',' '\n' | tr -d ' ' | while read -r m; do
  case "$m" in
    01) echo '"01-slash-commands"' ;;
    02) echo '"02-memory"' ;;
    03) echo '"03-skills"' ;;
    04) echo '"04-subagents"' ;;
    05) echo '"05-mcp"' ;;
    06) echo '"06-hooks"' ;;
    07) echo '"07-plugins"' ;;
    08) echo '"08-checkpoints"' ;;
    09) echo '"09-advanced-features"' ;;
    10) echo '"10-cli"' ;;
    *)  echo "\"$m\"" ;;
  esac
done | paste -sd ',' -)

printf " Notes? (optional, press Enter to skip): "
read -r NOTES </dev/tty

# Pass NOTES as a separate argument so Python handles JSON escaping —
# avoids broken JSON when notes contain quotes or backslashes.
python3 - "$PROGRESS_FILE" "$DATE" "$TIME" "$MODULES_JSON" "$NOTES" <<'PYEOF'
import sys, json

path, date, time_str, modules_raw, notes = sys.argv[1], sys.argv[2], sys.argv[3], sys.argv[4], sys.argv[5]

new_session = {
    "date": date,
    "time": time_str,
    "modules": json.loads(f"[{modules_raw}]") if modules_raw else [],
    "notes": notes,
}

with open(path, 'r') as f:
    data = json.load(f)

data.setdefault('sessions', []).append(new_session)

with open(path, 'w') as f:
    json.dump(data, f, indent=2)
PYEOF

echo " Saved to $PROGRESS_FILE"
```

**安裝** — 將腳本複製到專案的 hook 目錄，使 `settings.json` 中的路徑可以解析：

```bash
mkdir -p .claude/hooks
cp 06-hooks/session-end.sh .claude/hooks/
chmod +x .claude/hooks/session-end.sh
```

**設定**（在 `.claude/settings.json` 中）：

```json
{
  "hooks": {
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-end.sh\""
          }
        ]
      }
    ]
  }
}
```

**輸出 — `~/.claude-howto-progress.json`：**

```json
{
  "sessions": [
    {
      "date": "2026-04-18",
      "time": "14:32",
      "modules": ["06-hooks", "07-plugins"],
      "notes": "Installed first hook, tried pre-commit example"
    }
  ]
}
```

**關鍵模式說明：**

| 模式 | 重要性 |
|---------|----------------|
| `SessionEnd` 事件 | 在退出時觸發一次 — 而非像 `Stop` 每次回應後都觸發 |
| `read -r INPUT </dev/tty` | Hook 佔用 `stdin`（JSON 資料）；使用 `/dev/tty` 接收使用者輸入 |
| `$CLAUDE_PROJECT_DIR` | 可攜式路徑 — 永遠不要寫死 `/Users/yourname/...` |
| 頂部的防衛子句 | 防止全域安裝時在無關專案中執行鉤子 |
| 儲存在 repo 外部 | `~/` 路徑在 `git pull` 後仍能存活，不會覆蓋您的資料 |

**附屬工具：視覺化進度追蹤器**

如需涵蓋全部 10 個模組的完整核取方塊 UI，請在瀏覽器中開啟附帶的追蹤器：

```bash
open local-progress/index.html
```

進度儲存在瀏覽器 `localStorage` 中（永遠不會寫入 repo 內的磁碟）。使用**匯出**按鈕將快照儲存為 JSON，並使用**匯入**來還原。

## Plugin Hooks

外掛可以在其 `hooks/hooks.json` 檔案中包含鉤子：

**檔案：** `plugins/hooks/hooks.json`

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "${CLAUDE_PLUGIN_ROOT}/scripts/validate.sh"
          }
        ]
      }
    ]
  }
}
```

**外掛鉤子中的環境變數：**
- `${CLAUDE_PLUGIN_ROOT}` - 外掛目錄的路徑
- `${CLAUDE_PLUGIN_DATA}` - 外掛資料目錄的路徑

這讓外掛可以包含自訂的驗證與自動化鉤子。

## MCP 工具鉤子

MCP 工具遵循 `mcp__<server>__<tool>` 的模式：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "mcp__memory__.*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\": \"Memory operation logged\"}'"
          }
        ]
      }
    ]
  }
}
```

## 安全考量

### 免責聲明

**風險自負**：鉤子（hooks）會執行任意的 shell 命令。您須自行負責以下事項：
- 您所設定的命令
- 檔案存取/修改權限
- 可能的資料遺失或系統損壞
- 在正式環境使用前，先在安全環境中測試鉤子

### 安全筆記

- **需要工作區信任：** `statusLine` 與 `fileSuggestion` 鉤子的輸出命令現在需要先接受工作區信任後才會生效。
- **狀態列終端機尺寸（v2.1.153）：** 狀態列命令腳本現在會收到 `COLUMNS` 與 `LINES` 環境變數，讓腳本可以依終端機寬度/高度調整輸出（例如 `[ "$COLUMNS" -lt 80 ] && short_output`）。
- **HTTP 鉤子與環境變數：** HTTP 鉤子需要明確的 `allowedEnvVars` 列表，才能在 URL 中使用環境變數插值。這可以防止敏感的環境變數意外洩漏到遠端端點。
- **管理設定層級：** `disableAllHooks` 設定現在會遵循管理設定層級，這意味著組織層級的設定可以強制停用鉤子，且個人使用者無法覆蓋。
- **PowerShell 自動核准（v2.1.119）：** PowerShell 工具命令現在可以在權限模式下自動核准，與 Bash 保持一致。這為使用 PowerShell 支援 shell 工具執行 Claude Code 的 Windows 使用者帶來同等功能。
- **Bash 裸環境變數自動核准已關閉（v2.1.145）：** 在 v2.1.145 之前，形如 `FOO=bar somecommand` 的 Bash 命令（在非白名單命令前加上裸變數賦值）在白名單中只有 `FOO=bar` 本身時可能被自動核准。v2.1.145 關閉了此漏洞 — 此類命令現在會觸發權限提示。依賴此隱式允許的腳本將開始出現提示；請透過涵蓋完整命令（而非僅變數賦值）的 `Bash(...)` 權限規則明確重新允許它們。

### 最佳實務

| 應該 (Do) | 不應該 (Don't) |
|-----|-------|
| 驗證並清理所有輸入 | 盲目信任輸入資料 |
| 為 shell 變數加上引號：`"$VAR"` | 使用未加引號的變數：`$VAR` |
| 阻斷路徑穿越 (`..`) | 允許任意路徑 |
| 使用帶有 `$CLAUDE_PROJECT_DIR` 的絕對路徑 | 寫死路徑 |
| 跳過敏感檔案 (`.env`, `.git/`, keys) | 處理所有檔案 |
| 先進行隔離測試 | 部署未經測試的鉤子 |
| 為 HTTP 鉤子使用明確的 `allowedEnvVars` | 將所有環境變數暴露給 webhooks |

## 除錯

### 啟用除錯模式

使用 debug 旗標執行 Claude 以取得詳細的鉤子日誌：

```bash
claude --debug
```

### 詳細模式（Verbose Mode）

在 Claude Code 中使用 `Ctrl+O` 可啟用詳細模式，並查看鉤子的執行進度。

### 獨立測試鉤子

```bash
# 使用範例 JSON 輸入進行測試
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# 檢查結束代碼 (exit code)
echo $?
```

## 完整設定範例

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-bash.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/format-code.sh\"",
            "timeout": 30
          },
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/security-scan.py\"",
            "timeout": 10
          }
        ]
      }
    ],
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate-prompt.py\""
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "matcher": "startup",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR/.claude/hooks/session-init.sh\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Verify all tasks are complete before stopping.",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

## 鉤子執行詳情

| 項目 | 行為 |
|--------|----------|
| **Timeout** | command/http/mcp_tool 預設 600 秒（prompt 為 30 秒，agent 為 60 秒）；可針對每個 hook 進行設定 |
| **Parallelization** | 所有符合條件的鉤子將並行執行 |
| **Deduplication** | 相同的鉤子指令會進行去重 |
| **Environment** | 在目前目錄與 Claude Code 的環境中執行 |

## 疑難排解

### Hook 未執行
- 確認 JSON 設定語法正確
- 檢查 matcher 模式是否與工具名稱相符
- 確保腳本存在且具備執行權限：`chmod +x script.sh`
- 執行 `claude --debug` 以查看 hook 執行日誌
- 確認 hook 是從 stdin 讀取 JSON（而非透過命令參數）

### Hook 意外阻斷
- 使用範例 JSON 測試 hook：`echo '{"tool_name": "Write", ...}' | ./hook.py`
- 檢查結束代碼（exit code）：允許時應為 0，阻斷時應為 2
- 檢查 stderr 輸出（在結束代碼為 2 時顯示）

### JSON 解析錯誤
- 務必從 stdin 讀取，而非命令參數
- 使用正確的 JSON 解析方式（而非字串處理）
- 優雅地處理缺失欄位

## 安裝

### 步驟 1：建立 Hooks 目錄
```bash
mkdir -p ~/.claude/hooks
```

### 步驟 2：複製範例 Hooks
```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### 步驟 3：在設定檔中進行設定
編輯 `~/.claude/settings.json` 或 `.claude/settings.json` 並填入上述的 hook 設定。

## 相關概念

- **[Checkpoints 與 Rewind](../08-checkpoints/)** - 儲存與還原對話狀態
- **[Slash Commands](../01-slash-commands/)** - 建立自訂斜線命令
- **[Skills](../03-skills/)** - 可重複使用的自主能力
- **[Subagents](../04-subagents/)** - 委派任務執行
- **[Plugins](../07-plugins/)** - 綑綁的外掛套件
- **[Advanced Features](../09-advanced-features/)** - 探索進階 Claude Code 功能

## 其他資源

- **[Official Hooks Documentation](https://code.claude.com/docs/en/hooks)** - 完整的 hooks 參考文件
- **[CLI Reference](https://code.claude.com/docs/en/cli-reference)** - 命令列介面文件
- **[Memory Guide](../02-memory/)** - 持久化上下文設定指南

---
**最後更新日期**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/hooks
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/sub-agents
- https://code.claude.com/docs/en/permissions
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
