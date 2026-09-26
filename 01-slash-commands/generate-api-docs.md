---
description: 從原始碼產生完整的 API 文件
---

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
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/commands
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
