# Pull Request Preparation Checklist

在建立 PR 前，請執行以下步驟：

1. 執行 linting: `prettier --write .`
2. 執行測試: `npm test`
3. 檢視 git diff: `git diff HEAD`
4. 階段性新增變更: `git add .`
5. 建立符合 conventional commits 的 commit 訊息：
   - `fix:` 用於修正錯誤
   - `feat:` 用於新增功能
   - `docs:` 用於文件
   - `refactor:` 用於程式碼重構
   - `test:` 用於新增測試
   - `chore:` 用於維護

6. 產生 PR 摘要，包含：
   - 變更了什麼
   - 為什麼變更
   - 執行了哪些測試
   - 可能的影響

---
**上次更新**: 2026 年 4 月 9 日
