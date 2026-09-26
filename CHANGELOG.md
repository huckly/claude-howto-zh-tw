# 更新日誌

## [v2.1.245] — 2026-08-25

### 與 Claude Code v2.1.245 的文件同步

Claude Code 從 v2.1.235（2026-08-19）更新到 **v2.1.245**（2026-08-25），共十個
版本。本條目記錄這次同步。

> **注意**：本更新日誌沒有 v2.1.233 與 v2.1.235 同步的條目
> （commit `deb7e5c` 與 `da6e09e`），這兩次同步進入 repository 時並未附上
> 更新日誌條目。這個缺口在此記錄，而不是以重建的日期回填。

### 修正

- **版本現況的說法過時且互相矛盾** — 根目錄
  `README.md` 一處寫 `v2.1.235`，另一處寫 `v2.1.220 (July 2026)`，
  而本更新日誌的最上方條目仍稱 v2.1.220 為目前的
  Claude Code 版本。現在全部改為 v2.1.245。
- **Hook 預設逾時**由 60 秒更正為 **600** 秒（適用於
  `command`、`http` 與 `mcp_tool`；`prompt` 為 30、`agent` 為 60），並記錄了
  各事件的覆寫值。
- **技能優先順序**更正為 **enterprise > personal > project**；該
  課程把 project 與 personal 對調了，與它自己的測驗互相矛盾。
- **記憶檔案是串接，而非覆寫** — `02-memory/README.md` 中最後兩處
  「overrides root CLAUDE.md」字串已替換。
- **`SECURITY.md`** 不再宣稱支援「1.x releases」。
- **`STYLE_GUIDE.md`** 不再以範例教授過時的頁尾。
- 評分教材（`lesson-quiz` 與 `self-assessment` 技能）已與其評分的
  課程重新對齊：31 個 hook 事件、五種 hook 類型、六個 rewind
  選項、六種權限模式、匯入深度 4。

### 新增

- **WebSocket (`ws`) MCP transport** — 第四種 transport，可透過
  `.mcp.json` 或 `claude mcp add-json` 設定。
- **`Concise` 輸出風格** — 第五個內建風格（v2.1.237）。
- **新的設定鍵** — `modelPicker`、`promptCacheTtl`、
  `subagentPromptCacheTtl`、`modelPricing`、`keybindingFlavor`。
- **`ANTHROPIC_DEFAULT_MODEL`** 環境變數（v2.1.236）。
- **`/design`** — 設計畫布（design canvas）研究預覽版。
- 跨工作階段 `SendMessage` 的 **`notify_when_idle`**（v2.1.236）。
- **Plugin manifest 欄位** — `workflows`、`channels`、`dependencies`，以及
  相關的 CLI 新增項目。

### 變更

- **Remote Control** 已脫離研究預覽；執行
  `claude remote-control` 的機器現在會以裝置卡片的形式出現在 Claude app 中。
- **`Ctrl+L`** 改為重繪畫面，而不是清除畫面（v2.1.238）。
- **Plugin `commands/`** 標記為舊版（legacy）；目前的建議做法是 `skills/`。

## [v2.1.220-r2] — 2026-08-04

### 針對 Claude Code v2.1.220 的準確性校正（上游版本未變更）

v2.1.220（2026-07-25）仍是目前的 Claude Code 版本，因此本條目
記錄的是**內部修正**，而非版本同步。對照官方文件的稽核沒有發現
遺漏的上游功能 — v2.1.218–v2.1.220 的每項功能都已記錄在文件中。
它發現的是損壞的範例程式碼、各檔案間不一致的數量與名稱，以及
metadata 漂移。

### 修正

- **`06-hooks/pre-commit.sh` 實際上從未阻擋過 commit** — 該腳本
  在四處印出「Commit blocked.」後接著 `exit 1`。依照本 repo 自己的
  結束碼表（`06-hooks/README.md`），exit 1 是*非阻擋性*錯誤，因此
  commit 仍會繼續；只有 exit 2 才會阻擋。四處現在都改為 `exit 2`，並將
  原因寫入 stderr，Claude Code 正是從那裡讀取阻擋原因。
- **`06-hooks/dependency-check.sh` 從未執行** — 它從 `$1` 讀取目標，但
  hooks 是從 stdin 接收 JSON，所以路徑永遠是空的，腳本會
  立即結束。現在改為從 stdin 解析 `file_path`，與 `format-code.sh` 一致。
- **`claude_concepts_guide.md` 中的巢狀子代理深度寫成 5** — 原文為「up to 5
  levels deep as of v2.1.172」，與其他四個檔案中已記錄的深度 3 預設值（v2.1.219）
  互相矛盾。已改寫為先說明目前行為，並附上與
  `04-subagents/README.md` 相同的三階段歷史說明。
- **五個檔案中的 hook 事件數量寫成 29** — `README.md`、`CATALOG.md`、
  `QUICK_REFERENCE.md`（×4）、`INDEX.md` 與 `resources.md` 都寫 29；正確
  數字是 **31**，已逐一對照官方 hook 清單確認。上一次
  同步修正了兩個檔案，漏掉其餘的。`README.md` 的事件清單也
  只列出 25 個名稱，`INDEX.md` 只列出 29 個 — 現在兩者都列出全部 31 個。
- **`/less-permission-prompts` 不是真正存在的命令** — 內建命令是
  `/fewer-permission-prompts`。`CATALOG.md` 曾在同一個檔案中*同時*使用這兩個名稱。
- **`/fork` 與 `/subtask` 的意義被對調記錄** — 兩者在
  v2.1.212 互換了角色。`/fork` 現在會將對話複製到一個新的獨立
  背景工作階段；分岔子代理（forked-subagent）的行為則移到 `/subtask`，而後者
  完全不存在於 repo 中。已在四個檔案中更正並補上。
- **`08-checkpoints/README.md` 聲稱 `cleanupPeriodDays` 是唯一的檢查點
  設定** — 另外還有 `fileCheckpointingEnabled`（預設 `true`），以及
  `CLAUDE_CODE_DISABLE_FILE_CHECKPOINTING` 與 100 個檢查點的保留上限。本
  repo 自己的 `config-examples.json` 早已使用了它所否認存在的那個鍵。
- **`03-skills/refactor/SKILL.md` 宣告了 `name: code-refactor`**，但卻位於
  `refactor/` 中 — 與上次同步中 `doc-generator` 修正的是同一種缺陷。已在
  英文版中重新命名，並同步到 `ja/`、`zh/`、`uk/`、`vi/`。
- **`03-skills/README.md` 以兩種互相矛盾的方式陳述技能優先順序** —
  一處寫「enterprise > personal > project」，另一處寫「project wins by default」。
  現在一律為 **enterprise > project > personal**。
- **`claude-md` 技能對 AGENTS.md 的描述錯誤** — 把它稱為
  agent 定義格式。它其實是跨工具的專案情境檔案，Claude Code
  **不會直接讀取它**；必須透過 `@AGENTS.md` 匯入或建立符號連結。
- **`02-memory/directory-api-CLAUDE.md` 聲稱它會「覆寫」根目錄 CLAUDE.md** —
  記憶檔案是串接的，從不覆寫。
- **`INDEX.md` 把 `auto` 與 `dontAsk` 描述反了** — `dontAsk` 被寫成
  寬鬆模式（「accept all except risky」），但它其實是最嚴格的模式：
  它會自動拒絕任何未預先核准的操作。已更正為官方定義。
- **`permissions.mode` 不是設定鍵** — `09-advanced-features/README.md` 與
  `claude_concepts_guide.md` 中有四個設定範例使用了它（搭配已被取代的值
  `default`）。現在全部改用 `permissions.defaultMode: "manual"`。
- **`09-advanced-features/config-examples.json` 沒有 Opus 5** — 其三個
  「最強大」設定檔仍使用 `claude-opus-4-8`，儘管自 v2.1.219 起 Opus 5 已是
  預設的 Opus。
- **`05-mcp/database-mcp.json` 寫死了憑證** —
  `postgresql://user:pass@localhost/mydb`，與該模組自己的「不要
  寫死憑證」規則相矛盾。現在改為 `${DATABASE_URL}`。
- **`05-mcp/README.md` 記錄了 MCP scope，卻從未提到 `--scope` 旗標** —
  讀者根本無法實際選擇 scope。已補上並附範例。
- **`/output-style` 被列為已棄用** — 它其實已在 v2.1.91 *移除*。
  輸出風格仍可透過 `/config` 或 `outputStyle` 設定使用。
- **Hook 的 `permissionDecision` 缺少 `defer`** — 可接受的值為
  `allow`、`deny`、`ask`、`defer`，優先順序為 `deny` > `defer` > `ask` > `allow`。
- **Context-tracker hooks 假設 context window 為 128k** — 目前沒有任何 Claude 模型是這個大小。
  預設值已提高到 1M，並註明 Haiku 4.5 為 200k。
- **`claude_concepts_guide.md` 中的本機記憶路徑錯誤** —
  `.claude/local/CLAUDE.md` 已更正為 `./CLAUDE.local.md`。
- **`03-skills/doc-generator/SKILL.md` 中的 fence 損壞** — 外層的 ```` ```markdown ````
  區塊提早關閉，並留下一個未關閉的多餘 fence，導致範例有一半被渲染
  成實際的 markdown。現在改用四個反引號的外層 fence。兩個驗證器都沒有抓到
  這個問題，因為 `check_markdown_rendering.py` 只掃描 README 檔案。
- **三個命令範本的技能名稱無效** — `Documentation Refactor`、
  `Setup CI/CD Pipeline` 與 `Expand Unit Tests` 不是有效的 `name:` 值
  （只能用小寫與連字號），因此若依照 README 的指示複製到 `.claude/skills/`
  就會失敗。同時移除了非標準的 `tags:` 欄位。
- **九個 plugin agent 使用小寫工具名稱**（`read, grep, bash`），而
  `04-subagents/` 範本使用標準的 `Read, Grep, Bash`。已統一。
- **五個檔案中的「Claude Code 1.0+」需求** — 提高為 2.1+。
- **清點數量錯誤** — `CATALOG.md` 的摘要表加總不正確，*而且*
  其中三個 Examples 數量有誤（Subagents 11→9、MCP 8→4、Hooks 8→11）；
  `INDEX.md` 低估了 Plugins（27→39）、Skills（21→23）、Hooks（9→12）、Subagents
  （9→10）與 Advanced（3→4），漏列三個 hook 腳本以及
  `setup-auto-mode-permissions.py`，並標錯三個 hook 事件。全部依
  檔案樹重新清點。
- **`CONTRIBUTING.md` 寫著「all four checks」** — 實際上有五項。
- **兩個子代理範本未記錄在文件中** — `clean-code-reviewer.md` 與
  `performance-optimizer.md` 存在於磁碟上，但既不在 `04-subagents/README.md`
  的範例清單中，也不在其檔案樹中。
- **`resources.md` 的 Mermaid 圖使用了不符指南的配色** — 已重新對應到
  `STYLE_GUIDE.md` 的五種顏色，並加上必要的 `stroke`/`color`。
- **Metadata 頁尾** — 61 個檔案使用舊版只有 `Last Updated` 的頁尾，或完全
  沒有頁尾；現在全部都有完整區塊。四個模組 README 缺少
  `Compatible Models` 行，而 `02-memory/README.md` 停留在 v2.1.217，且
  日期為 ISO 格式。範圍內全部 87 個 markdown 檔案現在都標示 **2.1.220**。
- **較小的修正** — 在 `session-end.sh` 中明確加上 `exit 0`；四個腳本中的
  `# Hook: Event:Matcher` 簡寫（並非真正的語法）已改寫；四個 MCP 範例設定
  加上 `"type": "stdio"`；一個實作範例在一處寫 42 個 PR、另一處寫
  47 個，已修正。

## [v2.1.220] — 2026-07-29

### 已同步至 Claude Code v2.1.220

將教學涵蓋範圍從 v2.1.217 基準（2026-07-22 同步）提升到
v2.1.220 — 三個連續版本（v2.1.218、v2.1.219、v2.1.220），沒有缺漏。
本次同步大多由 v2.1.219 驅動：它新增了 Claude Opus 5，並再次反轉了
上一次同步才剛記錄的巢狀子代理預設值。

### 修正

- **巢狀子代理預設值再次反轉（v2.1.219）** — 在
  `04-subagents/README.md`、`CATALOG.md`、`10-cli/README.md` 與
  `QUICK_REFERENCE.md` 中共有五處寫著巢狀生成預設為停用。這
  只在 v2.1.217–v2.1.218 正確。v2.1.219 將預設值設為**深度
  3**；`CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH=1` 現在是*停用*巢狀，而
  不是啟用。五處現在都先說明目前行為，並附上一段
  完全相同的三階段歷史說明（v2.1.172–v2.1.216 預設巢狀，最多 5
  層且無法更改；v2.1.217 將巢狀改為需選擇加入、深度 1；
  v2.1.219 將預設值設為 3），讓下一次反轉時文件也能平順過渡。
- **Opus 4.8 被稱為預設的 Opus 模型** — `10-cli/README.md` 寫著
  「Opus 4.8 remains the default on Max, Team Premium, Enterprise pay-as-you-go,
  and the Claude API」。自 v2.1.219 起，預設模型是 **Claude Opus 5**
  （`claude-opus-5`，1M context，預設 effort 為 `high`），而它在 repo 的
  所有模型表中都不存在。已加入 `10-cli` 與
  `claude_concepts_guide.md` 的模型表、全部 18 個 `Compatible Models`
  頁尾，以及約 20 處 effort 層級列舉。Microsoft Foundry 仍將
  `opus` 別名解析為 Opus 4.6。
- **Fast mode 模型清單** — `/fast` 現在適用於 **Opus 5 與 Opus 4.8**；
  Opus 4.7 已移除（v2.1.219）。同時更正了 `10-cli/README.md` 中以現在式描述
  在 Opus 4.6 上啟用 fast mode 的說明。
- **Auto mode 適用資格自相矛盾** — `09-advanced-features/README.md`
  同時寫著「available on all plans」與「Team, Enterprise, or API」/
  「Anthropic API only」。已依照官方記錄的四項要求改寫
  （Plan / Organization / Model / Provider）。Auto mode 在**所有
  方案**皆可使用，Team/Enterprise 預設開啟，管理員可透過
  `permissions.disableAutoMode` 選擇*退出* — 該檔案先前描述的是由 Owner
  選擇*加入*。
- **過時的 auto mode 選擇加入說明** — `CATALOG.md` 仍告訴使用者要在
  第三方供應商上設定 `CLAUDE_CODE_ENABLE_AUTO_MODE=1`。這項要求
  已在 v2.1.207 移除；此變數為了相容性仍被接受，但已
  沒有作用。
- **Hook 事件數量不一致** — `LEARNING-ROADMAP.md` 與
  `claude_concepts_guide.md` 寫 29，而 `06-hooks/README.md` 寫 30（現為
  31）。兩者皆已對齊為 31。`claude_concepts_guide.md` 的具名事件表
  也缺少 `MessageDisplay`（v2.1.152 起就存在的缺口）以及
  新的 `DirectoryAdded`；其事件清單現在與 `06-hooks` 完全相同。
- **`doc-generator` 技能名稱不符** — `03-skills/doc-generator/SKILL.md`
  宣告了 `name: api-documentation-generator`，卻位於 `doc-generator/` 中，
  與每一個目錄與索引條目都矛盾。已在英文版中重新命名，並
  同步到 `ja/`、`zh/`、`uk/`。
- **過時的「New Features (May 2026)」標題** — 已在 `CATALOG.md` 與
  `resources.md` 中改為不含日期、不會再過時的標題，並同步更新
  `CATALOG.md` 的導覽錨點。
- **舊版 `docs.anthropic.com` 連結** — `01-slash-commands/README.md`、
  `05-mcp/README.md` 與 `10-cli/README.md` 中剩下的三個頁尾連結已
  遷移到 `code.claude.com`。
- **技能範例遮蔽了內建技能** — `03-skills/README.md` 使用
  `name: deep-research` 作為自訂技能範例，而它現在會遮蔽
  內建的 `/deep-research`。已重新命名為 `topic-research`，並同步到
  `vi/`、`ja/`、`uk/`。

### 新增

- **`DirectoryAdded` hook（v2.1.219）** — 在 `/add-dir` 或 SDK 的
  `register_repo_root` 控制請求於工作階段中途註冊新的工作目錄後觸發。
  已加入 `06-hooks/README.md` 的事件表（數量 30 → 31）。
- **`/deep-research` 僅限明確呼叫（v2.1.218）** — Claude 不再
  自行啟動它。已加入內建技能表。
- **`/code-review` 以背景子代理執行（v2.1.218）** — 審查工作不再
  塞滿對話，且堆疊的斜線命令仍是它的審查
  目標。記錄於 `03-skills/README.md` 與 `CATALOG.md`。
- **Fork 技能預設在背景執行（v2.1.218）** — 設定
  `context: fork` 的技能預設為 `background: true`；設為 `false` 即可退出。新增了
  `background` frontmatter 列、對應的 YAML 範例行，以及對
  預設值的明確說明，避免讀起來與 `04-subagents` 相矛盾 —
  在後者中 `background: true` 是*強制*背景執行。
- **Agent frontmatter hooks 需要工作區信任（v2.1.218）** — 專案子代理中的
  hooks 只有在 agent 檔案所在的資料夾已
  接受工作區信任後才會執行。
- **動態 workflow 規模準則（v2.1.219）** — workflow 現在預設採用
  中等準則（目標少於 15 個 agent），可透過 `/config` 中的 **Dynamic
  workflow size** 或新的 `workflowSizeGuideline` 設定鍵選擇。
- **`sandbox.network.strictAllowlist`（v2.1.219）** — 對沙箱中的命令直接拒絕
  不在允許清單中的主機，不再詢問。
- **MCP 錯誤顯示（v2.1.219）** — `claude mcp list` 與 `/mcp` 現在會在
  連線失敗時回報 HTTP 狀態與錯誤文字；設定值中隱藏的前後空白
  會發出警告；headless 執行會在 stream-json init 事件中提供
  `mcp_server_errors`。已作為疑難排解指引加入 `05-mcp/README.md`。
- **stream-json 中的巢狀子代理轉發（v2.1.219）** — 設定 `--forward-subagent-text`
  時，深度 2 以上的子代理現在也會出現，並以生成它們的
  `Agent` `tool_use` id 作為鍵。
- **Frontmatter 布林值接受更多值（v2.1.218）** — 除了 `true`/`false` 之外，
  也接受 `yes`/`no`、`on`/`off`、`1`/`0`，不分大小寫。
- **Agent 名稱不接受 `:`（v2.1.218）** — 保留給 plugin 命名空間使用。
- **Auto 與 plan mode 更多交由分類器判斷（v2.1.218）** — `rm -rf /`
  與 `rm -rf ~`（包括在命令替換/行程替換內）現在由
  分類器裁決，而不是開啟權限對話框；搭配 auto 的
  plan mode 也不再對靜態分析器
  無法證明為唯讀的 Bash 命令發出詢問（`useAutoModeDuringPlan`，預設開啟）。
- **Opus 5 安全分類器後備機制（v2.1.219）** — 被標記為資安相關的
  請求會改在 Opus 4.8 上重新執行；被標記為生物相關的請求則以拒絕結束，因為
  Opus 5 執行自己的生物分類器，沒有後備機制。這與
  使用本 repo 的 security-review 子代理與 plugin 範本的讀者相關，因為
  滲透測試/CTF 工作負載經常觸發它。

## [v2.1.217] — 2026-07-22

### 已同步至 Claude Code v2.1.217

將教學涵蓋範圍從 v2.1.212 基準（2026-07-18 同步）提升到
v2.1.217（上游跳過了 v2.1.213 — 更新日誌與 GitHub releases
都從 2.1.212 直接跳到 2.1.214），另外還進行了 repo 內部的準確性稽核。

### 修正

- **巢狀子代理生成的說法反轉（v2.1.217）** — `04-subagents/README.md`
  與 `CATALOG.md` 將「subagents can spawn their
  own subagents, nested up to 5 levels deep」（在 v2.1.172–v2.1.216 為真）陳述為目前行為。自
  v2.1.217 起此功能預設關閉；需透過
  `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH` 選擇加入。兩個檔案現在都先說明目前
  行為，並保留歷史說明。
- **過時的 `--enable-auto-mode` 旗標** — 已在 v2.1.111 移除（auto mode 移入
  預設的 `Shift+Tab` 循環；`--permission-mode auto` 是目前
  以該模式啟動的方式），但 `10-cli/README.md` 仍有四處把它當作有效
  旗標，`QUICK_REFERENCE.md` 也有三處使用它。七處全部
  更新為 `--permission-mode auto`。
- **匯入深度矛盾（「5 levels」vs「4 hops」）** — 在 2026-07-18 同步修正了
  另外兩處後，`02-memory/README.md` 的 Best Practices 區域仍殘留兩處「5 levels」；
  兩者現在都改為「4 hops」，與檔案其餘部分
  一致。
- **缺少 Sonnet 5 的模型表** — `claude_concepts_guide.md` 的「Models &
  Reasoning Effort」表遺漏了 Claude Sonnet 5，儘管該檔案自己的
  頁尾早已將其列為相容模型。已新增為
  Pro/Team Standard/Enterprise 的預設列。
- **`config-examples.json` 虛構的結構** — 全部 11 個範例設定都使用了
  捏造的欄位（`mode: "unrestricted"/"confirm"`、`planning.*`、
  `extendedThinking.*`、`headless.*`、`checkpoints.autoCheckpoint`），這些欄位
  並不存在於真正的設定結構中，另外還有過時的 `claude-opus-4-7` 模型 ID。
  已改寫為使用真正的 `settings.json` 鍵（`permissions.defaultMode`、
  `permissions.allow`/`deny`、`env`、`hooks`、`fileCheckpointingEnabled`、
  `sandbox.*`）以及目前的模型 ID。
- **`brand-voice` 技能與其 README 範例不符** — 實際的
  `03-skills/brand-voice/SKILL.md`（`name: brand-voice-consistency`，
  使用者可呼叫）與 `03-skills/README.md` 的「Example 4」逐步說明
  （`name: brand-voice`、`user-invocable: false`）相矛盾。已將技能檔案對齊
  README 的背景知識定位。
- **CLAUDE.md 長度指引衝突** — `03-skills/claude-md/SKILL.md`
  （「<300 lines, ideally <100」）vs. `02-memory/README.md`（「<500 lines」）。
  兩個檔案統一為「保持在幾百行以內；越短越好」
  （官方並無具體上限 — 先前的兩個數字都是
  編輯自訂的）。

### 新增

- **新的子代理上限（v2.1.217）** — 同時執行的子代理數量上限
  （預設 20，`CLAUDE_CODE_MAX_CONCURRENT_SUBAGENTS`），與上述的巢狀
  深度控制並列。記錄於 `04-subagents/README.md`、
  `10-cli/README.md` 與 `QUICK_REFERENCE.md`。
- **`--max-budget-usd` 現在會停止背景子代理（v2.1.217）** — 這個花費
  上限先前只會阻擋新的生成；現在也會停止已在執行的
  背景子代理。記錄於 `10-cli/README.md`。
- **Hook `if:` glob 範圍縮小（v2.1.214）** — 單段式的 `dir/**` hook
  `if:` 條件現在只會比對 `<cwd>/dir`（任意深度請用 `**/dir/**`）；
  deny/ask 權限規則不受影響。記錄於 `06-hooks/README.md`。
- **SessionStart 的 `"fork"` 來源（v2.1.214）** — 分岔的工作階段現在會回報
  來源 `"fork"`，而不是 `"resume"`。記錄於 `06-hooks/README.md`。
- **`sandbox.filesystem.disabled`（v2.1.216）** — 略過檔案系統隔離，
  同時仍強制執行網路出口控制。記錄於
  `09-advanced-features/README.md` 與 `10-cli/README.md`。
- **`/rewind` 符號連結/硬連結保護（v2.1.216）** — 不再透過
  追蹤路徑上的符號連結或硬連結還原或刪除檔案；會回報
  略過了多少個路徑。記錄於 `08-checkpoints/README.md`。
- **自動記憶 `modified` 時間戳記 + 非阻擋式 `/memory` 編輯器** — 記憶檔案
  frontmatter 上的 ISO `modified` 時間戳記（v2.1.214），以及 `/memory`
  在其 GUI 編輯器開啟時不再阻擋工作階段（v2.1.216）。
  記錄於 `02-memory/README.md`。
- **CLI/設定批次更新（v2.1.214–v2.1.217）** — `emojiCompletionEnabled`、
  `FORCE_HYPERLINK=0`、`CLAUDE_CODE_OTEL_CONTENT_MAX_LENGTH`、`--settings`
  檔案上限 2 MiB，以及權限強化（Docker/Podman
  daemon 重新導向旗標、`file -m`/`--magic-file`/`-f`/`--files-from`、
  10,000 字元以上的命令）記錄於 `10-cli/README.md`。`/context` 的
  超出 context window 警告，以及 `/compact` 失敗現在會顯示為
  錯誤（v2.1.216），記錄於 `01-slash-commands/README.md`。
- **`/verify` 與 `/code-review` 僅限明確呼叫（v2.1.215）** —
  在 `03-skills/README.md` 與 `QUICK_REFERENCE.md` 中補充說明；repo
  從未聲稱它們會自動呼叫。

### 備註

- `QUICK_REFERENCE.md` 中 `--enable-auto-mode` / `--permission-mode auto`
  不一致的問題，在對照線上 CLI 參考文件確認前者已在 v2.1.111 移除後，
  以後者為準解決。
- 已針對上述 P0/P1 說法檢查翻譯鏡像（`vi`/`ja`/`zh`/`uk`）；
  大多數翻譯版的 `04-subagents/README.md` 與 `CATALOG.md`
  不是早於巢狀生成功能，就是沒有受影響的
  章節，因此那裡不需要同步編輯。`10-cli/README.md` 的
  `--max-budget-usd` 說明已同步到全部四種語言。

## [v2.1.212] — 2026-07-18

### 已同步至 Claude Code v2.1.212

將教學涵蓋範圍從 v2.1.206 基準（2026-07-11 同步）提升到
v2.1.212，另外還進行了 repo 內部的準確性稽核，找出了
與版本差異無關的缺陷。

### 修正

- **移除已失效的 `#` 記憶捷徑** — `02-memory/README.md` 在兩處
  （「Method 3」與「Example 4」逐步說明）把 `#` 前綴快速新增記憶的方式
  當作可用功能記錄，直接與
  檔案開頭三行處自己的命令表相矛盾，該表早已將
  `#` 標記為 **Discontinued**。兩個章節都已移除/改寫，改為指向
  `/memory` 與在對話中提出記憶請求。
- **Bedrock/Vertex/Foundry 上的 auto mode 從選擇加入改為選擇退出（v2.1.207）** — auto
  mode 現在在這些供應商（以及已登入的 Claude
  apps gateway 工作階段）上預設可用，適用於 Sonnet 5、Opus 4.7/4.8 與 Fable 5；
  `CLAUDE_CODE_ENABLE_AUTO_MODE` 選擇加入旗標自 v2.1.207 起
  不再有作用。已在 `09-advanced-features/README.md` 與 `10-cli/README.md` 中修正。
- **`auto` 權限模式被誤標為「Research Preview」** — 目前的官方
  文件將 `auto` 呈現為正式版（GA，所有方案皆可用，僅受模型與
  供應商資格限制），而非預覽功能。已在
  `09-advanced-features/README.md`、`CATALOG.md` 與 `README.md` 中更正，後者
  也在其權限模式摘要中補上缺少的 `auto` 列。
- **更正 `/fork` / `/branch` 的歷史** — repo 聲稱「`/fork`
  renamed to `/branch` in v2.1.77 (alias retained)」。目前的文件顯示它們是
  兩個不同且都仍存在的命令：`/fork` 會生成一個
  繼承對話的背景子代理，`/branch` 則是就地切換到一份副本中。
  它們只有在 v2.1.77 到 v2.1.161 期間是同一個具別名的命令。已在
  `01-slash-commands/README.md` 與 `09-advanced-features/README.md` 中修正。
- **補齊 `effort` frontmatter 列舉值** — `04-subagents/README.md` 只列出
  4 個層級（`low`/`medium`/`high`/`max`），缺少 `xhigh`，而其他
  記錄此列舉的檔案都有它。
- **統一內建技能數量** — `CATALOG.md` 的摘要表寫著「9
  bundled」，但其自己的詳細表有 10 列；已更正為 10（共 16 個）。
- **`INDEX.md` 功能涵蓋矩陣全面重新計算** — 標題聲稱「16
  files」，而 Skills 列對同一類別寫著「28」；兩者都不符合
  六個已記錄技能實際的 21 個檔案。Plugins 列
  同樣加總為 36 卻顯示 40。每一列都已手動重新清點；
  Skills 列現在是 `5 | 9 | 7`（**21**），Plugins 列現在是
  `11 | 9 | 3 | 3 | 3 | 3 | 7`（**39**）— 每一列的 Total 現在都等於
  該列各欄的加總。
- **更新過時的 `2.1.160` 頁尾群組** — 五個檔案
  （`08-checkpoints/checkpoint-examples.md`、
  `09-advanced-features/planning-mode-examples.md`，以及三個
  `07-plugins/*/README.md` 範例套件檔案）仍帶有 2026 年 6 月的
  頁尾，且未發現版本相關的內容漂移；已更新為 2.1.212。
- **`QUICK_REFERENCE.md` 全面重新同步** — 原本落後 52 個版本，停在
  `2.1.160`/6 月 2 日。權限模式區塊更新為 `manual`
  （原為 `default`），Compatible Models 加入 Sonnet 5，而「New
  Features (May 2026)」章節也重新命名並更新內容。
- **改寫 `02-memory/README.md` 的 Memory Hierarchy 章節** — 以經過驗證的結構
  取代捏造的 8 層嚴格優先順序模型：
  CLAUDE.md 檔案與規則會被串接進 context（而非以
  覆寫方式擇一），而 `managed-settings.d/` 是 `settings.json` 的機制，不是
  CLAUDE.md 的機制。Memory Architecture 圖也已更正 — 它
  先前把 claude.ai 的 24 小時綜整週期與 Claude Code 的
  持續性自動記憶混為一談。

### 新增

- **子代理輸出掃描（v2.1.210）** — Claude Code 會掃描子代理
  報告中的提示注入模式（偽造的系統標籤、捏造的
  對話回合、提及繞過權限）並加以中和。
  記錄於 `04-subagents/README.md` 的新小節中。
- **工作階段層級的生成上限（v2.1.212）** — 預設每個工作階段 200 次的上限，適用於
  WebSearch 呼叫（`CLAUDE_CODE_MAX_WEB_SEARCHES_PER_SESSION`）與子代理
  生成（`CLAUDE_CODE_MAX_SUBAGENTS_PER_SESSION`，由 `/clear` 重設）。
  記錄於 `09-advanced-features/README.md`、`04-subagents/README.md`
  與 `10-cli/README.md`。
- **MCP 長時間執行工具自動轉入背景（v2.1.212）** — 超過 2 分鐘的 MCP 工具呼叫
  現在會自動轉入背景，而不是阻擋工作階段；
  可透過 `CLAUDE_CODE_MCP_AUTO_BACKGROUND_MS` 設定。記錄於
  `05-mcp/README.md`。
- **`claude auto-mode reset` 與 `/resume` 選擇器（v2.1.212）** — 新的 CLI
  子命令可還原預設的 auto-mode 設定；不帶參數的 `/resume` 現在會開啟
  過去工作階段（包括已移除者）的選擇器，並以
  背景工作階段的方式恢復。記錄於 `09-advanced-features/README.md`、
  `01-slash-commands/README.md` 與 `10-cli/README.md`。
- **螢幕閱讀器模式（v2.1.208）** — 可選擇啟用的純文字渲染，透過
  `--ax-screen-reader`、`CLAUDE_AX_SCREEN_READER=1` 或 `"axScreenReader":
  true` 開啟。記錄於 `09-advanced-features/README.md` 與
  `10-cli/README.md`。
- **Task 工具 `mode` 參數棄用（v2.1.212）** — 於
  `04-subagents/README.md` 中註明：子代理現在預設繼承父工作階段的
  權限模式；Task 工具每次呼叫的 `mode` 參數會被
  忽略。

### 已知缺口（延後處理，本次同步未修正）

- `03-skills/.claude/skills/blog-draft/` 是被 gitignore 的本機測試暫存區
  （`03-skills/.gitignore` 中的 `# Local skill testing`），而不是
  `03-skills/blog-draft/` 的受追蹤副本 — 已確認不需處理。

## [v2.1.160] — 2026-06-02

### 已同步至 Claude Code v2.1.160

將教學涵蓋範圍提升到 Claude Code v2.1.160 版本。中間的
v2.1.156 同步（Claude Opus 4.8，#129）已套用到文件中，但沒有另外
寫入更新日誌；本條目從那裡接續，涵蓋 v2.1.157–v2.1.160 的
差異。此範圍內沒有破壞性變更 — 新增的只是少數
新的 CLI/功能介面，加上例行的頁尾更新。第三方供應商上的
auto mode 是**選擇加入**，而非新的預設值。

### 新增

- **`claude plugin init <name>`（v2.1.157）** — 直接在
  `.claude/skills` 中建立新 plugin 的骨架；放在那裡的 plugin 現在會自動載入，
  不需要 marketplace。記錄於 `10-cli/README.md`、`07-plugins/README.md` 與
  `CATALOG.md`。
- **Bedrock / Vertex / Foundry 上的 auto mode（v2.1.158）** — auto mode 現在
  可在這三個第三方供應商上用於 Opus 4.7/4.8，需透過
  `CLAUDE_CODE_ENABLE_AUTO_MODE=1` 環境變數**選擇加入**。記錄於
  `09-advanced-features/README.md`、`10-cli/README.md` 與 `CATALOG.md`。
- **`EnterWorktree` 工作階段中途切換（v2.1.157）** — `EnterWorktree`
  工具現在可以在工作階段內切換 Claude 管理的 worktree，且
  完成的 worktree 會保持未鎖定狀態，讓 `git worktree remove`/`prune` 可以
  清理它們。記錄於 `09-advanced-features/README.md`。

### 行為變更

- **`acceptEdits` 寫入安全詢問（v2.1.160）** — 即使在 `acceptEdits`
  模式下，Claude Code 現在也會在寫入 shell 啟動檔（`.zshenv`、
  `.zlogin`、`.bash_login`、`~/.config/git/`）與會執行程式碼的建置設定
  （`.npmrc`、`.yarnrc*`、`bunfig.toml`、`.bazelrc`、`.pre-commit-config.yaml`、
  `.devcontainer/`）之前詢問，因為這些檔案可能導致非預期的命令
  執行。記錄於 `09-advanced-features/README.md`。
- **動態 workflow 觸發關鍵字 `workflow` → `ultracode`（v2.1.160）** —
  單獨的「workflow」一詞不再觸發動態 workflow 執行；觸發
  關鍵字現在是 `ultracode`。於 `09-advanced-features/README.md` 中註明。

### 移除

- **`CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE` 現在沒有作用（v2.1.160）** —
  此環境變數已移除，現在不會產生任何效果。`10-cli/README.md` 中環境變數表的
  文字已從「removed 2026-06-01」更新為
  「Removed (no-op as of v2.1.160)」。

### 文件

- 修正 `README.md` 中三個內部不一致的版本字串（徽章與
  FAQ 內文停留在 `2.1.145` / `v2.1.150`），並統一一個過時的 Sources
  連結。
- 將每份英文文件的 metadata 頁尾更新為 **v2.1.160 / June 2, 2026**，
  以保持同步一致。

## [v2.1.150] — 2026-05-25

### 已同步至 Claude Code v2.1.150

將教學涵蓋範圍從 Claude Code v2.1.145 → v2.1.150（2026 年 5 月 23 日
版本）。自上次同步以來，Anthropic 發布了五個修補版本（v2.1.146 到 v2.1.150）。
主要變更是**內建的 `/simplify`
技能重新命名為 `/code-review`**（v2.1.146）— 純粹的重新命名，**沒有別名**，因此
舊名稱已無法使用。由於本 repo 也附帶自己的本機
code-review 技能，該目錄已重新命名為 `code-review-specialist`，以
避免遮蔽新的內建技能。其他重點：`/usage` 現在會依類別
細分花費、背景工作階段可以用 `Ctrl+T` 釘選、
markdown 渲染器支援 GFM 任務清單核取方塊，並新增了
`allowAllClaudeAiMcps` 管理設定。本次同步也補上了
四個仍停留在 v2.1.143 的模組 README（`04-subagents`、`05-mcp`、`07-plugins`、
`09-advanced-features`）。

### 行為變更

- **`/simplify` 重新命名為 `/code-review`（v2.1.146）**：內建的審查
  技能現在以 `/code-review` 呼叫，並可接受選用的 effort 層級
  （例如 `/code-review high`）；加上 `--comment` 可將發現以行內
  GitHub PR 評論的形式發布（v2.1.147）。舊的 `/simplify` 名稱已無法使用 —
  沒有別名。已在 `01-slash-commands/README.md`、
  `03-skills/README.md`、`CATALOG.md`、`QUICK_REFERENCE.md` 與
  `claude_concepts_guide.md` 中更新。

### 變更

- **將 repo 的本機 `code-review` 技能重新命名為 `code-review-specialist`**，
  以避免與新的內建 `/code-review` 衝突。目錄
  `03-skills/code-review/` → `03-skills/code-review-specialist/`，且
  `README.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`CATALOG.md`、
  `LEARNING-ROADMAP.md`、`claude_concepts_guide.md` 與 `03-skills/README.md` 中的
  所有安裝命令、目錄樹與交叉參照都已更新。
  新增說明解釋此衝突以及如何避免遮蔽
  內建技能。

### 新增

- **`/usage` 依類別細分花費（v2.1.149）** — 花費檢視現在會
  依類別（技能、子代理、plugin，以及
  各 MCP 伺服器的花費）細分支出。記錄於 `CATALOG.md` 與
  `claude_concepts_guide.md`。
- **釘選背景工作階段 — `Ctrl+T`（v2.1.147）** — 在
  `claude agents` 中釘選工作階段可讓它在閒置時保持運作、就地重新啟動以套用
  Claude Code 更新，並且在記憶體壓力下只會在
  未釘選的工作階段之後才被釋放。記錄於 `10-cli/README.md`。
- **GFM 任務清單核取方塊渲染（v2.1.149）** — markdown 渲染器現在會
  將 `- [ ]` / `- [x]` 渲染為核取方塊。記錄於
  `09-advanced-features/README.md`。
- **`allowAllClaudeAiMcps` 管理設定（v2.1.149）** — 允許在整個組織中載入
  claude.ai 雲端 MCP connectors。記錄於
  `05-mcp/README.md`。

### 移除

- **Stop/SubagentStop hook 輸入欄位 `background_tasks` 與 `session_crons`**
  — 已從 `06-hooks/README.md` 與 `resources.md` 中移除。這些欄位是根據
  v2.1.145 版本說明加入的，但並未列在官方 hooks
  參考頁面中；為了讓文件與已發布的參考文件保持一致而移除。

### 文件

- 將四個模組 README 從 v2.1.143 補上到 v2.1.150：
  `04-subagents/README.md`、`05-mcp/README.md`、`07-plugins/README.md`、
  `09-advanced-features/README.md`。
- 將每份英文文件的 metadata 頁尾更新為 **v2.1.150 / May 25, 2026**，
  以保持同步一致。

## [v2.1.145] — 2026-05-20

### 已同步至 Claude Code v2.1.145

將教學涵蓋範圍從 Claude Code v2.1.143 → v2.1.145（2026 年 5 月 19 日
版本）。自上次同步以來，Anthropic 發布了兩個修補版本（v2.1.144 與 v2.1.145）。
重點：`/extra-usage` 重新命名為 `/usage-credits`、`/model`
預設只套用於當前工作階段、三個新的內建技能（`/run`、`/verify`、
`/run-skill-generator`）、Stop/SubagentStop hook 輸入欄位
`background_tasks` 與 `session_crons`、用於腳本的 `claude agents --json`，
以及一項修補單獨環境變數 Bash 自動核准漏洞的安全性修正。
本次同步也補上了六份根目錄參考文件
（`LEARNING-ROADMAP.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`resources.md`、
`claude_concepts_guide.md`、`STYLE_GUIDE.md`），這些文件在
v2.1.143 同步中被遺漏，仍停留在 v2.1.138。

### 新增

- `/usage-credits` 斜線命令（v2.1.144）— 取代 `/extra-usage` 成為
  主要名稱；`/extra-usage` 仍可作為別名使用。記錄於
  `01-slash-commands/README.md` 與 `CATALOG.md`。
- 三個新的內建技能（v2.1.145）— `/run`（啟動專案的應用程式以
  查看變更的執行情形）、`/verify`（建置、執行並觀察應用程式以
  確認修正有效）、`/run-skill-generator`（透過產生每個專案專屬的技能，教導 `/run`/`/verify`
  如何處理特定專案）。記錄
  於 `03-skills/README.md`、`CATALOG.md` 與 `QUICK_REFERENCE.md`。使
  標準內建技能數量達到 **9** 個。
- Stop/SubagentStop hook 輸入欄位 `background_tasks` 與 `session_crons`
  （v2.1.145）— hook 作者可以讀取這些欄位，以決定在背景工作或排程任務
  仍在等待時是否要阻擋停止。記錄於
  `06-hooks/README.md`。
- `claude agents --json`（v2.1.145）— 以機器可讀的
  JSON 輸出 agent 清單，供腳本使用（狀態列、工作階段選擇器、tmux-resurrect）。
  記錄於 `10-cli/README.md`。
- 摘要表中缺少的五個 hook 事件列 — `Setup`、
  `UserPromptExpansion`、`PermissionDenied`、`PostToolBatch`（敘述中
  已宣稱「29 events」；但 `CATALOG.md`、
  `claude_concepts_guide.md` 與 `INDEX.md` 的摘要表只列出 25 個）。

### 行為變更

- **`/model` 預設只套用於當前工作階段（v2.1.144）**：選擇模型現在
  只會套用到目前的工作階段；選擇後按 `d` 可將該
  選擇設為未來工作階段的新預設值。記錄於
  `01-slash-commands/README.md`。
- **封閉 Bash 單獨環境變數自動核准漏洞（v2.1.145 安全性修正）**：當允許清單上
  只有 `FOO=bar` 時，`FOO=bar somecommand` 形式的命令不再被
  自動核准。請透過涵蓋完整命令的
  `Bash(...)` 權限規則明確重新允許這類命令。記錄於
  `06-hooks/README.md`。
- **`context: fork` 無限迴圈修正（v2.1.145）**：使用
  `context: fork` 的技能先前在少數情況下可能觸發無限重複呼叫的迴圈。
  已在 `03-skills/README.md` 中以註記說明。

### 文件

- 將六份根目錄參考文件從 v2.1.138 補上到 v2.1.145：
  `LEARNING-ROADMAP.md`、`QUICK_REFERENCE.md`、`INDEX.md`、`resources.md`、
  `claude_concepts_guide.md`、`STYLE_GUIDE.md`。
- 修正內建技能不一致的問題 — `CATALOG.md`、`QUICK_REFERENCE.md` 與
  `03-skills/README.md` 先前列出三份不同的 5 項清單；
  已統一為標準的 9 個（`/batch`、`/claude-api`、`/debug`、
  `/fewer-permission-prompts`、`/loop`、`/run`、`/run-skill-generator`、
  `/simplify`、`/verify`）。`QUICK_REFERENCE.md` 的儲存格也曾錯誤地
  將 `/voice` 與 `/browse` 列為內建技能 — 兩者都不是內建的。
- 在 `QUICK_REFERENCE.md` 與 `resources.md` 中將「New Features (March 2026)」→「New Features (May 2026)」，
  以與 repo 其餘部分一致。
- 將 `README.md` 中的版本徽章從 `2.1.138` 更新為 `2.1.145`，並
  更新內文中兩處「latest: v2.1.138」的說法。
- 將 STYLE_GUIDE 的範例 metadata 頁尾從 `2.1.97` 更新為 `2.1.145`，
  讓貢獻者複製的是目前的版本。

## [v2.1.143] — 2026-05-19

### 已同步至 Claude Code v2.1.143

將教學涵蓋範圍從 Claude Code v2.1.138 → v2.1.143（2026 年 5 月 15 日
版本）。自上次同步以來，Anthropic 發布了五個修補版本（v2.1.139–v2.1.143）。
重點：`/goal` 與 `/scroll-speed` 斜線命令、`claude
agents` Agent View（研究預覽版）及其完整的派送旗標、Stop
hook 安全上限、hook exec 形式（`args`）、PostToolUse 的 `continueOnBlock`、
hook `terminalSequence` 輸出、Fast Mode 預設改為 Opus 4.7、
Windows 上的 Bedrock/Vertex/Foundry 預設使用 PowerShell，以及
`worktree.bgIsolation` 設定。

### 新增

- `/goal <statement>` 斜線命令（v2.1.139）— 註冊一個工作階段層級的
  完成條件，並提供即時覆蓋面板，顯示經過時間、回合
  數與 token 用量。記錄於 `01-slash-commands/README.md`，並
  從 `10-cli/README.md` 交叉連結。
- `/scroll-speed <±N>` 斜線命令（v2.1.139）— 調整 TUI 即時預覽的
  捲動速度；依機器保存設定。記錄於
  `01-slash-commands/README.md`。
- `claude agents` Agent View（研究預覽版，v2.1.139），派送旗標包括
  `--cwd`（v2.1.141）、`--add-dir`、`--settings`、`--mcp-config`、
  `--plugin-dir`、`--permission-mode`、`--model`、`--effort`、
  `--dangerously-skip-permissions`（v2.1.142）。記錄於
  `10-cli/README.md`。
- `claude plugin details <name>`（v2.1.139）— 完整的 plugin 清單，加上
  預估的每回合/每次呼叫 token 成本。v2.1.142 在詳細資訊窗格中加入了
  LSP 伺服器。記錄於 `07-plugins/README.md`。
- `/plugin` 瀏覽窗格中的 Marketplace context 成本預估（v2.1.143）。
  記錄於 `07-plugins/README.md`。
- Hook **exec 形式**（`args: string[]`，v2.1.139）— 直接以 `execve()` 生成，
  不經 shell 解析；與 shell 形式的 `command`
  欄位互斥。記錄於 `06-hooks/README.md`。
- PostToolUse 上的 Hook `continueOnBlock: true` 欄位（v2.1.139）— 將
  被阻擋的工具結果以 `tool_result` 回傳給 Claude，而不是中止
  該回合。記錄於 `06-hooks/README.md`。
- Hook `terminalSequence` JSON 輸出欄位（v2.1.141）— 輸出原始的 OSC 跳脫
  序列，用於桌面通知、視窗標題與提示音。記錄
  於 `06-hooks/README.md`。
- `worktree.bgIsolation: "none"` 設定（v2.1.143）— 背景工作階段
  直接編輯目前的工作副本，而不是隔離的 worktree。
  記錄於 `09-advanced-features/README.md`。
- `CLAUDE_PROJECT_DIR` 現在會傳入每個 MCP stdio 伺服器的環境
  （v2.1.139），且 plugin 與專案 `.mcp.json` 的 `command`/`args`/`env` 欄位
  支援 `${CLAUDE_PROJECT_DIR}` 替換。記錄於
  `05-mcp/README.md`。
- 子代理 OTEL 標頭 `x-claude-code-agent-id` 與
  `x-claude-code-parent-agent-id`（v2.1.139），在
  `claude_code.llm_request` OTEL span 上以 `agent_id` / `parent_agent_id` 屬性提供。
  記錄於 `04-subagents/README.md`。
- `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`（v2.1.142）— 在 v2.1.142 預設改為 Opus 4.7 之後，
  將 Fast Mode 固定回 Opus 4.6。記錄於
  `10-cli/README.md`。
- `CLAUDE_CODE_USE_POWERSHELL_TOOL=0` 與
  `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`（v2.1.143）— 退出
  預設開啟的 PowerShell 工具，或讓它遵循系統執行
  原則，而不是使用 `-ExecutionPolicy Bypass`。記錄於
  `09-advanced-features/README.md`。
- `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`（v2.1.143）— 覆寫 Stop hooks 的連續 8 次
  阻擋安全上限（設為 `0` 可停用）。記錄於
  `06-hooks/README.md` 與 `09-advanced-features/README.md`。
- `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1`（v2.1.141）— 強制 plugin 安裝時
  透過 HTTPS clone GitHub plugin 來源，適用於沒有 SSH 金鑰的 CI runner。
  記錄於 `07-plugins/README.md`。
- `ANTHROPIC_WORKSPACE_ID`（v2.1.141）— 將聯合工作負載身分
  token 限定於特定工作區。記錄於 `09-advanced-features/README.md`。
- 根目錄層級 `SKILL.md` 的 plugin 模式（v2.1.142）— 只有
  最上層 `SKILL.md`（沒有 `skills/` 子目錄）的 plugin 會被呈現為單一
  技能。記錄於 `07-plugins/README.md`。
- `/schedule` 的 Plugins 行銷名稱 **Routines**（Anthropic 部落格，
  2026-05-14）— 在 `09-advanced-features/README.md` 中以一行註記呈現；
  CLI 介面仍為 `/schedule`。

### 行為變更

- **Fast Mode 預設改為 Opus 4.7（v2.1.142）**：`/fast` 現在預設執行 Opus
  4.7（原為 Opus 4.6）。設定 `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`
  可改回。
- **Windows 上的 Bedrock/Vertex/Foundry 預設啟用 PowerShell 工具
  （v2.1.143）**：Claude Code 以 `-ExecutionPolicy Bypass` 呼叫 PowerShell。
  可透過 `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY=1`（遵循
  系統原則）或 `CLAUDE_CODE_USE_POWERSHELL_TOOL=0`（停用此工具）退出。
- **設定 API 金鑰驗證時，Remote Control、`/schedule`、claude.ai MCP connectors 與通知
  偏好設定會自動停用（v2.1.139）**：設定
  `ANTHROPIC_API_KEY`、`ANTHROPIC_AUTH_TOKEN` 或 `apiKeyHelper` 會停用全部
  四個透過 claude.ai 橋接的介面，即使同時也有有效的 claude.ai 登入。
- **Stop hook 阻擋迴圈上限為連續 8 次（v2.1.143）**：連續 8 次後
  工作階段會以警告結束，防止有問題的 Stop hooks
  讓工作階段永遠循環下去。可透過
  `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` 覆寫。
- **`subagent_type` 比對現在不分大小寫與分隔符號（v2.1.140）**：
  `code-reviewer`、`Code Reviewer` 與 `code_reviewer` 都會解析到
  同一個 agent。記錄於 `04-subagents/README.md`。

### 變更

- 根目錄參考文件（`README.md`、`CATALOG.md`）從 `28 hook
  events` 更新為 `29 hook events` — 在 v2.1.138 加入 `Setup` hook 後，與 `06-hooks/README.md` 及
  `LEARNING-ROADMAP.md` 一致。

### 給譯者的說明

- 教學翻譯（`vi/`、`ja/`、`uk/`、`zh/`）以英文為準；請同步
  本輪在模組 README 與上方 CHANGELOG 中的差異。頁尾
  必須反映 `Last Updated: May 19, 2026` 與 `Claude Code Version: 2.1.143`。

## [v2.1.138] — 2026-05-09

### 已同步至 Claude Code v2.1.138

將教學涵蓋範圍從 Claude Code v2.1.131 → v2.1.138（2026 年 5 月 9 日
版本）。自上次同步以來，Anthropic 在 v2.1.132 與 v2.1.138 之間發布了七個修補版本。

### 新增（英文文件）

- `worktree.baseRef` 設定（v2.1.133）— 控制 `claude --worktree`
  是從 `origin/<default>`（`"fresh"`，預設）還是本機 `HEAD`
  （`"head"`）建立分支。**行為變更**：`"fresh"` 預設值還原了 v2.1.128 的
  行為，因此在 v2.1.128 之後依賴本機 `HEAD` 建立分支的使用者必須
  重新選擇加入。記錄於 `09-advanced-features/README.md`。
- `autoMode.hard_deny` 管理員鍵（v2.1.136）— 分類器規則陣列，
  無論推斷出的使用者意圖為何，都會阻擋某一類操作。用於
  在 auto mode 中絕不能執行的操作（例如 `rm -rf /`、強制推送到
  受保護分支）。與 `soft_deny` 不同，hard-deny 規則不能
  被分類器通融。記錄於 `09-advanced-features/README.md`。
- `parentSettingsBehavior` 管理員鍵（v2.1.133+，管理員層級）— 控制
  SDK 的 `managedSettings` 如何與父行程的設定合併。
  `"first-wins"` 保留現有的優先順序；`"merge"` 會深度合併值。
  記錄於 `09-advanced-features/README.md`。
- `Setup` hook 事件 — 初始環境設定（每個工作階段一次）；用於
  佈建工具或安裝相依套件。使文件記錄的 hook 事件
  總數從 28 個增加到 29 個。記錄於 `06-hooks/README.md`。
- hook 輸入 JSON 中的 `effort.level` 欄位（v2.1.133）— 向 hooks 提供目前的
  effort 層級（`low`/`medium`/`high`/`xhigh`/`max`）。記錄於
  `06-hooks/README.md`。
- Bash 子行程中的 `CLAUDE_CODE_SESSION_ID` 環境變數
  （v2.1.132）— 與 hook 輸入 JSON 中 `session_id` 欄位相符的工作階段 UUID，
  用於將 bash 日誌與 hook 遙測資料相互對應。記錄於
  `06-hooks/README.md`。
- Bash 子行程中的 `CLAUDE_EFFORT` 環境變數（v2.1.133）—
  目前的 effort 層級，與 hook 輸入 JSON 中的 `effort.level` 相符。記錄
  於 `06-hooks/README.md`。
- `sandbox.bwrapPath` 與 `sandbox.socatPath` 設定（v2.1.133+，Linux/WSL）
  — 讓 Claude Code 指向 `bubblewrap` 與
  `socat` 的非標準安裝位置。預設透過 `$PATH` 查找。記錄於
  `09-advanced-features/README.md`。
- `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN` 環境變數（v2.1.132）。
  記錄於 `09-advanced-features/README.md`。
- `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL` 環境變數
  （v2.1.136）— 為擷取 OpenTelemetry 資料的組織重新啟用
  工作階段品質問卷；在 OTEL 部署中預設關閉。
  記錄於 `09-advanced-features/README.md`。

### 變更

- **行為變更**：Plan mode 現在會無條件阻擋所有檔案寫入
  （v2.1.136），即使 `permissions.allow` 中存在相符的 `Edit(...)` 規則也一樣。
  先前寬鬆的 `Edit(...)` 規則可能讓
  寫入在 plan mode 中通過；這個繞過途徑已被封閉。依賴
  舊行為的工作流程必須先退出 plan mode（`Shift+Tab`）再
  進行編輯。記錄於 `09-advanced-features/README.md`。
- 含空格的 plugin 斜線命令（例如 `/myplugin review`）現在會解析為
  `/myplugin:review`。Plugin 的 `skills` 設定項目不再隱藏
  預設的 `skills/` 目錄 — 兩者會合併。記錄於
  `07-plugins/README.md`。
- MCP 伺服器現在在 `/clear` 之後仍會保留（v2.1.132+）。記錄於
  `05-mcp/README.md`。
- 子代理可透過 Skill 工具發現專案、使用者與 plugin 技能
  （v2.1.133）。記錄於 `04-subagents/README.md`。
- 恢復 plan-mode 工作階段時，現在會遵循 `--permission-mode`
  （v2.1.132）。記錄於 `09-advanced-features/README.md`。
- `CronList` 輸出現在包含限定條件以及排程的提示詞
  內容（v2.1.136），讓你不必開啟即可稽核每個 cron 將執行的內容。
  記錄於 `09-advanced-features/README.md`。

### 修正

- OAuth refresh token 同時重新整理的競態條件。
- INDEX.md 數量漂移：Skills 28 → 16、Plugins 40 → 27、Hooks 腳本
  8 → 9（依 markdown 內容樹重新清點）。新的總數採用
  僅計算 `.md` 的方法，將計數範圍限定於教學內容，而不是
  建置產出物與設定檔。
- `CATALOG.md`（v2.1.118 → v2.1.138）與
  `claude_concepts_guide.md`（v2.1.117 → v2.1.138）中過時的來源 URL。移除了概念指南中
  重複的舊版頁尾。

### 給翻譯維護者的說明

`vi/`、`zh/`、`uk/` 與 `ja/` 的在地化目錄由社群維護，
可能落後於英文原文。同步翻譯的貢獻者應將
本版本中更新的英文檔案進行 diff 比對。

## [v2.1.131] — 2026-05-06

### 已同步至 Claude Code v2.1.131

將教學涵蓋範圍從 Claude Code v2.1.126 → v2.1.131（2026 年 5 月 6 日
版本）。自上次同步以來，Anthropic 發布了 v2.1.128、v2.1.129 與 v2.1.131；
v2.1.127 與 v2.1.130 被跳過，從未公開發布。

### 新增（英文文件）

- `--plugin-url <url>` 旗標（v2.1.129）— 從 URL 擷取 plugin `.zip` 封存檔，
  供目前的工作階段使用。可重複指定。記錄於
  `07-plugins/README.md`。
- `CLAUDE_CODE_FORCE_SYNC_OUTPUT` 環境變數（v2.1.129）— 在自動偵測失效的
  終端機（例如 Emacs `eat`）上強制同步輸出。
  記錄於 `10-cli/README.md` 與 `09-advanced-features/README.md`。
- `CLAUDE_CODE_PACKAGE_MANAGER_AUTO_UPDATE` 環境變數（v2.1.129）— 為
  Homebrew/WinGet 安裝（通常不會自動更新）啟用背景升級。記錄於 `10-cli/README.md` 與
  `09-advanced-features/README.md`。
- `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY` 環境變數（v2.1.129）— 選擇加入
  `/v1/models` gateway 探索時必須設定（見「變更」）。記錄於
  `10-cli/README.md`。
- `disableRemoteControl` 設定（v2.1.128）— 管理員可透過 managed/policy 範圍
  封鎖 `claude remote-control` 與 `/remote-control`。
  記錄於 `09-advanced-features/README.md`。
- `--plugin-dir` 接受 `.zip` 封存檔（v2.1.128）— 與目錄
  輸入並存。記錄於 `07-plugins/README.md`。
- `skillOverrides` 接受 `"name-only"` 與 `"user-invocable-only"`
  （v2.1.129）— 除了原本的 `"on"`/`"off"` 之外。記錄於
  `03-skills/README.md`。

### 變更

- **行為變更**：Gateway `/v1/models` 探索現在改為**選擇加入**
  （v2.1.129）。先前（v2.1.126）設定 `ANTHROPIC_BASE_URL` 會自動
  從 gateway 的 `/v1/models` 端點填入 `/model`。從 v2.1.129 起，
  使用者必須另外設定 `CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY=1`；
  沒有這個環境變數時，`/model` 會退回內建的靜態清單。
  記錄於 `10-cli/README.md`。
- `/mcp` 會顯示每個伺服器的工具數量，並以視覺方式標示回報 0 個
  工具的伺服器（v2.1.128）。記錄於 `05-mcp/README.md`。
- 不帶參數的 `/color` 會隨機選擇工作階段顏色（v2.1.128）；明確指定的
  `/color <name|hex>` 仍會設定特定顏色。記錄於
  `01-slash-commands/README.md`。
- `--channels` 旗標現在可搭配 API 金鑰（console）驗證使用
  （v2.1.128）。先前的版本需要 Pro/Max OAuth。記錄於
  `09-advanced-features/README.md`。
- Ctrl+R 歷史選擇器預設顯示**所有專案中的所有提示詞**
  （v2.1.129）。在選擇器中按 Ctrl+S 可將範圍縮小到目前的
  專案。記錄於 `09-advanced-features/README.md`。
- `/context` 不再將其 ASCII 視覺化內容輸出到對話中
  （v2.1.129）。視覺化只顯示在 UI 中；每次呼叫不再耗費約 1.6k token。
  記錄於 `09-advanced-features/README.md`。
- 拖放的過大圖片會自動縮小（v2.1.128）— 先前的
  版本會直接拒絕這些圖片。

### 修正

- Windows 上的 VS Code 擴充功能啟用問題（v2.1.131）。
- Mantle 端點驗證（v2.1.131）。
- 1 小時的 prompt-cache TTL 不再被截短為 5 分鐘（v2.1.129）。
- stdin 酬載大於 10 MB 時當機（v2.1.128）。

### 給翻譯維護者的說明

`vi/`、`zh/`、`uk/` 與 `ja/` 的在地化目錄由社群維護，
可能落後於英文原文。同步翻譯的貢獻者應將
本版本中更新的英文檔案進行 diff 比對。

## [v2.1.126] — 2026-05-02

### 已同步至 Claude Code v2.1.126

將教學涵蓋範圍從 Claude Code v2.1.119 → v2.1.126（2026 年 5 月 1 日版本）。
v2.1.120 在首次發布當天（2026-04-24）被撤回，但已於
2026-04-28 成功重新發布，並修正了最初回報的迴歸問題。
v2.1.124 與 v2.1.125 被 Anthropic 跳過，從未發布。

### 新增（英文文件）

- `claude project purge [path]` 子命令（v2.1.126）— 刪除某個專案的所有 Claude Code
  狀態（對話紀錄、任務、除錯日誌、檔案編輯歷史、
  提示詞歷史、`~/.claude.json` 項目）。支援 `--dry-run`、`-y/--yes`、
  `-i/--interactive`、`--all`。記錄於 `10-cli/README.md`。
- `claude plugin prune` 子命令（v2.1.121）— 移除孤立的自動安裝
  plugin 相依套件；`plugin uninstall --prune` 會連帶移除。記錄於
  `07-plugins/README.md`。
- `claude ultrareview [target]` 子命令（v2.1.120）— 從 CI/腳本以
  非互動方式執行 `/ultrareview`，將發現輸出到 stdout，成功/失敗時
  分別以 0/1 結束；支援 `--json` 與 `--timeout <minutes>`。記錄於
  `10-cli/README.md`。
- 技能內容中可使用 `${CLAUDE_EFFORT}` 預留位置（v2.1.120）—
  解析為目前的 effort 層級。記錄於 `03-skills/README.md`。
- `alwaysLoad` MCP 伺服器設定選項（v2.1.121）— 設為 `true` 時，該伺服器的
  所有工具都會略過 tool-search 延遲載入。記錄於 `05-mcp/README.md`。
- `PostToolUse.hookSpecificOutput.updatedToolOutput` 現在適用於所有工具
  （v2.1.121），先前僅限 MCP。記錄於 `06-hooks/README.md`。
- `ANTHROPIC_BEDROCK_SERVICE_TIER` 環境變數（v2.1.122）— 選擇
  Bedrock 服務層級（`default`、`flex`、`priority`）。記錄於
  `10-cli/README.md` 的環境變數表。
- `--dangerously-skip-permissions` 擴大路徑涵蓋範圍（v2.1.121、v2.1.126）
  — 現在寫入 `.claude/skills/`、`.claude/agents/`、
  `.claude/commands/`、`.claude/`、`.git/`、`.vscode/` 與 shell 設定檔時會略過詢問。
  災難性的刪除命令（`rm -rf /` 等）仍會詢問。記錄於
  `09-advanced-features/README.md` 的權限模式章節。
- OAuth code 貼上後備機制（v2.1.126）— 當瀏覽器回呼無法連到
  localhost 時（WSL2、SSH、容器），`claude auth login` 接受貼到終端機中的 OAuth
  code。記錄於 `10-cli/README.md`。
- 可輸入文字篩選的 `/skills` 選單（v2.1.121）。記錄於 `03-skills/README.md`。
- `AI_AGENT` 環境變數（v2.1.120）— 設定在子行程上，讓 `gh` 可以
  將流量歸屬於 Claude Code。記錄於 `10-cli/README.md` 的環境變數
  表。

### 變更

- `--from-pr`（v2.1.119）與 `/resume` 的 PR URL 搜尋（v2.1.122）現在都
  支援 GitHub、GitHub Enterprise、GitLab 與 Bitbucket 的 URL。
- Windows：不再需要 Git for Windows / Git Bash（v2.1.120）— 當沒有 Git Bash 時，Claude
  Code 會使用 PowerShell 作為 shell 工具。從 v2.1.126 起，
  啟用 PowerShell 工具時，PowerShell 就是主要的 shell。偵測範圍
  擴及透過 Microsoft Store、未加入 PATH 的 MSI 或
  `.NET global tool` 安裝的 PowerShell 7。記錄於 `09-advanced-features/README.md` 的平台
  說明。
- 當 `ANTHROPIC_BASE_URL` 指向相容 Anthropic 的 gateway 時，`/model` 選擇器現在會列出
  gateway `/v1/models` 端點提供的模型
  （v2.1.126）。記錄於 `10-cli/README.md`。
- `--dangerously-skip-permissions` 對範圍大幅擴大的允許清單寫入時不再
  詢問（見「新增」）。災難性的刪除仍會詢問。
- 貼上圖片時自動縮小（v2.1.126）— 大於 2000px 的圖片在貼上時會
  縮小；歷史中過大的圖片會自動移除並
  重試請求。（與教學的關聯僅在於安全性/UX 說明。）

### 安全性

- 修正當較高優先順序的 managed-settings 來源缺少 `sandbox` 區塊時，
  `allowManagedDomainsOnly` / `allowManagedReadPathsOnly` 被忽略的問題
  （v2.1.126）。

### 給翻譯維護者的說明

`vi/`、`zh/`、`uk/` 與 `ja/` 的在地化目錄由社群維護，
可能落後於英文原文。同步翻譯的貢獻者應將
本版本中更新的英文檔案進行 diff 比對。

## [v2.4.0] — 2026-04-27

### 已同步至 Claude Code v2.1.119

將教學涵蓋範圍從 Claude Code v2.1.112 → v2.1.119（2026 年 4 月 23 日版本）。
v2.1.120 於 4 月 24 日發布，因迴歸問題在同一天短暫撤回，
並於 4 月 28 日修正後重新發布 — 現在已是正常發布線的一部分。
後續的 v2.1.126（2026 年 5 月 1 日）是下一個穩定目標，涵蓋於上方的
v2.1.126 條目中。

### 新增（英文文件）

- 原生二進位封裝說明（v2.1.113）— CLI 現在針對各平台提供原生二進位檔
- 原生 macOS/Linux 建置上以 `bfs`/`ugrep` 取代 Glob/Grep 的註腳（v2.1.117）
- `mcp_tool` hook 類型及範例（v2.1.118）
- PostToolUse / PostToolUseFailure 輸入上的 `duration_ms` 欄位（v2.1.119）
- `prUrlTemplate` 設定（v2.1.119）以及擴充的 `--from-pr` 供應商清單（GitLab、Bitbucket）
- `cleanupPeriodDays` 擴大範圍（檢查點 + 任務 + shell 快照 + 備份，v2.1.117）
- 在每個生命週期事件上強制執行 plugin marketplace 限制（v2.1.117）以及 `hostPattern`/`pathPattern` 正規表示式（v2.1.119）
- 新的環境變數：`DISABLE_UPDATES`、`CLAUDE_CODE_HIDE_CWD`、`CLAUDE_CODE_FORK_SUBAGENT`、`OTEL_LOG_TOOL_DETAILS`、`ENABLE_TOOL_SEARCH` Vertex 選擇加入
- 新的斜線命令：`/btw`、支援自訂主題的 `/theme`
- `/usage` 標準命令（合併 `/cost` + `/stats`，v2.1.118）
- 分岔子代理（`CLAUDE_CODE_FORK_SUBAGENT=1`，v2.1.117）
- Auto mode 的 `"$defaults"` token（v2.1.118）
- `wslInheritsWindowsSettings` 管理原則（v2.1.118）
- Vim visual / visual-line 模式（v2.1.118）
- `claude install [version]` 與 `claude plugin tag` 子命令

### 變更

- 文件主機已遷移：`docs.anthropic.com/en/docs/claude-code/*` → `code.claude.com/docs/en/*`
- Opus 4.7 effort 層級：自 2026-04-16 推出以來，`xhigh` 現在是 Claude Code 的預設值；已確認 Opus 4.7 原生 context window 為 1M（v2.1.117 修正了 `/context` 將其誤算為 200K 的問題）
- Pro/Max 訂閱者在 Opus 4.6 / Sonnet 4.6 上的預設 effort 從 `medium` 提高為 `high`（v2.1.117）
- `STYLE_GUIDE.md` 的來源 URL 從 Claude Apps 文章更新為 `code.claude.com/docs/en/changelog`

### 已棄用（持續追蹤，尚未移除）

- `includeCoAuthoredBy` 設定 → 改用 `attribution.commit` / `attribution.pr`
- `voiceEnabled` 設定 → 改用 `voice.enabled`

### 給翻譯維護者的說明

`vi/`、`zh/` 與 `uk/` 的在地化目錄由社群維護，可能落後於英文原文。同步翻譯的貢獻者應將本版本中更新的英文檔案進行 diff 比對。

## v2.1.112 — 2026-04-16

### 重點摘要

- 將所有英文教學與 Claude Code v2.1.112 及全新的 Opus 4.7 模型 (`claude-opus-4-7`) 同步，包含全新的 `xhigh` 努力層級（在 Opus 4.7 上為預設值，介於 `high` 與 `max` 之間）、兩個全新的內建斜線命令 (`/ultrareview`, `/less-permission-prompts`)、針對 Opus 4.7 的 Max 訂閱者不再需要 `--enable-auto-mode` 的自動模式、Windows 上的 PowerShell 工具、「Auto (match terminal)」主題，以及以提示詞命名的計畫檔案。所有 18 個英文文件頁尾皆已更新至 Claude Code v2.1.112。 @Luong NGUYEN

### 新功能

- 在所有模組、根目錄文件、範例與參考資料中加入完整的烏克蘭語 (uk) 本地化 (039dde2) @Evgenij I

### Bug 修復

- 修正 `pre-tool-check.sh` 鉤子協定錯誤 (bce7cf8) @yarlinghe
- 將錯誤的 mermaid 範例更改為文字區塊以通過 CI (b8a7b1f) @Evgenij I
- 修正烏克蘭語 `claude_concepts_guide.md` 目錄中的 CP1251 編碼問題 (d970cc6) @Evgenij I
- 將暫存的烏克蘭語 README 替換為完整翻譯，並修復損壞的錨點 (f6d73e2) @Evgenij I
- 將所有頁尾的 Claude Code 版本更正為 2.1.97 (63a1416) @Luong NGUYEN
- 套用 2026-04-09 的文件準確性更新 (e015f39) @Luong NGUYEN

### 文件

- 同步至 Claude Code v2.1.112 (Opus 4.7, `xhigh` 努力層級, `/ultrareview`, `/less-permission-prompts`, PowerShell 工具, Auto-match-terminal 主題) @Luong NGUYEN
- 同步至 Claude Code v2.1.110 (TUI, 推送通知, 工作階段摘要) (15f0085) @Luong NGUYEN
- 同步至 Claude Code v2.1.101，包含 `/team-onboarding`, `/ultraplan`, Monitor 工具 (2deba3a) @Luong NGUYEN
- 將越南文文件與英文原始碼同步 (561c6cb) @Thiên Toán
- 更新所有檔案的「最後更新日期」與 Claude Code 版本 (7f2e773) @Luong NGUYEN
- 在語言切換器中加入烏克蘭語連結 (9c224ff) @Luong NGUYEN
- 移除貢獻者區塊 (f07313d) @Luong NGUYEN
- 更新 GitHub 指標至 21,800+ 顆星、2,585+ 次 fork (4f55374) @Luong NGUYEN

**完整更新日誌**: https://github.com/luongnv89/claude-howto/compare/v2.3.0...v2.1.112

---

## v2.3.0 — 2026-04-07

### 新功能

- 依語言建置並發佈 EPUB 產出物 (90e9c30) @Thiên Toán
- 在 06-hooks 中新增缺失的 pre-tool-check.sh 鉤子 (b511ed1) @JiayuWang
- 在 zh/ 目錄中新增中文翻譯 (89e89d4) @Luong NGUYEN
- 新增 performance-optimizer 子代理與 dependency-check 鉤子 (f53d080) @qk

### Bug 修復

- Windows Git Bash 相容性 + stdin JSON 協定 (2cbb10c) @Luong NGUYEN
- 修正 08-checkpoints 中的 autoCheckpoint 設定文件 (749c79f) @JiayuWang
- 嵌入 SVG 圖片而非使用佔位符取代 (1b16709) @Thiên Toán
- 修正 memory README 中的巢狀程式碼區塊渲染問題 (ce24423) @Zhaoshan Duan
- 補回因 squash merge 而遺失的審查修復 (34259ca) @Luong NGUYEN
- 使鉤子腳本相容於 Windows Git Bash 並使用 stdin JSON 協定 (107153d) @binyu li

### 文件

- 將所有教學與最新的 Claude Code 文件同步 (2026 年 4 月) (72d3b01) @Luong NGUYEN
- 在語言切換器中新增中文連結 (6cbaa4d) @Luong NGUYEN
- 在英文與越南文之間新增語言切換器 (100c45e) @Luong NGUYEN
- 新增 GitHub #1 Trending 徽章 (0ca8c37) @Luong NGUYEN
- 引入 cc-context-stats 用於 context 區域監控 (d41b335) @Luong NGUYEN
- 引入 luongnv89/skills 集合與 luongnv89/asm 技能管理器 (7e3c0b6) @Luong NGUYEN
- 更新 README 資料以反映目前的 GitHub 指標 (5,900+ stars, 690+ forks) (5001525) @Luong NGUYEN
- 更新 README 資料以反映目前的 GitHub 指標 (3,900+ stars, 460+ forks) (9cb92d6) @Luong NGUYEN

### 重構

- 將 Kroki HTTP 依賴替換為本地 mmdc 渲染 (e76bbe4) @Luong NGUYEN
- 將品質檢查移至 pre-commit，將 CI 作為第二階段 (6d1e0ae) @Luong NGUYEN
- 縮減 auto-mode 權限基準 (2790fb2) @Luong NGUYEN
- 將 auto-adapt 鉤子替換為一次性權限設定腳本 (995a5d6) @Luong NGUYEN

### 其他

- 左移品質閘門 (shift-left quality gates) — 在 pre-commit 中加入 mypy，修復 CI 失敗問題 (699fb39) @Luong NGUYEN
- 新增越南文 (Tiếng Việt) 本地化 (a70777e) @Thiên Toán

**完整變更紀錄**： https://github.com/luongnv89/claude-howto/compare/v2.2.0...v2.3.0

---

## v2.2.0 — 2026-03-26

### 文件

- 將所有教學與參考文件與 Claude Code v2.1.84 (f78c094) 同步 @luongnv89
  - 更新斜線命令至 55 個以上內建功能 + 5 個綑綁技能，並將 3 個棄用功能標記為棄用
  - 將 hook 事件從 18 個擴展至 25 個，新增 `agent` hook 類型（現在共有 4 種類型）
  - 在進階功能中新增 Auto Mode、Channels、Voice Dictation
  - 新增 `effort`、`shell` 技能 frontmatter 欄位；新增 `initialPrompt`、`disallowedTools` agent 欄位
  - 新增 WebSocket MCP transport、elicitation、2KB tool cap
  - 新增 plugin LSP 支援、`userConfig`、`${CLAUDE_PLUGIN_DATA}`
  - 更新所有參考文件 (CATALOG, QUICK_REFERENCE, LEARNING-ROADMAP, INDEX)
- 將 README 重寫為著陸頁結構的指南 (32a0776) @luongnv89

### Bug 修復

- 補上缺失的 cSpell 單字與 README 章節以符合 CI 規範 (93f9d51) @luongnv89
- 將 `Sandboxing` 加入 cSpell 字典 (b80ce6f) @luongnv89

**完整變更紀錄**： https://github.com/luongnv89/claude-howto/compare/v2.1.1...v2.2.0

---

## v2.1.1 — 2026-03-13

### Bug 修復

- 移除導致 CI 連結檢查失敗的失效 marketplace 連結 (3fdf0d6) @luongnv89
- 將 `sandboxed` 與 `pycache` 加入 cSpell 字典 (dc64618) @luongnv89

**完整變更紀錄**： https://github.com/luongnv89/claude-howto/compare/v2.1.0...v2.1.1

---

## v2.1.0 — 2026-03-13

### 新功能

- 新增包含自我評估與課程測驗技能的適應性學習路徑 (1ef46cd) @luongnv89
  - `/self-assessment` — 橫跨 10 個功能領域的互動式熟練度測驗，並提供個人化學習路徑
  - `/lesson-quiz [lesson]` — 針對每堂課的知識檢查，包含 8-10 個目標問題

### Bug 修復

- 更新失效的 URL、棄用功能以及過時的參考內容 (8fe4520) @luongnv89
- 修復資源與自我評估技能中的失效連結 (7a05863) @luongnv89
- 在概念指南中使用 tilde fences 處理巢狀程式碼區塊 (5f82719) @VikalpP
- 將缺失的單字加入 cSpell 字典 (8df7572) @luongnv89

### 文件

- 第 5 階段 QA — 修復文件中的一致性、URL 與術語 (00bbe4c) @luongnv89
- 完成第 3-4 階段 — 新功能覆蓋與參考文件更新 (132de29) @luongnv89
- 在 MCP context bloat 章節中新增 MCPorter runtime (ef52705) @luongnv89
- 在 6 個指南中補上缺失的命令、功能與設定 (4bc8f15) @luongnv89
- 根據現有 repo 約定新增風格指南 (84141d0) @luongnv89
- 在指南比較表中新增自我評估列 (8fe0c96) @luongnv89
- 在貢獻者名單中為 PR #7 新增 VikalpP (d5b4350) @luongnv89
- 在 README 與 roadmap 中新增自我評估與 lesson-quiz 技能的參考 (d5a6106) @luongnv89

### 新貢獻者

- @VikalpP 在 #7 中完成了首次貢獻

**完整變更紀錄**： https://github.com/luongnv89/claude-howto/compare/v2.0.0...v2.1.0

---

## v2.0.0 — 2026-02-01

### 新功能

- 將所有文件與 Claude Code 2026 年 2 月的新功能同步 (487c96d)
  - 更新所有 10 個教學目錄與 7 個參考文件中的 26 個檔案
  - 新增 **Auto Memory** 文件 — 每個專案的持久化學習內容
  - 新增 **Remote Control**、**Web Sessions** 與 **Desktop App** 文件
  - 新增 **Agent Teams**（實驗性多代理協作）文件
  - 新增 **MCP OAuth 2.0**、**Tool Search** 與 **Claude.ai Connectors** 文件
  - 新增針對子代理的 **Persistent Memory** 與 **Worktree Isolation** 文件
  - 新增 **Background Subagents**、**Task List**、**Prompt Suggestions** 文件
  - 新增 **Sandboxing** 與 **Managed Settings**（企業版）文件
  - 新增 **HTTP Hooks** 與 7 個新的 hook 事件文件
  - 新增 **Plugin Settings**、**LSP Servers** 與 Marketplace 更新文件
  - 新增 **Summarize from Checkpoint** 回溯選項文件
  - 記錄 17 個新的斜線命令 (`/fork`, `/desktop`, `/teleport`, `/tasks`, `/fast` 等)
  - 記錄新的 CLI 參數 (`--worktree`, `--from-pr`, `--remote`, `--teleport`, `--teammate-mode` 等)
  - 記錄用於 auto memory、effort levels、agent teams 等功能的新環境變數

### 設計

- 重新設計 Logo 為指南針括號標誌，並採用極簡色調 (20779db)

### Bug 修復 / 更正

- 更新模型名稱：Sonnet 4.5 → **Sonnet 4.6**，Opus 4.5 → **Opus 4.6**
- 修復權限模式名稱：將虛構的 "Unrestricted/Confirm/Read-only" 替換為實際的 `default`/`acceptEdits`/`plan`/`dontAsk`/`bypassPermissions`
- 修復 hook 事件：移除虛構的 `PreCommit`/`PostCommit`/`PrePush`，新增真實事件 (`SubagentStart`, `WorktreeCreate`, `ConfigChange` 等)
- 修復 CLI 語法：將 `claude-code --headless` 替換為 `claude -p` (print mode)
- 修復 checkpoint 命令：將虛構的 `/checkpoint save/list/rewind/diff` 替換為實際的 `Esc+Esc` / `/rewind` 介面
- 修復工作階段管理：將虛構的 `/session list/new/switch/save` 替換為真實的 `/resume`/`/rename`/`/fork`
- 修復 plugin manifest 格式：將 `plugin.yaml` 遷移至 `.claude-plugin/plugin.json`
- 修復 MCP 設定路徑：`~/.claude/mcp.json` → `.mcp.json` (專案) / `~/.claude.json` (使用者)
- 修復文件 URL：`docs.claude.com` → `docs.anthropic.com`；移除虛構的 `plugins.claude.com`
- 移除多個檔案中的虛構設定欄位
- 將所有「最後更新」日期更新為 2026 年 2 月

**完整變更紀錄**： https://github.com/luongnv89/claude-howto/compare/20779db...v2.0.0

---

**最後更新日期**：2026 年 8 月 25 日
**Claude Code 版本**：2.1.245
**來源**：
- https://code.claude.com/docs/en/changelog
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
