<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 斜線命令

## 概述

斜線命令是在互動式工作階段中控制 Claude 行為的捷徑。它們分為幾種類型：

- **內建命令**：由 Claude Code 提供（`/help`、`/clear`、`/model`）
- **技能**：使用者定義的命令，以 `SKILL.md` 檔案形式建立（`/optimize`、`/pr`）
- **外掛命令**：來自已安裝外掛的命令（`/frontend-design:frontend-design`）
- **MCP 提示詞**：來自 MCP 伺服器的命令（`/mcp__github__list_prs`）

> **注意**：自訂斜線命令已併入技能。位於 `.claude/commands/` 中的檔案仍可運作，但現在建議使用技能（`.claude/skills/`）。兩者都會建立 `/command-name` 捷徑。請參閱 [技能指南](../03-skills/) 以獲取完整參考。

## 內建命令參考

內建命令是常用操作的捷徑。目前提供 **60 多個內建命令** 與 **10 個內建技能**。在 Claude Code 中輸入 `/` 即可查看完整列表，或在 `/` 後輸入任何字母進行篩選。

> **注意**：自 v2.1.236 起，對打錯的斜線命令（或目前工作階段中無法使用的命令）按下 Enter 時，會回報錯誤，而不是默默執行最接近的模糊比對結果。不會混淆的前綴與已定義的別名仍照常執行。

| 命令 | 用途 |
|---------|---------|
| `/add-dir <path>` | 新增工作目錄 |
| `/advisor [model\|off]` | 設定 advisor。以互動式對話框開啟；在桌面應用程式、Remote Control 及 headless（`-p` / Agent SDK）工作階段中則改用文字形式 — 單獨的 `/advisor`、`/advisor <model>` 或 `/advisor off`（v2.1.260+） |
| `/agents` | 管理代理設定 |
| `/branch [name]` | 切換到此時間點的對話副本，並保留原始對話（可用 `/resume` 回到原對話） |
| `/fork [prompt]` | 將目前對話複製到新的**背景工作階段**，並繼續在這裡工作；兩者從此各自獨立，副本會在 `claude agents` 中擁有自己的一列（v2.1.212+）。除非副本是就地編輯，否則 Claude Code 會指示它在修改程式碼前先建立自己的 worktree（隔離指示需要 v2.1.221+） |
| `/subtask <task>` | 產生一個繼承完整對話的**分叉子代理**，在你繼續工作時處理該任務；完成後結果會回傳到此對話（v2.1.212+） |
| `/btw <question>` | 在 Claude 處理主要任務時提出附帶問題；不會污染主對話的上下文 |
| `/cd <path>` | 將工作階段移至新的工作目錄，且不會破壞提示詞快取（v2.1.169 新增） |
| `/chrome` | 設定 Chrome 瀏覽器整合 |
| `/clear` | 清除對話（別名：`/reset`、`/new`） |
| `/color [color\|default]` | 設定提示詞列顏色。單獨使用 `/color`（不帶參數）會隨機選擇一個工作階段顏色 (v2.1.128+)；傳入顏色名稱或十六進位值可明確設定。 |
| `/compact [instructions]` | 壓縮對話，可選擇性加入專注指令。壓縮失敗時現在會在 UI 中顯示錯誤，而不是默默不做任何事（v2.1.216） |
| `/config` | 開啟設定（別名：`/settings`） |
| `/context` | 以彩色網格形式視覺化上下文使用情況。使用量超過上下文視窗上限時會顯示明確警告（v2.1.216） |
| `/copy [N]` | 將助手回應複製到剪貼簿；`w` 會寫入檔案 |
| `/cost` | `/usage` 的打字捷徑別名 — 開啟費用頁籤 (v2.1.118+) |
| `/desktop` | 在桌面應用程式中繼續（別名：`/app`） |
| `/diff` | 未提交變更的互動式 diff 查看器。在全螢幕渲染模式下，會改為在對話旁開啟 diff 面板，並在你繼續工作時保持開啟 — 它會列出變更的檔案及新增/刪除的行數，並在 Claude 每次編輯檔案或執行 shell 命令時重新整理；再次執行 `/diff` 或點擊 `✕` 即可關閉（v2.1.260+）。傳統渲染器則會以查看器取代提示詞區域 |
| `/doctor` | 診斷安裝健康狀況 — 可在 Claude 回應時開啟；顯示狀態圖示；按 `f` 可自動修復問題（v2.1.116 增強；v2.1.178 版面改為扁平樹狀並使用更清楚的圖示） |
| `/effort [low\|medium\|high\|xhigh\|max\|auto]` | 透過互動式方向鍵滑桿設定努力程度。層級：`low` → `medium` → `high` → `xhigh`（v2.1.111 新增）→ `max`。Opus 5、Sonnet 5 與 Opus 4.8 的預設值為 `high`（Opus 4.7 為 `xhigh`）；`xhigh` 需要 Opus 5、Sonnet 5、Opus 4.8 或 Opus 4.7；`max` 可用於 Opus 5、Sonnet 5、Opus 4.8/4.7/4.6 與 Sonnet 4.6。選單中也提供 `ultracode`（並非模型努力程度 — 它會送出 `xhigh` 並讓 Claude 編排動態工作流程；僅限目前工作階段） |
| `/exit` | 退出 REPL（別名：`/quit`） |
| `/export [filename]` | 將當前對話匯出到檔案或剪貼簿 |
| `/usage-credits` | 設定額外使用量以應對速率限制（v2.1.144 從 `/extra-usage` 重新命名；`/extra-usage` 仍可作為別名使用） |
| `/fast [on\|off]` | 切換快速模式。適用於 Opus 5 與 Opus 4.8（v2.1.219） |
| `/feedback` | 提交回饋（別名：`/bug`）。自 v2.1.141 起，可附加最近的工作階段（最近 24 小時或 7 天），讓跨越多個工作階段的回報包含完整上下文。自 v2.1.178 起，`/bug` 必須填寫描述才能提交。 |
| `/focus` | 切換專注檢視（v2.1.110 新增；取代 `Ctrl+O` 的專注切換功能） |
| `/goal <statement>` | 為當前工作階段登記一個完成條件；Claude 持續工作直到達成目標。`/goal clear` 可移除目標。進行中的目標會顯示在狀態列，並有即時覆蓋面板顯示已用時間、回合數與 token 使用量（v2.1.139 新增）。 |
| `/help` | 顯示說明 |
| `/hooks` | 查看鉤子設定 |
| `/ide` | 管理 IDE 整合 |
| `/init` | 初始化 `CLAUDE.md`。設定 `CLAUDE_CODE_NEW_INIT=1` 以進行互動式流程 |
| `/insights` | 生成工作階段分析報告 |
| `/install-github-app` | 設定 GitHub Actions 應用程式 |
| `/install-slack-app` | 安裝 Slack 應用程式 |
| `/keybindings` | 開啟按鍵綁定設定 |
| `/fewer-permission-prompts` | 分析最近的 Bash/MCP 工具呼叫，並在 `.claude/settings.json` 中新增優先白名單以減少權限提示（v2.1.111 新增） |
| `/login` | 切換 Anthropic 帳號 |
| `/logout` | 從您的 Anthropic 帳號登出 |
| `/mcp` | 管理 MCP 伺服器與 OAuth |
| `/memory` | 編輯 `CLAUDE.md`，切換自動記憶功能 |
| `/mobile` | 行動應用程式的 QR code（別名：`/ios`、`/android`） |
| `/model [model]` | 選擇模型，使用左右箭頭調整投入程度。自 v2.1.153 起，選擇會**儲存為**新工作階段的**預設值**（與 IDE 一致）；選擇後按 `s` 可僅套用於目前工作階段。（快捷鍵 `modelPicker:setAsDefault` 已重新命名為 `modelPicker:thisSessionOnly`；舊的 `d` 動作現在是 `s`。）自 v2.1.219 起，選取器會將合併後的 Opus 列顯示為「Opus (1M context)」。 |
| `/output-style [name]` | 列出並切換輸出風格（在 v2.1.91 移除後，於 v2.1.269 重新加入）。不帶參數時會列出所有風格並標示目前使用的風格。可在 headless 與 Remote Control 工作階段中使用；選擇會儲存至 `.claude/settings.local.json` |
| `/passes` | 分享一週的 Claude Code 免費使用權 |
| `/permissions` | 查看/更新權限（別名：`/allowed-tools`） |
| `/plan [description]` | 進入計畫模式 |
| `/plugin` | 管理外掛 |
| `/proactive` | `/loop` 的別名（v2.1.105 新增） |
| `/powerup` | 透過帶有動畫示範的互動式課程探索功能 |
| `/privacy-settings` | 隱私設定（僅限 Pro/Max 使用者） |
| `/release-notes` | 查看變更日誌 |
| `/recap` | 返回工作階段時顯示工作階段摘要 / 總結（v2.1.108 新增） |
| `/reload-plugins` | 重新載入啟用的外掛。自 v2.1.221 起，大多數安裝會立即啟用，因此只有在安裝摘要顯示 `Run /reload-plugins to activate.` 時才需要執行。自 v2.1.260 起可在 headless 工作階段中使用，因此也會出現在 Claude Code Desktop 與 SDK 的命令清單中 |
| `/reload-skills` | 不需重新啟動工作階段即可重新掃描技能目錄（v2.1.152 新增） |
| `/remote-control` | 從 claude.ai 進行遠端控制（別名：`/rc`） |
| `/remote-env` | 設定預設的遠端環境 |
| `/rename [name]` | 重新命名工作階段 |
| `/resume [session]` | 恢復對話（別名：`/continue`） |
| `/review [low\|medium\|high\|xhigh\|max\|ultra] [--fix] [--comment] [pr#\|branch\|path]` | `/code-review` 的別名（v2.1.223）：審查目前的 diff，或你傳入的 PR 編號、分支或路徑 — 例如 `/review 1234`。接受相同的努力程度與旗標。未指定層級時，會沿用你上次輸入的 `low`–`max` 層級 |
| `/rewind` | 回溯對話及/或程式碼（別名：`/checkpoint`） |
| `/sandbox` | 切換沙盒模式 |
| `/schedule [description]` | 建立/管理雲端排程任務 |
| `/scroll-speed <+N\|-N>` | 使用即時預覽調整 TUI 即時預覽窗格的滑鼠滾輪速度。每台機器的設定會持久化儲存於 `~/.claude/preferences.json`（v2.1.139 新增）。 |
| `/security-review` | 分析分支是否存在安全性漏洞 |
| `/skill-doctor` | 顯示哪些已載入的技能未被使用，以及每個技能佔用多少上下文，方便你決定要關閉哪些。報告會在 `/plugin` 管理器的 **Stats** 頁籤中開啟；在非互動式 `-p` 模式下則以文字輸出。透過 Remote Control 執行時會回覆 `Skill usage reports are not available on this connection.` — 請在主控該工作階段的機器上於終端機中執行（需要 v2.1.252+） |
| `/skills` | 列出可用技能 |
| `/stats` | `/usage` 的打字捷徑別名 — 開啟統計頁籤（每日使用量、工作階段、連續紀錄）(v2.1.118+) |
| `/stickers` | 訂購 Claude Code 貼紙 |
| `/status` | 顯示版本、模型、帳號，以及一列 `Session kind`，其值為 `background job · attached`、`background job · unattended` 或 `interactive`（該列於 v2.1.221 新增）。可在 Claude 回應時開啟 |
| `/statusline` | 設定狀態列 |
| `/tasks` | 列出/管理背景任務 |
| `/team-onboarding` | 根據專案的 Claude Code 設定生成團隊成員上手指南（v2.1.101 新增） |
| `/teleport` | 在此終端機中恢復 Claude Code 網頁版的工作階段；會開啟你的網頁工作階段選取器（別名：`/tp`）。需要 claude.ai 訂閱 |
| `/terminal-setup` | 設定終端機快捷鍵 |
| `/theme` | 開啟主題選擇器 / 管理自訂主題 (v2.1.118)。透過 `~/.claude/themes/<name>.json` 中的 JSON 定義自訂主題 |
| `/tui` | 切換無閃爍渲染的全螢幕 TUI（文字使用者介面）模式（v2.1.110 新增） |
| `/ultrareview` | 全面的雲端多代理程式碼審查（v2.1.111 新增）。目前建議的呼叫方式為 `/code-review ultra`；`/ultrareview` 保留為別名。Pro 與 Max 方案包含 3 次免費執行，之後需要使用額度 |
| `/upgrade` | 開啟升級頁面以獲取更高階的方案 |
| `/usage` | 標準使用量儀表板 (v2.1.118) — 整合方案使用限制、速率限制、費用與每日工作階段統計資料。`/cost` 與 `/stats` 是開啟特定頁籤的打字捷徑別名 |
| `/voice` | 切換按住說話語音輸入功能 |
| `/workflows` | 查看執行中與已完成的動態工作流程（v2.1.154 新增）。請參閱 [Dynamic Workflows](../09-advanced-features/README.md#動態工作流程-dynamic-workflows) |

> **為什麼 `/cd` 很重要：** 以往切換目錄會讓快取失溫（使下一個回合更慢、成本更高）；`/cd` 可在切換時保留提示詞快取。

### 內建技能

這些技能隨 Claude Code 一起發佈，並可像斜線命令一樣呼叫：

| 技能 | 用途 |
|-------|---------|
| `/batch <instruction>` | 使用 worktrees 編排大規模的並行變更 |
| `/claude-api` | 載入專案語言的 Claude API 參考文件 |
| `/dataviz` | 圖表與儀表板設計指引，附可執行的調色盤驗證工具（v2.1.198） |
| `/debug [description]` | 啟用除錯日誌 |
| `/design [description]` | 建立**設計畫布** — 多畫板的視覺設計（UI 模型、畫面流程、登陸頁、海報），以 artifact 形式發佈並以視覺方式而非程式碼進行修改。當你想反覆調整的是版面時，用它取代手寫 HTML。研究預覽版；需要 v2.1.233+ 以及 Pro、Max、Team 或 Enterprise 方案 |
| `/loop [interval] <prompt>` | 按間隔重複執行提示詞 |
| `/code-review [low\|medium\|high\|xhigh\|max\|ultra] [--fix] [--comment] [pr#\|branch\|path]` | 審查目前的 diff — 或你傳入的 PR 編號、分支或路徑 — 找出正確性錯誤。傳入 `--fix` 可套用發現的修正、`--comment` 可將其以行內 GitHub PR 評論發佈，或傳入 `ultra` 執行深度雲端審查；對 `github.com` PR 目標使用 `ultra` 時，`--post` 會預先選取將發現發佈到 PR。未指定努力程度時，審查會沿用你上次輸入的層級（v2.1.223）。最初於 v2.1.146 吸收了 `/simplify`，但 `/simplify` 在 v2.1.154 重新成為獨立命令 |
| `/simplify` | 執行僅限清理的審查（重用 / 簡化 / 效率 / 抽象層級）並套用修正；**不會**尋找錯誤 — 找錯請用 `/code-review`。曾短暫作為 `/code-review --fix` 的別名（v2.1.152），於 v2.1.154 改為僅限清理 |

### 已棄用的命令

| 命令 | 狀態 |
|---------|--------|
| `/pr-comments` | 已在 v2.1.91 中移除 — 請直接詢問 Claude 以查看 PR 評論 |
| `/vim` | 已在 v2.1.92 中移除 — 請使用 /config → Editor mode |
| `/undo` | 自 v2.1.245 起不再列於官方命令參考中（它於 v2.1.108 新增為 `/rewind` 的別名）— 請使用 `/rewind` 或按兩次 `Esc` |

### 最近變更

- `/fork` 與 `/subtask` 在 **v2.1.212** 互換角色。`/fork` 現在會將對話複製到新的獨立背景工作階段；它原本的分叉子代理行為移至新的 `/subtask` 命令。歷史：`/fork` 從 v2.1.77 到 v2.1.161 是 `/branch` 的別名；從 v2.1.161 到 v2.1.211 則會啟動分叉子代理（即現在 `/subtask` 的功能）。當 agent view 關閉時，`/subtask` 無法使用，`/fork` 會保留分叉子代理行為
- `/resume`（不帶參數）會開啟過往工作階段的選取器 — 包括已從可見清單中移除的工作階段 — 並將所選工作階段以背景工作階段恢復 (v2.1.212)
- `/output-style` 曾被棄用 (v2.1.73) 並移除 (v2.1.91)，之後於 **v2.1.269 重新加入** — 在 v2.1.278 中再度是可用的命令，且可在 headless 與 Remote Control 工作階段中使用。輸出風格也仍可透過 `/config` → Output style 或 `outputStyle` 設定使用；內建風格有 Default、Proactive、Explanatory、Learning 與 Concise（v2.1.237 新增）
- `/review` 成為 `/code-review` 的完整別名 — 相同的目標、努力程度與旗標 (v2.1.223)。歷史：它在 v2.1.186 首次改用 `/code-review medium` 引擎，當時仍僅限 PR
- 新增 `/effort` 命令；`max` 層級可用於 Opus 4.6+（原先僅限 Opus 4.6）
- 新增 `/voice` 命令，用於按住說話（push-to-talk）語音聽寫
- 新增 `/schedule` 命令，用於建立/管理排程任務
- 新增 `/color` 命令，用於自訂提示詞列
- /pr-comments 已在 v2.1.91 中移除 — 請直接詢問 Claude 以查看 PR 評論
- /vim 已在 v2.1.92 中移除 — 請改用 /config → Editor mode
- `/ultraplan` 已在 v2.1.222 移除 — 請改用計畫模式
- 新增 /powerup，用於互動式功能課程
- 新增 /sandbox，用於切換沙盒模式
- `/model` 選取器現在顯示易讀的標籤（例如「Sonnet 4.6」）而非原始模型 ID
- `/resume` 支援 `/continue` 別名
- MCP 提示詞現在可透過 `/mcp__<server>__<prompt>` 命令使用（參閱 [MCP Prompts as Commands](#mcp-prompts-as-commands)）
- 新增 `/team-onboarding`，用於自動生成團隊成員上手指南 (v2.1.101)
- 新增 `/tui` 命令，用於無閃爍的全螢幕 TUI 渲染 (v2.1.110)
- 新增 `/focus` 命令，用於切換專注檢視模式；`Ctrl+O` 現在僅切換詳細逐字稿 (v2.1.110)
- 新增 `/recap` 命令，用於手動觸發工作階段上下文摘要 (v2.1.108)
- `/undo` 已新增為 `/rewind` 的別名 (v2.1.108)；自 v2.1.245 起不再出現在官方命令參考中 — 請使用 `/rewind` 或 `Esc Esc`
- `/proactive` 已新增為 `/loop` 的別名 (v2.1.105)
- `/effort` 新增互動式方向鍵滑桿與 `high` 和 `max` 之間的新 `xhigh` 層級；Opus 4.7 方案的預設努力程度提升為 `xhigh` (v2.1.111)。Opus 4.8 的預設值為 `high` (v2.1.154)；Opus 5 的預設值也是 `high` (v2.1.219)
- 新增 `/ultrareview`，用於全面的雲端多代理程式碼審查 (v2.1.111)
- 新增 `/fewer-permission-prompts`，用於分析 Bash/MCP 工具呼叫並透過 `.claude/settings.json` 的白名單減少權限提示 (v2.1.111)
- Auto 模式對於 Max 訂閱者使用 Opus 4.7 時不再需要 `--enable-auto-mode` 旗標 (v2.1.112)
- 新增 `/goal` — 工作階段層級的完成條件，Claude 跨回合持續朝目標工作；即時覆蓋面板顯示已用時間、回合數與 token 使用量 (v2.1.139)
- 新增 `/scroll-speed` — 調整 TUI 即時預覽窗格的滑鼠滾輪速度；設定每台機器持久化儲存 (v2.1.139)
- 新增 `/reload-skills` — 不需重新啟動工作階段即可重新掃描技能目錄 (v2.1.152)
- `/model` 現在會將所選模型儲存為新工作階段的預設值；按 `s` 則僅套用於目前工作階段（快捷鍵 `modelPicker:setAsDefault` → `modelPicker:thisSessionOnly`）(v2.1.153)
- 新增 `/workflows` — 查看執行中與已完成的動態工作流程 (v2.1.154)
- `/simplify` 重新成為獨立的僅限清理審查命令（重用 / 簡化 / 效率 / 抽象層級），與 `/code-review` 的找錯功能分開 (v2.1.154)
- `/status` 新增 `Session kind` 列，區分附加（attached）與無人值守（unattended）的背景工作與互動式工作階段 (v2.1.221)
- 外掛安裝現在會在安全時立即啟用；只有在安裝摘要要求時才需要 `/reload-plugins` (v2.1.221)
- `/ultraplan` 已移除 — 請使用計畫模式 (v2.1.222)
- `/code-review` 與 `/review` 在你省略努力程度時，會記住你上次輸入的層級 (v2.1.223)
- `/code-review ultra` 成為雲端多代理審查的建議入口；`/ultrareview` 保留為別名 (v2.1.223)
- `/code-review` 在 `high`、`xhigh` 與 `max` 努力程度下，現在也和其他層級一樣在背景代理中執行 (v2.1.232)
- 移除建議你建立自訂子代理的啟動提示，以及 `/powerup` 導覽中對應的提醒 (v2.1.232)
- `/permissions` 現在可在 Claude 工作時開啟 — 規則變更會套用於目前回合的剩餘部分 (v2.1.234)
- `/add-dir <path>` 現在可在 Claude 工作時使用；`/add-dir`、`/autocompact`、`/theme`、`/help`、`/config` 與 `/advisor` 對話框會在回合進行中**於全螢幕 TUI 中**開啟，而不是排隊等到 Claude 回應完畢（`/bug` 自 v2.1.232 起已會立即開啟）(v2.1.234)

### `/goal` — 工作階段層級的完成條件

> **v2.1.139 新增功能**

使用 `/goal` 為當前工作階段登記一個完成條件。Claude 跨回合持續朝目標工作，覆蓋面板會顯示已用時間、回合數與已使用的 token。使用 `/goal clear` 可清除目標。可在互動模式、`claude -p` 及遠端控制中使用。

```
User: /goal Migrate the payments service from REST to gRPC and get the integration tests passing.
Claude: Goal registered. I'll work toward this until you clear it.
[Goal panel: ⏱ 0s · turns 0 · tokens 0]

User: start by listing the REST endpoints
Claude: [does the work, panel updates]
```

**停滯背景任務的主動回報（v2.1.234）：** 目標進行中時，若背景任務超過 30 分鐘沒有進展，Claude 會主動回報狀態，而不是默默繼續。可用 `CLAUDE_CODE_GOAL_CHECKIN_MINUTES` 環境變數調整門檻（以分鐘為單位），或設為 `0` 完全停用回報。

### `/team-onboarding` — 團隊成員上手指南

> **v2.1.101 新增功能**

使用 `/team-onboarding` 可以根據您專案中本地的 Claude Code 使用情況來生成團隊成員上手指南。此命令會檢查您的 `CLAUDE.md`、已安裝的技能、子代理、鉤子以及最近的工作流程，然後生成一份入職文件，幫助新開發人員快速上手。

這是內建命令 — 無需安裝。

**用法：**

```bash
claude /team-onboarding
```

生成的指南摘要包含：

- 來自 [`CLAUDE.md`](../02-memory/README.md) 的專案目的與關鍵慣例
- 可用的 [skills](../03-skills/README.md) 以及它們何時會被自動呼叫
- 已設定的 [subagents](../04-subagents/README.md) 及其職責
- 在常見事件中執行的 [Hooks](../06-hooks/README.md)
- 新手應該了解的常見工作流程

**可用性：** 隨 Claude Code v2.1.101 發佈（2026 年 4 月 11 日）。

## 自訂命令（現為技能）

自訂斜線命令已**整合至技能（skills）中**。兩種方式都能建立您可以透過 `/command-name` 呼叫的命令：

| 方式 | 位置 | 狀態 |
|----------|----------|--------|
| **技能（建議）** | `.claude/skills/<name>/SKILL.md` | 目前標準 |
| **舊版命令** | `.claude/commands/<name>.md` | 仍可運作 |

如果技能與命令名稱相同，**技能將具有優先權**。例如，當 `.claude/commands/review.md` 與 `.claude/skills/review/SKILL.md` 同時存在時，將使用技能版本。

### 遷移路徑

您現有的 `.claude/commands/` 檔案可以繼續運作而無需更改。若要遷移至技能：

**之前（命令）：**
```
.claude/commands/optimize.md
```

**之後（技能）：**
```
.claude/skills/optimize/SKILL.md
```

### 為什麼要使用技能？

與舊版命令相比，技能提供了額外功能：

- **目錄結構**：可封裝腳本、範本與參考檔案
- **自動呼叫**：當相關時，Claude 可以自動觸發技能
- **呼叫控制**：可選擇由使用者、Claude 或兩者皆可呼叫
- **子代理執行**：透過 `context: fork` 在隔離的上下文中執行技能
- **漸進式揭露**：僅在需要時才載入額外檔案

### 將自訂命令建立為技能

建立一個包含 `SKILL.md` 檔案的目錄：

```bash
mkdir -p .claude/skills/my-command
```

**檔案：** `.claude/skills/my-command/SKILL.md`

```yaml
---
name: my-command
description: 此命令的功能以及何時使用
---

# My Command

當此命令被呼叫時，Claude 需遵循的指令。

1. 第一步
2. 第二步
3. 第三步
```

### Frontmatter 參考

| 欄位 | 用途 | 預設值 |
|-------|---------|---------|
| `name` | 命令名稱（將成為 `/name`） | 目錄名稱 |
| `description` | 簡短描述（幫助 Claude 判斷何時使用） | 第一段文字 |
| `argument-hint` | 用於自動完成的預期參數 | 無 |
| `allowed-tools` | 命令無需許可即可使用的工具 | 繼承 |
| `model` | 指定使用的模型 | 繼承 |
| `disable-model-invocation` | 若為 `true`，則只有使用者可以呼叫（Claude 不行） | `false` |
| `user-invocable` | 若為 `false`，則從 `/` 選單中隱藏 | `true` |
| `context` | 設定為 `fork` 以在隔離的子代理中執行 | 無 |
| `agent` | 使用 `context: fork` 時的代理類型 | `general-purpose` |
| `hooks` | 技能範圍內的鉤子（PreToolUse, PostToolUse, Stop） | 無 |

### 參數

命令可以接收參數：

**所有參數使用 `$ARGUMENTS`：**

```yaml
---
name: fix-issue
description: 透過編號修復 GitHub issue
---

按照我們的編碼標準修復 issue #$ARGUMENTS
```

用法：`/fix-issue 123` → `$ARGUMENTS` 變為 "123"

**個別參數使用 `$0`、`$1` 等：**

```yaml
---
name: review-pr
description: 優先審查 PR
---

優先審查 PR #$0，優先級為 $1
```

用法：`/review-pr 456 high` → `$0`="456", `$1`="high"

`${CLAUDE_PROJECT_DIR}` 會解析為專案根目錄的絕對路徑（v2.1.196）。

### 使用 Shell 命令進行動態上下文處理

在提示詞執行前，使用 `` !`command` `` 執行 bash 命令：

```yaml
---
name: commit
description: 建立帶有上下文的 git commit
allowed-tools: Bash(git *)
---

## Context

- 目前 git 狀態：!`git status`
- 目前 git diff：!`git diff HEAD`
- 目前分支：!`git branch --show-current`
- 最近的 commits：!`git log --oneline -5`

## Your task

根據上述變更，建立單一的 git commit。
```

### 檔案參考

使用 `@` 包含檔案內容：

```markdown
Review the implementation in @src/utils/helpers.js
Compare @src/old-version.js with @src/new-version.js
```

## 外掛命令

外掛可以提供自訂命令：

```
/plugin-name:command-name
```

或者在沒有命名衝突時，直接使用 `/command-name`。

**範例：**
```bash
/frontend-design:frontend-design
/commit-commands:commit
```

## MCP Prompts as Commands

MCP 伺服器可以將提示詞作為斜線命令公開：

```
/mcp__<server-name>__<prompt-name> [arguments]
```

**範例：**
```bash
/mcp__github__list_prs
/mcp__github__pr_review 456
/mcp__jira__create_issue "Bug title" high
```

### MCP 權限語法

在權限設定中控制 MCP 伺服器存取權：

- `mcp__github` — 存取整個 GitHub MCP 伺服器
- `mcp__github__*` — 使用萬用字元存取所有工具
- `mcp__github__get_issue` — 存取特定工具

## 命令架構

```mermaid
graph TD
    A["User Input: /command-name"] --> B{"Command Type?"}
    B -->|Built-in| C["Execute Built-in"]
    B -->|Skill| D["Load SKILL.md"]
    B -->|Plugin| E["Load Plugin Command"]
    B -->|MCP| F["Execute MCP Prompt"]

    D --> G["Parse Frontmatter"]
    G --> H["Substitute Variables"]
    H --> I["Execute Shell Commands"]
    I --> J["Send to Claude"]
    J --> K["Return Results"]
```

## 命令生命週期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as File System
    participant CLI as Shell/Bash

    User->>Claude: Types /optimize
    Claude->>FS: Searches .claude/skills/ and .claude/commands/
    FS-->>Claude: Returns optimize/SKILL.md
    Claude->>Claude: Parses frontmatter
    Claude->>CLI: Executes !`command` substitutions
    CLI-->>Claude: Command outputs
    Claude->>Claude: Substitutes $ARGUMENTS
    Claude->>User: Processes prompt
    Claude->>User: Returns results
```

## 此資料夾中的可用命令

這些範例命令可以作為 skills 或舊版命令進行安裝。

### 1. `/optimize` - 程式碼最佳化

分析程式碼中的效能問題、記憶體洩漏以及最佳化機會。

**用法：**
```
/optimize
[貼上您的程式碼]
```

### 2. `/pr` - Pull Request 準備

引導完成 PR 準備檢查清單，包括 linting、測試與 commit 格式化。

**用法：**
```
/pr
```

**螢幕截圖：**
![/pr](pr-slash-command.png)

### 3. `/generate-api-docs` - API 文件產生器

從原始碼產生完整的 API 文件。

**用法：**
```
/generate-api-docs
```

### 4. `/commit` - 帶有上下文的 Git Commit

根據您儲存庫中的動態上下文建立一個 git commit。

**用法：**
```
/commit [optional message]
```

### 5. `/push-all` - 暫存、提交與推送

暫存所有變更、建立 commit 並進行安全檢查後推送至遠端。

**用法：**
```
/push-all
```

**安全檢查：**
- 敏感資訊：`.env*`, `*.key`, `*.pem`, `credentials.json`
- API Keys：偵測真實金鑰與佔位符
- 大型檔案：未經 Git LFS 的 `>10MB` 檔案
- 建置產物：`node_modules/`, `dist/`, `__pycache__/`

### 6. `/doc-refactor` - 文件重構

重構專案文件以提升清晰度與易讀性。

**用法：**
```
/doc-refactor
```

### 7. `/setup-ci-cd` - CI/CD 流水線設定

實作 pre-commit hooks 與 GitHub Actions 以進行品質保證。

**用法：**
```
/setup-ci-cd
```

### 8. `/unit-test-expand` - 測試覆蓋率擴充

透過針對未測試的分支與邊際情況來增加測試覆蓋率。

**用法：**
```
/unit-test-expand
```

## 安裝

### 作為技能（建議）

複製到您的 skills 目錄：

```bash
# 建立 skills 目錄
mkdir -p .claude/skills

# 為每個指令檔案建立一個 skill 目錄
for cmd in optimize pr commit; do
  mkdir -p .claude/skills/$cmd
  cp 01-slash-commands/$cmd.md .claude/skills/$cmd/SKILL.md
done
```

### 作為舊版命令

複製到您的 commands 目錄：

```bash
# 專案範圍（團隊使用）
mkdir -p .claude/commands
cp 01-slash-commands/*.md .claude/commands/

# 個人使用
mkdir -p ~/.claude/commands
cp 01-slash-commands/*.md ~/.claude/commands/
```

## 建立您自己的命令

### 技能範本（建議）

建立 `.claude/skills/my-command/SKILL.md`：

```yaml
---
name: my-command
description: 此命令的功能。當 [觸發條件] 時使用。
argument-hint: [可選參數]
allowed-tools: Bash(npm *), Read, Grep
---

# 命令標題

## 上下文

- 目前分支：!`git branch --show-current`
- 相關檔案：@package.json

## 指令

1. 第一步
2. 第二步（帶有參數）：$ARGUMENTS
3. 第三步

## 輸出格式

- 如何格式化回應
- 應包含的內容
```

### 僅限使用者使用的命令（無自動呼叫）

適用於具有副作用且 Claude 不應自動觸發的命令：

```yaml
---
name: deploy
description: 部署至正式環境
disable-model-invocation: true
allowed-tools: Bash(npm *), Bash(git *)
---

將應用程式部署至正式環境：

1. 執行測試
2. 建置應用程式
3. 推送到部署目標
4. 驗證部署
```

## 最佳實務

| 應該（Do） | 不應該（Don't） |
|------|---------|
| 使用清晰且具備行動導向的名稱 | 為一次性任務建立命令 |
| 包含帶有觸發條件的 `description` | 在命令中構建複雜邏輯 |
| 讓命令專注於單一任務 | 將敏感資訊寫死（Hardcode） |
| 使用 `disable-model-invocation` 來處理副作用 | 跳過 description 欄位 |
| 使用 `!` 前綴來處理動態上下文 | 假設 Claude 知道目前的狀態 |
| 將相關檔案整理在 skill 目錄中 | 將所有內容都放在單一檔案中 |

## 疑難排解

### 找不到命令（Command Not Found）

**解決方案：**
- 檢查檔案是否位於 `.claude/skills/<name>/SKILL.md` 或 `.claude/commands/<name>.md`
- 確認 frontmatter 中的 `name` 欄位與預期的命令名稱一致
- 重啟 Claude Code 工作階段（session）
- 執行 `/help` 查看可用命令

### 命令執行結果不如預期

**解決方案：**
- 加入更具體的指令
- 在 skill 檔案中包含範例
- 若使用 bash 命令，請檢查 `allowed-tools`
- 先使用簡單的輸入進行測試

### Skill 與 Command 衝突

如果兩者名稱相同，**skill 將具有優先權**。請刪除其中一個或重新命名。

## 相關指南

- **[Skills](../03-skills/)** — 關於 skills（自動觸發的能力）的完整參考
- **[Memory](../02-memory/)** — 透過 CLAUDE.md 實現的持久化上下文
- **[Subagents](../04-subagents/)** — 委派的 AI 代理（agents）
- **[Plugins](../07-plugins/)** — 綑綁的命令集合
- **[Hooks](../06-hooks/)** — 事件驅動的自動化

## 其他資源

- [Official Interactive Mode Documentation](https://code.claude.com/docs/en/interactive-mode) — 內建命令參考
- [Official Skills Documentation](https://code.claude.com/docs/en/skills) — 完整的 skills 參考
- [CLI Reference](https://code.claude.com/docs/en/cli-reference) — 命令列選項

---

**最後更新日期**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/output-styles
- https://code.claude.com/docs/en/skills
- https://code.claude.com/docs/en/slash-commands
- https://code.claude.com/docs/en/interactive-mode
- https://code.claude.com/docs/en/interactive-mode#review-changes-with-diff
- https://code.claude.com/docs/en/changelog
- https://code.claude.com/docs/en/commands
- https://code.claude.com/docs/en/whats-new/2026-w34
- https://code.claude.com/docs/en/model-config
- https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
- https://github.com/anthropics/claude-code/releases/tag/v2.1.139
- https://github.com/anthropics/claude-code/releases/tag/v2.1.144
- https://github.com/anthropics/claude-code/releases/tag/v2.1.152
- https://github.com/anthropics/claude-code/releases/tag/v2.1.153
- https://github.com/anthropics/claude-code/releases/tag/v2.1.154

**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5

*屬於 [Claude How To](../) 指南系列的一部分*
