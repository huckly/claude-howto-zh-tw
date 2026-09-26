---
name: self-assessment
version: 2.5.0
description: Comprehensive Claude Code self-assessment and learning path advisor. Runs a multi-category quiz covering 10 feature areas, produces a detailed skill profile with per-topic scores, identifies specific gaps, and generates a personalized learning path with prioritized next steps. Use when asked to "assess my level", "take the quiz", "find my level", "where should I start", "what should I learn next", "check my skills", "skill check", or "level up".
---

# 自我評估與學習路徑顧問

全面的互動式評估，衡量 10 個功能領域的 Claude Code 熟練度、找出具體的技能缺口，並產生個人化的學習路徑來提升能力。

## 說明

### Step 1：歡迎並選擇評估模式

讓使用者選擇評估深度：

使用 AskUserQuestion 並提供以下選項：
- **快速評估** — 「8 題，約 2 分鐘。判斷你的整體等級（初學者/中階/進階）並提供學習路徑。」
- **深度評估** — 「5 個類別的詳細題目，約 5 分鐘。提供各主題的技能分數、找出具體缺口，並建立優先排序的學習路徑。」

若使用者選擇**快速評估**，前往 Step 2A。
若使用者選擇**深度評估**，前往 Step 2B。

---

### Step 2A：快速評估

呈現兩個多選題（AskUserQuestion 每題最多 4 個選項）：

**問題 1**（標題："Basics"）：
「第 1/2 部分：以下哪些 Claude Code 技能你已經具備？」
選項：
1. 「啟動 Claude Code 並對話」— 我能執行 `claude` 並與它互動
2. 「建立/編輯過 CLAUDE.md」— 我設定過專案或使用者記憶
3. 「用過 3 個以上 slash 指令」— 例如 /help、/compact、/model、/clear
4. 「建立過自訂指令/skill」— 寫過 SKILL.md 或自訂指令檔

**問題 2**（標題："Advanced"）：
「第 2/2 部分：以下哪些進階技能你已經具備？」
選項：
1. 「設定過 MCP 伺服器」— 例如 GitHub、資料庫或其他外部資料來源
2. 「設定過 hooks」— 在 ~/.claude/settings.json 中設定過 hooks
3. 「建立/使用過子代理」— 用過 .claude/agents/ 進行任務委派
4. 「用過 print 模式（claude -p）」— 用過 `claude -p` 進行非互動或 CI/CD 用途

**計分：**
- 總計 0-2 = Level 1：初學者
- 總計 3-5 = Level 2：中階
- 總計 6-8 = Level 3：進階

帶著等級結果前往 Step 3，並將未勾選的具體項目列為缺口。

---

### Step 2B：深度評估

呈現 5 輪題目，每輪一次 AskUserQuestion 呼叫。每輪涵蓋 2 個相關的功能領域。所有輪次都使用多選。

**重要**：AskUserQuestion 每題最多 4 個選項。每輪恰好 1 題、4 個選項，涵蓋 2 個主題（每個主題 2 個選項）。

---

**第 1 輪 — Slash Commands 與 Memory**（標題："Commands"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「建立過自訂 slash 指令或 skill」— 寫過帶 frontmatter 的 SKILL.md 檔，或建立過 .claude/commands/ 檔案
2. 「在指令中使用過動態情境」— 在 skill/指令檔中用過 `$ARGUMENTS`、`$0`/`$1`、反引號 `!command` 語法，或 `@file` 引用
3. 「設定過專案 + 個人記憶」— 同時建立了專案 CLAUDE.md 與個人 ~/.claude/CLAUDE.md（或 CLAUDE.local.md）
4. 「使用過記憶階層功能」— 理解 7 個記憶位置如何串接進情境（由根目錄往下載入、每一層的 CLAUDE.local.md 附加在 CLAUDE.md 之後、子目錄檔案按需載入）、用過 .claude/rules/ 目錄、路徑專屬規則，或 @import 語法

**第 1 輪計分：**
- 選項 1-2 對應 **Slash Commands**（0-2 分）
- 選項 3-4 對應 **Memory**（0-2 分）

---

**第 2 輪 — Skills 與 Hooks**（標題："Automation"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「安裝並使用過自動觸發的 skill」— 一個根據 description 自動觸發、無需手動 /command 呼叫的 skill
2. 「控制過 skill 的觸發行為」— 在 SKILL.md frontmatter 中用過 `disable-model-invocation`、`user-invocable`，或帶 agent 欄位的 `context: fork`
3. 「設定過 PreToolUse 或 PostToolUse hook」— 設定過在工具執行前/後執行的 hook（例如指令驗證器、自動格式化）
4. 「使用過進階 hook 功能」— 設定過 prompt 型 hook、SKILL.md 中的元件範圍 hook、HTTP hook，或帶自訂 JSON 輸出（updatedInput、systemMessage）的 hook

**第 2 輪計分：**
- 選項 1-2 對應 **Skills**（0-2 分）
- 選項 3-4 對應 **Hooks**（0-2 分）

---

**第 3 輪 — MCP 與 Subagents**（標題："Integration"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「連接過 MCP 伺服器並使用其工具」— 例如用 GitHub MCP 處理 PR/issue、用資料庫 MCP 查詢，或任何外部資料來源
2. 「使用過進階 MCP 功能」— 專案範圍 .mcp.json、OAuth 驗證、以 @提及 的 MCP 資源、Tool Search，或 `claude mcp serve`
3. 「建立或設定過自訂子代理」— 在 .claude/agents/ 中定義帶自訂 tools、model 或權限的代理
4. 「使用過進階子代理功能」— Worktree 隔離、持久化代理記憶、以 Ctrl+B 執行背景任務、以 `Agent(agent_type)` 設定代理允許清單（`Task(...)` 形式是為了向後相容而保留的別名），或代理團隊

**第 3 輪計分：**
- 選項 1-2 對應 **MCP**（0-2 分）
- 選項 3-4 對應 **Subagents**（0-2 分）

---

**第 4 輪 — Checkpoints 與 Advanced Features**（標題："Power User"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「用 checkpoints 做安全實驗」— 建立過 checkpoint、用過 Esc+Esc 或 /rewind、還原過程式碼與/或對話，或用過任一種 Summarize 選項（summarize from here / summarize up to here）
2. 「使用過規劃模式或延伸思考」— 透過 /plan、Shift+Tab 或 --permission-mode plan 啟用規劃；用 Alt+T/Option+T 切換延伸思考
3. 「設定過權限模式」— 透過 CLI 旗標、鍵盤快捷鍵或設定使用過六種模式中的任一種 — manual（於 v2.1.200 由 default 更名）、acceptEdits、plan、auto、dontAsk 或 bypassPermissions
4. 「使用過遠端/桌面/網頁功能」— 用過 `claude --remote-control`、`claude --cloud`、`/teleport`、`/desktop`，或以 `claude -w` 使用 worktrees

**第 4 輪計分：**
- 選項 1 對應 **Checkpoints**（0-1 分）
- 選項 2-4 對應 **Advanced Features**（0-3 分）

---

**第 5 輪 — Plugins 與 CLI**（標題："Mastery"）

「以下哪些你做過？請勾選所有符合的項目。」
選項：
1. 「安裝或建立過 plugin」— 用過市集捆綁的 plugin，或建立過含 plugin.json 資訊清單的 .claude-plugin/ 目錄
2. 「使用過 plugin 進階功能」— Plugin skills（`skills/` — 建議的形式；命名空間的 `commands/` 屬舊式做法但仍可運作）、plugin hooks、plugin MCP 伺服器、LSP 設定，或用於測試的 --plugin-dir 旗標
3. 「在腳本或 CI/CD 中使用過 print 模式」— 用過 `claude -p` 搭配 --output-format json、--max-turns、管線輸入，或整合進 GitHub Actions / CI 流程
4. 「使用過進階 CLI 功能」— 工作階段續接（-c/-r）、--agents 旗標、供結構化輸出的 --json-schema、--fallback-model、--from-pr，或批次處理迴圈

**第 5 輪計分：**
- 選項 1-2 對應 **Plugins**（0-2 分）
- 選項 3-4 對應 **CLI**（0-2 分）

---

### Step 3：計算並呈現結果

#### 3A：快速評估

計算勾選總數並判斷等級。接著呈現：

```markdown
## Claude Code Skill Assessment Results

### Your Level: [Level 1: Beginner / Level 2: Intermediate / Level 3: Advanced]

You checked **N/8** items.

[One-line motivational summary based on level]

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

[For each unchecked item, provide a 1-line description of what to learn and a link to the tutorial]

### Your Personalized Learning Path

[Output the level-specific learning path — see Step 4]
```

#### 3B：深度評估

根據 5 輪結果計算各主題分數。每個主題 0-2 分，但 Advanced Features（0-3）與 Checkpoints（0-1）除外。接著呈現：

```markdown
## Claude Code Skill Assessment Results

### Overall Level: [Level 1 / Level 2 / Level 3]

**Total Score: N/20 points**

[One-line motivational summary]

### Your Skill Profile

| Feature Area | Score | Mastery | Status |
|-------------|-------|---------|--------|
| Slash Commands | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Memory | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Skills | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Hooks | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| MCP | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Subagents | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Checkpoints | N/1 | [None/Proficient] | [Learn/Mastered] |
| Advanced Features | N/3 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| Plugins | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |
| CLI | N/2 | [None/Basic/Proficient] | [Learn/Review/Mastered] |

**Mastery key:** 0 = None, 1 = Basic, 2 = Proficient

### Strength Areas
[List topics with score 2/2 — these are mastered]

### Priority Gaps (Learn Next)
[List topics with score 0 — these need attention first, ordered by dependency]

### Review Areas
[List topics with score 1/2 — basics known but advanced features not yet used]

### Your Personalized Learning Path

[Output gap-specific learning path — see Step 4]
```

**深度評估的整體等級計算：**
- 總分 0-6 = Level 1：初學者
- 總分 7-13 = Level 2：中階
- 總分 14-20 = Level 3：進階

---

### Step 4：產生個人化學習路徑

根據評估結果，產生針對使用者缺口的學習路徑。**不要**只是重複通用的等級路徑 — 要依情況調整。

#### 路徑產生規則

1. **略過已精通的主題**：若某主題得分 2/2，不要納入路徑。
2. **依相依順序排定優先**：Slash Commands 先於 Skills、Memory 先於 Subagents，依此類推。相依順序如下：
   - Slash Commands（無相依）-> Skills（相依於 Slash Commands）
   - Memory（無相依）-> Subagents（相依於 Memory）
   - CLI 基礎（無相依）-> CLI 精通（相依於全部）
   - Checkpoints（無相依）
   - Hooks（相依於 Slash Commands）
   - MCP（無相依）-> Plugins（相依於 MCP、Skills、Hooks）
   - Advanced Features（相依於前面所有主題）
3. **得分 1/2 的主題**：建議「深入探討」— 連結到他們尚未掌握的具體進階章節。
4. **估算時間**：只加總他們需要學習/複習的主題。
5. **分組成階段**：將剩餘主題組織成合理的階段，每階段 2-3 個主題。

#### 路徑輸出格式

```markdown
### Your Personalized Learning Path

**Estimated time**: ~N hours (adjusted for your current skills)

#### Phase 1: [Phase Name] (~N hours)
[Only if they have gaps in these areas]

**[Topic Name]** — [Learn from scratch / Deep dive into advanced features]
- Tutorial: [link to tutorial directory]
- Focus on: [specific sections/concepts they need]
- Key exercise: [one concrete exercise to do]
- You'll know it's done when: [specific success criterion]

**[Topic Name]** — ...

---

#### Phase 2: [Phase Name] (~N hours)
...

---

### Recommended Practice Projects

Based on your gaps, try these real-world exercises to solidify your learning:

1. **[Project name]**: [1-line description combining 2-3 gap topics]
2. **[Project name]**: [1-line description]
3. **[Project name]**: [1-line description]
```

#### 主題專屬建議

當某個主題是缺口時，使用以下的具體建議：

**Slash Commands（分數 0）**：
- 教學：[01-slash-commands/](../../../01-slash-commands/)
- 重點：內建指令參考、建立你的第一個 SKILL.md、`$ARGUMENTS` 語法
- 關鍵練習：建立一個 `/optimize` 指令並測試它
- 完成標準：你能建立一個帶有參數與動態情境的自訂 skill

**Slash Commands（分數 1 — 複習）**：
- 重點：以 `!`反引號`` 語法帶入動態情境、`@file` 引用、`disable-model-invocation` 與 `user-invocable` 的控制差異
- 完成標準：你能建立一個會注入即時指令輸出、並自行控制觸發行為的 skill

**Memory（分數 0）**：
- 教學：[02-memory/](../../../02-memory/)
- 重點：建立 CLAUDE.md、`/init` 與 `/memory` 指令、以 `#` 前綴快速更新
- 關鍵練習：建立一個包含你程式碼規範的專案 CLAUDE.md
- 完成標準：Claude 能跨工作階段記住你的偏好

**Memory（分數 1 — 複習）**：
- 重點：7 個記憶位置，以及它們如何串接進情境（由根目錄往下載入，而非互相覆蓋）、含路徑專屬規則的 .claude/rules/ 目錄、`@import` 語法（最大深度 4）、Auto Memory MEMORY.md（Claude 會載入其前 200 行或前 25 KB，以先達到者為準 — 這是載入上限，而非檔案大小限制）
- 完成標準：你為不同目錄建立了模組化規則，並理解每個記憶檔案如何串接進情境

**Skills（分數 0）**：
- 教學：[03-skills/](../../../03-skills/)
- 重點：SKILL.md 格式、透過 description 欄位自動觸發、漸進式揭露（3 個載入層級）
- 關鍵練習：安裝 code-review skill 並確認它會自動觸發
- 完成標準：某個 skill 能根據對話情境自動啟用

**Skills（分數 1 — 複習）**：
- 重點：以 `context: fork` 搭配 `agent` 欄位在子代理中執行、`disable-model-invocation` 與 `user-invocable`、skill 清單預算（情境視窗的 1%，備用值 8,000 字元，每個項目 250 字元）、隨附資源（scripts/、references/、assets/）
- 完成標準：你能建立一個在子代理中以 forked 情境執行的 skill

**Hooks（分數 0）**：
- 教學：[06-hooks/](../../../06-hooks/)
- 重點：設定結構（matcher + hooks 陣列）、PreToolUse/PostToolUse 事件、離開碼（0=成功、2=阻擋）、JSON 輸入/輸出格式
- 關鍵練習：建立一個驗證 Bash 指令的 PreToolUse hook
- 完成標準：某個 hook 能在執行前阻擋危險指令

**Hooks（分數 1 — 複習）**：
- 重點：全部 33 種 hook 事件（含 PostToolUseFailure、StopFailure、TaskCreated、CwdChanged、FileChanged、PostCompact、Elicitation、ElicitationResult、Setup、UserPromptExpansion、MessageDisplay、PreModelSwitch、PostModelSwitch — 最後兩個於 v2.1.251 新增）、5 種 hook 類型（command、http、mcp_tool、prompt、agent — agent hooks 仍屬實驗性質，未來可能變動）、SKILL.md frontmatter 中的元件範圍 hook、帶 allowedEnvVars 的 HTTP hook、供 SessionStart/CwdChanged/FileChanged 使用的 `CLAUDE_ENV_FILE`
- 完成標準：你能建立一個 prompt 型的 Stop hook，以及一個 skill 內的元件範圍 hook

**MCP（分數 0）**：
- 教學：[05-mcp/](../../../05-mcp/)
- 重點：`claude mcp add` 指令、傳輸類型（建議用 `http`，另有 `stdio`、適用於推送型伺服器的 `ws`，以及已棄用的 `sse` — 注意 `--transport` 不接受 `ws`，因此 WebSocket 伺服器要用 `claude mcp add-json` 新增）、GitHub MCP 設定、環境變數展開
- 關鍵練習：新增 GitHub MCP 伺服器並查詢 PR
- 完成標準：你能透過 MCP 從外部服務查詢即時資料

**MCP（分數 1 — 複習）**：
- 重點：專案範圍 .mcp.json（需團隊核准）、OAuth 2.0 驗證、以 `@server:resource` 提及的 MCP 資源、Tool Search（ENABLE_TOOL_SEARCH）、`claude mcp serve`、輸出上限（10,000 tokens 時警告；透過 `MAX_MCP_OUTPUT_TOKENS` 設定的預設上限 25,000 tokens；50,000 字元的磁碟持久化門檻）
- 完成標準：你有一份專案 .mcp.json，並理解 Tool Search 的 auto 模式

**Subagents（分數 0）**：
- 教學：[04-subagents/](../../../04-subagents/)
- 重點：Agent 檔案格式（.claude/agents/*.md）、內建代理（Explore、Plan、general-purpose、claude、statusline-setup、claude-code-guide）、tools/model/permissionMode 設定、產生數量限制（自 v2.1.219 起預設深度為 3、預設並行數為 20，每個工作階段 200 個的產生上限已於 v2.1.224 移除）
- 關鍵練習：建立一個 code-reviewer 子代理並測試委派
- 完成標準：Claude 會把程式碼審查委派給你的自訂代理

**Subagents（分數 1 — 複習）**：
- 重點：Worktree 隔離（`isolation: worktree`）、持久化代理記憶（帶範圍的 `memory` 欄位）、背景代理（Ctrl+B/Ctrl+F）、以 `Agent(agent_type)` 設定代理允許清單（`Task(...)` 仍保留為向後相容的別名）、代理團隊（`--teammate-mode`）
- 完成標準：你有一個在 worktree 隔離中執行、且具持久化記憶的子代理

**Checkpoints（分數 0）**：
- 教學：[08-checkpoints/](../../../08-checkpoints/)
- 重點：以 Esc+Esc 與 /rewind 存取、6 種 rewind 選項（還原程式碼與對話、還原對話、還原程式碼、從此處摘要、摘要至此處、取消）、限制 — bash 檔案系統操作、子代理的編輯（前景執行的 `context: fork` skill 除外）、在 Claude Code 之外所做的編輯，以及符號連結/硬連結的路徑，都不會被追蹤
- 關鍵練習：做一些實驗性變更，再 rewind 還原
- 完成標準：你能放心實驗，因為知道隨時可以 rewind

**Advanced Features（分數 0）**：
- 教學：[09-advanced-features/](../../../09-advanced-features/)
- 重點：規劃模式（/plan 或 Shift+Tab）、權限模式（6 種：manual — 於 v2.1.200 由 default 更名 — acceptEdits、plan、auto、dontAsk、bypassPermissions）、延伸思考（Alt+T 切換）
- 關鍵練習：用規劃模式設計一個功能，再實作它
- 完成標準：你能在規劃與實作模式間流暢切換

**Advanced Features（分數 1 — 複習）**：
- 重點：遠端控制（`claude --remote-control`，別名 `--rc`）、網頁工作階段（`claude --cloud`；`--remote` 是已棄用的別名）、桌面交接（`/desktop`）、worktrees（`claude -w`）、任務清單（Ctrl+T）、企業用託管設定
- 完成標準：你能在 CLI、網頁與桌面之間交接工作階段

**Plugins（分數 0）**：
- 教學：[07-plugins/](../../../07-plugins/)
- 重點：Plugin 結構（.claude-plugin/plugin.json）、plugin 能捆綁什麼（skills、agents、MCP、hooks、settings — 另有舊式的 `commands/` 目錄，仍可運作，但新的 plugins 建議使用 `skills/`）、從市集安裝
- 關鍵練習：安裝一個 plugin 並探索它的元件
- 完成標準：你理解何時該用 plugin、何時該用獨立元件

**Plugins（分數 1 — 複習）**：
- 重點：建立 plugin.json 資訊清單、plugin hooks（hooks/hooks.json）、LSP 設定（.lsp.json）、`${CLAUDE_PLUGIN_ROOT}` 變數、以 --plugin-dir 測試、發佈到市集
- 完成標準：你能為團隊建立並測試一個 plugin

**CLI（分數 0）**：
- 教學：[10-cli/](../../../10-cli/)
- 重點：互動模式 vs print 模式、`claude -p` 搭配管線、`--output-format json`、工作階段管理（-c/-r）
- 關鍵練習：把一個檔案管線傳給 `claude -p` 並取得 JSON 輸出
- 完成標準：你能在腳本中非互動地使用 Claude

**CLI（分數 1 — 複習）**：
- 重點：帶 JSON 設定的 --agents 旗標、供結構化輸出的 --json-schema、--fallback-model、--from-pr、--strict-mcp-config、以 for 迴圈批次處理、`claude mcp serve`
- 完成標準：你有一個使用 Claude 並產出結構化 JSON 輸出的 CI/CD 腳本

---

### Step 5：提供後續行動

呈現結果後，詢問使用者接下來想做什麼：

使用 AskUserQuestion 並提供以下選項：
- **開始學習** — 「現在就帶我開始學習路徑中的第一個主題」
- **深入探討某個缺口** — 「詳細解說我的其中一個缺口領域，讓我在這裡學會它」
- **實作專案** — 「設定一個涵蓋我缺口領域的實作專案」
- **重新評估** — 「我想重新做一次測驗（也許換另一種模式）」

若選擇**開始學習**：閱讀第一個缺口教學的 README.md，並帶使用者完成第一個練習。
若選擇**深入探討某個缺口**：詢問是哪個缺口主題，然後閱讀相關教學的 README.md，並搭配範例解說關鍵概念。
若選擇**實作專案**：設計一個結合 2-3 個缺口主題的小型專案，並附上具體步驟。
若選擇**重新評估**：回到 Step 1。

## 錯誤處理

### 使用者在某一輪沒有勾選任何項目
該輪的主題以 0 分計。繼續下一輪。

### 使用者在所有輪次都沒有勾選任何項目
判定為 Level 1：初學者。鼓勵從頭開始，並輸出完整的 Level 1 路徑。

### 使用者想重新評估
從 Step 1 重新執行一次全新的評估。

### 使用者不同意自己的等級
尊重他們的看法。詢問他們認為自己屬於哪個等級。呈現他們所選等級的路徑，並針對可能遺漏的主題附上先備知識檢查。

### 使用者詢問特定主題
若使用者在評估過程中說出類似「跟我說說 hooks」或「我想學 MCP」，記下來。呈現結果後，不論分數如何，都在學習路徑中突顯該主題。

## 驗證

### 觸發測試套件

**應觸發：**
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

**不應觸發：**
- "review my code"
- "create a skill"
- "help me with MCP"
- "explain slash commands"
- "what is a checkpoint"
