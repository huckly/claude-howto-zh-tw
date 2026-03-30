<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Hooks（鉤子）

Hooks 是在 Claude Code 工作階段中，針對特定事件自動執行的腳本。它們可用於自動化、驗證、權限管理及自訂工作流程。

## 概覽

Hooks 是自動化動作（shell 指令、HTTP webhook、LLM 提示或子代理評估），會在 Claude Code 中特定事件發生時自動執行。它們透過 JSON 接收輸入，並以退出碼和 JSON 輸出回傳結果。

**主要特色：**
- 事件驅動的自動化
- 以 JSON 為基礎的輸入/輸出
- 支援 command、prompt、HTTP 和 agent 四種 hook 類型
- 可針對特定工具進行模式比對

## 設定

Hooks 在設定檔中以特定結構進行配置：

- `~/.claude/settings.json` - 使用者設定（適用於所有專案）
- `.claude/settings.json` - 專案設定（可共享、可提交）
- `.claude/settings.local.json` - 本地專案設定（不提交）
- Managed policy - 組織層級設定
- Plugin `hooks/hooks.json` - 外掛範圍的 hooks
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

**主要欄位：**

| 欄位 | 說明 | 範例 |
|-------|-------------|---------|
| `matcher` | 比對工具名稱的模式（區分大小寫） | `"Write"`, `"Edit\|Write"`, `"*"` |
| `hooks` | Hook 定義陣列 | `[{ "type": "command", ... }]` |
| `type` | Hook 類型：`"command"`（bash）、`"prompt"`（LLM）、`"http"`（webhook）或 `"agent"`（子代理） | `"command"` |
| `command` | 要執行的 shell 指令 | `"$CLAUDE_PROJECT_DIR/.claude/hooks/format.sh"` |
| `timeout` | 選填的逾時秒數（預設 60） | `30` |
| `once` | 若為 `true`，每個工作階段只執行一次 | `true` |

### 比對器模式

| 模式 | 說明 | 範例 |
|---------|-------------|---------|
| 精確字串 | 比對特定工具 | `"Write"` |
| Regex 模式 | 比對多個工具 | `"Edit\|Write"` |
| 萬用字元 | 比對所有工具 | `"*"` 或 `""` |
| MCP 工具 | 伺服器與工具模式 | `"mcp__memory__.*"` |

## Hook 類型

Claude Code 支援四種 hook 類型：

### Command Hooks

預設的 hook 類型。執行 shell 指令，並透過 JSON stdin/stdout 和退出碼進行溝通。

```json
{
  "type": "command",
  "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/validate.py\"",
  "timeout": 60
}
```

### HTTP Hooks

> 於 v2.1.63 新增。

遠端 webhook 端點，接收與 command hooks 相同的 JSON 輸入。HTTP hooks 會以 POST 方式傳送 JSON 至指定 URL 並接收 JSON 回應。啟用沙箱時，HTTP hooks 會透過沙箱路由。在 URL 中使用環境變數插值時，需提供明確的 `allowedEnvVars` 清單以確保安全。

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
- `"type": "http"` -- 標識此為 HTTP hook
- `"url"` -- webhook 端點 URL
- 啟用沙箱時透過沙箱路由
- 在 URL 中進行環境變數插值時，需提供明確的 `allowedEnvVars` 清單

### Prompt Hooks

以 LLM 評估的提示型 hooks，hook 內容為 Claude 評估的提示語。主要用於 `Stop` 和 `SubagentStop` 事件，以智慧型方式檢查任務是否完成。

```json
{
  "type": "prompt",
  "prompt": "Evaluate if Claude completed all requested tasks.",
  "timeout": 30
}
```

LLM 會評估提示並回傳結構化決策（詳見 [Prompt-Based Hooks](#以提示為基礎的-hooks)）。

### Agent Hooks

以子代理為基礎的驗證 hooks，會生成專屬代理來評估條件或執行複雜檢查。與 prompt hooks（單輪 LLM 評估）不同，agent hooks 可使用工具並進行多步驟推理。

```json
{
  "type": "agent",
  "prompt": "Verify the code changes follow our architecture guidelines. Check the relevant design docs and compare.",
  "timeout": 120
}
```

**主要屬性：**
- `"type": "agent"` -- 標識此為 agent hook
- `"prompt"` -- 子代理的任務描述
- 代理可使用工具（Read、Grep、Bash 等）進行評估
- 回傳與 prompt hooks 類似的結構化決策

## Hook 事件

Claude Code 支援 **25 個 hook 事件**：

| 事件 | 觸發時機 | 比對器輸入 | 可阻擋 | 常見用途 |
|-------|---------------|---------------|-----------|------------|
| **SessionStart** | 工作階段開始/恢復/清除/壓縮 | startup/resume/clear/compact | 否 | 環境設置 |
| **InstructionsLoaded** | CLAUDE.md 或規則檔案載入後 | （無） | 否 | 修改/過濾指令 |
| **UserPromptSubmit** | 使用者提交提示 | （無） | 是 | 驗證提示 |
| **PreToolUse** | 工具執行前 | 工具名稱 | 是（allow/deny/ask） | 驗證、修改輸入 |
| **PermissionRequest** | 顯示權限對話框 | 工具名稱 | 是 | 自動核准/拒絕 |
| **PostToolUse** | 工具執行成功後 | 工具名稱 | 否 | 新增上下文、回饋 |
| **PostToolUseFailure** | 工具執行失敗後 | 工具名稱 | 否 | 錯誤處理、記錄 |
| **Notification** | 發送通知 | 通知類型 | 否 | 自訂通知 |
| **SubagentStart** | 子代理啟動 | 代理類型名稱 | 否 | 子代理初始化 |
| **SubagentStop** | 子代理完成 | 代理類型名稱 | 是 | 子代理驗證 |
| **Stop** | Claude 完成回應 | （無） | 是 | 任務完成確認 |
| **StopFailure** | API 錯誤結束輪次 | （無） | 否 | 錯誤恢復、記錄 |
| **TeammateIdle** | 代理團隊成員閒置 | （無） | 是 | 成員協調 |
| **TaskCompleted** | 任務標記為完成 | （無） | 是 | 任務後處理 |
| **TaskCreated** | 透過 TaskCreate 建立任務 | （無） | 否 | 任務追蹤、記錄 |
| **ConfigChange** | 設定檔變更 | （無） | 是（policy 除外） | 回應設定變更 |
| **CwdChanged** | 工作目錄變更 | （無） | 否 | 目錄專屬設置 |
| **FileChanged** | 監看的檔案變更 | （無） | 否 | 檔案監控、重建 |
| **PreCompact** | 上下文壓縮前 | manual/auto | 否 | 壓縮前動作 |
| **PostCompact** | 壓縮完成後 | （無） | 否 | 壓縮後動作 |
| **WorktreeCreate** | 建立 worktree | （無） | 是（路徑回傳） | Worktree 初始化 |
| **WorktreeRemove** | 移除 worktree | （無） | 否 | Worktree 清理 |
| **Elicitation** | MCP 伺服器請求使用者輸入 | （無） | 是 | 輸入驗證 |
| **ElicitationResult** | 使用者回應問詢 | （無） | 是 | 回應處理 |
| **SessionEnd** | 工作階段結束 | （無） | 否 | 清理、最終記錄 |

### PreToolUse

在 Claude 建立工具參數後、開始處理前執行。用於驗證或修改工具輸入。

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

**常用比對器：** `Task`, `Bash`, `Glob`, `Grep`, `Read`, `Edit`, `Write`, `WebFetch`, `WebSearch`

**輸出控制：**
- `permissionDecision`：`"allow"`、`"deny"` 或 `"ask"`
- `permissionDecisionReason`：決策說明
- `updatedInput`：修改後的工具輸入參數

### PostToolUse

在工具完成後立即執行。用於驗證、記錄或向 Claude 提供上下文。

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
- `"block"` 決策會以回饋提示 Claude
- `additionalContext`：為 Claude 新增的上下文

### UserPromptSubmit

在使用者提交提示後、Claude 處理前執行。

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
- `decision`：`"block"` 可阻止處理
- `reason`：阻擋時的說明
- `additionalContext`：新增至提示的上下文

### Stop 與 SubagentStop

在 Claude 完成回應（Stop）或子代理完成任務（SubagentStop）時執行。支援以提示為基礎的評估，用於智慧型任務完成確認。

**額外輸入欄位：** `Stop` 和 `SubagentStop` hooks 均會在 JSON 輸入中收到 `last_assistant_message` 欄位，包含 Claude 或子代理停止前的最後訊息。這對評估任務完成情況非常有用。

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

### SubagentStart

在子代理開始執行時運行。比對器輸入為代理類型名稱，允許 hooks 針對特定子代理類型。

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

在工作階段啟動或恢復時執行。可持久化環境變數。

**比對器：** `startup`, `resume`, `clear`, `compact`

**特殊功能：** 使用 `CLAUDE_ENV_FILE` 來持久化環境變數（在 `CwdChanged` 和 `FileChanged` hooks 中同樣可用）：

```bash
#!/bin/bash
if [ -n "$CLAUDE_ENV_FILE" ]; then
  echo 'export NODE_ENV=development' >> "$CLAUDE_ENV_FILE"
fi
exit 0
```

### SessionEnd

在工作階段結束時執行，用於清理或最終記錄。無法阻擋終止。

**Reason 欄位值：**
- `clear` - 使用者清除了工作階段
- `logout` - 使用者登出
- `prompt_input_exit` - 使用者透過提示輸入退出
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

通知事件的更新比對器：
- `permission_prompt` - 權限請求通知
- `idle_prompt` - 閒置狀態通知
- `auth_success` - 驗證成功
- `elicitation_dialog` - 顯示給使用者的對話框

## 元件範圍的 Hooks

Hooks 可透過 frontmatter 附加至特定元件（skills、agents、commands）：

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

**元件 hooks 支援的事件：** `PreToolUse`, `PostToolUse`, `Stop`

這樣可以直接在使用 hooks 的元件中定義，讓相關程式碼保持在一起。

### 子代理 Frontmatter 中的 Hooks

當子代理的 frontmatter 中定義了 `Stop` hook 時，它會自動轉換為限定在該子代理範圍內的 `SubagentStop` hook。這確保了停止 hook 只在該特定子代理完成時觸發，而非在主工作階段停止時觸發。

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

## PermissionRequest 事件

以自訂輸出格式處理權限請求：

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

所有 hooks 均透過 stdin 接收 JSON 輸入：

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

**常用欄位：**

| 欄位 | 說明 |
|-------|-------------|
| `session_id` | 唯一的工作階段識別碼 |
| `transcript_path` | 對話記錄檔案的路徑 |
| `cwd` | 目前工作目錄 |
| `hook_event_name` | 觸發此 hook 的事件名稱 |
| `agent_id` | 執行此 hook 的代理識別碼 |
| `agent_type` | 代理類型（`"main"`、子代理類型名稱等） |
| `worktree` | 代理若在 git worktree 中執行，則為其路徑 |

### 退出碼

| 退出碼 | 意義 | 行為 |
|-----------|---------|----------|
| **0** | 成功 | 繼續，解析 JSON stdout |
| **2** | 阻擋性錯誤 | 阻擋操作，stderr 顯示為錯誤 |
| **其他** | 非阻擋性錯誤 | 繼續，stderr 在詳細模式下顯示 |

### JSON 輸出（stdout，退出碼 0）

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

## 環境變數

| 變數 | 可用範圍 | 說明 |
|----------|-------------|-------------|
| `CLAUDE_PROJECT_DIR` | 所有 hooks | 專案根目錄的絕對路徑 |
| `CLAUDE_ENV_FILE` | SessionStart, CwdChanged, FileChanged | 用於持久化環境變數的檔案路徑 |
| `CLAUDE_CODE_REMOTE` | 所有 hooks | 若在遠端環境中執行，則為 `"true"` |
| `${CLAUDE_PLUGIN_ROOT}` | Plugin hooks | 外掛目錄的路徑 |
| `${CLAUDE_PLUGIN_DATA}` | Plugin hooks | 外掛資料目錄的路徑 |
| `CLAUDE_CODE_SESSIONEND_HOOKS_TIMEOUT_MS` | SessionEnd hooks | SessionEnd hooks 的可設定逾時毫秒數（覆寫預設值） |

## 以提示為基礎的 Hooks

對於 `Stop` 和 `SubagentStop` 事件，可使用基於 LLM 的評估：

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

### 範例 1：Bash 指令驗證器（PreToolUse）

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

### 範例 2：資安掃描器（PostToolUse）

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

### 範例 3：自動格式化程式碼（PostToolUse）

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

### 範例 4：提示驗證器（UserPromptSubmit）

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

### 範例 5：智慧型停止 Hook（以提示為基礎）

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

### 範例 6：上下文使用量追蹤器（Hook 配對）

使用 `UserPromptSubmit`（訊息前）和 `Stop`（回應後）hooks 配對，追蹤每次請求的 token 消耗量。

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

**運作方式：**
1. `UserPromptSubmit` 在您的提示被處理前觸發，儲存目前的 token 數量
2. `Stop` 在 Claude 回應後觸發，計算差值並回報使用量
3. 每個工作階段透過暫存檔案名稱中的 `session_id` 進行隔離

**Token 計算方法：**

| 方法 | 精確度 | 相依套件 | 速度 |
|--------|----------|--------------|-------|
| 字元估算 | ~80-90% | 無 | <1ms |
| tiktoken (p50k_base) | ~90-95% | `pip install tiktoken` | <10ms |

> **注意：** Anthropic 尚未發布官方的離線 tokenizer。兩種方法均為估算。記錄包含使用者提示、Claude 的回應和工具輸出，但**不包含**系統提示或內部上下文。

### 範例 7：自動適應模式（PostToolUse）

自動從您的工具核准記錄中學習，並更新 `~/.claude/settings.json` 的權限設定。每次您核准工具執行後，hook 會將指令泛化為可重用的權限規則，讓您再也不需要核准同類型的指令。危險/破壞性的指令**永遠不會**被記憶。

首次執行時，會以自動模式等效的基準權限進行初始設定（讀寫檔案、git 操作、套件管理工具、常用 CLI 工具）。

**檔案：** `.claude/hooks/auto-adapt-mode.py`

```python
#!/usr/bin/env python3
"""
auto-adapt-mode: Learn from user's tool approvals and update Claude config.

Hook Type: PostToolUse
Event: Fires after a tool is successfully executed (meaning user approved it)
"""

import json
import os
import sys
import re
from pathlib import Path

SETTINGS_PATH = Path.home() / ".claude" / "settings.json"
LOG_PATH = Path.home() / ".claude" / "auto-adapt-mode.log"

# Auto-mode baseline: safe, local, reversible operations
AUTO_MODE_BASELINE = [
    "Read(*)", "Edit(*)", "Write(*)", "Glob(*)", "Grep(*)",
    "Bash(git status:*)", "Bash(git log:*)", "Bash(git diff:*)",
    "Bash(git add:*)", "Bash(git commit:*)", "Bash(git checkout:*)",
    "Bash(npm install:*)", "Bash(npm test:*)", "Bash(npm run:*)",
    "Bash(pip install:*)", "Bash(pytest:*)",
    "Bash(ls:*)", "Bash(cat:*)", "Bash(find:*)", "Bash(mkdir:*)",
    "Bash(cp:*)", "Bash(mv:*)", "Bash(chmod:*)",
    "Bash(gh pr view:*)", "Bash(gh issue list:*)",
    "Agent(*)", "Skill(*)", "WebSearch(*)", "WebFetch(*)",
    # ... (full list includes 70+ safe patterns)
]

# Commands that are NEVER auto-remembered
DANGEROUS_PATTERNS = [
    r"rm\s+(-[a-zA-Z]*r[a-zA-Z]*|--recursive)",   # rm -rf
    r"git\s+push\s+(-[a-zA-Z]*f|--force)",          # force push
    r"git\s+reset\s+--hard",                         # hard reset
    r"DROP\s+(TABLE|DATABASE)",                       # SQL destructive
    r"curl\s+.*\|\s*(bash|sh)",                       # pipe to shell
    r"sudo\b",                                        # privilege escalation
    r"docker\s+(rm|rmi|system\s+prune)",              # container destructive
    r"kubectl\s+delete",                              # k8s destructive
    r"terraform\s+destroy",                           # infra destructive
    r"npm\s+publish",                                 # irreversible publish
    r"deploy\s+.*prod",                               # production deploy
    # ... (full list includes 25+ patterns)
]


def is_dangerous_command(command: str) -> bool:
    """Check if a bash command matches any dangerous pattern."""
    return any(re.search(p, command, re.IGNORECASE) for p in DANGEROUS_PATTERNS)


def generalize_tool_permission(tool_name: str, tool_input: dict) -> str | None:
    """Convert a specific tool invocation into a generalized permission rule."""
    if tool_name == "Bash":
        command = tool_input.get("command", "")
        if not command or is_dangerous_command(command):
            return None
        parts = command.strip().split()
        base = parts[0]
        # Compound commands: "git push" -> "Bash(git push:*)"
        compound = ["git", "npm", "npx", "pip", "cargo", "go", "gh", "python3"]
        if base in compound and len(parts) > 1:
            sub = parts[1]
            if sub.lower() in {"rm", "delete", "destroy", "publish"}:
                return None
            return f"Bash({base} {sub}:*)"
        return f"Bash({base}:*)"
    elif tool_name == "Bash":  # Never allow generic Bash(*)
        return None
    else:
        return f"{tool_name}(*)"


def main():
    try:
        hook_input = json.load(sys.stdin)
    except (json.JSONDecodeError, EOFError):
        sys.exit(0)

    tool_name = hook_input.get("tool_name", "")
    tool_input = hook_input.get("tool_input", {})
    if not tool_name:
        sys.exit(0)

    # Load settings, ensure baseline, add new rule if safe
    settings = json.load(open(SETTINGS_PATH)) if SETTINGS_PATH.exists() else {}
    allow = settings.setdefault("permissions", {}).setdefault("allow", [])

    # Seed baseline on first run
    marker = Path.home() / ".claude" / ".auto-adapt-mode-initialized"
    if not marker.exists():
        existing = set(allow)
        for rule in AUTO_MODE_BASELINE:
            if rule not in existing:
                allow.append(rule)
        marker.touch()

    # Generalize and add the new rule
    rule = generalize_tool_permission(tool_name, tool_input)
    if rule and rule not in allow:
        allow.append(rule)
        with open(SETTINGS_PATH, "w") as f:
            json.dump(settings, f, indent=2)
            f.write("\n")

    sys.exit(0)

if __name__ == "__main__":
    main()
```

**設定：**
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "command",
            "command": "python3 \"$CLAUDE_PROJECT_DIR/.claude/hooks/auto-adapt-mode.py\"",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

**運作方式：**
1. `PostToolUse` 在**每次**成功工具執行後觸發（表示您已核准）
2. Hook 提取工具名稱和輸入，然後泛化為權限規則
3. 複合指令如 `git push origin main` 會變成 `Bash(git push:*)` — 可比對任何 `git push` 變體
4. 若規則尚未存在，則新增至 `~/.claude/settings.json` → `permissions.allow`
5. 首次執行時，設置約 70 條自動模式等效的基準權限

**安全保證：**
- 危險指令（force push、rm -rf、sudo、DROP TABLE 等）**永遠不會**被記憶
- 不可逆操作（npm publish、terraform destroy、production 部署）**永遠阻擋**
- `deny` 清單中的指令永不被覆寫
- Hook 永不阻擋工具執行（始終以 exit 0 退出）
- `~/.claude/auto-adapt-mode.log` 記錄所有決策以供稽核

**泛化範例：**

| 您核准的指令 | 新增的規則 | 涵蓋範圍 |
|-------------|-----------|--------|
| `git push origin main` | `Bash(git push:*)` | 所有 git push 變體 |
| `npm run build` | `Bash(npm run:*)` | 所有 npm scripts |
| `ls -la src/` | `Bash(ls:*)` | 所有 ls 呼叫 |
| `rm -rf /tmp/test` | *（阻擋）* | 永不記憶 |
| `git push --force` | *（阻擋）* | 永不記憶 |
| `Write` 工具 | `Write(*)` | 所有檔案寫入 |

> **提示：** 刪除 `~/.claude/.auto-adapt-mode-initialized` 可重新設置基準權限。查看 `~/.claude/auto-adapt-mode.log` 稽核新增了哪些規則以及哪些被阻擋。

## 外掛 Hooks

外掛可在其 `hooks/hooks.json` 檔案中包含 hooks：

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

**外掛 Hooks 中的環境變數：**
- `${CLAUDE_PLUGIN_ROOT}` - 外掛目錄的路徑
- `${CLAUDE_PLUGIN_DATA}` - 外掛資料目錄的路徑

這讓外掛可以包含自訂的驗證和自動化 hooks。

## MCP 工具 Hooks

MCP 工具遵循 `mcp__<server>__<tool>` 的命名模式：

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

**使用時請自行承擔風險**：Hooks 可執行任意 shell 指令。您需獨自承擔以下責任：
- 您設定的指令
- 檔案存取/修改權限
- 潛在的資料遺失或系統損壞
- 在正式環境使用前於安全環境中測試 hooks

### 安全注意事項

- **需要工作區信任：** `statusLine` 和 `fileSuggestion` hook 輸出指令現在需要在工作區信任確認後才生效。
- **HTTP hooks 與環境變數：** HTTP hooks 需提供明確的 `allowedEnvVars` 清單，才能在 URL 中使用環境變數插值。這可防止敏感環境變數意外洩露至遠端端點。
- **Managed 設定階層：** `disableAllHooks` 設定現在遵循 managed 設定階層，表示組織層級設定可強制停用 hooks，而個別使用者無法覆寫。

### 最佳實踐

| 應做 | 不應做 |
|-----|-------|
| 驗證並清理所有輸入 | 盲目信任輸入資料 |
| 為 shell 變數加引號：`"$VAR"` | 使用未加引號的：`$VAR` |
| 阻擋路徑穿越（`..`） | 允許任意路徑 |
| 使用 `$CLAUDE_PROJECT_DIR` 的絕對路徑 | 硬式編碼路徑 |
| 跳過敏感檔案（`.env`、`.git/`、密鑰） | 處理所有檔案 |
| 先在隔離環境中測試 hooks | 部署未測試的 hooks |
| 為 HTTP hooks 使用明確的 `allowedEnvVars` | 將所有環境變數暴露給 webhook |

## 除錯

### 啟用除錯模式

以除錯旗標執行 Claude 以查看詳細的 hook 記錄：

```bash
claude --debug
```

### 詳細模式

在 Claude Code 中使用 `Ctrl+O` 啟用詳細模式，查看 hook 執行進度。

### 獨立測試 Hooks

```bash
# Test with sample JSON input
echo '{"tool_name": "Bash", "tool_input": {"command": "ls -la"}}' | python3 .claude/hooks/validate-bash.py

# Check exit code
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

## Hook 執行細節

| 面向 | 行為 |
|--------|----------|
| **逾時** | 預設 60 秒，可依指令設定 |
| **平行化** | 所有符合的 hooks 並行執行 |
| **去重** | 相同的 hook 指令會去重 |
| **環境** | 在目前目錄以 Claude Code 的環境執行 |

## 疑難排解

### Hook 未執行
- 確認 JSON 設定語法正確
- 確認比對器模式符合工具名稱
- 確保腳本存在且可執行：`chmod +x script.sh`
- 執行 `claude --debug` 查看 hook 執行記錄
- 確認 hook 從 stdin 讀取 JSON（非命令列參數）

### Hook 意外阻擋
- 以範例 JSON 測試 hook：`echo '{"tool_name": "Write", ...}' | ./hook.py`
- 檢查退出碼：允許應為 0，阻擋應為 2
- 查看 stderr 輸出（在退出碼 2 時顯示）

### JSON 解析錯誤
- 始終從 stdin 讀取，而非命令列參數
- 使用正確的 JSON 解析（非字串操作）
- 優雅地處理缺少的欄位

## 安裝

### 步驟一：建立 Hooks 目錄
```bash
mkdir -p ~/.claude/hooks
```

### 步驟二：複製範例 Hooks
```bash
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

### 步驟三：在設定中配置
編輯 `~/.claude/settings.json` 或 `.claude/settings.json`，加入上述 hook 設定。

## 相關概念

- **[Checkpoints 與 Rewind](../08-checkpoints/)** - 儲存與恢復對話狀態
- **[Slash Commands](../01-slash-commands/)** - 建立自訂斜線指令
- **[Skills](../03-skills/)** - 可重用的自主能力
- **[Subagents](../04-subagents/)** - 委派任務執行
- **[Plugins](../07-plugins/)** - 捆綁式擴展套件
- **[Advanced Features](../09-advanced-features/)** - 探索 Claude Code 進階功能

## 額外資源

- **[官方 Hooks 文件](https://code.claude.com/docs/en/hooks)** - 完整 hooks 參考
- **[CLI 參考](https://code.claude.com/docs/en/cli-reference)** - 命令列介面文件
- **[Memory 指南](../02-memory/)** - 持久化上下文設定

