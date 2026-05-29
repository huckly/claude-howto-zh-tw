<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目錄

> 一份關於 Claude Code 所有功能的快速參考：命令、agents、skills、plugins 與 hooks。

**目錄**: [命令](#斜線命令) | [權限模式](#權限模式) | [Subagents](#subagents) | [Skills](#skills) | [Plugins](#plugins) | [MCP Servers](#mcp-servers) | [Hooks](#hooks) | [Memory](#memory-files) | [2026 年 5 月新功能](#2026-年-5-月新功能)

---

## 概覽

| 功能 | 內建 | 範例 | 總計 | 參考資料 |
|---------|----------|----------|-------|-----------|
| **Slash Commands** | 68+ | 8 | 68+ | [01-slash-commands/](01-slash-commands/) |
| **Subagents** | 6 | 11 | 17 | [04-subagents/](04-subagents/) |
| **Skills** | 9 個內建 | 6 | 15 | [03-skills/](03-skills/) |
| **Plugins** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP Servers** | 1 | 8 | 9 | [05-mcp/](05-mcp/) |
| **Hooks** | 29 個事件 | 8 | 8 | [06-hooks/](06-hooks/) |
| **Memory** | 7 種類型 | 3 | 3 | [02-memory/](02-memory/) |
| **總計** | **110** | **47** | **125** | |

---

## 斜線命令

命令是使用者手動觸發的快捷操作，用來執行特定任務。

### 內建命令

| 命令 | 說明 | 使用時機 |
|---------|-------------|-------------|
| `/help` | 顯示說明資訊 | 開始上手、學習命令 |
| `/btw` | 提出不寫入主要上下文的插話問題 | 臨時補充問題 |
| `/chrome` | 設定 Chrome 整合 | 瀏覽器自動化 |
| `/clear` | 清除對話記錄 | 重新開始、減少上下文 |
| `/diff` | 互動式 diff 檢視器 | 審查變更 |
| `/config` | 查看/編輯設定 | 自定義行為 |
| `/status` | 顯示會話狀態 | 查看目前狀態 |
| `/agents` | 列出可用的 agents | 查看委派選項 |
| `/skills` | 列出可用的 skills | 查看可自動觸發的能力 |
| `/hooks` | 列出已設定的 hooks | 除錯自動化流程 |
| `/insights` | 分析會話模式 | 最佳化會話使用 |
| `/install-slack-app` | 安裝 Claude Slack 應用程式 | Slack 整合 |
| `/keybindings` | 自定義鍵盤快捷鍵 | 按鍵自定義 |
| `/mcp` | 列出 MCP servers | 檢查外部整合 |
| `/memory` | 查看已載入的 memory 檔案 | 除錯上下文載入 |
| `/mobile` | 產生行動裝置 QR code | 行動裝置存取 |
| `/passes` | 查看使用通行證 | 訂閱資訊 |
| `/plugin` | 管理外掛 | 安裝/移除擴充功能 |
| `/plan` | 進入規劃模式 | 複雜實作 |
| `/proactive` | `/loop` 的別名 (v2.1.105) | 與 `/loop` 相同 |
| `/recap` | 返回現有會話時顯示會話摘要 | 離開一段時間後取得已完成工作的上下文 |
| `/rewind` | 回退到檢查點 | 撤銷變更、探索替代方案 |
| `/checkpoint` | 管理檢查點 | 儲存/還原狀態 |
| `/cost` | 開啟 `/usage` 成本分頁的捷徑別名 (v2.1.118+) | 監控費用 |
| `/context` | 顯示上下文視窗使用情況 | 管理對話長度 |
| `/export` | 匯出對話 | 儲存以供參考 |
| `/usage-credits` | 設定額外使用限制 (v2.1.144 從 `/extra-usage` 更名；舊名稱仍可作為別名使用) | 速率限制管理 |
| `/feedback` | 提交回饋或錯誤報告 | 回報問題 |
| `/login` | 向 Anthropic 進行身分驗證 | 存取功能 |
| `/logout` | 登出 | 切換帳號 |
| `/sandbox` | 切換沙盒模式 | 安全的指令執行 |
| `/doctor` | 執行診斷 | 疑難排解問題 |
| `/reload-plugins` | 重新載入已安裝的外掛 | 外掛管理 |
| `/release-notes` | 顯示版本說明 | 查看新功能 |
| `/remote-control` | 啟用遠端控制 | 遠端存取 |
| `/permissions` | 管理權限 | 控制存取 |
| `/session` | 管理會話 | 多會話工作流程 |
| `/rename` | 重新命名目前會話 | 整理會話 |
| `/resume` | 恢復先前會話 | 繼續工作 |
| `/todo` | 查看/管理待辦事項清單 | 追蹤任務 |
| `/tui` | 切換全螢幕 TUI（文字使用者介面）模式 | 在全螢幕/tmux 中進行無閃爍渲染 |
| `/tasks` | 查看背景任務 | 監控非同步操作 |
| `/copy` | 將最後的回應複製到剪貼簿 | 快速分享輸出內容 |
| `/teleport` | 將會話傳輸到另一台機器 | 遠端繼續工作 |
| `/desktop` | 開啟 Claude Desktop 應用程式 | 切換至桌面介面 |
| `/theme` | 更改配色主題；v2.1.118 新增透過 `~/.claude/themes/<name>.json` 自定義命名主題（外掛可附帶 `themes/` 目錄） | 自定義外觀 |
| `/usage` | 使用量/成本/統計數據的標準指令 — 將 `/cost` 與 `/stats` 合併為單一分頁檢視 (v2.1.118)；自 v2.1.149 起，成本檢視會依類別細分費用（skills、subagents、plugins、每個 MCP server） | 監控配額與成本 |
| `/focus` | 切換專注檢視模式（無干擾的輸出顯示） | 在長時間任務中減少視覺干擾 |
| `/fork` | 分叉目前對話 | 探索其他可能性 |
| `/stats` | 開啟 `/usage` 統計分頁的捷徑別名 (v2.1.118+) | 查看會話指標 |
| `/statusline` | 設定狀態列 | 自定義狀態顯示 |
| `/stickers` | 查看會話貼紙 | 趣味獎勵 |
| `/fast` | 切換快速輸出模式 | 加速回應速度 |
| `/terminal-setup` | 設定終端機整合 | 設定終端機功能 |
| `/undo` | `/rewind` 的別名 (v2.1.108) | 與 `/rewind` 相同 |
| `/upgrade` | 檢查更新 | 版本管理 |
| `/team-onboarding` | 根據此專案的 Claude Code 使用情況生成團隊成員上手指南 | 新成員入職引導 (v2.1.101) |
| `/ultraplan` | 在規劃模式下將規劃任務交給 Claude Code 網頁會話 | 重度規劃任務卸載 (Research Preview, v2.1.91+) |
| `/ultrareview` | 對您目前的變更執行雲端多代理程式碼審查 | 跨多個代理程式的深度合併前審查 (v2.1.112) |
| `/less-permission-prompts` | 掃描記錄並為常見的唯讀工具提出優先順序排列的允許清單 | 減少專案中重複的權限提示 (v2.1.112) |

### 自定義指令（範例）

| 指令 | 說明 | 使用時機 | 範圍 | 安裝方式 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 分析程式碼以進行最佳化 | 效能改進 | 專案 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 準備 Pull Request | 在提交 PR 之前 | 專案 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 生成 API 文件 | 文件化 API | 專案 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 帶有上下文的 git commit | 提交變更 | 使用者 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | Stage、commit 並 push | 快速部署 | 使用者 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重構文件結構 | 改進文件 | 專案 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 設定 CI/CD 流水線 | 新專案 | 專案 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 擴展測試覆蓋率 | 改進測試 | 專案 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **範圍**: `User` = 個人工作流程 (`~/.claude/commands/`)，`Project` = 團隊共享 (`.claude/commands/`)

**參考資料**: [01-slash-commands/](01-slash-commands/) | [官方文件](https://code.claude.com/docs/en/interactive-mode)

**快速安裝（所有自定義指令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 權限模式

Claude Code 支援 6 種權限模式，用以控制工具使用的授權方式。

| 模式 | 說明 | 使用時機 |
|------|-------------|-------------|
| `default` | 每次工具呼叫時進行提示 | 標準互動式使用 |
| `acceptEdits` | 自動接受檔案編輯，其他操作則進行提示 | 受信任的編輯工作流程 |
| `plan` | 僅限唯讀工具，不允許寫入 | 規劃與探索 |
| `auto` | 自動接受所有工具，無需提示 | 完全自主運行 (Research Preview) |
| `bypassPermissions` | 跳過所有權限檢查 | CI/CD、無介面 (headless) 環境 |
| `dontAsk` | 跳過需要權限的工具 | 非互動式腳本 |

> **注意**: `auto` 模式為 Research Preview 功能 (2026 年 3 月)。請僅在受信任的沙盒環境中使用 `bypassPermissions`。

**參考資料**: [官方文件](https://code.claude.com/docs/en/permissions)

---

## Subagents

具有隔離上下文的專門化 AI 助手，用於執行特定任務。

### 內建 Subagents

| 代理 | 說明 | 工具 | 模型 | 使用時機 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 多步驟任務、研究 | 所有工具 | 繼承當前模型 | 複雜研究、多檔案任務 |
| **Plan** | 實作規劃 | Read、Glob、Grep、Bash | 繼承當前模型 | 架構設計、規劃 |
| **Explore** | 程式碼庫探索 | Read、Glob、Grep | Haiku 4.5 | 快速搜尋、理解程式碼 |
| **Bash** | 指令執行 | Bash | 繼承當前模型 | Git 操作、終端機任務 |
| **statusline-setup** | 狀態列設定 | Bash、Read、Write | Sonnet 4.6 | 設定狀態列顯示 |
| **Claude Code Guide** | 說明與文件 | Read、Glob、Grep | Haiku 4.5 | 取得說明、學習功能 |

### Subagent 設定欄位

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | 代理識別碼 |
| `description` | string | 此代理的用途 |
| `model` | string | 模型覆蓋值（例如 `haiku-4.5`） |
| `tools` | array | 允許使用的工具清單 |
| `effort` | string | 推理強度等級（`low`、`medium`、`high`） |
| `initialPrompt` | string | 代理啟動時注入的系統提示詞 |
| `disallowedTools` | array | 明確禁止此代理使用的工具 |

### 自定義 Subagents（範例）

| 代理 | 說明 | 使用時機 | 範圍 | 安裝方式 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 全面的程式碼品質審查 | 程式碼審查會話 | 專案 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `code-architect` | 功能架構設計 | 新功能規劃 | 專案 | `cp 04-subagents/code-architect.md .claude/agents/` |
| `code-explorer` | 程式碼庫深度分析 | 理解現有功能 | 專案 | `cp 04-subagents/code-explorer.md .claude/agents/` |
| `clean-code-reviewer` | Clean Code 原則審查 | 維護性審查 | 專案 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 測試策略與覆蓋率 | 測試規劃 | 專案 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術文件撰寫 | API 文件、指南 | 專案 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 以安全性為核心的審查 | 安全審核 | 專案 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能實作 | 功能開發 | 專案 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根本原因分析 | 錯誤調查 | 使用者 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查詢、資料分析 | 資料任務 | 使用者 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | 效能分析與調優 | 瓶頸調查 | 專案 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **範圍**: `User` = 個人 (`~/.claude/agents/`)，`Project` = 團隊共享 (`.claude/agents/`)

**參考資料**: [04-subagents/](04-subagents/) | [官方文件](https://code.claude.com/docs/en/sub-agents)

**快速安裝（所有自定義代理）**：
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## Skills

包含指令、腳本與範本的自動觸發能力。

### 範例 Skills

| Skill | 說明 | 何時自動觸發 | 範圍 | 安裝方式 |
|-------|-------------|-------------------|-------|--------------|
| `code-review-specialist` | 全面的程式碼審查 | "Review this code"、"Check quality" | 專案 | `cp -r 03-skills/code-review-specialist .claude/skills/` |
| `brand-voice` | 品牌一致性檢查器 | 撰寫行銷文案時 | 專案 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文件產生器 | "Generate docs"、"Document API" | 專案 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系統化程式碼重構（Martin Fowler） | "Refactor this"、"Clean up code" | 使用者 | `cp -r 03-skills/refactor ~/.claude/skills/` |

> **範圍**: `User` = 個人 (`~/.claude/skills/`)，`Project` = 團隊共享 (`.claude/skills/`)

### Skill 結構

```
~/.claude/skills/skill-name/
├── SKILL.md          # Skill 定義與指令
├── scripts/          # 輔助腳本
└── templates/        # 輸出範本
```

### Skill Frontmatter 欄位

Skills 支援在 `SKILL.md` 中使用 YAML frontmatter 進行設定：

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | Skill 顯示名稱 |
| `description` | string | 此 skill 的功能 |
| `autoInvoke` | array | 用於自動觸發的觸發詞組 |
| `effort` | string | 推理強度等級（`low`、`medium`、`high`） |
| `shell` | string | 腳本使用的 Shell（`bash`、`zsh`、`sh`） |

**參考資料**: [03-skills/](03-skills/) | [官方文件](https://code.claude.com/docs/en/skills)

**快速安裝（所有 skills）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 內建 Skills

| Skill | 說明 | 何時自動觸發 |
|-------|-------------|-------------------|
| `/batch` | 對多個檔案執行提示詞 | 批次操作 |
| `/claude-api` | 使用 Claude API 開發應用程式 | API 開發 |
| `/debug` | 除錯失敗的測試/錯誤 | 除錯會話 |
| `/fewer-permission-prompts` | 掃描記錄並為常見的唯讀工具提出優先順序排列的允許清單 | 減少重複的權限提示 |
| `/loop` | 定期執行提示詞 | 循環任務 |
| `/run` *(v2.1.145+)* | 啟動此專案的應用程式以查看變更是否生效 | 在真實應用程式中驗證變更 |
| `/run-skill-generator` *(v2.1.145+)* | 教導 `/run`/`/verify` 如何處理特定專案 | 針對 `/run` 的首次專案設定 |
| `/code-review` *(v2.1.146 從 `/simplify` 更名)* | 以指定強度等級審查目前的 diff 是否有正確性錯誤（例如 `/code-review high`）；加上 `--comment` 可將結果發布為 PR 內嵌評論 | 撰寫程式碼後、合併 PR 前 |
| `/verify` *(v2.1.145+)* | 建置、執行並觀察應用程式，以確認修復是否生效 | 端對端驗證修復成效 |

---

## 外掛

包含指令、代理、MCP servers 與 hooks 的集合包。

### 外掛範例

| 外掛 | 說明 | 組件 | 使用時機 | 範圍 | 安裝方式 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR review 工作流程 | 3 個指令、3 個代理、GitHub MCP | 程式碼審查 | 專案 | `/plugin install pr-review` |
| `devops-automation` | 部署與監控 | 4 個指令、3 個代理、K8s MCP | DevOps 任務 | 專案 | `/plugin install devops-automation` |
| `documentation` | 文件生成套件 | 4 個指令、3 個代理、範本 | 文件撰寫 | 專案 | `/plugin install documentation` |

> **範圍**: `Project` = 團隊共享，`User` = 個人工作流程

### 外掛結構

```
.claude-plugin/
├── plugin.json       # Manifest 檔案
├── commands/         # 斜線命令
├── agents/           # Subagents
├── skills/           # Skills
├── mcp/              # MCP 設定
├── hooks/            # Hook 腳本
└── scripts/          # 工具腳本
```

**參考資料**: [07-plugins/](07-plugins/) | [官方文件](https://code.claude.com/docs/en/plugins)

**外掛管理指令**：
```bash
/plugin list              # 列出已安裝的外掛
/plugin install <name>    # 安裝外掛
/plugin remove <name>     # 移除外掛
/plugin update <name>     # 更新外掛
```

---

## MCP Servers

用於存取外部工具與 API 的 Model Context Protocol servers。

### 常見 MCP Servers

| Server | 說明 | 使用時機 | 範圍 | 安裝方式 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 管理、issues、程式碼 | GitHub 工作流程 | 專案 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查詢、資料存取 | 資料庫操作 | 專案 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 進階檔案操作 | 複雜的檔案任務 | 使用者 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 團隊溝通 | 通知、更新 | 專案 | 在設定中配置 |
| **Google Docs** | 文件存取 | 文件編輯、審查 | 專案 | 在設定中配置 |
| **Asana** | 專案管理 | 任務追蹤 | 專案 | 在設定中配置 |
| **Stripe** | 付款資料 | 財務分析 | 專案 | 在設定中配置 |
| **Memory** | 持久化記憶 | 跨會話回溯 | 使用者 | 在設定中配置 |
| **Context7** | 函式庫文件 | 查詢最新文件 | 內建 | 內建 |

> **範圍**: `Project` = 團隊 (`.mcp.json`)，`User` = 個人 (`~/.claude.json`)，`Built-in` = 預裝

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

**參考資料**: [05-mcp/](05-mcp/) | [MCP 協定文件](https://modelcontextprotocol.io)

**快速安裝（GitHub MCP）**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## Hooks

事件驅動的自動化功能，可在 Claude Code 事件發生時執行 shell 命令。

### Hook 事件

| 事件 | 說明 | 觸發時機 | 使用案例 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 會話開始/恢復 | 會話初始化 | 設定任務 |
| `Setup` | 初始環境設定（每個會話僅執行一次） | 首次會話引導程序 | 佈建工具、安裝相依項目 |
| `InstructionsLoaded` | 指令已載入 | CLAUDE.md 或規則檔案載入時 | 自定義指令處理 |
| `UserPromptSubmit` | 在提示詞處理前 | 使用者發送訊息時 | 輸入驗證 |
| `UserPromptExpansion` | 使用者提示詞已展開（@-mentions、斜線命令已解析） | 展開後、提交前 | 轉換或檢查展開後的提示詞 |
| `PreToolUse` | 在工具執行前 | 在任何工具執行前 | 驗證、記錄 |
| `PermissionRequest` | 顯示權限對話框 | 在敏感操作前 | 自定義審核流程 |
| `PermissionDenied` | 使用者拒絕權限提示 | 權限被拒絕後 | 記錄、分析、政策執行 |
| `PostToolUse` | 工具執行成功後 | 在任何工具完成後 | 格式化、通知 |
| `PostToolUseFailure` | 工具執行失敗 | 在工具錯誤後 | 錯誤處理、記錄 |
| `PostToolBatch` | 一批工具使用完成後 | 工具批次結束時 | 彙總報告、批次驗證 |
| `Notification` | 已發送通知 | Claude 發送通知時 | 外部警報 |
| `SubagentStart` | 子代理已啟動 | 子代理任務開始時 | 初始化子代理上下文 |
| `SubagentStop` | 子代理已結束 | 子代理任務完成時 | 串聯動作 |
| `Stop` | Claude 完成回應 | 回應完成時 | 清理、報告 |
| `StopFailure` | API 錯誤結束回合 | 發生 API 錯誤時 | 錯誤恢復、記錄 |
| `TeammateIdle` | 隊友代理閒置 | 代理團隊協調時 | 分配工作 |
| `TaskCompleted` | 任務被標記為完成 | 任務完成時 | 任務後處理 |
| `TaskCreated` | 透過 TaskCreate 建立任務 | 新任務建立時 | 任務追蹤、記錄 |
| `ConfigChange` | 設定已更新 | 設定被修改時 | 對設定變更做出反應 |
| `CwdChanged` | 工作目錄變更 | 目錄已變更時 | 特定目錄的設定 |
| `FileChanged` | 監控的檔案變更 | 檔案被修改時 | 檔案監控、重新建置 |
| `PreCompact` | 在壓縮操作前 | 上下文壓縮時 | 狀態保存 |
| `PostCompact` | 壓縮完成後 | 壓縮已完成時 | 壓縮後動作 |
| `WorktreeCreate` | 正在建立 Worktree | Git worktree 已建立時 | 設定 worktree 環境 |
| `WorktreeRemove` | 正在移除 Worktree | Git worktree 已移除時 | 清理 worktree 資源 |
| `Elicitation` | MCP server 要求輸入 | MCP 誘導時 | 輸入驗證 |
| `ElicitationResult` | 使用者回應誘導 | 使用者回應時 | 回應處理 |
| `SessionEnd` | 會話終止 | 會話結束時 | 清理、儲存狀態 |

### 範例 Hooks

| Hook | 說明 | 事件 | 範圍 | 安裝方式 |
|------|-------------|-------|-------|--------------|
| `validate-bash.py` | 指令驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/validate-bash.py .claude/hooks/` |
| `security-scan.py` | 安全掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/security-scan.py .claude/hooks/` |
| `format-code.sh` | 自動格式化 | PostToolUse:Write | 使用者 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.py` | 提示詞驗證 | UserPromptSubmit | 專案 | `cp 06-hooks/validate-prompt.py .claude/hooks/` |
| `context-tracker.py` | Token 使用量追蹤 | Stop | 使用者 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `pre-commit.sh` | 提交前驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 指令記錄 | PostToolUse:Bash | 使用者 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | 針對 manifest 變更進行漏洞掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/dependency-check.sh .claude/hooks/` |

> **範圍**: `Project` = 團隊 (`.claude/settings.json`)，`User` = 個人 (`~/.claude/settings.json`)

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

**參考資料**: [06-hooks/](06-hooks/) | [官方文件](https://code.claude.com/docs/en/hooks)

**快速安裝（所有 Hooks）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## Memory Files

跨會話自動載入的持久化上下文。

### 記憶類型

| 類型 | 位置 | 範圍 | 使用時機 |
|------|----------|-------|-------------|
| **Managed Policy** | 組織管理的政策 | 組織 | 強制執行全組織標準 |
| **Project** | `./CLAUDE.md` | 專案（團隊） | 團隊標準、專案上下文 |
| **Project Rules** | `.claude/rules/` | 專案（團隊） | 模組化專案規則 |
| **User** | `~/.claude/CLAUDE.md` | 使用者（個人） | 個人偏好 |
| **User Rules** | `~/.claude/rules/` | 使用者（個人） | 模組化個人規則 |
| **Local** | `./CLAUDE.local.md` | 本機（git-ignored） | 機器特定的本機覆寫（已忽略 git 追蹤）。官方文件 https://code.claude.com/docs/en/memory 已收錄為支援的每位開發者覆寫檔案。 |
| **Auto Memory** | 自動 | 會話 | 自動擷取的洞察與修正 |

> **範圍**: `Organization` = 由管理員管理，`Project` = 透過 git 與團隊共享，`User` = 個人偏好，`Local` = 不會提交，`Session` = 自動管理

**參考資料**: [02-memory/](02-memory/) | [官方文件](https://code.claude.com/docs/en/memory)

**快速安裝**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 2026 年 5 月新功能

| 功能 | 說明 | 如何使用 |
|---------|-------------|------------|
| **/focus** | 切換專注檢視模式，以實現無干擾的輸出顯示 (v2.1.110) | 執行 `/focus` 以在長時間任務期間減少視覺雜訊 |
| **/proactive** | `/loop` 的別名 — 具有相同的循環任務行為 (v2.1.105) | 可將 `/proactive` 與 `/loop` 交替使用 |
| **/recap** | 返回現有會話時顯示會話摘要 (v2.1.108) | 在離開一段時間後執行 `/recap` 以取得已完成工作的上下文 |
| **/tui** | 切換全螢幕 TUI（文字使用者介面）模式，以實現無閃爍渲染 (v2.1.110) | 在全螢幕終端機或 tmux 中使用 `/tui` |
| **/undo** | `/rewind` 的別名 — 還原至上一個檢查點 (v2.1.108) | 可將 `/undo` 與 `/rewind` 交替使用 |
| **Monitor Tool** | 監控背景指令的 stdout 串流並對事件做出反應，而非使用輪詢 (v2.1.98+) | 透過 [Advanced Features](09-advanced-features/) 使用 Monitor 工具 |
| **/team-onboarding** | 從專案的 Claude Code 設定自動生成團隊成員上手指南 (v2.1.101) | 在您的專案中執行 `/team-onboarding` |
| **Ultraplan auto-create** | 首次呼叫 `/ultraplan` 時自動建立雲端環境 — 無需手動設定 (v2.1.101) | 使用 `/ultraplan <prompt>` |
| **Remote Control** | 透過 API 遠端控制 Claude Code 會話 | 使用遠端控制 API 以程式化方式發送提示詞並接收回應 |
| **Web Sessions** | 在瀏覽器環境中執行 Claude Code | 透過 `claude web` 或透過 Anthropic Console 進行存取 |
| **Desktop App** | Claude Code 的原生桌面應用程式 | 使用 `/desktop` 或從 Anthropic 網站下載 |
| **Agent Teams** | 協調多個處理相關任務的代理 | 設定可協作並共享上下文的團隊成員代理 |
| **Task List** | 背景任務管理與監控 | 使用 `/tasks` 查看並管理背景操作 |
| **Prompt Suggestions** | 具備上下文感知能力的指令建議 | 根據目前上下文自動顯示建議 |
| **Git Worktrees** | 用於平行開發的隔離 git worktrees | 使用 worktree 指令進行安全的平行分支工作 |
| **Sandboxing** | 用於安全性的隔離執行環境 | 使用 `/sandbox` 進行切換；在受限環境中執行指令 |
| **MCP OAuth** | MCP 伺服器的 OAuth 身份驗證 | 在 MCP 伺服器設定中配置 OAuth 憑證以進行安全存取 |
| **MCP Tool Search** | 動態搜尋與發現 MCP 工具 | 使用工具搜尋功能在已連接的伺服器中尋找可用的 MCP 工具 |
| **Scheduled Tasks** | 使用 `/loop` 和 cron 工具設定循環任務 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome Integration** | 使用 headless Chromium 進行瀏覽器自動化 | 使用 `--chrome` 旗標或 `/chrome` 指令 |
| **Keyboard Customization** | 自定義按鍵綁定，包括組合鍵（chord）支援 | 使用 `/keybindings` 或編輯 `~/.claude/keybindings.json` |
| **Auto Mode** | 完全自主運行，無需權限提示（Research Preview） | 使用 `--mode auto` 或 `/permissions auto`；2026 年 3 月 |
| **Channels** | 多頻道通訊（Telegram、Slack 等）（Research Preview） | 設定頻道外掛；2026 年 3 月 |
| **Voice Dictation** | 用於提示詞的語音輸入 | 使用麥克風圖示或語音快捷鍵 |
| **Agent Hook Type** | 產生子代理而非執行 shell 命令的 hook | 在 hook 設定中設定 `"type": "agent"` |
| **Prompt Hook Type** | 將提示詞文字注入對話的 hook | 在 hook 設定中設定 `"type": "prompt"` |
| **MCP Elicitation** | MCP 伺服器可以在工具執行期間請求使用者輸入 | 透過 `Elicitation` 和 `ElicitationResult` hook 事件處理 |
| **Plugin LSP Support** | 透過外掛整合語言伺服器協定（LSP） | 在 `plugin.json` 中配置 LSP 伺服器以獲得編輯器功能 |
| **Managed Drop-ins** | 組織管理的快速套用設定 (v2.1.83) | 由管理員透過管理政策配置；自動套用到所有使用者 |

---

## 快速參考矩陣

### 功能選擇指南

| 需求 | 建議功能 | 原因 |
|------|---------------------|-----|
| 快速捷徑 | 斜線命令 | 手動、即時 |
| 持久上下文 | Memory | 自動載入 |
| 複雜自動化 | Skill | 自動觸發 |
| 特殊任務 | Subagent | 隔離的上下文 |
| 外部資料 | MCP Server | 即時存取 |
| 事件自動化 | Hook | 事件觸發 |
| 完整解決方案 | Plugin | 一站式組合包 |

### 安裝優先順序

| 優先級 | 功能 | 指令 |
|----------|---------|---------|
| 1. 必要 | Memory | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | 斜線命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品質 | Subagents | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | Hooks | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 進階 | Skills | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整 | Plugins | `/plugin install pr-review` |

---

## 一鍵完整安裝

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

---

## 其他資源

- [Official Claude Code Documentation](https://code.claude.com/docs/en/overview)
- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Learning Roadmap](LEARNING-ROADMAP.md)
- [Main README](README.md)

---

**最後更新日期**：2026 年 5 月 29 日
**Claude Code 版本**：2.1.150
**來源**：
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/hooks
- https://github.com/anthropics/claude-code/releases/tag/v2.1.144
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
- https://github.com/anthropics/claude-code/releases/tag/v2.1.143
**相容模型**：Claude Sonnet 4.6、Claude Opus 4.7、Claude Haiku 4.5
