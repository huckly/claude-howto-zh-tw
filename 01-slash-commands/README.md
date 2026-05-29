<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 斜線命令

## 概述

斜線命令是在互動式會話中控制 Claude 行為的捷徑。它們分為幾種類型：

- **內建命令**：由 Claude Code 提供（`/help`、`/clear`、`/model`）
- **技能**：使用者定義的命令，以 `SKILL.md` 檔案形式建立（`/optimize`、`/pr`）
- **外掛命令**：來自已安裝外掛的命令（`/frontend-design:frontend-design`）
- **MCP 提示詞**：來自 MCP 伺服器的命令（`/mcp__github__list_prs`）

> **注意**：自定義斜線命令已併入技能。位於 `.claude/commands/` 中的檔案仍可運作，但現在建議使用技能（`.claude/skills/`）。兩者都會建立 `/command-name` 捷徑。請參閱 [技能指南](../03-skills/) 以獲取完整參考。

## 內建命令參考

內建命令是常用操作的捷徑。目前提供 **60 多個內建命令** 與 **5 個內建技能**。在 Claude Code 中輸入 `/` 即可查看完整列表，或在 `/` 後輸入任何字母進行篩選。

| 命令 | 用途 |
|---------|---------|
| `/add-dir <path>` | 新增工作目錄 |
| `/agents` | 管理代理配置 |
| `/branch [name]` | 將對話分支到新的會話（別名：`/fork`）。注意：`/fork` 在 v2.1.77 中已重新命名為 `/branch` |
| `/btw <question>` | 在 Claude 處理主要任務時提出附帶問題；不會污染主對話的上下文 |
| `/chrome` | 配置 Chrome 瀏覽器整合 |
| `/clear` | 清除對話（別名：`/reset`、`/new`） |
| `/color [color\|default]` | 設定提示詞列顏色。單獨使用 `/color`（不帶參數）會隨機選擇一個會話顏色 (v2.1.128+)；傳入顏色名稱或十六進位值可明確設定。 |
| `/compact [instructions]` | 壓縮對話，可選擇性加入專注指令 |
| `/config` | 開啟設定（別名：`/settings`） |
| `/context` | 以彩色網格形式視覺化上下文使用情況 |
| `/copy [N]` | 將助手回應複製到剪貼簿；`w` 會寫入檔案 |
| `/cost` | `/usage` 的打字捷徑別名 — 開啟費用頁籤 (v2.1.118+) |
| `/desktop` | 在桌面應用程式中繼續（別名：`/app`） |
| `/diff` | 未提交變更的互動式 diff 查看器 |
| `/doctor` | 診斷安裝健康狀況 — 可在 Claude 回應時開啟；顯示狀態圖示；按 `f` 可自動修復問題（v2.1.116 增強） |
| `/effort [low\|medium\|high\|xhigh\|max\|auto]` | 透過互動式方向鍵滑桿設定努力程度。層級：`low` → `medium` → `high` → `xhigh`（v2.1.111 新增）→ `max`。Opus 4.7 的預設值為 `xhigh`；`max` 需要 Opus 4.7 |
| `/exit` | 退出 REPL（別名：`/quit`） |
| `/export [filename]` | 將當前對話匯出到檔案或剪貼簿 |
| `/usage-credits` | 配置額外使用量以應對速率限制（v2.1.144 從 `/extra-usage` 重新命名；`/extra-usage` 仍可作為別名使用） |
| `/fast [on\|off]` | 切換快速模式 |
| `/feedback` | 提交回饋（別名：`/bug`）。自 v2.1.141 起，可附加最近的會話（最近 24 小時或 7 天），讓跨越多個會話的回報包含完整上下文。 |
| `/focus` | 切換專注檢視（v2.1.110 新增；取代 `Ctrl+O` 的專注切換功能） |
| `/goal <statement>` | 為當前會話登記一個完成條件；Claude 持續工作直到達成目標。`/goal clear` 可移除目標。進行中的目標會顯示在狀態列，並有即時覆蓋面板顯示已用時間、回合數與 token 使用量（v2.1.139 新增）。 |
| `/help` | 顯示說明 |
| `/hooks` | 查看鉤子配置 |
| `/ide` | 管理 IDE 整合 |
| `/init` | 初始化 `CLAUDE.md`。設定 `CLAUDE_CODE_NEW_INIT=1` 以進行互動式流程 |
| `/insights` | 生成會話分析報告 |
| `/install-github-app` | 設定 GitHub Actions 應用程式 |
| `/install-slack-app` | 安裝 Slack 應用程式 |
| `/keybindings` | 開啟按鍵綁定配置 |
| `/less-permission-prompts` | 分析最近的 Bash/MCP 工具呼叫，並在 `.claude/settings.json` 中新增優先白名單以減少權限提示（v2.1.111 新增） |
| `/login` | 切換 Anthropic 帳號 |
| `/logout` | 從您的 Anthropic 帳號登出 |
| `/mcp` | 管理 MCP 伺服器與 OAuth |
| `/memory` | 編輯 `CLAUDE.md`，切換自動記憶功能 |
| `/mobile` | 行動應用程式的 QR code（別名：`/ios`、`/android`） |
| `/model [model]` | 選擇模型，使用左右箭頭調整投入程度。自 v2.1.144 起，選擇僅預設套用於當前會話；選擇模型後按 `d` 可將其設為新會話的預設值。 |
| `/passes` | 分享一週的 Claude Code 免費使用權 |
| `/permissions` | 查看/更新權限（別名：`/allowed-tools`） |
| `/plan [description]` | 進入計畫模式 |
| `/plugin` | 管理外掛 |
| `/proactive` | `/loop` 的別名（v2.1.105 新增） |
| `/powerup` | 透過帶有動畫示範的互動式課程探索功能 |
| `/privacy-settings` | 隱私設定（僅限 Pro/Max 使用者） |
| `/release-notes` | 查看變更日誌 |
| `/recap` | 返回會話時顯示會話摘要 / 總結（v2.1.108 新增） |
| `/reload-plugins` | 重新載入啟用的外掛 |
| `/remote-control` | 從 claude.ai 進行遠端控制（別名：`/rc`） |
| `/remote-env` | 配置預設的遠端環境 |
| `/rename [name]` | 重命名會話 |
| `/resume [session]` | 恢復對話（別名：`/continue`） |
| `/review` | **已棄用** — 請改為安裝 `code-review` 外掛 |
| `/rewind` | 回溯對話及/或程式碼（別名：`/checkpoint`） |
| `/sandbox` | 切換沙盒模式 |
| `/schedule [description]` | 建立/管理雲端排程任務 |
| `/scroll-speed <+N\|-N>` | 使用即時預覽調整 TUI 即時預覽窗格的滑鼠滾輪速度。每台機器的設定會持久化儲存於 `~/.claude/preferences.json`（v2.1.139 新增）。 |
| `/security-review` | 分析分支是否存在安全性漏洞 |
| `/skills` | 列出可用技能 |
| `/stats` | `/usage` 的打字捷徑別名 — 開啟統計頁籤（每日使用量、會話、連續紀錄）(v2.1.118+) |
| `/stickers` | 訂購 Claude Code 貼紙 |
| `/status` | 顯示版本、模型、帳號 |
| `/statusline` | 配置狀態列 |
| `/tasks` | 列出/管理背景任務 |
| `/team-onboarding` | 根據專案的 Claude Code 設定生成團隊成員上手指南（v2.1.101 新增） |
| `/terminal-setup` | 配置終端機快捷鍵 |
| `/theme` | 開啟主題選擇器 / 管理自定義主題 (v2.1.118)。透過 `~/.claude/themes/<name>.json` 中的 JSON 定義自定義主題 |
| `/tui` | 切換無閃爍渲染的全螢幕 TUI（文字使用者介面）模式（v2.1.110 新增） |
| `/ultraplan <prompt>` | 在 ultraplan 會話中草擬計畫，並在瀏覽器中審查 |
| `/ultrareview` | 使用多代理分析進行全面的雲端程式碼審查（v2.1.111 新增） |
| `/undo` | `/rewind` 的別名（v2.1.108 新增） |
| `/upgrade` | 開啟升級頁面以獲取更高階的方案 |
| `/usage` | 標準使用量儀表板 (v2.1.118) — 整合方案使用限制、速率限制、費用與每日會話統計數據。`/cost` 與 `/stats` 是開啟特定頁籤的打字捷徑別名 |
| `/voice` | 切換按住說話語音輸入功能 |

### 內建技能

這些技能隨 Claude Code 一起發佈，並可像斜線命令一樣呼叫：

| 技能 | 用途 |
|-------|---------|
| `/batch <instruction>` | 使用 worktrees 編排大規模的並行變更 |
| `/claude-api` | 載入專案語言的 Claude API 參考文件 |
| `/debug [description]` | 啟用除錯日誌 |
| `/loop [interval] <prompt>` | 按間隔重複執行提示詞 |
| `/code-review [effort]` | 以指定的努力程度審查當前 diff 的正確性問題（例如 `/code-review high`）；v2.1.146 從 `/simplify` 重新命名 |

### 已棄用的命令

| 命令 | 狀態 |
|---------|--------|
| `/review` | 已棄用 — 已被 `code-review` 外掛取代 |
| `/output-style` | 自 v2.1.73 起已棄用 |
| `/fork` | 已重新命名為 `/branch`（別名仍可使用，v2.1.77） |
| `/pr-comments` | 已在 v2.1.91 中移除 — 請直接詢問 Claude 以查看 PR 評論 |
| `/vim` | 已在 v2.1.92 中移除 — 請使用 /config → Editor mode |

### 最近變更

- `/fork` 更名為 `/branch`，並保留 `/fork` 作為別名 (v2.1.77)
- `/output-style` 已棄用 (v2.1.73)
- `/review` 已棄用，改由 `code-review` 外掛取代
- 新增 `/effort` 命令，其中 `max` 層級需要 Opus 4.7（原先僅限 Opus 4.6）
- 新增 `/voice` 命令，用於按住說話（push-to-talk）語音聽寫
- 新增 `/schedule` 命令，用於建立/管理排程任務
- 新增 `/color` 命令，用於自定義提示詞列
- /pr-comments 已在 v2.1.91 中移除 — 請直接詢問 Claude 以查看 PR 評論
- /vim 已在 v2.1.92 中移除 — 請改用 /config → Editor mode
- 新增 /ultraplan，用於基於瀏覽器的計畫審查與執行
- 新增 /powerup，用於互動式功能課程
- 新增 /sandbox，用於切換沙盒模式
- `/model` 選取器現在顯示易讀的標籤（例如「Sonnet 4.6」）而非原始模型 ID
- `/resume` 支援 `/continue` 別名
- MCP 提示詞現在可透過 `/mcp__<server>__<prompt>` 命令使用（參閱 [MCP Prompts as Commands](#mcp-prompts-as-commands)）
- 新增 `/team-onboarding`，用於自動生成團隊成員上手指南 (v2.1.101)
- 新增 `/tui` 命令，用於無閃爍的全螢幕 TUI 渲染 (v2.1.110)
- 新增 `/focus` 命令，用於切換專注檢視模式；`Ctrl+O` 現在僅切換詳細逐字稿 (v2.1.110)
- 新增 `/recap` 命令，用於手動觸發會話上下文摘要 (v2.1.108)
- `/undo` 已新增為 `/rewind` 的別名 (v2.1.108)
- `/proactive` 已新增為 `/loop` 的別名 (v2.1.105)
- `/effort` 新增互動式方向鍵滑桿與 `high` 和 `max` 之間的新 `xhigh` 層級；Opus 4.7 方案的預設努力程度提升為 `xhigh` (v2.1.111)
- 新增 `/ultrareview`，用於全面的雲端多代理程式碼審查 (v2.1.111)
- 新增 `/less-permission-prompts`，用於分析 Bash/MCP 工具呼叫並透過 `.claude/settings.json` 的白名單減少權限提示 (v2.1.111)
- Auto 模式對於 Max 訂閱者使用 Opus 4.7 時不再需要 `--enable-auto-mode` 旗標 (v2.1.112)
- 新增 `/goal` — 會話層級的完成條件，Claude 跨回合持續朝目標工作；即時覆蓋面板顯示已用時間、回合數與 token 使用量 (v2.1.139)
- 新增 `/scroll-speed` — 調整 TUI 即時預覽窗格的滑鼠滾輪速度；設定每台機器持久化儲存 (v2.1.139)

### `/goal` — 會話層級的完成條件

> **v2.1.139 新增功能**

使用 `/goal` 為當前會話登記一個完成條件。Claude 跨回合持續朝目標工作，覆蓋面板會顯示已用時間、回合數與已使用的 token。使用 `/goal clear` 可清除目標。可在互動模式、`claude -p` 及遠端控制中使用。

```
User: /goal Migrate the payments service from REST to gRPC and get the integration tests passing.
Claude: Goal registered. I'll work toward this until you clear it.
[Goal panel: ⏱ 0s · turns 0 · tokens 0]

User: start by listing the REST endpoints
Claude: [does the work, panel updates]
```

### `/team-onboarding` — 團隊成員上手指南

> **v2.1.101 新增功能**

使用 `/team-onboarding` 可以根據您專案中本地的 Claude Code 使用情況來生成團隊成員上手指南。此命令會檢查您的 `CLAUDE.md`、已安裝的技能、子代理、鉤子以及最近的工作流程，然後生成一份入職文件，幫助新開發人員快速上手。

這是內建命令 — 無需安裝。

**用法：**

```bash
claude /team-onboarding
```

生成的指南摘要包含：

- 來自 [`CLAUDE.md`](../02-memory/README.md) 的專案目的與關鍵慣例
- 可用的 [skills](../03-skills/README.md) 以及它們何時會被自動呼叫
- 已配置的 [subagents](../04-subagents/README.md) 及其職責
- 在常見事件中執行的 [Hooks](../06-hooks/README.md)
- 新手應該了解的常見工作流程

**可用性：** 隨 Claude Code v2.1.101 發佈（2026 年 4 月 11 日）。

## 自定義命令（現為技能）

自定義斜線命令已**整合至技能（skills）中**。兩種方式都能建立您可以透過 `/command-name` 呼叫的命令：

| 方式 | 位置 | 狀態 |
|----------|----------|--------|
| **技能（建議）** | `.claude/skills/<name>/SKILL.md` | 目前標準 |
| **舊版命令** | `.claude/commands/<name>.md` | 仍可運作 |

如果技能與命令名稱相同，**技能將具有優先權**。例如，當 `.claude/commands/review.md` 與 `.claude/skills/review/SKILL.md` 同時存在時，將使用技能版本。

### 遷移路徑

您現有的 `.claude/commands/` 檔案可以繼續運作而無需更改。若要遷移至技能：

**之前（命令）：**
```
.claude/commands/optimize.md
```

**之後（技能）：**
```
.claude/skills/optimize/SKILL.md
```

### 為什麼要使用技能？

與舊版命令相比，技能提供了額外功能：

- **目錄結構**：可封裝腳本、範本與參考檔案
- **自動呼叫**：當相關時，Claude 可以自動觸發技能
- **呼叫控制**：可選擇由使用者、Claude 或兩者皆可呼叫
- **子代理執行**：透過 `context: fork` 在隔離的上下文中執行技能
- **漸進式揭露**：僅在需要時才載入額外檔案

### 將自定義命令建立為技能

建立一個包含 `SKILL.md` 檔案的目錄：

```bash
mkdir -p .claude/skills/my-command
```

**檔案：** `.claude/skills/my-command/SKILL.md`

```yaml
---
name: my-command
description: 此命令的功能以及何時使用
---

# My Command

當此命令被呼叫時，Claude 需遵循的指令。

1. 第一步
2. 第二步
3. 第三步
```

### Frontmatter 參考

| 欄位 | 用途 | 預設值 |
|-------|---------|---------|
| `name` | 命令名稱（將成為 `/name`） | 目錄名稱 |
| `description` | 簡短描述（幫助 Claude 判斷何時使用） | 第一段文字 |
| `argument-hint` | 用於自動完成的預期參數 | 無 |
| `allowed-tools` | 命令無需許可即可使用的工具 | 繼承 |
| `model` | 指定使用的模型 | 繼承 |
| `disable-model-invocation` | 若為 `true`，則只有使用者可以呼叫（Claude 不行） | `false` |
| `user-invocable` | 若為 `false`，則從 `/` 選單中隱藏 | `true` |
| `context` | 設定為 `fork` 以在隔離的子代理中執行 | 無 |
| `agent` | 使用 `context: fork` 時的代理類型 | `general-purpose` |
| `hooks` | 技能範圍內的鉤子（PreToolUse, PostToolUse, Stop） | 無 |

### 參數

命令可以接收參數：

**所有參數使用 `$ARGUMENTS`：**

```yaml
---
name: fix-issue
description: 透過編號修復 GitHub issue
---

按照我們的編碼標準修復 issue #$ARGUMENTS
```

用法：`/fix-issue 123` → `$ARGUMENTS` 變為 "123"

**個別參數使用 `$0`、`$1` 等：**

```yaml
---
name: review-pr
description: 優先審查 PR
---

優先審查 PR #$0，優先級為 $1
```

用法：`/review-pr 456 high` → `$0`="456", `$1`="high"

### 使用 Shell 命令進行動態上下文處理

在提示詞執行前，使用 `` !`command` `` 執行 bash 命令：

```yaml
---
name: commit
description: 建立帶有上下文的 git commit
allowed-tools: Bash(git *)
---

## Context

- 目前 git 狀態：!`git status`
- 目前 git diff：!`git diff HEAD`
- 目前分支：!`git branch --show-current`
- 最近的 commits：!`git log --oneline -5`

## Your task

根據上述變更，建立單一的 git commit。
```

### 檔案參考

使用 `@` 包含檔案內容：

```markdown
Review the implementation in @src/utils/helpers.js
Compare @src/old-version.js with @src/new-version.js
```

## 外掛命令

外掛可以提供自定義命令：

```
/plugin-name:command-name
```

或者在沒有命名衝突時，直接使用 `/command-name`。

**範例：**
```bash
/frontend-design:frontend-design
/commit-commands:commit
```

## MCP Prompts as Commands

MCP 伺服器可以將提示詞作為斜線命令公開：

```
/mcp__<server-name>__<prompt-name> [arguments]
```

**範例：**
```bash
/mcp__github__list_prs
/mcp__github__pr_review 456
/mcp__jira__create_issue "Bug title" high
```

### MCP 權限語法

在權限設定中控制 MCP 伺服器存取權：

- `mcp__github` — 存取整個 GitHub MCP 伺服器
- `mcp__github__*` — 使用萬用字元存取所有工具
- `mcp__github__get_issue` — 存取特定工具

## 命令架構

```mermaid
graph TD
    A["User Input: /command-name"] --> B{"Command Type?"}
    B -->|Built-in| C["Execute Built-in"]
    B -->|Skill| D["Load SKILL.md"]
    B -->|Plugin| E["Load Plugin Command"]
    B -->|MCP| F["Execute MCP Prompt"]

    D --> G["Parse Frontmatter"]
    G --> H["Substitute Variables"]
    H --> I["Execute Shell Commands"]
    I --> J["Send to Claude"]
    J --> K["Return Results"]
```

## 命令生命週期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/skills/ and .claude/commands/
    FS-->>Claude: Returns optimize/SKILL.md
    Claude->>Claude: Parses frontmatter
    Claude->>CLI: Executes !`command` substitutions
    CLI-->>Claude: Command outputs
    Claude->>Claude: Substitutes $ARGUMENTS
    Claude->>User: Processes prompt
    Claude->>User: Returns results
```

## 此資料夾中的可用命令

這些範例命令可以作為 skills 或舊版命令進行安裝。

### 1. `/optimize` - 程式碼優化

分析程式碼中的效能問題、記憶體洩漏以及優化機會。

**用法：**
```
/optimize
[貼上您的程式碼]
```

### 2. `/pr` - Pull Request 準備

引導完成 PR 準備檢查清單，包括 linting、測試與 commit 格式化。

**用法：**
```
/pr
```

**螢幕截圖：**
![/pr](pr-slash-command.png)

### 3. `/generate-api-docs` - API 文件產生器

從原始碼產生完整的 API 文件。

**用法：**
```
/generate-api-docs
```

### 4. `/commit` - 帶有上下文的 Git Commit

根據您儲存庫中的動態上下文建立一個 git commit。

**用法：**
```
/commit [optional message]
```

### 5. `/push-all` - 暫存、提交與推送

暫存所有變更、建立 commit 並進行安全檢查後推送至遠端。

**用法：**
```
/push-all
```

**安全檢查：**
- 敏感資訊：`.env*`, `*.key`, `*.pem`, `credentials.json`
- API Keys：偵測真實金鑰與佔位符
- 大型檔案：未經 Git LFS 的 `>10MB` 檔案
- 建置產物：`node_modules/`, `dist/`, `__pycache__/`

### 6. `/doc-refactor` - 文件重構

重構專案文件以提升清晰度與易讀性。

**用法：**
```
/doc-refactor
```

### 7. `/setup-ci-cd` - CI/CD 流水線設定

實作 pre-commit hooks 與 GitHub Actions 以進行品質保證。

**用法：**
```
/setup-ci-cd
```

### 8. `/unit-test-expand` - 測試覆蓋率擴充

透過針對未測試的分支與邊際情況來增加測試覆蓋率。

**用法：**
```
/unit-test-expand
```

## 安裝

### 作為技能（建議）

複製到您的 skills 目錄：

```bash
# 建立 skills 目錄
mkdir -p .claude/skills

# 為每個指令檔案建立一個 skill 目錄
for cmd in optimize pr commit; do
  mkdir -p .claude/skills/$cmd
  cp 01-slash-commands/$cmd.md .claude/skills/$cmd/SKILL.md
done
```

### 作為舊版命令

複製到您的 commands 目錄：

```bash
# 專案範圍（團隊使用）
mkdir -p .claude/commands
cp 01-slash-commands/*.md .claude/commands/

# 個人使用
mkdir -p ~/.claude/commands
cp 01-slash-commands/*.md ~/.claude/commands/
```

## 建立您自己的命令

### 技能範本（建議）

建立 `.claude/skills/my-command/SKILL.md`：

```yaml
---
name: my-command
description: 此命令的功能。當 [觸發條件] 時使用。
argument-hint: [可選參數]
allowed-tools: Bash(npm *), Read, Grep
---

# 命令標題

## 上下文

- 目前分支：!`git branch --show-current`
- 相關檔案：@package.json

## 指令

1. 第一步
2. 第二步（帶有參數）：$ARGUMENTS
3. 第三步

## 輸出格式

- 如何格式化回應
- 應包含的內容
```

### 僅限使用者使用的命令（無自動呼叫）

適用於具有副作用且 Claude 不應自動觸發的命令：

```yaml
---
name: deploy
description: 部署至正式環境
disable-model-invocation: true
allowed-tools: Bash(npm *), Bash(git *)
---

將應用程式部署至正式環境：

1. 執行測試
2. 建置應用程式
3. 推送到部署目標
4. 驗證部署
```

## 最佳實務

| 應該（Do） | 不應該（Don't） |
|------|---------|
| 使用清晰且具備行動導向的名稱 | 為一次性任務建立命令 |
| 包含帶有觸發條件的 `description` | 在命令中構建複雜邏輯 |
| 讓命令專注於單一任務 | 將敏感資訊寫死（Hardcode） |
| 使用 `disable-model-invocation` 來處理副作用 | 跳過 description 欄位 |
| 使用 `!` 前綴來處理動態上下文 | 假設 Claude 知道目前的狀態 |
| 將相關檔案整理在 skill 目錄中 | 將所有內容都放在單一檔案中 |

## 疑難排解

### 找不到命令（Command Not Found）

**解決方案：**
- 檢查檔案是否位於 `.claude/skills/<name>/SKILL.md` 或 `.claude/commands/<name>.md`
- 確認 frontmatter 中的 `name` 欄位與預期的命令名稱一致
- 重啟 Claude Code 會話（session）
- 執行 `/help` 查看可用命令

### 命令執行結果不如預期

**解決方案：**
- 加入更具體的指令
- 在 skill 檔案中包含範例
- 若使用 bash 命令，請檢查 `allowed-tools`
- 先使用簡單的輸入進行測試

### Skill 與 Command 衝突

如果兩者名稱相同，**skill 將具有優先權**。請刪除其中一個或重新命名。

## 相關指南

- **[Skills](../03-skills/)** — 關於 skills（自動觸發的能力）的完整參考
- **[Memory](../02-memory/)** — 透過 CLAUDE.md 實現的持久化上下文
- **[Subagents](../04-subagents/)** — 委派的 AI 代理（agents）
- **[Plugins](../07-plugins/)** — 綑綁的命令集合
- **[Hooks](../06-hooks/)** — 事件驅動的自動化

## 其他資源

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode) — 內建命令參考
- [Official Skills Documentation](https://code.claude.com/docs/en/skills) — 完整的 skills 參考
- [CLI Reference](https://code.claude.com/docs/en/cli-reference) — 命令列選項

---

**最後更新日期**：2026 年 5 月 25 日
**Claude Code 版本**：2.1.150
**來源**：
- https://code.claude.com/docs/en/slash-commands
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/commands
- https://github.com/anthropics/claude-code/releases/tag/v2.1.118
- https://github.com/anthropics/claude-code/releases/tag/v2.1.116
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/releases/tag/v2.1.141
- https://github.com/anthropics/claude-code/releases/tag/v2.1.144
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145

**相容模型**：Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5

*屬於 [Claude How To](../) 指南系列的一部分*
