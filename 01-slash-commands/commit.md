---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*)
argument-hint: [message]
description: 建立包含上下文的 git commit
---

## Context

- 目前 git status: !`git status`
- 目前 git diff: !`git diff HEAD`
- 目前分支: !`git branch --show-current`
- 最近的 commits: !`git log --oneline -10`

## Your task

根據上述變更，建立單一的 git commit。

如果透過參數提供了訊息，請使用它：$ARGUMENTS

否則，請分析變更內容，並按照 conventional commits 格式建立適當的 commit 訊息：
- `feat:` 用於新功能
- `fix:` 用於修復 bug
- `docs:` 用於文件變更
- `refactor:` 用於程式碼重構
- `test:` 用於新增測試
- `chore:` 用於維護任務

---
**Last Updated**: April 9, 2026
