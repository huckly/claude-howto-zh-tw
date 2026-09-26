---
description: 清理程式碼、暫存變更，並準備 pull request
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(npm test:*), Bash(npm run lint:*)
---

# Pull Request 準備清單

在建立 PR 前，請執行以下步驟：

1. 執行 linting: `prettier --write .`
2. 執行測試: `npm test`
3. 檢視 git diff: `git diff HEAD`
4. 暫存變更: `git add .`
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
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/commands
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
