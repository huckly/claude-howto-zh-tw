<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

[![GitHub Stars](https://img.shields.io/github/stars/luongnv89/claude-howto?style=flat&color=gold)](https://github.com/luongnv89/claude-howto/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/luongnv89/claude-howto?style=flat)](https://github.com/luongnv89/claude-howto/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.2.0-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

# 週末精通 Claude Code

從輸入 `claude` 開始，到編排 agents、hooks、skills 和 MCP servers——附帶視覺化教學、可直接複製的範本，以及引導式學習路徑。

**[15 分鐘快速開始](#-15-分鐘快速開始)** | **[找到適合你的程度](#-不確定從哪裡開始)** | **[瀏覽功能目錄](CATALOG.md)**

---

## 目錄

- [問題所在](#問題所在)
- [Claude How To 如何解決這些問題](#claude-how-to-如何解決這些問題)
- [運作方式](#運作方式)
- [不確定從哪裡開始？](#-不確定從哪裡開始)
- [15 分鐘快速開始](#-15-分鐘快速開始)
- [你能用這個做什麼？](#你能用這個做什麼)
- [常見問題](#常見問題)
- [貢獻指南](#貢獻指南)
- [授權條款](#授權條款)

---

## 問題所在

你已經安裝了 Claude Code，也執行過幾個提示。然後呢？

- **官方文件描述功能，但沒告訴你如何組合使用。** 你知道 slash commands 的存在，卻不知道如何把它們與 hooks、memory 和 subagents 串接成真正能節省數小時工作的工作流程。
- **沒有清晰的學習路徑。** 應該先學 MCP 還是 hooks？先學 skills 還是 subagents？最後你翻遍所有東西，卻什麼都沒真正掌握。
- **範例太基礎。** 一個「hello world」的 slash command 無法幫助你建立一個使用 memory、委派給專門 agents、並自動執行安全掃描的生產級程式碼審查流水線。

你正在浪費 Claude Code 90% 的能力——而且你甚至不知道自己不知道什麼。

---

## Claude How To 如何解決這些問題

這不是另一份功能參考文件。這是一份**結構化、視覺化、以範例為導向的指南**，透過你今天就能複製到專案中的真實範本，教你使用每一個 Claude Code 功能。

| | 官方文件 | 本指南 |
|--|---------------|------------|
| **格式** | 參考文件 | 附 Mermaid 圖表的視覺化教學 |
| **深度** | 功能描述 | 深入底層原理 |
| **範例** | 基礎片段 | 可立即使用的生產就緒範本 |
| **結構** | 功能導向 | 漸進式學習路徑（入門到進階）|
| **引導** | 自學 | 附時間估算的引導路線圖 |
| **自我評估** | 無 | 互動式測驗，找出你的知識空缺並建立個人化路徑 |

### 你將獲得：

- **10 個教學模組**，涵蓋每一個 Claude Code 功能——從 slash commands 到自訂 agent 團隊
- **可複製的配置**——slash commands、CLAUDE.md 範本、hook 腳本、MCP 配置、subagent 定義，以及完整 plugin 套件
- **Mermaid 圖表**，展示每個功能的內部運作方式，讓你理解*為什麼*，而不只是*如何*
- **引導式學習路徑**，帶你在 11-13 小時內從入門到進階使用者
- **內建自我評估**——在 Claude Code 中直接執行 `/self-assessment` 或 `/lesson-quiz hooks` 來找出知識空缺

**[開始學習路徑 ->](LEARNING-ROADMAP.md)**

---

## 運作方式

### 1. 找到你的程度

進行[自我評估測驗](LEARNING-ROADMAP.md#-find-your-level)，或在 Claude Code 中執行 `/self-assessment`。根據你已有的知識獲得個人化路線圖。

### 2. 按引導路徑學習

按順序完成 10 個模組——每個都建立在上一個的基礎上。學習過程中直接將範本複製到你的專案。

### 3. 將功能組合成工作流程

真正的威力在於功能的組合。學習如何將 slash commands + memory + subagents + hooks 串接成自動化流水線，處理程式碼審查、部署和文件生成。

### 4. 測試你的理解

每個模組完成後執行 `/lesson-quiz [topic]`。測驗會找出你遺漏的地方，讓你快速補足知識空缺。

**[15 分鐘快速開始](#-15-分鐘快速開始)**

---

## 3,900+ 開發者的信賴

- **3,900+ GitHub 星標**，來自每天使用 Claude Code 的開發者
- **460+ Fork**——團隊將本指南改編成自己的工作流程
- **持續維護**——與每個 Claude Code 版本同步更新（最新：v2.2.0，2026 年 3 月）
- **社群驅動**——來自分享真實配置的開發者貢獻

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不確定從哪裡開始？

進行自我評估，或按程度選擇：

| 程度 | 你能做到... | 從這裡開始 | 時間 |
|-------|-----------|------------|------|
| **入門** | 啟動 Claude Code 並聊天 | [Slash Commands](01-slash-commands/) | ~2.5 小時 |
| **中級** | 使用 CLAUDE.md 和自訂指令 | [Skills](03-skills/) | ~3.5 小時 |
| **進階** | 配置 MCP servers 和 hooks | [進階功能](09-advanced-features/) | ~5 小時 |

**完整學習路徑，包含所有 10 個模組：**

| 順序 | 模組 | 程度 | 時間 |
|-------|--------|-------|------|
| 1 | [Slash Commands](01-slash-commands/) | 入門 | 30 分鐘 |
| 2 | [Memory](02-memory/) | 入門+ | 45 分鐘 |
| 3 | [Checkpoints](08-checkpoints/) | 中級 | 45 分鐘 |
| 4 | [CLI 基礎](10-cli/) | 入門+ | 30 分鐘 |
| 5 | [Skills](03-skills/) | 中級 | 1 小時 |
| 6 | [Hooks](06-hooks/) | 中級 | 1 小時 |
| 7 | [MCP](05-mcp/) | 中級+ | 1 小時 |
| 8 | [Subagents](04-subagents/) | 中級+ | 1.5 小時 |
| 9 | [進階功能](09-advanced-features/) | 進階 | 2-3 小時 |
| 10 | [Plugins](07-plugins/) | 進階 | 2 小時 |

**[完整學習路線圖 ->](LEARNING-ROADMAP.md)**

---

## 15 分鐘快速開始

```bash
# 1. 複製指南
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 複製你的第一個 slash command
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 試試看——在 Claude Code 中輸入：
# /optimize

# 4. 準備好更多了嗎？設定專案 memory：
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 安裝一個 skill：
cp -r 03-skills/code-review ~/.claude/skills/
```

想要完整設定？以下是 **1 小時基礎設定**：

```bash
# Slash commands（15 分鐘）
cp 01-slash-commands/*.md .claude/commands/

# 專案 memory（15 分鐘）
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 安裝 skill（15 分鐘）
cp -r 03-skills/code-review ~/.claude/skills/

# 週末目標：新增 hooks、subagents、MCP 和 plugins
# 按照學習路徑進行引導設定
```

**[查看完整安裝參考](#安裝快速參考)**

---

## 你能用這個做什麼？

| 使用場景 | 你將組合的功能 |
|----------|------------------------|
| **自動化程式碼審查** | Slash Commands + Subagents + Memory + MCP |
| **團隊新人引導** | Memory + Slash Commands + Plugins |
| **CI/CD 自動化** | CLI 參考 + Hooks + 背景任務 |
| **文件生成** | Skills + Subagents + Plugins |
| **安全審計** | Subagents + Skills + Hooks（唯讀模式） |
| **DevOps 流水線** | Plugins + MCP + Hooks + 背景任務 |
| **複雜重構** | Checkpoints + 規劃模式 + Hooks |

---

## 常見問題

**這是免費的嗎？**
是的。MIT 授權，永久免費。可用於個人專案、工作、團隊——唯一要求是保留授權聲明。

**這有在維護嗎？**
有持續維護。本指南與每個 Claude Code 版本同步更新。目前版本：v2.2.0（2026 年 3 月），相容於 Claude Code 2.1+。

**這與官方文件有何不同？**
官方文件是功能參考。本指南是附有圖表、生產就緒範本和漸進式學習路徑的教學。兩者互補——先在這裡學習，需要具體細節時再查閱官方文件。

**讀完全部需要多長時間？**
完整路徑需要 11-13 小時。但你在 15 分鐘內就能獲得立即的價值——只需複製一個 slash command 範本並試用即可。

**我可以使用 Claude Sonnet / Haiku / Opus 嗎？**
可以。所有範本都適用於 Claude Sonnet 4.6、Claude Opus 4.6 和 Claude Haiku 4.5。

**我可以貢獻嗎？**
當然可以。請參閱 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南。歡迎提供新範例、錯誤修正、文件改進和社群範本。

**我可以離線閱讀嗎？**
可以。執行 `uv run scripts/build_epub.py` 生成包含所有內容和渲染圖表的 EPUB 電子書。

---

## 立即開始精通 Claude Code

你已經安裝了 Claude Code。你與 10 倍生產力之間唯一的距離，是知道如何使用它。本指南為你提供結構化路徑、視覺化說明和可複製的範本。

MIT 授權。永久免費。複製它、Fork 它、讓它成為你的。

**[開始學習路徑 ->](LEARNING-ROADMAP.md)** | **[瀏覽功能目錄](CATALOG.md)** | **[15 分鐘快速開始](#-15-分鐘快速開始)**

---

<details>
<summary>快速導覽——所有功能</summary>

| 功能 | 描述 | 資料夾 |
|---------|-------------|--------|
| **功能目錄** | 附安裝指令的完整參考 | [CATALOG.md](CATALOG.md) |
| **Slash Commands** | 使用者觸發的快捷方式 | [01-slash-commands/](01-slash-commands/) |
| **Memory** | 持久化上下文 | [02-memory/](02-memory/) |
| **Skills** | 可重複使用的能力 | [03-skills/](03-skills/) |
| **Subagents** | 專門化的 AI 助理 | [04-subagents/](04-subagents/) |
| **MCP Protocol** | 外部工具存取 | [05-mcp/](05-mcp/) |
| **Hooks** | 事件驅動的自動化 | [06-hooks/](06-hooks/) |
| **Plugins** | 功能套件 | [07-plugins/](07-plugins/) |
| **Checkpoints** | 會話快照與回溯 | [08-checkpoints/](08-checkpoints/) |
| **進階功能** | 規劃、思考、背景任務 | [09-advanced-features/](09-advanced-features/) |
| **CLI 參考** | 指令、旗標和選項 | [10-cli/](10-cli/) |
| **部落格文章** | 真實使用範例 | [Blog Posts](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能比較</summary>

| 功能 | 觸發方式 | 持久性 | 最適用於 |
|---------|-----------|------------|----------|
| **Slash Commands** | 手動（`/cmd`）| 僅限會話 | 快速捷徑 |
| **Memory** | 自動載入 | 跨會話 | 長期學習 |
| **Skills** | 自動觸發 | 檔案系統 | 自動化工作流程 |
| **Subagents** | 自動委派 | 獨立上下文 | 任務分工 |
| **MCP Protocol** | 自動查詢 | 即時 | 即時資料存取 |
| **Hooks** | 事件觸發 | 已配置 | 自動化與驗證 |
| **Plugins** | 單一指令 | 所有功能 | 完整解決方案 |
| **Checkpoints** | 手動/自動 | 基於會話 | 安全實驗 |
| **規劃模式** | 手動/自動 | 規劃階段 | 複雜實作 |
| **背景任務** | 手動 | 任務期間 | 長時間執行的操作 |
| **CLI 參考** | 終端機指令 | 會話/腳本 | 自動化與腳本 |

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

# Checkpoints（自動啟用，在設定中配置）
# 請參閱 08-checkpoints/README.md

# 進階功能（在設定中配置）
# 請參閱 09-advanced-features/config-examples.json

# CLI 參考（無需安裝）
# 請參閱 10-cli/README.md 中的使用範例
```

</details>

<details>
<summary>01. Slash Commands</summary>

**位置**：[01-slash-commands/](01-slash-commands/)

**說明**：以 Markdown 檔案儲存的使用者觸發快捷方式

**範例**：
- `optimize.md` - 程式碼最佳化分析
- `pr.md` - Pull request 準備
- `generate-api-docs.md` - API 文件生成器

**安裝**：
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**使用方式**：
```
/optimize
/pr
/generate-api-docs
```

**延伸閱讀**：[探索 Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. Memory</summary>

**位置**：[02-memory/](02-memory/)

**說明**：跨會話的持久化上下文

**範例**：
- `project-CLAUDE.md` - 全團隊專案標準
- `directory-api-CLAUDE.md` - 目錄特定規則
- `personal-CLAUDE.md` - 個人偏好

**安裝**：
```bash
# 專案 memory
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# 目錄 memory
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 個人 memory
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**使用方式**：由 Claude 自動載入

</details>

<details>
<summary>03. Skills</summary>

**位置**：[03-skills/](03-skills/)

**說明**：可重複使用、自動觸發的能力，包含指令和腳本

**範例**：
- `code-review/` - 附腳本的全面程式碼審查
- `brand-voice/` - 品牌聲音一致性檢查
- `doc-generator/` - API 文件生成器

**安裝**：
```bash
# 個人 skills
cp -r 03-skills/code-review ~/.claude/skills/

# 專案 skills
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**使用方式**：相關時自動觸發

</details>

<details>
<summary>04. Subagents</summary>

**位置**：[04-subagents/](04-subagents/)

**說明**：具有獨立上下文和自訂提示的專門化 AI 助理

**範例**：
- `code-reviewer.md` - 全面的程式碼品質分析
- `test-engineer.md` - 測試策略和覆蓋率
- `documentation-writer.md` - 技術文件
- `secure-reviewer.md` - 安全導向審查（唯讀）
- `implementation-agent.md` - 完整功能實作

**安裝**：
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**使用方式**：由主 agent 自動委派

</details>

<details>
<summary>05. MCP Protocol</summary>

**位置**：[05-mcp/](05-mcp/)

**說明**：Model Context Protocol，用於存取外部工具和 API

**範例**：
- `github-mcp.json` - GitHub 整合
- `database-mcp.json` - 資料庫查詢
- `filesystem-mcp.json` - 檔案操作
- `multi-mcp.json` - 多個 MCP servers

**安裝**：
```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 透過 CLI 新增 MCP server
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 或手動新增到專案 .mcp.json（請參閱 05-mcp/ 中的範例）
```

**使用方式**：配置後 MCP 工具自動提供給 Claude 使用

</details>

<details>
<summary>06. Hooks</summary>

**位置**：[06-hooks/](06-hooks/)

**說明**：事件驅動的 shell 指令，自動回應 Claude Code 事件執行

**範例**：
- `format-code.sh` - 寫入前自動格式化程式碼
- `pre-commit.sh` - 提交前執行測試
- `security-scan.sh` - 掃描安全問題
- `log-bash.sh` - 記錄所有 bash 指令
- `validate-prompt.sh` - 驗證使用者提示
- `notify-team.sh` - 在事件發生時發送通知

**安裝**：
```bash
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh
```

在 `~/.claude/settings.json` 中配置 hooks：
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

**使用方式**：Hooks 在事件發生時自動執行

**Hook 類型**（4 種類型，25 個事件）：
- **工具 Hooks**：`PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`
- **會話 Hooks**：`SessionStart`、`SessionEnd`、`Stop`、`StopFailure`、`SubagentStart`、`SubagentStop`
- **任務 Hooks**：`UserPromptSubmit`、`TaskCompleted`、`TaskCreated`、`TeammateIdle`
- **生命週期 Hooks**：`ConfigChange`、`CwdChanged`、`FileChanged`、`PreCompact`、`PostCompact`、`WorktreeCreate`、`WorktreeRemove`、`Notification`、`InstructionsLoaded`、`Elicitation`、`ElicitationResult`

</details>

<details>
<summary>07. Plugins</summary>

**位置**：[07-plugins/](07-plugins/)

**說明**：commands、agents、MCP 和 hooks 的套件集合

**範例**：
- `pr-review/` - 完整 PR 審查工作流程
- `devops-automation/` - 部署和監控
- `documentation/` - 文件生成

**安裝**：
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**使用方式**：使用套件中的 slash commands 和功能

</details>

<details>
<summary>08. Checkpoints 與回溯</summary>

**位置**：[08-checkpoints/](08-checkpoints/)

**說明**：儲存對話狀態並回溯到前一個時間點，探索不同方法

**關鍵概念**：
- **Checkpoint**：對話狀態的快照
- **Rewind**：返回前一個 checkpoint
- **Branch Point**：從同一個 checkpoint 探索多種方法

**使用方式**：
```
# 每次使用者提示時自動建立 checkpoints
# 若要回溯，按兩次 Esc 或使用：
/rewind

# 然後從五個選項中選擇：
# 1. 還原程式碼和對話
# 2. 還原對話
# 3. 還原程式碼
# 4. 從此處摘要
# 5. 取消
```

**使用場景**：
- 嘗試不同的實作方式
- 從錯誤中恢復
- 安全實驗
- 比較替代方案
- A/B 測試不同設計

</details>

<details>
<summary>09. 進階功能</summary>

**位置**：[09-advanced-features/](09-advanced-features/)

**說明**：複雜工作流程和自動化的進階能力

**包含**：
- **規劃模式** — 在撰寫程式碼前建立詳細的實作計畫
- **延伸思考** — 複雜問題的深度推理（用 `Alt+T` / `Option+T` 切換）
- **背景任務** — 在不阻塞對話的情況下執行長時間操作
- **權限模式** — `default`、`acceptEdits`、`plan`、`dontAsk`、`bypassPermissions`
- **Headless 模式** — 在 CI/CD 中執行 Claude Code：`claude -p "Run tests and generate report"`
- **會話管理** — `/resume`、`/rename`、`/fork`、`claude -c`、`claude -r`
- **配置** — 在 `~/.claude/settings.json` 中自訂行為

完整配置請參閱 [config-examples.json](09-advanced-features/config-examples.json)。

</details>

<details>
<summary>10. CLI 參考</summary>

**位置**：[10-cli/](10-cli/)

**說明**：Claude Code 的完整命令列介面參考

**快速範例**：
```bash
# 互動模式
claude "explain this project"

# Print 模式（非互動式）
claude -p "review this code"

# 處理檔案內容
cat error.log | claude -p "explain this error"

# 腳本用 JSON 輸出
claude -p --output-format json "list functions"

# 繼續會話
claude -r "feature-auth" "continue implementation"
```

**使用場景**：CI/CD 流水線整合、腳本自動化、批次處理、多會話工作流程、自訂 agent 配置

</details>

<details>
<summary>工作流程範例</summary>

### 完整程式碼審查工作流程

```markdown
# 使用：Slash Commands + Subagents + Memory + MCP

User: /review-pr

Claude:
1. 載入專案 memory（程式碼標準）
2. 透過 GitHub MCP 取得 PR
3. 委派給 code-reviewer subagent
4. 委派給 test-engineer subagent
5. 整合發現
6. 提供全面審查
```

### 自動化文件生成

```markdown
# 使用：Skills + Subagents + Memory

User: "Generate API documentation for the auth module"

Claude:
1. 載入專案 memory（文件標準）
2. 偵測文件生成請求
3. 自動觸發 doc-generator skill
4. 委派給 api-documenter subagent
5. 建立附範例的全面文件
```

### DevOps 部署

```markdown
# 使用：Plugins + MCP + Hooks

User: /deploy production

Claude:
1. 執行 pre-deploy hook（驗證環境）
2. 委派給 deployment-specialist subagent
3. 透過 Kubernetes MCP 執行部署
4. 監控進度
5. 執行 post-deploy hook（健康檢查）
6. 回報狀態
```

</details>

<details>
<summary>目錄結構</summary>

```
├── 01-slash-commands/
│   ├── optimize.md
│   ├── pr.md
│   ├── generate-api-docs.md
│   └── README.md
├── 02-memory/
│   ├── project-CLAUDE.md
│   ├── directory-api-CLAUDE.md
│   ├── personal-CLAUDE.md
│   └── README.md
├── 03-skills/
│   ├── code-review/
│   │   ├── SKILL.md
│   │   ├── scripts/
│   │   └── templates/
│   ├── brand-voice/
│   │   ├── SKILL.md
│   │   └── templates/
│   ├── doc-generator/
│   │   ├── SKILL.md
│   │   └── generate-docs.py
│   └── README.md
├── 04-subagents/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   ├── documentation-writer.md
│   ├── secure-reviewer.md
│   ├── implementation-agent.md
│   └── README.md
├── 05-mcp/
│   ├── github-mcp.json
│   ├── database-mcp.json
│   ├── filesystem-mcp.json
│   ├── multi-mcp.json
│   └── README.md
├── 06-hooks/
│   ├── format-code.sh
│   ├── pre-commit.sh
│   ├── security-scan.sh
│   ├── log-bash.sh
│   ├── validate-prompt.sh
│   ├── notify-team.sh
│   └── README.md
├── 07-plugins/
│   ├── pr-review/
│   ├── devops-automation/
│   ├── documentation/
│   └── README.md
├── 08-checkpoints/
│   ├── checkpoint-examples.md
│   └── README.md
├── 09-advanced-features/
│   ├── config-examples.json
│   ├── planning-mode-examples.md
│   └── README.md
├── 10-cli/
│   └── README.md
└── README.md（本檔案）
```

</details>

<details>
<summary>最佳實踐</summary>

### 應該做的
- 從 slash commands 開始，保持簡單
- 漸進式新增功能
- 使用 memory 存放團隊標準
- 先在本地測試配置
- 記錄自訂實作
- 對專案配置進行版本控制
- 與團隊分享 plugins

### 不應該做的
- 不要建立重複的功能
- 不要硬編碼憑證
- 不要跳過文件
- 不要把簡單任務複雜化
- 不要忽視安全最佳實踐
- 不要提交敏感資料

</details>

<details>
<summary>疑難排解</summary>

### 功能未載入
1. 檢查檔案位置和命名
2. 驗證 YAML frontmatter 語法
3. 檢查檔案權限
4. 確認 Claude Code 版本相容性

### MCP 連線失敗
1. 驗證環境變數
2. 檢查 MCP server 安裝
3. 測試憑證
4. 確認網路連線

### Subagent 未委派
1. 檢查工具權限
2. 驗證 agent 描述的清晰度
3. 確認任務複雜度
4. 獨立測試 agent

</details>

<details>
<summary>測試</summary>

本專案包含全面的自動化測試：

- **單元測試**：使用 pytest 的 Python 測試（Python 3.10、3.11、3.12）
- **程式碼品質**：使用 Ruff 進行 linting 和格式化
- **安全性**：使用 Bandit 進行漏洞掃描
- **型別檢查**：使用 mypy 進行靜態型別分析
- **建置驗證**：EPUB 生成測試
- **覆蓋率追蹤**：Codecov 整合

```bash
# 安裝開發依賴
uv pip install -r requirements-dev.txt

# 執行所有單元測試
pytest scripts/tests/ -v

# 執行附覆蓋率報告的測試
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行程式碼品質檢查
ruff check scripts/
ruff format --check scripts/

# 執行安全掃描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 執行型別檢查
mypy scripts/ --ignore-missing-imports
```

測試會在每次推送至 `main`/`develop` 以及每次對 `main` 的 PR 時自動執行。詳細資訊請參閱 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 生成</summary>

想要離線閱讀本指南？生成 EPUB 電子書：

```bash
uv run scripts/build_epub.py
```

這會建立 `claude-howto-guide.epub`，包含所有內容和渲染後的 Mermaid 圖表。

更多選項請參閱 [scripts/README.md](scripts/README.md)。

</details>

<details>
<summary>貢獻方式</summary>

發現問題或想要貢獻範例？我們非常歡迎你的幫助！

**請閱讀 [CONTRIBUTING.md](CONTRIBUTING.md) 了解詳細指南，包含：**
- 貢獻類型（範例、文件、功能、錯誤修正、回饋）
- 如何設定開發環境
- 目錄結構和如何新增內容
- 寫作指南和最佳實踐
- 提交和 PR 流程

**我們的社群標準：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我們如何相互對待
- [SECURITY.md](SECURITY.md) - 安全政策和漏洞回報

### 回報安全問題

如果你發現安全漏洞，請負責任地回報：

1. **使用 GitHub 私人漏洞回報**：https://github.com/luongnv89/claude-howto/security/advisories
2. **或閱讀** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 了解詳細說明
3. **請勿**為安全漏洞開設公開 issue

快速開始：
1. Fork 並複製儲存庫
2. 建立描述性分支（`add/feature-name`、`fix/bug`、`docs/improvement`）
3. 按照指南進行更改
4. 提交附清晰描述的 pull request

**需要幫助？** 開設 issue 或討論，我們會引導你完成流程。

</details>

<details>
<summary>額外資源</summary>

- [Claude Code 文件](https://code.claude.com/docs/en/overview)
- [MCP Protocol 規範](https://modelcontextprotocol.io)
- [Skills 儲存庫](https://github.com/luongnv89/skills) - 現成可用的 skills 集合
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny 的 Claude Code 工作流程](https://x.com/bcherny/status/2007179832300581177) - Claude Code 的創造者分享他的系統化工作流程：平行 agents、共享 CLAUDE.md、Plan 模式、slash commands、subagents，以及用於自主長時間執行會話的驗證 hooks。

</details>

---

## 貢獻指南

我們歡迎貢獻！請參閱我們的[貢獻指南](CONTRIBUTING.md)了解如何開始。

## 貢獻者

感謝所有為本專案做出貢獻的人！

| 貢獻者 | PRs |
|-------------|-----|
| [wjhrdy](https://github.com/wjhrdy) | [#1 - add a tool to create an epub](https://github.com/luongnv89/claude-howto/pull/1) |
| [VikalpP](https://github.com/VikalpP) | [#7 - fix(docs): Use tilde fences for nested code blocks in concepts guide](https://github.com/luongnv89/claude-howto/pull/7) |

---

## 授權條款

MIT 授權——請參閱 [LICENSE](LICENSE)。可自由使用、修改和分發。唯一要求是保留授權聲明。

---

**最後更新**：2026 年 3 月
**Claude Code 版本**：2.1+
