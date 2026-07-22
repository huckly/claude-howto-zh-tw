---
description: 從原始碼建立全面的 API 文件
---

# API Documentation Generator

透過以下步驟產生 API 文件：

1. 掃描 `/src/api/` 中的所有檔案
2. 提取函式簽章與 JSDoc 註解
3. 依據端點/模組進行整理
4. 建立包含範例的 Markdown 檔案
5. 包含請求/回應的 schema
6. 加入錯誤文件

輸出格式：
- 位於 `/docs/api.md` 的 Markdown 檔案
- 為所有端點包含 curl 範例
- 加入 TypeScript 型別

---
**最後更新日期**：2026 年 4 月 9 日
