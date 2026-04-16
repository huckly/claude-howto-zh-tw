<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 資源列表

## 官方文件

| 資源 | 描述 | 連結 |
|----------|-------------|------|
| Claude Code 文件 | 官方 Claude Code 文件 | [code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview) |
| Anthropic 文件 | 完整的 Anthropic 文件 | [docs.anthropic.com](https://docs.anthropic.com) |
| MCP 協定 | Model Context Protocol 規格 | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| MCP 伺服器 | 官方 MCP 伺服器實作 | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| Anthropic Cookbook | 程式碼範例和教學 | [github.com/anthropics/anthropic-cookbook](https://github.com/anthropics/anthropic-cookbook) |
| Claude Code 技能 | 社群技能儲存庫 | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| Agent Teams | 多代理協調和協作 | [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams) |
| 排程任務 | 使用 `/loop` 和 cron 的重複任務 | [code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks) |
| Chrome 整合 | 瀏覽器自動化 | [code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome) |
| 快捷鍵 | 鍵盤快捷鍵自訂 | [code.claude.com/docs/en/keybindings](https://code.claude.com/docs/en/keybindings) |
| 桌面應用程式 | 原生桌面應用程式 | [code.claude.com/docs/en/desktop](https://code.claude.com/docs/en/desktop) |
| 遠端控制 | 遠端會話控制 | [code.claude.com/docs/en/remote-control](https://code.claude.com/docs/en/remote-control) |
| 自動模式 | 自動權限管理 | [code.claude.com/docs/en/permissions](https://code.claude.com/docs/en/permissions) |
| 頻道 | 多頻道通訊 | [code.claude.com/docs/en/channels](https://code.claude.com/docs/en/channels) |
| 語音輸入 | Claude Code 的語音輸入 | [code.claude.com/docs/en/voice-dictation](https://code.claude.com/docs/en/voice-dictation) |

## Anthropic Engineering Blog

| Article | Description | Link |
|---------|-------------|------|
| Code Execution with MCP | How to solve MCP context bloat using code execution — 98.7% token reduction | [anthropic.com/engineering/code-execution-with-mcp](https://www.anthropic.com/engineering/code-execution-with-mcp) |

---

## Mastering Claude Code in 30 Minutes

_Video_: https://www.youtube.com/watch?v=6eBSHbLKuN0

_**All Tips**_
- **Explore Advanced Features and Shortcuts**
  - 定期查看 Claude 的發布說明，了解其新的程式碼編輯和上下文功能。
  - 學習鍵盤快捷鍵，以便快速切換聊天、檔案和編輯器視窗。

- **Efficient Setup**
  - 建立具有明確名稱/描述的專案特定會話，以便輕鬆檢索。
  - 釘選最常用的檔案或資料夾，以便 Claude 隨時存取。
  - 設置 Claude 的整合 (例如 GitHub、流行的 IDE)，以簡化您的編碼流程。

- **Effective Codebase Q&A**
  - 向 Claude 詢問關於架構、設計模式和特定模組的詳細問題。
  - 在您的問題中使用檔案和行號參考 (例如，「`app/models/user.py` 中的邏輯是什麼？」。
  - 對於大型程式碼庫，請提供摘要或清單，以幫助 Claude 專注。
  - **範例提示詞**: _"可以解釋一下 src/auth/AuthService.ts:45-120 中實作的驗證流程嗎？它如何與 src/middleware/auth.ts 中的中間件整合？"_

- **Code Editing & Refactoring**
  - 在程式碼區塊中使用行內註釋或請求以獲得專注的編輯（“重構這個函數以提高清晰度”）。
  - 請求並排放置編輯前後的比較。
  - 讓 Claude 在進行重大編輯後產生測試或文件以進行品質保證。
  - **範例提示詞**: _"將 api/users.js 中的 getUserData 函數重構為使用 async/await 而不是 promises。請向我展示編輯前後的比較，並為重構後的版本產生單元測試。" _

- **Context Management**
  - 限制您貼上的程式碼/上下文，僅限於與當前任務相關的內容。
  - 使用結構化提示詞（“這裡有檔案 A，這裡有函數 B，我的問題是 X”）以獲得最佳效能。
  - 移除或折疊提示視窗中的大型檔案以避免超出上下文限制。
  - **範例提示詞**: _"這裡有 models/User.js 中的 User 模型和 utils/validation.js 中的 validateUser 函數。我的問題是：如何在不影響向後相容性的情況下添加電子郵件驗證？"_

- **Integrate Team Tools**
  - 將 Claude 會話連接到團隊的儲存庫和文件。
  - 使用內建範本或建立自訂範本以用於重複的工程任務。
  - 通過分享會話記錄和提示詞與團隊成員協作。

- **Boosting Performance**
  - 給 Claude 提供清晰、以目標為導向的指示（例如，“將這個類別總結成五個要點”）。
  - 移除上下文視窗中不必要的註釋和樣板程式碼。
  - 如果 Claude 的輸出偏離軌道，請重置上下文或重新措辭問題以獲得更好的對齊。

- **範例提示詞**: _"摘要說明 src/db/Manager.ts 中的 DatabaseManager 類別，以五個要點呈現，重點在於其主要職責和關鍵方法。"_

- **實用使用範例**
  - 除錯：貼上錯誤和堆疊追蹤，然後詢問可能的起因和解決方案。
  - 測試產生：請求基於屬性、單元或整合測試，用於複雜邏輯。
  - 程式碼審查：請 Claude 找出有風險的變更、邊緣案例或程式碼氣味。
  - **範例提示詞**:
    - _"我收到這個錯誤：'TypeError: Cannot read property 'map' of undefined at line 42 in components/UserList.jsx'。以下是堆疊追蹤和相關程式碼。是什麼導致了這個錯誤，我該如何解決？"_
    - _"為 PaymentProcessor 類別產生全面的單元測試，包括交易失敗、逾時和無效輸入的邊緣案例。"_
    - _"審查這個拉取請求的 diff，找出潛在的安全問題、效能瓶頸和程式碼氣味。"_

- **工作流程自動化**
  - 使用 Claude 提示詞來處理重複性任務（例如格式設定、清理和重複重新命名）。
  - 使用 Claude 來草擬 PR 說明、發布說明或文件，基於程式碼 diff。
  - **範例提示詞**: _"基於 git diff，建立詳細的 PR 說明，其中包含變更摘要、已修改檔案清單、測試步驟和潛在影響。同時產生版本 2.3.0 的發布說明。"_

**提示**: 為了獲得最佳效果，請結合多種這些方法：先固定關鍵檔案並摘要說明您的目標，然後使用專注的提示詞和 Claude 的重構工具，逐步改進您的程式碼庫和自動化流程。


**與 Claude Code 協同工作的工作流程**

### 與 Claude Code 協同工作的工作流程

#### 對於新的存放庫

1. **初始化存放庫和 Claude 整合**
   - 建立具有必要結構的新存放庫：README、LICENSE、.gitignore、root configs。
   - 建立一個 `CLAUDE.md` 檔案，描述架構、高階目標和編碼指南。
   - 安裝 Claude Code 並將其與您的存放庫連結，以獲得程式碼建議、測試腳手架和工作流程自動化。

2. **使用計畫模式和規格**
   - 使用計畫模式 (`shift-tab` 或 `/plan`) 在實作功能之前先草擬詳細的規格。
   - 請求 Claude 提出架構建議和初始專案佈局。
   - 保持清晰、以目標為導向的提示詞序列：請求元件大綱、主要模組和職責。

3. **迭代開發和審查**
   - 以小塊實作核心功能，請求 Claude 產生程式碼、重構和文件。
   - 每次增量後請求單元測試和範例。
   - 在 CLAUDE.md 中維護正在進行的任務清單。

4. **自動化 CI/CD 和部署**
   - 使用 Claude 來建立 GitHub Actions、npm/yarn 腳本或部署工作流程。
   - 透過更新您的 CLAUDE.md 並請求對應的命令/腳本，輕鬆調整流水線。

```mermaid
graph TD
    A[開始新的存放庫] --> B[初始化存放庫結構]

#### 對現有儲存機

1. **儲存機與上下文設定**
   - 新增或更新 `CLAUDE.md` 以記錄儲存機結構、編碼模式和關鍵檔案。對於舊有儲存機，請使用 `CLAUDE_LEGACY.md`，其中涵蓋框架、版本地圖、說明、錯誤和升級注意事項。
   - 鎖定或強調顯示 Claude 應該用於上下文的主要檔案。

2. **上下文程式碼 Q&A**
   - 請求 Claude 進行程式碼審查、錯誤說明、重構或遷移計畫，並參考特定的檔案/函式。
   - 為 Claude 提供明確的界限（例如，「僅修改這些檔案」或「不增加任何新的相依性」）。

3. **分支、工作樹和多會話管理**
   - 使用多個 git 工作樹來隔離功能或錯誤修正，並為每個工作樹啟動獨立的 Claude 會話。
   - 保持終端標籤/視窗根據分支或功能進行組織，以進行並行工作流程。

4. **團隊工具和自動化**
   - 透過 `.claude/commands/` 同步自訂命令，以確保團隊之間的一致性。
   - 透過 Claude 的斜線命令或鉤子來自動化重複性任務、PR 建立和程式碼格式設定。
   - 與團隊成員共享會話和上下文，以進行協同式問題排除和審查。

```mermaid
graph TD
    A[從現有儲存機開始] --> B{舊有程式碼庫？}
    B -->|是| C[建立 CLAUDE_LEGACY.md]
    B -->|否| D[建立/更新 CLAUDE.md]
    C --> E[記錄框架和版本地圖]
    D --> F[記錄結構和模式]
    E --> G[鎖定關鍵檔案以用於上下文]
    F --> G

    G --> H[識別任務類型]
    H --> I{任務類別}
    I -->|錯誤修正| J[請求 Claude 進行錯誤分析]
    I -->|程式碼審查| K[請求程式碼審查]
    I -->|重構| L[制定重構策略]
    I -->|遷移| M[建立遷移計畫]

    J --> N[設定明確的界限]
    K --> N
    L --> N
    M --> N

    N --> O{多個功能？}
    O -->|是| P[建立 Git 工作樹]
    O -->|否| Q[在主要分支上進行作業]
    P --> R[啟動獨立的 Claude 會話]
    R --> S[組織終端標籤]
    Q --> S

    S --> T[設定團隊自動化]
    T --> U[同步 .claude/commands/]
    U --> V[設定斜線命令]
    V --> W[設定鉤子以進行自動化]
    W --> X[與團隊共享會話上下文]
```

X --> Y{More Tasks?}
Y -->|Yes| H
Y -->|No| Z[Workflow Complete]

```
style A fill:#e1f5ff
style C fill:#ffecec
style D fill:#fff4e1
style P fill:#f0ffe1
style T fill:#ffe1f5
style Z fill:#90EE90
```

**Tips**:
- 針對新功能或修復，請先以規格和計畫模式提示。
- 對於過時且複雜的專案，請在 CLAUDE.md/CLAUDE_LEGACY.md 中儲存詳細的指導。
- 提供清晰且專注的指示，並將複雜的工作分解為多階段計畫。
- 定期清理會話，修剪上下文，並移除已完成的工作樹，以避免雜亂。

這些步驟捕捉了在既有和新專案中使用 Claude Code 的核心建議，以實現順暢的工作流程。

---

## 新功能與功能 (2026 年 3 月)

### 關鍵功能資源

| 功能 | 描述 | 瞭解更多 |
|---------|-------------|------------|
| **自動記憶** | Claude 會自動學習並記住您在不同會話中的偏好 | [記憶指南](02-memory/) |
| **遠端控制** | 從外部工具和腳本程式化地控制 Claude Code 會話 | [進階功能](09-advanced-features/) |
| **網頁會話** | 透過瀏覽器介面存取 Claude Code，用於遠端開發 | [CLI 參考](10-cli/) |
| **桌面應用程式** | 具有增強型 UI 的 Claude Code 原生桌面應用程式 | [Claude Code 文件](https://code.claude.com/docs/en/desktop) |
| **深度思考** | 透過 `Alt+T`/`Option+T` 或 `MAX_THINKING_TOKENS` 環境變數啟用深度推理功能 | [進階功能](09-advanced-features/) |
| **權限模式** | 細粒度的控制：預設、acceptEdits、plan、auto、dontAsk、bypassPermissions | [進階功能](09-advanced-features/) |
| **七層記憶** | 策略管理、專案、專案規則、使用者、使用者規則、本機、自動記憶 | [記憶指南](02-memory/) |
| **鉤子事件** | 25 個事件：PreToolUse、PostToolUse、PostToolUseFailure、Stop、StopFailure、SubagentStart、SubagentStop、Notification、Elicitation 等 | [鉤子指南](06-hooks/) |
| **代理團隊** | 協調多個代理共同處理複雜任務 | [子代理指南](04-subagents/) |
| **排程任務** | 使用 `/loop` 和 cron 工具設定重複性任務 | [進階功能](09-advanced-features/) |
| **Chrome 整合** | 瀏覽器自動化，使用無頭 Chromium | [進階功能](09-advanced-features/) |
| **鍵盤自訂** | 自訂按鍵綁定，包括和弦序列 | [進階功能](09-advanced-features/) |
| **監控工具** | 觀看背景命令的 stdout 流，並根據事件做出反應，而不是輪詢 (v2.1.98+) | [進階功能](09-advanced-features/) |

---
**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/overview
