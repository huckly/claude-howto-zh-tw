---
name: lesson-quiz
description: "Test a learner on a single Claude Code tutorial lesson (01-10) with 10 questions, scoring answers and flagging weak spots. Use before, during, or after a lesson. Don't use for whole-tutorial assessment or explaining a topic instead of testing it."
effort: high
metadata:
  version: 1.3.0
  author: Luong NGUYEN
---

# 課程測驗

互動式測驗，透過 8-10 道題目測試對特定 Claude Code 課程的理解，提供逐題回饋，並找出需要複習的領域。

## 先決條件

此 skill 需要：

- 已 checkout 教學儲存庫，以便讀取課程目錄 `01-slash-commands/` … `10-cli/` 及各自的 `README.md`。
- 此 skill 中存在 `references/question-bank.md`（所有題目的來源）。

開始之前，確認目標課程的 `README.md` 存在。若不存在，不要捏造題目 — 警告使用者並請其檢查儲存庫結構（見錯誤處理）。

**防護規則：**

- **絕不自行編造題目或答案。** 只使用 `references/question-bank.md` 中所選課程的題目；若題庫缺少某課程的題目，應直接說明，而非自行編造。
- **準確計分。** 為每道題記錄打亂順序後正確答案所在的位置，並據此驗證每個回答 — 絕不猜測分數。
- **計分前先確認時機。** 學前／學中／學後的選擇會改變結果的呈現方式；不可略過。

## 說明

### 步驟 1：確定課程

若使用者提供了課程作為參數（例如 `/lesson-quiz hooks` 或 `/lesson-quiz 03`），將其對應到課程目錄：

**課程對應表：**
- `01`, `slash-commands`, `commands` → 01-slash-commands
- `02`, `memory` → 02-memory
- `03`, `skills` → 03-skills
- `04`, `subagents`, `agents` → 04-subagents
- `05`, `mcp` → 05-mcp
- `06`, `hooks` → 06-hooks
- `07`, `plugins` → 07-plugins
- `08`, `checkpoints`, `checkpoint` → 08-checkpoints
- `09`, `advanced`, `advanced-features` → 09-advanced-features
- `10`, `cli` → 10-cli

若未提供參數，使用 AskUserQuestion 呈現選擇提示：

**問題 1**（標題：「課程」）：
「您想測驗哪堂課？」
選項：
1. "Slash Commands (01)" — 自訂指令、skills、frontmatter、參數
2. "Memory (02)" — CLAUDE.md、記憶層級、規則、自動記憶
3. "Skills (03)" — 漸進式揭露、自動觸發、SKILL.md
4. "Subagents (04)" — 任務委派、代理設定、隔離

**問題 2**（標題：「課程」）：
「您想測驗哪堂課？（續）」
選項：
1. "MCP (05)" — 外部整合、傳輸協定、伺服器、Tool Search
2. "Hooks (06)" — 事件自動化、PreToolUse、exit codes、JSON I/O
3. "Plugins (07)" — 整合方案、marketplace、plugin.json
4. "更多課程..." — Checkpoints、Advanced Features、CLI

若選擇「更多課程...」，呈現：

**問題 3**（標題：「課程」）：
「請選擇您的課程：」
選項：
1. "Checkpoints (08)" — 回溯、還原、安全實驗
2. "Advanced Features (09)" — 規劃模式、權限、print mode、思考模式
3. "CLI Reference (10)" — 旗標、輸出格式、腳本、管道

### 步驟 2：閱讀課程內容

閱讀課程 README.md 檔案以更新脈絡：
- 讀取檔案：`<lesson-directory>/README.md`

然後使用 `references/question-bank.md` 中該課程的題庫（10 道預設題目，附答案與解說）。為了控制脈絡預算，只讀取所選的那一堂課的 README — 不要十堂全讀。

### 步驟 3：呈現測驗

詢問使用者測驗的時機背景：

使用 AskUserQuestion（標題：「時機」）：
「您是在課程的哪個階段進行這次測驗？」
選項：
1. "學前（預先測試）" — 我還沒讀過這堂課，測試我的先備知識
2. "學中（進度檢查）" — 我正在學習這堂課的途中
3. "學後（精熟檢查）" — 我已完成這堂課，想驗證理解程度

此脈絡會影響結果的呈現方式（見步驟 5）。

### 步驟 4：分回合呈現題目

從題庫中選取 10 道題目，分 5 回合呈現，每回合 2 題。每道題使用 AskUserQuestion 附上題目文字與 3-4 個選項。

**重要**：AskUserQuestion 每題最多 4 個選項，每回合 2 題。

每回合呈現 2 道題目。全部 5 回合完成後，進入計分。

**每回合題目格式：**

題庫中的每道題包含：
- `question`：題目文字
- `options`：3-4 個選項（其中一個正確，題庫中有標示）
- `correct`：正確答案標籤
- `explanation`：為何此答案正確
- `category`：「conceptual」或「practical」

使用 AskUserQuestion 呈現每道題目。記錄使用者的每題回答。

### 步驟 5：計分並呈現結果

所有回合結束後，計算分數並呈現結果。

**計分方式：**
- 每答對一題 = 1 分
- 滿分 = 10 分

**等級標準：**
- 9-10：精熟 — 理解優異
- 7-8：熟練 — 掌握良好，有少許缺口
- 5-6：發展中 — 已理解基礎，需要複習
- 3-4：起步 — 有明顯缺口，建議複習
- 0-2：尚未掌握 — 請從本課開頭開始學習

**輸出格式：** 依照 `references/results-template.md` 中的報告範本。它定義了分數行、逐題結果表格、「答錯的題目 — 請複習這些」區塊、時機專屬訊息（學前／學中／學後），以及「建議的下一步」章節。以本次執行的資料填入其方括號佔位內容。

### 步驟 6：提供後續選項

呈現結果後，使用 AskUserQuestion：

「您接下來想做什麼？」
選項：
1. "重新測驗" — 再試一次同一堂課的測驗
2. "測驗其他課程" — 切換到不同的課程
3. "解說答錯的主題" — 取得答錯題目的詳細解說
4. "結束" — 結束測驗

若選擇**重新測驗**：回到步驟 4（跳過時機問題，使用相同時機）。
若選擇**測驗其他課程**：回到步驟 1。
若選擇**解說主題**：詢問哪個題號，然後閱讀課程 README.md 的相關章節並搭配範例解說。

## 驗收標準

完成的測驗執行必須滿足以下所有條件 — 在呈現最終結果前逐一確認：

- 在提出任何題目之前，已確定恰好一堂課程（01-10）。
- 已透過 AskUserQuestion 取得時機背景（學前／學中／學後）。
- 已呈現 `references/question-bank.md` 中該課程的 10 道題目，分 5 回合、每回合 2 題，且每題的選項順序皆已打亂。
- 每個回答都已依記錄的正確位置計分，得出 0-10 範圍內的整數分數。
- **預期輸出**：一份 `## Lesson Quiz Results` 報告，包含分數行（`Score: N/10` 加上等級標籤）、逐題結果表格、每道答錯題目的「答錯的題目 — 請複習這些」區塊，以及時機專屬訊息。
- 已提供後續選項提示（重新測驗／其他課程／解說／結束）。

範例：一次 Hooks 課程「學後」測驗得 7/10，報告應顯示 `Score: 7/10 — Proficient`，結果表格列出全部 10 列，並展開 3 道答錯題目的正確答案、解說與複習指引。

## 邊界情況與錯誤處理

### 無效的課程參數
若參數不符合任何課程，顯示有效的課程列表並要求使用者選擇。

### 使用者在測驗中途想退出
若使用者在任何回合中表示想停止，呈現目前已作答題目的部分結果（分數以已作答題數為分母，而非 10）。

### 找不到課程 README
若在預期路徑找不到 README.md 檔案，通知使用者並建議檢查儲存庫結構。不要以捏造的內容繼續進行。

### 題庫缺少某課程的題目
若 `references/question-bank.md` 沒有所選課程的題目，告知使用者並停止 — 絕不自行編造題目來補足。

## 驗證

### 觸發測試套件

**應觸發：**
- "quiz me on hooks"
- "lesson quiz"
- "test my knowledge of lesson 3"
- "practice quiz for MCP"
- "do I understand skills"
- "quiz me on slash commands"
- "lesson-quiz 06"
- "test me on checkpoints"
- "how well do I know the CLI"
- "quiz me before I start the memory lesson"

**不應觸發：**
- "assess my overall level"（請使用 /self-assessment）
- "explain hooks to me"
- "create a hook"
- "what is MCP"
- "review my code"

