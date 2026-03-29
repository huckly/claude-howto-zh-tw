<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 優質資源清單

## 官方文件

| 資源 | 說明 | 連結 |
|------|------|------|
| Claude Code 文件 | Claude Code 官方文件 | [code.claude.com/docs/en/overview](https://code.claude.com/docs/en/overview) |
| Anthropic 文件 | 完整 Anthropic 文件 | [docs.anthropic.com](https://docs.anthropic.com) |
| MCP 協議 | Model Context Protocol 規格 | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| MCP 伺服器 | 官方 MCP 伺服器實作 | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| Anthropic Cookbook | 程式碼範例和教學 | [github.com/anthropics/anthropic-cookbook](https://github.com/anthropics/anthropic-cookbook) |
| Claude Code Skills | 社群技能倉庫 | [github.com/anthropics/skills](https://github.com/anthropics/skills) |
| Agent Teams | 多代理協調與協作 | [code.claude.com/docs/en/agent-teams](https://code.claude.com/docs/en/agent-teams) |
| Scheduled Tasks | 使用 /loop 和 cron 的排程任務 | [code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks) |
| Chrome 整合 | 瀏覽器自動化 | [code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome) |
| Keybindings | 鍵盤快捷鍵自訂 | [code.claude.com/docs/en/keybindings](https://code.claude.com/docs/en/keybindings) |
| Desktop App | 原生桌面應用程式 | [code.claude.com/docs/en/desktop](https://code.claude.com/docs/en/desktop) |
| Remote Control | 遠端會話控制 | [code.claude.com/docs/en/remote-control](https://code.claude.com/docs/en/remote-control) |
| Auto Mode | 自動權限管理 | [code.claude.com/docs/en/auto-mode](https://code.claude.com/docs/en/auto-mode) |
| Channels | 多頻道通訊 | [code.claude.com/docs/en/channels](https://code.claude.com/docs/en/channels) |
| Voice Dictation | Claude Code 語音輸入 | [code.claude.com/docs/en/voice-dictation](https://code.claude.com/docs/en/voice-dictation) |

## Anthropic 工程部落格

| 文章 | 說明 | 連結 |
|------|------|------|
| Code Execution with MCP | 如何使用程式碼執行解決 MCP 情境膨脹——減少 98.7% token | [anthropic.com/engineering/code-execution-with-mcp](https://www.anthropic.com/engineering/code-execution-with-mcp) |

---

## 30 分鐘精通 Claude Code

_影片_: https://www.youtube.com/watch?v=6eBSHbLKuN0

_**所有技巧**_
- **探索進階功能和快捷鍵**
  - 定期查看 Claude 發行說明中的新程式碼編輯和情境功能。
  - 學習快捷鍵，快速切換聊天、檔案和編輯器視圖。

- **高效設置**
  - 使用清晰的名稱/描述建立專案特定的會話，方便日後查找。
  - 固定常用的檔案或資料夾，讓 Claude 隨時存取。
  - 設定 Claude 的整合（如 GitHub、主流 IDE），簡化你的程式開發流程。

- **有效的程式碼庫問答**
  - 詢問 Claude 關於架構、設計模式和特定模組的詳細問題。
  - 在問題中使用檔案和行號引用（如「`app/models/user.py` 中的邏輯實現了什麼？」）。
  - 對於大型程式碼庫，提供摘要或清單幫助 Claude 聚焦。
  - **範例提示**: _「能否解釋 src/auth/AuthService.ts:45-120 中實現的認證流程？它如何與 src/middleware/auth.ts 中的中間件整合？」_

- **程式碼編輯和重構**
  - 使用內聯注釋或程式碼塊中的請求來獲得精確的編輯（「重構這個函數使其更清晰」）。
  - 請求並排的前後對比。
  - 讓 Claude 在重大編輯後生成測試或文件以確保品質。
  - **範例提示**: _「重構 api/users.js 中的 getUserData 函數，使用 async/await 替代 promises。向我展示前後對比，並為重構後的版本生成單元測試。」_

- **情境管理**
  - 僅粘貼與當前任務相關的程式碼/情境。
  - 使用結構化提示（「這是檔案 A，這是函數 B，我的問題是 X」）以獲得最佳性能。
  - 在提示窗口中刪除或折疊大型檔案，避免超過情境限制。

- **整合團隊工具**
  - 將 Claude 會話連接到你的團隊倉庫和文件。
  - 使用內置範本或為重複的工程任務創建自訂範本。
  - 通過與隊友共享會話記錄和提示進行協作。

- **提升性能**
  - 給 Claude 清晰、目標導向的指令（如「以五個要點總結這個類」）。
  - 從情境窗口中刪除不必要的注釋和樣板代碼。
  - 如果 Claude 的輸出偏離軌道，重置情境或重新表述問題以獲得更好的結果。

- **實際使用範例**
  - 除錯：粘貼錯誤和堆疊追蹤，然後詢問可能的原因和修復方案。
  - 測試生成：請求對複雜邏輯進行基於屬性的、單元的或整合測試。
  - 程式碼審查：請 Claude 識別危險的變更、邊緣情況或程式碼壞味道。

- **工作流自動化**
  - 使用 Claude 提示腳本化重複任務（如格式化、清理和重複重命名）。
  - 使用 Claude 根據程式碼差異起草 PR 描述、發行說明或文件。

**提示**：為獲得最佳效果，結合多種實踐——從固定關鍵檔案和總結目標開始，然後使用精確提示和 Claude 的重構工具逐步改進你的程式碼庫和自動化。

---

## 新功能和能力（2026 年 3 月）

### 關鍵功能資源

| 功能 | 說明 | 了解更多 |
|------|------|---------|
| **Auto Memory** | Claude 自動學習並記住你跨會話的偏好 | [記憶指南](02-memory/) |
| **Remote Control** | 以程式化方式從外部工具和腳本控制 Claude Code 會話 | [進階功能](09-advanced-features/) |
| **Web Sessions** | 通過基於瀏覽器的界面訪問 Claude Code 進行遠端開發 | [CLI 參考](10-cli/) |
| **Desktop App** | 具有增強 UI 的 Claude Code 原生桌面應用程式 | [Claude Code 文件](https://code.claude.com/docs/en/desktop) |
| **Extended Thinking** | 通過 `Alt+T`/`Option+T` 或 `MAX_THINKING_TOKENS` 環境變數切換深度推理 | [進階功能](09-advanced-features/) |
| **Permission Modes** | 細粒度控制：default、acceptEdits、plan、auto、dontAsk、bypassPermissions | [進階功能](09-advanced-features/) |
| **7 層記憶** | 受管策略、專案、專案規則、使用者、使用者規則、本地、Auto Memory | [記憶指南](02-memory/) |
| **Hook 事件（25 個）** | PreToolUse、PostToolUse、PostToolUseFailure、Stop、StopFailure、SubagentStart、SubagentStop、Notification、Elicitation 等 | [Hooks 指南](06-hooks/) |
| **Agent Teams** | 協調多個代理協同處理複雜任務 | [Subagents 指南](04-subagents/) |
| **Scheduled Tasks** | 使用 `/loop` 和 cron 工具設置排程任務 | [進階功能](09-advanced-features/) |
| **Chrome 整合** | 使用 headless Chromium 進行瀏覽器自動化 | [進階功能](09-advanced-features/) |
| **鍵盤自訂** | 自訂快捷鍵，包括和弦序列 | [進階功能](09-advanced-features/) |
