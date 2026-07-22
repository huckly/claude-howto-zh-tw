# Checkpoint 範例

在 Claude Code 中有效使用 checkpoint 的實際案例。

注意：每次使用者輸入提示詞時都會自動建立 checkpoint。您不需要手動儲存。若要回溯，請按兩次 `Esc` (`Esc+Esc`) 或使用 `/rewint` 開啟 checkpoint 瀏覽器。

## 範例 1：資料庫遷移

### 場景
您需要從 MySQL 遷移到 PostgreSQL，但想要同時探索「直接遷移」與「漸進式雙寫（dual-write）」兩種方案。

### 工作流程

```
User: Let's implement a direct migration from MySQL to PostgreSQL
      [自動建立 Checkpoint A]

Claude: I'll implement the direct migration...
[更改資料庫驅動程式、更新查詢語句、修改 schema]

User: Run the tests
      [自動建立 Checkpoint B]

Claude: Running tests...
❌ 15 tests failing - connection pool issues

User: This approach has issues. Let's try the dual-write pattern instead.
[使用者按 Esc+Esc，選擇 Checkpoint A，選擇 "Restore code and conversation"]

User: Now implement a dual-write pattern where we write to both databases

Claude: I'll implement dual-write migration...
[實作雙寫層、漸進式遷移策略]

User: Run the tests

Claude: Running tests...
✅ All tests passing

User: The dual-write approach is clearly safer. Let's commit this.
```

## 範例 2：效能優化

### 場景
需要嘗試多種效能優化策略。

### 工作流程

```
User: Let's optimize the API response time. Current: 450ms
      [自動建立 Checkpoint "Baseline"]

Claude: I'll add database query caching...
[實作 Redis 快取]

User: Run performance tests

Claude: Results: 280ms average response time (38% improvement)

User: Let me try a different approach.
[使用者按 Esc+Esc，選擇 "Baseline" checkpoint，選擇 "Restore code and conversation"]

User: Try database query optimization instead

Claude: I'll optimize database queries...
[新增索引、優化 N+1 查詢]

User: Run performance tests

Claude: Results: 180ms average response time (60% improvement)

User: Let me also try a third approach.
[使用者按 Esc+Esc，選擇 "Baseline" checkpoint，選擇 "Restore code and conversation"]

User: Try response compression and CDN

Claude: I'll add compression and CDN...
[實作 gzip、設定 CDN]

User: Run performance tests

Claude: Results: 320ms average response time (29% improvement)

User: Query optimization was the best. Let me go back and combine it with caching.
[使用者按 Esc+Esc，選擇優化查詢後的 checkpoint，選擇 "Restore code and conversation"]

User: Now add Redis caching on top of these optimized queries

Claude: [在優化後的查詢之上新增 Redis 快取]

User: Run performance tests

Claude: Results: 95ms average response time (79% improvement) 🎉

User: Perfect! This is the winner. Let's commit this.
```

## 範例 3：UI/UX 迭代

### 場景
嘗試不同的儀表板佈局。

### 工作流程

```
User: Let's redesign the dashboard with a sidebar layout
      [Checkpoint "Start" created automatically]

Claude: [Implements sidebar layout]

User: Show me a screenshot

Claude: [Shows screenshot]

User: The sidebar feels cramped. Let's try a top navigation instead.
[User presses Esc+Esc, selects "Start" checkpoint, chooses "Restore code and conversation"]

User: Try a top navigation layout

Claude: [Implements top navigation]

User: Show me a screenshot

Claude: [Shows screenshot]

User: Better, but let's try a card-based grid layout
[User presses Esc+Esc, selects "Start" checkpoint, chooses "Restore code and conversation"]

User: Try a card-based grid layout

Claude: [Implements card grid]

User: Show me a screenshot

Claude: [Shows screenshot]

User: This is great! But can we combine the top nav with the card grid?
[User presses Esc+Esc, selects the checkpoint after the card grid, chooses "Restore code and conversation"]

User: Add top navigation to the card grid layout

Claude: [Adds top navigation to card grid]

User: Perfect! This is the final design.
```

## 範例 4：除錯會話

### 場景
針對多個假設進行複雜的 bug 追蹤。

### 工作流程

```
User: There's a memory leak in the application
      [Checkpoint "Before debugging" created automatically]

Claude: Let me investigate...
[Analyzes code]

User: I think it's in the event listeners. Let's fix those.

Claude: [Fixes event listener cleanup]

User: Run memory profiler

Claude: Still leaking memory - event listeners weren't the issue

User: Let me go back and try a different hypothesis.
[User presses Esc+Esc, selects "Before debugging" checkpoint, chooses "Restore code and conversation"]

User: Maybe it's the database connections?

Claude: [Implements connection pooling fixes]

User: Run memory profiler

Claude: Still leaking - not the connections either

User: Let me go back and try another angle.
[User presses Esc+Esc, selects "Before debugging" checkpoint, chooses "Restore code and conversation"]

User: Let's check for circular references

Claude: [Finds and fixes circular references in cache]

User: Run memory profiler

Claude: ✅ Memory leak fixed! The circular references in the caching layer were the culprit.

User: Excellent! Let's commit this fix.
```

## 範例 5：API 設計演進

### 場景
進行多次迭代的 REST API 設計。

### 工作流程

```
User: 為我們的部落格平台建立一個 REST API
      [自動建立檢查點 "Before API design"]

Claude: [建立基礎的 CRUD API]

User: 這太簡單了。讓我們加入分頁與篩選功能

Claude: [加入分頁與篩選功能]

User: 我們需要更好的資源關聯性

Claude: [實作 HATEOAS 連結]

User: 事實上，讓我們改用 GraphQL 而不是 REST
[使用者按下 Esc+Esc，選擇 "Before API design" 檢查點，選擇 "Restore code and conversation"]

User: 改為為我們的部落格平台實作 GraphQL API

Claude: [實作 GraphQL API]

User: 我覺得 REST 更適合我們的使用案例。
[使用者按下 Esc+Esc，選擇 HATEOAS 之後的檢查點，選擇 "Restore code and conversation"]

User: 讓我們定案 REST 的做法。
```

## 範例 6：組態管理

### 場景
測試不同的組態管理方法。

### 工作流程

```
User: 將組態移至環境變數
      [自動建立檢查點 "Original config"]

Claude: [實作環境變數組態]

User: 在開發環境測試

Claude: 本地端運作正常，但生產環境部署時有問題

User: 讓我嘗試另一種方法。
[使用者按下 Esc+Esc，選擇 "Original config" 檢查點，選擇 "Restore code and conversation"]

User: 嘗試使用帶有驗證功能的組態檔案方法

Claude: [實作帶有 JSON schema 驗證的 YAML 組態]

User: 在開發環境測試

Claude: ✅ 運作良好，驗證錯誤訊息正確

User: 我們可以同時支援使用環境變數來處理敏感數值嗎？

Claude: [為 secrets 加入環境變數覆蓋功能]

User: 測試部署

Claude: ✅ 所有環境皆運作正常

User: 太棒了！這已經可以投入生產環境。
```

## 範例 7：測試策略

### 場景
實施全面的測試。

### 工作流程

```
User: Add unit tests for the auth module
      [Checkpoint created automatically]

Claude: [Adds Jest unit tests]

User: Run tests

Claude: 45 tests passing, 78% coverage

User: Add integration tests too

Claude: [Adds integration tests]

User: Run tests

Claude: 89 tests passing, 92% coverage, but tests are slow (3 minutes)

User: The tests are too slow. Let's optimize.

Claude: [Optimizes test setup, adds parallel execution]

User: Run tests

Claude: 89 tests passing, 92% coverage, 35 seconds ✅

User: Great! Now add E2E tests for critical paths

Claude: [Adds Playwright E2E tests]

User: Run all tests

Claude: 112 tests passing, 94% coverage, 2 minutes

User: Perfect balance of coverage and speed!
```

## 範例 8：使用 Summarize from Checkpoint

### 場景
在長時間的除錯會話之後，您想要在保留上下文的同時壓縮對話內容。

### 工作流程

```
User: [After 20+ messages of debugging and exploration]

[User presses Esc+Esc, selects an early checkpoint, chooses "Summarize from here"]
[Optionally provides instructions: "Focus on what we tried and what worked"]

Claude: [Generates a summary of the conversation from that point forward]
[Original messages are preserved in the transcript]
[The summary replaces the visible conversation, reducing context window usage]

User: Now let's continue with the approach that worked.
```

## 重點摘要

1. **Checkpoints 是自動化的**：每個使用者提示詞都會建立一個 checkpoint —— 不需要手動儲存
2. **使用 Esc+Esc 或 /rewind**：這是兩種存取 checkpoint 瀏覽器的方式
3. **選擇正確的還原選項**：根據您的需求選擇還原程式碼、對話、兩者皆是，或進行摘要
4. **不要害怕實驗**：Checkpoints 讓嘗試激進的變更變得安全
5. **與 git 結合使用**：使用 checkpoints 進行探索，使用 git 進行最終定案的工作
6. **摘要長會話**：使用 "Summarize from here" 來保持對話內容易於管理

---
**最後更新日期**：2026 年 4 月 9 日
