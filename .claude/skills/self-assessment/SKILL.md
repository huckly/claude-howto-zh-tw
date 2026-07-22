---
name: self-assessment
version: 2.3.0
description: 全面的 Claude Code 自我評估與學習路徑顧問。執行涵蓋 10 個功能領域的多類別測驗，產出包含各主題分數的詳細技能概況，識別特定的技能差距，並生成具有優先順序的個人化學習路徑。當被要求「評估我的程度」、「進行測驗」、「找出我的等級」、「我該從哪裡開始」、「接下來該學什麼」、「檢查我的技能」、「技能檢查」或「提升等級」時使用。
---

# 自我評估與學習路徑顧問

全面的互動式評估，用於評估 Claude Code 在 10 個功能領域的熟練程度，識別特定的技能差距，並生成個人化的學習路徑以提升等級。

## 指令

### Step 1: 歡迎與選擇評估模式

向使用者提供評估深度的選擇：

使用 AskUserQuestion 並提供以下選項：
- **快速評估** — 「8 個問題，約 2 分鐘。確定您的整體等級（初學者/中級/進階）並提供學習路徑。」
- **深度評估** — 「5 個類別與詳細問題，約 5 分鐘。提供各主題的技能分數，識別特定的差距，並建立具優先順序的學習路徑。」

如果使用者選擇 **快速評估**，前往 Step 2A。
如果使用者選擇 **深度評估**，前往 Step 2B。

---

### Step 2A: 快速評估

提供兩個多選題（AskUserQuestion 每個問題最多支援 4 個選項）：

**問題 1** (標題: "Basics"):
「第 1/2 部分：您已經掌握了哪些 Claude Code 技能？」
選項：
1. 「啟動 Claude Code 並進行對話」 — 我可以執行 `claude` 並與其互動
2. 「建立/編輯 CLAUDE.md」 — 我已經設定了專案或使用者記憶
3. 「使用 3 個以上的斜線命令」 — 例如 /help, /compact, /model, /clear
4. 「建立自定義命令/技能」 — 已撰寫 SKILL.md 或自定義命令檔案

**問題 2** (標題: "Advanced"):
「第 2/2 部分：您具備哪些進階技能？」
選項：
1. 「配置 MCP 伺服器」 — 例如 GitHub、資料庫或其他外部資料來源
2. 「設定鉤子」 — 在 ~/.claude/settings.json 中配置了 hooks
3. 「建立/使用子代理」 — 使用 .claude/agents/ 進行任務委派
4. 「使用列印模式 (claude -p)」 — 使用 `claude -p` 進行非互動式或 CI/CD 使用

**計分：**
- 總分 0-2 = 等級 1：初學者
- 總分 3-5 = 等級 2：中級
- 總分 6-8 = 等級 3：進階

帶著等級結果前往 Step 3，並列出哪些未被勾選的項目作為技能差距。

---

### Step 2B: 深度評估

提供 5 輪問題，每輪呼叫一次 AskUserQuestion。每輪涵蓋 2 個相關的功能領域。所有輪次均使用多選模式。

**重要提示**：AskUserQuestion 每個問題最多支援 4 個選項。每輪剛好包含 1 個問題，具有 4 個選項，涵蓋 2 個主題（每個主題 2 個選項）。

---

**第 1 輪 — 斜線命令與記憶** (標題: "Commands")

「您執行過哪些操作？請選擇所有適用選項。」
選項：
1. 「建立自定義斜線命令或技能」 — 已撰寫包含 frontmatter 的 SKILL.md 檔案，或建立了 .claude/commands/ 檔案

2. 「在指令中使用動態上下文」— 使用了 `$ARGUMENTS`、`$0`/`$1`、反引號 `!command` 語法，或在 skill/command 檔案中使用 `@file` 引用
3. 「設定專案 + 個人記憶」— 同時建立了專案級別的 CLAUDE.md 與個人級別的 `~/.claude/CLAUDE.md`（或 CLAUDE.local.md）
4. 「使用記憶層級功能」— 理解 7 層優先順序，使用了 `.claude/rules/` 目錄、路徑特定規則或 `@import` 語法

**第一輪評分：**
- 選項 1-2 對應 **斜線命令** (0-2 分)
- 選項 3-4 對應 **記憶** (0-2 分)

---

**第二輪 — 技能與鉤子** (標題: "Automation")

「您執行過下列哪些操作？請選擇所有適用的選項。」
選項：
1. 「安裝並使用自動觸發的技能」— 根據其描述自動觸發的技能，無需手動輸入 `/command` 進行呼叫
2. 「控制技能呼叫行為」— 使用了 `disable-model-invocation`、`user-invocable`，或在 SKILL.md frontmatter 中使用帶有 agent 欄位的 `context: fork`
3. 「設定 PreToolUse 或 PostToolUse 鉤子」— 配置在工具執行前/後運行的鉤子（例如：指令驗證器、自動格式化工具）
4. 「使用進階鉤子功能」— 配置了 prompt-type 鉤子、SKILL.md 中的組件範圍鉤子、HTTP 鉤子，或帶有自定義 JSON 輸出（updatedInput, systemMessage）的鉤子

**第二輪評分：**
- 選項 1-2 對應 **技能** (0-2 分)
- 選項 3-4 對應 **鉤子** (0-2 分)

---

**第三輪 — MCP 與子代理** (標題: "Integration")

「您執行過下列哪些操作？請選擇所有適用的選項。」
選項：
1. 「連接 MCP 伺服器並使用其工具」— 例如：用於 PR/issue 的 GitHub MCP、用於查詢的資料庫 MCP，或任何外部資料來源
2. 「使用進階 MCP 功能」— 專案範圍的 `.mcp.json`、OAuth 驗證、帶有 `@mentions` 的 MCP 資源、工具搜尋 (Tool Search) 或 `claude mcp serve`
3. 「建立或配置自定義子代理」— 在 `.claude/agents/` 中定義具有自定義工具、模型或權限的 agent
4. 「使用進階子代理功能」— Worktree 隔離、持久化 agent 記憶、使用 Ctrl+B 的背景任務、帶有 `Task(agent_name)` 的 agent 白名單，或 agent 團隊

**第三輪評分：**
- 選項 1-2 對應 **MCP** (0-2 分)
- 選項 3-4 對應 **子代理** (0-2 分)

---

**第四輪 — 檢查點與進階功能** (標題: "Power User")

「您執行過下列哪些操作？請選擇所有適用的選項。」
選項：
1. 「使用檢查點進行安全實驗」— 建立檢查點、使用 Esc+Esc 或 `/rewind`、還原程式碼及/或對話，或使用 Summarize 選項
2. 「使用規劃模式或延伸思考」— 透過 `/plan`、Shift+Tab 或 `--permission-mode plan` 啟動規劃；透過 Alt+T/Option+T 切換延伸思考
3. 「配置權限模式」— 透過 CLI 旗標、鍵盤快捷鍵或設定使用 `acceptEdits`、`plan`、`dontAsk` 或 `bypassPermissions` 模式
4. 「使用遠端/桌面/網頁功能」— 使用 `claude remote-control`、`claude --remote`、`/teleport`、`/desktop` 或使用 `claude -w` 的 worktrees

**第四輪評分：**
- 選項 1 對應 **檢查點** (0-1 分)

- 選項 2-4 對應至 **Advanced Features**（0-3 分，上限為 2 分）

---

**Round 5 — Plugins & CLI**（標題："Mastery"）

「您曾執行過下列哪些項目？請勾選所有適用選項。」
選項：
1. "Installed or created a plugin" — 使用來自 marketplace 的內建外掛，或建立包含 plugin.json 清單的 .claude-plugin/ 目錄
2. "Used plugin advanced features" — 使用 plugin hooks、plugin MCP servers、LSP configuration、plugin namespaced commands，或用於測試的 --plugin-dir 旗標
3. "Used print mode in scripts or CI/CD" — 使用 `claude -p` 搭配 --output-format json、--max-turns、piped input，或整合至 GitHub Actions / CI pipelines
4. "Used advanced CLI features" — 使用 Session resumption (-c/-r)、--agents 旗標、用於結構化輸出的 --json-schema、--fallback-model、--from-pr，或批次處理迴圈

**Round 5 評分方式：**
- 選項 1-2 對應至 **Plugins**（0-2 分）
- 選項 3-4 對應至 **CLI**（0-2 分）

---

### Step 3: Calculate & Present Results

#### 3A: For Quick Assessment

計算總勾選數並判定等級。接著呈現：

```markdown
## Claude Code Skill Assessment Results

### Your Level: [Level 1: Beginner / Level 2: Intermediate / Level 3: Advanced]

您勾選了 **N/8** 個項目。

[根據等級提供的一行式勵志總結]

### Your Skill Profile

| Area | Status |
|------|--------|
| Basic CLI & Conversations | [Checked/Gap] |
| CLAUDE.md & Memory | [Checked/Gap] |
| Slash Commands (built-in) | [Checked/Gap] |
| Custom Commands & Skills | [Checked/Gap] |
| MCP Servers | [Checked/Gap] |
| Hooks | [Checked/Gap] |
| Subagents | [Checked/Gap] |
| Print Mode & CI/CD | [Checked/Gap] |

### Identified Gaps

[針對每個未勾選的項目，提供一行關於學習內容的描述以及教學連結]

### Your Personalized Learning Path

[輸出特定等級的學習路徑 — 請參閱 Step 4]
```

#### 3B: For Deep Assessment

根據 5 個輪次的結果計算每個主題的分數。每個主題獲得 0-2 分。接著呈現：

```markdown

## Claude Code 技能評估結果

### 整體等級：[Level 1 / Level 2 / Level 3]

**總分：N/20 分**

[一句話激勵性總結]

### 您的技能概況

| 功能領域 | 分數 | 精通程度 | 狀態 |
|-------------|-------|---------|--------|
| 斜線命令 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 記憶 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 技能 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 鉤子 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| MCP | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 子代理 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 檢查點 | N/1 | [None/Proficient] | [Learn/Mastered] |
| 進階功能 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| 外掛 | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| CLI | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |

**精通程度說明：** 0 = None, 1 = Basic, 2 = Proficient

### 優勢領域
[列出分數為 2/2 的主題 — 這些代表已掌握]

### 優先差距 (下一步學習)
[列出分數為 0 的主題 — 這些需要優先關注，依據依賴關係排序]

### 複習領域
[列出分數為 1/2 的主題 — 已掌握基礎但尚未使用進階功能]

### 您的個人化學習路徑

[輸出針對特定差距的學習路徑 — 請參閱步驟 4]
```

**深度評估的整體等級計算方式：**
- 總分 0-6 分 = Level 1: 初學者
- 總分 7-13 分 = Level 2: 中級
- 總分 14-20 分 = Level 3: 進階

---

### 步驟 4：生成個人化學習路徑

根據評估結果，生成針對使用者技能差距的特定學習路徑。請勿只是重複通用的等級路徑 — 請進行調整。

#### 路徑生成規則

1. **跳過已掌握的主題**：如果主題分數為 2/2，請不要將其包含在路徑中。
2. **依據依賴順序排列優先權**：斜線命令在前於技能，記憶在前於子代理，依此類推。依賴順序如下：
   - 斜線命令 (無依賴) -> 技能 (依賴斜線命令)
   - 記憶 (無依賴) -> 子代理 (依賴記憶)
   - CLI 基礎 (無依賴) -> CLI 精通 (依賴所有項目)
   - 檢查點 (無依賴)
   - 鉤子 (依賴斜線命令)
   - MCP (無依賴) -> 外掛 (依賴 MCP、技能、鉤子)
   - 進階功能 (依賴之前所有項目)
3. **針對分數為 1/2 的主題**：建議進行「深度探索」— 連結到他們缺失的特定進階章節。
4. **估算時間**：僅加總他們需要學習/複習的主題。
5. **分階段分組**：將剩餘主題組織成邏輯性的階段，每個階段包含 2-3 個主題。

#### 路徑輸出格式

```markdown
### 您的個人化學習路徑

**預估時間**：~N 小時 (根據您目前的技能進行調整)

#### 第一階段：[階段名稱] (~N 小時)
[僅在這些領域存在差距時顯示]

**[主題名稱]** — [從零開始學習 / 深入探索進階功能]
- 教學：[連結至教學目錄]
- 重點：[他們需要的特定章節/概念]
```

- 關鍵練習：[一個具體的練習項目]
- 完成標準：[特定的成功準則]

**[主題名稱]** — ...

---

#### 第二階段：[階段名稱] (~N 小時)
...

---

### 推薦實作專案

根據您的知識缺口，嘗試進行這些真實世界的練習以鞏固您的學習：

1. **[專案名稱]**：[結合 2-3 個缺口主題的單行描述]
2. **[專案名稱]**：[單行描述]
3. **[專案名稱]**：[單行描述]

#### 特定主題建議

當某個主題屬於知識缺口時，請使用這些特定建議：

**斜線命令 (分數 0)**：
- 教學：[01-slash-commands/](../../../01-slash-commands/)
- 重點：內建命令參考、建立您的第一個 SKILL.md、`$ARGUMENTS` 語法
- 關鍵練習：建立一個 `/optimize` 命令並進行測試
- 完成標準：您可以建立一個帶有參數與動態上下文的自定義技能

**斜線命令 (分數 1 — 複習)**：
- 重點：使用 `!` 語法的動態上下文、`@file` 引用、`disable-model-invocation` 與 `user-invocable` 控制
- 完成標準：您可以建立一個能注入即時命令輸出並控制自身調用行為的技能

**記憶 (分數 0)**：
- 教學：[02-memory/](../../../02-memory/)
- 重點：建立 CLAUDE.md、`/init` 與 `/memory` 命令、用於快速更新的 `#` 前綴
- 關鍵練習：建立一個包含您編碼標準的專案 CLAUDE.md
- 完成標準：Claude 能在不同會話之間記住您的偏好

**記憶 (分數 1 — 複習)**：
- 重點：7 層層級結構與優先順序、包含路徑特定規則的 `.claude/rules/` 目錄、`@import` 語法（最大深度 5）、自動記憶 MEMORY.md（200 行限制）
- 完成標準：您已為不同目錄建立模組化規則並理解完整的層級結構

**技能 (分數 0)**：
- 教學：[03-skills/](../../../03-skills/)
- 重點：SKILL.md 格式、透過描述欄位進行自動調用、漸進式揭露（3 個載入層級）
- 關鍵練習：安裝 code-review 技能並驗證其是否自動觸發
- 完成標準：技能能根據對話上下文自動啟動

**技能 (分數 1 — 複習)**：
- 重點：帶有 `agent` 欄位的 `context: fork` 用於子代理執行、`disable-model-invocation` 與 `user-invocable`、2% 上下文預算、綑綁資源 (scripts/, references/, assets/)
- 完成標準：您可以建立一個在具有分叉上下文的子代理中運行的技能

**鉤子 (分數 0)**：
- 教學：[06-hooks/](../../../06-hooks/)
- 重點：配置結構 (matcher + hooks 陣列)、PreToolUse/PostToolUse 事件、結束碼 (0=成功, 2=阻斷)、JSON 輸入/輸出格式
- 關鍵練習：建立一個用於驗證 Bash 命令的 PreToolUse 鉤子
- 完成標準：鉤子能在執行前阻斷危險命令

**鉤子 (分數 1 — 複習)**：
- 重點：所有 25 個鉤子事件（包括 PostToolUseFailure, StopFailure, TaskCreated, CwdChanged, FileChanged, PostCompact, Elicitation, ElicitationResult）、4 種鉤子類型 (command, http, prompt, agent)、SKILL.md frontmatter 中的組件範圍鉤子、帶有 allowedEnvVars 的 HTTP 鉤子、用於 SessionStart/CwdChanged/FileChanged 的 `CLAUDE_ENV_FILE`

- 完成標準：你可以在一個 skill 中建立基於 prompt 的 Stop hook 以及組件範圍的 hook

**MCP (score 0)**:
- 教學：[05-mcp/](../../../05-mcp/)
- 重點：`claude mcp add` 指令、傳輸類型（建議使用 HTTP）、GitHub MCP 設定、環境變數展開
- 關鍵練習：新增 GitHub MCP server 並查詢 PR
- 完成標準：你可以透過 MCP 從外部服務查詢即時數據

**MCP (score 1 — review)**:
- 重點：專案範圍的 .mcp.json（需要團隊審核）、OAuth 2.0 驗證、包含 `@server:resource` 提及的 MCP resources、Tool Search (ENABLE_TOOL_SEARCH)、`claude mcp serve`、輸出限制 (10k/25k/50k)
- 完成標準：你擁有一個專案 .mcp.json 並理解 Tool Search 自動模式

**Subagents (score 0)**:
- 教學：[04-subagents/](../../../04-subagents/)
- 重點：Agent 檔案格式 (.claude/agents/*.md)、內建 agents (general-purpose, Plan, Explore)、tools/model/permissionMode 設定
- 關鍵練習：建立一個 code-reviewer 子代理並測試委派
- 完成標準：Claude 將程式碼審查委派給你的自定義 agent

**Subagents (score 1 — review)**:
- 重點：Worktree 隔離 (`isolation: worktree`)、持久化 agent 記憶 (`memory` 欄位與範圍)、背景 agents (Ctrl+B/Ctrl+F)、使用 `Task(agent_name)` 的 agent 白名單、agent 團隊 (`--teammate-mode`)
- 完成標準：你擁有一個在 worktree 隔離模式下執行且具有持久化記憶的子代理

**Checkpoints (score 0)**:
- 教學：[08-checkpoints/](../../../08-checkpoints/)
- 重點：Esc+Esc 與 /rewind 存取、5 種 rewind 選項 (restore code+conversation, restore conversation, restore code, summarize, cancel)、限制 (bash 檔案系統操作不會被追蹤)
- 關鍵練習：進行實驗性修改，然後使用 rewind 進行還原
- 完成標準：你可以放心地進行實驗，因為知道可以隨時 rewind

**Advanced Features (score 0)**:
- 教學：[09-advanced-features/](../../../09-advanced-features/)
- 重點：規劃模式 (/plan 或 Shift+Tab)、權限模式 (5 種類型)、延伸思考 (Alt+T 切換)
- 關鍵練習：使用規劃模式設計一個功能，然後將其實作
- 完成標準：你可以在規劃模式與實作模式之間流暢切換

**Advanced Features (score 1 — review)**:
- 重點：遠端控制 (`claude remote-control`)、網頁會話 (`claude --remote`)、桌面接手 (`/desktop`)、worktrees (`claude -w`)、任務列表 (Ctrl+T)、企業級管理設定
- 完成標準：你可以在 CLI、網頁與桌面之間進行會話接手

**Plugins (score 0)**:
- 教學：[07-plugins/](../../../07-plugins/)
- 重點：外掛結構 (.claude-plugin/plugin.json)、外掛封裝內容 (commands, agents, MCP, hooks, settings)、從 marketplace 安裝
- 關鍵練習：安裝一個外掛並探索其組件
- 完成標準：你理解何時該使用外掛，何時該使用獨立組件

**Plugins (score 1 — review)**:
- 重點：建立 plugin.json 清單、外掛 hooks (hooks/hooks.json)、LSP 設定 (.lsp.json)、`${CLAUDE_PLUGIN_ROOT}` 變數、用於測試的 --plugin-dir、發佈至 marketplace

- 完成標準：您可以為您的團隊建立並測試一個外掛

**CLI (分數 0)**：
- 教學：[10-cli/](../../../10-cli/)
- 重點：互動模式 vs 列印模式、使用管道（piping）搭配 `claude -p`、`--output-format json`、會話管理 (-c/-r)
- 關鍵練習：將檔案透過管道傳送至 `claude -p` 並取得 JSON 輸出
- 完成標準：您可以在腳本中以非互動方式使用 Claude

**CLI (分數 1 — 審查)**：
- 重點：搭配 JSON 設定的 `--agents` 旗標、用於結構化輸出的 `--json-schema`、`--fallback-model`、`--from-pr`、`--strict-mcp-config`、使用 for 迴圈進行批次處理、`claude mcp serve`
- 完成標準：您擁有一個使用 Claude 並產生結構化 JSON 輸出的 CI/CD 腳本

---

### 第 5 步：提供後續行動

在呈現結果後，詢問使用者接下來想要做什麼：

使用 AskUserQuestion 並提供以下選項：
- **開始學習** — 「請幫我立即開始學習路徑中的第一個主題」
- **深入研究落差點** — 「詳細解釋我的一個落差領域，以便我可以在這裡學習」
- **實作專案** — 「建立一個涵蓋我落差領域的實作專案」
- **重新進行評估** — 「我想重新進行測驗（或許是另一種模式）」

如果選擇 **Start learning**：閱讀第一個落差教學的 README.md，並引導使用者完成第一個練習。
如果選擇 **Deep dive on a gap**：詢問要深入研究哪一個落差主題，然後閱讀相關教學的 README.md，並透過範例解釋關鍵概念。
如果選擇 **Practice project**：設計一個結合 2-3 個落差主題的小型專案，並提供具體的步驟。
如果選擇 **Retake assessment**：回到第 1 步。

## Error Handling

### 使用者在某一輪未選擇任何項目
將該輪主題視為 0 分。繼續進行下一輪。

### 使用者在任何一輪都未選擇任何項目
分配 Level 1: Beginner。鼓勵從頭開始。輸出完整的 Level 1 路徑。

### 使用者想要重新測試
從 Step 1 開始重新執行全新的評估。

### 使用者不同意其所屬等級
確認其偏好。詢問他們認為自己屬於哪個等級。呈現其選擇等級的路徑，並針對他們可能遺漏的主題進行前置需求檢查。

### 使用者詢問特定主題
如果使用者在評估過程中提到類似「告訴我關於 hooks 的資訊」或「我想學習 MCP」之類的話，請記錄下來。在呈現結果後，無論分數如何，都在其學習路徑中強調該主題。

## Validation

### 觸發測試套件

**應該觸發：**
- "assess my level"
- "take the quiz"
- "find my level"
- "where should I start"
- "what level am I"
- "learning path quiz"
- "self-assessment"
- "what should I learn next"
- "check my skills"
- "skill check"
- "level up"
- "how good am I at Claude Code"
- "evaluate my Claude Code knowledge"

**不應該觸發：**
- "review my code"
- "create a skill"
- "help me with MCP"
- "explain slash commands"
- "what is a checkpoint"
