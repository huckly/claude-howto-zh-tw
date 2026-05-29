allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [message]
description: 建立含上下文的 Git 提交
---

## 上下文

- 目前 git 狀態: !`git status`
- 目前 git diff: !`git diff HEAD`
- 目前分支: !`git branch --show-current`
- 最近的提交: !`git log --oneline -10`

## 你的任務

根據以上變更，建立一個單一的 Git 提交。

若透過參數傳入了 message，直接使用它：$ARGUMENTS

否則，分析這些變更，並依照 conventional commits 格式產生適當的提交訊息：
- `feat:` 新功能
- `fix:` 修復 bug
- `docs:` 文件變更
- `refactor:` 程式碼重構
- `test:` 新增測試
- `chore:` 維護任務

---
**Last Updated**: April 9, 2026
