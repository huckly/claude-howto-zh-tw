<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目錄

> 涵蓋所有 Claude Code 功能的快速參考指南：指令、代理、skills、plugins 和 hooks。

**導覽**：[指令](#slash-commands) | [權限模式](#permission-modes) | [Subagents](#subagents) | [Skills](#skills) | [Plugins](#plugins) | [MCP 伺服器](#mcp-servers) | [Hooks](#hooks) | [記憶體](#memory-files) | [新功能](#new-features-march-2026)

---

## 摘要

| 功能 | 內建 | 範例 | 合計 | 參考資料 |
|---------|----------|----------|-------|-----------|
| **Slash Commands** | 55+ | 8 | 63+ | [01-slash-commands/](01-slash-commands/) |
| **Subagents** | 6 | 10 | 16 | [04-subagents/](04-subagents/) |
| **Skills** | 5 個隨附 | 4 | 9 | [03-skills/](03-skills/) |
| **Plugins** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 伺服器** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **Hooks** | 25 個事件 | 7 | 7 | [06-hooks/](06-hooks/) |
| **記憶體** | 7 種類型 | 3 | 3 | [02-memory/](02-memory/) |
| **合計** | **99** | **43** | **117** | |

---

## Slash Commands

指令是由使用者觸發、執行特定動作的快捷方式。

### 內建指令

| 指令 | 說明 | 使用時機 |
|---------|-------------|-------------|
| `/help` | 顯示說明資訊 | 入門、學習指令 |
| `/btw` | 側邊提問，不加入上下文 | 簡短的題外話 |
| `/chrome` | 設定 Chrome 整合 | 瀏覽器自動化 |
| `/clear` | 清除對話記錄 | 重新開始、縮減上下文 |
| `/diff` | 互動式差異檢視器 | 審閱變更 |
| `/config` | 檢視／編輯設定 | 自訂行為 |
| `/status` | 顯示工作階段狀態 | 檢視目前狀態 |
| `/agents` | 列出可用代理 | 查看委派選項 |
| `/skills` | 列出可用 skills | 查看自動呼叫能力 |
| `/hooks` | 列出已設定的 hooks | 除錯自動化 |
| `/insights` | 分析工作階段模式 | 工作階段最佳化 |
| `/install-slack-app` | 安裝 Claude Slack 應用程式 | Slack 整合 |
| `/keybindings` | 自訂鍵盤快速鍵 | 按鍵自訂 |
| `/mcp` | 列出 MCP 伺服器 | 檢查外部整合 |
| `/memory` | 檢視已載入的記憶體檔案 | 除錯上下文載入 |
| `/mobile` | 產生行動裝置 QR 碼 | 行動裝置存取 |
| `/passes` | 查看使用點數 | 訂閱資訊 |
| `/plugin` | 管理 plugins | 安裝／移除擴充功能 |
| `/plan` | 進入規劃模式 | 複雜實作 |
| `/rewind` | 回退至檢查點 | 撤銷變更、探索替代方案 |
| `/checkpoint` | 管理檢查點 | 儲存／恢復狀態 |
| `/cost` | 顯示 token 使用成本 | 監控費用 |
| `/context` | 顯示上下文視窗使用狀況 | 管理對話長度 |
| `/export` | 匯出對話 | 儲存以供參考 |
| `/extra-usage` | 設定額外使用限制 | 速率限制管理 |
| `/feedback` | 提交回饋或錯誤回報 | 回報問題 |
| `/login` | 向 Anthropic 進行驗證 | 存取功能 |
| `/logout` | 登出 | 切換帳號 |
| `/sandbox` | 切換沙箱模式 | 安全指令執行 |
| `/vim` | 切換 vim 模式 | Vim 風格編輯 |
| `/doctor` | 執行診斷 | 排除問題 |
| `/reload-plugins` | 重新載入已安裝的 plugins | Plugin 管理 |
| `/release-notes` | 顯示版本說明 | 查看新功能 |
| `/remote-control` | 啟用遠端控制 | 遠端存取 |
| `/permissions` | 管理權限 | 控制存取 |
| `/session` | 管理工作階段 | 多工作階段工作流程 |
| `/rename` | 重新命名目前工作階段 | 整理工作階段 |
| `/resume` | 恢復先前的工作階段 | 繼續工作 |
| `/todo` | 檢視／管理待辦清單 | 追蹤任務 |
| `/tasks` | 檢視背景任務 | 監控非同步操作 |
| `/copy` | 複製最後回應至剪貼簿 | 快速分享輸出 |
| `/teleport` | 將工作階段轉移至另一台機器 | 遠端繼續工作 |
| `/desktop` | 開啟 Claude 桌面應用程式 | 切換至桌面介面 |
| `/theme` | 變更色彩主題 | 自訂外觀 |
| `/usage` | 顯示 API 使用統計 | 監控配額與費用 |
| `/fork` | 分叉目前對話 | 探索替代方案 |
| `/stats` | 顯示工作階段統計 | 審閱工作階段指標 |
| `/statusline` | 設定狀態列 | 自訂狀態顯示 |
| `/stickers` | 查看工作階段貼圖 | 趣味獎勵 |
| `/fast` | 切換快速輸出模式 | 加速回應 |
| `/terminal-setup` | 設定終端機整合 | 設置終端機功能 |
| `/upgrade` | 檢查更新 | 版本管理 |

### 自訂指令（範例）

| 指令 | 說明 | 使用時機 | 範圍 | 安裝方式 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 分析程式碼以進行最佳化 | 效能改善 | 專案 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 準備 pull request | 提交 PR 前 | 專案 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 產生 API 文件 | 記錄 API | 專案 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 建立帶有上下文的 git commit | 提交變更 | 使用者 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 暫存、提交並推送 | 快速部署 | 使用者 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重組文件 | 改善文件 | 專案 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 設定 CI/CD 流水線 | 新專案 | 專案 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 擴充測試涵蓋率 | 改善測試 | 專案 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **範圍**：`使用者` = 個人工作流程（`~/.claude/commands/`），`專案` = 團隊共用（`.claude/commands/`）

**參考資料**：[01-slash-commands/](01-slash-commands/) | [官方文件](https://code.claude.com/docs/en/interactive-mode)

**快速安裝（所有自訂指令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## Permission Modes

Claude Code 支援 6 種權限模式，控制工具使用的授權方式。

| 模式 | 說明 | 使用時機 |
|------|-------------|-------------|
| `default` | 每次工具呼叫均提示確認 | 標準互動使用 |
| `acceptEdits` | 自動接受檔案編輯，其他操作仍提示 | 受信任的編輯工作流程 |
| `plan` | 僅限唯讀工具，不執行寫入 | 規劃與探索 |
| `auto` | 不提示即接受所有工具（Research Preview） | 完全自主操作 |
| `bypassPermissions` | 跳過所有權限檢查 | CI/CD、無頭環境 |
| `dontAsk` | 跳過需要權限的工具 | 非互動式指令稿 |

> **注意**：`auto` 模式為 Research Preview 功能（2026 年 3 月）。`bypassPermissions` 僅應在受信任的沙箱環境中使用。

**參考資料**：[官方文件](https://code.claude.com/docs/en/permissions)

---

## Subagents

具備獨立上下文、針對特定任務的專業 AI 助理。

### 內建 Subagents

| 代理 | 說明 | 工具 | 模型 | 使用時機 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 多步驟任務、研究 | 所有工具 | 繼承模型 | 複雜研究、多檔案任務 |
| **Plan** | 實作規劃 | Read, Glob, Grep, Bash | 繼承模型 | 架構設計、規劃 |
| **Explore** | 程式碼庫探索 | Read, Glob, Grep | Haiku 4.5 | 快速搜尋、理解程式碼 |
| **Bash** | 指令執行 | Bash | 繼承模型 | Git 操作、終端機任務 |
| **statusline-setup** | 狀態列設定 | Bash, Read, Write | Sonnet 4.6 | 設定狀態列顯示 |
| **Claude Code Guide** | 說明與文件 | Read, Glob, Grep | Haiku 4.5 | 取得幫助、學習功能 |

### Subagent 設定欄位

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | 代理識別碼 |
| `description` | string | 代理的功能說明 |
| `model` | string | 模型覆寫（例如 `haiku-4.5`） |
| `tools` | array | 允許的工具清單 |
| `effort` | string | 推理努力程度（`low`、`medium`、`high`） |
| `initialPrompt` | string | 代理啟動時注入的系統提示 |
| `disallowedTools` | array | 明確禁止此代理使用的工具 |

### 自訂 Subagents（範例）

| 代理 | 說明 | 使用時機 | 範圍 | 安裝方式 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 全面程式碼品質審查 | 程式碼審查工作階段 | 專案 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架構設計 | 新功能規劃 | 專案 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 深度程式碼庫分析 | 理解現有功能 | 專案 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | Clean Code 原則審查 | 可維護性審查 | 專案 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 測試策略與涵蓋率 | 測試規劃 | 專案 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術文件撰寫 | API 文件、指南 | 專案 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 資安導向審查 | 資安稽核 | 專案 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能實作 | 功能開發 | 專案 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根本原因分析 | 錯誤調查 | 使用者 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查詢、資料分析 | 資料任務 | 使用者 | `cp 04-subagents/data-scientist.md .claude/agents/` |

> **範圍**：`使用者` = 個人（`~/.claude/agents/`），`專案` = 團隊共用（`.claude/agents/`）

**參考資料**：[04-subagents/](04-subagents/) | [官方文件](https://code.claude.com/docs/en/sub-agents)

**快速安裝（所有自訂代理）**：
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## Skills

具備指令、指令稿與範本的自動呼叫能力。

### 範例 Skills

| Skill | 說明 | 自動呼叫時機 | 範圍 | 安裝方式 |
|-------|-------------|-------------------|-------|--------------|
| `code-review` | 全面程式碼審查 | 「審查此程式碼」、「檢查品質」 | 專案 | `cp -r 03-skills/code-review .claude/skills/` |
| `brand-voice` | 品牌一致性檢查器 | 撰寫行銷文案 | 專案 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文件產生器 | 「產生文件」、「記錄 API」 | 專案 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系統化程式碼重構（Martin Fowler） | 「重構此段」、「清理程式碼」 | 使用者 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **範圍**：`使用者` = 個人（`~/.claude/skills/`），`專案` = 團隊共用（`.claude/skills/`）

### Skill 結構

```
~/.claude/skills/skill-name/
├── SKILL.md          # Skill 定義與指令
├── scripts/          # 輔助指令稿
└── templates/        # 輸出範本
```

### Skill Frontmatter 欄位

Skills 在 `SKILL.md` 中支援 YAML frontmatter 設定：

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | Skill 顯示名稱 |
| `description` | string | Skill 的功能說明 |
| `autoInvoke` | array | 自動呼叫的觸發詞 |
| `effort` | string | 推理努力程度（`low`、`medium`、`high`） |
| `shell` | string | 指令稿使用的 shell（`bash`、`zsh`、`sh`） |

**參考資料**：[03-skills/](03-skills/) | [官方文件](https://code.claude.com/docs/en/skills)

**快速安裝（所有 Skills）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 隨附的 Skills

| Skill | 說明 | 自動呼叫時機 |
|-------|-------------|-------------------|
| `/simplify` | 審查程式碼品質 | 撰寫程式碼後 |
| `/batch` | 對多個檔案執行提示 | 批次操作 |
| `/debug` | 除錯失敗的測試／錯誤 | 除錯工作階段 |
| `/loop` | 定期執行提示 | 週期性任務 |
| `/claude-api` | 使用 Claude API 建立應用程式 | API 開發 |

---

## Plugins

指令、代理、MCP 伺服器和 hooks 的捆綁集合。

### 範例 Plugins

| Plugin | 說明 | 元件 | 使用時機 | 範圍 | 安裝方式 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR 審查工作流程 | 3 個指令、3 個代理、GitHub MCP | 程式碼審查 | 專案 | `/plugin install pr-review` |
| `devops-automation` | 部署與監控 | 4 個指令、3 個代理、K8s MCP | DevOps 任務 | 專案 | `/plugin install devops-automation` |
| `documentation` | 文件產生套件 | 4 個指令、3 個代理、範本 | 文件撰寫 | 專案 | `/plugin install documentation` |

> **範圍**：`專案` = 團隊共用，`使用者` = 個人工作流程

### Plugin 結構

```
.claude-plugin/
├── plugin.json       # 清單檔案
├── commands/         # Slash commands
├── agents/           # Subagents
├── skills/           # Skills
├── mcp/              # MCP 設定
├── hooks/            # Hook 指令稿
└── scripts/          # 工具指令稿
```

**參考資料**：[07-plugins/](07-plugins/) | [官方文件](https://code.claude.com/docs/en/plugins)

**Plugin 管理指令**：
```bash
/plugin list              # 列出已安裝的 plugins
/plugin install <name>    # 安裝 plugin
/plugin remove <name>     # 移除 plugin
/plugin update <name>     # 更新 plugin
```

---

## MCP Servers

用於存取外部工具和 API 的 Model Context Protocol 伺服器。

### 常見 MCP 伺服器

| 伺服器 | 說明 | 使用時機 | 範圍 | 安裝方式 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 管理、issues、程式碼 | GitHub 工作流程 | 專案 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查詢、資料存取 | 資料庫操作 | 專案 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 進階檔案操作 | 複雜檔案任務 | 使用者 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 團隊溝通 | 通知、更新 | 專案 | 在設定中配置 |
| **Google Docs** | 文件存取 | 文件編輯、審閱 | 專案 | 在設定中配置 |
| **Asana** | 專案管理 | 任務追蹤 | 專案 | 在設定中配置 |
| **Stripe** | 付款資料 | 財務分析 | 專案 | 在設定中配置 |
| **Memory** | 持久化記憶體 | 跨工作階段召回 | 使用者 | 在設定中配置 |
| **Context7** | 函式庫文件 | 查詢最新文件 | 內建 | 內建 |

> **範圍**：`專案` = 團隊（`.mcp.json`），`使用者` = 個人（`~/.claude.json`），`內建` = 預先安裝

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

**參考資料**：[05-mcp/](05-mcp/) | [MCP 協定文件](https://modelcontextprotocol.io)

**快速安裝（GitHub MCP）**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## Hooks

事件驅動的自動化，在 Claude Code 事件發生時執行 shell 指令。

### Hook 事件

| 事件 | 說明 | 觸發時機 | 使用情境 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 工作階段開始／恢復 | 工作階段初始化 | 設置任務 |
| `InstructionsLoaded` | 指令已載入 | CLAUDE.md 或規則檔案載入後 | 自訂指令處理 |
| `UserPromptSubmit` | 提示處理前 | 使用者傳送訊息 | 輸入驗證 |
| `PreToolUse` | 工具執行前 | 任何工具執行前 | 驗證、記錄 |
| `PermissionRequest` | 顯示權限對話框 | 敏感操作前 | 自訂核准流程 |
| `PostToolUse` | 工具執行成功後 | 任何工具完成後 | 格式化、通知 |
| `PostToolUseFailure` | 工具執行失敗後 | 工具錯誤後 | 錯誤處理、記錄 |
| `Notification` | 發送通知 | Claude 傳送通知 | 外部警報 |
| `SubagentStart` | 子代理生成 | 子代理任務開始 | 初始化子代理上下文 |
| `SubagentStop` | 子代理完成 | 子代理任務完成 | 串接動作 |
| `Stop` | Claude 完成回應 | 回應完成 | 清理、報告 |
| `StopFailure` | API 錯誤結束輪次 | 發生 API 錯誤 | 錯誤恢復、記錄 |
| `TeammateIdle` | 夥伴代理閒置 | 代理團隊協調 | 分配工作 |
| `TaskCompleted` | 任務標記為完成 | 任務完成 | 任務後處理 |
| `TaskCreated` | 透過 TaskCreate 建立任務 | 建立新任務 | 任務追蹤、記錄 |
| `ConfigChange` | 設定已更新 | 修改設定 | 回應設定變更 |
| `CwdChanged` | 工作目錄變更 | 目錄變更 | 目錄專屬設置 |
| `FileChanged` | 監看的檔案變更 | 檔案修改 | 檔案監控、重建 |
| `PreCompact` | 壓縮操作前 | 上下文壓縮 | 狀態保存 |
| `PostCompact` | 壓縮完成後 | 壓縮完成 | 壓縮後動作 |
| `WorktreeCreate` | 正在建立 worktree | 建立 Git worktree | 設置 worktree 環境 |
| `WorktreeRemove` | 正在移除 worktree | 移除 Git worktree | 清理 worktree 資源 |
| `Elicitation` | MCP 伺服器請求輸入 | MCP elicitation | 輸入驗證 |
| `ElicitationResult` | 使用者回應問詢 | 使用者回應 | 回應處理 |
| `SessionEnd` | 工作階段終止 | 工作階段終止 | 清理、儲存狀態 |

### 範例 Hooks

| Hook | 說明 | 事件 | 範圍 | 安裝方式 |
|------|-------------|-------|-------|--------------|
| `validate-bash.py` | 指令驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 資安掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自動格式化 | PostToolUse:Write | 使用者 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 提示驗證 | UserPromptSubmit | 專案 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | Token 使用量追蹤 | Stop | 使用者 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 提交前驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 指令記錄 | PostToolUse:Bash | 使用者 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |

> **範圍**：`專案` = 團隊（`.claude/settings.json`），`使用者` = 個人（`~/.claude/settings.json`）

### Hook 設定

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

**參考資料**：[06-hooks/](06-hooks/) | [官方文件](https://code.claude.com/docs/en/hooks)

**快速安裝（所有 Hooks）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## Memory Files

跨工作階段自動載入的持久化上下文。

### 記憶體類型

| 類型 | 位置 | 範圍 | 使用時機 |
|------|----------|-------|-------------|
| **Managed Policy** | 組織管理的政策 | 組織 | 強制執行全組織標準 |
| **Project** | `./CLAUDE.md` | 專案（團隊） | 團隊標準、專案上下文 |
| **Project Rules** | `.claude/rules/` | 專案（團隊） | 模組化專案規則 |
| **User** | `~/.claude/CLAUDE.md` | 使用者（個人） | 個人偏好 |
| **User Rules** | `~/.claude/rules/` | 使用者（個人） | 模組化個人規則 |
| **Local** | `./CLAUDE.local.md` | 本地（git 忽略） | 機器專屬覆寫（截至 2026 年 3 月官方文件未提及，可能為舊版功能） |
| **Auto Memory** | 自動 | 工作階段 | 自動擷取的洞察與修正 |

> **範圍**：`組織` = 由管理員管理，`專案` = 透過 git 與團隊共用，`使用者` = 個人偏好，`本地` = 不提交，`工作階段` = 自動管理

**參考資料**：[02-memory/](02-memory/) | [官方文件](https://code.claude.com/docs/en/memory)

**快速安裝**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## New Features (March 2026)

| 功能 | 說明 | 使用方式 |
|---------|-------------|------------|
| **Remote Control** | 透過 API 遠端控制 Claude Code 工作階段 | 使用遠端控制 API 以程式化方式傳送提示並接收回應 |
| **Web Sessions** | 在瀏覽器環境中執行 Claude Code | 透過 `claude web` 或 Anthropic Console 存取 |
| **Desktop App** | Claude Code 原生桌面應用程式 | 使用 `/desktop` 或從 Anthropic 網站下載 |
| **Agent Teams** | 協調多個代理處理相關任務 | 設定相互協作且共用上下文的夥伴代理 |
| **Task List** | 背景任務管理與監控 | 使用 `/tasks` 檢視和管理背景操作 |
| **Prompt Suggestions** | 基於上下文的指令建議 | 根據目前上下文自動顯示建議 |
| **Git Worktrees** | 用於平行開發的獨立 Git worktrees | 使用 worktree 指令安全地進行平行分支工作 |
| **Sandboxing** | 用於安全的隔離執行環境 | 使用 `/sandbox` 切換；在受限環境中執行指令 |
| **MCP OAuth** | MCP 伺服器的 OAuth 驗證 | 在 MCP 伺服器設定中配置 OAuth 認證以進行安全存取 |
| **MCP Tool Search** | 動態搜尋和發現 MCP 工具 | 使用工具搜尋在已連接的伺服器中尋找可用 MCP 工具 |
| **Scheduled Tasks** | 使用 `/loop` 和 cron 工具設定週期性任務 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome Integration** | 使用 headless Chromium 進行瀏覽器自動化 | 使用 `--chrome` 旗標或 `/chrome` 指令 |
| **Keyboard Customization** | 自訂按鍵綁定，包含和弦支援 | 使用 `/keybindings` 或編輯 `~/.claude/keybindings.json` |
| **Auto Mode** | 完全自主操作，無需權限提示（Research Preview） | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **Channels** | 多管道通訊（Telegram、Slack 等）（Research Preview） | 設定管道 plugins；2026 年 3 月 |
| **Voice Dictation** | 提示的語音輸入 | 使用麥克風圖示或語音按鍵綁定 |
| **Agent Hook Type** | 生成子代理而非執行 shell 指令的 hooks | 在 hook 設定中指定 `"type": "agent"` |
| **Prompt Hook Type** | 將提示文字注入對話的 hooks | 在 hook 設定中指定 `"type": "prompt"` |
| **MCP Elicitation** | MCP 伺服器可在工具執行期間請求使用者輸入 | 透過 `Elicitation` 和 `ElicitationResult` hook 事件處理 |
| **WebSocket MCP Transport** | MCP 伺服器連接的 WebSocket 傳輸 | 在 MCP 伺服器設定中使用 `"transport": "websocket"` |
| **Plugin LSP Support** | 透過 plugins 整合 Language Server Protocol | 在 `plugin.json` 中設定 LSP 伺服器以獲得編輯器功能 |
| **Managed Drop-ins** | 組織管理的嵌入式設定（v2.1.83） | 由管理員透過受管政策設定；自動套用至所有使用者 |

---

## 快速參考矩陣

### 功能選擇指南

| 需求 | 建議功能 | 原因 |
|------|---------------------|-----|
| 快速快捷方式 | Slash Command | 手動、即時 |
| 持久化上下文 | Memory | 自動載入 |
| 複雜自動化 | Skill | 自動呼叫 |
| 專業任務 | Subagent | 獨立上下文 |
| 外部資料 | MCP Server | 即時存取 |
| 事件自動化 | Hook | 事件觸發 |
| 完整解決方案 | Plugin | 一體化套件 |

### 安裝優先順序

| 優先順序 | 功能 | 指令 |
|----------|---------|---------|
| 1. 必要 | Memory | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | Slash Commands | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品質 | Subagents | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | Hooks | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 進階 | Skills | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整 | Plugins | `/plugin install pr-review` |

---

## 一鍵完整安裝

從本儲存庫安裝所有範例：

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

---

## 額外資源

- [Claude Code 官方文件](https://code.claude.com/docs/en/overview)
- [MCP 協定規範](https://modelcontextprotocol.io)
- [學習路線圖](LEARNING-ROADMAP.md)
- [主要 README](README.md)

---

**最後更新**：2026 年 3 月
