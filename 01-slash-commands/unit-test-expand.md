---
name: Expand Unit Tests
description: 透過針對未測試的分支與邊際情況來增加測試覆蓋率
tags: testing, coverage, unit-tests
---

# Expand Unit Tests

擴充現有的單元測試，並使其符合專案的測試框架：

1. **分析覆蓋率**：執行覆蓋率報告以識別未測試的分支、邊際情況以及低覆蓋率區域
2. **找出缺口**：審查程式碼中的邏輯分支、錯誤路徑、邊界條件、null/空輸入
3. **使用專案框架編寫測試**：
   - Jest/Vitest/Mocha (JavaScript/TypeScript)
   - pytest/unittest (Python)
   - Go testing/testify (Go)
   - Rust test framework (Rust)
4. **針對特定情境**：
   - 錯誤處理與異常
   - 邊界值 (min/max, empty, null)
   - 邊際情況 (edge cases) 與極端情況 (corner cases)
   - 狀態轉移與副作用
5. **驗證改進結果**：再次執行覆蓋率報告，確認覆蓋率有可衡量的提升

僅呈現新的測試程式碼區塊。請遵循現有的測試模式與命名慣例。

---
**Last Updated**: April 9, 2026
