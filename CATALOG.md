<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude Code 功能目錄

> 所有 Claude Code 功能的快速參考指南：命令、代理人、技能、外掛與鉤子。

**導覽**：[命令](#斜線命令) | [權限模式](#權限模式) | [子代理人](#子代理人) | [技能](#技能) | [外掛](#外掛) | [MCP 伺服器](#mcp-伺服器) | [鉤子](#鉤子) | [記憶體](#記憶體檔案) | [新功能](#新功能)

---

## 摘要

| 功能 | 內建 | 範例 | 總計 | 參考 |
|---------|----------|----------|-------|-----------|
| **斜線命令** | 60+ | 8 | 68+ | [01-slash-commands/](01-slash-commands/) |
| **子代理人** | 6 | 9 | 15 | [04-subagents/](04-subagents/) |
| **技能** | 10 個內建 | 6 | 16 | [03-skills/](03-skills/) |
| **外掛** | - | 3 | 3 | [07-plugins/](07-plugins/) |
| **MCP 伺服器** | 1 | 4 | 5 | [05-mcp/](05-mcp/) |
| **鉤子** | 33 個事件 | 11 | 44 | [06-hooks/](06-hooks/) |
| **記憶體** | 7 種類型 | 3 | 10 | [02-memory/](02-memory/) |
| **總計** | **117** | **44** | **161** | |

---

## 斜線命令

命令是使用者手動呼叫的捷徑，用於執行特定動作。

### 內建命令

| 命令 | 說明 | 使用時機 |
|---------|-------------|-------------|
| `/help` | 顯示說明資訊 | 入門、學習命令 |
| `/btw` | 暫時性的旁問——不會污染主要脈絡 | 快速提問岔題 |
| `/chrome` | 設定 Chrome 整合 | 瀏覽器自動化 |
| `/clear` | 清除對話紀錄 | 重新開始、減少脈絡 |
| `/diff` | 互動式差異檢視器。在全螢幕 TUI（v2.1.260+）中，會在對話旁開啟一個保持開啟的差異面板，每當 Claude 編輯檔案或執行命令時都會重新整理；傳統算繪器則會以檢視器取代提示輸入區 | 審查變更 |
| `/config` | 檢視／編輯設定 | 自訂行為 |
| `/status` | 顯示工作階段狀態 | 確認目前狀態 |
| `/agents` | 列出可用的代理人 | 查看委派選項 |
| `/skills` | 列出可用的技能 | 查看自動呼叫能力 |
| `/hooks` | 列出已設定的鉤子 | 除錯自動化流程 |
| `/insights` | 分析工作階段模式 | 工作階段最佳化 |
| `/install-slack-app` | 安裝 Claude Slack 應用程式 | Slack 整合 |
| `/keybindings` | 自訂鍵盤快捷鍵 | 按鍵自訂 |
| `/mcp` | 列出 MCP 伺服器 | 確認外部整合 |
| `/memory` | 檢視已載入的記憶體檔案 | 除錯脈絡載入 |
| `/mobile` | 產生行動裝置 QR 碼 | 行動裝置存取 |
| `/passes` | 檢視使用通行證 | 訂閱資訊 |
| `/plugin` | 管理外掛 | 安裝／移除擴充功能 |
| `/plan` | 進入規劃模式 | 複雜實作 |
| `/proactive` | `/loop` 的別名（v2.1.105） | 與 `/loop` 相同 |
| `/recap` | 返回工作階段時顯示摘要 | 離開後重新取得脈絡 |
| `/rewind` | 回溯至檢查點 | 還原變更、探索替代方案 |
| `/checkpoint` | 管理檢查點 | 儲存／還原狀態 |
| `/cost` | 開啟 `/usage` 費用分頁的捷徑別名（v2.1.118+） | 監控花費 |
| `/context` | 顯示脈絡視窗使用量 | 管理對話長度 |
| `/export` | 匯出對話 | 儲存以供參考 |
| `/usage-credits` | 設定額外使用量上限（在 v2.1.144 中從 `/extra-usage` 更名；舊名稱仍可作為別名使用） | 速率限制管理 |
| `/feedback` | 提交意見或錯誤回報 | 回報問題 |
| `/login` | 向 Anthropic 驗證身分 | 存取功能 |
| `/logout` | 登出 | 切換帳號 |
| `/sandbox` | 切換沙箱模式 | 安全命令執行 |
| `/doctor` | 執行診斷 | 排除故障 |
| `/reload-plugins` | 重新載入已安裝的外掛。自 v2.1.221 起，大多數安裝會立即啟用；只有在安裝摘要顯示 `Run /reload-plugins to activate.` 時才需要。自 v2.1.260+ 起可在無頭工作階段中使用，因此會出現在 Claude Code Desktop 與 SDK 的命令清單中 | 外掛管理 |
| `/reload-skills` | 不需重新啟動即可重新掃描技能目錄（v2.1.152） | 技能管理 |
| `/workflows` | 檢視執行中與已完成的動態工作流程執行紀錄（v2.1.154） | 多代理人協調 |
| `/release-notes` | 顯示版本說明 | 確認新功能 |
| `/remote-control` | 啟用遠端控制 | 遠端存取 |
| `/permissions` | 管理權限 | 控制存取 |
| `/session` | 管理工作階段 | 多工作階段流程 |
| `/rename` | 重新命名目前的工作階段 | 整理工作階段 |
| `/resume` | 恢復上一個工作階段 | 繼續工作 |
| `/todo` | 檢視／管理待辦清單 | 追蹤任務 |
| `/tui` | 切換全螢幕 TUI（文字使用者介面）模式 | 在全螢幕／tmux 中無閃爍渲染 |
| `/tasks` | 檢視背景任務 | 監控非同步操作 |
| `/copy` | 將最後一則回應複製到剪貼簿 | 快速分享輸出 |
| `/teleport`（別名 `/tp`） | 在此終端機中恢復 Claude Code on the web 的工作階段；會開啟選擇器。需要 claude.ai 訂閱 | 在遠端繼續工作 |
| `/desktop` | 開啟 Claude 桌面應用程式 | 切換至桌面介面 |
| `/theme` | 變更色彩主題；v2.1.118 新增透過 `~/.claude/themes/<name>.json` 自訂命名主題（外掛可附帶 `themes/` 目錄） | 自訂外觀 |
| `/usage` | 使用量／費用／統計的標準命令——將 `/cost` 與 `/stats` 合併為單一分頁檢視（v2.1.118）；自 v2.1.149 起費用檢視依類別（技能、子代理人、外掛、每個 MCP 伺服器）細分花費。在 **VSCode 擴充功能**（v2.1.174）中，`/usage`（Account & usage）對話框新增歸因明細——快取未命中、長脈絡費用、子代理人，以及 24 小時與 7 天區間內每個技能／代理人／外掛／MCP 的使用量 | 監控配額與費用 |
| `/focus` | 切換專注檢視（無干擾輸出顯示） | 在長任務期間減少視覺雜訊 |
| `/fork` | 將對話複製到一個新的獨立背景工作階段（v2.1.212+） | 平行探索替代方案 |
| `/subtask` | 產生一個繼承對話並回報結果的分叉子代理人（v2.1.212+） | 委派旁支任務而不失去目前進度 |
| `/stats` | 開啟 `/usage` 統計分頁的捷徑別名（v2.1.118+） | 查看工作階段指標 |
| `/statusline` | 設定狀態列 | 自訂狀態顯示 |
| `/stickers` | 檢視工作階段貼圖 | 趣味獎勵 |
| `/fast` | 切換快速輸出模式；適用於 **Opus 5 與 Opus 4.8**（v2.1.219） | 加速回應 |
| `/terminal-setup` | 設定終端機整合 | 設定終端機功能 |
| `/undo` | **已不再記載於文件**——於 v2.1.108 新增為 `/rewind` 的別名，但官方命令參考中已找不到 | 改用 `/rewind`（或 `Esc Esc`） |
| `/upgrade` | 檢查更新 | 版本管理 |
| `/team-onboarding` | 從本專案的 Claude Code 使用狀況產生隊友入門指南 | 新隊友入職（v2.1.101） |
| `/code-review ultra` | 對目前的變更執行雲端多代理人程式碼審查。`/ultrareview` 仍保留為別名；建議使用 `/code-review ultra`。Pro 與 Max 方案含 3 次免費執行，之後需要使用額度 | 合併前跨多個代理人的深度審查（v2.1.112） |
| `/fewer-permission-prompts` | 掃描逐字稿並為常見唯讀工具提出優先允許清單 | 減少專案中重複的權限提示（v2.1.112） |
| `/skill-doctor` | 顯示哪些已載入的技能未被使用，以及它們占用多少脈絡。在 `/plugin` 管理器的 **Stats** 分頁中開啟；在 `-p` 下以文字輸出 | 刪減不再需要的技能（v2.1.252+） |

### 自訂命令（範例）

| 命令 | 說明 | 使用時機 | 範疇 | 安裝 |
|---------|-------------|-------------|-------|--------------|
| `/optimize` | 分析程式碼以進行最佳化 | 效能改善 | 專案 | `cp 01-slash-commands/optimize.md .claude/commands/` |
| `/pr` | 準備拉取請求 | 提交 PR 前 | 專案 | `cp 01-slash-commands/pr.md .claude/commands/` |
| `/generate-api-docs` | 產生 API 文件 | 文件化 API | 專案 | `cp 01-slash-commands/generate-api-docs.md .claude/commands/` |
| `/commit` | 帶有脈絡的 git 提交 | 提交變更 | 使用者 | `cp 01-slash-commands/commit.md .claude/commands/` |
| `/push-all` | 暫存、提交並推送 | 快速部署 | 使用者 | `cp 01-slash-commands/push-all.md .claude/commands/` |
| `/doc-refactor` | 重新整理文件結構 | 改善文件 | 專案 | `cp 01-slash-commands/doc-refactor.md .claude/commands/` |
| `/setup-ci-cd` | 設定 CI/CD 流水線 | 新專案 | 專案 | `cp 01-slash-commands/setup-ci-cd.md .claude/commands/` |
| `/unit-test-expand` | 擴展測試覆蓋率 | 改善測試 | 專案 | `cp 01-slash-commands/unit-test-expand.md .claude/commands/` |

> **範疇**：`使用者` = 個人工作流程（`~/.claude/commands/`），`專案` = 團隊共用（`.claude/commands/`）

**參考**：[01-slash-commands/](01-slash-commands/) | [官方文件](https://code.claude.com/docs/en/interactive-mode)

**快速安裝（所有自訂命令）**：
```bash
cp 01-slash-commands/*.md .claude/commands/
```

---

## 權限模式

Claude Code 支援 6 種權限模式，用於控制工具使用的授權方式。

| 模式 | 說明 | 使用時機 |
|------|-------------|-------------|
| `manual` | 每次工具呼叫前提示 | 標準互動使用（於 v2.1.200 從 `default` 更名；仍接受 `default`） |
| `acceptEdits` | 自動接受檔案編輯，其他則提示 | 受信任的編輯工作流程 |
| `plan` | 僅限唯讀工具，不寫入 | 規劃與探索 |
| `auto` | 所有動作，並由背景安全分類器檢查 | 長時間任務、減少提示疲勞 |
| `bypassPermissions` | 跳過所有權限檢查 | CI/CD、無頭環境 |
| `dontAsk` | 跳過需要權限的工具 | 非互動式腳本 |

> **注意**：`auto` 模式需要符合資格的方案、模型與供應商——請參閱 [09-advanced-features/](09-advanced-features/#auto-mode)。`bypassPermissions` 僅在受信任的沙箱環境中使用。

**參考**：[官方文件](https://code.claude.com/docs/en/permissions)

---

## 子代理人

具有隔離脈絡的專業 AI 助理，用於處理特定任務。

> **巢狀生成預設開啟，深度為 3（v2.1.219）**：子代理人可以在主對話之下最多三層生成自己的子代理人。設定 `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH` 可變更上限，設為 `1` 則停用巢狀。（歷史：v2.1.172–v2.1.216 預設巢狀最多 5 層且無法變更；v2.1.217 改為需選擇啟用，深度為 1；v2.1.219 將預設值設為 3。）關於限制特定子代理人可生成哪些子代理人的 `Agent(agent_type)` 語法，請參閱 [04-subagents/README.md](04-subagents/README.md#限制可生成的子代理)。

### 內建子代理人

| 代理人 | 說明 | 工具 | 模型 | 使用時機 |
|-------|-------------|-------|-------|-------------|
| **general-purpose** | 多步驟任務、研究 | 所有工具 | 繼承模型 | 複雜研究、多檔案任務 |
| **Plan** | 實作規劃 | Read, Glob, Grep, Bash | 繼承模型 | 架構設計、規劃 |
| **Explore** | 程式碼庫探索 | Read, Glob, Grep | 繼承（上限為 Opus） | 快速搜尋、理解程式碼 |
| **claude** | 處理不適合更專門代理人之任務的通用代理人 | 所有工具 | 繼承模型 | 沒有專門代理人的任務；派送背景工作階段時的預設代理人 |
| **statusline-setup** | 狀態列設定 | Bash, Read, Write | Sonnet 4.6 | 設定狀態列顯示 |
| **claude-code-guide** | 說明與文件 | Read, Glob, Grep | Haiku 4.5 | 取得說明、學習功能 |

### 子代理人設定欄位

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | 代理人識別碼 |
| `description` | string | 代理人的功能描述 |
| `model` | string | 模型覆寫（例如 `haiku-4.5`） |
| `tools` | array | 允許的工具清單 |
| `effort` | string | 推理努力程度（`low`、`medium`、`high`） |
| `initialPrompt` | string | 代理人啟動時注入的系統提示 |
| `disallowedTools` | array | 明確拒絕此代理人使用的工具 |

### 自訂子代理人（範例）

| 代理人 | 說明 | 使用時機 | 範疇 | 安裝 |
|-------|-------------|-------------|-------|--------------|
| `code-reviewer` | 全面的程式碼品質審查 | 程式碼審查工作階段 | 專案 | `cp 04-subagents/code-reviewer.md .claude/agents/` |
| `clean-code-reviewer` | 整潔程式碼原則審查 | 可維護性審查 | 專案 | `cp 04-subagents/clean-code-reviewer.md .claude/agents/` |
| `test-engineer` | 測試策略與覆蓋率 | 測試規劃 | 專案 | `cp 04-subagents/test-engineer.md .claude/agents/` |
| `documentation-writer` | 技術文件撰寫 | API 文件、指南 | 專案 | `cp 04-subagents/documentation-writer.md .claude/agents/` |
| `secure-reviewer` | 以安全性為重點的審查 | 安全性稽核 | 專案 | `cp 04-subagents/secure-reviewer.md .claude/agents/` |
| `implementation-agent` | 完整功能實作 | 功能開發 | 專案 | `cp 04-subagents/implementation-agent.md .claude/agents/` |
| `debugger` | 根本原因分析 | 錯誤調查 | 使用者 | `cp 04-subagents/debugger.md .claude/agents/` |
| `data-scientist` | SQL 查詢、資料分析 | 資料任務 | 使用者 | `cp 04-subagents/data-scientist.md .claude/agents/` |
| `performance-optimizer` | 效能剖析與調整 | 瓶頸調查 | 專案 | `cp 04-subagents/performance-optimizer.md .claude/agents/` |

> **範疇**：`使用者` = 個人（`~/.claude/agents/`），`專案` = 團隊共用（`.claude/agents/`）

**參考**：[04-subagents/](04-subagents/) | [官方文件](https://code.claude.com/docs/en/sub-agents)

**快速安裝（所有自訂代理人）**：
```bash
cp 04-subagents/*.md .claude/agents/
```

---

## 技能

具有指令、腳本與範本的自動呼叫能力。

### 技能範例

| 技能 | 說明 | 自動呼叫時機 | 範疇 | 安裝 |
|-------|-------------|-------------------|-------|--------------|
| `code-review-specialist` | 全面的程式碼審查 | 「審查這段程式碼」、「確認品質」 | 專案 | `cp -r 03-skills/code-review-specialist .claude/skills/` |
| `brand-voice` | 品牌一致性檢查器 | 撰寫行銷文案 | 專案 | `cp -r 03-skills/brand-voice .claude/skills/` |
| `doc-generator` | API 文件產生器 | 「產生文件」、「記錄 API」 | 專案 | `cp -r 03-skills/doc-generator .claude/skills/` |
| `refactor` | 系統性程式碼重構（Martin Fowler） | 「重構這個」、「清理程式碼」 | 使用者 | `cp -r 03-skills/refactor ~/.claude/skills/` |
| `claude-md` | 建立或更新 CLAUDE.md 檔案 | 「建立 CLAUDE.md」、「稽核 CLAUDE.md」 | 專案 | `cp -r 03-skills/claude-md .claude/skills/` |
| `blog-draft` | 根據想法與資源草擬部落格文章 | 「寫一篇部落格文章」、「草擬一篇文章」 | 使用者 | `cp -r 03-skills/blog-draft ~/.claude/skills/` |

> **範疇**：`使用者` = 個人（`~/.claude/skills/`），`專案` = 團隊共用（`.claude/skills/`）

### 技能結構

```text
~/.claude/skills/skill-name/
├── SKILL.md          # 技能定義與指令
├── scripts/          # 輔助腳本
└── templates/        # 輸出範本
```

### 技能前置元資料欄位

技能在 `SKILL.md` 中支援 YAML 前置元資料進行設定：

| 欄位 | 類型 | 說明 |
|-------|------|-------------|
| `name` | string | 技能顯示名稱 |
| `description` | string | 技能的功能描述 |
| `autoInvoke` | array | 自動呼叫的觸發詞 |
| `effort` | string | 推理努力程度（`low`、`medium`、`high`） |
| `shell` | string | 腳本使用的 shell（`bash`、`zsh`、`sh`） |

**參考**：[03-skills/](03-skills/) | [官方文件](https://code.claude.com/docs/en/skills)

**快速安裝（所有技能）**：
```bash
cp -r 03-skills/* ~/.claude/skills/
```

### 內建技能

| 技能 | 說明 | 自動呼叫時機 |
|-------|-------------|-------------------|
| `/batch` | 對多個檔案執行提示 | 批次操作 |
| `/claude-api` | 使用 Claude API 建置應用程式 | API 開發 |
| `/debug` | 除錯失敗的測試／錯誤 | 除錯工作階段 |
| `/design` *(研究預覽，v2.1.233+)* | 建立多畫板的設計畫布——UI 模型、畫面流程、登陸頁、海報——以視覺方式而非程式碼進行調整。適用 Pro/Max/Team/Enterprise | 設計你寧可手動微調、而非用 HTML 撰寫的畫面或頁面 |
| `/fewer-permission-prompts` | 掃描逐字稿並提出優先允許清單 | 減少重複的權限提示 |
| `/loop` | 依間隔執行提示 | 週期性任務 |
| `/run` *(v2.1.145+)* | 啟動本專案的應用程式以確認變更運作正常 | 在實際應用程式中驗證變更 |
| `/run-skill-generator` *(v2.1.145+)* | 教導 `/run`／`/verify` 如何處理特定專案 | 首次為 `/run` 設定專案 |
| `/code-review [low\|medium\|high\|xhigh\|max\|ultra] [--fix] [--comment] [pr#\|branch\|path]` | 審查目前的差異——或某個 PR、分支或路徑——中的正確性錯誤。`--fix` 會套用審查結果，`--comment` 會將結果發布為內嵌 PR 評論。未指定等級時沿用你上次輸入的等級（v2.1.223）。較低的努力等級在 v2.1.218 移至**背景子代理人**執行；`high`、`xhigh` 與 `max` 於 v2.1.232 跟進，因此審查輸出不再塞滿對話 | 撰寫程式碼後、合併 PR 前 |
| `/simplify` *(自 v2.1.154 起再次獨立)* | 僅做清理的審查（重用／簡化／效率／抽象層級），並套用修正；不會搜尋錯誤 | 整理程式碼而不進行錯誤搜查 |
| `/verify` *(v2.1.145+)* | 建置、執行並觀察應用程式以確認修復有效 | 端對端驗證修復 |

---

## 外掛

命令、代理人、MCP 伺服器與鉤子的捆綁集合。

### 外掛範例

| 外掛 | 說明 | 元件 | 使用時機 | 範疇 | 安裝 |
|--------|-------------|------------|-------------|-------|--------------|
| `pr-review` | PR 審查工作流程 | 3 個命令、3 個代理人、GitHub MCP | 程式碼審查 | 專案 | `/plugin install pr-review` |
| `devops-automation` | 部署與監控 | 4 個命令、3 個代理人、K8s MCP | DevOps 任務 | 專案 | `/plugin install devops-automation` |
| `documentation` | 文件產生套件 | 4 個命令、3 個代理人、範本 | 文件撰寫 | 專案 | `/plugin install documentation` |

> **範疇**：`專案` = 團隊共用，`使用者` = 個人工作流程

### 外掛結構

```text
.claude-plugin/
├── plugin.json       # 清單檔案
├── commands/         # 斜線命令
├── agents/           # 子代理人
├── skills/           # 技能
├── mcp/              # MCP 設定
├── hooks/            # 鉤子腳本
└── scripts/          # 工具腳本
```

**參考**：[07-plugins/](07-plugins/) | [官方文件](https://code.claude.com/docs/en/plugins)

**外掛管理命令**：
```bash
/plugin list              # 列出已安裝的外掛
/plugin install <name>    # 安裝外掛
/plugin remove <name>     # 移除外掛
claude plugin update <name>   # 更新外掛（CLI；/plugin update 斜線形式僅在說明文字中提及，未列於命令參考）
```

---

## MCP 伺服器

用於存取外部工具與 API 的模型脈絡協定伺服器。

### 常用 MCP 伺服器

| 伺服器 | 說明 | 使用時機 | 範疇 | 安裝 |
|--------|-------------|-------------|-------|--------------|
| **GitHub** | PR 管理、議題、程式碼 | GitHub 工作流程 | 專案 | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| **Database** | SQL 查詢、資料存取 | 資料庫操作 | 專案 | `claude mcp add db -- npx -y @modelcontextprotocol/server-postgres` |
| **Filesystem** | 進階檔案操作 | 複雜檔案任務 | 使用者 | `claude mcp add fs -- npx -y @modelcontextprotocol/server-filesystem` |
| **Slack** | 團隊溝通 | 通知、更新 | 專案 | 在設定中設定 |
| **Google Docs** | 文件存取 | 文件編輯、審查 | 專案 | 在設定中設定 |
| **Asana** | 專案管理 | 任務追蹤 | 專案 | 在設定中設定 |
| **Stripe** | 付款資料 | 財務分析 | 專案 | 在設定中設定 |
| **Memory** | 持久記憶體 | 跨工作階段回憶 | 使用者 | 在設定中設定 |
| **Context7** | 程式庫文件 | 查詢最新文件 | 內建 | 內建 |

> **範疇**：`專案` = 團隊（`.mcp.json`），`使用者` = 個人（`~/.claude.json`），`內建` = 預先安裝

### MCP 設定範例

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**參考**：[05-mcp/](05-mcp/) | [MCP 協定文件](https://modelcontextprotocol.io)

**快速安裝（GitHub MCP）**：
```bash
export GITHUB_TOKEN="your_token" && claude mcp add github -- npx -y @modelcontextprotocol/server-github
```

---

## 鉤子

事件驅動的自動化，在 Claude Code 事件發生時執行 shell 命令。

### 鉤子事件

| 事件 | 說明 | 觸發時機 | 使用案例 |
|-------|-------------|----------------|-----------|
| `SessionStart` | 工作階段開始／恢復 | 工作階段初始化 | 設定任務 |
| `Setup` | 初始環境設定（每個工作階段一次） | 首次工作階段引導 | 佈建工具、安裝相依套件 |
| `InstructionsLoaded` | 指令已載入 | CLAUDE.md 或規則檔案載入 | 自訂指令處理 |
| `UserPromptSubmit` | 提示處理前 | 使用者傳送訊息 | 輸入驗證 |
| `UserPromptExpansion` | 使用者提示已展開（@-提及、斜線命令已解析） | 展開後、提交前 | 轉換或檢查展開後的提示 |
| `PreToolUse` | 工具執行前 | 任何工具執行前 | 驗證、記錄 |
| `PermissionRequest` | 顯示權限對話框 | 敏感動作前 | 自訂核准流程 |
| `PermissionDenied` | 使用者拒絕權限提示 | 權限拒絕後 | 記錄、分析、政策執行 |
| `PostToolUse` | 工具成功後 | 任何工具完成後 | 格式化、通知 |
| `PostToolUseFailure` | 工具執行失敗 | 工具錯誤後 | 錯誤處理、記錄 |
| `PostToolBatch` | 一批工具使用完成後 | 工具批次結束 | 彙總報告、批次驗證 |
| `Notification` | 通知已傳送 | Claude 傳送通知 | 外部警示 |
| `MessageDisplay` | 助理訊息文字正在顯示 | 訊息算繪期間 | 轉換或隱藏顯示的文字 |
| `SubagentStart` | 子代理人已生成 | 子代理人任務開始 | 初始化子代理人脈絡 |
| `SubagentStop` | 子代理人完成 | 子代理人任務完成 | 串接動作 |
| `Stop` | Claude 完成回應 | 回應完成 | 清理、報告 |
| `StopFailure` | API 錯誤結束回合 | API 錯誤發生 | 錯誤復原、記錄 |
| `TeammateIdle` | 隊友代理人閒置 | 代理人團隊協調 | 分配工作 |
| `TaskCompleted` | 任務標記為完成（僅在啟用 todo 工具時觸發——請參閱 [Hooks](06-hooks/README.md#hook-events)） | 任務完成 | 任務後處理 |
| `TaskCreated` | 透過 TaskCreate 建立任務（僅在啟用 todo 工具時觸發——請參閱 [Hooks](06-hooks/README.md#hook-events)） | 新任務建立 | 任務追蹤、記錄 |
| `ConfigChange` | 設定已更新 | 設定修改 | 對設定變更作出反應 |
| `CwdChanged` | 工作目錄變更 | 目錄已變更 | 目錄特定設定 |
| `DirectoryAdded` | 工作階段中途登記新的工作目錄 | `/add-dir` 或 SDK `register_repo_root` | 為新目錄設定工具 |
| `FileChanged` | 監看的檔案變更 | 檔案已修改 | 檔案監控、重建 |
| `PreCompact` | 壓縮操作前 | 脈絡壓縮 | 狀態儲存 |
| `PostCompact` | 壓縮完成後 | 壓縮完成 | 壓縮後動作 |
| `PreModelSwitch` | 在套用所請求的模型切換之前 | 請求切換模型 | 把關或否決模型變更 |
| `PostModelSwitch` | 工作階段的模型變更之後 | 模型切換完成 | 記錄或回應模型變更 |
| `WorktreeCreate` | 工作樹正在建立 | Git 工作樹已建立 | 設定工作樹環境 |
| `WorktreeRemove` | 工作樹正在移除 | Git 工作樹已移除 | 清理工作樹資源 |
| `Elicitation` | MCP 伺服器請求輸入 | MCP 引導 | 輸入驗證 |
| `ElicitationResult` | 使用者回應引導 | 使用者回應 | 回應處理 |
| `SessionEnd` | 工作階段結束 | 工作階段終止 | 清理、儲存狀態 |

### 鉤子範例

| 鉤子 | 說明 | 事件 | 範疇 | 安裝 |
|------|-------------|-------|-------|--------------|
| `pre-tool-check.sh` | 封鎖／警告有風險的 Bash 命令 | PreToolUse:Bash | 使用者 | `cp 06-hooks/pre-tool-check.sh ~/.claude/hooks/` |
| `security-scan.sh` | 安全性掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/security-scan.sh .claude/hooks/` |
| `format-code.sh` | 自動格式化 | PostToolUse:Write | 使用者 | `cp 06-hooks/format-code.sh ~/.claude/hooks/` |
| `validate-prompt.sh` | 提示驗證 | UserPromptSubmit | 專案 | `cp 06-hooks/validate-prompt.sh .claude/hooks/` |
| `context-tracker.py` | Token 使用量追蹤 | UserPromptSubmit, Stop | 使用者 | `cp 06-hooks/context-tracker.py ~/.claude/hooks/` |
| `context-tracker-tiktoken.py` | Token 使用量追蹤（tiktoken，約 90-95% 準確度） | UserPromptSubmit, Stop | 使用者 | `cp 06-hooks/context-tracker-tiktoken.py ~/.claude/hooks/` |
| `pre-commit.sh` | 提交前驗證 | PreToolUse:Bash | 專案 | `cp 06-hooks/pre-commit.sh .claude/hooks/` |
| `log-bash.sh` | 命令記錄 | PostToolUse:Bash | 使用者 | `cp 06-hooks/log-bash.sh ~/.claude/hooks/` |
| `dependency-check.sh` | 清單變更時的漏洞掃描 | PostToolUse:Write | 專案 | `cp 06-hooks/dependency-check.sh .claude/hooks/` |
| `notify-team.sh` | git push 時通知團隊 | PostToolUse:Bash | 專案 | `cp 06-hooks/notify-team.sh .claude/hooks/` |
| `session-end.sh` | 工作階段結束時擷取進度 | SessionEnd | 使用者 | `cp 06-hooks/session-end.sh ~/.claude/hooks/` |

> **範疇**：`專案` = 團隊（`.claude/settings.json`），`使用者` = 個人（`~/.claude/settings.json`）

### 鉤子設定

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "command": "~/.claude/hooks/pre-tool-check.sh"
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "~/.claude/hooks/format-code.sh"
      }
    ]
  }
}
```

**參考**：[06-hooks/](06-hooks/) | [官方文件](https://code.claude.com/docs/en/hooks)

**快速安裝（所有鉤子）**：
```bash
mkdir -p ~/.claude/hooks && cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh
```

---

## 記憶體檔案

跨工作階段自動載入的持久脈絡。

### 記憶體類型

| 類型 | 位置 | 範疇 | 使用時機 |
|------|----------|-------|-------------|
| **受管理政策** | 組織管理政策 | 組織 | 強制執行全組織標準 |
| **專案** | `./CLAUDE.md` | 專案（團隊） | 團隊標準、專案脈絡 |
| **專案規則** | `.claude/rules/` | 專案（團隊） | 模組化專案規則 |
| **使用者** | `~/.claude/CLAUDE.md` | 使用者（個人） | 個人偏好設定 |
| **使用者規則** | `~/.claude/rules/` | 使用者（個人） | 模組化個人規則 |
| **本機** | `./CLAUDE.local.md` | 本機（git 忽略） | 機器特定的本機覆寫（已加入 gitignore）。於 https://code.claude.com/docs/en/memory 記載為支援的每位開發者覆寫檔案。 |
| **自動記憶體** | 自動 | 工作階段 | 自動擷取的洞察與更正 |

> **範疇**：`組織` = 由管理員管理，`專案` = 透過 git 與團隊共用，`使用者` = 個人偏好，`本機` = 不提交，`工作階段` = 自動管理

**參考**：[02-memory/](02-memory/) | [官方文件](https://code.claude.com/docs/en/memory)

**快速安裝**：
```bash
cp 02-memory/project-CLAUDE.md ./CLAUDE.md
cp 02-memory/personal-CLAUDE.md ~/.claude/CLAUDE.md
```

---

## 新功能

| 功能 | 說明 | 使用方式 |
|---------|-------------|------------|
| **/focus** | 切換無干擾輸出顯示的專注檢視（v2.1.110） | 執行 `/focus` 以在長任務期間減少視覺雜訊 |
| **/proactive** | `/loop` 的別名——相同的週期性任務行為（v2.1.105） | 可與 `/loop` 互換使用 `/proactive` |
| **/recap** | 返回現有工作階段時顯示摘要（v2.1.108） | 離開後執行 `/recap` 以取得已完成工作的脈絡 |
| **/tui** | 切換全螢幕 TUI（文字使用者介面）模式以實現無閃爍渲染（v2.1.110） | 在全螢幕終端機或 tmux 中使用 `/tui` |
| **/undo** | **已不再記載於文件**——於 v2.1.108 新增為 `/rewind` 的別名，但官方命令參考中已找不到 | 改用 `/rewind`（或 `Esc Esc`） |
| **Monitor 工具** | 監看背景命令的 stdout 串流並對事件作出反應，而非輪詢（v2.1.98+） | 透過[進階功能](09-advanced-features/)使用 Monitor 工具 |
| **Output Styles** | 透過系統提示變更 Claude 的角色、語氣與預設回應格式。內建：Default、Proactive、Explanatory、Learning、Concise | `/config` → Output style、設定 `outputStyle`，或執行 `/output-style`（於 v2.1.91 移除後，v2.1.269 重新加入）。請參閱[進階功能](09-advanced-features/#輸出樣式-output-styles) |
| **Status Line** | 由命令算繪自訂狀態列，該命令會透過 stdin 接收工作階段、模型、費用與脈絡的 JSON | `/statusline` 或 `statusLine` 設定。請參閱[進階功能](09-advanced-features/#狀態列-status-line) |
| **社群市集** | 通過 Anthropic 自動化驗證的第三方外掛，每個都固定於某個 commit SHA | `/plugin marketplace add anthropics/claude-plugins-community`，接著 `/plugin install <name>@claude-community` |
| **/team-onboarding** | 從專案的 Claude Code 設定自動產生隊友入門指南（v2.1.101） | 在你的專案中執行 `/team-onboarding` |
| **遠端控制** | 透過 API 遠端控制 Claude Code 工作階段。**已不再是研究預覽**——任何執行 `claude remote-control` 的機器都會以裝置卡片形式出現在 Claude app 的 Code 分頁中，因此可以從手機在該機器上啟動工作階段 | 在該機器上執行 `claude remote-control`，然後在 Claude app 中選擇其裝置卡片。請參閱[進階功能](09-advanced-features/README.md) |
| **網頁工作階段** | 在瀏覽器環境中執行 Claude Code | 透過 `claude web` 或 Anthropic Console 存取 |
| **桌面應用程式** | Claude Code 的原生桌面應用程式 | 使用 `/desktop` 或從 Anthropic 網站下載 |
| **跨工作階段訊息** | 工作階段之間可以互傳訊息——包括你的其他機器與雲端工作階段——透過 `ListAgents` 探索（v2.1.224+，macOS/Linux） | 請參閱[進階功能](09-advanced-features/README.md#跨工作階段訊息傳遞)；以 `crossSessionInbound` 控制傳入訊息 |
| **自架執行器** | 在你自己的機器或容器上執行 Claude Code 網頁版、行動版與桌面版工作階段（v2.1.224+，Team/Enterprise） | `claude self-hosted-runner setup`；請參閱 [CLI](10-cli/README.md) |
| **`archive` 外掛來源** | 從 HTTPS zip 安裝外掛，可選擇以 `sha256` 固定（v2.1.224+） | 請參閱[外掛](07-plugins/README.md#archive-來源-v21224) |
| **`command` 外掛來源** | 由本機安裝的工具輸出外掛目錄路徑（v2.1.229+） | 請參閱[外掛](07-plugins/README.md#command-來源-v21229) |
| **市集擁有者萬用字元** | `"owner/*"` 可允許或封鎖某個 GitHub 擁有者底下的所有市集儲存庫——僅限 `strictKnownMarketplaces` 與 `blockedMarketplaces`（v2.1.223+） | 請參閱[外掛](07-plugins/README.md#市場設定) |
| **沙箱憑證遮罩** | 沙箱化的命令讀取的是哨兵值，而代理伺服器會在對外傳輸時替換成真正的密鑰（v2.1.221+，Linux/WSL） | 請參閱[進階功能](09-advanced-features/README.md#憑證遮罩v21221-v21224) |
| **代理人團隊** | 協調多個代理人處理相關任務 | 設定協作並共用脈絡的隊友代理人 |
| **任務清單** | 背景任務管理與監控 | 使用 `/tasks` 檢視和管理背景操作 |
| **提示建議** | 情境感知的命令建議 | 建議根據目前脈絡自動出現 |
| **Git 工作樹** | 用於平行開發的隔離 git 工作樹 | 使用工作樹命令進行安全的平行分支工作 |
| **沙箱化** | 用於安全的隔離執行環境 | 使用 `/sandbox` 切換；在受限環境中執行命令 |
| **MCP OAuth** | MCP 伺服器的 OAuth 驗證 | 在 MCP 伺服器設定中設定 OAuth 憑證以進行安全存取 |
| **MCP 工具搜尋** | 動態搜尋並探索 MCP 工具 | 使用工具搜尋在已連接的伺服器中尋找可用的 MCP 工具 |
| **排程任務** | 使用 `/loop` 和 cron 工具設定週期性任務 | 使用 `/loop 5m /command` 或 CronCreate 工具 |
| **Chrome 整合** | 使用無頭 Chromium 進行瀏覽器自動化 | 使用 `--chrome` 旗標或 `/chrome` 命令 |
| **鍵盤自訂** | 自訂鍵盤綁定，包括和弦支援 | 使用 `/keybindings` 或編輯 `~/.claude/keybindings.json` |
| **Auto 模式** | 完全自主操作，並由背景安全分類器檢查 | 按 `Shift+Tab` 切換至此模式，或使用 `--permission-mode auto` |
| **頻道** | 多頻道通訊（Telegram、Slack 等）（研究預覽） | 設定頻道外掛；2026 年 3 月 |
| **語音輸入** | 提示的語音輸入 | 使用麥克風圖示或語音鍵盤綁定 |
| **代理人鉤子類型** | 生成子代理人而非執行 shell 命令的鉤子 | 在鉤子設定中設定 `"type": "agent"` |
| **提示鉤子類型** | 將提示文字注入對話的鉤子 | 在鉤子設定中設定 `"type": "prompt"` |
| **MCP 引導** | MCP 伺服器可在工具執行期間請求使用者輸入 | 透過 `Elicitation` 和 `ElicitationResult` 鉤子事件處理 |
| **外掛 LSP 支援** | 透過外掛整合語言伺服器協定 | 在 `plugin.json` 中設定 LSP 伺服器以獲得編輯器功能 |
| **受管理插入設定** | 組織管理的插入設定（v2.1.83） | 由管理員透過受管理政策設定；自動套用至所有使用者 |
| **`claude plugin init`** | 在 `.claude/skills` 中建立新外掛的骨架；這類外掛不需市集即可自動載入（v2.1.157） | 執行 `claude plugin init <name>` |
| **第三方供應商上的 Auto 模式** | 在 Amazon Bedrock、Google Cloud 的 Agent Platform、Microsoft Foundry 以及已登入的 Claude apps gateway 工作階段上預設可用，支援的模型為 Claude Sonnet 5、Opus 4.7 或更新版本（包括 Opus 5）以及 Fable 5（v2.1.158–v2.1.206 需選擇啟用；v2.1.207 移除此要求——`CLAUDE_CODE_ENABLE_AUTO_MODE` 仍可接受但已無作用） | 按 `Shift+Tab` 切換至此模式，或使用 `--permission-mode auto` |
| **即時對話框命令** | 可在 Claude 工作時開啟 `/permissions`（規則變更會套用到該回合的剩餘部分），而 `/add-dir`、`/autocompact`、`/theme`、`/help`、`/config` 與 `/advisor` 對話框**在全螢幕 TUI 中**會於回合中途開啟，而不是排隊等到 Claude 回應結束（`/bug` 自 v2.1.232 起已是如此）（v2.1.234） | 在 Claude 工作時執行上述命令之一。請參閱[斜線命令](01-slash-commands/README.md) |
| **`CLAUDE_CODE_PROJECT_DIR_NAME`** | 控制每個專案逐字稿目錄命名的環境變數（v2.1.234） | 在啟動 Claude Code 前於 shell／環境中設定。請參閱 [CLI](10-cli/README.md) |
| **目標回報門檻** | 當 `/goal` 啟用時，若背景任務超過 30 分鐘沒有進展，Claude 會回報狀態更新，而不是默默繼續；可用 `CLAUDE_CODE_GOAL_CHECKIN_MINUTES` 調整，設為 `0` 則停用（v2.1.234） | 在啟用 `/goal` 時設定 `CLAUDE_CODE_GOAL_CHECKIN_MINUTES=<n>`。請參閱[斜線命令](01-slash-commands/README.md) |
| **使用上限自動繼續** | 若工作階段因 claude.ai 使用上限而被阻擋，Claude Code 會在上限重設時自動繼續（v2.1.234） | 預設開啟；可在 `/config` →「Continue automatically at usage limit」中關閉。請參閱[進階功能](09-advanced-features/) |
| **`spellcheck` 設定** | 使用 `PATH` 中可用的 aspell、hunspell 或 ispell，在提示輸入中為拼錯的單字加上底線；預設關閉（v2.1.235） | 安裝拼字檢查器，然後在 `~/.claude/settings.json` 中設定 `"spellcheck": { "enabled": true }`。僅限使用者設定、`--settings` 與受管理設定——在專案設定中會被忽略。請參閱[進階功能](09-advanced-features/) |
| **代理人團隊預設模型** | 已移除「Default teammate model」`/config` 設定；除非生成呼叫明確指定，否則隊友預設繼承團隊負責人的模型（v2.1.234） | 請參閱[子代理人——代理人團隊](04-subagents/README.md#agent-teams實驗性功能) |
| **`/design`** | 設計畫布——建立在 artifacts 之上的多畫板視覺設計（UI 模型、畫面流程、登陸頁、海報），以視覺方式而非程式碼進行調整。研究預覽；需要 v2.1.233+；適用 Pro/Max/Team/Enterprise | 在 CLI 或 Desktop app 中執行 `/design`。請參閱[斜線命令](01-slash-commands/README.md) |
| **`notify_when_idle`** | 跨工作階段 `SendMessage` 的輸入參數，要求同一台機器上的另一個工作階段在下次閒置時傳送一次通知——需選擇啟用、單次、不輪詢（v2.1.236）。相關：`ListAgents` 會回報工作階段自己的名稱並列出存活中的隊友，且 Windows 也支援跨工作階段訊息（v2.1.239） | 將 `notify_when_idle` 傳給 `SendMessage`。請參閱[進階功能](09-advanced-features/README.md#跨工作階段訊息傳遞) |
| **外掛清單欄位** | `plugin.json` 接受 `workflows`、`channels`、`dependencies`（semver）、`outputStyles`、`keywords`、`metadata`、`lspServers` 以及 `experimental.themes`／`experimental.monitors`。CLI 新增 `claude plugin new`、`remove`/`rm`、`prune`/`autoremove`，以及旗標 `--with`、`-f`/`--force`、`--available`、`--push`、`--dry-run` | 請參閱[外掛](07-plugins/README.md) |
| **Restricted Mode** | 除非 `--tools` 明確指定，否則移除會執行命令或程式碼的內建工具（Bash、PowerShell、REPL）及 WebFetch；忽略使用者、專案與本機設定（受管理設定與 `--settings` 仍會套用）；將檔案工具限制在工作目錄內；拒絕 `bypassPermissions`；並拒絕建立雲端工作階段（v2.1.248+） | `claude --restricted`，或 `CLAUDE_CODE_RESTRICTED=1`。請參閱 [CLI](10-cli/README.md) |
| **`/advisor` 文字形式** | `/advisor`、`/advisor <model>` 與 `/advisor off` 在桌面應用程式、Remote Control 以及其他無頭（`-p`／Agent SDK）工作階段中可作為文字命令使用，而不只是對話框（v2.1.260+） | 在無頭工作階段中輸入 `/advisor off`。請參閱[斜線命令](01-slash-commands/README.md) |

---

## 快速參考矩陣

### 功能選擇指南

| 需求 | 建議功能 | 原因 |
|------|---------------------|-----|
| 快速捷徑 | 斜線命令 | 手動、即時 |
| 持久脈絡 | 記憶體 | 自動載入 |
| 複雜自動化 | 技能 | 自動呼叫 |
| 專業任務 | 子代理人 | 隔離脈絡 |
| 外部資料 | MCP 伺服器 | 即時存取 |
| 事件自動化 | 鉤子 | 事件觸發 |
| 完整解決方案 | 外掛 | 一站式捆綁 |

### 安裝優先順序

| 優先順序 | 功能 | 命令 |
|----------|---------|---------|
| 1. 必要 | 記憶體 | `cp 02-memory/project-CLAUDE.md ./CLAUDE.md` |
| 2. 日常使用 | 斜線命令 | `cp 01-slash-commands/*.md .claude/commands/` |
| 3. 品質 | 子代理人 | `cp 04-subagents/*.md .claude/agents/` |
| 4. 自動化 | 鉤子 | `cp 06-hooks/*.sh ~/.claude/hooks/ && chmod +x ~/.claude/hooks/*.sh` |
| 5. 外部 | MCP | `claude mcp add github -- npx -y @modelcontextprotocol/server-github` |
| 6. 進階 | 技能 | `cp -r 03-skills/* ~/.claude/skills/` |
| 7. 完整 | 外掛 | `/plugin install pr-review` |

---

## 完整一行命令安裝

從此儲存庫安裝所有範例：

```bash
# 建立目錄
mkdir -p .claude/{commands,agents,skills} ~/.claude/{hooks,skills}

# 安裝所有功能
cp 01-slash-commands/*.md .claude/commands/ && \
cp 02-memory/project-CLAUDE.md ./CLAUDE.md && \
cp -r 03-skills/* ~/.claude/skills/ && \
cp 04-subagents/*.md .claude/agents/ && \
cp 06-hooks/*.sh ~/.claude/hooks/ && \
chmod +x ~/.claude/hooks/*.sh
```

---

## 其他資源

- [Claude Code 官方文件](https://code.claude.com/docs/en/overview)
- [MCP 協定規格](https://modelcontextprotocol.io)
- [學習路線圖](LEARNING-ROADMAP.md)
- [主要 README](README.md)

---

**最後更新**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/sub-agents
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://code.claude.com/docs/en/overview
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/hooks
- https://code.claude.com/docs/en/permission-modes
- https://code.claude.com/docs/en/changelog#2-1-172
- https://code.claude.com/docs/en/changelog#2-1-174
- https://github.com/anthropics/claude-code/releases/tag/v2.1.145
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154
- https://code.claude.com/docs/en/plugins
- https://code.claude.com/docs/en/cli-reference
- https://code.claude.com/docs/en/model-config
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/plugin-marketplaces
- https://code.claude.com/docs/en/discover-plugins
- https://code.claude.com/docs/en/settings
- https://code.claude.com/docs/en/plugins-reference
- https://code.claude.com/docs/en/slash-commands
**相容模型**：Claude Fable 5.1、Claude Fable 5、Claude Opus 5、Claude Sonnet 5、Claude Sonnet 4.6、Claude Opus 4.8、Claude Haiku 4.5
