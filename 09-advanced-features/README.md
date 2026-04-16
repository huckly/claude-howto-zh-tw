<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 進階功能

全面指南，介紹 Claude Code 的進階功能，包括規劃模式、擴展思維、自動模式、背景任務、權限模式、列印模式（非互動式）、會話管理、互動功能、頻道、語音錄入、遠端控制、網頁會話、桌面應用程式、任務清單、提示詞建議、git 工作樹、沙箱、管理設定和配置。

## 目錄

1. [總覽](#overview)
2. [規劃模式](#planning-mode)
3. [Ultraplan（雲端計畫草擬）](#ultraplan-cloud-plan-drafting)
4. [擴展思維](#extended-thinking)
5. [自動模式](#auto-mode)
6. [背景任務](#background-tasks)
7. [監控工具（事件驅動串流）](#monitor-tool-event-driven-streams)
8. [排程任務](#scheduled-tasks)
9. [權限模式](#permission-modes)
10. [無頭模式](#headless-mode)
11. [會話管理](#session-management)
12. [互動功能](#interactive-features)
13. [語音錄入](#voice-dictation)
14. [頻道](#channels)
15. [Chrome 整合](#chrome-integration)
16. [遠端控制](#remote-control)
17. [網頁會話](#web-sessions)
18. [桌面應用程式](#desktop-app)
19. [任務清單](#task-list)
20. [提示詞建議](#prompt-suggestions)
21. [git 工作樹](#git-worktrees)
22. [沙箱](#sandboxing)
23. [管理設定（企業版）](#managed-settings-enterprise)
24. [配置和設定](#configuration-and-settings)
25. [代理團隊](#agent-teams)
26. [最佳實務](#best-practices)
27. [其他資源](#additional-resources)

---

## 總覽

Claude Code 的進階功能擴展了核心功能，包含規劃、推理、自動化和控制機制。這些功能能夠實現複雜開發任務、程式碼審查、自動化和多會話管理的複雜工作流程。

**主要進階功能包含：**
- **規劃模式**: 在編碼前創建詳細的實施計畫
- **擴展思考**: 對於複雜問題進行深入推理
- **自動模式**: 背景安全分類器在每個動作執行前進行審查（研究預覽版）
- **背景任務**: 在不影響對話的情況下執行長時間操作
- **權限模式**: 控制 Claude 可以執做的動作 (`default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions`)
- **列印模式**: 針對自動化和 CI/CD，以非互動方式執行 Claude Code (`claude -p`)
- **會話管理**: 管理多個工作會話
- **互動功能**: 鍵盤快捷鍵、多行輸入和命令歷史記錄
- **語音錄入**: 支援 20 種語言的 STT 語音輸入，推送到麥克風
- **頻道**: MCP 伺服器將訊息推送到正在執行的會話 (研究預覽版)
- **遠端控制**: 從 Claude.ai 或 Claude 應用程式控制 Claude Code
- **網頁會話**: 在 claude.ai/code 上的瀏覽器中執行 Claude Code
- **桌面應用程式**: 獨立應用程式，用於視覺化 diff 審查和多個會話
- **任務清單**: 在跨上下文壓縮中持續追蹤任務
- **提示詞建議**: 根據上下文提供智慧型命令建議
- **Git 工作樹**: 隔離工作樹分支，用於並行工作
- **沙箱**: 作業系統層級的檔案系統和網路隔離
- **管理設定**: 透過 plist、Registry 或管理檔案進行企業部署
- **設定**: 使用 JSON 設定檔案自訂行為

## 計畫模式

計畫模式允許 Claude 在執行複雜任務之前進行思考，創建一個您可以審閱和批准的詳細計畫。

### 什麼是計畫模式？

計畫模式是一種雙階段方法：
1. **計畫階段**: Claude 分析任務並創建一個詳細的實施計畫
2. **實施階段**: 批准後，Claude 執行計畫

### 何時使用計畫模式

✅ 使用計畫模式的場合：
- 複雜的多檔案重構
- 新功能實施
- 架構變更
- 資料庫遷移
- 重大的 API 重新設計

❌ 不使用計畫模式的場合：
- 簡單的錯誤修正
- 格式變更
- 單檔案編輯
- 快速查詢

### 啟用計畫模式

**斜線命令**:
```bash
/plan Implement user authentication system
```

**CLI flag**:
```bash
claude --permission-mode plan
```

**設定為預設**:
```json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

**快捷鍵**:
- `Shift + Tab` - 切換權限模式（包括計畫模式）
- `Alt + M` - 切換權限模式 (Windows/Linux)

### 計畫模式的優點

- **結構化的實施方法**: 實施步驟的詳細分解
- **審閱和批准**: 在執行之前批准或調整計畫
- **風險識別**: 在實施之前識別潛在問題
- **清晰的階段**: 有組織的實施階段和里程碑

### 範例：功能實施

```
User: /plan Add a real-time notification system

Claude: I'll create a plan for implementing real-time notifications.
```

## 實施計畫

### 第一階段：基礎架構設定
1. 安裝 WebSocket 函式庫 (socket.io)
2. 設定 WebSocket 伺服器
3. 設定連線管理

### 第二階段：後端實施
4. 建立通知服務
5. 實施通知佇列
6. 增加通知資料庫 Schema
7. 建立 API 端點

### 第三階段：前端整合
8. 設定 WebSocket 客戶端
9. 建立通知 UI 元件
10. 實施通知狀態管理
11. 增加通知權限

### 第四階段：測試與部署
12. 撰寫單元測試
13. 撰寫整合測試
14. 更新文件
15. 部署到測試環境

預估時間：3-4 小時
需要修改的檔案：12
需要新增的檔案：8

準備好開始了嗎？ (yes/no/modify plan)

使用者：yes

Claude: [開始依照計畫實施]
```

### 計畫修改

```
使用者：修改計畫 - 暫時跳過佇列，稍後再新增

Claude: 更新後的計畫：
[顯示移除佇列的修改後計畫]

使用者：看起來不錯，繼續

Claude: [實施修改後的計畫]
```

### 規劃模式設定

規劃模式透過 CLI 標記或斜線命令啟用：

```bash
# 透過 CLI 啟用計畫模式
claude --permission-mode plan

# 或在 REPL 內使用 /plan 斜線命令
/plan 實作使用者驗證系統
```

**規劃的模型別名**: 使用 `opusplan` 作為模型別名，以使用 Opus 進行規劃，並使用 Sonnet 進行執行：

```bash
claude --model opusplan "設計並實作新的 API"
```

**外部編輯計畫**: 點選 `Ctrl+G` 以在外部編輯器中開啟目前計畫以進行詳細修改。

---

## Ultraplan (雲端計畫草擬)

> **新功能於 v2.1.101 版**: Ultraplan 現在第一次執行時會自動在網頁雲端環境中建立一個 Claude Code — 無需手動設定，無需等待容器啟動才能開始草擬。

> **注意**: Ultraplan 是一個研究預覽版，需要 Claude Code v2.1.91 或更新版本。

`/ultraplan` 將您的本地 CLI 的規劃任務傳遞給網頁環境中以計畫模式運行的 Claude Code。Claude 在雲端草擬計畫，同時您的終端保持可用於其他工作，然後您在瀏覽器中檢閱草擬，並選擇在哪裡執行 — 在相同的雲端會話或傳送到您的終端。

### 何時使用 Ultraplan

- 您想要比終端更豐富的檢閱介面：行內註解、表情符號反應、大綱側邊欄和持續歷史記錄。
- 您希望在繼續本地編碼時進行無人參與的草擬 — 雲端會話研究您的程式碼庫並撰寫計畫，而不會阻塞您的 CLI。
- 計畫需要在執行前獲得利益相關者檢閱 — 共享的網頁 URL 比貼上終端滾動條更方便。

### 需求

- 網頁環境中的 Claude Code 帳戶。
- GitHub 程式碼庫 (雲端會話會將您的程式碼庫複製到程式碼庫以進行草擬)。
- **不可用** 在 Amazon Bedrock、Google Cloud Vertex AI 或 Microsoft Foundry 上。

### 三種啟動方式

- **命令**: `/ultraplan <prompt>` — 明確呼叫。
- **關鍵字**: 在普通提示詞中包含單詞 `ultraplan`，Claude 會將請求路由到雲端。
- **從本地計畫**: 當 Claude 完成本地計畫後，在批准對話框中選擇「不，使用 Ultraplan 在 Claude Code on the web 上進行精煉」，將草稿移交給雲端進行更深入的研究。

### 使用範例

```bash
/ultraplan migrate the auth service from sessions to JWTs
```

Claude 確認，啟動雲端環境（首次運行時在 v2.1.101+ 版本中自動建立），並返回您可以在瀏覽器中開啟的會話連結。

### 狀態指示器

| 狀態 | 意義 |
|---|---|
| `◇ ultraplan` | Claude 正在研究您的程式碼庫並草擬計畫 |
| `◇ ultraplan 需要您的輸入` | Claude 有一個澄清問題；開啟會話連結以回應 |
| `◆ ultraplan 準備完成` | 計畫已準備好在您的瀏覽器中檢閱 |

### 執行選項

當計畫準備完成後，您有兩種執行路徑。 在瀏覽器中批准計畫以在相同的雲端會話中執行 — Claude 遠端實施變更並從網頁 UI 開啟拉取請求。 或者，選擇「批准計畫並傳送到終端」以在本地實施。 終端傳送對話框提供三個選項：

- **在此處實施** — 在您目前的終端會話中執行批准的計畫。
- **啟動新會話** — 在相同的作業目錄中開啟一個新的會話並在那裡實施。
- **取消** — 將計畫儲存到檔案，以便您稍後可以繼續。

> **警告**: 遠端控制在 ultraplan 啟動時斷線。 兩個功能共享 claude.ai/code 介面，因此一次只能啟用其中一個。

## 擴展思考

擴展思考讓 Claude 能夠花費更多時間來思考複雜問題，並在提供解決方案前進行推理。

### 什麼是擴展思考？

擴展思考是一種刻意的、循序漸進的推理過程，在這個過程中，Claude：
- 將複雜問題分解
- 考慮多種方法
- 評估權衡
- 探討邊緣案例

### 啟用擴展思考

**快捷鍵**:
- `Option + T` (macOS) / `Alt + T` (Windows/Linux) - 啟用擴展思考

**自動啟用**:
- 預設為所有模型啟用 (Opus 4.6, Sonnet 4.6, Haiku 4.5)
- Opus 4.6: 自適應推理，並具有努力程度：`low` (○), `medium` (◐), `high` (●), `max` (僅限 Opus 4.6)
- 其他模型：固定預算，最高可達 31,999 個 token

**配置方法**:
- 啟用/停用：`Alt+T` / `Option+T`，或透過 `/config`
- 檢視推理：`Ctrl+O` (詳細模式)
- 設定努力程度：`/effort` 指令或 `--effort` 標誌

**自訂預算**:
```bash
export MAX_THINKING_TOKENS=1024
```

**努力程度** (僅限 Opus 4.6)：
```bash
export CLAUDE_CODE_EFFORT_LEVEL=high   # low (○), medium (◐), high (●), or max (Opus 4.6 only)
```

**CLI 標誌**:
```bash
claude --effort high "complex architectural review"
```

**斜線命令**:
```
/effort high
```

> **注意:** 在提示詞中使用 "ultrathink" 關鍵字會啟用深度推理模式。努力程度 `low`、`medium`、`high` 和 `max` (僅限 Opus 4.6) 控制 Claude 進行多少推理。

### 擴展思考的好處

- **深度推理**: 花費更多時間思考複雜問題
- **更好的架構決策**: 透過全面的評估做出明智的選擇
- **考慮邊緣案例**: 探討潛在問題和情境
- **徹底的分析**: 結構化的複雜問題解決方法

### 範例：架構決策

```
User: /think 我們的電商平台應該使用微服務還是單一體？

Claude: 讓我仔細思考這個架構決策...

<extended_thinking>
```

## 上下文分析
- 團隊規模：5 位開發人員
- 預期流量：初期每天 1 萬位使用者
- 需要擴展到 10 萬以上的使用者
- 有限的 DevOps 資源
- 6 個月時程以達成 MVP

## 方案 1：微服務
優點：
+ 獨立擴展
+ 技術彈性
+ 團隊獨立性
+ 容錯隔離

缺點：
- 運營複雜性
- 網路額外開銷
- 需要分散式追蹤
- 需要 DevOps 專業知識
- 初始開發速度較慢

## 方案 2：單一體應用程式 (Monolith)
優點：
+ 部署更簡單
+ 初始開發速度更快
+ 除錯更容易
+ 運營額外開銷更低
+ 單一程式碼庫

缺點：
- 後續擴展挑戰
- 全部或無的部署
- 技術鎖定
- 可能出現緊耦合

## 決策因素
1. 團隊規模 (5 位開發人員) → 傾向單一體應用程式
2. DevOps 資源 (有限) → 傾向單一體應用程式
3. 時程 (6 個月) → 傾向單一體應用程式
4. 未來擴展 (10 萬位使用者) → 透過單一體應用程式 + 快取可以達成
5. 複雜度預算 → 有限

## 建議
從 **模組化單一體應用程式** 開始：
- 單一部署單元
- 清晰的模組界限
- 模組之間乾淨的介面
- 準備好日後提取到微服務，如果需要

這樣可以獲得：
✓ 快速的初始開發
✓ 低運營複雜度
✓ 後續提取服務的選項
✓ 清晰的架構

遷移路徑：當您達到 5 萬以上的使用者，或特定模組需要獨立擴展時，將這些模組提取到微服務。

**建議**: 實作具有乾淨模組界限的模組化單一體應用程式。

此方法在平衡您目前的限制（團隊規模、時程、DevOps 資源）與未來的可擴展性需求之間取得了平衡。 當有明確的業務需求時，您可以稍後將特定模組遷移到微服務。

```
### 擴展思考配置

擴展思考由環境變數、快捷鍵和 CLI 標誌控制：

```bash
# 設定思考 token 預算
export MAX_THINKING_TOKENS=16000

# 設定努力程度 (僅限 Opus 4.6)：低 (○)、中 (◐)、高 (●) 或 最大 (僅限 Opus 4.6)
export CLAUDE_CODE_EFFORT_LEVEL=high
```

使用 `Alt+T` / `Option+T` 在會話期間切換，使用 `/effort` 設定努力程度，或使用 `/config` 設定。

---
```

## Auto Mode

Auto Mode 是一個研究預覽權限模式（2026 年 3 月），它使用背景安全分類器來審查每個動作在執行前的狀態。它允許 Claude 自主地工作，同時阻止危險的操作。

### 需求

- **Plan**: Team、Enterprise 或 API (Pro 或 Max 方案不可用)
- **Model**: Claude Sonnet 4.6 或 Opus 4.6
- **Provider**: 僅限 Anthropic API (不支援 Bedrock、Vertex 或 Foundry)
- **Classifier**: 在 Claude Sonnet 4.6 上執行 (會增加額外 token 成本)

### 啟用 Auto Mode

```bash
# 使用 CLI 標誌解鎖 auto mode
claude --enable-auto-mode

# 然後在 REPL 中使用 Shift+Tab 切換到它
```

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

### 分類器的工作方式

背景分類器會根據以下決策順序評估每個動作：

1. **Allow/deny 規則** -- 首先檢查明確的權限規則
2. **唯讀/編輯自動批准** -- 檔案讀取和編輯會自動通過
3. **分類器** -- 背景分類器審查動作
4. **Fallback** -- 在連續 3 次或總共 20 次區塊後回退到提示

### 預設阻止的動作

Auto mode 預設會阻止以下動作：

| 阻止的動作 | 範例 |
|----------------|---------|
| Pipe-to-shell 安裝 | `curl \| bash` |
| 傳送敏感資料到外部 | API 金鑰、網路上的憑證 |
| 產品部署 | 目標產品的部署命令 |
| 大量刪除 | `rm -rf` 在大型目錄中 |
| IAM 變更 | 權限和角色修改 |
| 強制推送到主分支 | `git push --force origin main` |

### 預設允許的動作

| 允許的動作 | 範例 |
|----------------|---------|
| 本地檔案操作 | 讀取、寫入、編輯專案檔案 |
| 宣告的相依性安裝 | `npm install`、`pip install` 從清單檔 |
| 唯讀 HTTP | `curl` 用於取得文件 |
| 推送到目前分支 | `git push origin feature-branch` |

### 設定 Auto Mode

**將預設規則以 JSON 格式輸出**：
```bash
claude auto-mode defaults
```

**透過 `autoMode.environment` 受管設定配置信任的基礎架構**，用於企業部署。這讓管理員可以定義信任的 CI/CD 環境、部署目標和基礎架構模式。

### Fallback 行為

當分類器不確定時，Auto Mode 會回退到提示使用者：

- 在 **連續 3 次** 分類器區塊後
- 在 **總共 20 次** 會話中的分類器區塊後

這確保了使用者始終保留控制權，當分類器無法自信地批准動作時。

### 種植 Auto-Mode-Equivalent 權限 (不需要 Team 方案)

如果您沒有 Team 方案或想要一種更簡單的方法，而不需要背景分類器，您可以將 `~/.claude/settings.json` 種植一個保守的安全性規則基礎線。 腳本從唯讀和本地檢查規則開始，然後讓您選擇性地啟用編輯、測試、本地 git 寫入、套件安裝和 GitHub 寫入動作，只有當您想要它們時。

**檔案:** `09-advanced-features/setup-auto-mode-permissions.py`

```bash
# 預覽將新增的規則 (不寫入任何變更)
python3 09-advanced-features/setup-auto-mode-permissions.py --dry-run

# 應用保守的基本規則
python3 09-advanced-features/setup-auto-mode-permissions.py

# 僅在需要時新增更多能力
python3 09-advanced-features/setup-auto-mode-permissions.py --include-edits --include-tests
python3 09-advanced-features/setup-auto-mode-permissions.py --include-git-write --include-packages
```

這個腳本會新增以下類別的規則：

| 類別 | 範例 |
|----------|---------|
| 核心唯讀工具 | `Read(*)`, `Glob(*)`, `Grep(*)`, `Agent(*)`, `WebSearch(*)`, `WebFetch(*)` |
| 本地檢查 | `Bash(git status:*)`, `Bash(git log:*)`, `Bash(git diff:*)`, `Bash(cat:*)` |
| 可選編輯 | `Edit(*)`, `Write(*)`, `NotebookEdit(*)` |
| 可選測試/建置 | `Bash(pytest:*)`, `Bash(python3 -m pytest:*)`, `Bash(cargo test:*)` |
| 可選 git 寫入 | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git stash:*)` |
| Git (本機寫入) | `Bash(git add:*)`, `Bash(git commit:*)`, `Bash(git checkout:*)` |
| 套件管理工具 | `Bash(npm install:*)`, `Bash(pip install:*)`, `Bash(cargo build:*)` |
| 建置 & 測試 | `Bash(make:*)`, `Bash(pytest:*)`, `Bash(go test:*)` |
| 常用 shell | `Bash(ls:*)`, `Bash(cat:*)`, `Bash(find:*)`, `Bash(cp:*)`, `Bash(mv:*)` |
| GitHub CLI | `Bash(gh pr view:*)`, `Bash(gh pr create:*)`, `Bash(gh issue list:*)` |

危險操作 (`rm -rf`, `sudo`, 強制推送, `DROP TABLE`, `terraform destroy` 等) 刻意排除。這個腳本是冪等的 — 執行兩次不會重複規則。

---

## 背景任務

背景任務允許長時間運作的操作在不阻斷您的對話的情況下執行。

### 什麼是背景任務？

背景任務在您繼續工作時非同步執行：
- 長期測試套件
- 構建流程
- 資料庫遷移
- 部署腳本
- 分析工具

**基本用法：**
```bash
User: 執行背景測試

Claude: 啟動任務 bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 檢查進度
/task show bg-1234   # 檢視輸出
/task cancel bg-1234 # 停止任務
```

### 啟動背景任務

```
User: 在背景中執行完整的測試套件

Claude: 開始在背景中執行測試 (task-id: bg-1234)
您可以在測試執行期間繼續工作。

[您可以繼續對話]

User: 另一方面，我們來重構 auth 模組

Claude: [在測試在背景中執行時，處理 auth 模組]

[稍後，當測試完成時]

Claude: 📢 背景任務 bg-1234 成功完成：
✅ 245 個測試通過
❌ 3 個測試失敗
檢視結果：/task show bg-1234
```

### 管理背景任務

**列出所有活躍任務：**
```
User: /task list

活躍的背景任務：
1. [bg-1234] 執行測試 (完成 50%，剩餘 2 分鐘)
2. [bg-1235] 構建 Docker 映像 (完成 25%，剩餘 8 分鐘)
3. [bg-1236] 部署到 staging (完成 90%，剩餘 30 秒)
```

**檢查任務狀態：**
```
User: /task status bg-1234

任務 bg-1234：執行測試
狀態：進行中
進度：245/245 個測試 (49%)
開始時間：2025-11-08 10:30:15
預計完成時間：2025-11-08 10:34:22
```

**檢視任務輸出：**
```
User: /task show bg-1234

[顯示測試執行中的即時輸出]
```

**停止任務：**
```
User: /task cancel bg-1234

已取消背景任務 bg-1234
```

### 範例：平行開發

```
User: 在背景中執行構建

Claude: 開始構建... (task-id: bg-5001)

User: 另外，在背景中執行 linter

Claude: 開始執行 linter... (task-id: bg-5002)

User: 在這些執行期間，我們來實作新的 API 端點

Claude: [實作 API 端點，同時構建和 linter 執行]

[10 分鐘後]

Claude: 📢 構建成功完成 (bg-5001)
📢 Linter 發現 12 個問題 (bg-5002)

User: 顯示 linter 的問題

Claude: [顯示 bg-5002 的 linter 輸出]
```

### 設定

```json
{
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5,
    "notifyOnCompletion": true,
    "autoCleanup": true,
    "logOutput": true
  }
}
```

## 監控工具 (事件驅動的串流)

> **v2.1.98 新增**: 監控工具讓 Claude 能夠監看背景命令的標準輸出 (stdout) 並在出現符合條件的事件時做出反應，取代了輪詢迴圈和 `sleep` 用於等待長時間執行的處理序。

監控工具可以附加到任何將內容寫入標準輸出的 shell 命令。 命令的每一行標準輸出都會成為喚醒會話的通知。 Claude 指定命令；harness 串流輸出並在事件發生時傳遞事件。 請參閱相關的 [背景任務](#background-tasks) 區段以啟動基礎處理序。

### 為什麼這很重要

使用 `/loop` 或 `sleep` 進行輪詢會在每個循環中消耗完整的 API 請求，無論是否有任何變更。 監控工具在事件發生前保持靜默，在命令靜止時消耗 **零 token**。 當事件發生時，Claude 能夠立即做出反應 — 沒有延遲的發現等待下一次輪詢的時鐘滴答。 對於任何執行時間超過幾分鐘的任務，這比輪詢迴圈更便宜且更快。

### 兩種常見模式

**串流篩選器** 監看來自長時間執行來源的持續輸出。 命令會永久執行；每一行符合條件的行都是一個事件。

```bash
tail -f /var/log/app.log | grep --line-buffered "ERROR"
```

**週期性檢查並發出事件的篩選器** 週期性地檢查來源，僅當有任何變更時才發出事件。 適用於 API、資料庫或任何沒有原生串流的內容。

```bash
last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
while true; do
  gh api "repos/owner/repo/issues/123/comments?since=$last" || true
  last=$(date -u +%Y-%m-%dT%H:%M:%SZ)
  sleep 30
done
```

### 具體範例

“啟動我的開發伺服器並監控其錯誤。” Claude 將伺服器作為背景任務啟動，附加一個監控篩選器 (`tail -F server.log | grep --line-buffered -E "ERROR|FATAL"`)，然後會話保持靜默。 當日誌中出現錯誤行時，Claude 醒來，讀取錯誤，並可以做出反應 — 重新啟動伺服器、修復錯誤或將其呈現給您 — 而無需您檢查。

> **警告**: 將內容導入 `grep` 時，**務必** 使用 `grep --line-buffered`。 缺少它，grep 會以 4KB 的區塊大小緩衝標準輸出，這可能會在低流量串流中延遲事件長達幾分鐘。 這是監控工具在實務上失效的 #1 方式 — 如果您的篩選器似乎應該醒來卻沒有醒來，請首先檢查 `--line-buffered` 標誌。

---

## 排程任務

排程任務讓您能夠在重複的排程或一次性提醒中自動執行提示詞。 任務是會話範圍的——它們在 Claude Code 處於活動狀態時執行，並且在會話結束時清除。  начиная с v2.1.72+.

### `/loop` 命令

```bash
# 明確間隔
/loop 5m 檢查部署是否完成

# 自然語言
/loop 檢查每 30 分鐘的建置狀態
```

也支援標準的 5 個欄位的 cron 表達式，以進行精確的排程。

### 一次性提醒

設定在特定時間點觸發的一次性提醒：

```
提醒我下午 3 點推送發布分支
45 分鐘後，執行整合測試
```

### 管理排程任務

| 工具 | 描述 |
|------|-------------|
| `CronCreate` | 建立新的排程任務 |
| `CronList` | 列出所有活動的排程任務 |
| `CronDelete` | 移除一個排程任務 |

**限制和行為**:
- 每會話最多 **50 個排程任務**
- 會話範圍的——在會話結束時清除
- 週期性任務在 **3 天**後自動過期
- 任務只有在 Claude Code 執行時才會觸發——沒有遺漏觸發的補償

### 行為細節

| 方面 | 細節 |
|--------|--------|
| **週期性抖動** | 間隔的最高 10% (最高 15 分鐘) |
| **一次性抖動** | 在 :00/:30 邊界上最高 90 秒 |
| **遺漏觸發** | 沒有補償——如果 Claude Code 沒有執行，則會跳過 |
| **持久性** | 不會在重新啟動時持久 |

### 雲端排程任務

使用 `/schedule` 建立在 Anthropic 基礎架構上執行的雲端排程任務：

```
/schedule 每天上午 9 點執行測試套件並報告失敗
```

雲端排程任務會在重新啟動時持久，並且不需要 Claude Code 在本機執行。

### 停用排程任務

```bash
export CLAUDE_CODE_DISABLE_CRON=1
```

### 範例：監控部署

```
/loop 5m 檢查 staging 環境的部署狀態。
        如果部署成功，通知我並停止循環。
        如果失敗，顯示錯誤日誌。
```

> **提示**: 排程任務是會話範圍的。 若要進行持久的自動化，可在重新啟動時使用 CI/CD 流水線、GitHub Actions 或 Desktop App 排程任務。

---

## 權限模式

權限模式控制 Claude 可以採取哪些動作而無需明確的批准。

### 可用的權限模式

| 模式 | 行為 |
|---|---|
| `default` | 僅讀取檔案；對於所有其他動作都會提示 |
| `acceptEdits` | 讀取和編輯檔案；對於命令會提示 |
| `plan` | 僅讀取檔案（研究模式，不編輯） |
| `auto` | 所有動作，並進行背景安全分類器檢查（研究預覽版） |
| `bypassPermissions` | 所有動作，不進行權限檢查（危險） |
| `dontAsk` | 僅執行預先批准的工具；其他工具會被拒絕 |

使用 `Shift+Tab` 在 CLI 中循環瀏覽模式。 使用 `--permission-mode` 標誌或 `permissions.defaultMode` 設定來設定預設模式。

### 啟用方法

**鍵盤快捷鍵**:
```bash
Shift + Tab  # 循環瀏覽所有 6 種模式
```

**斜線命令**:
```bash
/plan                  # 進入計畫模式
```

**CLI 標誌**:
```bash
claude --permission-mode plan
claude --permission-mode auto
```

**設定**:
```json
{
  "permissions": {
    "defaultMode": "auto"
  }
}
```

### 權限模式範例

#### 預設模式
Claude 會要求確認重要的動作：

```
使用者：修正 auth.ts 中的錯誤

Claude: 我需要修改 src/auth.ts 以修正錯誤。
變更將更新密碼驗證邏輯。

是否批准此變更？ (yes/no/show)
```

#### 計畫模式
在執行前審查實施計畫：

```
使用者：/plan 實作使用者驗證系統

Claude: 我會建立一個實作驗證的計畫。
```

## 實施計畫
[詳細計畫，包含階段和步驟]

準備好繼續嗎？ (yes/no/modify)
```

#### 接受編輯模式
自動接受檔案修改：

```
User: acceptEdits
User: 修正 auth.ts 中的錯誤

Claude: [進行變更，無需詢問]
```

### 使用案例

**程式碼審查**:
```
User: claude --permission-mode plan
User: 審查這個 PR 並提出建議

Claude: [閱讀程式碼，提供回饋，但無法修改]
```

**配對編程**:
```
User: claude --permission-mode default
User: 讓我們一起實作這個功能

Claude: [在每次變更前要求批准]
```

**自動化任務**:
```
User: claude --permission-mode acceptEdits
User: 修正程式碼庫中的所有 linting 問題

Claude: [無需詢問，自動接受檔案編輯]
```

---

## 無頭模式

列印模式 (`claude -p`) 允許 Claude Code 在沒有互動式輸入的情況下執行，非常適合自動化和 CI/CD。 這是取代舊版 `--headless` 標記的非互動模式。

### 什麼是列印模式？

列印模式允許：
- 自動化腳本執行
- CI/CD 整合
- 批次處理
- 排程任務

### 在列印模式 (非互動) 中執行

```bash
# 執行特定任務
claude -p "執行所有測試"

# 處理管道內容
cat error.log | claude -p "分析這些錯誤"

# CI/CD 整合 (GitHub Actions)
- name: AI 程式碼審查
  run: claude -p "審查 PR"
```

### 列印模式額外使用範例

```bash
# 執行特定任務並捕獲輸出
claude -p "執行所有測試並產生 coverage 報告"

# 具有結構化輸出
claude -p --output-format json "分析程式碼品質"

# 從 stdin 接收輸入
echo "分析程式碼品質" | claude -p "解釋這個"
```

### 範例：CI/CD 整合

**GitHub Actions**:
```yaml
# .github/workflows/code-review.yml
name: AI 程式碼審查

on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: 安裝 Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: 執行 Claude Code 審查
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude -p --output-format json \
            --max-turns 3 \
            "審查這個 PR，關注：
            - 程式碼品質問題
            - 安全漏洞
            - 效能疑慮
            - 測試覆蓋率
            以 JSON 格式輸出結果" > review.json

      - name: 發表審查評論
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

列印模式 (`claude -p`) 支援多個用於自動化的標記：

```bash

# 限制自主回饋輪數
claude -p --max-turns 5 "重構這個模組"

# 結構化 JSON 輸出
claude -p --output-format json "分析這個程式碼庫"

# 搭配 Schema 驗證
claude -p --json-schema '{"type":"object","properties":{"issues":{"type":"array"}}}' \
  "在這個程式碼中找出錯誤"

# 禁用會話持久性
claude -p --no-session-persistence "一次性分析"
```

---

## 會話管理

有效管理多個 Claude Code 會話。

### 會話管理命令

| 命令 | 描述 |
|---------|-------------|
| `/resume` | 根據 ID 或名稱繼續對話 |
| `/rename` | 命名目前的會話 |
| `/fork` | 將目前會話分叉到新的分支 |
| `claude -c` | 繼續最近的對話 |
| `claude -r "session"` | 根據名稱或 ID 繼續會話 |

### 繼續會話

**繼續上一個對話**:
```bash
claude -c
```

**繼續已命名的會話**:
```bash
claude -r "auth-refactor" "完成這個 PR"
```

**重新命名目前的會話** (在 REPL 內):
```
/rename auth-refactor
```

### 分叉會話

分叉會話以嘗試不同的方法，而不會失去原始會話：

```
/fork
```

或者從 CLI：
```bash
claude --resume auth-refactor --fork-session "嘗試 OAuth 替代方案"
```

### 會話持久性

會話會自動儲存，並且可以繼續：

```bash
# 繼續上一個對話
claude -c

# 根據名稱或 ID 繼續特定會話
claude -r "auth-refactor"

# 繼續並分叉以進行實驗
claude --resume auth-refactor --fork-session "替代方法"
```

## 互動功能

### 快捷鍵

Claude Code 支援快捷鍵以提高效率。以下是官方文件中的完整參考：

| 快捷鍵 | 描述 |
|----------|-------------|
| `Ctrl+C` | 停止目前輸入/生成 |
| `Ctrl+D` | 退出 Claude Code |
| `Ctrl+G` | 在外部編輯器中編輯計畫 |
| `Ctrl+L` | 清除終端機畫面 |
| `Ctrl+O` | 啟用詳細輸出 (查看推理) |
| `Ctrl+R` | 反向搜尋歷史記錄 |
| `Ctrl+T` | 啟用任務列表檢視 |
| `Ctrl+B` | 背景執行任務 |
| `Esc+Esc` | 回溯程式碼/對話 |
| `Shift+Tab` / `Alt+M` | 啟用權限模式 |
| `Option+P` / `Alt+P` | 轉換模型 |
| `Option+T` / `Alt+T` | 啟用擴展思考 |

**行編輯 (標準 readline 快捷鍵)：**

| 快捷鍵 | 動作 |
|----------|--------|
| `Ctrl + A` | 移動到行首 |
| `Ctrl + E` | 移動到行尾 |
| `Ctrl + K` | 切割到行尾 |
| `Ctrl + U` | 切割到行首 |
| `Ctrl + W` | 向後刪除單詞 |
| `Ctrl + Y` | 貼上 (yank) |
| `Tab` | 自動完成 |
| `↑ / ↓` | 命令歷史記錄 |

### 自訂快捷鍵

透過執行 `/keybindings` 建立自訂快捷鍵，這會開啟 `~/.claude/keybindings.json` 以供編輯 (v2.1.18+)。

**配置格式**:

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

將綁定設定為 `null` 以取消預設快捷鍵。

### 可用的上下文

快捷鍵的範圍限定於特定的 UI 上下文：

| 上下文 | 主要動作 |
|---------|-------------|
| **Chat** | `submit`, `cancel`, `cycleMode`, `modelPicker`, `thinkingToggle`, `undo`, `externalEditor`, `stash`, `imagePaste` |
| **Confirmation** | `yes`, `no`, `previous`, `next`, `nextField`, `cycleMode`, `toggleExplanation` |
| **Global** | `interrupt`, `exit`, `toggleTodos`, `toggleTranscript` |
| **Autocomplete** | `accept`, `dismiss`, `next`, `previous` |
| **HistorySearch** | `search`, `previous`, `next` |
| **Settings** | 上下文特定的設定導航 |
| **Tabs** | 標籤切換和管理 |
| **Help** | 說明面板導航 |

總共有 18 個上下文，包括 `Transcript`、`Task`、`ThemePicker`、`Attachments`、`Footer`、`MessageSelector`、`DiffDialog`、`ModelPicker` 和 `Select`。

### 和弦支援

快捷鍵支援和弦序列（多鍵組合）：

```
"ctrl+k ctrl+s"   → 雙鍵序列：按下 ctrl+k，然後按下 ctrl+s
"ctrl+shift+p"    → 同時按下修飾鍵
```

**按鍵語法**:
- **修飾鍵**: `ctrl`, `alt` (或 `opt`), `shift`, `meta` (或 `cmd`)
- **大寫字母表示 Shift**: `K` 等同於 `shift+k`
- **特殊按鍵**: `escape`, `enter`, `return`, `tab`, `space`, `backspace`, `delete`, 箭頭鍵

### 預設及衝突的鍵

| 鍵 | 狀態 | 備註 |
|-----|--------|-------|
| `Ctrl+C` | 預設 | 無法重新綁定 (中斷) |
| `Ctrl+D` | 預設 | 無法重新綁定 (退出) |
| `Ctrl+B` | 終端機衝突 | tmux 前置鍵 |
| `Ctrl+A` | 終端機衝突 | GNU Screen 前置鍵 |
| `Ctrl+Z` | 終端機衝突 | 程式暫停 |

> **提示**: 如果快捷鍵無法使用，請檢查是否與您的終端機模擬器或多路復用器發生衝突。

### 標籤自動完成

Claude Code 提供智慧型標籤自動完成：

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

### 命令歷史記錄

存取先前的命令：

```
User: <↑>  # 前一個命令
User: <↓>  # 下一個命令
User: Ctrl+R  # 搜尋歷史記錄

(reverse-i-search)`test': 執行所有測試
```

### 多行輸入

對於複雜的查詢，請使用多行模式：

```bash
User: \
> 長且複雜的提示詞
> 跨越多行
> \end
```

**範例：**

```
User: \
> 實作一個使用者驗證系統
> 具有以下需求：
> - JWT token
> - 電子郵件驗證
> - 密碼重設
> - 2FA 支援
> \end

Claude: [處理多行請求]
```

### 行內編輯

編輯命令後再發送：

```
User: Deploy to prodcution<Backspace><Backspace>uction

[在發送前就地編輯]
```

### Vim 模式

啟用 Vi/Vim 鍵綁定以進行文字編輯：

**啟用：**
- 使用 `/vim` 命令或 `/config` 啟用
- 使用 `Esc` 模式切換到 NORMAL 模式，使用 `i/a/o` 切換到 INSERT 模式

**導航鍵：**
- `h` / `l` - 向左/向右移動
- `j` / `k` - 向下/向上移動
- `w` / `b` / `e` - 依單詞移動
- `0` / `$` - 移動到行首/行尾
- `gg` / `G` - 移至文字開頭/結尾

**文字物件：**
- `iw` / `aw` - 內部/周圍單詞
- `i"` / `a"` - 內部/周圍引號字串
- `i(` / `a(` - 內部/周圍括號

### Bash 模式

使用 `!` 前置詞直接執行 shell 命令：

```bash
! npm test
! git status
! cat src/index.js
```

使用此方法快速執行命令，而無需切換上下文。

---

## 語音輸入

語音輸入為 Claude Code 提供的推播式語音輸入功能，讓您可以使用語音輸入提示詞，而不是打字。

### 啟用語音輸入

```
/voice
```

### 功能

| 功能 | 描述 |
|---------|-------------|
| **推播式語音輸入** | 按住按鈕錄製，放開按鈕發送 |
| **20 種語言** | 語音轉文字支援 20 種語言 |
| **自訂按鍵綁定** | 透過 `/keybindings` 設定推播式語音輸入按鈕 |
| **帳戶要求** | 需要 Claude.ai 帳戶才能進行語音轉文字處理 |

### 設定

在您的按鍵綁定檔案 (`/keybindings`) 中自訂推播式語音輸入按鈕。語音輸入使用您的 Claude.ai 帳戶進行語音轉文字處理。

---

## 頻道

頻道是一個研究預覽功能，它將來自外部服務的事件推送到正在執行的 Claude Code 會話中，透過 MCP 伺服器。來源包括 Telegram、Discord、iMessage 和任意的 Webhooks，讓 Claude 能夠在無需輪詢的情況下，對即時通知做出反應。

### 訂閱頻道

```bash
# 在啟動時訂閱頻道外掛
claude --channels discord,telegram

# 訂閱多個來源
claude --channels discord,telegram,imessage,webhooks
```

### 支援的整合

| 整合 | 描述 |
|-------------|-------------|
| **Discord** | 接收並回覆您會話中的 Discord 訊息 |
| **Telegram** | 接收並回覆您會話中的 Telegram 訊息 |
| **iMessage** | 接收您會話中的 iMessage 通知 |
| **Webhooks** | 接收來自任意 Webhook 來源的事件 |

### 設定

使用 `--channels` 標記在啟動時設定頻道。對於企業部署，請使用管理設定來控制允許哪些頻道外掛：

```json
{
  "allowedChannelPlugins": ["discord", "telegram"]
}
```

`allowedChannelPlugins` 管理設定控制組織中允許哪些頻道外掛。

### 運作方式

1. MCP 伺服器作為頻道外掛，連接到外部服務
2. 傳入的訊息和事件被推送到正在執行的 Claude Code 會話中
3. Claude 可以在會話上下文中閱讀並回覆訊息
4. 頻道外掛必須透過 `allowedChannelPlugins` 管理設定批准
5. 不需要輪詢 — 事件以即時推播的方式傳送

## Chrome 整合

Chrome 整合將 Claude Code 連接到您的 Chrome 或 Microsoft Edge 瀏覽器，用於即時網路自動化和除錯。這是一個自 v2.0.73+ 之後可用的測試版功能（Edge 支援新增於 v1.0.36+）。

### 啟用 Chrome 整合

**啟動時：**

```bash
claude --chrome      # 啟用 Chrome 連線
claude --no-chrome   # 停用 Chrome 連線
```

**在會話中：**

```
/chrome
```

選取「預設啟用」以啟用所有未來會話的 Chrome 整合。Claude Code 會與您的瀏覽器共用登入狀態，因此它可以與經過驗證的網路應用程式互動。

### 功能

| 功能 | 描述 |
|------------|-------------|
| **即時除錯** | 讀取主控台記錄，檢查 DOM 元素，即時除錯 JavaScript |
| **設計驗證** | 將渲染的頁面與設計原型進行比較 |
| **表單驗證** | 測試表單提交、輸入驗證和錯誤處理 |
| **網路應用程式測試** | 與經過驗證的應用程式互動 (Gmail、Google Docs、Notion 等) |
| **資料提取** | 從網頁抓取和處理內容 |
| **會話錄製** | 將瀏覽器互動記錄為 GIF 檔案 |

### 網站層級權限

Chrome 擴充程式管理每個網站的存取權。您可以隨時透過擴充程式彈出視窗授予或撤銷特定網站的存取權。Claude Code 只會與您明確允許的網站互動。

### 運作方式

Claude Code 在可見的視窗中控制瀏覽器 — 您可以觀看動作發生的實況。當瀏覽器遇到登入頁面或 CAPTCHA 時，Claude 會暫停並等待您手動處理，然後再繼續。

### 已知限制

- **瀏覽器支援**: 僅支援 Chrome 和 Edge — Brave、Arc 和其他 Chromium 瀏覽器不受支援
- **WSL**: 無法在 Windows Subsystem for Linux 中使用
- **第三方提供者**: 不支援 Bedrock、Vertex 或 Foundry API 提供者
- **服務工作者閒置**: Chrome 擴充程式服務工作者可能在長時間的會話期間閒置

> **提示**: Chrome 整合是一個測試版功能。未來版本可能會擴充瀏覽器支援。

---

## 遠端控制

遠端控制功能讓您能夠從手機、平板電腦或任何瀏覽器繼續執行本機的 Claude Code 工作階段。您的本機工作階段會在您的機器上持續執行 — 任何內容都不會移至雲端。適用於 Pro、Max、Team 和 Enterprise 方案 (v2.1.51+)。

### 啟動遠端控制

**從 CLI**:

```bash
# 使用預設工作階段名稱啟動
claude remote-control

# 使用自訂名稱啟動
claude remote-control --name "Auth Refactor"
```

**在工作階段內**:

```
/remote-control
/remote-control "Auth Refactor"
```

**可用的參數**:

| 參數 | 說明 |
|------|-------------|
| `--name "title"` | 方便識別的自訂工作階段標題 |
| `--verbose` | 顯示詳細的連線記錄 |
| `--sandbox` | 啟用檔案系統和網路隔離 |
| `--no-sandbox` | 停用沙盒 (預設) |

### 連線到工作階段

有三種方法可以從另一部裝置連線：

1. **工作階段 URL** — 工作階段啟動時會印出到終端機；可在任何瀏覽器中開啟
2. **QR 碼** — 啟動後按下 `spacebar` 以顯示可掃描的 QR 碼
3. **依名稱搜尋** — 在 claude.ai/code 或 Claude 行動應用程式 (iOS/Android) 中瀏覽您的工作階段

### 安全性

- **沒有**在您的機器上開啟任何**入埠埠**
- **僅限出埠 HTTPS**，使用 TLS
- **範圍限定憑證** — 許多短時間、範圍限定的 token
- **工作階段隔離** — 每個遠端工作階段都是獨立的

### 遠端控制與 Claude Code 在網頁上的比較

| 方面 | 遠端控制 | Claude Code 在網頁上 |
|--------|---------------|-------------------|
| **執行** | 在您的機器上執行 | 在 Anthropic 雲端上執行 |
| **本機工具** | 完整存取本機 MCP 伺服器、檔案和 CLI | 沒有本機相依性 |
| **使用案例** | 從另一部裝置繼續本機工作 | 從任何瀏覽器開始全新工作 |

### 限制

- 每一個 Claude Code 實例只能有一個遠端工作階段
- 主機機器的終端機必須保持開啟狀態
- 如果網路無法存取，工作階段會在 ~10 分鐘後逾時

### 使用案例

- 使用行動裝置或平板電腦控制 Claude Code，即使您不在辦公桌前
- 使用更豐富的 claude.ai UI，同時保持本機工具的執行
- 使用完整的本機開發環境，隨時進行快速程式碼審查

## Web Sessions

Web Sessions 允許您直接在 claude.ai/code 瀏覽器中執行 Claude Code，或從 CLI 建立網頁 Sessions。

### 建立 Web Session

```bash
# 從 CLI 建立新的網頁 Session
claude --remote "implement the new API endpoints"
```

這會在 claude.ai 上啟動一個 Claude Code Session，您可以從任何瀏覽器存取。

### 繼續 Web Sessions 本地

如果您在網頁上啟動了一個 Session，並想在本地繼續它：

```bash
# 在本地終端機中繼續一個網頁 Session
claude --teleport
```

或從互動式 REPL 內：
```
/teleport
```

### 使用案例

- 在一台機器上開始工作，然後在另一台上繼續
- 與團隊成員分享 Session URL
- 使用網頁 UI 進行視覺化差異檢視，然後切換到終端機進行執行

---

## Desktop App

Claude Code Desktop App 提供一個獨立應用程式，具有視覺化差異檢視、並行 Sessions 以及整合的連接器。 適用於 macOS 和 Windows (Pro, Max, Team 和 Enterprise 計劃)。

### 安裝

從 [claude.ai](https://claude.ai) 下載適用於您平台的應用程式：
- **macOS**: 通用構建 (Apple Silicon 和 Intel)
- **Windows**: 提供 x64 和 ARM64 安裝程式

請參閱 [Desktop Quickstart](https://code.claude.com/docs/en/desktop-quickstart) 以取得設定指示。

### 從 CLI 移交

將您目前的 CLI Session 移交到 Desktop App：

```
/desktop
```

### 核心功能

| 功能 | 描述 |
|---------|-------------|
| **Diff 檢視** | 逐個檔案的視覺化檢視，帶有內嵌評論；Claude 讀取評論並進行修改 |
| **App 預覽** | 自動啟動開發伺服器，帶有內嵌瀏覽器以進行即時驗證 |
| **PR 監控** | GitHub CLI 整合，可自動修正 CI 失敗並在檢查通過時自動合併 |
| **並行 Sessions** | 側邊欄中有多個 Sessions，並具有自動 Git 工作區隔離 |
| **排程任務** | 循環任務（每小時、每日、工作日、每週），在應用程式開啟時執行 |
| **豐富渲染** | 具有語法高光的程式碼、Markdown 和圖表渲染 |

### App 預覽設定

在 `.claude/launch.json` 中設定開發伺服器行為：

```json
{
  "command": "npm run dev",
  "port": 3000,
  "readyPattern": "ready on",
  "persistCookies": true
}
```

### 連接器

連接外部服務以獲得更豐富的上下文：

| 連接器 | 功能 |
|-----------|------------|
| **GitHub** | PR 監控、問題追蹤、程式碼檢視 |
| **Slack** | 通知、頻道上下文 |
| **Linear** | 問題追蹤、衝刺管理 |
| **Notion** | 文件、知識庫存取 |
| **Asana** | 任務管理、專案追蹤 |
| **Calendar** | 排程意識、會議上下文 |

> **注意**: 連接器無法用於遠端 (雲端) Sessions。

### 遠端和 SSH Sessions

- **遠端 Sessions**: 在 Anthropic 雲端基礎架構上執行；即使應用程式關閉也能繼續。 可以從 claude.ai/code 或 Claude 行動應用程式存取
- **SSH Sessions**: 通過 SSH 連接到遠端機器，可以完全存取遠端檔案系統和工具。 Claude Code 必須在遠端機器上安裝

### Desktop 中的權限模式

Desktop App 支援與 CLI 相同的 4 種權限模式：

| 模式 | 行為 |
|------|----------|
| **要求權限** (預設) | 審查並批准每個編輯和命令 |
| **自動接受編輯** | 檔案編輯自動批准；命令需要手動批准 |
| **計畫模式** | 在進行任何變更之前審查方法 |
| **繞過權限** | 自動執行 (僅限沙箱，由管理員控制) |

### 企業功能

- **管理主控台**: 控管 Code 標籤存取權和組織的權限設定
- **MDM 部署**: 在 macOS 上透過 MDM 或在 Windows 上透過 MSIX 進行部署
- **SSO 整合**: 要求組織成員進行單一登入
- **管理設定**: 集中管理團隊設定和模型可用性

---

## 工作清單

工作清單功能提供持續性的任務追蹤，即使在上下文壓縮（當對話記錄被修剪以符合上下文視窗）時也能存續。

### 啟用/停用工作清單

按下 `Ctrl+T` 可以在會話期間啟用或停用工作清單視窗。

### 持續性的任務

任務會跨越上下文壓縮而持續存在，確保長時間運作的項目不會在對話上下文被修剪時遺失。 這對於複雜的、多步驟的實作特別有用。

### 命名的任務目錄

使用 `CLAUDE_CODE_TASK_LIST_ID` 環境變數來建立跨會話共享的命名任務目錄：

```bash
export CLAUDE_CODE_TASK_LIST_ID=my-project-sprint-3
```

這允許多個會話共享相同的任務清單，使其對團隊工作流程或多會話專案非常有用。

## 提示詞建議

提示詞建議會根據您的 git 歷史記錄和目前的會話上下文，顯示灰色的範例命令。

### 運作方式

- 建議會以灰色文字顯示在您的輸入提示詞下方
- 點選 `Tab` 以接受建議
- 點選 `Enter` 以接受並立即提交
- 建議會考慮上下文，從 git 歷史記錄和會話狀態中提取資訊

### 停用提示詞建議

```bash
export CLAUDE_CODE_ENABLE_PROMPT_SUGGESTION=false
```

---

## Git 工作樹

Git 工作樹允許您以隔離的工作樹中啟動 Claude Code，從而無需儲存或切換，即可並行處理不同的分支。

### 在工作樹中啟動

```bash
# 在隔離的工作樹中啟動 Claude Code
claude --worktree
# 或
claude -w
```

### 工作樹位置

工作樹會建立在：
```
<repo>/.claude/worktrees/<name>
```

### 對於單一存放區的稀疏檢出

使用 `worktree.sparsePaths` 設定來對單一存放區進行稀疏檢出，以減少磁碟使用量和複製時間：

```json
{
  "worktree": {
    "sparsePaths": ["packages/my-package", "shared/"]
  }
}
```

### 工作樹工具和鉤子

| 項目 | 描述 |
|------|-------------|
| `ExitWorktree` | 用於退出並清理目前工作樹的工具 |
| `WorktreeCreate` | 當建立工作樹時觸發的鉤子事件 |
| `WorktreeRemove` | 當移除工作樹時觸發的鉤子事件 |

### 自動清理

如果沒有在工作樹中進行任何變更，當會話結束時，它將自動清理。

### 使用案例

- 在不影響主分支的情況下，在功能分支上進行開發
- 在隔離的環境中執行測試，而不會影響工作目錄
- 在一次性環境中嘗試實驗性變更
- 對於更快的啟動，對單一存放區中的特定套件進行稀疏檢出

## 沙箱

沙箱為 Claude Code 執行的 Bash 命令提供作業系統層級的檔案系統和網路隔離。這與權限規則相輔相成，並提供額外的安全層。

### 啟用沙箱

**斜線命令**:
```
/sandbox
```

**CLI 標誌**:
```bash
claude --sandbox       # 啟用沙箱
claude --no-sandbox    # 停用沙箱
```

### 設定選項

| 設定 | 說明 |
|---------|-------------|
| `sandbox.enabled` | 啟用或停用沙箱 |
| `sandbox.failIfUnavailable` | 如果無法啟用沙箱，則失敗 |
| `sandbox.filesystem.allowWrite` | 允許寫入存取的路徑 |
| `sandbox.filesystem.allowRead` | 允許讀取存取的路徑 |
| `sandbox.filesystem.denyRead` | 拒絕讀取存取的路徑 |
| `sandbox.enableWeakerNetworkIsolation` | 在 macOS 上啟用較弱的網路隔離 |

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

### 運作方式

- Bash 命令在具有限制檔案系統存取的沙箱環境中執行
- 網路存取可以隔離，以防止意外的外部連線
- 與權限規則一起運作，以提供多重防線
- 在 macOS 上，使用 `sandbox.enableWeakerNetworkIsolation` 進行網路限制（macOS 上無法提供完整的網路隔離）

### 使用案例

- 安全地執行不可靠或生成的程式碼
- 防止意外修改專案外部的檔案
- 限制自動化任務期間的網路存取

---

## 企業版管理設定

企業版管理設定讓企業管理員可以使用平台原生管理工具，在組織內部署 Claude Code 設定。

### 部署方法

| 平台 | 方法 | 適用版本 |
|----------|--------|-------|
| macOS | 管理 plist 檔案 (MDM) | v2.1.51+ |
| Windows | Windows 登錄機碼 | v2.1.51+ |
| 跨平台 | 管理設定檔案 | v2.1.51+ |
| 跨平台 | 管理的「drop-ins」( `managed-settings.d/` 目錄) | v2.1.83+ |

### 管理的「drop-ins」

從 v2.1.83 開始，管理員可以將多個管理設定檔案部署到 `managed-settings.d/` 目錄。檔案會以字母順序合併，允許團隊之間進行模組化設定：

```
~/.claude/managed-settings.d/
  00-org-defaults.json
  10-team-policies.json
  20-project-overrides.json
```

### 可用的管理設定

| 設定 | 說明 |
|---------|-------------|
| `disableBypassPermissionsMode` | 阻止使用者啟用繞過權限 |
| `availableModels` | 限制使用者可以選擇的模型 |
| `allowedChannelPlugins` | 控制允許哪些頻道外掛 |
| `autoMode.environment` | 設定自動模式的受信任基礎架構 |
| 自訂策略 | 組織特定的權限和工具策略 |

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

## 配置與設定

### 配置文件位置

1. **全域設定**: `~/.claude/config.json`
2. **專案設定**: `./.claude/config.json`
3. **使用者設定**: `~/.config/claude-code/settings.json`

### 完整設定範例

**核心進階功能設定：**

```json
{
  "permissions": {
    "mode": "default"
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

**擴展設定範例：**

```json
{
  "permissions": {
    "mode": "default",
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

### 環境變數

使用環境變數覆寫設定：

```bash
# 模型選擇
export ANTHROPIC_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_OPUS_MODEL=claude-opus-4-6
export ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
export ANTHROPIC_DEFAULT_HAIKU_MODEL=claude-haiku-4-5

# API 設定
export ANTHROPIC_API_KEY=sk-ant-...

# 思考設定
export MAX_THINKING_TOKENS=16000
export CLAUDE_CODE_EFFORT_LEVEL=high

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
export CLAUDE_CODE_SIMPLE=true              # Set by --bare flag

# MCP 設定
export MAX_MCP_OUTPUT_TOKENS=50000
export ENABLE_TOOL_SEARCH=true

# 任務管理
export CLAUDE_CODE_TASK_LIST_ID=my-project-tasks

# 代理團隊 (實驗性)
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# 子代理和外掛設定
export CLAUDE_CODE_SUBAGENT_MODEL=sonnet
export CLAUDE_CODE_PLUGIN_SEED_DIR=./my-plugins
export CLAUDE_CODE_NEW_INIT=1

# 子進程和串流
export CLAUDE_CODE_SUBPROCESS_ENV_SCRUB="SECRET_KEY,DB_PASSWORD"
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=80
export CLAUDE_STREAM_IDLE_TIMEOUT_MS=30000
export ANTHROPIC_CUSTOM_MODEL_OPTION=my-custom-model
export SLASH_COMMAND_TOOL_CHAR_BUDGET=50000
```

### 配置管理指令

/config
[Opens interactive configuration menu]

```

The `/config` command provides an interactive menu to toggle settings such as:
- Extended thinking on/off
- Verbose output
- Permission mode
- Model selection

### Per-Project Configuration

Create `.claude/config.json` in your project:

```json
{
  "hooks": {
    "PreToolUse": [{ "matcher": "Bash", "hooks": ["npm test && npm run lint"] }]
  },
  "permissions": {
    "mode": "default"
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

## Agent Teams

Agent Teams is an experimental feature that enables multiple Claude Code instances to collaborate on a task. It is disabled by default.

### Enabling Agent Teams

Enable via environment variable or settings:

```bash
# Environment variable
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

Or add to your settings JSON:

```json
{
  "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
}
```

### How Agent Teams Work

- A **team lead** coordinates the overall task and delegates subtasks to teammates
- **Teammates** work independently, each with their own context window
- A **shared task list** enables self-coordination between team members
- Use subagent definitions (`.claude/agents/` or `--agents` flag) to define teammate roles and specializations

### Display Modes

Agent Teams support two display modes, configured with the `--teammate-mode` flag:

| Mode | Description |
|------|-------------|
| `in-process` (default) | Teammates run within the same terminal process |
| `tmux` | Each teammate gets a dedicated split pane (requires tmux or iTerm2) |
| `auto` | Automatically selects the best display mode |

```bash
# Use tmux split panes for teammate display
claude --teammate-mode tmux

# Explicitly use in-process mode
claude --teammate-mode in-process
```

### Use Cases

- Large refactoring tasks where different teammates handle different modules
- Parallel code review and implementation
- Coordinated multi-file changes across a codebase

> **Note**: Agent Teams is experimental and may change in future releases. See [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams) for the full reference.

## 最佳實踐

### 計畫模式
- ✅ 用於複雜的多步驟任務
- ✅ 批准前審查計畫
- ✅ 根據需要修改計畫
- ❌ 不要用於簡單任務

### 擴展思考
- ✅ 用於架構決策
- ✅ 用於複雜問題解決
- ✅ 審查思考過程
- ❌ 不要用於簡單查詢

### 背景任務
- ✅ 用於長時間運作的操作
- ✅ 監控任務進度
- ✅ 處理任務失敗
- ❌ 不要啟動過多的並行任務

### 權限
- ✅ 使用 `plan` 進行程式碼審查（僅讀取）
- ✅ 使用 `default` 進行互動式開發
- ✅ 使用 `acceptEdits` 進行自動化工作流程
- ✅ 使用 `auto` 進行具有安全防護的自主工作
- ❌ 除非絕對必要，否則不要使用 `bypassPermissions`

### 會話
- ✅ 對於不同的任務使用獨立的會話
- ✅ 儲存重要的會話狀態
- ✅ 清理舊的會話
- ❌ 不要將無關的工作混合在一個會話中

---

## 額外資源

有關 Claude Code 和相關功能的更多資訊：

- [官方互動模式文件](https://code.claude.com/docs/en/interactive-mode)
- [官方無頭模式文件](https://code.claude.com/docs/en/headless)
- [CLI 參考](https://code.claude.com/docs/en/cli-reference)
- [檢查點指南](../08-checkpoints/) - 會話管理和倒帶
- [斜線命令](../01-slash-commands/) - 命令參考
- [記憶指南](../02-memory/) - 持續的上下文
- [技能指南](../03-skills/) - 自主能力
- [子代理指南](../04-subagents/) - 委派任務執行
- [MCP 指南](../05-mcp/) - 外部資料存取
- [鉤子指南](../06-hooks/) - 事件驅動的自動化
- [外掛指南](../07-plugins/) - 封裝的擴充功能
- [官方排定任務文件](https://code.claude.com/docs/en/scheduled-tasks)
- [官方 Chrome 整合文件](https://code.claude.com/docs/en/chrome)
- [官方遠端控制文件](https://code.claude.com/docs/en/remote-control)
- [官方按鍵綁定文件](https://code.claude.com/docs/en/keybindings)
- [官方桌面應用程式文件](https://code.claude.com/docs/en/desktop)
- [官方代理團隊文件](https://code.claude.com/docs/en/agent-teams)

---
**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/ultraplan
- https://code.claude.com/docs/en/tools-reference
- https://code.claude.com/docs/en/scheduled-tasks
- https://code.claude.com/docs/en/remote-control
- https://code.claude.com/docs/en/agent-teams
**相容模型**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
