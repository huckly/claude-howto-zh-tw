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
| `/btw <question>` | 提出附帶問題而不加入歷史紀錄 |
| `/chrome` | 配置 Chrome 瀏覽器整合 |
| `/clear` | 清除對話（別名：`/reset`、`/new`） |
| `/color [color\|default]` | 設定提示詞列顏色 |
| `/compact [instructions]` | 壓縮對話，可選擇性加入專注指令 |
| `/config` | 開啟設定（別名：`/settings`） |
| `/context` | 以彩色網格形式視覺化上下文使用情況 |
| `/copy [N]` | 將助手回應複製到剪貼簿；`w` 會寫入檔案 |
| `/cost` | 顯示 token 使用統計數據 |
| `/desktop` | 在桌面應用程式中繼續（別名：`/app`） |
| `/diff` | 未提交變更的互動式 diff 查看器 |
| `/doctor` | 診斷安裝健康狀況 |
| `/effort [low\|medium\|high\|max\|auto]` | 設定努力程度。`max` 需要 Opus 4.6 |
| `/exit` | 退出 REPL（別名：`/quit`） |
| `/export [filename]` | 將當前對話匯出到檔案或剪貼簿 |
| `/extra-usage` | 配置額外使用量以應對速率限制 |
| `/fast [on\|off]` | 切換快速模式 |
| `/feedback` | 提交回饋（別名：`/bug`） |
| `/focus` | 切換專注檢視（v2.1.110 新增；取代 `Ctrl+O` 的專注切換功能） |
| `/help` | 顯示說明 |
| `/hooks` | 查看鉤子配置 |
| `/ide` | 管理 IDE 整合 |
| `/init` | 初始化 `CLAUDE.md`。設定 `CLAUDE_CODE_NEW_INIT=1` 以進行互動式流程 |
| `/insights` | 生成會話分析報告 |
| `/install-github-app` | 設定 GitHub Actions 應用程式 |
| `/install-slack-app` | 安裝 Slack 應用程式 |
| `/keybindings` | 開啟按鍵綁定配置 |

| `/login` | 切換 Anthropic 帳號 |
| `/logout` | 從您的 Anthropic 帳號登出 |
| `/mcp` | 管理 MCP 伺服器與 OAuth |
| `/memory` | 編輯 `CLAUDE.md`，切換自動記憶功能 |
| `/mobile` | 行動應用程式的 QR code (別名: `/ios`, `/android`) |
| `/model [model]` | 選擇模型，使用左右箭頭調整投入程度 |
| `/passes` | 分享一週的 Claude Code 免費使用權 |
| `/permissions` | 查看/更新權限 (別名: `/allowed-tools`) |
| `/plan [description]` | 進入計畫模式 |
| `/plugin` | 管理外掛 |
| `/proactive` | `/loop` 的別名 (新增於 v2.1.105) |
| `/powerup` | 透過帶有動畫示範的互動式課程探索功能 |
| `/privacy-settings` | 隱私設定 (僅限 Pro/Max 使用者) |
| `/release-notes` | 查看變更日誌 |
| `/recap` | 返回會話時顯示會話摘要 / 總結 (新增於 v2.1.108) |
| `/reload-plugins` | 重新載入啟用的外掛 |
| `/remote-control` | 從 claude.ai 進行遠端控制 (別名: `/rc`) |
| `/remote-env` | 配置預設的遠端環境 |
| `/rename [name]` | 重命名會話 |
| `/resume [session]` | 恢復對話 (別名: `/continue`) |
| `/review` | **已棄用** — 請改為安裝 `code-review` 外掛 |
| `/rewind` | 回溯對話及/或程式碼 (別name: `/checkpoint`) |
| `/sandbox` | 切換沙盒模式 |
| `/schedule [description]` | 建立/管理雲端排程任務 |
| `/security-review` | 分析分支是否存在安全性漏洞 |
| `/skills` | 列出可用技能 |
| `/stats` | 將每日使用量、會話、連續紀錄視覺化 |
| `/stickers` | 訂購 Claude Code 貼紙 |
| `/status` | 顯示版本、模型、帳號 |
| `/statusline` | 配置狀態列 |
| `/tasks` | 列出/管理背景任務 |
| `/team-onboarding` | 根據專案的 Claude Code 設定生成團隊成員上手指南 (新增於 v2.1.101) |
| `/terminal-setup` | 配置終端機快捷鍵 |
| `/theme` | 更改配色主題 |
| `/tui` | 切換無閃爍渲染的全螢幕 TUI (文字使用者介面) 模式 (新增於 v2.1.110) |
| `/ultraplan <prompt>` | 在 ultraplan 會話中草擬計畫，並在瀏覽器中審查 |
| `/undo` | `/rewind` 的別名 (新增於 v2.1.108) |
| `/upgrade` | 開啟升級頁面以獲取更高階的方案 |
| `/usage` | 顯示方案使用限制與速率限制狀態 |
| `/voice` | 切換按住說話語音輸入功能 |

### Bundled Skills

這些技能隨 Claude Code 一起發佈，並可像斜線命令一樣呼叫：

| 技能 | 用途 |
|-------|---------|
| `/batch <instruction>` | 使用 worktrees 編排大規模的並行變更 |
| `/claude-api` | 載入專案語言的 Claude API 參考文件 |
| `/debug [description]` | 啟用除錯日誌 |
| `/loop [interval] <prompt>` | 按間隔重複執行提示詞 |
| `/simplify [focus]` | 審查變更檔案的程式碼品質 |

### Deprecated Commands

| 命令 | 狀態 |
|---------|--------|
| `/review` | 已棄用 — 已被 `code-review` 外掛取代 |
| `/output-style` | 自 v2.1.73 起已棄用 |
| `/fork` | 已重新命名為 `/branch` (別名仍可使用，v2.1.77) |

| `/pr-comments` | 已在 v2.1.91 中移除 — 請直接詢問 Claude 以查看 PR 評論 |
| `/vim` | 已在 v2.1.92 中移除 — 請使用 /config → Editor mode |

### 最近變更

- `/fork` 更名為 `/branch`，並保留 `/fork` 作為別名 (v2.1.77)
- `/output-style` 已棄用 (v2.1.73)
- `/review` 已棄用，改由 `code-review` 外掛取代
- 新增 `/effort` 命令，其中 `max` 層級需要 Opus 4.6
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
- 可用的 [skills](../03-skills/README.md) 以及它們何時會被自動調用
- 已配置的 [subagents](../04-subagents/README.md) 及其職責
- 在常見事件中執行的 [Hooks](../06-hooks/README.md)
- 新手應該了解的常見工作流程

**可用性：** 隨 Claude Code v2.1.101 發佈 (2026 年 4 月 11 日)。

## 自定義命令（現為技能）

自定義斜線命令已**整合至技能（skills）中**。兩種方式都能建立您可以透過 `/command-name` 呼叫的命令：

| 方式 | 位置 | 狀態 |
|----------|----------|--------|
| **技能 (建議)** | `.claude/skills/<name>/SKILL.md` | 目前標準 |
| **舊版命令** | `.claude/commands/<name>.md` | 仍可運作 |

如果技能與命令名稱相同，**技能將具有優先權**。例如，當 `.claude/commands/review.md` 與 `.claude/skills/review/SKILL.md` 同時存在時，將使用技能版本。

### 遷移路徑

您現有的 `.claude/commands/` 檔案可以繼續運作而無需更改。若要遷移至技能：

**之前 (命令)：**
```
.claude/commands/optimize.md
```

**之後 (技能)：**
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
| `name` | 命令名稱 (將成為 `/name`) | 目錄名稱 |
| `description` | 簡短描述 (幫助 Claude 判斷何時使用) | 第一段文字 |
| `argument-hint` | 用於自動完成的預期參數 | 無 |
| `allowed-tools` | 命令無需許可即可使用的工具 | 繼承 |
| `model` | 指定使用的模型 | 繼承 |
| `disable-model-invocation` | 若為 `true`，則只有使用者可以呼叫 (Claude 不行) | `false` |
| `user-invocable` | 若為 `false`，則從 `/` 選單中隱藏 | `true` |
| `context` | 設定為 `fork` 以在隔離的子代理中執行 | 無 |
| `agent` | 使用 `context: fork` 時的代理類型 | `general-purpose` |
| `hooks` | 技能範圍內的鉤子 (PreToolUse, PostToolUse, Stop) | 無 |

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

在提示詞執行前，使用 `!`command`` 執行 bash 命令：

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

### File References

使用 `@` 包含檔案內容：

```markdown
Review the implementation in @src/utils/helpers.js
Compare @src/old-version.js with @src/new-version.js
```

## Plugin Commands

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

### MCP Permission Syntax

在權限設定中控制 MCP 伺服器存取權：

- `mcp__github` - 存取整個 GitHub MCP 伺服器
- `mcp__github__*` - 使用萬用字元存取所有工具
- `mcp__github__get_issue` - 存取特定工具

## Command Architecture

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

### 作為技能 (推薦)

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

### 作為舊版指令

複製到您的 commands 目錄：

```bash
# 專案範圍 (團隊使用)
mkdir -p .claude/commands
cp 01-slash-commands/*.md .claude/commands/

# 個人使用
mkdir -p ~/.claude/commands
cp 01-slash-commands/*.md ~/.claude/commands/
```

## 建立您自己的指令

### 技能範本 (推薦)

建立 `.claude/skills/my-command/SKILL.md`：

```yaml
---
name: my-command
description: 此指令的功能。當 [觸發條件] 時使用。
argument-hint: [可選參數]
allowed-tools: Bash(npm *), Read, Grep
---

# 指令標題

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

### 僅限使用者使用的指令 (無自動呼叫)

適用於具有副作用且 Claude 不應自動觸發的指令：

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

| 應該 (Do) | 不應該 (Don't) |
|------|---------|
| 使用清晰且具備行動導向的名稱 | 為一次性任務建立命令 |
| 包含帶有觸發條件的 `description` | 在命令中構建複雜邏輯 |
| 讓命令專注於單一任務 | 將敏感資訊寫死 (Hardcode) |
| 使用 `disable-model-invocation` 來處理副作用 | 跳過 description 欄位 |
| 使用 `!` 前綴來處理動態 context | 假設 Claude 知道目前的狀態 |
| 將相關檔案整理在 skill 目錄中 | 將所有內容都放在單一檔案中 |

## 疑難排解

### 找不到命令 (Command Not Found)

**解決方案：**
- 檢查檔案是否位於 `.claude/skills/<name>/SKILL.md` 或 `.claude/commands/<name>.md`
- 確認 frontmatter 中的 `name` 欄位與預期的命令名稱一致
- 重啟 Claude Code 會話 (session)
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

- **[Skills](../03-skills/)** - 關於 skills（自動觸發的能力）的完整參考
- **[Memory](../02-memory/)** - 透過 CLAUDE.md 實現的持久化 context
- **[Subagents](../04-subagents/)** - 委派的 AI 代理 (agents)
- **[Plugins](../07-plugins/)** - 綑綁的命令集合
- **[Hooks](../06-hooks/)** - 事件驅動的自動化

## 其他資源

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode) - 內建命令參考
- [Official Skills Documentation](https://code.claude.com/docs/en/skills) - 完整的 skills 參考
- [CLI Reference](https://code.claude.com/docs/en/cli-reference) - 命令列選項

---
**最後更新日期**：2026 年 4 月 16 日
**Claude Code 版本**：2.1.110
**來源**：
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/commands
**相容模型**：Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5

*屬於 [Claude How To](../) 指南系列的一部分*
