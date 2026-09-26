<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 記憶體系統

記憶功能讓 Claude 能夠在不同的工作階段與對話之間保留上下文。它以兩種形式存在：claude.ai 中的自動合成，以及 Claude Code 中基於檔案系統的 CLAUDE.md。

## 概述

Claude Code 中的記憶提供了可跨多個工作階段與對話持續存在的持久上下文。與暫時性的上下文視窗不同，記憶檔案讓您可以：

- 在團隊中共享專案標準
- 儲存個人開發偏好
- 維持特定目錄的規則與設定
- 匯入外部文件
- 將記憶作為專案的一部分進行版本控制

記憶系統在多個層級上運作，從全域個人偏好到特定的子目錄，實現了對 Claude 記憶內容及其應用方式的細粒度控制。

## 記憶命令快速參考

| 命令 | 用途 | 用法 | 使用時機 |
|---------|---------|-------|-------------|
| `/init` | 初始化專案記憶 | `/init` | 開始新專案、首次設定 CLAUDE.md 時 |
| `/memory` | 在編輯器中編輯記憶檔案 | `/memory` | 大規模更新、重新組織、審查內容時 |
| `#` 前綴 | ~~快速單行新增記憶~~ **已廢棄** | — | 改用 `/memory` 或透過對話方式提出 |
| `@path/to/file` | 匯入外部內容 | `@README.md` 或 `@docs/api.md` | 在 CLAUDE.md 中引用現有文件時 |

## 快速入門：初始化記憶

### `/init` 命令

`/init` 命令是在 Claude Code 中設定專案記憶最快的方式。它會初始化一個包含基礎專案文件的 `CLAUDE.md` 檔案。

**用法：**

```bash
/init
```

**功能說明：**

- 在您的專案中建立一個新的 `CLAUDE.md` 檔案（通常位於 `./CLAUDE.md` 或 `./.claude/CLAUDE.md`）
- 建立專案慣例與指南
- 為跨工作階段的上下文持久化奠定基礎
- 提供用於記錄專案標準的範本結構

**增強型互動模式：** 設定 `CLAUDE_CODE_NEW_INIT=1` 可啟用多階段互動流程，引導您逐步完成專案設定：

```bash
CLAUDE_CODE_NEW_INIT=1 claude
/init
```

**何時使用 `/init`：**

- 使用 Claude Code 開啟新專案時
- 建立團隊程式碼標準與慣例時
- 建立關於程式碼庫結構的文件時
- 為協作開發設定記憶層級時

**範例工作流程：**

```markdown
# 在您的專案目錄中
/init

# Claude 會建立具有如下結構的 CLAUDE.md：
# Project Configuration
## Project Overview
- Name: Your Project
- Tech Stack: [Your technologies]
- Team Size: [Number of developers]

## Development Standards
- Code style preferences
- Testing requirements
- Git workflow conventions
```

### 快速更新記憶

> **注意**：用於行內記憶的 `#` 快捷鍵已停止使用。請使用 `/memory` 直接編輯記憶檔案，或透過對話要求 Claude 記住某些內容（例如：「記住我們總是使用 TypeScript strict mode」）。

建議將資訊加入記憶的方式如下：

**選項 1：使用 `/memory` 命令**

```bash
/memory
```

在您的系統編輯器中開啟記憶檔案以進行直接編輯。

**選項 2：透過對話要求**

```
Remember that we always use TypeScript strict mode in this project.
Please add to memory: prefer async/await over promise chains.
```

Claude 將根據您的要求更新適當的 `CLAUDE.md` 檔案。

**歷史參考**（已失效）：

先前使用 `#` 前綴快捷鍵可以在行內加入規則：

```markdown
# Always use TypeScript strict mode in this project  ← 已無法運作
```

如果您先前依賴此模式，請切換至 `/memory` 命令或使用對話式要求。

### `/memory` 命令

`/memory` 命令提供在 Claude Code 工作階段中直接存取並編輯 `CLAUDE.md` 記憶檔案的功能。它會在您的系統編輯器中開啟記憶檔案，以便進行全面的編輯。當檔案在 GUI 編輯器中開啟時，工作階段不再因檔案保持開啟而被阻塞，因此您可以同時繼續工作（v2.1.216）；而 Vim 等終端機編輯器仍會佔用終端機，直到您離開為止。

**用法：**

```bash
/memory
```

**功能說明：**

- 在系統預設編輯器中開啟您的記憶檔案
- 允許您進行大量的增加、修改與重組
- 提供對層級結構中所有記憶檔案的直接存取
- 使您能夠管理跨工作階段的持久上下文

**何時使用 `/memory`：**

- 審查現有的記憶內容時
- 對專案標準進行大規模更新時
- 重組記憶結構時
- 加入詳細文件或指南時
- 隨著專案演進維護與更新記憶

**比較：`/memory` vs `/init`**

| 項目 | `/memory` | `/init` |
|--------|-----------|---------|
| **目的** | 編輯現有的記憶檔案 | 初始化新的 CLAUDE.md |
| **使用時機** | 更新/修改專案上下文 | 開始新專案 |
| **動作** | 開啟編輯器進行變更 | 生成初始範本 |
| **工作流程** | 持續性維護 | 一次性設定 |

**範例工作流程：**

```markdown
# Open memory for editing
/memory

# Claude presents options:
# 1. Managed Policy Memory
# 2. Project Memory (./CLAUDE.md)
# 3. User Memory (~/.claude/CLAUDE.md)
# 4. Local Project Memory

# Choose option 2 (Project Memory)
# Your default editor opens with ./CLAUDE.md content

# Make changes, save, and close editor
# Claude automatically reloads the updated memory
```

**使用記憶匯入 (Memory Imports)：**

CLAUDE.md 檔案支援 `@path/to/file` 語法來包含外部內容：

```markdown
# Project Documentation
See @README.md for project overview
See @package.json for available npm commands
See @docs/architecture.md for system design

# Import from home directory using absolute path
@~/.claude/my-project-instructions.md
```

**匯入功能特性：**

- 同時支援相對路徑與絕對路徑（例如：`@docs/api.md` 或 `@~/.claude/my-project-instructions.md`）
- 支援遞迴匯入，最大深度為 4 次跳轉（hops）
- 首次從外部位置進行匯入時，會觸發安全性審核對話框
- 匯入指令不會在 Markdown 的行內程式碼或程式碼區塊內被執行（因此在範例中記錄這些指令是安全的）
- 透過引用現有文件，有助於避免重複內容
- 自動將引用的內容包含在 Claude 的上下文中

## 記憶架構

Claude Code 中的記憶採用層級式系統，不同的範圍（scope）用於不同的目的。與 Claude Web/Desktop 每 24 小時進行一次的綜合週期不同（請參閱下方的 [Claude Web/Desktop 中的記憶](#claude-webdesktop-中的記憶)），Claude Code 有兩套記憶系統，兩者都會在每個工作階段開始時載入，並持續更新，而非依照計時器更新：

```mermaid
graph TB
    A["Session Start"]
    B["CLAUDE.md Files<br/>(you write)"]
    C["Auto Memory<br/>(Claude writes)"]
    D["Claude Session"]
    E["Your Correction /<br/>Preference"]

    B -->|loaded in full| A
    C -->|MEMORY.md loaded| A
    A --> D
    D -->|"Remember that..."| E
    E -->|writes during session| C
    D -->|"add this to CLAUDE.md"| B
```

## Claude Code 中的記憶層級

Claude Code 有兩套互補的記憶系統，兩者都會在每次對話開始時載入：**CLAUDE.md 檔案**（由您撰寫的指令）與 **auto memory**（Claude 自己撰寫的筆記）。CLAUDE.md 檔案會**串接至上下文中，而不是彼此覆寫** — 這並非較高層級取代較低層級的嚴格優先順序鏈。`.claude/rules/*.md` 檔案則是另一個相關但獨立的機制，用於依主題或路徑劃分範圍的指令。

**CLAUDE.md 檔案位置，依載入順序排列（從最廣範圍到最具體）：**

| 範圍 | 位置 | 用途 |
|-------|----------|---------|
| 管理政策 | macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`<br>Linux/WSL: `/etc/claude-code/CLAUDE.md`<br>Windows: `C:\Program Files\ClaudeCode\CLAUDE.md` | 由 IT/DevOps 管理的組織範圍指令。無法被個人設定排除。 |
| 使用者指令 | `~/.claude/CLAUDE.md` | 適用於所有專案的個人偏好 |
| 專案指令 | `./CLAUDE.md` 或 `./.claude/CLAUDE.md` | 團隊共享的指令，受版本控制。自 v2.1.277 起，當工作目錄及其上層都不存在 `CLAUDE.md` 或 `CLAUDE.local.md` 時，改在同一層級載入 `./AGENTS.md` — 由 `/config` 中的 **Project instructions** 控制 |
| 本地指令 | `./CLAUDE.local.md` | 個人的專案特定偏好；請加入 `.gitignore` |

在目錄樹中，Claude Code 會從您的工作目錄向上走訪：若您從 `foo/bar/` 啟動，`foo/CLAUDE.md` 會在 `foo/bar/CLAUDE.md` 之前載入，因此越接近啟動位置的指令會*越晚*被讀取 — 這並非覆寫意義上的「最高優先權」，只是在上下文中最新出現。在每個目錄中，`CLAUDE.local.md` 會附加在 `CLAUDE.md` 之後。位於工作目錄*之下*子目錄中的 CLAUDE.md 與 CLAUDE.local.md 檔案，會在 Claude 讀取這些子目錄中的檔案時按需載入，而不是在啟動時載入。

組織也可以透過 `claudeMd` 鍵，將受管理的 CLAUDE.md 內容直接放在 `managed-settings.json` 中，而不必另外部署檔案。此設定僅在管理/政策設定中有效 — 在使用者或專案設定中設定 `claudeMd` 不會有任何效果。

**`.claude/rules/*.md`** — 模組化、特定主題的指令，可透過 `paths` frontmatter 選擇性地限定於特定檔案路徑。沒有 `paths` 欄位的規則會無條件載入，優先順序與 `.claude/CLAUDE.md` 相同；限定路徑的規則則會在 Claude 讀取符合的檔案時按需載入。使用者層級規則（`~/.claude/rules/`）會在專案規則之前載入。

**Auto memory**（`~/.claude/projects/<project>/memory/`）是另一套獨立的系統：它是 Claude 自己的筆記，不是 CLAUDE.md 的內容，也不屬於上述的串接順序。請參閱下方的 [Auto Memory](#auto-memory)。

> **注意**：`CLAUDE.local.md` 在 [官方文件](https://code.claude.com/docs/en/memory) 中得到完全支援與說明。它提供不會被提交至版本控制的個人專案特定偏好。請將 `CLAUDE.local.md` 加入您的 `.gitignore`。

**記憶探索行為：**

```mermaid
graph TD
    A["Managed Policy<br/>/Library/.../ClaudeCode/CLAUDE.md"] -->|loads first| B["User Instructions<br/>~/.claude/CLAUDE.md"]
    B --> C["Project Instructions<br/>./CLAUDE.md or ./.claude/CLAUDE.md"]
    C --> D["Local Instructions<br/>./CLAUDE.local.md"]

    C -->|imports| H["@docs/architecture.md"]
    H -->|imports| I["@docs/api-standards.md"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#f3e5f5,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#e8f5e9,stroke:#333,color:#333
    style H fill:#e1f5fe,stroke:#333,color:#333
    style I fill:#e1f5fe,stroke:#333,color:#333
```

圖中所有檔案都會串接成單一上下文，而不是透過覆寫來選擇 — 後面的方塊會較晚出現在上下文中，而不是「取代」前面的方塊。

## AGENTS.md

`AGENTS.md` 是跨工具的專案上下文檔案：與 CLAUDE.md 屬於同一*類*文件，撰寫目的是讓多個程式設計代理能共用同一套專案慣例。自 **v2.1.277** 起，Claude Code 會直接將其作為專案指令讀取，而不需要您匯入它。

**預設行為**（`claude-md-or-agents-md`）：

| 專案包含的檔案 | Claude Code 讀取的內容 |
|---------------------------|------------------------|
| `AGENTS.md`，沒有 `CLAUDE.md` | `AGENTS.md` |
| 同時有 `AGENTS.md` 與 `CLAUDE.md` | 僅 `CLAUDE.md` |
| 以 `@AGENTS.md` 匯入 `AGENTS.md` 的 `CLAUDE.md` | `CLAUDE.md`，並展開匯入內容 |

**哪些檔案會抑制 AGENTS.md。** Claude Code 會檢查工作目錄及其所有上層目錄中是否有 `CLAUDE.md`、`.claude/CLAUDE.md` 或 `CLAUDE.local.md`。只要找到其中任何一個，就不會讀取 `AGENTS.md`。`~/.claude/CLAUDE.md`、受管理的 CLAUDE.md 與 `.claude/rules/` **不**計入此檢查，並會與最終採用的專案檔案一同持續載入。

> **警告**：新增 `CLAUDE.local.md` 會在不知不覺中讓 `AGENTS.md` 不再被讀取。若您在建立本地覆寫後，專案指令似乎消失了，通常就是這個原因。

**會讀取哪些內容。** 工作目錄及其上層的每個 `AGENTS.md` 與 `.claude/AGENTS.md` 都會在工作階段開始時載入；子目錄中的檔案則會在 Claude 讀取該處檔案時按需載入。`@path` 匯入的展開方式與 CLAUDE.md 相同，且 `claudeMdExcludes` 同樣適用。

**永遠不會讀取的內容**：`AGENTS.local.md`、`AGENTS.override.md`，以及 `.agents/` 底下的任何內容。

**選擇行為。** **Project instructions** 設定有四個值：

| 值 | 效果 |
|-------|--------|
| `claude-md-or-agents-md` | 預設 — 僅在找不到 CLAUDE.md 檔案時讀取 `AGENTS.md` |
| `claude-md-and-agents-md` | 兩者同時存在時都會讀取 |
| `claude-md` | 僅讀取 CLAUDE.md 檔案；忽略 `AGENTS.md` |
| `managed-only` | 僅讀取管理政策指令 |

可透過 `/config` 或在設定中進行設定：

```jsonc
{
  "pluginConfigs": {
    "agents-md@builtin": {
      "options": {
        "instructionFiles": "claude-md-and-agents-md"
      }
    }
  }
}
```

此鍵僅在使用者與管理設定中有效 — 在專案或本地設定中設定不會有任何效果。

**無法直接讀取的情況。** 在 v2.1.277 之前的版本、在 Bedrock/Vertex/Foundry 上、停用遙測時、升級後的第一個工作階段期間、在 `disableAllHooks` 或 `allowManagedHooksOnly` 下，或停用內建的 `agents-md` 外掛時，Claude Code 不會自行讀取 `AGENTS.md`。在這些情況下，請從 CLAUDE.md 匯入它：

```markdown
@AGENTS.md
```

## 使用 `claudeMdExcludes` 排除 CLAUDE.md 檔案

在大型 monorepo 中，某些 CLAUDE.md 檔案可能與您目前的工作無關。`claudeMdExcludes` 設定讓您可以跳過特定的 CLAUDE.md 檔案，使其不會被載入至上下文：

```jsonc
// In ~/.claude/settings.json or .claude/settings.json
{
  "claudeMdExcludes": [
    "packages/legacy-app/CLAUDE.md",
    "vendors/**/CLAUDE.md"
  ]
}
```

模式將會與相對於專案根目錄的路徑進行比對。這對於以下情況特別有用：

- 包含許多子專案的 monorepo，其中只有部分專案與目前工作相關
- 包含廠商提供或第三方 CLAUDE.md 檔案的儲存庫
- 透過排除過時或無關的指令，減少 Claude 上下文視窗中的雜訊

## 設定檔層級結構

Claude Code 的設定（包括 `autoMemoryDirectory`、`claudeMdExcludes` 以及其他設定）依優先順序解析 — 與上述的 CLAUDE.md 檔案不同，設定是真正的覆寫而非串接。當同一項設定出現在多個範圍時，較高層級者勝出：

| 層級 | 位置 | 範圍 |
|-------|----------|-------|
| 1 (最高) | 管理 — `managed-settings.json`、plist/登錄檔或伺服器管理 | 整個組織的強制執行；無法被覆寫 |
| 2 | 命令列參數 | 暫時性的工作階段覆寫 |
| 3 | `.claude/settings.local.json` | 本地覆寫 (git-ignored) |
| 4 | `.claude/settings.json` | 專案層級 (已提交至 git) |
| 5 (最低) | `~/.claude/settings.json` | 使用者偏好 |

管理設定也支援放在 `managed-settings.json` 旁的 drop-in 目錄 `managed-settings.d/`：先合併基礎檔案，再依字母順序將 drop-in 目錄中的 `*.json` 檔案合併於其上（純量值覆寫、陣列串接並去除重複、物件深度合併）。這讓不同團隊可以部署各自獨立的政策片段，而不必編輯共用檔案。請注意，這是 **settings.json** 的機制，而非 CLAUDE.md 的機制 — 它不適用於上述的 CLAUDE.md 檔案位置。

權限規則（`allow`/`ask`/`deny`）的行為與其他設定不同：它們會跨範圍合併，而不是由較高層級取代較低層級。

**平台特定設定 (v2.1.51+)：**

設定也可以透過以下方式進行設定：
- **macOS**: Property list (plist) 檔案
- **Windows**: Windows Registry

這些平台原生機制會與 JSON 設定檔一同讀取，並遵循相同的優先權規則。

> **注意 (v2.1.119)**：`/config` 的變更現在會持久化至 `~/.claude/settings.json`。透過 `/config` 寫入的值會參與上述正常的政策/本地/專案優先順序鏈，不再僅限於當前工作階段。請使用 `/config` 進行互動式編輯，並直接編輯 `settings.json` 檔案來進行腳本化或受管理的設定。

### 保留與清除設定

| 設定 | 類型 | 預設值 | 說明 |
|---------|------|---------|-------------|
| `cleanupPeriodDays` | 整數（天） | 30 | 磁碟上暫存物件的保留期限。**自 v2.1.117 起**，適用於以下四項：checkpoints（`~/.claude/checkpoints/`）、tasks（`~/.claude/tasks/`）、shell-snapshots（`~/.claude/shell-snapshots/`）以及 backups（`~/.claude/backups/`）。超出保留期限的檔案會在啟動時自動清除。 |

```jsonc
// ~/.claude/settings.json
{
  "cleanupPeriodDays": 14
}
```

### 署名、語音與 PR URL 設定

| 設定 | 類型 | 說明 |
|---------|------|-------------|
| `attribution.commit` | boolean | 在 Claude 建立的 commit 中加入 `Co-Authored-By: Claude` 標記。取代已廢棄的 `includeCoAuthoredBy` 旗標。 |
| `attribution.pr` | boolean | 在 pull request 說明中加入 Claude 署名。取代針對 PR 的已廢棄 `includeCoAuthoredBy` 旗標。 |
| `attribution.sessionUrl` | boolean | 在網頁版與 Remote Control 工作階段中建立的 commit 與 PR 中省略 claude.ai 工作階段連結（v2.1.183+）。 |
| `voice.enabled` | boolean | 啟用按住說話（push-to-talk）語音輸入（`/voice`）。取代已廢棄的 `voiceEnabled` 旗標。 |
| `prUrlTemplate` | string | **v2.1.119 新增。** 自訂頁尾 PR 徽章的 URL 範本；適用於 GitLab、Bitbucket 或內部 code review 平台。支援 `{{owner}}`、`{{repo}}` 與 `{{number}}` 佔位符。 |

```jsonc
// ~/.claude/settings.json
{
  "attribution": {
    "commit": false,
    "pr": true
  },
  "voice": {
    "enabled": true
  },
  "prUrlTemplate": "https://gitlab.internal/{{owner}}/{{repo}}/-/merge_requests/{{number}}"
}
```

#### 已廢棄的設定名稱

以下舊版設定鍵仍然有效，但已被廢棄。建議使用上方的替代設定。

| 已廢棄的鍵 | 替代設定 | 備注 |
|----------------|-------------|-------|
| `includeCoAuthoredBy` | `attribution.commit` / `attribution.pr` | 舊的單一旗標已拆分為獨立的 commit 與 PR 開關。舊版安裝的使用者可繼續使用舊鍵；新專案應使用巢狀格式。 |
| `voiceEnabled` | `voice.enabled` | 已整合至 `voice` 命名空間，以便未來新增更多語音相關選項。 |

## 模組化規則系統

使用 `.claude/rules/` 目錄結構來建立組織良好且特定於路徑的規則。規則可以在專案層級與使用者層級進行定義：

```
your-project/
├── .claude/
│   ├── CLAUDE.md
│   └── rules/
│       ├── code-style.md
│       ├── testing.md
│       ├── security.md
│       └── api/                  # 支援子目錄
│           ├── conventions.md
│           └── validation.md

~/.claude/
├── CLAUDE.md
└── rules/                        # 使用者層級規則（適用於所有專案）
    ├── personal-style.md
    └── preferred-patterns.md
```

規則會在 `rules/` 目錄及其所有子目錄中進行遞迴搜尋。位於 `~/.claude/rules/` 的使用者層級規則會在專案層級規則之前載入，這允許設定個人預設值，並由專案進行覆寫。

### 使用 YAML Frontmatter 的特定路徑規則

定義僅適用於特定檔案路徑的規則：

```markdown
---
paths: src/api/**/*.ts
---

# API 開發規則

- 所有 API 端點必須包含輸入驗證
- 使用 Zod 進行 schema 驗證
- 記錄所有參數與回應類型
- 為所有操作包含錯誤處理
```

**Glob 模式範例：**

- `**/*.ts` - 所有 TypeScript 檔案
- `src/**/*` - `src/` 下的所有檔案
- `src/**/*.{ts,tsx}` - 多種副檔名
- `{src,lib}/**/*.ts, tests/**/*.test.ts` - 多種模式

### 子目錄與符號連結 (Symlinks)

`.claude/rules/` 中的規則支援兩種組織特性：

- **子目錄**：規則會進行遞迴搜尋，因此您可以將它們組織到基於主題的資料夾中（例如：`rules/api/`、`rules/testing/`、`rules/security/`）。
- **符號連結 (Symlinks)**：支援透過符號連結在多個專案之間共享規則。例如，您可以從中央位置將共用的規則檔案符號連結到每個專案的 `.claude/rules/` 目錄中。

## 記憶位置表

CLAUDE.md 檔案與規則會串接至上下文中，而不是透過嚴格覆寫來選擇 — 下方的「載入順序」指的是*在上下文中出現的位置*，而不是*哪一個勝出*。Auto memory 是一套獨立的機制，擁有自己的儲存位置。

| 位置 | 類型 | 載入順序 | 共用 | 存取方式 | 最適合用於 |
|----------|------|-------------|--------|--------|----------|
| `/Library/Application Support/ClaudeCode/CLAUDE.md` (macOS) | 管理政策 | 第 1（最先載入） | 組織 | 系統 | 全公司政策 |
| `/etc/claude-code/CLAUDE.md` (Linux/WSL) | 管理政策 | 第 1（最先載入） | 組織 | 系統 | 組織標準 |
| `C:\Program Files\ClaudeCode\CLAUDE.md` (Windows) | 管理政策 | 第 1（最先載入） | 組織 | 系統 | 公司指南 |
| `~/.claude/rules/*.md` | 使用者規則 | 第 2 | 個人 | 檔案系統 | 個人規則 (所有專案) |
| `~/.claude/CLAUDE.md` | 使用者記憶 | 第 3 | 個人 | 檔案系統 | 個人偏好 (所有專案) |
| `./.claude/rules/*.md` | 專案規則 | 第 4 | 團隊 | Git | 特定路徑的模組化規則 |
| `./CLAUDE.md` 或 `./.claude/CLAUDE.md` | 專案記憶 | 第 5 | 團隊 | Git | 團隊標準、共用架構 |
| `./CLAUDE.local.md` | 專案本地 | 第 6（最後載入） | 個人 | Git (已忽略) | 個人專案特定偏好 |
| `~/.claude/projects/<project>/memory/` | 自動記憶 | 不適用 — 獨立機制 | 個人 | 檔案系統 | Claude 的自動筆記與學習內容 |

## 記憶更新生命週期

以下是記憶更新在您的 Claude Code 工作階段中的流向：

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as File System
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: Open ~/.claude/settings.json
    Claude->>Memory: Write to ./CLAUDE.md
    Memory-->>Claude: File saved
    Claude->>Claude: Load updated memory
    Claude-->>User: "Memory saved!"
```

## Auto Memory

Auto memory 是一個持久化的目錄，Claude 在處理您的專案時，會自動在此記錄學習心得、模式與洞察。與您手動撰寫並維護的 CLAUDE.md 檔案不同，auto memory 是由 Claude 在工作階段期間自動寫入的。

### Auto Memory 如何運作

- **位置**：`~/.claude/projects/<project>/memory/`
- **進入點**：`MEMORY.md` 作為 auto memory 目錄中的主要檔案
- **主題檔案**：針對特定主題的選用額外檔案（例如：`debugging.md`、`api-conventions.md`）
- **載入行為**：在工作階段開始時，會將 `MEMORY.md` 的前 200 行（或前 25KB，以先到者為準）載入至上下文。主題檔案則是根據需求載入，而非在啟動時載入。
- **讀取/寫入**：Claude 在工作階段期間會隨著發現模式與專案特定知識，進行記憶檔案的讀取與寫入。
- **Frontmatter**：以 YAML frontmatter 開頭的檔案會獲得 `modified` 欄位 — 這是 Claude Code 每次寫入該檔案時記錄的 ISO 8601 時間戳記（v2.1.214）

### Auto Memory 架構

```mermaid
graph TD
    A["Claude Session Starts"] --> B["Load MEMORY.md<br/>(first 200 lines / 25KB)"]
    B --> C["Session Active"]
    C --> D["Claude discovers<br/>patterns & insights"]
    D --> E{"Write to<br/>auto memory"}
    E -->|General notes| F["MEMORY.md"]
    E -->|Topic-specific| G["debugging.md"]
    E -->|Topic-specific| H["api-conventions.md"]
    C --> I["On-demand load<br/>topic files"]
    I --> C

    style A fill:#e1f5fe,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e8f5e9,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#fff3e0,stroke:#333,color:#333
    style F fill:#fce4ec,stroke:#333,color:#333
    style G fill:#fce4ec,stroke:#333,color:#333
    style H fill:#fce4ec,stroke:#333,color:#333
    style I fill:#f3e5f5,stroke:#333,color:#333
```

### Auto Memory 目錄結構

```
~/.claude/projects/<project>/memory/
├── MEMORY.md              # 進入點 (啟動時載入前 200 行 / 25KB)
├── debugging.md           # 主題檔案 (根據需求載入)
├── api-conventions.md     # 主題檔案 (根據需求載入)
└── testing-patterns.md    # 主題檔案 (根據需求載入)
```

### 版本需求

Auto memory 需要 **Claude Code v2.1.59 或更高版本**。如果您使用的是舊版本，請先進行升級：

```bash
npm install -g @anthropic-ai/claude-code@latest
```

### 開啟或關閉 Auto Memory

Auto memory **預設為開啟**。它由 `autoMemoryEnabled` 設定（預設為 `true`）控制；設為 `false` 時，Claude 既不會讀取也不會寫入 auto memory 目錄。您也可以在工作階段中使用 `/memory` 切換它。

```json
{
  "autoMemoryEnabled": false
}
```

若要改用環境變數停用，請設定 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`。將其設為 `0` 則會強制**開啟** auto memory，即使 `--bare` 模式或 `autoMemoryEnabled: false` 原本會將其停用。

### 自訂 Auto Memory 目錄

預設情況下，auto memory 儲存在 `~/.claude/projects/<project>/memory/`。您可以使用 `autoMemoryDirectory` 設定來更改此位置（自 **v2.1.74** 起可用）：

```jsonc
// 在 ~/.claude/settings.json 或 .claude/settings.local.json 中 (僅限使用者/本地設定)
{
  "autoMemoryDirectory": "/path/to/custom/memory/directory"
}
```

> **注意**：`autoMemoryDirectory` 只能在使用者層級 (`~/.claude/settings.json`) 或本地設定 (`.claude/settings.local.json`) 中設定，不能在專案或受管制的政策設定中設定。

這在以下情況非常有用：

- 將 auto memory 儲存在共享或同步的位置
- 將 auto memory 與預設的 Claude 設定目錄分開
- 使用位於預設層級之外的專案特定路徑

### Worktree 與 Repository 共用

同一個 git repository 中的所有 worktree 與子目錄共用單一的自動記憶目錄。這意味著在不同的 worktree 之間切換，或是在同一個 repo 的不同子目錄中工作時，都會讀取並寫入相同的記憶檔案。

### Subagent 記憶

Subagents（透過 Task 或平行執行等工具產生的代理）可以擁有各自的記憶上下文。在 subagent 定義中使用 `memory` frontmatter 欄位來指定要載入哪些記憶範圍：

```yaml
memory: user      # 僅載入使用者層級的記憶
memory: project   # 僅載入專案層級的記憶
memory: local     # 僅載入本地記憶
```

這讓 subagents 能夠在專注的上下文下運作，而不是繼承完整的記憶層級。

> **注意**：Subagents 也可以維護各自的自動記憶。詳情請參閱 [official subagent memory documentation](https://code.claude.com/docs/en/sub-agents#enable-persistent-memory)。

### 控制自動記憶

可以透過 `CLAUDE_CODE_DISABLE_AUTO_MEMORY` 環境變數來控制自動記憶：

| 值 | 行為 |
|-------|----------|
| `0` | 強制開啟自動記憶 |
| `1` | 強制關閉自動記憶 |
| *(unset)* | 預設行為（啟用自動記憶） |

```bash
# 為一個工作階段停用自動記憶
CLAUDE_CODE_DISABLE_AUTO_MEMORY=1 claude

# 明確強制開啟自動記憶
CLAUDE_CODE_DISABLE_AUTO_MEMORY=0 claude
```

## 使用 `--add-dir` 新增目錄

`--add-dir` 旗標允許 Claude Code 從目前工作目錄之外的額外目錄載入 CLAUDE.md 檔案。這對於單一程式碼庫（monorepos）或需要其他目錄上下文的多專案設定非常有用。

要啟用此功能，請設定環境變數：

```bash
CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1
```

然後使用該旗標啟動 Claude Code：

```bash
claude --add-dir /path/to/other/project
```

Claude 將會從指定的額外目錄載入 CLAUDE.md，並與來自您目前工作目錄的記憶檔案一同處理。

## 實務範例

### 範例 1：專案記憶結構

**檔案：** `./CLAUDE.md`

```markdown
# Project Configuration

## Project Overview
- **Name**: E-commerce Platform
- **Tech Stack**: Node.js, PostgreSQL, React 18, Docker
- **Team Size**: 5 developers
- **Deadline**: Q4 2025

## Architecture
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## Development Standards

### Code Style
- Use Prettier for formatting
- Use ESLint with airbnb config
- Maximum line length: 100 characters
- Use 2-space indentation

### Naming Conventions
- **Files**: kebab-case (user-controller.js)
- **Classes**: PascalCase (UserService)
- **Functions/Variables**: camelCase (getUserById)
- **Constants**: UPPER_SNAKE_CASE (API_BASE_URL)
- **Database Tables**: snake_case (user_accounts)

### Git Workflow
- Branch names: `feature/description` or `fix/description`
- Commit messages: Follow conventional commits
- PR required before merge
- All CI/CD checks must pass
- Minimum 1 approval required

### Testing Requirements
- Minimum 80% code coverage
- All critical paths must have tests
- Use Jest for unit tests
- Use Cypress for E2E tests
- Test filenames: `*.test.ts` or `*.spec.ts`

### API Standards
- RESTful endpoints only
- JSON request/response
- Use HTTP status codes correctly
- Version API endpoints: `/api/v1/`
- Document all endpoints with examples

### Database
- Use migrations for schema changes
- Never hardcode credentials
- Use connection pooling
- Enable query logging in development
- Regular backups required

### Deployment
- Docker-based deployment
- Kubernetes orchestration
- Blue-green deployment strategy
- Automatic rollback on failure
- Database migrations run before deploy

## Common Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server |
| `npm test` | Run test suite |
| `npm run lint` | Check code style |
| `npm run build` | Build for production |
| `npm run migrate` | Run database migrations |

## Team Contacts
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## Known Issues & Workarounds
- PostgreSQL connection pooling limited to 20 during peak hours
- Workaround: Implement query queuing
- Safari 14 compatibility issues with async generators
- Workaround: Use Babel transpiler

## Related Projects
- Analytics Dashboard: `/projects/analytics`
- Mobile App: `/projects/mobile`
- Admin Panel: `/projects/admin`
```

### 範例 2：特定目錄的記憶

**檔案：** `./src/api/CLAUDE.md`

````markdown
# API 模組標準

此檔案補充根目錄的 CLAUDE.md，適用於 /src/api/ 中的所有內容。記憶檔案是串接而非覆寫
——根目錄的 CLAUDE.md 仍然適用，而 Claude Code 會在讀取此子樹中的檔案時
按需載入此檔案。

## API 特定標準

### 請求驗證
- 使用 Zod 進行 schema 驗證
- 務必驗證輸入內容
- 若驗證失敗，回傳 400 錯誤
- 包含欄位層級的錯誤詳情

### 身分驗證
- 所有端點皆需要 JWT token
- Token 放置於 Authorization header
- Token 在 24 小時後過期
- 實作 refresh token 機制

### 回應格式

所有回應必須遵循此結構：

```json
{
  "success": true,
  "data": { /* actual data */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

錯誤回應：
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 分頁
- 使用基於游標的分頁（而非 offset）
- 包含 `hasMore` 布林值
- 最大分頁大小限制為 100
- 預設分頁大小：20

### 速率限制 (Rate Limiting)
- 已驗證使用者每小時 1000 次請求
- 公開端點每小時 100 次請求
- 超過限制時回傳 429
- 包含 retry-after header

### 快取
- 使用 Redis 進行 session 快取
- 快取時長：預設 5 分鐘
- 在寫入操作時失效快取
- 為快取鍵加上資源類型的標籤
````

### 範例 3：個人記憶

**檔案：** `~/.claude/CLAUDE.md`

```markdown
# 我的開發偏好

## 關於我
- **經驗程度**：8 年全端開發經驗
- **偏好語言**：TypeScript, Python
- **溝通風格**：直接，並附帶範例
- **學習風格**：結合程式碼的視覺化圖表

## 程式碼偏好

### 錯誤處理
我偏好使用 try-catch 區塊進行明確的錯誤處理，並提供具備意義的錯誤訊息。
避免使用通用的錯誤訊息。務必記錄錯誤以利除錯。

### 註解
註解應說明「為什麼（WHY）」，而非「做了什麼（WHAT）」。程式碼應具備自我文件化特性。
註解應解釋業務邏輯或非顯而易見的決策。

### 測試
我偏好 TDD（測試驅動開發）。
先撰寫測試，再進行實作。
專注於行為，而非實作細節。

### 架構
我偏好模組化、鬆散耦合的設計。
使用相依性注入（dependency injection）以提升可測試性。
關注點分離（Controllers, Services, Repositories）。

## 除錯偏好
- 使用帶有前綴的 `console.log`：`[DEBUG]`
- 包含上下文：函式名稱、相關變數
- 在可用時使用堆疊追蹤（stack traces）
- 務必在記錄中包含時間戳記

## 溝通
- 使用圖表解釋複雜概念
- 在解釋理論之前，先展示具體範例
- 包含修改前後的程式碼片段
- 在最後總結重點

## 專案組織
我將專案組織如下：

   project/
   ├── src/
   │   ├── api/
   │   ├── services/
   │   ├── models/
   │   └── utils/
   ├── tests/
   ├── docs/
   └── docker/

## 工具
- **IDE**：VS Code 搭配 vim keybindings
- **Terminal**：Zsh 搭配 Oh-My-Zsh
- **Format**：Prettier (100 char line length)
- **Linter**：ESLint 搭配 airbnb config
- **Test Framework**：Jest 搭配 React Testing Library
```

_My Test_
Ask Claude to save a new rule

![Ask Claude to save a new rule](memory-ask-claude.png)
Claude has not save the rule because I did not have any `Claude.md` file anywhere. Then asked Claude to confirm the location.

![Ask Claude to save a new rule](memory-saved.png)

### 範例 4：工作階段期間的記憶更新

你可以在進行中的 Claude Code 工作階段期間，透過對話方式提出要求，將新規則加入記憶：

```markdown
User: Remember that I prefer using React hooks instead of class components
     for all new components.

Claude: I'm adding that to your memory. Which memory file should this go in?
        1. Project memory (./CLAUDE.md)
        2. Personal memory (~/.claude/CLAUDE.md)

User: Project memory

Claude: ✅ Memory saved!

Added to ./CLAUDE.md:
---

### 元件開發
- 使用搭配 React Hooks 的函式式元件
- 偏好使用 hooks 而非 class components
- 使用自訂 hooks 來處理可重複使用的邏輯
- 使用 useCallback 處理事件處理常式
- 使用 useMemo 處理耗時的運算
```

或使用 `/memory` 直接編輯記憶檔案，以進行大量更新或重新整理。

#### 新增記憶的技巧

- 規則應具體且具備可操作性
- 將相關規則歸類在同一個章節標題下
- 更新現有章節，而非重複內容
- 選擇適當的記憶範圍（專案 vs. 個人）

## 記憶功能比較

| 功能 | Claude Web/Desktop | Claude Code (CLAUDE.md) |
|---------|-------------------|------------------------|
| 自動綜合 | ✅ 每 24 小時 | ✅ 自動記憶 |
| 跨專案 | ✅ 共享 | ❌ 專案特定 |
| 團隊存取 | ✅ 共享專案 | ✅ Git 追蹤 |
| 可搜尋性 | ✅ 內建 | ✅ 透過 `/memory` |
| 可編輯性 | ✅ 在對話中 | ✅ 直接編輯檔案 |
| 匯入/匯出 | ✅ 是 | ✅ 複製/貼上 |
| 持久性 | ✅ 24 小時以上 | ✅ 無限期 |

### Claude Web/Desktop 中的記憶

#### 記憶綜合時間軸

```mermaid
graph LR
    A["第 1 天：使用者<br/>對話"] -->|24 小時| B["第 2 天：記憶<br/>綜合"]
    B -->|自動| C["記憶已更新<br/>摘要"]
    C -->|載入於| D["第 2 天-N 天：<br/>新對話"]
    D -->|新增至| E["記憶"]
    E -->|24 小時後| F["記憶重新整理"]
```

**記憶摘要範例：**

```markdown
## Claude 對使用者的記憶

### 專業背景
- 擁有 8 年經驗的高級全端開發人員
- 專注於 TypeScript/Node.js 後端與 React 前端
- 活躍的開源貢獻者
- 對 AI 與機器學習感興趣

### 專案上下文
- 目前正在開發電商平台
- 技術棧：Node.js, PostgreSQL, React 18, Docker
- 與 5 名開發人員組成的團隊合作
- 使用 CI/CD 與藍綠部署

### 溝通偏好
- 偏好直接、簡潔的解釋
- 喜歡視覺化圖表與範例
- 欣賞程式碼片段
- 在註解中說明業務邏輯

### 目前目標
- 提升 API 效能
- 將測試覆蓋率提高至 90%
- 實作快取策略
- 撰寫架構文件
```

## 最佳實務

### 該做的事 - 應包含的內容

- **具體且詳細**：使用清晰、詳細的指令，而非模糊的指導
  - ✅ 正確： 「所有 JavaScript 檔案請使用 2 個空格的縮排」
  - ❌ 應避免： 「遵循最佳實務」

- **保持條理分明**：使用清晰的 Markdown 章節與標題來建構記憶檔案

- **使用適當的層級**：
  - **Managed policy**：全公司政策、安全標準、合規性要求
  - **Project memory**：團隊標準、架構、程式碼規範（提交至 git）
  - **User memory**：個人偏好、溝通風格、工具選擇
  - **Directory memory**：特定模組的規則與覆寫

- **利用 imports**：使用 `@path/to/file` 語法來引用現有的文件
  - 遞迴匯入最大深度為 4 次跳轉（hops）
  - 避免在不同記憶檔案之間產生重複內容
  - 範例： `請參閱 @README.md 以了解專案概觀`

- **記錄常用指令**：將重複使用的指令記錄下來以節省時間

- **對專案記憶進行版本控制**：將專案層級的 CLAUDE.md 檔案提交至 git，以造福團隊

- **定期審查**：隨著專案演進與需求變更，定期更新記憶

- **提供具體範例**：包含程式碼片段與特定情境

### 不該做的事 - 應避免的內容

- **不要儲存秘密**：絕不要包含 API keys、密碼、token 或憑證

- **不要包含敏感資料**：不包含 PII（個人識別資訊）、私人資訊或專有秘密

- **不要重複內容**：改用 imports (`@path`) 來引用現有的文件

- **不要含糊不清**：避免使用如「遵循最佳實務」或「撰寫良好的程式碼」等籠統的陳述

- **不要寫得太長**：每個 CLAUDE.md 的目標是**少於 200 行**。較長的檔案仍會完整載入，但遵循度會下降 — 請參閱下方的 [保持 CLAUDE.md 精簡](#保持-claudemd-精簡)

- **不要過度組織**：策略性地使用層級；不要建立過多的子目錄覆寫

- **不要忘記更新**：過時的記憶可能會導致混淆與使用過時的實務

- **不要超過嵌套限制**：記憶 imports 最大深度為 4 次跳轉（hops）

### 保持 CLAUDE.md 精簡

Anthropic 目前的指引與「把所有東西都放進 CLAUDE.md」正好相反。這個檔案會在**每個**工作階段中載入，因此您加入的每一行，都會在與它無關的任務上爭奪注意力。

**經驗法則：讓 CLAUDE.md 保持在 200 行以內。** 較長的檔案仍會完整載入，但隨著檔案變大，指令遵循度會下降。

當檔案開始變長時，應將內容移出，而不是精簡文字：

| 內容 | 應放置的位置 | 原因 |
|---------|------------------|-----|
| 多步驟流程 | [skill](../03-skills/) | 按需載入，只在相關時才載入 |
| 特定目錄或檔案類型的規則 | 帶有 `paths:` frontmatter 的 `.claude/rules/*.md` | 以 glob 限定範圍；只在您處理符合的檔案時載入 |
| 參考資料與長篇範例 | skill 的 `references/` 目錄 | 只在 skill 需要時讀取 |
| Claude 應該記住的關於*您*的事 | Auto memory（預設開啟） | 自動寫入與載入 |

> **注意**：`@path` 匯入可以整理大型 CLAUDE.md，但**不會**節省上下文 — 匯入的檔案同樣會在載入時被拉入。拆分成限定路徑的規則，才是真正減少載入內容的方式。

`/doctor`（v2.1.206+）會檢查您的設定，並在 CLAUDE.md 膨脹到失去效用時提出精簡建議。

### 不要撰寫驗證提醒

較舊的指引鼓勵加入「在說完成之前一定要執行測試」或「再檢查一次你的工作」之類的句子。在 **Claude Opus 5 與 Fable 5 上，這些現在會造成過度驗證** — Claude 會重新檢查原本已經正確的工作，浪費回合與 token。

Anthropic 在 Claude 5 世代中移除了 Claude Code 自身系統提示詞超過 80% 的內容，且未測得任何退步。同樣的原則也適用於您的 CLAUDE.md：優先陳述目標，讓 Claude 自行判斷，而不是逐一列舉它應執行的檢查。

請從針對 Opus 5 或 Fable 5 的現有 CLAUDE.md 檔案中刪除驗證提醒。保留真正不明顯的專案需求 — 「整合測試需要 Docker 正在執行」是資訊，而不是提醒。

### 記憶管理技巧

**選擇正確的記憶層級：**

| 使用情境 | 記憶層級 | 理由 |
|----------|-------------|-----------|
| 公司安全政策 | Managed Policy | 適用於整個組織的所有專案 |
| 團隊程式碼風格指南 | Project | 透過 git 與團隊共享 |
| 您偏好的編輯器快捷鍵 | User | 個人偏好，不與他人共享 |
| API 模組標準 | Directory | 僅針對該特定模組 |

**快速更新工作流程：**

1. 單一規則：使用 `/memory` 開啟編輯器，或透過對話方式提出
2. 多項變更：使用 `/memory` 開啟編輯器
3. 初始設定：使用 `/init` 建立範本

**Import 最佳實務：**

```markdown
# 正確：引用現有文件
@README.md
@docs/architecture.md
@package.json

# 錯誤：複製其他地方已存在的內容
# 與其將 README 內容複製到 CLAUDE.md，不如直接引用它
```

## 安裝說明

### 設定專案記憶

#### 方法 1：使用 `/init` 命令（推薦）

設定專案記憶最快的方式：

1. **切換至您的專案目錄：**
   ```bash
   cd /path/to/your/project
   ```

2. **在 Claude Code 中執行 init 命令：**
   ```bash
   /init
   ```

3. **Claude 將會建立並填充 CLAUDE.md**，並包含範本結構。

4. **自訂產生的檔案**以符合您的專案需求。

5. **提交至 git：**
   ```bash
   git add CLAUDE.md
   git commit -m "Initialize project memory with /init"
   ```

#### 方法 2：手動建立

如果您偏好手動設定：

1. **在您的專案根目錄建立 CLAUDE.md：**
   ```bash
   cd /path/to/your/project
   touch CLAUDE.md
   ```

2. **加入專案標準：**
   ```bash
   cat > CLAUDE.md << 'EOF'
   # Project Configuration

   ## Project Overview
   - **Name**: Your Project Name
   - **Tech Stack**: List your technologies
   - **Team Size**: Number of developers

   ## Development Standards
   - Your coding standards
   - Naming conventions
   - Testing requirements
   EOF
   ```

3. **提交至 git：**
   ```bash
   git add CLAUDE.md
   git commit -m "Add project memory configuration"
   ```

### 設定個人記憶

1. **建立 ~/.claude 目錄：**
   ```bash
   mkdir -p ~/.claude
   ```

2. **建立個人 CLAUDE.md：**
   ```bash
   touch ~/.claude/CLAUDE.md
   ```

3. **加入您的偏好設定：**
   ```bash
   cat > ~/.claude/CLAUDE.md << 'EOF'
   # My Development Preferences

   ## About Me
   - Experience Level: [Your level]
   - Preferred Languages: [Your languages]
   - Communication Style: [Your style]

   ## Code Preferences
   - [Your preferences]
   EOF
   ```

### 設定特定目錄的記憶

1. **為特定目錄建立記憶：**
   ```bash
   mkdir -p /path/to/directory/.claude
   touch /path/to/directory/CLAUDE.md
   ```

2. **加入特定於該目錄的規則：**
   ```bash
   cat > /path/to/directory/CLAUDE.md << 'EOF'
   # [Directory Name] Standards

   This file supplements root CLAUDE.md for this directory. Memory files are
   concatenated, not overridden — Claude Code loads this file on demand when it
   reads files in this directory.

   ## [Specific Standards]
   EOF
   ```

3. **提交至版本控制：**
   ```bash
   git add /path/to/directory/CLAUDE.md
   git commit -m "Add [directory] memory configuration"
   ```

### 驗證設定

1. **檢查記憶位置：**
   ```bash
   # Project root memory
   ls -la ./CLAUDE.md

   # Personal memory
   ls -la ~/.claude/CLAUDE.md
   ```

2. **Claude Code 在啟動工作階段時**會自動載入這些檔案。

3. **使用 Claude Code 進行測試**，在您的專案中啟動一個新工作階段。

## 官方文件

欲獲取最新資訊，請參閱官方 Claude Code 文件：

- **[Memory Documentation](https://code.claude.com/docs/en/memory)** - 完整的記憶系統參考指南
- **[Slash Commands Reference](https://code.claude.com/docs/en/interactive-mode)** - 所有內建的斜線命令，包含 `/init` 與 `/memory`
- **[CLI Reference](https://code.claude.com/docs/en/cli-reference)** - 命令列介面文件

### 官方文件中的關鍵技術細節

**Memory Loading：**

- 當 Claude Code 啟動時，所有 memory 檔案都會自動載入
- Claude 會從目前的作業目錄向上遍歷，以尋找 CLAUDE.md 檔案
- 當存取子樹目錄時，會自動發現並根據上下文載入該目錄下的檔案

**Import Syntax：**

- 使用 `@path/to/file` 來包含外部內容（例如 `@~/.claude/my-project-instructions.md`）
- 同時支援相對路徑與絕對路徑（相對路徑是相對於包含該匯入的檔案來解析，而非工作目錄）
- 支援遞迴匯入，最大深度為 4 次跳轉（hops）
- 首次進行外部匯入時會觸發核准對話框
- 不會在 Markdown 的行內程式碼或程式碼區塊內進行評估
- 自動將引用的內容包含在 Claude 的上下文（context）中

**CLAUDE.md 載入順序**（串接至上下文，而非嚴格覆寫 — 請參閱上方的 [Claude Code 中的記憶層級](#claude-code-中的記憶層級)）：

1. Managed Policy（最先載入）
2. User-Level Rules (`~/.claude/rules/`)
3. User Memory
4. Project Rules (`.claude/rules/`)
5. Project Memory
6. Local Project Memory（最後載入）

Auto Memory 是一套獨立的機制（`~/.claude/projects/<project>/memory/`），不屬於此串接順序。

## 相關概念連結

### 整合點
- [MCP Protocol](../05-mcp/) - 與記憶並行的即時資料存取
- [Slash Commands](../01-slash-commands/) - 特定於工作階段（session）的快捷方式
- [Skills](../03-skills/) - 結合記憶上下文的自動化工作流程

### 相關 Claude 功能
- [Claude Web Memory](https://claude.ai) - 自動化綜合處理
- [Official Memory Docs](https://code.claude.com/docs/en/memory) - Anthropic 官方文件

---

**最後更新日期**：2026 年 9 月 19 日
**Claude Code 版本**：2.1.278
**來源**：
- https://code.claude.com/docs/en/memory
- https://code.claude.com/docs/en/memory#agents-md
**相容模型**：Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
