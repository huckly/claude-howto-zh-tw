<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# Checkpoints 與 Rewind

Checkpoints 讓您可以儲存對話狀態，並在您的 Claude Code 會話中回溯到先前的點。這對於探索不同的方法、從錯誤中恢復或比較替代方案來說非常寶貴。

## 概述

Checkpoints 讓您可以儲ว存對話狀態並回溯到先前的點，從而實現安全的實驗以及對多種方法的探索。它們是您對話狀態的快照，包含：
- 所有交換的訊息
- 所做的檔案修改
- 工具使用歷史
- 會話上下文

在探索不同方法、從錯誤中恢復或比較替代方案時，Checkpoints 非常有用。

## 核心概念

| Concept | Description |
|---------|-------------|
| **Checkpoint** | 對話狀態的快照，包含訊息、檔案與上下文 |
| **Rewind** | 回溯到先前的 checkpoint，並捨棄隨後的變更 |
| **Branch Point** | 用於探索多種方法的 checkpoint 分支點 |

## 存取 Checkpoints

您可以透過兩種主要方式存取與管理 checkpoints：

### 使用鍵盤快捷鍵
按兩次 `Esc` (`Esc` + `Esc`) 即可開啟 checkpoint 介面並瀏覽已儲存的 checkpoints。

### 使用斜線命令
使用 `/rewind` 命令（別名：`/checkpoint`）進行快速存取：

```bash
# Open rewind interface
/rewind

# Or use the alias
/checkpoint
```

## 回溯選項

當您進行回溯時，系統會顯示五個選項的選單：

1. **Restore code and conversation** -- 將檔案與訊息同時還原至該檢查點
2. **Restore conversation** -- 僅回溯訊息，保留您目前的程式碼不變
3. **Restore code** -- 僅還原檔案變更，保留完整的對話歷史
4. **Summarize from here** -- 將從此點開始的對話壓縮成由 AI 生成的摘要，以釋放上下文視窗空間。選定點之前的訊息將保持不變。磁碟上的檔案不會被更改。原始訊息會保留在會話紀錄中。您可以選擇提供指令，讓摘要集中在特定主題上。
5. **Never mind** -- 取消並返回目前狀態

> **注意**：在還原對話或進行摘要後，選定訊息中的原始提示詞會被還原到輸入欄位中，以便您可以重新發送或進行編輯。

## 自動檢查點

Claude Code 會自動為您建立檢查點：

- **每一次使用者提示詞** - 每次使用者輸入都會建立一個新的檢查點
- **持久性** - 檢查點會在不同會話之間持續存在
- **自動清理** - 檢查點會在 30 天後自動清理

這意味著您可以隨時回溯到對話中的任何先前點，從幾分鐘前到幾天前皆可。

## 使用情境

| 情境 | 工作流程 |
|----------|----------|
| **探索方法** | 儲存 → 嘗試 A → 儲存 → 回溯 → 嘗試 B → 比較 |
| **安全重構** | 儲存 → 重構 → 測試 → 若失敗：回溯 |
| **A/B 測試** | 儲存 → 設計 A → 儲存 → 回溯 → 設計 B → 比較 |
| **錯誤恢復** | 發現問題 → 回溯至最後一個良好的狀態 |

## 使用檢查點

### 查看與回溯

按兩次 `Esc` 或使用 `/rewind` 來開啟檢查點瀏覽器。您將會看到一個包含時間戳記的所有可用檢查點列表。選擇任何一個檢查點即可回溯到該狀態。

### 檢查點詳情

每個檢查點會顯示：
- 建立時的時間戳記
- 被修改的檔案
- 對話中的訊息數量
- 使用過的工具

## 實際範例

### 範例 1：探索不同的方法

```
User: Let's add a caching layer to the API

Claude: I'll add Redis caching to your API endpoints...
[Makes changes at checkpoint A]

User: Actually, let's try in-memory caching instead

Claude: I'll rewind to explore a different approach...
[User presses Esc+Esc and rewinds to checkpoint A]
[Implements in-memory caching at checkpoint B]

User: Now I can compare both approaches
```

### 範例 2：從錯誤中恢復

```
User: Refactor the authentication module to use JWT

Claude: I'll refactor the authentication module...
[Makes extensive changes]

User: Wait, that broke the OAuth integration. Let's go back.

Claude: I'll help you rewind to before the refactoring...
[User presses Esc+Esc and selects the checkpoint before the refactor]

User: Let's try a more conservative approach this time
```

### 範例 3：安全實驗

```
User: Let's try rewriting this in a functional style
[Creates checkpoint before experiment]

Claude: [Makes experimental changes]

User: The tests are failing. Let's rewind.
[User presses Esc+Esc and rewinds to the checkpoint]

Claude: I've rewound the changes. Let's try a different approach.
```

### 範例 4：分支方法

```
User: I want to compare two database designs
[Takes note of checkpoint - call it "Start"]

Claude: I'll create the first design...
[Implements Schema A]

User: Now let me go back and try the second approach
[User presses Esc+Esc and rewinds to "Start"]

Claude: Now I'll implement Schema B...
[Implements Schema B]

User: Great! Now I have both schemas to choose from
```

## Checkpoint 保留機制

Claude Code 會自動管理您的檢查點：

- 每次使用者輸入提示詞時都會自動建立檢查點
- 舊的檢查點最多會保留 30 天
- 系統會自動清理檢查點，以防止儲存空間無限增長

## 工作流程模式

### 探索用的分支策略

當您在嘗試多種不同的方法時：

```
1. 從初始實作開始 → Checkpoint A
2. 嘗試方法 1 → Checkpoint B
3. 回溯至 Checkpoint A
4. 嘗試方法 2 → Checkpoint C
5. 比較 B 與 C 的結果
6. 選擇最佳方法並繼續
```

### 安全重構模式

當進行重大變更時：

```
1. 目前狀態 → Checkpoint (自動)
2. 開始重構
3. 執行測試
4. 如果測試通過 → 繼續工作
5. 如果測試失敗 → 回溯並嘗試不同的方法
```

## 最佳實務

由於檢查點是自動建立的，您可以專注於工作，無需擔心手動儲存狀態。然而，請記住以下實務做法：

### 有效使用檢查點

✅ **建議做法：**
- 在回溯之前，先檢視可用的檢查點
- 當您想要探索不同方向時，使用回溯功能
- 保留檢查點以比較不同的方法
- 了解每個回溯選項的功能（還原程式碼與對話、僅還原對話、僅還原程式碼，或進行摘要）

❌ **不建議做法：**
- 僅依賴檢查點來保存程式碼
- 期望檢查點能追蹤外部檔案系統的變更
- 將檢查點當作 git commit 的替代品

## Configuration

Checkpoints 是 Claude Code 內建的預設行為，不需要任何配置即可啟用。每一次的使用者 prompt 都會自動建立一個 checkpoint。

唯一與 checkpoint 相關的設定是 `cleanupPeriodDays`，它控制會話與 checkpoint 的保留時間：

```json
{
  "cleanupPeriodDays": 30
}
```

- `cleanupPeriodDays`: 保留會話歷史與 checkpoint 的天數（預設值：`30`）

## Limitations

Checkpoints 具有以下限制：

- **Bash 命令變更不會被追蹤** - 在檔案系統上執行的 `rm`、`mv`、`cp` 等操作不會被記錄在 checkpoint 中
- **外部變更不會被追蹤** - 在 Claude Code 之外（例如在您的編輯器、終端機等）所做的變更不會被記錄
- **不能取代版本控制** - 請使用 git 對您的程式碼庫進行永久且可審核的變更

## Troubleshooting

### Missing Checkpoints

**問題**：找不到預期的 checkpoint

**解決方案**：
- 檢查 checkpoint 是否已被清除
- 檢查磁碟空間
- 確保 `cleanupPeriodDays` 設定得夠長（預設值：30 天）

### Rewind Failed

**問題**：無法回溯（rewind）至 checkpoint

**解決方案**：
- 確保沒有未提交的變更產生衝突
- 檢查 checkpoint 是否已損壞
- 嘗試回溯至另一個不同的 checkpoint

## Integration with Git

Checkpoints 是 git 的補充（而非取代）：

| 功能 | Git | Checkpoints |
|---------|-----|-------------|
| 範圍 | 檔案系統 | 對話 + 檔案 |
| 持久性 | 永久 | 基於會話 |
| 細粒度 | Commits | 任何時間點 |
| 速度 | 較慢 | 即時 |
| 分享 | 是 | 有限 |

同時使用兩者：
1. 使用 checkpoints 進行快速實驗
2. 使用 git commits 記錄最終確定的變更
3. 在進行 git 操作前建立 checkpoint
4. 將成功的 checkpoint 狀態 commit 到 git

## 快速入門指南

### 基本工作流程

1. **正常工作** - Claude Code 會自動建立檢查點 (checkpoints)
2. **想要回溯？** - 按兩次 `Esc` 或使用 `/rewind`
3. **選擇檢查點** - 從列表中選擇一個進行回溯
4. **選擇要還原的內容** - 從「還原程式碼與對話」、「僅還原對話」、「僅還原程式碼」、「從此處開始摘要」或「取消」中進行選擇
5. **繼續工作** - 您已回到該時間點

### 鍵盤快捷鍵

- **`Esc` + `Esc`** - 開啟檢查點瀏覽器
- **`/rewind`** - 存取檢查點的另一種方式
- **`/checkpoint`** - `/rewind` 的別名

## 判斷何時該回溯：上下文監控

檢查點讓您可以回溯 — 但您如何知道「何時」該這麼做？隨著對話內容增加，Claude 的上下文視窗 (context window) 會逐漸填滿，模型品質也會在不知不覺中下降。您可能會在沒有察覺的情況下，從一個「半盲」的模型中產出程式碼。

**[cc-context-stats](https://github.com/luongnv89/cc-context-stats)** 透過在您的 Claude Code 狀態列中加入即時的**上下文區域 (context zones)** 來解決這個問題。它會追蹤您在上下文視窗中所處的位置 — 從 **Plan**（綠色，適合進行規劃與寫程式）到 **Code**（黃色，應避免開始新的規劃），再到 **Dump**（橘色，應完成工作並進行回溯）。當您看到區域切換時，就知道該建立檢查點並重新開始，而不是帶著品質下降的輸出硬著頭皮繼續。

## 相關概念

- **[進階功能](../09-advanced-features/)** - 規劃模式與其他進階能力
- **[記憶管理](../02-memory/)** - 管理對話歷史與上下文
- **[斜線命令](../01-slash-commands/)** - 使用者觸發的快捷方式
- **[鉤子 (Hooks)](../06-hooks/)** - 事件驅動的自動化
- **[外掛 (Plugins)](../07-plugins/)** - 綑綁的擴充套件包

## 其他資源

- [官方 Checkpointing 文件](https://code.claude.com/docs/en/checkpointing)
- [進階功能指南](../09-advanced-features/) - 延伸思考與其他功能

## 摘要

Checkpoints 是 Claude Code 中的一項自動化功能，讓您可以安全地探索不同的方法，而無需擔心遺失工作成果。每一次的使用者 prompt 都會自動建立一個新的 checkpoint，因此您可以將會話回溯到之前的任何時間點。

核心優勢：
- 能夠無懼地嘗試多種不同的方法進行實驗
- 快速從錯誤中恢復
- 進行不同解決方案的並列比較
- 與版本控制系統安全地整合

請記住：checkpoints 並非 git 的替代品。請將 checkpoints 用於快速實驗，並將 git 用於永久性的程式碼變更。

---
**最後更新日期**：2026 年 4 月 16 日
**Claude Code 版本**：2.1.110
**來源**：
- https://code.claude.com/docs/en/checkpointing
**相容模型**：Claude Sonnet 4.6, Claude Opus 4.6, Claude Haiku 4.5
