---
description: 清理程式碼、暫存變更並準備 pull request
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# Pull Request 準備檢查清單

在建立 PR 之前，請執行以下步驟：

1. 執行 linting：`prettier --write .`
2. 執行測試：`npm test`
3. 查看 git diff：`git diff HEAD`
4. 暫存變更：`git add .`
5. 依照 conventional commits 建立 commit 訊息：
   - `fix:` 用於修復 bug
   - `feat:` 用於新功能
   - `docs:` 用於文件
   - `refactor:` 用於程式碼重構
   - `test:` 用於新增測試
   - `chore:` 用於維護工作

6. 生成 PR 摘要，包含：
   - 變更內容
   - 變更原因
   - 已執行的測試
   - 潛在影響

---
**最後更新日期**：2026 年 4 月 9 日
