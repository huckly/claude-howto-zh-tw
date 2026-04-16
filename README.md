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
[![Version](https://img.shields.io/badge/version-2.3.0-brightgreen)](CHANGELOG.md)
[![Claude Code](https://img.shields.io/badge/Claude_Code-2.1+-purple)](https://code.claude.com)

🌐 **Language / Ngôn ngữ / 语言 / Мова:** [English](README.md) | [Tiếng Việt](vi/README.md) | [中文](zh/README.md) | [Українська](uk/README.md)

# 在週末內掌握 Claude Code

從輸入 `claude` 到協調代理、鉤子、技能和 MCP 伺服器——透過視覺化教學、複製貼上範本和引導式學習路徑。

**[15 分鐘上手](#get-started-in-15-minutes)** | **[不確定從哪裡開始？](#not-sure-where-to-start)** | **[瀏覽功能目錄](CATALOG.md)**

---

## 目錄

- [問題](#the-problem)
- [Claude How To 如何解決這個問題](#how-claude-how-to-fixes-this)
- [運作方式](#how-it-works)
- [不確定從哪裡開始？](#not-sure-where-to-start)
- [15 分鐘上手](#get-started-in-15-minutes)
- [你可以用這個做什麼？](#what-can-you-build-with-this)
- [常見問題](#faq)
- [貢獻](#contributing)
- [授權條款](#license)

---

## 問題

你安裝了 Claude Code。你執行了一些提示詞。接下來該怎麼辦？

- **官方文件描述了功能 — 但沒有展示如何將它們組合起來。** 你知道斜線命令存在，但不知道如何將它們與鉤子、記憶體和子代理串聯成可以節省數小時的工作流程。
- **沒有明確的學習路徑。** 應該先學習 MCP 還是鉤子？先學習技能還是子代理？你最終只會草草瀏覽所有內容，卻沒有真正掌握任何東西。
- **範例太過基礎。** 一個「Hello World」斜線命令無法幫助你構建一個使用記憶體、委派給專門代理並自動執行安全性掃描的生產級程式碼審查流水線。

你正在浪費 Claude Code 90% 的潛力 — 而且你不知道自己不知道什麼。

---

## Claude How To 如何解決這個問題

這不是另一份功能參考文件。這是一個 **結構化、視覺化、以範例為導向的指南**，教你如何使用 Claude Code 的每個功能，並提供可以直接複製到你專案中的真實世界範本。

| | 官方文件 | 本指南 |
|--|---------------|------------|
| **格式** | 參考文件 | 帶有 Mermaid 圖表的視覺化教學 |
| **深度** | 功能描述 | 如何在後台運作 |
| **範例** | 基礎片段 | 立即使用的生產級範本 |
| **結構** | 功能組織 | 循序漸進的學習路徑（初學者到進階） |
| **導入** | 自行導向 | 帶有時間估算的指導路線圖 |
| **自我評估** | 無 | 互動式小測驗，找出你的不足並建立個人化路徑 |

### 你將獲得：

- **10 個教學模組**，涵蓋 Claude Code 的每個功能 — 從斜線命令到自訂代理團隊
- **複製-貼上設定** — 斜線命令、CLAUDE.md 範本、鉤子腳本、MCP 設定、子代理定義和完整的外掛程式包
- **Mermaid 圖表**，展示每個功能如何在內部運作，讓你了解 *為什麼*，而不仅仅是 *如何*
- **循序漸進的學習路徑**，讓你從初學者到高級使用者只需 11-13 小時
- **內建自我評估** — 在 Claude Code 中直接執行 `/self-assessment` 或 `/lesson-quiz hooks` 以找出你的不足

**[開始學習路徑 ->](LEARNING-ROADMAP.md)**

## 如何運作

### 1. 評估你的程度

使用 [自我評估測驗](LEARNING-ROADMAP.md#-find-your-level) 或在 Claude Code 中執行 `/self-assessment`。 根據你已知的內容獲得個人化的學習路徑。

### 2. 遵循引導路徑

順序完成 10 個模組 — 每個模組都建立在前一個基礎上。 在學習的同時，將範本直接複製到你的專案中。

### 3. 將功能組合到工作流程中

真正的力量在於組合功能。 學習如何將斜線命令 + 記憶 + 子代理 + 鉤子 整合到自動化的流水線中，以處理程式碼審查、部署和文件生成。

### 4. 測試你的理解

在每個模組後執行 `/lesson-quiz [topic]`。 測驗會指出你錯過的地方，以便快速填補知識缺口。

**[15 分鐘快速入門](#get-started-in-15-minutes)**

---

## 受到 21,800+ 位開發者信賴

- **21,800+ GitHub 星星**來自每天使用 Claude Code 的開發者
- **2,585+ 次 Fork** — 團隊為自己的工作流程調整此指南
- **持續維護** — 與每個 Claude Code 版本同步（最新版本：v2.3.0，2026 年 4 月）
- **社群驅動** — 來自分享他們真實世界配置的開發者的貢獻

[![Star History Chart](https://api.star-history.com/svg?repos=luongnv89/claude-howto&type=Date)](https://star-history.com/#luongnv89/claude-howto&Date)

---

## 不確定從哪裡開始？

進行自我評估或選擇你的程度：

| 程度 | 你可以... | 開始這裡 | 時間 |
|-------|-----------|------------|------|
| **入門** | 開始使用 Claude Code 並進行聊天 | [斜線命令](01-slash-commands/) | ~2.5 小時 |
| **中級** | 使用 CLAUDE.md 和自訂命令 | [技能](03-skills/) | ~3.5 小時 |
| **進階** | 配置 MCP 伺服器和鉤子 | [進階功能](09-advanced-features/) | ~5 小時 |

**完整的 10 個模組學習路徑：**

| 順序 | 模組 | 程度 | 時間 |
|-------|--------|-------|------|
| 1 | [斜線命令](01-slash-commands/) | 入門 | 30 分鐘 |
| 2 | [記憶](02-memory/) | 入門+ | 45 分鐘 |
| 3 | [檢查點](08-checkpoints/) | 中級 | 45 分鐘 |
| 4 | [CLI 基礎](10-cli/) | 入門+ | 30 分鐘 |
| 5 | [技能](03-skills/) | 中級 | 1 小時 |
| 6 | [鉤子](06-hooks/) | 中級 | 1 小時 |
| 7 | [MCP](05-mcp/) | 中級+ | 1 小時 |
| 8 | [子代理](04-subagents/) | 中級+ | 1.5 小時 |
| 9 | [進階功能](09-advanced-features/) | 進階 | 2-3 小時 |
| 10 | [外掛](07-plugins/) | 進階 | 2 小時 |

**[完整學習路徑 ->](LEARNING-ROADMAP.md)**

## 快速上手 (15 分鐘)

```bash
# 1. 複製程式碼庫
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto

# 2. 複製你的第一個斜線命令
mkdir -p /path/to/your-project/.claude/commands
cp 01-slash-commands/optimize.md /path/to/your-project/.claude/commands/

# 3. 試試看 — 在 Claude Code 中，輸入：
# /optimize

# 4. 準備好更多了嗎？設定專案記憶體：
cp 02-memory/project-CLAUDE.md /path/to/your-project/CLAUDE.md

# 5. 安裝一個技能：
cp -r 03-skills/code-review ~/.claude/skills/
```

想要完整的設定嗎？這裡提供**基礎設定 (1 小時)**：

```bash
# 斜線命令 (15 分鐘)
cp 01-slash-commands/*.md .claude/commands/

# 專案記憶體 (15 分鐘)
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 安裝一個技能 (15 分鐘)
cp -r 03-skills/code-review ~/.claude/skills/

# 週末目標：新增鉤子、子代理、MCP 和外掛
# 遵循學習路徑以獲得引導式設定
```

**[查看完整的安裝參考資料](#get-started-in-15-minutes)**

---

## 你可以使用這個工具建立什麼？

| 使用案例 | 結合的特性 |
|----------|------------------------|
| **自動化程式碼審查** | 斜線命令 + 子代理 + 記憶體 + MCP |
| **團隊導入** | 記憶體 + 斜線命令 + 外掛 |
| **CI/CD 自動化** | CLI 參考資料 + 鉤子 + 背景任務 |
| **文件產生** | 技能 + 子代理 + 外掛 |
| **安全性審核** | 子代理 + 技能 + 鉤子 (唯讀模式) |
| **DevOps 流水線** | 外掛 + MCP + 鉤子 + 背景任務 |
| **複雜的重構** | 檢查點 + 規劃模式 + 鉤子 |

---

## 常見問題

**這免費嗎？**
是的。採用 MIT 授權，永久免費。可以在個人專案、工作場合、團隊中使用，除了包含授權聲明之外，沒有其他限制。

**這是否有人維護？**
積極維護。本指南會與 Claude Code 的每個版本同步。目前版本：v2.3.0 (2026 年 4 月)，與 Claude Code 2.1+ 相容。

**這與官方文件有何不同？**
官方文件是功能參考。本指南是具有圖表、生產力範本和循序漸進學習路徑的教學指南。它們相互補充——從這裡開始學習，在需要具體資訊時參考官方文件。

**完成所有內容需要多長時間？**
完整的學習路徑需要 11-13 小時。但您只需 15 分鐘就能獲得即時價值——只需複製斜線命令範本並試用它。

**我可以將其與 Claude Sonnet / Haiku / Opus 一起使用嗎？**
可以。所有範本都與 Claude Sonnet 4.6、Claude Opus 4.6 和 Claude Haiku 4.5 相容。

**我可以貢獻嗎？**
絕對可以。請參閱 [CONTRIBUTING.md](CONTRIBUTING.md) 以取得指南。我們歡迎新的範例、錯誤修正、文件改進和社群範本。

**我可以離線閱讀嗎？**
可以。執行 `uv run scripts/build_epub.py` 以產生包含所有內容和渲染圖表的 EPUB 電子書。

---

## 今天開始掌握 Claude Code

您已經安裝了 Claude Code。您與 10 倍生產力之間唯一的障礙是知道如何使用它。本指南為您提供了結構化的學習路徑、視覺化的說明和可複製貼上的範本，讓您快速上手。

採用 MIT 授權。永久免費。複製它、分叉它、讓它成為您的。

**[開始學習路徑 ->](LEARNING-ROADMAP.md)** | **[瀏覽功能目錄](CATALOG.md)** | **[15 分鐘快速入門](#get-started-in-15-minutes)**

---

<details>
<summary>快速導航 — 所有功能</summary>

| 功能 | 描述 | 資料夾 |
|---------|-------------|--------|
| **功能目錄** | 完整的安裝命令參考 | [CATALOG.md](CATALOG.md) |
| **斜線命令** | 用戶觸發的捷徑 | [01-slash-commands/](01-slash-commands/) |
| **記憶** | 持續的上下文 | [02-memory/](02-memory/) |
| **技能** | 可重複使用的功能 | [03-skills/](03-skills/) |
| **子代理** | 專業的 AI 助理 | [04-subagents/](04-subagents/) |
| **MCP 協議** | 外部工具存取 | [05-mcp/](05-mcp/) |
| **鉤子** | 觸發事件驅動的自動化 | [06-hooks/](06-hooks/) |
| **外掛** | 封裝的功能 | [07-plugins/](07-plugins/) |
| **檢查點** | 工作階段快照和回溯 | [08-checkpoints/](08-checkpoints/) |
| **進階功能** | 規劃、思考、背景任務 | [09-advanced-features/](09-advanced-features/) |
| **CLI 參考** | 命令、旗標和選項 | [10-cli/](10-cli/) |
| **部落格文章** | 實際使用案例範例 | [Blog Posts](https://medium.com/@luongnv89) |

</details>

<details>
<summary>功能比較</summary>

| 功能 | 觸發方式 | 持續性 | 適用於 |
|---------|-----------|------------|----------|
| **斜線命令** | 手動 (`/cmd`) | 工作階段僅限 | 快速捷徑 |

| **記憶** | 自動載入 | 跨會話 | 長期學習 |
| **技能** | 自動觸發 | 檔案系統 | 自動化工作流程 |
| **子代理** | 自動委派 | 隔離上下文 | 任務分配 |
| **MCP 協定** | 自動查詢 | 即時 | 存取即時資料 |
| **鉤子** | 事件觸發 | 配置 | 自動化與驗證 |
| **外掛** | 一個命令 | 所有功能 | 完整的解決方案 |
| **檢查點** | 手動/自動 | 基於會話 | 安全實驗 |
| **規劃模式** | 手動/自動 | 規劃階段 | 複雜的實作 |
| **背景任務** | 手動 | 任務時長 | 長時間運作 |
| **CLI 參考** | 終端命令 | 會話/腳本 | 自動化與腳本編寫 |

</details>

<details>
<summary>安裝快速參考</summary>

```bash
# 斜線命令
cp 01-slash-commands/*.md .claude/commands/

# 記憶
cp 02-memory/project-CLAUDE.md ./CLAUDE.md

# 技能
cp -r 03-skills/code-review ~/.claude/skills/

# 子代理
cp 04-subagents/*.md .claude/agents/

# MCP
export GITHUB_TOKEN="token"
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 鉤子
mkdir -p ~/.claude/hooks
cp 06-hooks/*.sh ~/.claude/hooks/
chmod +x ~/.claude/hooks/*.sh

# 外掛
/plugin install pr-review

# 檢查點 (預設啟用，在設定中配置)
# 參閱 08-checkpoints/README.md

# 進階功能 (在設定中配置)
# 參閱 09-advanced-features/config-examples.json

# CLI 參考 (無需安裝)
# 參閱 10-cli/README.md 以取得使用範例
```

</details>

<details>
<summary>01. 斜線命令</summary>

**位置**: [01-slash-commands/](01-slash-commands/)

**說明**: 用戶觸發的快捷方式，儲存在 Markdown 檔案中

**範例**:
- `optimize.md` - 程式碼優化分析
- `pr.md` - 提取請求準備
- `generate-api-docs.md` - API 文件產生器

**安裝**:
```bash
cp 01-slash-commands/*.md /path/to/project/.claude/commands/
```

**使用**:
```
/optimize
/pr
/generate-api-docs
```

**進一步了解**: [Discovering Claude Code Slash Commands](https://medium.com/@luongnv89/discovering-claude-code-slash-commands-cdc17f0dfb29)

</details>

<details>
<summary>02. 記憶</summary>

**位置**: [02-memory/](02-memory/)

**說明**: 跨會話的持續上下文

**範例**:
- `project-CLAUDE.md` - 團隊範圍的專案標準
- `directory-api-CLAUDE.md` - 目錄特定的規則
- `personal-CLAUDE.md` - 個人偏好

**安裝**:
```bash
# 專案記憶
cp 02-memory/project-CLAUDE.md /path/to/project/CLAUDE.md

# 目錄記憶
cp 02-memory/directory-api-CLAUDE.md /path/to/project/src/api/CLAUDE.md

# 個人記憶
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

**使用**: 由 Claude 自動載入

</details>

<details>
<summary>03. 技能</summary>

**位置**: [03-skills/](03-skills/)

**說明**: 具有指令和腳本的可重複使用的、自動觸發的機能

**範例**:
- `code-review/` - 具有腳本的全面程式碼審查
- `brand-voice/` - 品牌聲音一致性檢查器

- `doc-generator/` - API 文件產生器

**安裝**:
```bash
# 個人技能
cp -r 03-skills/code-review ~/.claude/skills/

# 專案技能
cp -r 03-skills/code-review /path/to/project/.claude/skills/
```

**使用**: 自動觸發

</details>

<details>
<summary>04. 子代理</summary>

**位置**: [04-subagents/](04-subagents/)

**說明**: 具有獨立上下文和自訂提示詞的專業 AI 助理

**範例**:
- `code-reviewer.md` - 全面的程式碼品質分析
- `test-engineer.md` - 測試策略和覆蓋率
- `documentation-writer.md` - 技術文件撰寫
- `secure-reviewer.md` - 專注於安全性的審查 (唯讀)
- `implementation-agent.md` - 完整功能實作

**安裝**:
```bash
cp 04-subagents/*.md /path/to/project/.claude/agents/
```

**使用**: 由主要代理自動委派

</details>

<details>
<summary>05. MCP 協定</summary>

**位置**: [05-mcp/](05-mcp/)

**說明**: 模型上下文協定，用於存取外部工具和 API

**範例**:
- `github-mcp.json` - GitHub 整合
- `database-mcp.json` - 資料庫查詢
- `filesystem-mcp.json` - 檔案操作
- `multi-mcp.json` - 複数の MCP 伺服器

**安裝**:
```bash
# 設定環境變數
export GITHUB_TOKEN="your_token"
export DATABASE_URL="postgresql://..."

# 使用 CLI 增加 MCP 伺服器
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 或手動將其新增到專案 .mcp.json (請參閱 05-mcp/ 範例)
```

**使用**: 一旦設定完成，Claude 就能自動使用 MCP 工具

</details>

<details>
<summary>06. 鉤子</summary>

**位置**: [06-hooks/](06-hooks/)

**說明**: 事件驅動的 shell 命令，會在 Claude Code 事件發生時自動執行

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

在 `~/.claude/settings.json` 中設定鉤子:
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

**使用**: 鉤子會在事件發生時自動執行

**鉤子類型** (4 種類型，25 種事件):
- **工具鉤子**: `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`
- **會話鉤子**: `SessionStart`, `SessionEnd`, `Stop`, `StopFailure`, `SubagentStart`, `SubagentStop`
- **任務鉤子**: `UserPromptSubmit`, `TaskCompleted`, `TaskCreated`, `TeammateIdle`
- **生命週期鉤子**: `ConfigChange`, `CwdChanged`, `FileChanged`, `PreCompact`, `PostCompact`, `WorktreeCreate`, `WorktreeRemove`, `Notification`, `InstructionsLoaded`, `Elicitation`, `ElicitationResult`

<details>
<summary>07. 外掛</summary>

**位置**: [07-plugins/](07-plugins/)

**說明**: 封裝的命令、代理、MCP 和鉤子的集合

**範例**:
- `pr-review/` - 完整的 PR 審查工作流程
- `devops-automation/` - 部署和監控
- `documentation/` - 文件產生

**安裝**:
```bash
/plugin install pr-review
/plugin install devops-automation
/plugin install documentation
```

**使用**: 使用封裝的斜線命令和功能

</details>

<details>
<summary>檢查點與回溯</summary>

**位置**: [08-checkpoints/](08-checkpoints/)

**說明**: 儲存會話狀態，並回溯到先前點，以探索不同的方法

**關鍵概念**:
- **檢查點**: 會話狀態的快照
- **回溯**: 回到先前的檢查點
- **分支點**: 從相同的檢查點探索多種方法

**使用**:
```
# 檢查點會隨著每個使用者提示自動建立
# 要回溯，請按 Esc 兩次或使用：
/rewind

# 然後從五個選項中選擇：
# 1. 恢復程式碼和會話
# 2. 恢復會話
# 3. 恢復程式碼
# 4. 從此摘要
# 5. 算了
```

**使用案例**:
- 嘗試不同的實作方法
- 從錯誤中恢復
- 安全的實驗
- 比較替代方案
- A/B 測試不同的設計

</details>

<details>
<summary>進階功能</summary>

**位置**: [09-advanced-features/](09-advanced-features/)

**說明**: 針對複雜工作流程和自動化的高階功能

**包含**:
- **規劃模式** — 在編碼之前建立詳細的實作計畫
- **擴展思考** — 針對複雜問題進行深入推理（使用 `Alt+T` / `Option+T` 啟用）
- **背景任務** — 在不封鎖的情況下執行長時間操作
- **權限模式** — `default`、`acceptEdits`、`plan`、`dontAsk`、`bypassPermissions`
- **無頭模式** — 在 CI/CD 中執行 Claude Code: `claude -p "執行測試並產生報告"`
- **會話管理** — `/resume`, `/rename`, `/fork`, `claude -c`, `claude -r`
- **設定** — 在 `~/.claude/settings.json` 中自訂行為

請參閱 [config-examples.json](09-advanced-features/config-examples.json) 以取得完整的設定。

</details>

<details>
<summary>CLI 參考</summary>

**位置**: [10-cli/](10-cli/)

**說明**: Claude Code 的完整命令列介面參考

**快速範例**:
```bash
# 互動模式
claude "explain this project"

# 輸出模式 (非互動)
claude -p "review this code"

# 處理檔案內容
cat error.log | claude -p "explain this error"

# JSON 輸出，用於腳本
claude -p --output-format json "list functions"

# 繼續會話
claude -r "feature-auth" "continue implementation"
```

**使用案例**: CI/CD 流程整合、腳本自動化、批次處理、多會話工作流程、自訂代理設定

</details>

<details>
<summary>範例工作流程</summary>

### 完整的程式碼審查工作流程

```markdown
# Uses: Slash Commands + Subagents + Memory + MCP

## 審查 PR

Claude:
1. 載入專案記憶體 (程式碼標準)
2. 透過 GitHub MCP 抓取 PR
3. 委派給 code-reviewer 子代理
4. 委派給 test-engineer 子代理
5. 綜合分析結果
6. 提供全面的審查

```markdown
# 使用：技能 + 子代理 + 記憶

User: "為 auth 模組產生 API 文件"

Claude:
1. 載入專案記憶體 (文件標準)
2. 偵測文件產生請求
3. 自動呼叫 doc-generator 技能
4. 委派給 api-documenter 子代理
5. 產生包含範例的全面文件
```

## DevOps 部署

```markdown
# 使用：外掛 + MCP + 鉤子

User: /deploy production

Claude:
1. 執行預部署鉤子 (驗證環境)
2. 委派給 deployment-specialist 子代理
3. 透過 Kubernetes MCP 執行部署
4. 監控進度
5. 執行後部署鉤子 (健康檢查)
6. 報告狀態
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
└── README.md (此檔案)
```

</details>

<details>
<summary>最佳實踐</summary>

### 應該做的事情
- 從斜線命令開始
- 逐步新增功能
- 使用記憶體來儲存團隊標準
- 首次在本地測試配置
- 記錄自訂實作
- 版本控制專案配置
- 與團隊分享外掛

### 不應該做的事情
- 不要建立重複的功能
- 不要硬編碼憑證
- 不要跳過文件
- 不要過度複雜簡單的任務
- 不要忽略安全性最佳實踐
- 不要提交敏感資料

</details>

<details>
<summary>疑難排解</summary>

### 功能未載入
1. 檢查檔案位置和命名
2. 驗證 YAML 前置詞語法
3. 檢查檔案權限
4. 檢查 Claude Code 版本相容性

### MCP 連線失敗

1. 驗證環境變數
2. 檢查 MCP 伺服器安裝
3. 測試憑證
4. 審查網路連線

### 子代理未委派
1. 檢查工具權限
2. 驗證代理描述的清晰度
3. 審查任務複雜度
4. 獨立測試代理

</details>

<details>
<summary>測試</summary>

這個專案包含全面的自動化測試：

- **單元測試**: 使用 pytest 的 Python 測試 (Python 3.10, 3.11, 3.12)
- **程式碼品質**: 使用 Ruff 進行 Linting 和格式化
- **安全性**: 使用 Bandit 進行漏洞掃描
- **類型檢查**: 使用 mypy 進行靜態類型分析
- **建置驗證**: EPUB 產生測試
- **覆蓋率追蹤**: Codecov 整合

```bash
# 安裝開發相依性
uv pip install -r requirements-dev.txt

# 執行所有單元測試
pytest scripts/tests/ -v

# 執行包含覆蓋率報告的測試
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行程式碼品質檢查
ruff check scripts/
ruff format --check scripts/

# 執行安全性掃描
bandit -c pyproject.toml -r scripts/ --exclude scripts/tests/

# 執行類型檢查
mypy scripts/ --ignore-missing-imports
```

測試會在每次推送到 `main`/`develop` 以及每次 PR 到 `main` 時自動執行。 詳情請參閱 [TESTING.md](.github/TESTING.md)。

</details>

<details>
<summary>EPUB 產生</summary>

想離線閱讀這份指南嗎？ 產生一個 EPUB 電子書：

```bash
uv run scripts/build_epub.py
```

這會建立 `claude-howto-guide.epub`，其中包含所有內容，包括渲染的 Mermaid 圖表。

更多選項請參閱 [scripts/README.md](scripts/README.md)。

</details>

<details>
<summary>貢獻</summary>

發現了問題或想貢獻範例嗎？ 我們很樂意接受您的幫助！

**請先閱讀 [CONTRIBUTING.md](CONTRIBUTING.md) 以取得詳細的指南：**
- 貢獻類型 (範例、文件、功能、錯誤、回饋)
- 如何設定您的開發環境
- 目錄結構以及如何新增內容
- 撰寫指南和最佳實務
- 提交和 PR 流程

**我們的社群標準：**
- [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) - 我們如何彼此對待
- [SECURITY.md](SECURITY.md) - 安全政策和漏洞報告

### 報告安全問題

如果您發現安全漏洞，請負責地報告：

1. **使用 GitHub 私有漏洞報告**: https://github.com/luongnv89/claude-howto/security/advisories
2. **或閱讀** [.github/SECURITY_REPORTING.md](.github/SECURITY_REPORTING.md) 以取得詳細的指示
3. **不要** 為安全漏洞開啟公開問題

快速入門：
1. 複製並下載這個存放庫
2. 建立一個描述性分支 (`add/feature-name`, `fix/bug`, `docs/improvement`)
3. 進行您的變更，遵循指南
4. 提交一個具有清晰描述的拉取請求

**需要幫助嗎？** 開啟一個問題或討論，我們會引導您完成流程。

</details>

<details>
<summary>其他資源</summary>

- [Claude Code 文件](https://code.claude.com/docs/en/overview)

- [MCP Protocol Specification](https://modelcontextprotocol.io)
- [Skills Repository](https://github.com/luongnv89/skills) - Collection of ready-to-use skills
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) - The creator of Claude Code shares his systematized workflow: parallel agents, shared CLAUDE.md, Plan mode, 斜線命令, subagents, and verification hooks for autonomous long-running sessions.

</details>

---

## 貢獻

我們歡迎貢獻！請參閱我們的 [貢獻指南](CONTRIBUTING.md) 以了解如何開始。

---

## 授權

MIT 授權 - 參閱 [LICENSE](LICENSE)。 自由使用、修改和發布。唯一的條件是包含授權聲明。

---

**上次更新**: 2026 年 4 月 11 日
**Claude Code 版本**: 2.1.101
**來源**:
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
**相容模型**: Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
