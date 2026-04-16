# API 文件產生器

透過以下方式產生 API 文件：

1. 掃描 `/src/api/` 中的所有檔案
2. 提取函式簽章和 JSDoc 註解
3. 依端點/模組整理
4. 產生 Markdown 文件，包含範例
5. 包含請求/回應 Schema
6. 增加錯誤文件

輸出格式：
- Markdown 檔案位於 `/docs/api.md`
- 為所有端點包含 curl 範例
- 增加 TypeScript 類型

---
**上次更新**: 2026 年 4 月 9 日
