<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

<p align="center">
  <a href="https://github.com/trending">
    <img src="https://img.shields.io/badge/GitHub-🔥%20%231%20Trending-purple?style=for-the-badge&logo=github"/>
  </a>
</p>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.1.112-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

🌐 **Language / Ngôn ngữ / 语言 / Мова:** [English](README.md) | [Tiếng Việt](vi/README.md) | [中文](zh/README.md) | [Українська](uk/README.md)

# 在一個週末內精通 Claude Code

從輸入 `claude` 開始，進階到編排代理、鉤子、技能與 MCP 伺服器 — 透過視覺化教學、可直接複製的範本以及引導式學習路徑。

**[15 分鐘快速上手](#get-started-in-15-minutes)** | **[尋找你的程度](#not-sure-where-to-start)** | **[瀏覽功能目錄](CATALOG.md)**

---

## 目錄

- [問題所在](#the-problem)
- [Claude How To 如何解決問題](#how-claude-how-to-fixes-this)
- [運作原理](#how-it-works)
- [不知道從何開始？](#not-sure-where-to-start)
- [15 分鐘快速上手](#get-started-in-15-minutes)
- [你可以用它建立什麼？](#what-can-you-build-with-this)
- [常見問題 (FAQ)](#faq)
- [貢獻指南](#contributing)
- [授權許可](#license)

---

## 問題所在

你安裝了 Claude Code，也執行了一些提示詞。接下來該怎麼辦？

- **官方文件僅描述功能 —— 但沒有教你如何組合使用。** 你知道有斜線命令，但不知道如何將它們與 hooks、memory 和 subagents 串聯成一個能真正節省數小時時間的工作流程。
- **缺乏清晰的學習路徑。** 你應該先學 MCP 再學 hooks 嗎？還是先學 skills 再學 subagents？你最後只能走馬看花，卻無法精通任何一項。
- **範例過於基礎。** 一個「hello world」等級的斜線命令，無法幫助你建立一個能使用 memory、委派給專業 agent 並自動執行安全性掃描的生產級程式碼審查流水線。

你正錯失 Claude Code 90% 的強大功能 —— 而你甚至不知道自己漏掉了什麼。

---

## 《Claude How To》如何解決這個問題

這不是另一份功能參考手冊。這是一份**結構化、視覺化且以範例為驅動的指南**，教你如何利用真實世界的範本來使用每一項 Claude Code 功能，你可以直接將其複製到你的專案中。

| | 官方文件 | 本指南 |
|--|---------------|------------|
| **格式** | 參考文件 | 帶有 Mermaid 圖表的視覺化教學 |
| **深度** | 功能描述 | 底層運作原理 |
| **範例** | 基礎程式碼片段 | 可立即使用的生產級範本 |
| **結構** | 以功能分類 | 進階式學習路徑（從初學者到進階者） |
| **新手引導** | 自主學習 | 帶有時間估計的引導式路線圖 |
| **自我評估** | 無 | 互動式測驗，幫助你發現知識缺口並建立個人化路徑 |

### 你將獲得：

- **10 個教學模組**，涵蓋所有 Claude Code 功能 —— 從斜線命令到自定義 agent 團隊
- **可直接複製的設定** —— 斜線命令、CLAUDE.md 範本、hook 腳本、MCP 設定、subagent 定義以及完整的外掛組合包
- **Mermaid 圖表**，展示每個功能的內部運作方式，讓你理解「為什麼」而不僅僅是「如何做」
- **引導式學習路徑**，讓你從初學者在 11-13 小時內晉升為進階使用者
- **內建自我評估** —— 直接在 Claude Code 中執行 `/self-assessment` 或 `/lesson-quiz hooks` 來找出你的知識缺口

**[開始學習路徑  ->](LEARNING-ROADMAP.md)**

---

## 運作原理

### 1. 找出你的程度

進行 [自我評估測驗](LEARNING-ROADMAP.md#-find-your-level) 或在 Claude Code 中執行 `/self-assessment`。根據你已掌握的知識獲得個人化的學習路線圖。

### 2. 跟隨引導路徑

依序完成 10 個模組 — 每個模組都建立在前一個模組的基礎之上。在學習過程中，直接將範本複製到你的專案中。

### 3. 將功能整合至工作流程

真正的威力在於功能的組合。學習將斜線命令 + 記憶 + 子代理 + 鉤子整合進自動化流水線，以處理程式碼審查、部署和文件生成。

### 4. 測試你的理解程度

在每個模組結束後執行 `/lesson-quiz [topic]`。測驗會精確指出你遺漏的部分，讓你能夠快速填補知識缺口。

**[在 15 分鐘內開始學習](#get-started-in-15-minutes)**

---

## 深受 21,800+ 名開發者信賴

- **21,800+ GitHub stars** 來自每天使用 Claude Code 的開發者
- **2,585+ forks** — 團隊正將此指南改編至他們自己的工作流程中
- **積極維護中** — 與每次 Claude Code 版本同步（最新版本：v2.1.112，2026 年 4 月）
- **社群驅動** — 來自分享真實配置的開發者的貢獻

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不確定從何開始？

進行自我評估或選擇你的程度：

| 程度 | 你可以... | 從這裡開始 | 時間 |
|-------|-----------|------------|------|
| **初學者** | 啟動 Claude Code 並進行對話 | [Slash Commands](01-slash-commands/) | ~2.5 小時 |
| **中級** | 使用 CLAUDE.md 和自定義命令 | [Skills](03-skills/) | ~3.5 小時 |
| **進階** | 配置 MCP servers 和 hooks | [Advanced Features](09-advanced-features/) | ~5 小時 |

**包含所有 10 個模組的完整學習路徑：**

| 順序 | 模組 | 程度 | 時間 |
|-------|--------|-------|------|
| 1 | [Slash Commands](01-slash-commands/) | 初學者 | 30 分鐘 |
| 2 | [Memory](02-memory/) | 初學者+ | 45 分鐘 |
| 3 | [Checkpoints](08-checkpoints/) | 中級 | 45 分鐘 |
| 4 | [CLI Basics](10-cli/) | 初學者+ | 30 分鐘 |
| 5 | [Skills](03-skills/) | 中級 | 1 小時 |
| 6 | [Hooks](06-hooks/) | 中級 | 1 小時 |
| 7 | [MCP](05-mcp/) | 中級+ | 1 小時 |
| 8 | [Subagents](04-subagents/) | 中級+ | 1.5 小時 |
| 9 | [Advanced Features](09-advanced-features/) | 進階 | 2-3 小時 |
| 10 | [Plugins](07-plugins/) | 進階 | 2 小時 |

**[完整學習路線圖 ->](LEARNING-ROADMAP.md)**

---

## 15 分鐘快速上手

```bash
# 1. Clone 指南
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 複製你的第一個斜線命令
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 嘗試使用 — 在 Claude Code 中，輸入：
# /optimize

# 4. 想要更多？設定專案記憶：
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 安裝一個技能：
cp -r 03-skills/code-review ~/.claude/skills/
```

想要完整的設定？這裡有 **1 小時精華設定指南**：

```bash
# 斜線命令 (15 分鐘)
cp 01-slash-commands/*.md .claude/commands/

# 專案記憶 (15 分鐘)
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 安裝一個技能 (15 分鐘)
cp -r 03-skills/code-review ~/.claude/skills/

# 週末目標：新增 hooks、subagents、MCP 與 plugins
# 依照學習路徑進行引導式設定
```

**[查看完整安裝指南](#get-started-in-15-minutes)**

---

## 你可以用這個建立什麼？

| 使用情境 | 你將組合使用的功能 |
|----------|------------------------|
| **自動化程式碼審查** | 斜線命令 + Subagents + 記憶 + MCP |
| **團隊入職培訓** | 記憶 + 斜線命令 + Plugins |
| **CI/CD 自動化** | CLI Reference + Hooks + 背景任務 |
| **文件生成** | 技能 + Subagents + Plugins |
| **安全性稽核** | Subagents + 技能 + Hooks (唯讀模式) |
| **DevOps 流水線** | Plugins + MCP + Hooks + 背景任務 |
| **複雜重構** | 檢查點 + 規劃模式 + Hooks |

---

## FAQ

**這是免費的嗎？**
是的。採用 MIT 授權，永久免費。您可以將其用於個人專案、工作或團隊中 —— 除了包含授權聲明外，沒有其他限制。

**這有在維護嗎？**
有的，正積極維護中。本指南會隨著每一次 Claude Code 的發佈進行同步。目前版本：v2.1.112 (2026 年 4 月)，相容於 Claude Code 2.1+。

**這與官方文件有什麼不同？**
官方文件是功能參考手冊。而本指南是一份包含圖解、生產力等級範本以及漸進式學習路徑的教學指南。兩者相輔相成 —— 您可以從這裡開始學習，並在需要特定細節時查閱官方文件。

**看完所有內容需要多長時間？**
完整學習路徑約需 11-13 小時。但您只需 15 分鐘就能獲得即時價值 —— 只要複製一個斜線命令範本並嘗試使用即可。

**我可以使用於 Claude Sonnet / Haiku / Opus 嗎？**
可以。所有範本皆適用於 Claude Sonnet 4.6、Claude Opus 4.7 與 Claude Haiku 4.5。

**我可以參與貢獻嗎？**
絕對可以。請參閱 [CONTRIBUTING.md](CONTRIBUTING.md) 以了解指南。我們歡迎新的範例、錯誤修復、文件改進以及社群範本。

**我可以離線閱讀嗎？**
可以。執行 `uv run scripts/build_epub.py` 即可生成包含所有內容與渲染圖解的 EPUB 電子書。

---

## 從今天開始精通 Claude Code

您已經安裝了 Claude Code。您與 10 倍生產力之間唯一的差距，就在於是否知道如何使用它。本指南將為您提供結構化的路徑、視覺化解釋以及可直接複製貼上的範本，助您達成目標。

MIT 授權。永久免費。複製它、分支它、讓它成為您的一部分。

**[開始學習路徑 ->](LEARNING-ROADMAP.md)** | **[瀏覽功能目錄](CATALOG.md)** | **[15 分鐘快速上手](#get-started-in-15-minutes)**

---

<details>
<summary>快速導覽 — 所有功能</summary>

| 功能 | 描述 | 資料夾 |
|---------|-------------|--------|
| **功能目錄** | 包含安裝指令的完整參考 | [CATALOG.md](CATALOG.md) |
| **斜線命令** | 使用者觸發的捷徑 | [01-slash-commands/](01-slash-commands/) |
| **記憶** | 持久化的上下文 | [02-memory/](02-memory/) |
| **技能** | 可重複使用的能力 | [03-skills/](03-skills/) |
| **子代理** | 專業化的 AI 助手 | [04-subagents/](04-subagents/) |
| **MCP 協定** | 外部工具存取 | [05-mcp/](05-mcp/) |
| **鉤子** | 事件驅動的自動化 | [06-hooks/](06-hooks/) |
| **外掛** | 綑綁功能 | [07-plugins/](07-plugins/) |
| **檢查點** | 會話快照與回溯 | [08-checkpoints/](08-checkpoints/) |
| **進階功能** | 規劃、思考、背景任務 | [09-advanced-features/](09-advanced-features/) |
| **CLI 參考** | 指令、旗標與選項 | [10-cli/](10-cli/) |
| **部落格文章** | 真實世界的應用範例 | [Blog Posts](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能比較</summary>

| 功能 | 呼叫方式 | 持久性 | 最適合用於 |
|---------|-----------|------------|----------|

| **斜線命令** | 手動 (`/cmd`) | 僅限會話 | 快速捷徑 |
| **記憶** | 自動載入 | 跨會話 | 長期學習 |
| **技能** | 自動調用 | 檔案系統 | 自動化工作流程 |
| **子代理** | 自動委派 | 隔離的上下文 | 任務分配 |
| **MCP Protocol** | 自動查詢 | 即時 | 即時數據存取 |
| **鉤子** | 事件觸發 | 已配置 | 自動化與驗證 |
| **外掛** | 單一命令 | 所有功能 | 完整解決方案 |
| **檢查點** | 手動/自動 | 基於會話 | 安全實驗 |
| **規劃模式** | 手動/自動 | 規劃階段 | 複雜實作 |
| **背景任務** | 手動 | 任務持續時間 | 長時間運行的操作 |
| **CLI Reference** | 終端機命令 | 會話/腳本 | 自動化與腳本編寫 |

</details>

<details>
<summary>安裝快速參考</summary>

```bash
# Slash Commands
cp 01-slash-commands/*.md .claude/commands/

# Memory
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# Skills
cp -r 03-skills/code-review ~/.claude/skills/

# Subagents
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# Hooks
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# Plugins
/plugin install pr-review

# Checkpoints (auto-enabled, configure in settings)
# See 08-checkpoints/README.md

# Advanced Features (configure in settings)
# See 09-advanced-features/config-examples.json

# CLI Reference (no installation needed)
# See 10-cli/README.md for usage examples
```

</details>

<details>
<summary>01. 斜線命令</summary>

**位置**: [01-slash-commands/](01-slash-commands/)

**內容**: 使用者調用的捷徑，以 Markdown 檔案形式儲存

**範例**:
- `optimize.md` - 程式碼優化分析
- `pr.md` - Pull request 準備
- `generate-api-docs.md` - API 文件生成器

**安裝**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**用法**:
```
/optimize
/pr
/generate-api-docs
```

**了解更多**: [Discovering Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. 記憶</summary>

**位置**: [02-memory/](02-memory/)

**內容**: 跨會話的持久化上下文

**範例**:
- `project-CLAUDE.md` - 全團隊專案標準
- `directory-api-CLAUDE.md` - 特定目錄規則
- `personal-CLAUDE.md` - 個人偏好

**安裝**:
```bash
# Project memory
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# Directory memory
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# Personal memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**用法**: 由 Claude 自動載入

</details>

<details>
<summary>03. 技能</summary>

**位置**: [03-skills/](03-skills/)

**內容**: 包含指令與腳本的可重用、自動調用能力

**範例**:

- `code-review/` - 使用腳本進行全面的程式碼審查
- `brand-voice/` - 品牌語氣一致性檢查器
- `doc-generator/` - API 文件產生器

**安裝**:
```bash
# 個人技能
cp -r 03-skills/code-review ~/.claude/skills/

# 專案技能
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**用法**: 當相關時會自動觸發

</details>

<details>
<summary>04. Subagents</summary>

**位置**: [04-subagents/](04-subagents/)

**內容**: 具有隔離上下文與自定義提示詞的專業化 AI 助手

**範例**:
- `code-reviewer.md` - 全面的程式碼品質分析
- `test-engineer.md` - 測試策略與覆蓋率
- `documentation-writer.md` - 技術文件撰寫
- `secure-reviewer.md` - 以安全性為核心的審查（唯讀）
- `implementation-agent.md` - 功能完整實作

**安裝**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**用法**: 由主代理自動委派

</details>

<details>
<summary>05. MCP Protocol</summary>

**位置**: [05-mcp/](05-mcp/)

**內容**: 用於存取外部工具與 API 的 Model Context Protocol

**範例**:
- `github-mcp.json` - GitHub 整合
- `database-mcp.json` - 資料庫查詢
- `filesystem-mcp.json` - 檔案操作
- `multi-mcp.json` - 多個 MCP 伺服器

**安裝**:
```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 透過 CLI 新增 MCP 伺服器
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 或手動新增至專案的 .mcp.json (請參閱 05-mcp/ 中的範例)
```

**用法**: 配置完成後，MCP 工具會自動提供給 Claude 使用

</details>

<details>
<summary>06. Hooks</summary>

**位置**: [06-hooks/](06-hooks/)

**內容**: 事件驅動的 shell 命令，會根據 Claude Code 事件自動執行

**範例**:
- `format-code.sh` - 在寫入前自動格式化程式碼
- `pre-commit.sh` - 在提交前執行測試
- `security-scan.sh` - 掃描安全性問題
- `log-bash.sh` - 記錄所有 bash 命令
- `validate-prompt.sh` - 驗證使用者提示詞
- `notify-team.sh` - 在事件發生時發送通知

**安裝**:
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

在 `~/.claude/settings.json` 中配置鉤子：
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/format-code.sh"]
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "hooks": ["~/.claude/hooks/security-scan.sh"]
    }]
  }
}
```

**用法**: 鉤子會在事件發生時自動執行

**鉤子類型** (4 種類型，25 個事件):
- **工具鉤子 (Tool Hooks)**: `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`
- **會話鉤子 (Session Hooks)**: `SessionStart`, `SessionEnd`, `Stop`, `StopFailure`, `SubagentStart`, `SubagentStop`
- **任務鉤子 (Task Hooks)**: `UserPromptSubmit`, `TaskCompleted`, `TaskCreated`, `TeammateIdle`
- **生命週期鉤子 (Lifecycle Hooks)**: `ConfigChange`, `CwdChanged`, `FileChanged`, `PreCompact`, `PostCompact`, `WorktreeCreate`, `WorktreeRemove`, `Notification`, `InstructionsLoaded`, `Elicitation`, `ElicitationResult`

<details>
<summary>07. Plugins</summary>

**位置**: [07-plugins/](07-plugins/)

**內容**: 綑綁式的指令、代理、MCP 與鉤子集合

**範例**:
- `pr-review/` - 完整的 PR 審查工作流程
- `devops-automation/` - 部署與監控
- `documentation/` - 文件生成

**安裝**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**用法**: 使用綑綁的斜線命令與功能

</details>

<details>
<summary>08. Checkpoints and Rewind</summary>

**位置**: [08-checkpoints/](08-checkpoints/)

**內容**: 儲存對話狀態並回溯到先前的檢查點，以探索不同的方法

**核心概念**:
- **Checkpoint**: 對話狀態的快照
- **Rewind**: 回溯到先前的檢查點
- **Branch Point**: 從同一個檢查點探索多種方法

**用法**:
```
# 每次使用者輸入 prompt 時都會自動建立檢查點
# 若要回溯，請按兩次 Esc 或使用：
/rewind

# 然後從五個選項中進行選擇：
# 1. Restore code and conversation (還原程式碼與對話)
# 2. Restore conversation (還原對話)
# 3. Restore code (還原程式碼)
# 4. Summarize from here (從此處開始摘要)
# 5. Never mind (取消)
```

**使用情境**:
- 嘗試不同的實作方法
- 從錯誤中恢復
- 安全的實驗
- 比較替代方案
- 針對不同設計進行 A/B 測試

</details>

<details>
<summary>09. Advanced Features</summary>

**位置**: [09-advanced-features/](09-advanced-features/)

**內容**: 用於複雜工作流程與自動化的進階功能

**包含**:
- **Planning Mode** — 在寫程式碼之前建立詳細的實作計畫
- **Extended Thinking** — 針對複雜問題的深度推理 (使用 `Alt+T` / `Option+T` 切換)
- **Background Tasks** — 在不阻塞的情況下執行長時間的操作
- **Permission Modes** — `default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions`
- **Headless Mode** — 在 CI/CD 中執行 Claude Code: `claude -p "Run tests and generate report"`
- **Session Management** — `/resume`, `/rename`, `/fork`, `claude -c`, `claude -r`
- **Configuration** — 在 `~/.claude/settings.json` 中自定義行為

完整配置請參閱 [config-examples.json](09-advanced-features/config-examples.json)。

</details>

<details>
<summary>10. CLI Reference</summary>

**位置**: [10-cli/](10-cli/)

**內容**: Claude Code 完整的命令列介面參考指南

**快速範例**:
```bash
# 互動模式
claude "explain this project"

# 列印模式 (非互動式)
claude -p "review this code"

# 處理檔案內容
cat error.log | claude -p "explain this error"

# 用於腳本的 JSON 輸出
claude -p --output-format json "list functions"

# 恢復會話
claude -r "feature-auth" "continue implementation"
```

**使用情境**: CI/CD 流水線整合、腳本自動化、批次處理、多會話工作流程、自定義代理配置

</details>

<details>
<summary>Example Workflows</summary>

### Complete Code Review Workflow

```markdown
# Uses: Slash Commands + Subagents + Memory + MCP
```

User: /review-pr

Claude:
1. Loads project memory (coding standards
2. Fetches PR via GitHub MCP
3. way
4. way
5. way
6. way
7. way
8. way
9. way
10. way
11. way
12. way
13. way
14. way
15. way
16. way
17. way
18. way
19. way
20. way
21. way
22. way
23. way
24. way
25. way
26. way
27. way
28. way
29. way
30. way
31. way
32. way
33. way
34. way
35. way
36. way
37. way
38. way
39. way
40. way
41. way
42. way
43. way
44. way
45. way
46. way
47. way
48. way
49. way
50. way
51. way
52. way
53. way
54. way
55. way
56. way
57. way
58. way
59. way
60. way
61. way
62. way
63. way
64. way
65. way
66. way
67. way
68. way
69. way
70. way
71. way
72. way
73. way
74. way
75. way
76. way
77. way
78. way
79. way
80. way
81. way
82. way
83. way
84. way
85. way
86. way
87. way
88. way
89. way
90. way
91. way
92. way
93. way
94. way
95. way
96. way
97. way
98. way
99. way
100. way
101. way
102. way
103. way
104. way
105. way
106. way
107. way
108. way
109. way
110. way
111. way
112. way
113. way
114. way
115. way
116. way
117. way
118. way
119. way
120. way
121. way
122. way
123. way
124. way
125. way
126. way
127. way
128. way
129. way
130. way
131. way
132. way
133. way
134. way
135. way
136. way
137. way
138. way
139. way
140. way
141. way
142. way
143. way
144. way
145. way
146. way
147. way
148. way
149. way
150. way
151. way
152. way
153. way
154. way
155. way
156. way
157. way
158. way
159. way
160. way
161. way
162. way
163. way
164. way
165. way
166. way
167. way
168. way
169. way
170. way
171. way
172. way
173. way
174. way
175. way
176. way
177. way
178. way
179. way
180. way
181. way
182. way
183. way
184. way
185. way
186. way
187. way
188. way
189. way
190. way
191. way
192. way
193. way
194. way
195. way
196. way
197. way
198. way
199. way
200. way
201. way
202. way
203. way
204. way
205. way
206. way
207. way
208. way
209. way
210. way
211. way
212. way
213. way
214. way
215. way
216. way
217. way
218. way
219. way
220. way
221. way
222. way
223. way
224. way
225. way
226. way
227. way
228. way
229. way
230. way
231. way
232. way
233. way
234. way
235. way
236. way
237. way
238. way
239. way
240. way
241. way
242. way
243. way
244. way
245. way
246. way
247. way
248. way
249. way
250. way
251. way
252. way
253. way
254. way
255. way
256. way
257. way
258. way
259. way
260. way
261. way
262. way
263. way
264. way
265. way
266. way
267. way
268. way
269. way
270. way
271. way
272. way
273. way
274. way
275. way
276. way
277. way
278. way
279. way
280. way
281. way
282. way
283. way
284. way
285. way
286. way
287. way
288. way
289. way
290. way
291. way
292. way
293. way
294. way
295. way
296. way
297. way
298. way
299. way
300. way
301. way
302. way
303. way
304. way
305. way
306. way
307. way
308. way
309. way
310. way
311. way
312. way
313. way
314. way
315. way
316. way
317. way
318. way
319. way
320. way
321. way
322. way
323. way
324. way
325. way
326. way
327. way
328. way
329. way
330. way
331. way
332. way
333. way
334. way
335. way
336. way
337. way
338. way
339. way
340. way
341. way
342. way
343. way
344. way
345. way
346. way
347. way
348. way
349. way
350. way
351. way
352. way
353. way
354. way
355. way
356. way
357. way
358. way
359. way
360. way
361. way
362. way
363. way
364. way
365. way
366. way
367. way
368. way
369. way
370. way
371. way
372. way
373. way
374. way
375. way
376. way
377. way
378. way
379. way
380. way
381. way
382. way
383. way
384. way
385. way
386. way
387. way
388. way
389. way
390. way
391. way
392. way
393. way
394. way
395. way
396. way
397. way
398. way
399. way
400. way
401. way
402. way
403. way
404. way
405. way
406. way
407. way
408. way
409. way
410. way
411. way
412. way
413. way
414. way
415. way
416. way
417. way
418. way
419. way
420. way
421. way
422. way
423. way
424. way
425. way
426. way
427. way
428. way
429. way
430. way
431. way
432. way
433. way
434. way
435. way
436. way
437. way
438. way
439. way
440. way
441. way
442. way
443. way
444. way
445. way
446. way
447. way
448. way
449. way
450. way
451. way
452. way
453. way
454. way
455. way
456. way
457. way
458. way
459. way
460. way
461. way
462. way
463. way
464. way
465. way
466. way
467. way
468. way
469. way
470. way
471. way
472. way
473. way
474. way
475. way
476. way
477. way
478. way
479. way
480. way
481. way
482. way
483. way
484. way
485. way
486. way
487. way
488. way
489. way
490. way
491. way
492. way
493. way
494. way
495. way
496. way
497. way
498. way
499. way
500. way
501. way
502. way
503. way
504. way
505. way
506. way
507. way
508. way
509. way
510. way
511. way
512. way
513. way
514. way
515. way
516. way
517. way
518. way
519. way
520. way
521. way
522. way
523. way
524. way
525. way
526. way
527. way
528. way
529. way
530. way
531. way
532. way
533. way
534. way
535. way
536. way
537. way
538. way
539. way
540. way
541. way
542. way
543. way
544. way
545. way
546. way
547. way
548. way
549. way
550. way
551. way
552. way
553. way
554. way
555. way
556. way
557. way
558. way
559. way
560. way
561. way
562. way
563. way
564. way
565. way
566. way
567. way
568. way
569. way
570. way
571. way
572. way
573. way
574. way
575. way
576. way
577. way
578. way
579. way
580. way
581. way
582. way
583. way
584. way
585. way
586. way
587. way
588. way
589. way
590. way
591. way
592. way
593. way
594. way
595. way
596. way
597. way
598. way
599. way
600. way
601. way
602. way
603. way
604. way
605. way
606. way
607. way
608. way
609. way
610. way
611. way
612. way
613. way
614. way
615. way
616. way
617. way
618. way
619. way
620. way
621. way
622. way
623. way
624. way
625. way
626. way
627. way
628. way
629. way
630. way
631. way
632. way
633. way
634. way
635. way
636. way
637. way
638. way
639. way
640. way
641. way
642. way
643. way
644. way
645. way
646. way
647. way
648. way
649. way
650. way
651. way
652. way
653. way
654. way
655. way
656. way
657. way
658. way
659. way
660. way
661. way
662. way
663. way
664. way
665. way
666. way
667. way
668. way
669. way
670. way
671. way
672. way
673. way
674. way
675. way
676. way
677. way
678. way
679. way
680. way
681. way
682. way
683. way
684. way
685. way
686. way
687. way
688. way
689. way
690. way
691. way
692. way
693. way
694. way
695. way
696. way
697. way
69

1. 驗證環境變數
2. 檢查 MCP server 安裝情況
3. 測試憑證
4. 審查網路連線狀態

### 子代理未進行委派
1. 檢查工具權限
2. 驗證代理描述的清晰度
3. 審查任務複雜度
4. 獨立測試代理

</details>

<details>
<summary>測試</summary>

本專案包含全面的自動化測試：

- **單元測試**：使用 pytest 的 Python 測試 (Python 3.10, 3.11, 3.12)
- **程式碼品質**：使用 Ruff 進行 linting 與格式化
- **安全性**：使用 Bandit 進行漏洞掃描
- **型別檢查**：使用 mypy 進行靜態型別分析
- **構建驗證**：EPUB 生成測試
- **覆蓋率追蹤**：Codecov 整合

```bash
# 安裝開發依賴
uv pip install -r requirements-dev.txt

# 執行所有單元測試
pytest scripts/tests/ -v

# 執行帶有覆蓋率報告的測試
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行程式碼品質檢查
ruff check scripts/
ruff format --check scripts/

# 執行安全性掃描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 執行型別檢查
mypy scripts/ --ignore-missing-imports
```

測試會在每次 push 到 `main`/`develop` 以及每次對 `main` 發送 PR 時自動執行。詳情請參閱 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 生成</summary>

想要離線閱讀本指南嗎？請生成 EPUB 電子書：

```bash
uv run scripts/build_epub.py
```

這將建立包含所有內容（包括渲染後的 Mermaid 圖表）的 `claude-howto-guide.epub`。

更多選項請參閱 [scripts/README.md](scripts/README.md)。

</details>

<details>
<summary>貢獻</summary>

發現問題或想貢獻範例？我們非常歡迎您的參與！

**請閱讀 [CONTRIBUTING.md](CONTRIBUTING.md) 以了解以下詳細指南：**
- 貢獻類型（範例、文件、功能、錯誤、回饋）
- 如何設定您的開發環境
- 目錄結構與如何新增內容
- 寫作指南與最佳實踐
- Commit 與 PR 流程

**我們的社群標準：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我們如何對待彼此
- [SECURITY.md](SECURITY.md) - 安全政策與漏洞回報

### 回報安全性問題

如果您發現安全性漏洞，請負責任地進行回報：

1. **使用 GitHub 私人漏洞回報功能**：https://github.com/luongnv89/claude-howto/security/advisories
2. **或閱讀** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 以獲取詳細說明
3. **切勿** 為安全性漏洞開啟公開的 issue

快速入門：
1. Fork 並 clone 本儲存庫
2. 建立具描述性的分支 (`add/feature-name`, `fix/bug`, `docs/improvement`)
3. 按照指南進行修改
4. 提交一個帶有清晰描述的 pull request

**需要幫助嗎？** 請開啟一個 issue 或 discussion，我們將引導您完成流程。

</details>

<details>
<summary>額外資源</summary>

- [Claude Code Documentation](https://code.claude.com/docs/en/overview)

- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Skills Repository](https://github.com/luongnv89/skills) - 即插即用的技能集合
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) - Claude Code 的創作者分享了他的系統化工作流程：並行代理、共享的 CLAUDE.md、Plan mode、斜線命令、子代理，以及用於自主長時間會話的驗證鉤子。

</details>

---

## Contributing

我們歡迎任何形式的貢獻！請參閱我們的 [Contributing Guide](CONTRIBUTING.md) 以了解如何開始。

---

## License

MIT License - 請參閱 [LICENSE](LICENSE)。可自由使用、修改與分發。唯一的限制是必須包含授權聲明。

---

**Last Updated**: April 16, 2026
**Claude Code Version**: 2.1.112
**Sources**:
- https://docs.anthropic.com/en/docs/claude-code
- https://www.anthropic.com/news/claude-opus-4-7
- https://support.claude.com/en/articles/12138966-release-notes
**Compatible Models**: Claude Sonnet 4.6, Claude Opus 4.7, Claude Haiku 4.5
