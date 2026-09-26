<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 進階功能

關於 Claude Code 進階能力的全面指南，包含規劃模式、延伸思考、自動模式、背景任務、權限模式、列印模式（非互動式）、工作階段管理、互動功能、頻道、語音聽寫、遠端控制、網頁工作階段、桌面應用程式、任務清單、提示詞建議、git worktrees、沙盒化、管理式設定以及組態。

## 目錄

1. [概述](#概觀)
2. [規劃模式](#planning-mode)
3. [延伸思考](#延伸思考-extended-thinking)
4. [自動模式](#auto-mode)
5. [背景任務](#背景任務)
6. [監控工具 (事件驅動串流)](#monitor-工具-事件驅動串流)
7. [動態工作流程](#動態工作流程-dynamic-workflows)
8. [排程任務](#排程任務)
9. [權限模式](#權限模式)
10. [無頭模式](#headless-模式)
11. [工作階段管理](#工作階段管理)
12. [跨工作階段訊息傳遞](#跨工作階段訊息傳遞)
13. [互動功能](#互動功能)
14. [輸出樣式](#輸出樣式-output-styles)
15. [狀態列](#狀態列-status-line)
16. [TUI 模式 (全螢幕)](#tui-模式全螢幕)
17. [語音聽寫](#語音聽寫)
18. [頻道](#channels)
19. [Chrome 整合](#chrome-integration)
20. [遠端控制](#遠端控制-remote-control)
21. [網頁工作階段](#web-sessions)
22. [桌面應用程式](#desktop-app)
23. [任務清單](#task-list)
24. [提示詞建議](#prompt-建議)
25. [Git Worktrees](#git-worktrees)
26. [沙盒化](#sandboxing)
27. [管理式設定 (企業版)](#管理設定企業版)
28. [設定與組態](#設定與設定選項)
29. [信任與權限範圍](#信任與權限範圍)
30. [代理團隊](#agent-teams)
31. [最佳實踐](#最佳實務)
32. [補充資源](#其他資源)

---

## 概觀

Claude Code 中的進階功能透過規劃、推理、自動化與控制機制擴展了核心能力。這些功能為複雜的開發任務、程式碼審查、自動化以及多工作階段管理提供了精密的 workflow。

**關鍵進階功能包括：**
- **Planning Mode**：在編寫程式碼之前建立詳細的實作計畫
- **Extended Thinking**：針對複雜問題進行深度推理
- **Auto Mode**：背景安全分類器在執行每個動作前進行審查
- **Background Tasks**：執行長時間的操作而不阻塞對話
- **Permission Modes**：控制 Claude 的權限（`manual`（原 `default`）、`acceptEdits`、`plan`、`auto`、`dontAsk`、`bypassPermissions`）
- **Print Mode**：以非互動方式執行 Claude Code，用於自動化與 CI/CD (`claude -p`)
- **Session Management**：管理多個工作階段
- **Interactive Features**：鍵盤快捷鍵、多行輸入與指令歷史紀錄
- **Voice Dictation**：支援 20 種語言 STT 的按住說話語音輸入
- **Channels**：MCP servers 將訊息推送到執行中的工作階段（研究預覽版）
- **Remote Control**：從 Claude.ai 或 Claude app 控制 Claude Code
- **Web Sessions**：在瀏覽器中的 claude.ai/code 執行 Claude Code
- **Desktop App**：用於視覺化 diff 審查與多個工作階段的獨立應用程式
- **Task List**：在 context 壓縮過程中維持持久的任務追蹤
- **Prompt Suggestions**：根據 context 提供智慧指令建議
- **Git Worktrees**：用於平行工作的隔離工作樹分支
- **Sandboxing**：作業系統層級的檔案系統與網路隔離
- **Managed Settings**：透過 plist、Registry 或管理檔案進行企業級部署
- **Configuration**：使用 JSON 設定檔自訂行為

---

## Planning Mode

Planning mode 讓 Claude 在執行複雜任務之前能夠進行思考，並建立一個您可以審查與核准的詳細計畫。

### 什麼是 Planning Mode？

Planning mode 是一種兩階段的方法：
1. **規劃階段 (Planning Phase)**：Claude 分析任務並建立詳細的實作計畫。
2. **執行階段 (Implementation Phase)**：獲得核准後，Claude 執行該計畫。

### 何時使用 Planning Mode

✅ 適用於：
- 複雜的多檔案重構
- 新功能實作
- 架構變更
- 資料庫遷移
- 重大的 API 重新設計

❌ 不適用於：
- 簡單的 bug 修復
- 格式變更
- 單一檔案編輯
- 快速查詢

### 啟動 Planning Mode

**斜線命令**：
```bash
/plan Implement user authentication system
```

**CLI flag**：
```bash
claude --permission-mode plan
```

**設定為預設值**：
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**鍵盤快捷鍵**：
- `Shift + Tab` - 切換權限模式（包含 plan）
- `Alt + M` - 切換權限模式（Windows/Linux）

### Planning Mode 的優點

- **結構化的實作方法**：詳細拆解實作步驟。
- **審查與核准**：在執行前核准或調整計畫。
- **風險識別**：在實作前識別潛在問題。
- **清晰的階段**：組織良好的實作階段與里程碑。

### 範例：功能實作

```
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.

## Implementation Plan

### Phase 1: Infrastructure Setup
1. Install WebSocket libraries (socket.io)
2. Set up WebSocket server
3. Configure connection management

### Phase 2: Backend Implementation
4. Create notification service
5. Implement notification queue
6. Add database schema for notifications
7. Create API endpoints

### Phase 3: Frontend Integration
8. Set up WebSocket client
9. Create notification UI components
10. Implement notification state management
11. Add notification permissions

### Phase 4: Testing & Deployment
12. Write unit tests
13. Write integration tests
14. Update documentation
15. Deploy to staging

Estimated time: 3-4 hours
Files to modify: 12
New files to create: 8

Ready to proceed? (yes/no/modify plan)

User: yes

Claude: [Begins implementation following the plan]
```

### 計畫修改

```
User: Modify the plan - skip the queue for now, we'll add it later

Claude: Updated plan:
[顯示移除佇列後的修改後計畫]

User: Looks good, proceed

Claude: [實作修改後的計畫]
```

### 計畫模式設定

規劃模式可透過 CLI 旗標或斜線命令啟動：

```bash
# 透過 CLI 啟動計畫模式
claude --permission-mode plan

# 或在 REPL 中使用 /plan 斜線命令
/plan Implement user authentication system
```

**用於規劃的模型別名**：使用 `opusplan` 作為模型別名，以便使用 Opus 進行規劃並使用 Sonnet 進行執行：

```bash
claude --model opusplan "design and implement the new API"
```

**從外部編輯計畫**：按下 `Ctrl+G` 可在您的外部編輯器中開啟目前的計畫，以便進行詳細修改。

> **v2.1.112 更新**：計畫檔案現在會以產生它們的提示詞命名（而非隨機詞彙），讓它們更容易瀏覽與重複使用。

> **v2.1.136 更新 — 計畫模式的寫入阻斷為無條件**：計畫模式現在會阻斷所有檔案寫入，包括當 `permissions.allow` 中存在匹配的 `Edit(...)` 規則時也不例外。先前較寬鬆的 `Edit(...)` 規則可能讓寫入在計畫模式中通過；此漏洞現已修補。如果某個工作流程依賴舊有行為，請在編輯前先退出計畫模式（`Shift+Tab`）。

---

## 延伸思考 (Extended Thinking)

延伸思考允許 Claude 在提供解決方案之前，花費更多時間對複雜問題進行推理。

### 什麼是延伸思考？

延伸思考是一種刻意且逐步的推理過程，在此過程中 Claude 會：
- 將複雜問題拆解
- 考慮多種方法
- 評估權衡（trade-offs）
- 針對邊際情況（edge cases）進行推理

### 啟動延伸思考

**鍵盤快捷鍵**：
- `Option + T` (macOS) / `Alt + T` (Windows/Linux) - 切換延伸思考

**自動啟動**：
- 所有模型預設皆已啟用 (Opus 5, Opus 4.8, Opus 4.7, Sonnet 4.6, Haiku 4.5)
- Opus 5 / Opus 4.8：具備不同努力程度的適應性推理：`low` (○), `medium` (◐), `high` (●), `xhigh`, `max`。Opus 5（v2.1.219）、Opus 4.8（v2.1.154）、Opus 4.6 與 Sonnet 4.6 的預設值為 `high`，Opus 4.7 則為 `xhigh`。`xhigh` 可用於 Opus 5、Opus 4.8 與 Opus 4.7（在 Opus 4.6 / Sonnet 4.6 上會退回為 `high`）。`max` 適用於 Opus 5、Opus 4.8/4.7/4.6 與 Sonnet 4.6（僅限目前工作階段）。Haiku 4.5 沒有努力程度。Opus 5、Opus 4.8 與 Opus 4.7 具備 1M token 的原生 context window（1M context 修復於 v2.1.117 — 在此之前，`/context` 對 Opus 4.7 的計算誤以 200K window 為準，導致過早觸發 autocompact）。自 v2.1.129 起，`/context` 僅在 UI 內顯示視覺化；ASCII 視覺化不再洩漏到對話 context 中（每次呼叫節省約 1.6k tokens），因此可以自由呼叫 `/context`。
- Pro/Max 訂閱者在使用 Opus 4.6 / Sonnet 4.6 時：預設努力程度已於 v2.1.117 從 `medium` 提升至 `high`。
- 其他模型：固定預算上限為 31,999 tokens

**設定方法**：
- 切換：`Alt+T` / `Option+T`，或透過 `/config`
- 查看推理過程：`Ctrl+O` (詳細模式)
- 設定努力程度：使用 `/effort` 命令或 `--effort` 旗標

**自訂預算**：
```bash
export MAX_THINKING_TOKENS=1024
```

**努力程度**（支援 Opus 5、Opus 4.8、Opus 4.7、Opus 4.6 與 Sonnet 4.6 — 不支援 Haiku 4.5）：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○), medium (◐), high (●), xhigh (Opus 5/4.8/4.7), 或 max — Opus 5 與 Opus 4.8 的預設為 high
```

**CLI 旗標**：
```bash
claude --effort high "complex architectural review"
```

**斜線命令**：
```
/effort high
```

> **注意：** 提示詞中的關鍵字 "ultrathink" 會啟動深度推理模式。努力程度 `low`、`medium`、`high` 與 `max` 支援 Opus 5、Opus 4.8、Opus 4.7、Opus 4.6 以及 Sonnet 4.6（Haiku 4.5 皆不支援）。`xhigh` 可用於 Opus 5、Opus 4.8 與 Opus 4.7。Opus 5、Opus 4.8（以及 Opus 4.6 / Sonnet 4.6）的預設努力程度為 `high`，Opus 4.7 則為 `xhigh`。與會在首次執行時鎖定預設努力程度的 Opus 4.8 和 Opus 4.7 不同，Opus 5 沒有這種鎖定 — 您先前設定的等級會沿用。`/effort` 選單也提供 `ultracode`，它**不是**模型的努力程度 — 它會傳送 `xhigh`，並讓 Claude 編排動態工作流程（僅限目前工作階段）。

### Opus 5 的安全分類器後備機制

有別於 [`fallbackModel` 設定](#後備模型fallbackmodel)（處理過載與無法使用的情況），當安全分類器標記某個請求時，Claude Code 會套用**依類別區分的後備機制**。此功能需要 Claude Code v2.1.219 或更新版本。

- **Opus 5**：被標記為資安相關的請求會改在 Opus 4.8 上重新執行。被標記為生物相關的請求則會以拒絕結束，因為 Opus 5 執行自己的生物分類器，且沒有後備模型。這類拒絕從第一個被標記的請求就會出現。
- 攻擊性資安或生物領域的工作負載 — 滲透測試、奪旗（Capture the Flag, CTF）練習，以及與生物相關的程式碼庫 — 會頻繁觸發後備機制，通常在第一個請求就發生。

為何這在此很重要：如果您在 Opus 5 上執行 [security-review subagents](../04-subagents/README.md) 或 CTF/滲透測試工作，資安相關的提示詞預期會被靜默地改在 Opus 4.8 上重新執行（努力程度上限與 context window 皆不同），而生物相關的提示詞則會直接被拒絕。若此行為造成困擾，請明確指定其他模型。

**控制切換行為** — `switchModelsOnFlag` 設定（v2.1.170+，預設 `true`）決定後備是否靜默發生。將其設為 `false` 時，Claude Code 會改為暫停，讓您選擇切換模型或編輯提示詞。它在 `/config` 中顯示為 **Switch models when a message is flagged**。

```json
{
  "switchModelsOnFlag": false
}
```

### 延伸思考的優點

- **深度推理**：花費更多時間思考複雜問題
- **更好的架構決策**：透過全面的評估做出明智的選擇
- **考慮邊際情況**：思考潛在的問題與情境
- **徹底的分析**：以結構化的方式解決複雜問題

### 範例：架構決策

```
User: ultrathink Should we use microservices or a monolith for our e-commerce platform?

Claude: Let me think through this architectural decision carefully...

<extended_thinking>
## Context Analysis
- Team size: 5 developers
- Expected traffic: 10k daily users initially
- Need to scale to 100k+ users
- Limited DevOps resources
- 6-month timeline to MVP

## Approach 1: Microservices
Pros:
+ Independent scaling
+ Technology flexibility
+ Team independence
+ Fault isolation

Cons:
- Operational complexity
- Network overhead
- Distributed tracing needed
- Requires DevOps expertise
- Slower initial development

## Approach 2: Monolith
Pros:
+ Simpler deployment
+ Faster initial development
+ Easier debugging
+ Lower operational overhead
+ Single codebase

Cons:
- Scaling challenges later
- All-or-nothing deployments
- Technology lock-in
- Potential for tight coupling

## Decision Factors
1. Team size (5 devs) → Favors monolith
2. DevOps resources (limited) → Favors monolith
3. Timeline (6 months) → Favors monolith
4. Future scale (100k users) → Achievable with monolith + caching
5. Complexity budget → Limited

## Recommendation
Start with a **modular monolith**:
- Single deployment unit
- Clear module boundaries
- Clean interfaces between modules
- Prepare for future extraction to microservices if needed

This gives you:
✓ Fast initial development
✓ Low operational complexity
✓ Option to extract services later
✓ Clear architecture

Migration path: When you reach 50k+ users or specific modules need independent scaling, extract those modules into microservices.
</extended_thinking>

**Recommendation**: Implement a modular monolith with clean module boundaries.

This approach balances your current constraints (team size, timeline, DevOps resources) with future scalability needs. You can migrate specific modules to microservices later when there's a clear business need.
```

### 延伸思考設定 (Extended Thinking Configuration)

延伸思考透過環境變數、鍵盤快捷鍵與 CLI 旗標進行控制：

```bash
# 設定思考 token 預算
export MAX_THINKING_TOKENS=16000

# 設定努力程度（Opus 5、Opus 4.8、Opus 4.7、Opus 4.6、Sonnet 4.6）：low (○), medium (◐), high (●), xhigh (Opus 5/4.8/4.7), 或 max — Opus 5 與 Opus 4.8 的預設為 high
export CLAUDE_CODE_EFFORT_LEVEL=high
```

在工作階段期間使用 `Alt+T` / `Option+T` 進行切換，使用 `/effort` 設定努力程度，或透過 `/config` 進行設定。

> **精簡系統提示詞（v2.1.154）：** 精簡系統提示詞現在是除 Haiku、Sonnet 以及 Opus 4.7 及更早版本以外所有模型的**預設值**，可降低 Opus 5 與 Opus 4.8 的基礎 token 開銷。

---

## Auto Mode

Auto Mode 是一種權限模式，它使用背景安全分類器在執行每個動作前進行審查。它允許 Claude 自主工作，同時阻斷危險的操作。所有方案皆可使用，但需要符合資格的模型（在 Anthropic API 與 Claude Platform on AWS 上為 Claude Opus 5、Opus 4.6+、Sonnet 4.6+ 或 Fable 5；在 Bedrock、Vertex、Foundry 以及已登入的 Claude apps gateway 工作階段上為 Opus 5、Sonnet 5、Opus 4.7、Opus 4.8 或 Fable 5）。在 Team 與 Enterprise 方案上預設為開啟 — 管理員可以在管理設定中為整個組織關閉它。

### 需求

Auto mode 僅在您的帳戶符合以下所有需求時才可使用：

- **Plan**：所有方案。
- **Organization**：在 Team 與 Enterprise 方案上，auto mode 預設可用。管理員可以在管理設定中將 `permissions.disableAutoMode` 設為 `"disable"`，為整個組織關閉此功能。
- **Model**：在 Anthropic API 與 Claude Platform on AWS 上 — Claude Opus 4.6 或更新版本（包含 Opus 5）、Sonnet 4.6 或更新版本，或 Fable 5。在 Amazon Bedrock、Google Cloud 的 Agent Platform（Vertex AI）、Microsoft Foundry 以及已登入的 Claude apps gateway 工作階段上 — 僅限 Claude Sonnet 5、Opus 4.7 或更新版本（包含 Opus 5）以及 Fable 5。較舊的模型 — Sonnet 4.5、Opus 4.5、Haiku 以及 claude-3 模型 — 在任何供應商上皆不支援。
- **Provider**：在 Anthropic API、Claude Platform on AWS、Amazon Bedrock、Google Cloud 的 Agent Platform（Vertex AI）、Microsoft Foundry 以及已登入的 Claude apps gateway 工作階段上預設可用。在 v2.1.158 到 v2.1.206 之間，除了 Anthropic API 與 Claude Platform on AWS 之外，auto mode 在上述所有平台上皆為關閉，直到您設定 `CLAUDE_CODE_ENABLE_AUTO_MODE=1`；v2.1.207 移除了此需求。此變數仍為相容性而接受，但自 v2.1.207 起不再有任何作用。
- **Classifier**：會增加額外的 token 成本，但 Enterprise 方案與 Claude API 帳戶除外，v2.1.278+ 會在伺服器端免費執行此檢查

### 啟用 Auto Mode

```bash
# 使用 CLI 旗標解鎖 auto mode（Max 訂閱者在 Opus 4.7 上不再需要此旗標，可直接存取）
claude --enable-auto-mode

# 然後在 REPL 中使用 Shift+Tab 切換至該模式
```

> **v2.1.112 更新**：Auto mode 不再需要 `--enable-auto-mode` 旗標。Max 訂閱者可直接在 Opus 4.7 上存取。

> **v2.1.158 更新**：Auto mode 開始在 Bedrock、Vertex 與 Foundry 上支援 Opus 4.7/4.8，但需透過 `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 啟用。
>
> **v2.1.207 更新**：該選擇加入需求已移除。Auto mode 現在在 Bedrock、Vertex AI、Microsoft Foundry 以及已登入的 Claude apps gateway 工作階段上，針對 Claude Sonnet 5、Opus 4.7、Opus 4.8 與 Fable 5（以及自 v2.1.219 起的 Opus 5）預設可用 — 不需任何旗標或環境變數。管理員可以在管理設定中使用 `disableAutoMode` 停用它。`CLAUDE_CODE_ENABLE_AUTO_MODE` 仍為相容性而接受，但自 v2.1.207 起不再有任何作用。

或者將其設定為預設權限模式：

```bash
claude --permission-mode auto
```

透過設定檔設定：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 分類器的運作方式

背景分類器使用以下決策順序來評估每個動作：

1. **Allow/deny rules** — 首先檢查明確的權限規則
2. **Read-only/edits auto-approved** — 檔案讀取與編輯會自動通過
3. **Classifier** — 背景分類器審查該動作
4. **Fallback** — 在連續 3 次或總共 20 次阻斷後，退回至提示模式

### 預設阻斷動作

Auto mode 預設會阻斷以下動作：

| 阻斷動作 | 範例 |
|----------------|---------|
| Pipe-to-shell 安裝 | `curl \| bash` |
| 向外部傳送敏感資料 | 透過網路傳送 API keys、憑證等敏感資料 |
| 正式環境部署 | 目標為正式環境的部署指令 |
| 大量刪除 | 在大型目錄執行 `rm -rf` |
| IAM 變更 | 權限與角色修改 |
| 強制推送至 main | `git push --force origin main` |

> **更多決策移交給分類器（v2.1.218）**：分類器也會判斷針對檔案系統根目錄或家目錄的移除操作，例如 `rm -rf /` 與 `rm -rf ~`，包括該移除位於命令替換或行程替換之內的情況。在 v2.1.218 之前，一般形式會改為提示您核准，而替換形式則在 v2.1.208 到 v2.1.217 之間會提示。背景 `&` 與可疑 Windows 路徑的檢查同樣不再開啟權限對話框 — 改由分類器判斷。

### 預設允許動作

| 允許動作 | 範例 |
|----------------|---------|
| 本地檔案操作 | 讀取、寫入、編輯專案檔案 |
| 已宣告的相依套件安裝 | 從清單中執行 `npm install`、`pip install` |
| 唯讀 HTTP | 使用 `curl` 取得文件 |
| 推送至目前分支 | `git push origin feature-branch` |

### 設定 Auto Mode

**將預設規則輸出為 JSON**：
```bash
claude auto-mode defaults
```

**還原預設的 auto-mode 設定**（v2.1.212），會顯示確認提示（使用 `--yes` 略過）：
```bash
claude auto-mode reset
claude auto-mode reset --yes
```

**設定受信任的基礎設施**：透過用於企業部署的 `autoMode.environment` 管理設定進行設定。這允許管理員定義受信任的 CI/CD 環境、部署目標與基礎設施模式。

#### 使用 `"$defaults"` 擴充預設值（v2.1.118）

自 v2.1.118 起，`autoMode.allow`、`autoMode.soft_deny` 與 `autoMode.environment` 接受一個 `"$defaults"` token，它會將您的規則**附加**到內建清單中，而非取代它。在 v2.1.118 之前，任何使用者定義的陣列都會靜默地覆蓋內建的預設值。

#### 使用 `autoMode.hard_deny` 設定無條件阻斷（v2.1.136）

`autoMode.hard_deny`（v2.1.136+）是一組分類器規則，它們會**無論使用者推斷意圖為何**，都阻斷某一類動作。將此用於在 auto mode 中絕對不應執行的動作 — 例如對根路徑執行 `rm -rf`，或對受保護分支執行 `git push --force`。與 `soft_deny` 不同，hard-deny 規則不能被分類器協商。

```json
{
  "autoMode": {
    "hard_deny": ["Bash(rm -rf /:*)", "Bash(git push --force*)"]
  }
}
```

**之前（取代內建值 — v2.1.118 之前的行為）：**

```json
{
  "autoMode": {
    "allow": ["Bash(gh pr list:*)"]
  }
}
```

**之後（擴充內建值 — v2.1.118+）：**

```json
{
  "autoMode": {
    "allow": ["$defaults", "Bash(gh pr list:*)"],
    "soft_deny": ["$defaults", "Bash(kubectl delete:*)"],
    "environment": ["$defaults", "trusted-ci.internal"]
  }
}
```

使用 `"$defaults"` 可在保留原廠基準規則的同時，疊加組織或專案特定的規則。

#### 使用 `autoMode.classifyAllShell` 分類每個 shell 指令（v2.1.193）

`autoMode.classifyAllShell`（布林值，v2.1.193+）會將**所有** Bash/PowerShell 指令都交由 auto-mode 分類器處理。當您希望分類器檢查工作階段中的每個 shell 指令時，請啟用此設定。

```json
{
  "autoMode": {
    "classifyAllShell": true
  }
}
```

同一版本也會在 auto mode 阻斷某個動作時顯示**拒絕原因** — 可在對話紀錄、拒絕提示通知以及 `/permissions` 下的最近拒絕清單中看到（v2.1.193+）。

#### 內建的意圖式保護（v2.1.183）

有別於使用者設定的 `hard_deny`，除非您在本次工作階段中明確要求，否則 auto mode 預設會阻斷以下破壞性指令：

- `git reset --hard`、`git checkout -- .`、`git clean -fd`、`git stash drop`
- `git commit --amend`（當該 commit 並非由代理在本次工作階段中建立時）
- `terraform destroy`、`pulumi destroy`、`cdk destroy`（除非您要求針對特定 stack）

這是由推斷意圖驅動的內建預設保護 — 您不需要自行將這些加入 `hard_deny`。

### 後備行為

當分類器不確定時，auto mode 會退回至提示使用者：
- 在 **3 次連續** 分類器阻斷後
- 在一個工作階段中達到 **總共 20 次** 分類器阻斷後

這確保了當分類器無法自信地核准動作時，使用者始終保有控制權。

### 植入等效 Auto Mode 的權限（不需要 Team 方案）

如果您沒有 Team 方案，或者想要一種不使用背景分類器的更簡單方法，您可以透過在 `~/.claude/settings.json` 中植入保守的基礎安全權限規則來達成類似效果。該腳本從唯讀與本地檢查規則開始，接著讓您僅在需要時才選擇加入編輯、測試、本地 git 寫入、套件安裝以及 GitHub 寫入動作。

**File:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 預覽將會新增的內容（不會寫入任何變更）
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 套用保守的基準設定
python3 09-advanced-features/setup-auto-mode-permissions.py

# 僅在需要時增加更多功能
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

此腳本會在以下類別中新增規則：

| 類別 | 範例 |
|----------|---------|
| 核心唯讀工具 | `Read(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| 本地檢查 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| 選用編輯功能 | `Edit(*)` |
| 選用測試/建置 | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| 選用 git 寫入 | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)`, `Bash(git switch:*)`, `Bash(git stash:*)`, `Bash(git tag:*)` |
| 套件管理員 | `Bash(npm ci:*)`, `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(pip3 install:*)` |
| 建置與測試 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 常用 shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

危險的操作（例如 `rm -rf`、`sudo`、force push、`DROP TABLE`、`terraform destroy` 等）已刻意排除。此腳本具有冪等性（idempotent）——執行兩次也不會導致規則重複。

---

## 背景任務

背景任務允許執行長時間執行的操作，而不會阻塞您的對話。

### 什麼是背景任務？

背景任務會在您繼續工作時非同步執行：
- 長時間的測試套件
- 建置流程
- 資料庫遷移
- 部署腳本
- 分析工具

**基本用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 檢查進度
/task show bg-1234   # 查看輸出
/task cancel bg-1234 # 取消任務
```

### 開始執行背景任務

```
User: Run the full test suite in the background

Claude: Starting tests in background (task-id: bg-1234)
You can continue working while tests run.

[You can continue the conversation]

User: Meanwhile, let's refactor the auth module

Claude: [Works on auth module while tests run in background]

[Later, when tests complete]

Claude: 📢 Background task bg-1234 completed:
✅ 245 tests passed
❌ 3 tests failed
View results: /task show bg-1234
```

### 管理背景任務

**列出活動中的任務**：
```
User: /task list

Active background tasks:
1. [bg-1234] Running tests (50% complete, 2min remaining)
2. [bg-1235] Building Docker image (25% complete, 8min remaining)
3. [bg-1236] Deploying to staging (90% complete, 30sec remaining)
```

**檢查任務狀態**：
```
User: /task status bg-1234

Task bg-1234: Running tests
Status: In progress
Progress: 120/245 tests (49%)
Started: 2025-11-08 10:30:15
Estimated completion: 2025-11-08 10:34:22
```

**查看任務輸出**：
```
User: /task show bg-1234

[Shows live output from the test run]
```

**取消任務**：
```
User: /task cancel bg-1234

Cancelled background task bg-1234
```

### 範例：平行開發

```
User: Run the build in the background

Claude: Starting build... (task-id: bg-5001)

User: Also run the linter in background

Claude: Starting linter... (task-id: bg-5002)

User: While those run, let's implement the new API endpoint

Claude: [Implements API endpoint while build and linter run]

[10 minutes later]

Claude: 📢 Build completed successfully (bg-5001)
📢 Linter found 12 issues (bg-5002)

User: Show me the linter issues

Claude: [Shows linter output from bg-5002]
```

### 設定

`settings.json` 中沒有背景任務的設定區塊。此功能由環境變數控制：

```bash
# 完全關閉背景任務
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
```

並行數量也不是背景任務的設定 — 同時執行多少個代理是由 `CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`（預設 `20`）控制。

---

## Monitor 工具 (事件驅動串流)

> **v2.1.98 新功能**：Monitor 工具讓 Claude 可以監控背景指令的 stdout，並在匹配的事件出現時立即做出反應 — 取代了用於等待長時間執行行程的輪詢迴圈與 `sleep`。

Monitor 可附加到任何會寫入 stdout 的 shell 指令。指令產生的每一行 stdout 都會變成一個喚醒工作階段的通知。由 Claude 指定指令；測試框架會串流輸出並在事件觸發時傳送。關於啟動底層行程，請參閱相關的 [背景任務](#背景任務) 章節。

> **截止期限（v2.1.271）**：每個 Monitor 監看現在都帶有截止期限 — 最多 30 分鐘，在單一提示詞的 `-p` 執行中則為 10 分鐘。截止期限到達時，Claude 會收到通知，以便重新設定監看。已無法再設定永不逾時的監看。

### 為什麼這很重要

使用 `/loop` 或 `sleep` 進行輪詢，無論內容是否有變化，每個週期都會消耗完整的 API 往返。Monitor 在事件觸發前會保持靜默，在指令靜止時消耗 **zero tokens**。當事件發生時，Claude 會立即做出反應 — 不會因為等待下一次輪詢週期而導致發現延遲。對於任何執行時間超過幾分鐘的任務，這比輪詢迴圈更便宜且更快。

### 兩種常見模式

**Stream filters（串流篩選器）** 監控來自長時間執行來源的連續輸出。指令會持續執行；每一行匹配的內容都是一個事件。

```bash
tail -f /var/log/app.log | grep --line-buffered "ERROR"
```

**Poll-and-emit filters（輪詢與發送篩選器）** 定期檢查來源，僅在發生變化時發送。適用於 API、資料庫或任何沒有原生串流的來源。

```bash
last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" || true
  last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
  sleep 30
done
```

### 具體範例

「啟動我的開發伺服器並監控錯誤。」Claude 會將伺服器作為背景任務啟動，附加一個 Monitor 篩選器 (`tail -F server.log | grep --line-buffered -E "ERROR|FATAL"`)，接著工作階段進入靜默狀態。當日誌中出現錯誤行的一瞬間，Claude 會被喚醒、讀取錯誤並做出反應 — 例如重啟伺服器、修復 bug 或向你回報 — 而不需要你手動檢查。

> **警告**：當使用管道（piping）傳輸至 `grep` 時，**務必**使用 `grep --line-buffered`。如果沒有使用它，grep 會以 4KB 為區塊緩衝 stdout，這可能導致在低流量串流中事件延遲數分鐘。這是實務中導致 Monitor 失效的首要原因 — 如果你的篩選器在不該靜默時看起來卻是靜默的，請先檢查是否使用了 `--line-buffered` 旗標。

---

## 動態工作流程 (Dynamic Workflows)

> **v2.1.154 新功能**

動態工作流程讓 Claude 能**確定性地**編排數十到數百個背景 [subagents](../04-subagents/README.md) — 將扇出（fan-out）、管線與平行階段編寫在腳本中，而非交由模型即興發揮。單一代理只持有一個 context window，而工作流程會將任務拆解給多個代理，再重新組合它們的結果。

自 v2.1.219 起，動態工作流程預設採用**中型規模準則（目標少於 10 個代理）** — 若您以 Pro 方案登入，則自 v2.1.271 起預設為**小型**。可透過 `/config` 中的 **Dynamic workflow size** 選擇其他規模（或不設限），或在設定檔中設定 `workflowSizeGuideline` 鍵。執行中工作流程的狀態列會顯示目前的規模，並指引您到 `/config` 進行變更。

### 何時使用

- **全面涵蓋** — 平行稽核或審查多個檔案/面向。
- **信心** — 產生彼此獨立的觀點，再於提交前對發現進行對抗式驗證。
- **超越單一 context 的規模** — 大型遷移、廣泛掃描，或任何單一 context 都容納不下的研究。

對於您已了解的一次性任務，單一代理（或直接編輯）仍是正確的工具 — 工作流程在工作需要扇出時才能發揮效益。

### 啟動與檢視

- **啟動**：請 Claude 為該任務建立工作流程（例如「run a workflow to review every file in `src/`」）。Claude 會撰寫編排腳本並在背景執行。
- **檢視**：`/workflows` 命令會顯示執行中與已完成的工作流程，並附帶即時進度。
- **`ultracode`**：在 `/effort` 選單中選擇 `ultracode` 會為目前工作階段開啟此功能 — 它會向模型傳送 `xhigh`，*並且*讓 Claude 預設編排動態工作流程。它僅限目前工作階段，且不接受在設定檔中設定。（自 v2.1.160 起觸發關鍵字為 `ultracode`；單獨的「workflow」一詞不再觸發執行。）

工作流程建立在 subagent 模型之上 — 請參閱 [Subagents](../04-subagents/README.md) 了解個別代理如何定義與劃定範圍。

---

## 排程任務

排程任務讓您能依照週期性時程或作為一次性提醒，自動執行提示詞。任務的範圍僅限於工作階段（session-scoped）——它們會在 Claude Code 執行期間運作，並在工作階段結束時清除。自 v2.1.72+ 版本起可用。

> **於 claude.com 以「Routines」名稱行銷（2026-05-14）**：Anthropic 的產品部落格將此介面介紹為 **Routines**。CLI 命令仍為 `/schedule`；本指南使用原始的「排程任務」名稱以保持一致性。如果您在 claude.com 文件或桌面應用程式中看到「Routines」，它指的是同一功能。

### `/loop` 命令

```bash
# 明確的間隔
/loop 5m check if the deployment finished

# 自然語言
/loop check build status every 30 minutes
```

也支援標準的 5 欄位 cron 表達式以進行精確排程。

### 一次性提醒

設定在特定時間觸發一次的提醒：

```
remind me at 3pm to push the release branch
in 45 minutes, run the integration tests
```

### 管理排程任務

| 工具 | 說明 |
|------|-------------|
| `CronCreate` | 建立新的排程任務 |
| `CronList` | 列出所有啟用的排程任務。自 v2.1.136 起，輸出也包含限定詞（qualifier）與排程提示詞內容，因此您無需開啟就能稽核每個 cron 將執行的內容。 |
| `CronDelete` | 移除排程任務 |

**限制與行為**：
- 每個工作階段最多支援 **50 個排程任務**
- 工作階段範圍限制 — 工作階段結束時會清除
- 週期性任務會在 **3 天** 後自動過期
- 任務僅在 Claude Code 執行時觸發 — 若錯過觸發時間，不會補執行

### 行為細節

| 項目 | 細節 |
|--------|--------|
| **週期性抖動 (Recurring jitter)** | 最高達間隔的 10%（最大 15 分鐘） |
| **一次性抖動 (One-shot jitter)** | 在 :00/:30 邊界時最高達 90 秒 |
| **錯過觸發 (Missed fires)** | 不會補執行 — 若 Claude Code 未在執行則會跳過 |
| **持久性 (Persistence)** | 在重新啟動後不會保留 |

### Cloud 排程任務

使用 `/schedule` 來建立在 Anthropic 基礎設施上執行的 Cloud 排程任務：

```
/schedule daily at 9am run the test suite and report failures
```

Cloud 排程任務在重新啟動後仍會保留，且不需要 Claude Code 在本地端執行。

### 停用排程任務

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

> **`/schedule` 因 API key 層級而自動停用（v2.1.139）**：當設定了 `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN` 或 `apiKeyHelper` 的任何一項時，即使您同時以 claude.ai 登入，Cloud `/schedule` 也會靜默不可用。相同條件也會停用[遠端控制](#遠端控制-remote-control)、claude.ai MCP 連接器以及通知偏好設定。請取消設定 API key（或在 Pro/Max OAuth 層級執行）以使用 `/schedule`。本地端的 `CronCreate` 不受影響。

### 範例：監控部署

```
/loop 5m check the deployment status of the staging environment.
        If the deploy succeeded, notify me and stop looping.
        If it failed, show the error logs.
```

> **提示**：排程任務是工作階段範圍的。若需要能在重新啟動後持續執行的持久自動化，請改用 CI/CD 流水線、GitHub Actions 或桌面應用程式的排程任務。

---

## 權限模式

權限模式控制 Claude 在無需明確核准的情況下可以執行的動作。

### 可用的權限模式

| 模式 | 行為 |
|---|---|
| `manual` | 僅讀取檔案；所有其他動作皆會提出提示。於 v2.1.200 由 `default` 更名 — `default` 仍可作為別名使用 |
| `acceptEdits` | 讀取與編輯檔案；執行指令時會提出提示 |
| `plan` | 僅讀取檔案（研究模式，不進行編輯） |
| `auto` | 所有動作皆透過背景安全分類器檢查。需要符合資格的模型（在多數供應商上為 Opus 5、Sonnet 5、Opus 4.7/4.8 或 Fable 5）與供應商 — 所有方案皆可使用，請參閱 [Auto Mode](#auto-mode) |
| `bypassPermissions` | 所有動作，不進行權限檢查（危險） |
| `dontAsk` | 僅執行預先核准的工具；其他所有動作皆被拒絕 |

> **注意**：互動式預設模式已於 v2.1.200 從 `default` 更名為 **Manual**（涵蓋 CLI、`--help`、VS Code 與 JetBrains），且啟用時頁尾會出現灰色的 ⏸ 標記（v2.1.203）。`--permission-mode manual` 與 `--permission-mode default` 皆可使用，設定中的 `"defaultMode": "manual"` 與 `"defaultMode": "default"` 亦同。請注意設定鍵為 `permissions.defaultMode` — 並沒有 `permissions.mode` 鍵，因此以下範例使用標準寫法。

在 CLI 中使用 `Shift+Tab` 可以在模式之間切換。可以使用 `--permission-mode` 旗標或 `permissions.defaultMode` 設定來設定預設值。

> **計畫模式將 shell 指令交由分類器判斷（v2.1.218）**：當 [auto mode](#auto-mode) 可用且 `useAutoModeDuringPlan` 設定為開啟（預設即為開啟）時，分類器會在規劃期間審查 shell 指令，而不是提示您。核准的指令會執行，被拒絕的則會被阻斷。計畫模式仍會無條件阻斷檔案寫入。

自 v2.1.160 起，即使是 `acceptEdits`，在寫入 shell 啟動檔（`.zshenv`、`.zlogin`、`.bash_login`、`~/.config/git/`）以及會執行程式碼的建置設定檔（`.npmrc`、`.yarnrc*`、`bunfig.toml`、`.bazelrc`、`.pre-commit-config.yaml`、`.devcontainer/`…）之前也會提示，因為這些檔案可能導致非預期的指令執行。

> **`--dangerously-skip-permissions` 擴充的路徑覆蓋範圍（v2.1.121, v2.1.126）**：`--dangerously-skip-permissions` CLI 旗標（以及等效的 `bypassPermissions` 模式）現在針對更廣泛的允許清單繞過提示 — 包括 `.claude/skills/`、`.claude/agents/`、`.claude/commands/`、`.claude/`、`.git/`、`.vscode/` 以及 shell 設定檔。破壞性的移除指令（`rm -rf /` 等）在此模式下仍會提示（在 auto mode 中則改由分類器判斷 — 請參閱 [Auto Mode](#auto-mode)）。請將此旗標視為比以往更鋒利的工具；僅在一次性的沙盒環境中使用。

> **Windows shell 偵測（v2.1.120, v2.1.126）**：不再需要 Git for Windows / Git Bash。當 Git Bash 不存在時，Claude Code 使用 PowerShell 作為 shell 工具。自 v2.1.126 起，當 PowerShell 工具啟用時，PowerShell 為**主要** shell，偵測範圍涵蓋透過 Microsoft Store 安裝、無 PATH 的 MSI，或作為 `.NET global tool` 安裝的 PowerShell 7。

> **PowerShell 工具在 Windows 上對 Bedrock/Vertex/Foundry 預設啟用（v2.1.143）**：自 v2.1.143 起，PowerShell 工具在 Windows 上對 Bedrock、Vertex 與 Foundry 使用者**預設啟用**。Claude Code 以 `-ExecutionPolicy Bypass` 呼叫 PowerShell，因此即使系統原則為 `Restricted`，腳本也能執行。若要讓 Claude Code 遵守系統執行原則，請設定 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`。若要完全停用 PowerShell 工具，請設定 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`。

### 啟動方式

**鍵盤快捷鍵**：
```bash
Shift + Tab  # 在所有 6 種模式之間切換
```

**斜線命令**：
```bash
/plan                  # 進入 plan 模式
```

**CLI 旗標**：
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**設定**：
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 權限模式範例

#### Default 模式
Claude 會針對重大動作要求確認：

```
User: Fix the bug in auth.ts

Claude: I need to modify src/auth.ts to fix the bug.
The change will update the password validation logic.

Approve this change? (yes/no/show)
```

#### Plan 模式
在執行前審查實作計畫：

```
User: /plan Implement user authentication system

Claude: I'll create a plan for implementing authentication.

## Implementation Plan
[包含階段與步驟的詳細計畫]

Ready to proceed? (yes/no/modify)
```

#### Accept Edits 模式
自動接受檔案修改：

```
User: acceptEdits
User: Fix the bug in auth.ts

Claude: [在不詢問的情況下進行修改]
```

### 使用案例

**程式碼審查 (Code Review)**：
```
User: claude --permission-mode plan
User: Review this PR and suggest improvements

Claude: [讀取程式碼並提供回饋，但無法進行修改]
```

**結對程式設計 (Pair Programming)**：
```
User: claude --permission-mode default
User: Let's implement the feature together

Claude: [在每次變更前都會請求核准]
```

**自動化任務**：
```
User: claude --permission-mode acceptEdits
User: Fix all linting issues in the codebase

Claude: [自動接受檔案編輯而無需詢問]
```

---

## Headless 模式

列印模式 (`claude -p`) 允許 Claude Code 在無需互動式輸入的情況下執行，非常適合自動化與 CI/CD。這是非互動模式，用以取代舊有的 `--headless` 旗標。

### 什麼是列印模式？

列印模式可實現：
- 自動化腳本執行
- CI/CD 整合
- 批次處理
- 排程任務

### 以列印模式執行（非互動式）

```bash
# 執行特定任務
claude -p "Run all tests"

# 處理透過管道（pipe）傳入的內容
cat error.log | claude -p "Analyze these errors"

# CI/CD 整合（GitHub Actions）
- name: AI Code Review
  run: claude -p "Review PR"
```

### 其他列印模式使用範例

```bash
# 執行特定任務並擷取輸出
claude -p "Run all tests and generate coverage report"

# 使用結構化輸出
claude -p --output-format json "Analyze code quality"

# 使用來自 stdin 的輸入
echo "Analyze code quality" | claude -p "explain this"
```

### 範例：CI/CD 整合

**GitHub Actions**：
```yaml
# .github/workflows/code-review.yml
name: AI Code Review

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "Review this PR for:
            - Code quality issues
            - Security vulnerabilities
            - Performance concerns
            - Test coverage
            Output results as JSON" > review.json

      - name: Post Review Comment
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const review = JSON.parse(fs.readFileSync('review.json', 'utf8'));
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: JSON.stringify(review, null, 2)
            });
```

### 列印模式設定

列印模式 (`claude -p`) 支援多個用於自動化的旗標：

```bash
# 限制自主回合數
claude -p --max-turns 5 "refactor this module"

# 結構化 JSON 輸出
claude -p --output-format json "analyze this codebase"

# 搭配 schema 驗證
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "find bugs in this code"

# 停用工作階段持久性
claude -p --no-session-persistence "one-off analysis"
```

### 安全模式（疑難排解）

`--safe-mode`（以及 `CLAUDE_CODE_SAFE_MODE` 環境變數，例如 `CLAUDE_CODE_SAFE_MODE=1`）會在**停用所有自訂項目**的情況下啟動 Claude Code — CLAUDE.md、plugins、skills、hooks 與 MCP servers 全部都會關閉。

```bash
# 在停用所有自訂項目的情況下啟動
claude --safe-mode

# 透過環境變數達到相同效果
CLAUDE_CODE_SAFE_MODE=1 claude
```

這是一個疑難排解工具：當自訂設定造成問題時，以安全模式啟動，即可釐清問題出在您的設定還是 Claude Code 本身。

---

## 工作階段管理

有效管理多個 Claude Code 工作階段。

### 工作階段管理命令

| 命令 | 描述 |
|---------|-------------|
| `/resume` | 透過 ID 或名稱恢復對話 |
| `/rename` | 為當前工作階段重新命名 |
| `/fork [prompt]` | 將對話複製到一個新的獨立背景工作階段，並在此處繼續工作（v2.1.212+） |
| `/subtask <task>` | 產生一個繼承完整對話的分叉 subagent，並將其結果回報到此處（v2.1.212+） |
| `/branch [name]` | 在此時間點切換到對話的副本，同時保留原始對話 |
| `claude -c` | 繼續最近一次的對話 |
| `claude -r "session"` | 透過名稱或 ID 恢復工作階段 |

### 恢復工作階段

**繼續最後一次對話**：
```bash
claude -c
```

**恢復指定的命名工作階段**：
```bash
claude -r "auth-refactor" "finish this PR"
```

**重新命名當前工作階段**（在 REPL 內部）：
```
/rename auth-refactor
```

> **v2.1.212 更新**：在代理檢視（agent view）中輸入 `/resume`（不帶參數）現在會開啟過往工作階段的選擇器 — 包括已從可見清單中移除的工作階段 — 並將所選的工作階段作為背景工作階段恢復。

### 分叉與分支工作階段

有三個命令可以複製對話，它們的差別在於*副本在哪裡執行*：

`/subtask <task>` 會產生一個繼承完整對話的分叉 subagent，在您繼續工作的同時處理該任務 — 它在 `claude agents` 中有自己的一列，完成時會將結果回傳到您的對話：

```
/subtask Investigate why the auth tests are flaky
```

`/fork [prompt]` 則會將對話複製到一個新的**背景工作階段**。副本包含到目前為止的所有內容並獨立執行 — 不會有任何內容回傳到此對話：

```
/fork Try the OAuth approach end to end
```

若要自己切換到副本而非委派出去，請使用 `/branch [name]`，它會保留原始對話，並讓您可以用 `/resume` 回到原始對話：

```
/branch try-oauth-instead
```

> **注意**：`/fork` 與 `/subtask` 在 **v2.1.212** 中互換了角色。在 v2.1.161 之前，`/fork` 是 `/branch` 的別名；從 v2.1.161 到 v2.1.211，它會啟動一個分叉 subagent — 也就是現在由 `/subtask` 承擔的行為。當代理檢視關閉時，`/subtask` 無法使用，而 `/fork` 會保留分叉 subagent 的行為。

或從 CLI 分叉：
```bash
claude --resume auth-refactor --fork-session "try OAuth instead"
```

### 工作階段持久性

工作階段會自動儲存並可以被恢復：

```bash
# 繼續最後一次對話
claude -c

# 透過名稱或 ID 恢復特定工作階段
claude -r "auth-refactor"

# 恢復並分叉以進行實驗
claude --resume auth-refactor --fork-session "alternative approach"
```

### 用量上限自動繼續（v2.1.234）

自 **v2.1.234** 起，因 claude.ai 用量上限而被阻擋的工作階段，會在該上限重設後自動繼續 — 不需要手動重新提示。可在 `/config` 的「Continue automatically at usage limit」中切換此行為。

### 工作階段摘要 (v2.1.108)

當您在離開一段時間後回到工作階段時，Claude 可以顯示已完成事項的簡短摘要。對於停用了遙測功能的使用者（Bedrock、Vertex、Foundry 使用者），此功能預設為啟用。

> **OTEL 遙測 — 重新啟用意見回饋調查（v2.1.136+）**：擷取 OpenTelemetry 資料的組織可以透過設定 `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL=1` 來重新啟用 Anthropic 的工作階段品質調查。在 OTEL 部署中，此調查預設為關閉，因為它先前曾被重新導向而遠離遙測管線。

> **OTEL 遙測 — `assistant_response` 日誌事件（v2.1.193+）**：Claude Code 會發出 `claude_code.assistant_response` OpenTelemetry 日誌事件，其中包含模型的回應文字，讓 OTEL 管線能在既有的工具/事件遙測之外，一併擷取 Claude 所說的內容。

**控制摘要行為：**

```bash
/recap                                 # 手動觸發摘要
/config                                # 切換自動摘要的開啟/關閉
```

或透過環境變數：
```bash
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0 claude   # 停用摘要
CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1 claude   # 強制啟用摘要
```

---

## 跨工作階段訊息傳遞

> **於 v2.1.224 新增**，並持續擴充至 v2.1.239。可在 **macOS、Linux 以及（自 v2.1.239 起）Windows** 上使用。

過去各個工作階段彼此孤立。跨工作階段訊息傳遞讓一個 Claude Code 工作階段可以與另一個工作階段溝通 — 包括您其他機器上的工作階段與雲端工作階段 — 因此您可以把問題交給已經載入正確 context 的工作階段，而不必重新解釋一遍。

### 探索工作階段

`ListAgents` 會列出所有您可以傳送訊息的對象：您產生的 subagents、這台機器上的其他本地工作階段、您的雲端工作階段，以及（在 Remote Control 連線時）您其他機器上的工作階段。每一列都會標示其種類，自 v2.1.229 起，各列也會帶有 `offline` 與 `cloud` 標籤，讓您能分辨可連線的工作階段與休眠中的工作階段。

**每一列中的名稱就是位址** — 也就是您要傳送的對象。

### 傳送訊息

`SendMessage` 接受目標與訊息：

```text
SendMessage({ to: "<session name>", message: "What did you conclude about the retry logic?" })
```

自 v2.1.232 起，只需名稱即可 — 除非兩列確實同名，否則不再需要附加用於區分的參照。

### 等待工作階段閒置（`notify_when_idle`，v2.1.236）

當您要傳送訊息的工作階段正在執行任務時，您通常會想知道它何時完成，而不是反覆輪詢。`SendMessage` 為此提供了 `notify_when_idle` 輸入：

```text
SendMessage({
  to: "auth-refactor",
  message: "ping me when the migration finishes",
  notify_when_idle: true
})
```

它是**選擇性啟用且只觸發一次**的 — 目標工作階段會在下一次進入閒置狀態時傳送單一通知，之後訂閱即告結束。沒有輪詢迴圈，若該工作階段再次忙碌後又閒置，也不會重複通知。

v2.1.239 的兩項相關變更：`ListAgents` 現在除了即時的隊友之外，也會回報**工作階段本身的名稱**（讓工作階段可以告訴其他人如何找到它），而跨工作階段訊息傳遞也開始支援 **Windows**。

### `@` 提及簡寫（v2.1.232）

您可以不明確呼叫工具，而是在提示詞中直接 `@` 提及某個工作階段來傳送訊息：

```text
@auth-refactor did the migration tests pass?
```

### 控制接收內容：`crossSessionInbound`

傳入的訊息由 `crossSessionInbound` 設定（v2.1.224+）控制：

| 值 | 行為 |
|---|---|
| `"accept"` | 傳入的訊息會傳遞給此工作階段中的 Claude |
| `"hold"` | 您會看到有訊息抵達的通知，但訊息不會被傳遞 |
| `"refuse"` | 傳入的訊息會被丟棄 |

這些值構成一個階梯 — `accept < hold < refuse` — 且**專案與本機設定只有在比使用者範圍的值更嚴格時才會生效**。專案可以收緊傳入訊息的傳遞，但永遠無法放寬。自 v2.1.232 起，此設定在 `/config` 中也有對應的一列：「Messages from your other sessions」。

### 觸及範圍與限制

- 同一台機器上的本地工作階段，以及您的雲端工作階段。
- 您其他機器上的 Remote Control 工作階段，可透過名稱定址（v2.1.225）。
- 雲端工作階段可以**接收**您的訊息，但目前還無法回傳訊息給本地工作階段 — 請在其自身的對話紀錄中查看它的回答。
- 自 v2.1.224 起支援 macOS 與 Linux；自 v2.1.239 起支援 Windows。

---

## 互動功能

### 鍵盤快捷鍵

Claude Code 支援鍵盤快捷鍵以提升效率。以下是來自官方文件的完整參考：

| 快捷鍵 | 說明 |
|----------|-------------|
| `Ctrl+C` | 取消目前的輸入/生成 |
| `Ctrl+D` | 退出 Claude Code |
| `Ctrl+G` | 在外部編輯器中編輯計畫 |
| `Ctrl+L` | 重新繪製畫面（僅重繪 — 按兩下執行 `/clear` 的快捷方式已於 v2.1.238 移除） |
| `Ctrl+O` | 切換詳細輸出（查看推理過程） |
| `Ctrl+R` | 反向搜尋歷史紀錄。自 v2.1.129+ 起，預設搜尋**所有專案的所有提示詞**；在選擇器中按 `Ctrl+S` 可縮小至目前專案。舊版本預設為僅限目前專案。 |
| `Ctrl+T` | 切換任務清單檢視 |
| `Ctrl+B` | 背景執行任務 |
| `Esc+Esc` | 回溯程式碼/對話 |
| `Shift+Tab` / `Alt+M` | 切換權限模式 |
| `Option+P` / `Alt+P` | 切換模型 |
| `Option+T` / `Alt+T` | 切換延伸思考 |
| `Option+O` / `Alt+O` | 切換快速模式（`/fast`） |
| `Ctrl+X` `Ctrl+K` | 停止所有背景 subagents |
| `Ctrl+Enter`（或 `Ctrl+X` `Ctrl+S`） | 立即傳送 — 中斷目前的回合，並一次傳送所有佇列中的訊息。已傳送與佇列中的訊息在模型接收前會保持灰色（v2.1.275） |
| `Ctrl+S` | 暫存目前的提示詞；再按一次即可還原 |
| `Ctrl+_` | 復原提示詞輸入的上一次編輯 |
| `:` | 在單字開頭輸入 `:` 以開啟表情符號短碼補完，例如 `:heart:`（v2.1.217+） |

**行編輯（標準 readline 快捷鍵）：**

| 快捷鍵 | 動作 |
|----------|--------|
| `Ctrl + A` | 移動到行首 |
| `Ctrl + E` | 移動到行尾 |
| `Ctrl + K` | 剪下至行尾 |
| `Ctrl + U` | 剪下至行首 |
| `Ctrl + W` | 向後刪除單字 |
| `Ctrl + Y` | 貼上 (yank) |
| `Tab` | 自動補完 |
| `↑ / ↓` | 指令歷史紀錄 |

### 無障礙功能

螢幕閱讀器模式（v2.1.208+）會將 CLI 切換為專為螢幕閱讀器設計的純文字呈現模式。可以透過 CLI 旗標、環境變數或設定鍵啟用：

```bash
claude --ax-screen-reader
```

```bash
export CLAUDE_AX_SCREEN_READER=1
```

```json
{
  "axScreenReader": true
}
```

### 自訂鍵位綁定

透過執行 `/keybindings` 來建立自訂鍵盤快捷鍵，這將開啟 `~/.claude/keybindings.json` 進行編輯 (v2.1.18+)。

**設定格式**：

```json
{
  "$schema": "https://www.schemastore.org/claude-code-keybindings.json",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null,
        "ctrl+k ctrl+s": "chat:stash"
      }
    },
    {
      "context": "Confirmation",
      "bindings": {
        "ctrl+a": "confirmation:yes"
      }
    }
  ]
}
```

將綁定設定為 `null` 可解除預設快捷鍵的綁定。

### 可用的上下文

鍵位綁定範圍限定在特定的 UI 上下文：

| 上下文 | 關鍵動作 |
|---------|-------------|
| **Chat** | `submit`, `cancel`, `cycleMode`, `modelPicker`, `thinkingToggle`, `undo`, `externalEditor`, `stash`, `imagePaste` |
| **Confirmation** | `yes`, `no`, `previous`, `next`, `nextField`, `cycleMode`, `toggleExplanation` |
| **Global** | `interrupt`, `exit`, `toggleTodos`, `toggleTranscript` |
| **Autocomplete** | `accept`, `dismiss`, `next`, `previous` |
| **HistorySearch** | `search`, `previous`, `next` |
| **Settings** | 特定上下文的設定導覽 |
| **Tabs** | 分頁切換與管理 |
| **Help** | 說明面板導覽 |

總共有 18 個上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 以及 `Select`。

### 和弦（Chord）支援

鍵位綁定支援和弦序列（多鍵組合）：

```
"ctrl+k ctrl+s"   → 雙鍵序列：先按 ctrl+k，然後按 ctrl+s
"ctrl+shift+p"    → 同時按下修飾鍵
```

**按鍵語法**：
- **修飾鍵**：`ctrl`、`alt`（或 `opt`）、`shift`、`meta`（或 `cmd`）
- **大寫代表 Shift**：`K` 等同於 `shift+k`
- **特殊鍵**：`escape`、`enter`、`return`、`tab`、`space`、`backspace`、`delete`、方向鍵

### 保留與衝突的按鍵

| 按鍵 | 狀態 | 備註 |
|-----|--------|-------|
| `Ctrl+C` | 保留 | 無法重新綁定（中斷） |
| `Ctrl+D` | 保留 | 無法重新綁定（退出） |
| `Ctrl+B` | 終端機衝突 | tmux 前綴鍵 |
| `Ctrl+A` | 終端機衝突 | GNU Screen 前綴鍵 |
| `Ctrl+Z` | 終端機衝突 | 行程暫停 |

> **提示**：如果快捷鍵無法運作，請檢查是否與您的終端機模擬器或多工器（multiplexer）發生衝突。

### Tab 補全

Claude Code 提供智慧型的 tab 補全功能：

```
User: /rew<TAB>
→ /rewind

User: /plu<TAB>
→ /plugin

User: /plugin <TAB>
→ /plugin install
→ /plugin enable
→ /plugin disable
```

### 指令歷史紀錄

存取先前的指令：

```
User: <↑>  # 上一個指令
User: <↓>  # 下一個指令
User: Ctrl+R  # 搜尋歷史紀錄

(reverse-i-search)`test': run all tests
```

### 多行輸入

對於複雜的查詢，請使用多行模式：

```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

**範例：**

```
User: \
> Implement a user authentication system
> with the following requirements:
> - JWT tokens
> - Email verification
> - Password reset
> - 2FA support
> \end

Claude: [Processes the multi-line request]
```

### 行內編輯

在送出前編輯指令：

```
User: Deploy to prodcution<Backspace><Backspace>uction

[Edit in-place before sending]
```

### Vim 模式

啟用 Vi/Vim 按鍵綁定以進行文字編輯：

**啟動方式**：
- 透過 `/config`（切換「Editor / Vim mode」）或在 `~/.claude/settings.json` 中設定 `editorMode: "vim"` 來啟用。獨立的 `/vim` 斜線命令已移除（請參閱 [issue #43370](https://github.com/anthropics/claude-code/issues/43370)）；vim 模式現在由設定驅動。
- 使用 `Esc` 切換至 NORMAL 模式，使用 `i/a/o` 切換至 INSERT 模式，使用 `v` 切換至 VISUAL 模式，使用 `V` 切換至 VISUAL-LINE 模式（v2.1.118+）

**導覽按鍵**：
- `h` / `l` - 向左/向右移動
- `j` / `k` - 向下/向上移動
- `w` / `b` / `e` - 按單字移動
- `0` / `$` - 移動至行首/行尾
- `gg` / `G` - 跳至文件開頭/結尾

**文字物件（Text objects）**：
- `iw` / `aw` - 內含/包含單字
- `i"` / `a"` - 內含/包含引號字串
- `i(` / `a(` - 內含/包含括號

**Visual 模式（v2.1.118+）**：

| 按鍵 | 模式 | 行為 |
|-----|------|----------|
| `v` | Visual | 以字元為單位選取並顯示視覺回饋；用移動鍵延伸選取範圍 |
| `V` | Visual-line | 以行為單位選取；始終選取整行 |
| `y` | Yank | 複製目前的視覺選取範圍 |
| `d` / `x` | Delete | 刪除目前的視覺選取範圍 |
| `c` | Change | 刪除選取範圍並進入 INSERT 模式 |
| `Esc` | Exit | 返回 NORMAL 模式 |

Visual 選取範圍會在輸入欄位中以高亮顯示，讓您在提交操作前能清楚看到將被複製、刪除或變更的內容。

### Bash 模式

使用 `!` 前綴直接執行 shell 指令：

```bash
! npm test
! git status
! cat src/index.js
```

用於快速執行指令而無需切換上下文（context）。

**自 v2.1.193 起：** bash 模式（`!`）支援即時的檔案路徑自動補完，因此在輸入 shell 指令時不必離開提示詞就能補完路徑。

**自 v2.1.186 起：** `!` 指令的輸出現在會自動傳送給 Claude，Claude 會對其做出回應。若要保留先前僅將輸出加入 context 而不回應的行為，請在 `settings.json` 中設定 `"respondToBashCommands": false`。

---

## 輸出樣式 (Output Styles)

輸出樣式改變的是 Claude **如何**回應，而不是它知道什麼。它們會修改系統提示詞，以設定角色、語氣與預設的回應格式。當您每一回合都得重複要求相同的語氣，或希望 Claude 扮演軟體工程師以外的角色時，就可以使用它。

關於您的專案或程式碼庫的指示，請改用 [CLAUDE.md](../02-memory/) — 那是另一種機制，取捨也不同。

### 內建樣式

| 樣式 | 行為 |
|-------|----------|
| **Default** | 標準的系統提示詞，針對高效完成軟體工程任務進行調校 |
| **Proactive** | Claude 會立即執行並做出合理的假設，而不是為了例行決策停下來。比 auto mode 有更強的自主執行指引，但它**不會**改變您的權限模式 — 您仍會看到權限提示 |
| **Explanatory** | 在步驟之間加入教學性的「Insights」，解說實作選擇與程式碼庫模式 |
| **Learning** | 協作式的從做中學。Claude 會分享見解，*並且*留下 `TODO(human)` 標記，讓您自己實作一些小而關鍵的部分 |
| **Concise**（v2.1.237） | Claude 會先給出結果，並略過開場白與旁白。徹底程度不變 — 只省略答案周圍的鋪陳。可在 `/config` → Output style 中選擇，或設定 `"outputStyle": "Concise"` |

### 選擇樣式

執行 `/config` 並選擇 **Output style**，或執行 `/output-style <style>` 直接切換 — 不帶參數時會列出可用的樣式並標示目前使用的樣式。無論哪種方式，選擇都會儲存到 `.claude/settings.local.json`。若不想透過選單設定，可以直接編輯設定：

```json
{
  "outputStyle": "Explanatory"
}
```

> **注意**：獨立的 `/output-style` 命令曾於 v2.1.73 被棄用並在 v2.1.91 移除，但它**在 v2.1.269 回歸**，並在 v2.1.278 中可用。它可在 headless 與 Remote Control 工作階段中使用。`/config` 與 `outputStyle` 設定也仍然有效。

輸出樣式是系統提示詞的一部分，Claude Code 只會在工作階段開始時讀取一次 — 變更會在 `/clear` 之後或新的工作階段中生效。

### 自訂輸出樣式

自訂樣式是一個帶有 frontmatter 的 Markdown 檔案，可儲存在以下三個層級之一：

- 使用者：`~/.claude/output-styles/`
- 專案：`.claude/output-styles/`
- 受管理政策：管理設定目錄內的 `.claude/output-styles/`

專案樣式會從工作目錄到儲存庫根目錄之間的每個 `.claude/output-styles/` 載入。自 v2.1.178 起，當巢狀目錄定義了同名樣式時，最接近工作目錄的那一個優先。

```markdown
---
name: Diagrams first
description: Lead every explanation with a diagram
keep-coding-instructions: true
---

When explaining code, architecture, or data flow, start with a Mermaid diagram
showing the structure, then explain in prose.
```

| Frontmatter | 用途 | 預設值 |
|-------------|---------|---------|
| `name` | 樣式名稱（若與檔名不同） | 沿用檔名 |
| `description` | 顯示在 `/config` 選擇器中 | 無 |
| `keep-coding-instructions` | 保留 Claude Code 內建的軟體工程指示 | `false` |
| `force-for-plugin` | 僅限 plugin 樣式：只要啟用該 plugin 就自動套用，並覆蓋使用者的 `outputStyle` | `false` |

當您想改變 Claude 的溝通方式、但仍希望它以相同方式撰寫程式碼時，**請設定 `keep-coding-instructions: true`**。當 Claude 完全不從事軟體工程時 — 例如寫作助理或資料分析師 — 則省略此設定。

### 範圍與成本

輸出樣式**僅套用於主對話**。subagent 會執行自己的系統提示詞，因此樣式不會改變 subagents 的回應方式；fork 是例外，因為它會繼承父層的完整系統提示詞。

加入指示會增加輸入 token，不過在第一次請求之後，prompt 快取會吸收大部分的成本。Explanatory 與 Learning 在設計上會產生較長的回應，因此會增加輸出 token。

### 比較

| 功能 | 運作方式 | 適用時機 |
|---------|--------------|-------------|
| 輸出樣式 | 修改系統提示詞 | 您希望每一回合都使用不同的角色、語氣或格式 |
| [CLAUDE.md](../02-memory/) | 在系統提示詞之後加入一則使用者訊息 | Claude 應始終了解您的專案慣例 |
| `--append-system-prompt` | 附加到系統提示詞，不移除任何內容 | 單次呼叫的一次性補充 |
| [Subagents](../04-subagents/) | 以自己的系統提示詞、模型與工具執行 | 您需要一個範圍獨立的助手 |
| [Skills](../03-skills/) | 在呼叫時載入特定任務的指示 | 您有可重複使用的工作流程 |

---

## 狀態列 (Status Line)

狀態列是一個自訂命令，其輸出會顯示在工作階段底部。可使用 `/statusline` 設定，或直接設定：

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline.sh",
    "padding": 0
  }
}
```

`padding` 預設為 `0`。Claude Code 會透過 stdin 將一個 JSON 物件傳給該命令，由腳本決定要顯示的內容。

### 可用的輸入欄位

| 群組 | 欄位 |
|-------|--------|
| 工作階段 | `session_id`, `session_name`, `prompt_id`, `transcript_path`, `cwd`, `version` |
| 模型 | `model.id`, `output_style.name`, `effort.level`, `fast_mode`, `thinking.enabled` |
| 代理 | `agent.name`, `vim.mode` |
| 成本 | `cost.total_cost_usd`, `cost.total_duration_ms`, `cost.total_api_duration_ms`, `cost.total_lines_added` |
| Context | `context_window.context_window_size`, `.current_usage`, `.remaining_percentage`, `.total_input_tokens`, `.used_percentage` |
| 限制 | `rate_limits.five_hour.used_percentage`, `.resets_at` |
| 儲存庫 | `pr.number`, `pr.review_state`, `workspace.project_dir`, `workspace.added_dirs`, `workspace.git_worktree`, `workspace.repo.host` |
| Worktree | `worktree.name`, `.branch`, `.path`, `.original_branch`, `.original_cwd` |

### 範例

```bash
#!/bin/bash
# ~/.claude/statusline.sh — 模型、context 使用量與成本
input=$(cat)
model=$(echo "$input" | jq -r '.model.id')
used=$(echo "$input" | jq -r '.context_window.used_percentage')
cost=$(echo "$input" | jq -r '.cost.total_cost_usd')
printf '%s | ctx %.0f%% | $%.2f' "$model" "$used" "$cost"
```

> **注意**：`statusLine` 需要工作區信任。狀態列腳本也會在環境中收到 `COLUMNS` 與 `LINES`（v2.1.153+），以便依終端機大小調整輸出。

---

## TUI 模式（全螢幕）

> **v2.1.110 新增功能**

TUI（Text User Interface）模式會以全螢幕方式渲染 Claude Code 並提供無閃爍的輸出 — 非常適合 tmux 或 iTerm2 分割窗格等終端機多工工具。

### 啟用 TUI 模式

使用 `/tui` 指令切換 TUI 模式，或使用 `--tui` 旗標啟動：

```bash
/tui          # 在工作階段中切換
claude --tui  # 直接以 TUI 模式啟動
```

### 設定

| 設定 | 描述 | 預設值 |
|---------|-------------|---------|
| `autoScrollEnabled` | 自動捲動至最新訊息 | `true` |

透過 `/config` 或 `settings.json` 停用自動捲動：

```json
{
  "autoScrollEnabled": false
}
```

### 焦點檢視

`/focus` 指令會切換焦點檢視 — 這是一種無干擾的顯示方式，僅顯示最相關的輸出。`Ctrl+O` 現在僅在一般模式與詳細紀錄模式之間切換（焦點檢視為 `/focus`）。

---

## 語音聽寫

語音聽寫為 Claude Code 提供按住說話（push-to-talk）的語音輸入功能，讓您可以直接說出提示詞，而無需打字。

### 啟動語音聽寫

```
/voice
```

### 功能

| 功能 | 描述 |
|---------|-------------|
| **按住說話** | 按住按鍵進行錄音，放開按鍵進行傳送 |
| **20 種語言** | 語音轉文字支援 20 種語言 |
| **自訂快捷鍵** | 透過 `/keybindings` 設定按住說話的按鍵 |
| **帳戶需求** | 需要 Claude.ai 帳戶進行 STT 處理 |

### 設定

在您的快捷鍵檔案（`/keybindings`）中自訂按住說話的快捷鍵。語音聽寫使用您的 Claude.ai 帳戶進行語音轉文字處理。

---

## Channels

Channels 是一項研究預覽（Research Preview）功能，透過 MCP servers 將來自外部服務的事件推送到正在執行的 Claude Code 工作階段中。來源包括 Telegram、Discord、iMessage 以及任意的 webhooks，讓 Claude 無需進行輪詢（polling）即可對即時通知做出反應。

> **驗證（v2.1.128+）**：`--channels` 現在同時支援 Pro/Max OAuth **以及** API-key（console）驗證。舊版需要 OAuth。

### 訂閱 Channels

```bash
# 在啟動時訂閱 channel plugins
claude --channels discord,telegram

# 訂閱多個來源
claude --channels discord,telegram,imessage,webhooks
```

### 支援的整合

| 整合項目 | 說明 |
|-------------|-------------|
| **Discord** | 在您的工作階段中接收並回應 Discord 訊息 |
| **Telegram** | 在您的工作階段中接收並回應 Telegram 訊息 |
| **iMessage** | 在您的工作階段中接收 iMessage 通知 |
| **Webhooks** | 從任意 webhook 來源接收事件 |

### 設定

在啟動時使用 `--channels` 旗標來設定 channels。對於企業級部署，請使用管理設定來控制允許使用哪些 channel plugins：

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 管理設定用於控制整個組織中允許使用的 channel plugins。

### 運作原理

1. MCP servers 作為 channel plugins，負責連接至外部服務
2. 入站的訊息與事件會被推送到活動中的 Claude Code 工作階段
3. Claude 可以在工作階段的上下文（context）中讀取並回應訊息
4. Channel plugins 必須透過 `allowedChannelPlugins` 管理設定進行核准
5. 無需輪詢 — 事件會以即時方式推送

---

## Chrome Integration

Chrome Integration 將 Claude Code 連接到您的 Chrome 或 Microsoft Edge 瀏覽器，以進行即時網頁自動化與除錯。這是一項自 v2.0.73+ 起提供的 beta 功能（v1.0.36+ 已新增 Edge 支援）。

### 啟用 Chrome Integration

**啟動時**：

```bash
claude --chrome      # 啟用 Chrome 連線
claude --no-chrome   # 停用 Chrome 連線
```

**在工作階段中**：

```
/chrome
```

選擇「Enabled by default」以在未來所有的工作階段中啟用 Chrome Integration。Claude Code 會共享您的瀏覽器登入狀態，因此它可以與已驗證的網頁應用程式進行互動。

### 功能

| 功能 | 描述 |
|------------|-------------|
| **即時除錯** | 讀取主控台日誌、檢查 DOM 元素、即時除錯 JavaScript |
| **設計驗證** | 將渲染後的頁面與設計稿進行比對 |
| **表單驗證** | 測試表單提交、輸入驗證與錯誤處理 |
| **網頁應用程式測試** | 與已驗證的應用程式進行互動（如 Gmail、Google Docs、Notion 等） |
| **資料擷取** | 從網頁中抓取並處理內容 |
| **工作階段錄製** | 將瀏覽器互動行為錄製為 GIF 檔案 |

### 網站層級權限

Chrome 擴充功能管理各個網站的存取權限。您可以隨時透過擴充功能彈出視窗，授予或撤銷特定網站的存取權限。Claude Code 僅會與您明確允許的網站進行互動。

### 運作原理

Claude Code 在一個可見的視窗中控制瀏覽器 — 您可以即時觀察操作過程。當瀏覽器遇到登入頁面或 CAPTCHA 時，Claude 會暫停並等待您手動處理後再繼續。

### 已知限制

- **瀏覽器支援**：僅支援 Chrome 與 Edge — 不支援 Brave、Arc 及其他 Chromium 瀏覽器
- **WSL**：無法在 Windows Subsystem for Linux 中使用
- **第三方供應商**：不支援 Bedrock、Vertex 或 Foundry API 供應商
- **Service worker 閒置**：在長時間的工作階段期間，Chrome 擴充功能的 service worker 可能會進入閒置狀態

> **提示**：Chrome Integration 是一項 beta 功能。瀏覽器支援範圍可能會在未來的版本中擴展。

---

## 遠端控制 (Remote Control)

遠端控制讓您可以從手機、平板電腦或任何瀏覽器中，繼續操作在本地執行的 Claude Code 工作階段。您的本地工作階段會持續在您的機器上執行 — 任何內容都不會移至雲端。此功能適用於 Pro、Max、Team 與 Enterprise 方案 (v2.1.51+)。

遠端控制**已不再是研究預覽功能** — 此標籤已於 2026 年第 34 週移除。任何執行 `claude remote-control` 的機器現在都會以**裝置卡片**的形式出現在 Claude app 的 Code 分頁中，因此您可以直接從手機在該機器上啟動工作階段，而不必先在該機器上啟動工作階段再連線。

### 開始遠端控制

**透過 CLI**：

```bash
# 使用預設工作階段名稱啟動
claude remote-control

# 使用自訂名稱啟動
claude remote-control --name "Auth Refactor"
```

**在工作階段內部**：

```
/remote-control
/remote-control "Auth Refactor"
```

**可用參數（flags）**：

| 參數 | 說明 |
|------|-------------|
| `--name "title"` | 自訂工作階段標題，以便識別 |
| `--verbose` | 顯示詳細的連線日誌 |
| `--sandbox` | 啟用檔案系統與網路隔離 |
| `--no-sandbox` | 停用沙盒機制（預設） |

### 連線至工作階段

有三種方式可以從其他裝置進行連線：

1. **工作階段 URL** — 當工作階段啟動時會印在終端機上；請在任何瀏覽器中開啟該 URL
2. **QR code** — 啟動後按下 `spacebar` 即可顯示可掃描的 QR code
3. **透過名稱尋找** — 在 claude.ai/code 或 Claude 行動應用程式（iOS/Android）中瀏覽您的工作階段

### 安全性

- **無需開啟任何入站連接埠（inbound ports）** 在您的機器上
- **僅限出站 HTTPS** 透過 TLS 進行傳輸
- **範圍限制的憑證（Scoped credentials）** — 使用多個短暫且範圍狹窄的 token
- **工作階段隔離** — 每個遠端工作階段都是獨立的

### 遠端控制 vs Claude Code 網頁版

| 項目 | 遠端控制 | Claude Code 網頁版 |
|--------|---------------|-------------------|
| **執行** | 在您的機器上執行 | 在 Anthropic 雲端執行 |
| **本地工具** | 具有對本地 MCP servers、檔案與 CLI 的完整存取權 | 無本地相依性 |
| **使用情境** | 從其他裝置繼續進行本地工作 | 從任何瀏覽器開始全新的工作 |

### 限制

- 每個 Claude Code 實例僅限一個遠端工作階段
- 主機上的終端機必須保持開啟狀態
- 如果網路無法連線，工作階段將在約 10 分鐘後逾時

### 使用情境

- 當您不在座位旁時，透過行動裝置或平板電腦控制 Claude Code
- 在維持本地工具執行的同時，使用更豐富的 claude.ai UI
- 在移動中利用完整的本地開發環境進行快速程式碼審查（code reviews）

### 推送通知 (v2.1.110)

當遠端控制處於活動狀態，且在 `/config` 中啟用了「當 Claude 決定時進行推送（Push when Claude decides）」時，Claude 可以向您的手機發送行動推送通知 — 例如，當長時間任務完成或需要您的輸入時。

如何啟用：
1. 啟動遠端控制：`/remote-control` 或 `claude --rc`
2. 開啟 `/config` 並啟用 **Push when Claude decides**

推送通知需要 Claude 訂閱方案與 Claude 行動應用程式。

### 停用遠端控制（`disableRemoteControl`，v2.1.128+）

Team 或 Enterprise 方案的管理員可以透過 `disableRemoteControl` 設定完全封鎖遠端控制。設定為 `true` 時，`claude remote-control` 與 `/remote-control` 均拒絕啟動。

```json
{
  "disableRemoteControl": true
}
```

此設定在**管理/政策**範圍（例如 macOS 上的 `/Library/Application Support/ClaudeCode/managed-settings.json`）中生效，因此個別使用者無法覆蓋。當需要在整個組織強制執行僅限本地執行時非常有用。

> **遠端控制因 API key 層級而自動停用（v2.1.139）**：當設定了以下任何一項時，即使您同時以 claude.ai 登入，遠端控制也會**靜默停用**：
>
> - `ANTHROPIC_API_KEY`
> - `ANTHROPIC_AUTH_TOKEN`
> - `apiKeyHelper`（settings.json）
>
> 相同條件也會停用 [`/schedule`](#排程任務)、claude.ai MCP 連接器以及通知偏好設定 — 所有四個透過 claude.ai 橋接的介面都以 OAuth 登入作為有效憑證為前提。請取消設定 API key（或在 Pro/Max OAuth 層級執行）以使用這些功能。

---

## Web Sessions

Web Sessions 讓您可以在 claude.ai/code 直接於瀏覽器中執行 Claude Code，或從 CLI 建立 web sessions。

### 建立 Web Session

```bash
# 從 CLI 建立一個新的 web session
claude --remote "implement the new API endpoints"
```

這會在 claude.ai 上啟動一個 Claude Code 工作階段，您可以從任何瀏覽器進行存取。

### 在本地端恢復 Web Sessions

如果您在網頁上啟動了工作階段並想在本地端繼續執行：

```bash
# 在本地終端機中恢復網頁工作階段 — 會開啟您的網頁工作階段選擇器
claude --teleport
```

或者在互動式 REPL 中執行：

```text
/teleport
```

`/tp` 是 `/teleport` 的別名。兩者皆需要 claude.ai 訂閱。雲端工作階段會顯示 `/teleport` 提示，
說明如何在本地繼續（v2.1.223）。

> **來源為變更紀錄**：v2.1.223 的變更紀錄顯示了一種帶參數的形式，
> `claude --teleport <session id>`，可直接跳到已知的工作階段。CLI
> 參考文件只記載了不帶參數的選擇器形式，因此除非您手上已有工作階段 ID，
> 否則請優先使用 `claude --teleport`。

### 使用情境

- 在一台機器上開始工作，並在另一台機器上繼續
- 與團隊成員分享工作階段 URL
- 使用網頁 UI 進行視覺化 diff 審查，然後切換到終端機進行執行

---

## Desktop App

Claude Code Desktop App 提供了一個獨立的應用程式，具備視覺化 diff 審查、並行工作階段以及整合式連接器。適用於 macOS 與 Windows（包含 Pro、Max、Team 與 Enterprise 計畫）。

### 安裝

從 [claude.ai](https://claude.ai) 下載適用於您平台的版本：
- **macOS**：通用版本（支援 Apple Silicon 與 Intel）
- **Windows**：提供 x64 與 ARM64 安裝程式

請參閱 [Desktop Quickstart](https://code.claude.com/docs/en/desktop-quickstart) 以取得設定說明。

### 從 CLI 移轉

將您目前的 CLI 工作階段移轉至 Desktop App：

```
/desktop
```

### 核心功能

| 功能 | 說明 |
|---------|-------------|
| **Diff view** | 逐檔案的視覺化審查並附帶行內註解；Claude 會讀取註解並進行修訂 |
| **App preview** | 自動啟動開發伺服器，並透過內嵌瀏覽器進行即時驗證 |
| **PR monitoring** | 與 GitHub CLI 整合，可自動修復 CI 失敗，並在檢查通過時自動合併 |
| **Parallel sessions** | 在側邊欄中管理多個工作階段，並具備自動 Git worktree 隔離功能 |
| **Scheduled tasks** | 在應用程式開啟時執行的週期性任務（每小時、每日、工作日、每週） |
| **Rich rendering** | 具備語法高亮的程式碼、markdown 與圖表渲染；GitHub-Flavored-Markdown 的任務清單核取方塊（`- [ ]` / `- [x]`）會渲染為可勾選的核取方塊（v2.1.149+） |

### App preview 設定

在 `.claude/launch.json` 中設定開發伺服器的行為：

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### Connectors

連接外部服務以獲得更豐富的上下文：

| 連接器 | 功能 |
|-----------|------------|
| **GitHub** | PR 監控、issue 追蹤、程式碼審查 |
| **Slack** | 通知、頻道上下文 |
| **Linear** | Issue 追蹤、衝刺管理 |
| **Notion** | 文件、知識庫存取 |
| **Asana** | 任務管理、專案追蹤 |
| **Calendar** | 行程感知、會議上下文 |

> **注意**：遠端（雲端）工作階段無法使用連接器。

### Remote 與 SSH sessions

- **Remote sessions**：在 Anthropic 雲端基礎設施上執行；即使應用程式關閉也會持續執行。可從 claude.ai/code 或 Claude 行動應用程式存取。
- **SSH sessions**：透過 SSH 連接到遠端機器，並擁有對遠端檔案系統與工具的完整存取權。必須在遠端機器上安裝 Claude Code。

### Desktop 中的權限模式

Desktop App 支援與 CLI 相同的權限模式：

| 模式 | 行為 |
|------|----------|
| **Ask permissions**（預設） | 審查並核准每一次的編輯與指令 |
| **Auto accept edits** | 檔案編輯自動核准；指令需要手動核准 |
| **Plan mode** | 在進行任何變更前審查執行方案 |
| **Bypass permissions** | 自動執行（僅限沙盒環境，由管理員控制） |

### 企業級功能

- **Admin console**：控制組織內的 Code 頁籤存取權限與權限設定
- **MDM deployment**：透過 macOS 的 MDM 或 Windows 的 MSIX 進行部署
- **SSO integration**：要求組織成員使用單一登入（SSO）
- **Managed settings**：集中管理團隊設定與模型可用性

---

## Task List

Task List 功能提供持久性的任務追蹤，即使在 context compaction（當對話紀錄被修剪以符合 context window 時）發生後仍能保留。

### 切換 Task List

在工作階段期間，按下 `Ctrl+T` 可切換開啟或關閉 task list 檢視。

### 持久性任務

任務會在 context compaction 後持續存在，確保在對話 context 被修剪時，長時間執行的工作項目不會遺失。這對於複雜且多步驟的實作特別有用。

### 具名的 Task Directories

使用 `CLAUDE_CODE_TASK_LIST_ID` 環境變數來建立可在不同工作階段間共用的具名任務目錄：

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

這允許多個工作階段共用同一個 task list，使其適用於團隊工作流程或多工作階段專案。

---

## Prompt 建議

Prompt 建議會根據您的 git 紀錄與目前的對話上下文，顯示灰色的範例指令。

### 運作方式

- 建議會以灰色文字形式顯示在您的輸入提示詞下方
- 按下 `Tab` 鍵即可接受建議
- 按下 `Enter` 鍵即可接受並立即提交
- 建議具備上下文感知能力，會從 git 紀錄與對話狀態中提取資訊

### 停用 Prompt 建議

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git Worktrees

Git Worktrees 允許您在隔離的 worktree 中啟動 Claude Code，讓您能夠在不同的分支上進行平行工作，而無需進行 stash 或切換分支。

### 在 Worktree 中啟動

```bash
# 在隔離的 worktree 中啟動 Claude Code
claude --worktree
# 或
claude -w
```

### Worktree 位置

Worktrees 會建立在：
```
<repo>/.claude/worktrees/<name>
```

### Monorepos 的 Sparse Checkout

使用 `worktree.sparsePaths` 設定可以在 monorepos 中執行 sparse-checkout，從而減少磁碟使用量與 clone 時間：

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 基礎分支參考（`worktree.baseRef`）

**`worktree.baseRef`**（v2.1.133 新增）— 控制 `claude --worktree` 是從 `origin/<default>` 還是從本地 `HEAD` 建立分支。

- `"fresh"`（預設）— 從 `origin/<default-branch>` 建立分支，忽略本地未推送的 commit。**這還原了 v2.1.128 引入的行為**，因此在 v2.1.128 之後依賴本地 HEAD 分支的使用者必須重新選擇加入。
- `"head"` — 從本地 `HEAD` 建立分支，保留未推送的 commit。

在 `~/.claude/settings.json` 中設定：

```json
{ "worktree": { "baseRef": "head" } }
```

### 背景工作階段隔離（`worktree.bgIsolation`）

**`worktree.bgIsolation`**（v2.1.143 新增）— 控制背景工作階段（例如來自 `/bg`、`claude --bg` 或 Agent View）是否獲得各自的 worktree，或直接編輯前景工作目錄。

- *（預設）* — 背景工作階段會在 `<repo>/.claude/worktrees/` 下建立隔離的 worktree，與 `--worktree` 的方式相同。
- `"none"` — 背景工作階段直接編輯目前的工作目錄。當 worktrees 不切實際時（例如龐大的原生建置產物），或當背景代理必須與前景工作階段協調編輯時，請使用此設定。

```json
{ "worktree": { "bgIsolation": "none" } }
```

取捨：`"none"` 移除了 worktree 隔離的安全網 — 來自背景和前景工作階段的並行編輯可能在實際工作目錄中產生合併衝突。

### Worktree 工具與鉤子

| 項目 | 說明 |
|------|-------------|
| `EnterWorktree` | 進入 worktree 的工具；自 v2.1.157 起可在工作階段中途於 Claude 管理的 worktrees 之間切換 |
| `ExitWorktree` | 用於退出並清理目前 worktree 的工具 |
| `WorktreeCreate` | 當建立 worktree 時觸發的鉤子事件 |
| `WorktreeRemove` | 當移除 worktree 時觸發的鉤子事件 |

自 v2.1.157 起，Claude 管理的 worktrees 在代理完成時會保持未鎖定，因此 `git worktree remove`/`prune` 可以清理它們。

### 自動清理

如果 worktree 中沒有進行任何變更，它會在工作階段結束時自動進行清理。

### 使用案例

- 在開發功能分支的同時，保持主分支不受影響
- 在隔離環境中執行測試，而不影響工作目錄
- 在可拋棄的環境中嘗試實驗性變更
- 在 monorepos 中針對特定套件進行 sparse-checkout 以加快啟動速度

---

## Sandboxing

Sandboxing 為 Claude Code 執行的 Bash 命令提供作業系統層級的檔案系統與網路隔離。這與權限規則互補，並提供額外的安全層。

### 啟用 Sandboxing

**斜線命令**：
```
/sandbox
```

**CLI 旗標**：
```bash
claude --sandbox       # 啟用 sandboxing
claude --no-sandbox    # 停用 sandboxing
```

### 設定選項

| 設定 | 說明 |
|---------|-------------|
| `sandbox.enabled` | 啟用或停用 sandboxing |
| `sandbox.failIfUnavailable` | 若無法啟動 sandboxing 則失敗 |
| `sandbox.filesystem.allowWrite` | 允許寫入存取的路徑 |
| `sandbox.filesystem.allowRead` | 允許讀取存取的路徑 |
| `sandbox.filesystem.denyRead` | 禁止讀取存取的路徑 |
| `sandbox.network.allowedDomains` | Bash 啟動的行程允許連線的網域（支援 `*.` 萬用字元） |
| `sandbox.network.deniedDomains` | 即使 `allowedDomains` 萬用字元允許也應封鎖的網域（v2.1.113+） |
| `sandbox.network.strictAllowlist` | （v2.1.219）對沙盒化指令直接拒絕不在允許清單中的主機，不再提示 |
| `sandbox.enableWeakerNetworkIsolation` | 在 macOS 上啟用較弱的網路隔離 |
| `sandbox.bwrapPath` | （v2.1.133+，Linux/WSL）`bubblewrap` 二進位檔的路徑。預設：`$PATH` 查找。 |
| `sandbox.socatPath` | （v2.1.133+，Linux/WSL）`socat` 二進位檔的路徑。預設：`$PATH` 查找。 |
| `sandbox.credentials` | （v2.1.187+）阻止沙盒化指令讀取憑證檔案與機密環境變數。 |
| `sandbox.allowAppleEvents` | （v2.1.181+，macOS）選擇讓沙盒化指令可以傳送 Apple Events。 |
| `sandbox.filesystem.disabled` | （v2.1.216+）完全略過檔案系統隔離，同時維持網路隔離 — 適用於檔案沙盒會破壞工具鏈、但仍必須維持網路出口控管的情況。僅在使用者設定、管理設定或 `--settings` 中生效；專案設定無法設定此項。 |

**Linux/WSL 二進位路徑**（v2.1.133+）— 指向非標準安裝位置：

```json
{
  "sandbox": {
    "bwrapPath": "/opt/bubblewrap/bin/bwrap",
    "socatPath": "/opt/socat/bin/socat"
  }
}
```

`deniedDomains` 覆蓋廣泛萬用字元的範例（v2.1.113+）：

```json
{
  "sandbox": {
    "network": {
      "allowedDomains": ["*.example.com"],
      "deniedDomains": ["evil.example.com"]
    }
  }
}
```

萬用字元允許 `example.com` 上的所有連線，但 `deniedDomains` 仍會封鎖指定的主機名稱。

> **注意**（v2.1.243）：沙盒化 Bash 工具的權限提示**不再列出允許的網路主機**。Claude 會直接嘗試請求，而您在每個新主機出現時逐一核准 — 因此不要期待提示會事先顯示允許清單。同一版本也修正了當被阻擋的指令剛好以 `0` 結束時會遺失網路違規細節的問題，因此看似靜默成功的情況現在仍會回報被阻擋的內容。

### 憑證遮罩（v2.1.221, v2.1.224）

> **來源為變更紀錄**：這些 `sandbox.credentials` 選項來自 v2.1.221 與
> v2.1.224 的變更紀錄項目；設定參考文件尚未詳細說明。

在 v2.1.221 之前，`sandbox.credentials` 只能對憑證檔案執行 `deny` — 需要該憑證的沙盒化
指令就會直接失敗。`mode: "mask"` 讓指令能繼續運作而不暴露機密：沙盒化的行程讀取的是
該檔案的**哨兵（sentinel）**副本，而沙盒代理伺服器會在請求送往網路時替換為真實的值。

```json
{
  "sandbox": {
    "network": { "tlsTerminate": true },
    "credentials": {
      "files": [
        { "path": "~/.aws/credentials", "mode": "mask" }
      ]
    }
  }
}
```

| 功能 | 起始版本 | 作用 |
|---|---|---|
| 憑證**檔案**的 `mode: "mask"` | v2.1.221 | 沙盒化指令讀取哨兵值；代理伺服器在對外傳出時換成真實值。**僅限 Linux 與 WSL** — 在 macOS 上，檔案遮罩會退回為 `deny`。 |
| `extract` / `onExtractNoMatch` | v2.1.224 | 遮罩結構化環境變數值中的單一欄位，而非整個變數，並決定模式不相符時的處理方式。 |
| `decode: "jwt"` 搭配 `maskClaims` | v2.1.224 | 解碼 JWT 並只遮罩指定的 claims，其餘部分保持可讀。 |
| `awsPairs` / `sigv4` | v2.1.224 | 在代理伺服器替換為真實的 access key 後，重新簽署 AWS SigV4 請求。 |

**兩個容易忽略的限制：**

- 所有遮罩都需要 `network.tlsTerminate` — 代理伺服器必須能看到請求內容，
  才能替換值。
- 這些選項**僅**在使用者設定、管理設定或 `--settings` 中生效。
  專案設定無法開啟遮罩，也無法變更遮罩的內容。

### 設定範例

```json
{
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "filesystem": {
      "allowWrite": ["/Users/me/project"],
      "allowRead": ["/Users/me/project", "/usr/local/lib"],
      "denyRead": ["/Users/me/.ssh", "/Users/me/.aws"]
    },
    "enableWeakerNetworkIsolation": true
  }
}
```

### 運作原理

- Bash 命令在具有受限檔案系統存取權限的沙盒環境中執行
- 網路存取可以被隔離，以防止非預期的外部連線
- 與權限規則並行，實現深度防禦
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 進行網路限制（macOS 不支援完全的網路隔離）

### 使用情境

- 安全地執行不受信任或自動產生的程式碼
- 防止對專案範圍外的檔案進行意外修改
- 在自動化任務期間限制網路存取

---

## 管理設定（企業版）

管理設定讓企業管理員能夠使用平台原生的管理工具，在整個組織中部署 Claude Code 設定。

### 部署方式

| 平台 | 方式 | 自從 |
|----------|--------|-------|
| macOS | 管理式 plist 檔案（MDM） | v2.1.51+ |
| Windows | Windows 登錄檔 | v2.1.51+ |
| 跨平台 | 管理式設定檔案 | v2.1.51+ |
| 跨平台 | 管理式 drop-ins（`managed-settings.d/` 目錄） | v2.1.83+ |

### 管理式 Drop-ins

自 v2.1.83 起，管理員可以將多個管理式設定檔案部署到 `managed-settings.d/` 目錄中。檔案會按字母順序進行合併，從而實現跨團隊的模組化設定：

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 可用的管理設定

| 設定 | 說明 |
|---------|-------------|
| `disableBypassPermissionsMode` | 防止使用者啟用繞過權限模式 |
| `availableModels` | 限制使用者可以選擇的模型 |
| `enforceAvailableModels` | （v2.1.175）設為 `true` 時，`availableModels` 允許清單*也*會限制**預設**模型 — 若設定的預設模型不在清單中，Claude Code 會退回使用第一個允許的模型。使用者與專案設定無法再放寬受管理的 `availableModels` 清單。 |
| `allowedChannelPlugins` | 控制允許使用哪些 channel plugins |
| `autoMode.environment` | 為 auto mode 設定受信任的基礎設施 |
| `workflowSizeGuideline` | （v2.1.219）設定建議性的[動態工作流程規模準則](#動態工作流程-dynamic-workflows)。可從任何設定檔讀取，不限於管理設定；只要有任一設定檔設定了它，**Dynamic workflow size** 列就會從 `/config` 中隱藏 |
| `wslInheritsWindowsSettings` | 僅限 Windows/WSL（v2.1.118+）：設為 `true` 時，在 WSL 內執行的 Claude Code 會繼承 Windows 主機的管理設定，使透過 Registry/MDM 部署的企業政策在 Windows 與 WSL shell 之間統一適用 |
| `parentSettingsBehavior` | （v2.1.133+，管理員層級）控制 SDK 的 `managedSettings` 如何與父行程設定合併。`"first-wins"` 維持現有優先順序（衝突時較早的設定優先）；`"merge"` 進行深度合併。 |
| 自訂政策 | 組織特定的權限與工具政策 |

### 範例：macOS Plist

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>disableBypassPermissionsMode</key>
  <true/>
  <key>availableModels</key>
  <array>
    <string>claude-sonnet-4-6</string>
    <string>claude-haiku-4-5</string>
  </array>
</dict>
</plist>
```

---

## 設定與設定選項

### 設定檔位置

1. **全域設定**：`~/.claude/config.json`
2. **專案設定**：`./.claude/config.json`
3. **使用者設定**：`~/.config/claude-code/settings.json`

### 完整設定範例

**核心進階功能設定：**

```json
{
  "permissions": {
    "defaultMode": "manual"
  },
  "hooks": {
    "PreToolUse:Edit": "eslint --fix ${file_path}",
    "PostToolUse:Write": "~/.claude/hooks/security-scan.sh"
  },
  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"]
      }
    }
  }
}
```

**擴充設定範例：**

```json
{
  "permissions": {
    "defaultMode": "manual",
    "allowedTools": ["Bash(git log:*)", "Read"],
    "disallowedTools": ["Bash(rm -rf:*)"]
  },

  "hooks": {
    "PreToolUse": [{ "matcher": "Edit", "hooks": ["eslint --fix ${file_path}"] }],
    "PostToolUse": [{ "matcher": "Write", "hooks": ["~/.claude/hooks/security-scan.sh"] }],
    "Stop": [{ "hooks": ["~/.claude/hooks/notify.sh"] }]
  },

  "mcp": {
    "enabled": true,
    "servers": {
      "github": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-github"],
        "env": {
          "GITHUB_TOKEN": "${GITHUB_TOKEN}"
        }
      }
    }
  }
}
```

### 其他個人設定

這些鍵放在 `~/.claude/settings.json`（或專案的 `.claude/settings.json`）中，用於控制個別使用者的互動行為：

| 設定 | 說明 |
|---------|-------------|
| `askUserQuestionTimeout` | 在閒置一段時間後，自動繼續未回答的 `AskUserQuestion` 對話框。自 **v2.1.200** 起，對話框預設不再自動繼續 — 設定此項即可重新選擇定時自動繼續。 |
| `enableArtifact` | 個別使用者啟用/停用 Artifact 工具（v2.1.196）。 |
| `crossSessionInbound` | （v2.1.224）如何處理傳入的[跨工作階段訊息](#跨工作階段訊息傳遞) — `"accept"`、`"hold"` 或 `"refuse"`。專案與本機的值只有在 `accept < hold < refuse` 階梯上*更嚴格*時才會生效。自 v2.1.232 起在 `/config` 中顯示為「Messages from your other sessions」。 |
| `dialogExpiry` | （v2.1.224）未回答的對話框保持開啟的時間。預設 `"5m"`；接受 `"60s"`、`"5m"`、`"10m"` 或 `"never"`。會被 `CLAUDE_CODE_USER_DIALOG_TIMEOUT_MS` 覆蓋。自 v2.1.232 起在 `/config` 中顯示為「Dialog expiry」。 |
| `modelPicker` | （v2.1.243）選擇 `/model` 選擇器要列出哪些模型，並使用您自己的順序與標籤。這是少數在各設定層之間**取代而非合併**的設定之一 — 最近範圍的值直接勝出。 |
| `promptCacheTtl` | （v2.1.243）選擇主對話的 prompt 快取存留時間。 |
| `subagentPromptCacheTtl` | （v2.1.243）針對 subagents 以及主對話以外的其他請求做相同的選擇。 |
| `modelPricing` | （v2.1.243）**管理設定。** 提供您組織的合約費率，讓 `/cost`、狀態列與遙測回報這些費率，而非牌價。 |
| `keybindingFlavor` | **自 v2.1.261 起已棄用且沒有作用。** 提示詞的單字編輯按鍵一律遵循 readline 慣例，與 Bash 相同：`Ctrl+W` 向後刪除到空白處，`Alt+F` 與 `Alt+D` 停在單字結尾，標點符號會分隔單字。Claude Code 仍接受此鍵，因此設定了它的設定檔仍然有效。（在 v2.1.238–v2.1.260 中，它用於在 `"classic"` 與 `"readline"` 之間選擇。） |
| `spellcheck` | （v2.1.235）使用 `PATH` 上可找到的 `aspell`、`hunspell` 或 `ispell`（依此順序嘗試），在提示詞輸入中為拼錯的單字加上底線。值為物件 — `{"enabled": true, "language": "en_GB"}` — 且預設關閉。**僅從使用者設定、`--settings` 旗標與管理設定讀取**：專案 `.claude/settings.json` 或 `.claude/settings.local.json` 中的 `spellcheck` 區塊會被忽略。 |
| `bashOutputMaxChars` | （v2.1.261）Claude 以內嵌方式接收的**成功** Bash 或 PowerShell 指令輸出的字元數，最多 128K。超過上限時，Claude Code 會將輸出儲存到檔案，Claude 只會收到簡短預覽與檔案路徑。設定此項後，Claude Code 會忽略 `BASH_MAX_OUTPUT_LENGTH`。 |
| `taskOutputMaxChars` | （v2.1.261）**已於 v2.1.278 被取代且沒有作用。** 它過去用於限制 Claude 以 `TaskOutput` 工具讀取**背景任務**輸出時，以內嵌方式接收的字元數。v2.1.278 移除了 `TaskOutput` 工具 — Claude 現在改用 `Read` 讀取背景任務的輸出檔案 — 因此此鍵與 `TASK_MAX_OUTPUT_LENGTH` 都已失效。Claude Code 仍接受此鍵，因此設定了它的設定檔仍然有效。 |
| `maxEffortLevel` | （v2.1.267）限制 Claude Code 可使用的努力程度上限，適用於所有供應商，包括 Bedrock、Vertex 與 Foundry。可在頂層設定，或在 `modelSettings` 下針對個別模型設定。使用者仍可選擇較低的等級。 |
| `bashEditDiffEnabled` | （v2.1.269）Bash 工具的結果會包含該指令所變更檔案的 diff。 |
| `syncClaudeAiSkills` / `syncClaudeAiPlugins` | （v2.1.275）將任一項設為 `false`，即可選擇不將您在 claude.ai 帳戶上啟用的 skills 或 plugins 同步到終端機工作階段。 |

### 後備模型（`fallbackModel`）

`fallbackModel` 設定讓您最多可以設定**三個**後備模型，當主要模型過載或無法使用時依序嘗試。

```json
{
  "fallbackModel": ["claude-opus-4-8", "claude-sonnet-4-6", "claude-haiku-4-5"]
}
```

自 **v2.1.166** 起，`--fallback-model` 旗標也適用於互動式工作階段（而不僅是 headless）。發生後備時，Claude Code 會對非預期的不可重試錯誤重試一次；驗證、速率限制、請求大小與傳輸錯誤仍會立即失敗。

### 環境變數

使用環境變數覆蓋設定：

```bash
# 模型選擇
export ANTHROPIC_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_MODEL=claude-opus-4-8   # (v2.1.236) 新工作階段啟動時使用的模型。與 ANTHROPIC_MODEL 不同，/model 的選擇仍會覆蓋它 — 且該選擇會在重新啟動後保留
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-8
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 設定
export ANTHROPIC_API_KEY=sk-ant-...

# 思考設定
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high   # low, medium, high, xhigh (Opus 5/4.8/4.7), 或 max — Opus 5 與 Opus 4.8 的預設為 high（支援 Opus 5、Opus 4.8、Opus 4.7、Opus 4.6、Sonnet 4.6）

# 功能開關
export CLAUDE_CODE_DISABLE_AUTO_MEMORY=true
export CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=true
export CLAUDE_CODE_DISABLE_CRON=1
export CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS=true
export CLAUDE_CODE_DISABLE_TERMINAL_TITLE=true
export CLAUDE_CODE_DISABLE_1M_CONTEXT=true
export CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK=true
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
export CLAUDE_CODE_ENABLE_TASKS=true
export CLAUDE_CODE_SIMPLE=true              # 由 --bare 旗標設定

# MCP 設定
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# Prompt 快取
export ENABLE_PROMPT_CACHING_1H=1      # 使用 1 小時的 prompt 快取 TTL（預設為 5 分鐘）

# 任務管理
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# 代理團隊（實驗性）
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# 子代理與外掛設定
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=1

# 子行程與串流
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000

# 輸出與套件管理員（v2.1.129+）
export CLAUDE_CODE_FORCE_SYNC_OUTPUT=1                      # 強制同步輸出，適用於自動偵測失效的終端機（Emacs eat 等）
export CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1            # 啟用 Homebrew/WinGet 安裝的背景升級
export CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1         # 設定 ANTHROPIC_BASE_URL 時選擇加入 /v1/models gateway 探索

# Windows PowerShell 工具（v2.1.143+）— 在 Windows 上對 Bedrock/Vertex/Foundry 預設啟用
export CLAUDE_CODE_USE_POWERSHELL_TOOL=0                    # 完全停用 PowerShell 工具
export CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1    # 遵守系統 ExecutionPolicy 而非 `-ExecutionPolicy Bypass`

# 工作負載身份聯合（v2.1.141+）
export ANTHROPIC_WORKSPACE_ID=ws_abc123                     # 當規則涵蓋多個工作區時，將聯合 token 範圍限定至特定工作區

# Stop hook 安全上限（v2.1.143+）
export CLAUDE_CODE_STOP_HOOK_BLOCK_CAP=8                    # 工作階段因連續 Stop-hook 阻斷而結束前的最大次數。設為 0 可停用上限。

# 工作階段層級的產生上限（v2.1.212）
export CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION=200         # 每個工作階段 WebSearch 工具呼叫次數的上限，用以阻止失控的搜尋迴圈。預設 200。

# 無障礙功能（v2.1.208）
export CLAUDE_AX_SCREEN_READER=1                            # 啟用純文字螢幕閱讀器呈現模式。效果等同 --ax-screen-reader 或設定中的 "axScreenReader": true。

# 較新的變數（v2.1.221–v2.1.234）— 來源為變更紀錄；CLI 參考文件沒有環境變數章節
export CLAUDE_CODE_ENABLE_TODO_TOOLS=1                      # (v2.1.233) 恢復待辦/任務追蹤工具（TaskCreate/Get/Update/List、TodoWrite），這些工具預設僅在 Claude 3.x 模型、Opus 4 至 4.7、Sonnet 4 至 4.6 以及 Haiku 4.5 上可用（v2.1.268）
export CLAUDE_CODE_WEBFETCH_CACHE_TTL_MS=900000             # (v2.1.233) WebFetch URL 快取 TTL。預設 15 分鐘。
export CLAUDE_CODE_TOOL_MEMORY_LIMIT=2G                     # (v2.1.233, Linux) 選擇性套用於 Bash 指令的記憶體 cgroup
export ANTHROPIC_BEDROCK_REGION_PREFIX=us                   # (v2.1.224) 偏好使用特定的 Bedrock 跨區域推論設定檔
export CLAUDE_CODE_DISABLE_UNKNOWN_MODEL_WINDOW_ENFORCEMENT=1  # (v2.1.223) 對無法識別的模型 ID 恢復 v2.1.223 之前的自動壓縮行為
export CLAUDE_CODE_WORKFLOW_PREFIX_STAGGER_MS=0             # (v2.1.229) 停用動態工作流程扇出時的前綴錯開
export CLAUDE_CODE_USER_DIALOG_TIMEOUT_MS=300000            # (v2.1.224) 覆蓋 dialogExpiry 設定
export CLAUDE_CODE_PROJECT_DIR_NAME=my-app                  # (v2.1.234) 每個專案對話紀錄目錄的簡短名稱，適用於為每個工作階段提供獨立設定目錄的主機
export CLAUDE_CODE_GOAL_CHECKIN_MINUTES=30                  # (v2.1.234) /goal 啟用期間，背景任務停滯多少分鐘後 Claude 會主動確認進度。設為 0 可停用。

# 較新的變數（v2.1.268-v2.1.278）- 來源為變更紀錄
export CLAUDE_CODE_MCP_STARTUP_WAIT_MS=5000                 # (v2.1.274) 第一個非互動回合等待仍在連線中的 MCP servers 的時間。0 = 不等待。
export CLAUDE_CODE_WEBFETCH_DEADLINE_MS=300000              # (v2.1.268) 覆蓋 WebFetch 新的 300 秒截止期限。0 會停用截止期限。
export CLAUDE_CODE_WORKFLOW_MAX_CONCURRENT_AGENTS=32        # (v2.1.269) 提高 Workflow 工具每次執行的並行代理上限。接受 1-256。
export CLAUDE_CODE_AUTO_MODE_SERVER=0                       # (v2.1.278) 選擇不在 Bedrock、Vertex、Foundry 與 gateways 上使用伺服器端的 auto-mode 檢查
```

> **v2.1.223 — `CLAUDE_CODE_DISABLE_1M_CONTEXT` 適用範圍擴大**：此變數現在會透過自動壓縮，將**所有**
> 具備原生 1M-token window 的 Claude 模型限制在 200K，而非只針對固定的模型 ID 清單。

> **v2.1.108**：`ENABLE_PROMPT_CACHING_1H=1` — 使用 1 小時的 prompt 快取 TTL，而非預設的 5 分鐘 TTL。這能減少在長時間且穩定的工作階段中的快取失效。（v2.1.129 修復了 1 小時 TTL 被靜默降級為 5 分鐘的退化問題。）

> **v2.1.129**：`CLAUDE_CODE_FORCE_SYNC_OUTPUT=1` 強制同步輸出，適用於功能自動偵測失效的終端機（例如 Emacs `eat`）。`CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE=1` 啟用 Homebrew/WinGet 安裝的背景升級，這些安裝方式原本不會自動更新。

### 設定管理命令

```
User: /config
[開啟互動式設定選單]
```

`/config` 命令提供一個互動式選單，用於切換以下設定：
- 開啟/關閉進階思考（Extended thinking）
- 詳細輸出（Verbose output）
- 權限模式（Permission mode）
- 模型選擇
- 動態工作流程規模（v2.1.219）— 當設定檔中設定了 `workflowSizeGuideline` 時會隱藏，請參閱[動態工作流程](#動態工作流程-dynamic-workflows)

在互動式選單中，按 Enter 或空白鍵可變更所選的設定，按 Esc 則會儲存並關閉（v2.1.183+）。

您也可以不開啟選單，直接從提示詞設定某項設定：

```bash
/config thinking=false      # 以行內方式設定單一設定（v2.1.181+）
/config --help              # 列出可用的簡寫鍵（v2.1.183+）
```

`key=value` 簡寫可在互動式工作階段、`-p` 以及 Remote Control 中使用。

### 專案特定設定

在您的專案中建立 `.claude/config.json`：

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "defaultMode": "manual"
  },
  "mcp": {
    "servers": {
      "project-db": {
        "command": "mcp-postgres",
        "env": {
          "DATABASE_URL": "${PROJECT_DB_URL}"
        }
      }
    }
  }
}
```

---

## 信任與權限範圍

> **來源為變更紀錄（v2.1.222, v2.1.232）**：這些收緊措施來自變更紀錄；
> 設定參考文件尚未明確說明。

近期版本反覆出現的主題：與安全相關的設定不再能被您複製下來的儲存庫放寬。
以下是三項需要了解的變更。

**巢狀儲存庫需要各自的信任確認（v2.1.232）。** 位於受信任父目錄中的 git 儲存庫
不再繼承該信任。如果您信任 `~/work/monorepo`，而其中包含一個 vendored submodule，
那麼 Claude Code 第一次在該 submodule 中工作時，會要求您另外信任它。

**`sandbox.ripgrep` 僅限使用者範圍（v2.1.232）。** 指定沙盒所使用之 ripgrep
執行檔的設定，只會從使用者設定、管理設定或 `--settings` 讀取。
專案設定無法再將沙盒指向其他執行檔。

**Remote Control 自動啟動僅限使用者範圍（v2.1.222）。** 儲存庫本地的設定無法
啟用 Remote Control 自動啟動；它只能在使用者範圍透過 `/config` 開啟。

需要內化的原則：如果某個設定會讓簽入版控的檔案擴大 Claude Code 在您機器上
被允許做的事，請假設它現在僅限使用者範圍。

---

## Agent Teams

Agent Teams 是一項實驗性功能，可讓多個 Claude Code 實例協作完成一項任務。此功能預設為停用。

### 啟用 Agent Teams

透過環境變數或設定來啟用：

```bash
# 環境變數
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

或者加入到您的設定 JSON 中：

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
}
```

### Agent Teams 如何運作

- **團隊領導者（team lead）** 負責協調整體任務，並將子任務分配給隊友
- **隊友（Teammates）** 獨立工作，每個人都有各自的上下文視窗（context window）
- **共享任務清單（shared task list）** 讓團隊成員之間能夠進行自我協調
- 使用子代理定義（`.claude/agents/` 或 `--agents` 旗標）來定義隊友的角色與專業領域

### 顯示模式

Agent Teams 支援兩種顯示模式，透過 `--teammate-mode` 旗標進行設定：

| 模式 | 說明 |
|------|-------------|
| `in-process`（預設） | 隊友在同一個終端機行程內執行 |
| `tmux` | 每位隊友擁有一個專屬的分隔窗格（需要 tmux 或 iTerm2） |
| `auto` | 自動選擇最佳的顯示模式 |

```bash
# 使用 tmux 分隔窗格來顯示隊友
claude --teammate-mode tmux

# 明確使用 in-process 模式
claude --teammate-mode in-process
```

### 使用情境

- 大型重構任務，由不同的隊友處理不同的模組
- 並行程式碼審查與實作
- 跨程式碼庫的協調式多檔案變更

> **注意**：Agent Teams 是實驗性功能，未來版本可能會有所變動。請參閱 [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams) 以取得完整參考資料。

---

## 最佳實務

### Planning Mode
- ✅ 用於複雜的多步驟任務
- ✅ 在核准前審查計畫
- ✅ 根據需要修改計畫
- ❌ 不要用於簡單任務

### Extended Thinking
- ✅ 用於架構決策
- ✅ 用於複雜的問題解決
- ✅ 審查思考過程
- ❌ 不要用於簡單查詢

### Background Tasks
- ✅ 用於長時間執行的操作
- ✅ 監控任務進度
- ✅ 優雅地處理任務失敗
- ❌ 不要啟動過多併發任務

### Permissions
- ✅ 使用 `plan` 進行程式碼審查（唯讀）
- ✅ 使用 `default` 進行互動式開發
- ✅ 使用 `acceptEdits` 進行自動化工作流程
- ✅ 使用 `auto` 進行具有安全護欄的自主工作
- ❌ 除非絕對必要，否則不要使用 `bypassPermissions`

### Sessions
- ✅ 為不同的任務使用獨立的工作階段
- ✅ 儲存重要的工作階段狀態
- ✅ 清理舊的工作階段
- ❌ 不要將不相關的工作混在同一個工作階段中

---

## 其他資源

欲了解更多關於 Claude Code 及相關功能的資訊：

- [官方互動模式文件](https://code.claude.com/docs/en/interactive-mode)
- [官方無介面模式文件](https://code.claude.com/docs/en/headless)
- [CLI 參考指南](https://code.claude.com/docs/en/cli-reference)
- [Checkpoints 指南](../08-checkpoints/) - 工作階段管理與回溯
- [斜線命令](../01-slash-commands/) - 命令參考
- [Memory 指南](../02-memory/) - 持久化上下文
- [Skills 指南](../03-skills/) - 自主能力
- [Subagents 指南](../04-subagents/) - 委派任務執行
- [MCP 指南](../05-mcp/) - 外部資料存取
- [Hooks 指南](../06-hooks/) - 事件驅動自動化
- [Plugins 指南](../07-plugins/) - 綑綁擴充功能
- [官方排程任務文件](https://code.claude.com/docs/en/scheduled-tasks)
- [官方 Chrome 整合文件](https://code.claude.com/docs/en/chrome)
- [官方遠端控制文件](https://code.claude.com/docs/en/remote-control)
- [官方按鍵綁定文件](https://code.claude.com/docs/en/keybindings)
- [官方桌面應用程式文件](https://code.claude.com/docs/en/desktop)
- [官方 Agent 團隊文件](https://code.claude.com/docs/en/agent-teams)

---

**最後更新日期**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/output-styles
- https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md
- https://code.claude.com/docs/en/permissions
- https://code.claude.com/docs/en/workflows#set-a-size-guideline
- https://code.claude.com/docs/en/tools-reference#task-tool-availability
- https://code.claude.com/docs/en/auto-mode-classifier-billing
- https://code.claude.com/docs/en/env-vars
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/sandboxing
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/cli-reference
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/settings.md
- https://code.claude.com/docs/en/settings-reference
- https://code.claude.com/docs/en/whats-new/2026-w34
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
