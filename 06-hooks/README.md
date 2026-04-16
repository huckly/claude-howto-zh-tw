<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 鉤子

鉤子是自動化腳本，會在 Claude Code 工作階段中的特定事件發生時執行。它們支援自動化、驗證、權限管理和自訂工作流程。

## 總覽

鉤子是自動動作（shell 命令、HTTP webhook、LLM 提示詞或子代理評估），會在 Claude Code 中特定事件發生時自動執行。它們接收 JSON 輸入，並透過結束碼和 JSON 輸出傳達結果。

**主要功能：**
- 驅動事件的自動化
- 基於 JSON 的輸入/輸出
- 支援命令、提示詞、HTTP 和代理鉤子類型
- 工具特定的鉤子模式匹配

## 設定

鉤子是在具有特定結構的設定檔案中設定的：

- `~/.claude/settings.json` - 用戶設定 (所有專案)
- `.claude/settings.json` - 專案設定 (可分享，已提交)
- `.claude/settings.local.json` - 本地專案設定 (未提交)
- 託管策略 - 組織範圍內的設定
- 外掛 `hooks/hooks.json` - 外掛範圍內的鉤子
- 技能/代理前置詞 - 元件生命週期鉤子

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

**主要欄位：**

| 欄位 | 描述 | 範例 |
|-------|-------------|---------|
| `matcher` | 用於匹配工具名稱的模式（區分大小寫） | `"Write"`, `"Edit\|Write"`, `"*"` |
| `hooks` | 鉤子定義的陣列 | `[{ "type": "command", ... }]` |
| `type` | 鉤子類型：「command」（bash）、「prompt」（LLM）、「http」（webhook）或「agent」（子代理） | `"command"` |
| `command` | 要執行的 shell 命令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 可選的逾時時間（秒），預設值為 60 | `30` |
| `once` | 如果為 `true`，則僅在每個工作階段中執行一次鉤子 | `true` |

### 模式匹配器

| 模式 | 描述 | 範例 |
|---------|-------------|---------|
| 精確字串 | 匹配特定工具 | `"Write"` |
| 正則表達式模式 | 匹配多個工具 | `"Edit\|Write"` |
| 通配符 | 匹配所有工具 | `"*"` 或 `""` |
| MCP 工具 | 伺服器和工具模式 | `"mcp__memory__.*"` |

**InstructionsLoaded 模式匹配器值：**

| 匹配器值 | 描述 |
|---------------|-------------|
| `session_start` | 工作階段啟動時載入指令 |
| `nested_traversal` | 巢狀目錄瀏覽期間載入指令 |
| `path_glob_match` | 透過路徑全域模式匹配載入指令 |

## 鉤子類型

Claude Code 支援四種鉤子類型：

### 命令鉤子

預設的鉤子類型。執行 shell 命令並透過 JSON 標準輸入/標準輸出和結束代碼進行通訊。

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

### HTTP 鉤子

> 在 v2.1.63 中新增。

遠端 webhook 端點，接收與命令鉤子相同的 JSON 輸入。HTTP 鉤子將 JSON POST 至 URL 並接收 JSON 回應。HTTP 鉤子在啟用沙盒時會經過沙盒。URL 中的環境變數插補需要明確的 `allowedEnvVars` 清單，以確保安全性。

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

**主要屬性：**
- `"type": "http"` -- 識別此為 HTTP 鉤子
- `"url"` -- webhook 端點 URL
- 在啟用沙盒時會經過沙盒
- URL 中的任何環境變數插補需要明確的 `allowedEnvVars` 清單

### 提示詞鉤子

LLM 評估的提示詞，其中鉤子內容是 Claude 評估的提示詞。主要用於 `Stop` 和 `SubagentStop` 事件，用於智慧型任務完成檢查。

```json
{
  "type": "prompt",
  "prompt": "評估 Claude 是否已完成所有要求的任務。",
  "timeout": 30
}
```

LLM 評估提示詞並回應用於結構化的決策（請參閱 [基於提示詞的鉤子](#prompt-based-hooks) 以取得詳細資訊）。

### 代理鉤子

基於子代理的驗證鉤子，會啟動專用的代理來評估條件或執行複雜的檢查。與提示詞鉤子（單回合 LLM 評估）不同，代理鉤子可以使用工具並執行多步驟推理。

```json
{
  "type": "agent",
  "prompt": "驗證程式碼變更是否符合我們的架構指南。檢查相關的設計文件並進行比較。",
  "timeout": 120
}
```

**主要屬性：**
- `"type": "agent"` -- 識別此為代理鉤子
- `"prompt"` -- 子代理的任務描述
- 代理可以使用工具（Read、Grep、Bash 等）進行評估
- 回應用於結構化的決策，類似於提示詞鉤子

## 鉤子事件

Claude Code 支援 **26 個鉤子事件**：

| 事件 | 觸發時機 | 比對輸入 | 是否可阻斷 | 常用用途 |
|-------|---------------|---------------|-----------|------------|
| **SessionStart** | 工作階段開始/恢復/清除/壓縮 | startup/resume/clear/compact | 否 | 環境設定 |
| **InstructionsLoaded** | 在 CLAUDE.md 或規則檔案載入後 | (無) | 否 | 修改/篩選指令 |
| **UserPromptSubmit** | 用戶提交提示詞 | (無) | 是 | 驗證提示詞 |
| **PreToolUse** | 工具執行前 | 工具名稱 | 是 (允許/拒絕/詢問) | 驗證、修改輸入 |
| **PermissionRequest** | 權限對話框顯示 | 工具名稱 | 是 | 自動批准/拒絕 |
| **PermissionDenied** | 用戶拒絕權限提示 | 工具名稱 | 否 | 記錄、分析、策略執行 |
| **PostToolUse** | 工具成功執行後 | 工具名稱 | 否 | 增加上下文、回饋 |
| **PostToolUseFailure** | 工具執行失敗 | 工具名稱 | 否 | 錯誤處理、記錄 |
| **Notification** | 發送通知 | 通知類型 | 否 | 自訂通知 |
| **SubagentStart** | 子代理啟動 | 代理類型名稱 | 否 | 子代理設定 |
| **SubagentStop** | 子代理完成 | 代理類型名稱 | 是 | 子代理驗證 |
| **Stop** | Claude 完成回應 | (無) | 是 | 任務完成檢查 |
| **StopFailure** | API 錯誤結束回合 | (無) | 否 | 錯誤恢復、記錄 |
| **TeammateIdle** | 代理團隊成員閒置 | (無) | 是 | 團隊成員協調 |
| **TaskCompleted** | 任務標記為完成 | (無) | 是 | 任務後動作 |
| **TaskCreated** | 透過 TaskCreate 建立任務 | (無) | 否 | 任務追蹤、記錄 |
| **ConfigChange** | 配置文件變更 | (無) | 是 (除了策略) | 響應配置更新 |
| **CwdChanged** | 工作目錄變更 | (無) | 否 | 目錄特定的設定 |
| **FileChanged** | 監控檔案變更 | (無) | 否 | 檔案監控、重建 |
| **PreCompact** | 上下文壓縮前 | manual/auto | 否 | 壓縮前動作 |
| **PostCompact** | 壓縮完成後 | (無) | 否 | 壓縮後動作 |
| **WorktreeCreate** | 工作樹正在建立 | (無) | 是 (路徑回傳) | 工作樹初始化 |
| **WorktreeRemove** | 工作樹正在移除 | (無) | 否 | 工作樹清理 |
| **Elicitation** | MCP 伺服器請求使用者輸入 | (無) | 是 | 輸入驗證 |
| **ElicitationResult** | 用戶回應請求 | (無) | 是 | 回應處理 |
| **SessionEnd** | 工作階段終止 | (無) | 否 | 清理、最終記錄 |

### PreToolUse

在 Claude 建立工具參數且處理之前執行。 用於驗證或修改工具輸入。

**配置:**
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

**常見比對器:** `Task`, `Bash`, `Glob`, `Grep`, `Read`, `Edit`, `Write`, `WebFetch`, `WebSearch`

**輸出控制:**
- `permissionDecision`: `"allow"`, `"deny"` 或 `"ask"`
- `permissionDecisionReason`: 決定的說明
- `updatedInput`: 修改後的工具輸入參數

### PostToolUse

在工具完成後立即執行。用於驗證、記錄或將上下文回饋給 Claude。

**配置:**
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

**輸出控制:**
- `"block"` 決定提示 Claude 提供回饋
- `additionalContext`: 為 Claude 增加的上下文

### UserPromptSubmit

當使用者提交提示詞時執行，在 Claude 處理提示詞之前執行。

**配置:**
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

**輸出控制:**
- `decision`: `"block"` 以防止處理
- `reason`: 如果被阻止的說明
- `additionalContext`: 為提示詞增加的上下文

### Stop and SubagentStop

當 Claude 完成回應 (Stop) 或子代理完成 (SubagentStop) 時執行。支援基於提示詞的評估，用於智慧型任務完成檢查。

**額外輸入欄位:** 兩個 `Stop` 和 `SubagentStop` 鉤子都會在其 JSON 輸入中接收一個 `last_assistant_message` 欄位，其中包含停止之前的 Claude 或子代理的最後訊息。這對於評估任務完成非常有用。

**配置:**
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "評估 Claude 是否已完成所有要求的任務。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### SubagentStart

當子代理開始執行時執行。 matcher 輸入是代理類型名稱，允許鉤子針對特定的子代理類型。

**配置:**
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

當會話開始或恢復時執行。可以持久化環境變數。

**匹配器:** `startup`, `resume`, `clear`, `compact`

**特殊功能:** 使用 `CLAUDE_ENV_FILE` 持久化環境變數 (也可用於 `CwdChanged` 和 `FileChanged` 鉤子)：

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

### SessionEnd

當會話結束時執行，以進行清理或最終記錄。不能阻止終止。

**Reason 欄位值:**
- `clear` - 使用者已清除會話
- `logout` - 使用者已登出
- `prompt_input_exit` - 使用者透過提示詞輸入退出

### 通知事件

更新了通知事件的匹配器：
- `permission_prompt` - 權限請求通知
- `idle_prompt` - 閒置狀態通知
- `auth_success` - 驗證成功
- `elicitation_dialog` - 顯示給使用者的對話

## 元件範圍的鉤子

鉤子可以附加到特定的元件（技能、代理、命令）的 frontmatter 中：

**在 SKILL.md、agent.md 或 command.md 中：**

```yaml
---
name: secure-operations
description: 執行安全性檢查的作業
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/check.sh"
          once: true  # 每一個會話僅執行一次
---
```

## 元件鉤子支援的事件：`PreToolUse`、`PostToolUse`、`Stop`

這允許在直接在需要它們的元件中定義鉤子，將相關程式碼放在一起。

### 子代理 Frontmatter 中的鉤子

當子代理的 frontmatter 中定義了 `Stop` 鉤子時，它會自動轉換為作用域為該子代理的 `SubagentStop` 鉤子。這確保了停止鉤子僅在該特定子代理完成時才觸發，而不是當主要會話停止時。

```yaml
---
name: code-review-agent
description: 自動程式碼審查子代理
hooks:
  Stop:
    - hooks:
        - type: prompt
          prompt: "驗證程式碼審查是否徹底且完整。"
  # 上方的 Stop 鉤子會自動轉換為該子代理的 SubagentStop
---
```

## PermissionRequest 事件

處理權限請求，使用自訂輸出格式：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow|deny",
      "updatedInput": {},
      "message": "自訂訊息",
      "interrupt": false
    }
  }
}
```

## 鉤子輸入與輸出

### JSON 輸入 (透過 stdin)

所有鉤子都透過 stdin 接收 JSON 輸入：

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
  "worktree": "/path/to/worktree"
}
```

**通用欄位：**

| 欄位 | 描述 |
|-------|-------------|
| `session_id` | 唯一的會話識別碼 |
| `transcript_path` | 對話記錄檔案的路徑 |
| `cwd` | 目前工作目錄 |
| `hook_event_name` | 觸發鉤子的事件名稱 |
| `agent_id` | 執行此鉤子的代理識別碼 |
| `agent_type` | 代理類型 (`"main"`, 子代理類型名稱等) |
| `worktree` | 如果代理在其中執行，則為 git worktree 的路徑 |

### 退出代碼

| 退出代碼 | 意義 | 行為 |
|-----------|---------|----------|
| **0** | 成功 | 繼續，解析 JSON stdout |
| **2** | 阻斷錯誤 | 阻斷操作，stderr 顯示為錯誤 |
| **其他** | 非阻斷錯誤 | 繼續，stderr 顯示在詳細模式下 |

### JSON 輸出 (stdout, 退出代碼 0)

```json
{
  "continue": true,
  "stopReason": "停止時的選填訊息",
  "suppressOutput": false,
  "systemMessage": "選填警告訊息",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "檔案位於允許目錄中",
    "updatedInput": {
      "file_path": "/modified/path.js"
    }
  }
}
```

## 環境變數

| 變數 | 適用範圍 | 描述 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` | 所有鉤子 | 專案根目錄的絕對路徑 |
| `CLAUDE_ENV_FILE` | SessionStart, CwdChanged, FileChanged | 持續保存環境變數的檔案路徑 |
| `CLAUDE_CODE_REMOTE` | 所有鉤子 | 如果在遠端環境中執行，則為 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | 外掛鉤子 | 外掛目錄的路徑 |
| `${CLAUDE_PLUGIN_DATA}` | 外掛鉤子 | 外掛資料目錄的路徑 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd 鉤子 | SessionEnd 鉤子的可配置逾時時間（毫秒），覆蓋預設值 |

## 基於提示詞的鉤子

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

**LLM 回應 Schema:**
```json
{
  "decision": "approve",
  "reason": "All tasks completed successfully",
  "continue": false,
  "stopReason": "Task complete"
}
```

## 範例

### 範例 1：Bash 命令驗證器 (PreToolUse)

**檔案:** `.claude/hooks/validate-bash.py`

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

**配置:**
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

### 範例 2：安全掃描器 (PostToolUse)

**檔案:** `.claude/hooks/security-scan.py`

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
```

### 範例 3：自動格式化程式碼 (PostToolUse)

**檔案：** `.claude/hooks/format-code.sh`

```bash
#!/bin/bash

# 從 stdin 讀取 JSON
INPUT=$(cat)
TOOL_NAME=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_name', ''))")
FILE_PATH=$(echo "$INPUT" | python3 -c "import sys, json; print(json.load(sys.stdin).get('tool_input', {}).get('file_path', ''))")

if [ "$TOOL_NAME" != "Write" ] && [ "$TOOL_NAME" != "Edit" ]; then
    exit 0
fi

# 根據檔案副檔名進行格式化
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

### 範例 5：智慧型停止鉤子 (基於提示詞)

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "檢查 Claude 是否已完成所有要求的任務。檢查：1) 是否已建立/修改所有檔案？ 2) 是否有未解決的錯誤？ 如果不完整，請說明缺少什麼。",
            "timeout": 30
          }
        ]
      }
    ]
  }
}
```

### 範例 6：上下文使用情況追蹤器 (鉤子配對)

使用 `UserPromptSubmit`（預先訊息）和 `Stop`（後續回覆）鉤子共同追蹤每個請求的 token 消耗量。

**檔案：** `.claude/hooks/context-tracker.py`

```python
#!/usr/bin/env python3
"""
上下文使用情況追蹤器 - 追蹤每個請求的 token 消耗量。

使用 UserPromptSubmit 作為 "預先訊息" 鉤子和 Stop 作為 "後續回覆" 鉤子
來計算每個請求的 token 使用量差異。

Token 統計方法：
1. 字元估計（預設）：~4 個字元每 token，沒有相依性
2. tiktoken（可選）：~90-95% 準確，需要：pip install tiktoken
```

```python
import json
import os
import sys
import tempfile

# Configuration
CONTEXT_LIMIT = 128000  # Claude's context window (adjust for your model)
USE_TIKTOKEN = False    # Set True if tiktoken is installed for better accuracy


def get_state_file(session_id: str) -> str:
    """Get temp file path for storing pre-message token count, isolated by session."""
    return os.path.join(tempfile.gettempdir(), f"claude-context-{session_id}.json")


def count_tokens(text: str) -> int:
    """
    Count tokens in text.

    Uses tiktoken with p50k_base encoding if available (~90-95% accuracy),
    otherwise falls back to character estimation (~80-90% accuracy).
    """
    if USE_TIKTOKEN:
        try:
            import tiktoken
            enc = tiktoken.get_encoding("p50k_base")
            return len(enc.encode(text))
        except ImportError:
            pass  # Fall back to estimation

    # Character-based estimation: ~4 characters per token for English
    return len(text) // 4


def read_transcript(transcript_path: str) -> str:
    """Read and concatenate all content from transcript file."""
    if not transcript_path or not os.path.exists(transcript_path):
        return ""

    content = []
    with open(transcript_path, "r") as f:
        for line in f:
            try:
                entry = json.loads(line.strip())
                # Extract text content from various message formats
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
    """Pre-message hook: Save current token count before request."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Save to temp file for later comparison
    state_file = get_state_file(session_id)
    with open(state_file, "w") as f:
        json.dump({"pre_tokens": current_tokens}, f)


def handle_stop(data: dict) -> None:
    """Post-response hook: Calculate and report token delta."""
    session_id = data.get("session_id", "unknown")
    transcript_path = data.get("transcript_path", "")

    transcript_content = read_transcript(transcript_path)
    current_tokens = count_tokens(transcript_content)

    # Load pre-message count
    state_file = get_state_file(session_id)
    pre_tokens = 0
    if os.path.exists(state_file):
        try:
            with open(state_file, "r") as f:
```

**狀態追蹤器鉤子**

此鉤子會追蹤每個會話的 token 使用量，並將其記錄到檔案中。

```python
                state = json.load(f)
                pre_tokens = state.get("pre_tokens", 0)
        except (json.JSONDecodeError, IOError):
            pass

    # 計算增量
    delta_tokens = current_tokens - pre_tokens
    remaining = CONTEXT_LIMIT - current_tokens
    percentage = (current_tokens / CONTEXT_LIMIT) * 100

    # 報告使用量
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

**配置:**
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

**運作方式:**
1. `UserPromptSubmit` 在您的提示詞被處理之前觸發 - 儲存目前的 token 數量
2. `Stop` 在 Claude 回應後觸發 - 計算增量並報告使用量
3. 每個會話透過暫存檔案中的 `session_id` 隔離

**Token 數量方法:**

| 方法 | 準確度 | 依賴 | 速度 |
|--------|----------|--------------|-------|
| 字元估計 | ~80-90% | 無 | <1ms |
| tiktoken (p50k_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **注意:** Anthropic 尚未發布官方的離線 tokenizer。這兩種方法都是近似值。記錄包含使用者提示、Claude 的回應和工具輸出，但不包含系統提示或內部上下文。

### 範例 7：Seed Auto-Mode 權限 (一次性設定腳本)

一個一次性設定腳本，將 `~/.claude/settings.json` 設定為 ~67 個安全的權限規則，相當於 Claude Code 的 auto-mode 基準線 — 而沒有任何鉤子，也沒有記住未來的選擇。執行一次；安全地重新執行 (略過已存在的規則)。

**檔案:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 預覽將會新增的內容
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 應用
python3 09-advanced-features/setup-auto-mode-permissions.py
```

**新增的內容:**

| 類別 | 範例 |
|----------|---------|
| 內建工具 | `Read(*)`, `Edit(*)`, `Write(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)` |
| Git 讀取 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)` |
| Git 寫入 (本機) | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |

## 套件管理工具

| 套件管理工具 | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
|---|---|
| 建置 & 測試 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 常用 Shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

## 排除項目 (此腳本永遠不會新增)：

- `rm -rf`, `sudo`, 強制推送, `git reset --hard`
- `DROP TABLE`, `kubectl delete`, `terraform destroy`
- `npm publish`, `curl | bash`, 產品部署

## 外掛鉤

外掛程式可以在其 `hooks/hooks.json` 檔案中包含鉤子：

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

**外掛鉤中的環境變數：**
- `${CLAUDE_PLUGIN_ROOT}` - 外掛程式目錄的路徑
- `${CLAUDE_PLUGIN_DATA}` - 外掛程式資料目錄的路徑

這允許外掛程式包含自訂驗證和自動化鉤子。

## MCP 工具鉤子

MCP 工具遵循 `mcp__<server>__<tool>` 模式：

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

### 聲明

**自行承擔風險使用**: 鉤子會執行任意的 shell 命令。您獨自負責：
- 您配置的命令
- 檔案存取/修改權限
- 可能發生的資料遺失或系統損壞
- 在生產環境中使用前，先在安全環境中測試鉤子

### 安全注意事項

- **工作區信任必要**: `statusLine` 和 `fileSuggestion` 鉤子的輸出命令現在需要工作區信任的確認才能生效。
- **HTTP 鉤子和環境變數**: HTTP 鉤子需要明確的 `allowedEnvVars` 清單才能在 URL 中使用環境變數的內插。這有助於防止敏感環境變數意外洩露到遠端端點。
- **管理的設定層級結構**: `disableAllHooks` 設定現在會尊重管理的設定層級結構，這表示組織層級的設定可以強制鉤子停用，而個別使用者無法覆寫。

### 最佳實務

| 執行 | 不要執行 |
|-----|-------|
| 驗證並清理所有輸入 | 盲目信任輸入資料 |
| 引用 shell 變數: `"$VAR"` | 使用未引用: `$VAR` |
| 阻止路徑穿越 (`..`) | 允許任意路徑 |
| 使用 `$CLAUDE_PROJECT_DIR` 的絕對路徑 | 硬編碼路徑 |
| 略過敏感檔案 (`.env`, `.git/`, keys) | 處理所有檔案 |
| 首次在隔離環境中測試鉤子 | 部署未測試的鉤子 |
| 對於 HTTP 鉤子，使用明確的 `allowedEnvVars` | 將所有環境變數暴露給 webhook |

## 除錯

### 啟用除錯模式

使用除錯 flag 執行 Claude，以取得詳細的鉤子記錄：

```bash
claude --debug
```

### 詳細模式

在 Claude Code 中使用 `Ctrl+O` 啟用詳細模式，並查看鉤子執行進度。

### 獨立測試鉤子

```bash
# 使用範例 JSON 輸入進行測試
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# 檢查退出代碼
echo $?
```

## 完整配置範例

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

## 鉤子執行細節

| 方面 | 行為 |
|--------|----------|
| **逾時** | 預設 60 秒，每個命令可配置 |
| **並行處理** | 所有符合條件的鉤子並行執行 |
| **去重** | 相同的鉤子命令會去重 |
| **環境** | 在 Claude Code 的環境中當前目錄中執行 |

## 故障排除

### 鉤子未執行

- 驗證 JSON 設定語法是否正確
- 檢查比對模式是否符合工具名稱
- 確保腳本存在且可執行：`chmod +x script.sh`
- 執行 `claude --debug` 以查看鉤子執行記錄
- 驗證鉤子是否從標準輸入讀取 JSON (而不是命令參數)

### 鉤子意外停止

- 使用範例 JSON 測試鉤子：`echo '{"tool_name": "Write", ...}' | ./hook.py`
- 檢查退出代碼：allow 應為 0，block 應為 2
- 檢查標準錯誤輸出 (在退出代碼 2 時顯示)

### JSON 解析錯誤

- 務必從標準輸入讀取，而不是命令參數
- 使用正確的 JSON 解析 (而不是字串操作)
- 妥善處理遺漏的欄位

## 安裝

### 第一步：建立鉤子目錄

```bash
mkdir -p ~/.claude/hooks
```

### 第二步：複製範例鉤子

```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### 第三步：在設定中進行設定

編輯 `~/.claude/settings.json` 或 `.claude/settings.json`，使用上述鉤子設定。

## 相關概念

- **[檢查點和回溯](../08-checkpoints/)** - 儲存和還原對話狀態
- **[斜線命令](../01-slash-commands/)** - 建立自訂斜線命令
- **[技能](../03-skills/)** - 可重複使用的自主功能
- **[子代理](../04-subagents/)** - 委派任務執行
- **[外掛](../07-plugins/)** - 封裝的擴充套件
- **[進階功能](../09-advanced-features/)** - 探索進階 Claude Code 功能

## 額外資源

- **[官方鉤子文件](https://code.claude.com/docs/en/hooks)** - 完整的鉤子參考
- **[CLI 參考](https://code.claude.com/docs/en/cli-reference)** - 命令列介面文件
- **[記憶指南](../02-memory/)** - 持續上下文設定

---
**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/hooks
**相容模型**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
