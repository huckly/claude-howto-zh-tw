<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目錄

> 快速參考指南，涵蓋所有 Claude Code 功能：斜線命令、代理、技能、外掛和鉤子。

**導航**: [斜線命令](#slash-commands) | [權限模式](#permission-modes) | [子代理](#subagents) | [技能](#skills) | [外掛](#plugins) | [MCP 伺服器](#mcp-servers) | [鉤子](#hooks) | [記憶](#memory-files) | [新功能](#new-features-april-2026)

---

## 摘要

| 功能 | 內建 | 範例 | 總數 | 參考 |
|---------|----------|----------|-------|-----------|
| **斜線命令** | 55+ | 8 | 63+ | [01-slash-commands/](01-slash-commands/) |
| **子代理** | 6 | 11 | 17 | [04-subagents/](04-subagents/) |
| **技能** | 5 內建 | 4 | 9 | [03-skills/](03-skills/) |
| **外掛** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 伺服器** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **鉤子** | 25 事件 | 8 | 8 | [06-hooks/](06-hooks/) |
| **記憶** | 7 類型 | 3 | 3 | [02-memory/](02-memory/) |
| **總數** | **99** | **45** | **119** | |

---

## 斜線命令

命令是用戶啟動的快捷方式，用於執行特定動作。

### 內建命令

| 命令 | 描述 | 何時使用 |
|---------|-------------|-------------|
| `/help` | 顯示幫助資訊 | 開始使用，學習命令 |
| `/btw` | 不加入上下文的側問 | 快速的延伸問題 |
| `/chrome` | 設置 Chrome 整合 | 瀏覽器自動化 |
| `/clear` | 清除對話歷史記錄 | 重新開始，減少上下文 |
| `/diff` | 互動式差異查看器 | 審查變更 |
| `/config` | 檢視/編輯配置 | 自訂行為 |
| `/status` | 顯示會話狀態 | 檢查當前狀態 |
| `/agents` | 列出可用的代理 | 檢視委派選項 |
| `/skills` | 列出可用的技能 | 檢視自動觸發功能 |
| `/hooks` | 列出配置的鉤子 | 除錯自動化 |
| `/insights` | 分析會話模式 | 會話優化 |
| `/install-slack-app` | 安裝 Claude Slack app | Slack 整合 |
| `/keybindings` | 自訂鍵盤快捷鍵 | 鍵盤自訂 |
| `/mcp` | 列出 MCP 伺服器 | 檢查外部整合 |
| `/memory` | 檢視已載入的記憶檔案 | 除錯上下文載入 |
| `/mobile` | 產生行動 QR 碼 | 行動存取 |
| `/passes` | 檢視使用次數 | 訂閱資訊 |
| `/plugin` | 管理外掛 | 安裝/移除擴充功能 |
| `/plan` | 進入規劃模式 | 複雜的實作 |
| `/rewind` | 回溯到檢查點 | 撤銷變更，探索替代方案 |
| `/checkpoint` | 管理檢查點 | 儲存/還原狀態 |
| `/cost` | 顯示 token 使用成本 | 監控支出 |
| `/context` | 顯示上下文視窗使用情況 | 管理對話長度 |
| `/export` | 匯出對話 | 儲存以供參考 |
| `/extra-usage` | 配置額外使用限制 | 速率限制管理 |
| `/feedback` | 提交回饋或 bug 報告 | 報告問題 |

| `/login` | 驗證登入 Anthropic | 存取功能 |
| `/logout` | 登出 | 轉換帳戶 |
| `/sandbox` | 啟用沙箱模式 | 安全命令執行 |
| `/doctor` | 執行診斷 | 排除問題 |
| `/reload-plugins` | 重新載入已安裝的外掛 | 外掛管理 |
| `/release-notes` | 顯示發布說明 | 檢查新功能 |
| `/remote-control` | 啟用遠端控制 | 遠端存取 |
| `/permissions` | 管理權限 | 控制存取 |
| `/session` | 管理會話 | 多會話工作流程 |
| `/rename` | 重新命名目前會話 | 整理會話 |
| `/resume` | 恢復先前會話 | 繼續工作 |
| `/todo` | 檢視/管理待辦事項清單 | 追蹤任務 |
| `/tasks` | 檢視背景任務 | 監控非同步操作 |
| `/copy` | 將上次回應複製到剪貼簿 | 快速分享輸出 |
| `/teleport` | 將會話傳送到另一台機器 | 繼續遠端工作 |
| `/desktop` | 開啟 Claude Desktop 應用程式 | 轉換到桌面介面 |
| `/theme` | 變更顏色主題 | 自訂外觀 |
| `/usage` | 顯示 API 使用統計資料 | 監控配額和成本 |
| `/fork` | 分支目前對話 | 探索替代方案 |
| `/stats` | 顯示會話統計資料 | 檢視會話指標 |
| `/statusline` | 設定狀態列 | 自訂狀態顯示 |
| `/stickers` | 檢視會話貼紙 | 趣味獎勵 |
| `/fast` | 啟用快速輸出模式 | 加快回應速度 |
| `/terminal-setup` | 設定終端機整合 | 設定終端機功能 |
| `/upgrade` | 檢查更新 | 版本管理 |
| `/team-onboarding` | 從此專案的 Claude Code 使用情況產生團隊成員上手指南 | 團隊成員上手 (v2.1.101) |
| `/ultraplan` | 將規劃任務交付給計畫模式中的 Claude Code 網路會話 | 密集規劃卸載 (研究預覽版，v2.1.91+) |

### 自訂命令 (範例)

| 命令 | 描述 | 何時使用 | 範圍 | 安裝 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 分析程式碼以進行優化 | 提升效能 | 專案 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 準備拉取請求 | 在提交 PR 前 | 專案 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 產生 API 文件 | 文件化 API | 專案 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 根據上下文建立 git 提交 | 提交變更 | 使用者 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 暫存、提交和推送 | 快速部署 | 使用者 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重構文件 | 改善文件 | 專案 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 設定 CI/CD 流水線 | 新專案 | 專案 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 擴展測試覆蓋率 | 改善測試 | 專案 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

## 範圍：`User` = 個人工作流程 (`~/.claude/commands/`)，`Project` = 團隊共享 (`.claude/commands/`)

**參考**: [01-slash-commands/](01-slash-commands/) | [官方文件](https://code.claude.com/docs/en/interactive-mode)

**快速安裝 (所有自訂命令)**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 權限模式

Claude Code 支援 6 種權限模式，用於控制工具使用的授權方式。

| 模式 | 描述 | 何時使用 |
|------|-------------|-------------|
| `default` | 每次工具呼叫都提示 | 標準互動使用 |
| `acceptEdits` | 自動接受檔案編輯，提示其他操作 | 信任的編輯工作流程 |
| `plan` | 僅限唯讀工具，不進行寫入 | 規劃和探索 |
| `auto` | 不提示就接受所有工具 | 完全自主操作 (研究預覽版) |
| `bypassPermissions` | 略過所有權限檢查 | CI/CD、無頭環境 |
| `dontAsk` | 略過需要權限的工具 | 非互動式腳本 |

> **注意**: `auto` 模式是研究預覽版功能 (2026 年 3 月)。僅在受信任的、沙箱化的環境中使用 `bypassPermissions`。

**參考**: [官方文件](https://code.claude.com/docs/en/permissions)

---

## 子代理

專門的 AI 助理，具有隔離的上下文，用於執行特定任務。

### 內建子代理

| 代理 | 描述 | 工具 | 模型 | 何時使用 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 多步驟任務，研究 | 所有工具 | 繼承模型 | 複雜研究，多檔案任務 |
| **Plan** | 實施規劃 | Read, Glob, Grep, Bash | 繼承模型 | 架構設計，規劃 |
| **Explore** | 程式碼庫探索 | Read, Glob, Grep | Haiku 4.5 | 快速搜尋，理解程式碼 |
| **Bash** | 命令執行 | Bash | 繼承模型 | Git 操作，終端任務 |
| **statusline-setup** | 狀態列設定 | Bash, Read, Write | Sonnet 4.6 | 設定狀態列顯示 |
| **Claude Code Guide** | 說明和文件 | Read, Glob, Grep | Haiku 4.5 | 尋求協助，學習功能 |

### 子代理設定欄位

| 欄位 | 類型 | 描述 |
|-------|------|-------------|
| `name` | string | 代理識別碼 |
| `description` | string | 代理的功能 |
| `model` | string | 模型覆寫 (例如，`haiku-4.5`) |
| `tools` | array | 允許的工具清單 |
| `effort` | string | 思考努力程度 (`low`, `medium`, `high`) |
| `initialPrompt` | string | 在代理啟動時注入的系統提示詞 |
| `disallowedTools` | array | 該代理明確拒絕使用的工具 |

### 自訂子代理 (範例)

| 代理 | 描述 | 何時使用 | 範圍 | 安裝 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 完整的程式碼品質 | 程式碼審查會議 | Project | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架構設計 | 新功能規劃 | Project | `cp 04-subagents/code-architect.md .claude/agents/` |

| `code-explorer` | 深度程式碼分析 | 了解現有功能 | 專案 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | 乾淨程式碼原則審查 | 可維護性審查 | 專案 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 測試策略與涵蓋率 | 測試規劃 | 專案 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術文件撰寫 | API 文件、指南 | 專案 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 專注於安全性的審查 | 安全審核 | 專案 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能實作 | 功能開發 | 專案 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根本原因分析 | Bug 調查 | 用戶 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查詢、資料分析 | 資料任務 | 用戶 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | Profiling 與效能調校 | 瓶頸調查 | 專案 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **Scope**: `User` = 個人 (`~/.claude/agents/`), `Project` = 團隊共享 (`.claude/agents/`)

**Reference**: [04-subagents/](04-subagents/) | [Official Docs](https://code.claude.com/docs/en/sub-agents)

**Quick Install (All Custom Agents)**:
```bash
cp 04-subagents/*.md .claude/agents/
```

## 技能

自動觸發的內建功能，包含指令、腳本和範本。

### 範例技能

| 技能 | 描述 | 自動觸發時機 | 範圍 | 安裝 |
|-------|-------------|-------------------|-------|--------------|
| `code-review` | 完整的程式碼審查 | "Review this code", "Check quality" | 專案 | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | 品牌一致性檢查器 | 撰寫行銷文案 | 專案 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文件產生器 | "Generate docs", "Document API" | 專案 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系統化的程式碼重構 (Martin Fowler) | "Refactor this", "Clean up code" | 使用者 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **範圍**: `User` = 個人 (`~/.claude/skills/`), `Project` = 團隊共享 (`.claude/skills/`)

### 技能結構

```
~/.claude/skills/skill-name/
├── SKILL.md          # 技能定義與說明
├── scripts/          # 輔助腳本
└── templates/        # 輸出範本
```

### 技能 Frontmatter 欄位

技能支援在 `SKILL.md` 中使用 YAML frontmatter 進行設定：

| 欄位 | 類型 | 描述 |
|-------|------|-------------|
| `name` | string | 技能顯示名稱 |
| `description` | string | 技能的功能 |
| `autoInvoke` | array | 自動觸發的關鍵詞 |
| `effort` | string | 思考努力程度 (`low`, `medium`, `high`) |
| `shell` | string | 用於腳本的 Shell (`bash`, `zsh`, `sh`) |

**參考**: [03-skills/](03-skills/) | [官方文件](https://code.claude.com/docs/en/skills)

**快速安裝 (所有技能)**:
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 內建技能

| 技能 | 描述 | 自動觸發時機 |
|-------|-------------|-------------------|
| `/simplify` | 檢查程式碼品質 | 撰寫程式碼後 |
| `/batch` | 在多個檔案上執行提示詞 | 批次操作 |
| `/debug` | 除錯失敗的測試/錯誤 | 除錯階段 |
| `/loop` | 在間隔內執行提示詞 | 週期性任務 |
| `/claude-api` | 使用 Claude API 建立應用程式 | API 開發 |

---

## 外掛

內建的命令、代理、MCP 伺服器和鉤子的集合。

### 範例外掛

| 外掛 | 描述 | 組件 | 何時使用 | 範圍 | 安裝 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR 審查工作流程 | 3 個命令、3 個代理、GitHub MCP | 程式碼審查 | 專案 | `/plugin install pr-review` |
| `devops-automation` | 部署與監控 | 4 個命令、3 個代理、K8s MCP | DevOps 任務 | 專案 | `/plugin install devops-automation` |
| `documentation` | 文件生成套件 | 4 個命令、3 個代理、範本 | 文件 | 專案 | `/plugin install documentation` |

> **範圍**: `Project` = 團隊共享, `User` = 個人工作流程

### 外掛結構

```
.claude-plugin/
├── plugin.json       # 清單檔案
├── commands/         # 斜線命令
├── agents/           # 子代理
├── skills/           # 技能
├── mcp/              # MCP 設定
├── hooks/            # 鉤子腳本
└── scripts/          # 公用程式腳本
```

**參考**: [07-plugins/](07-plugins/) | [官方文件](https://code.claude.com/docs/en/plugins)

**外掛管理命令**:
```bash
/plugin list              # 列出已安裝的外掛
/plugin install <name>    # 安裝外掛
/plugin remove <name>     # 移除外掛
/plugin update <name>     # 更新外掛
```

---

## MCP 伺服器

模型上下文協定伺服器，用於存取外部工具和 API。

### 常用 MCP 伺服器

| 伺服器 | 描述 | 何時使用 | 範圍 | 安裝 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 管理、議題、程式碼 | GitHub 工作流程 | 專案 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查詢、資料存取 | 資料庫操作 | 專案 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 進階檔案操作 | 複雜的檔案任務 | 使用者 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 團隊溝通 | 通知、更新 | 專案 | 在設定中設定 |
| **Google Docs** | 文件存取 | 文件編輯、審查 | 專案 | 在設定中設定 |
| **Asana** | 專案管理 | 任務追蹤 | 專案 | 在設定中設定 |
| **Stripe** | 支付資料 | 財務分析 | 專案 | 在設定中設定 |
| **Memory** | 持續記憶 | 跨會話回憶 | 使用者 | 在設定中設定 |
| **Context7** | 程式庫文件 | 最新文件查詢 | 內建 | 內建 |

> **範圍**: `Project` = 團隊 (`.mcp.json`), `User` = 個人 (`~/.claude.json`), `Built-in` = 預先安裝

### MCP 設定範例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**參考**: [05-mcp/](05-mcp/) | [MCP 協定文件](https://modelcontextprotocol.io)

**快速安裝 (GitHub MCP)**:
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 鉤子

事件驅動的自動化，會在 Claude Code 事件發生時執行 shell 命令。

### 鉤子事件

| 事件 | 描述 | 何時觸發 | 使用案例 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 工作階段開始/恢復 | 工作階段初始化 | 設定任務 |
| `InstructionsLoaded` | 指示載入 | CLAUDE.md 或規則檔案載入 | 自訂指示處理 |
| `UserPromptSubmit` | 在提示處理之前 | 用戶發送訊息 | 輸入驗證 |
| `PreToolUse` | 在工具執行之前 | 任何工具執行之前 | 驗證、記錄 |
| `PermissionRequest` | 權限對話框顯示 | 在敏感動作之前 | 自訂批准流程 |
| `PostToolUse` | 工具成功後 | 任何工具完成後 | 格式設定、通知 |
| `PostToolUseFailure` | 工具執行失敗 | 工具發生錯誤後 | 錯誤處理、記錄 |
| `Notification` | 發送通知 | Claude 發送通知 | 外部警報 |
| `SubagentStart` | 子代理產生 | 子代理任務開始 | 初始化子代理上下文 |
| `SubagentStop` | 子代理完成 | 子代理任務完成 | 連鎖動作 |
| `Stop` | Claude 完成回應 | 回應完成 | 清理、報告 |
| `StopFailure` | API 錯誤結束回合 | API 錯誤發生 | 錯誤恢復、記錄 |
| `TeammateIdle` | 團隊成員代理空閒 | 代理團隊協調 | 分配工作 |
| `TaskCompleted` | 任務標記完成 | 任務完成 | 任務後處理 |
| `TaskCreated` | 透過 TaskCreate 建立任務 | 建立新的任務 | 任務追蹤、記錄 |
| `ConfigChange` | 配置更新 | 設定修改 | 響應配置變更 |
| `CwdChanged` | 工作目錄變更 | 目錄變更 | 目錄特定的設定 |
| `FileChanged` | 監控檔案變更 | 檔案修改 | 檔案監控、重建 |
| `PreCompact` | 在壓縮操作之前 | 上下文壓縮 | 狀態保存 |
| `PostCompact` | 壓縮完成後 | 壓縮完成 | 壓縮後動作 |
| `WorktreeCreate` | 工作樹正在建立 | Git 工作樹建立 | 設定工作樹環境 |
| `WorktreeRemove` | 工作樹正在移除 | Git 工作樹移除 | 清理工作樹資源 |
| `Elicitation` | MCP 伺服器請求輸入 | MCP 提取 | 輸入驗證 |
| `ElicitationResult` | 用戶回應提取 | 用戶回應 | 回應處理 |
| `SessionEnd` | 工作階段終止 | 工作階段終止 | 清理、保存狀態 |

### 範例鉤子

| 鉤子 | 描述 | 事件 | 範圍 | 安裝 |
|------|-------------|-------|-------|--------------|
| `validate-bash.py` | 命令驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 安全性掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自動格式化 | PostToolUse:Write | 用戶 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |

| `validate-prompt.py` | Prompt validation | UserPromptSubmit | Project | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | Token usage tracking | Stop | User | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | Pre-commit validation | PreToolUse:Bash | Project | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | Command logging | PostToolUse:Bash | User | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | Vulnerability scan on manifest changes | PostToolUse:Write | Project | `cp 06-hooks/dependency-check.sh .claude/hooks/` |

> **Scope**: `Project` = team (`.claude/settings.json`), `User` = personal (`~/.claude/settings.json`)

### Hook Configuration

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/validate-bash.py"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**Reference**: [06-hooks/](06-hooks/) | [Official Docs](https://code.claude.com/docs/en/hooks)

**Quick Install (All Hooks)**:
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## Memory Files

Persistent context loaded automatically across sessions.

### Memory Types

| Type | Location | Scope | When to Use |
|------|----------|-------|-------------|
| **Managed Policy** | Org-managed policies | Organization | Enforce org-wide standards |
| **Project** | `./CLAUDE.md` | Project (team) | Team standards, project context |
| **Project Rules** | `.claude/rules/` | Project (team) | Modular project rules |
| **User** | `~/.claude/CLAUDE.md` | User (personal) | Personal preferences |
| **User Rules** | `~/.claude/rules/` | User (personal) | Modular personal rules |
| **Local** | `./CLAUDE.local.md` | Local (git-ignored) | Machine-specific overrides (not in official docs as of March 2026; may be legacy) |
| **Auto Memory** | Automatic | Session | Auto-captured insights and corrections |

> **Scope**: `Organization` = managed by admins, `Project` = shared with team via git, `User` = personal preferences, `Local` = not committed, `Session` = auto-managed

**Reference**: [02-memory/](02-memory/) | [Official Docs](https://code.claude.com/docs/en/memory)

**Quick Install**:
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

## 新功能 (2026 年 4 月)

| 功能 | 描述 | 如何使用 |
|---------|-------------|------------|
| **監控工具** | 觀察背景命令的標準輸出串流，並根據事件做出反應，而不是輪詢 (v2.1.98+) | 透過 [進階功能](09-advanced-features/) 使用監控工具 |
| **/team-onboarding** | 從專案的 Claude Code 設定自動生成團隊成員上手指南 (v2.1.101) | 在您的專案中執行 `/team-onboarding` |
| **Ultraplan 自動建立** | 首次執行 `/ultraplan` 時，雲端環境會自動建立 — 無需手動設定 (v2.1.101) | 使用 `/ultraplan <prompt>` |
| **遠端控制** | 透過 API 遠端控制 Claude Code 會話 | 使用遠端控制 API 以程式方式發送提示詞和接收回應 |
| **網頁會話** | 在瀏覽器環境中執行 Claude Code | 透過 `claude web` 或 Anthropic Console 存取 |
| **桌面應用程式** | Claude Code 的原生桌面應用程式 | 使用 `/desktop` 或從 Anthropic 網站下載 |
| **代理團隊** | 協調多個代理處理相關任務 | 設定可以協作和共享上下文的同伴代理 |
| **任務清單** | 背景任務管理和監控 | 使用 `/tasks` 檢視和管理背景操作 |
| **提示詞建議** | 考慮上下文的命令建議 | 根據目前上下文自動顯示建議 |
| **Git 工作樹** | 隔離的 Git 工作樹用於並行開發 | 使用工作樹命令進行安全並行分支作業 |
| **沙箱** | 隔離的執行環境以確保安全 | 使用 `/sandbox` 切換；在限制的環境中執行命令 |
| **MCP OAuth** | OAuth 驗證用於 MCP 伺服器 | 在 MCP 伺服器設定中配置 OAuth 憑證以獲得安全存取 |
| **MCP 工具搜尋** | 動態搜尋和發現 MCP 工具 | 使用工具搜尋來尋找連接伺服器上可用的 MCP 工具 |
| **排程任務** | 使用 `/loop` 和 cron 工具設定重複任務 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 整合** | 無頭 Chromium 的瀏覽器自動化 | 使用 `--chrome` 標誌或 `/chrome` 命令 |
| **鍵盤自訂** | 自訂按鍵綁定，包括和弦支援 | 使用 `/keybindings` 或編輯 `~/.claude/keybindings.json` |
| **自動模式** | 完全自主運作，無需許可提示 (研究預覽版) | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **頻道** | 多頻道通訊 (Telegram、Slack 等) (研究預覽版) | 配置頻道外掛；2026 年 3 月 |
| **語音錄入** | 提示詞的語音輸入 | 使用麥克風圖示或語音按鍵綁定 |
| **代理鉤子類型** | 產生子代理而非執行 shell 命令的鉤子 | 在鉤子配置中設定 `"type": "agent"` |
| **提示詞鉤子類型** | 將提示詞文字注入對話的鉤子 | 在鉤子配置中設定 `"type": "prompt"` |
| **MCP 提問** | MCP 伺服器可以在工具執行期間請求使用者輸入 | 透過 `Elicitation` 和 `ElicitationResult` 鉤子事件處理 |

| **Plugin LSP Support** | Language Server Protocol 整合 via 外掛 | 在 `plugin.json` 中設定 LSP 伺服器以獲得編輯器功能 |
| **Managed Drop-ins** | 組織管理的即插配置 (v2.1.83) | 由管理員透過管理策略設定；自動套用到所有使用者 |

---

## 快速參考矩陣

### 功能選擇指南

| 需求 | 推薦功能 | 原因 |
|------|---------------------|-----|
| 快速捷徑 | 斜線命令 | 手動、立即 |
| 持續的上下文 | 記憶 | 自動載入 |
| 複雜的自動化 | 技能 | 自動觸發 |
| 專業任務 | 子代理 | 隔離的上下文 |
| 外部資料 | MCP 伺服器 | 即時存取 |
| 事件自動化 | 鉤子 | 事件觸發 |
| 完整的解決方案 | 外掛 | 一體化套件 |

### 安裝優先順序

| 優先順序 | 功能 | 指令 |
|----------|---------|---------|
| 1. 必備 | 記憶 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 每日使用 | 斜線命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品質 | 子代理 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | 鉤子 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 進階 | 技能 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整 | 外掛 | `/plugin install pr-review` |

---

## 完整的一鍵式安裝

安裝此儲存庫中的所有範例：

```bash
# 建立目錄
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 安裝所有功能
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

## 額外資源

- [官方 Claude Code 文件](https://code.claude.com/docs/en/overview)
- [MCP 協定規格](https://modelcontextprotocol.io)
- [學習路徑](LEARNING-ROADMAP.md)
- [主要 README](README.md)

---

**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
