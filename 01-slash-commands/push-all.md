---
description: 暫存所有變更、建立 commit 並推送到遠端（請謹慎使用）
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---

# Commit and Push Everything

⚠️ **注意**：這會暫存所有變更、進行 commit 並推送到遠端。僅在確定所有變更都屬於同一個任務時才使用。

## 工作流程

### 1. 分析變更
並行執行：
- `git status` - 顯示修改/新增/刪除/未追蹤的檔案
- `git diff --stat` - 顯示變更統計資訊
- `git log -1 --oneline` - 顯示最近一次的 commit 以參考訊息風格

### 2. 安全檢查

**❌ 若偵測到以下內容，請停止並發出警告：**
- 敏感資訊：`.env*`, `*.key`, `*.pem`, `credentials.json`, `secrets.yaml`, `id_rsa`, `*.p12`, `*.pfx`, `*.cer`
- API Keys：任何包含真實數值的 `*_API_KEY`, `*_SECRET`, `*_TOKEN` 變數（非 `your-api-key`, `xxx`, `placeholder` 等佔位符）
- 大型檔案：`>10MB` 且未使用 Git LFS
- 建置產物：`node_modules/`, `dist/`, `build/`, `__pycache__/`, `*.pyc`, `.venv/`
- 暫存檔：`.DS_Store`, `thumbs.db`, `*.swp`, `*.tmp`

**API Key 驗證：**
檢查修改過的檔案中是否包含類似以下的模式：
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 偵測到真實金鑰！
AWS_SECRET_KEY=AKIA...         # ❌ 偵測到真實金鑰！
STRIPE_API_KEY=sk_live_...    # ❌ 偵測到真實金鑰！

# ✅ 可接受的佔位符：
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 驗證項目：**
- `.gitignore` 已正確配置
- 無合併衝突
- 分支正確（若是 main/master 請發出警告）
- API keys 僅為佔位符

### 3. 要求確認

呈現摘要：
```
📊 變更摘要：
- X 個檔案已修改，Y 個新增，Z 個刪除
- 總計：+AAA 插入，-BBB 刪除

🔒 安全性：✅ 無敏感資訊 | ✅ 無大型檔案 | ⚠️ [警告內容]
🌿 分支：[name] → origin/[name]

我將執行：git add . → commit → push

請輸入 'yes' 繼續，或輸入 'no' 取消。
```

**在繼續執行前，必須等待明確的 "yes" 指令。**

### 4. 執行（確認後）

依序執行：
```bash
git add .
git status  # 驗證暫存狀態
```

### 5. 生成 Commit 訊息

分析變更並建立符合 Conventional Commits 規範的訊息：

**格式：**
```
[type]: 簡短摘要 (最多 72 個字元)

- 關鍵變更 1
- 關鍵變更 2
- 關鍵變更 3
```

**類型：** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`

**範例：**
```
docs: Update concept README files with comprehensive documentation

- Add architecture diagrams and tables
- Include practical examples
- Expand best practices sections
```

### 6. Commit and Push

```bash
git commit -m "$(cat <<'EOF'
[生成的 commit 訊息]
EOF
)"
git push  # 若失敗：git pull --rebase && git push
git log -1 --oneline --decorate  # 驗證
```

### 7. 確認成功

```
✅ 已成功推送到遠端！

Commit: [hash] [message]
Branch: [branch] → origin/[branch]
檔案變更：X (+插入, -刪除)
```

## Error Handling

- **git add 失敗**：檢查權限、鎖定的檔案，並確認 repo 已初始化
- **git commit 失敗**：修復 pre-commit hooks，檢查 git config (user.name/email)
- **git push 失敗**：
  - Non-fast-forward：`git pull --rebase && git push`
  - 無遠端分支：`git push -u origin [branch]`
  - 受保護的分支：改用 PR workflow

## When to Use

✅ **適用情境：**
- 多檔案文件更新
- 包含測試與文件的功能開發
- 跨檔案的 Bug 修復
- 全專案範圍的格式化/重構
- 設定變更

❌ **應避免的情境：**
- 不確定提交的內容是什麼
- 包含 secrets/敏感資料
- 未經審核的受保護分支
- 存在合併衝突 (Merge conflicts)
- 需要細粒度的提交歷史
- Pre-commit hooks 失敗

## Alternatives

如果使用者想要更多控制權，可以建議：
1. **選擇性暫存 (Selective staging)**：審查/暫存特定檔案
2. **互動式暫存 (Interactive staging)**：使用 `git add -p` 進行 patch 選擇
3. **PR workflow**：建立分支 → push → PR（使用 `/pr` 命令）

**⚠️ 提醒**：在 push 之前務必審查變更。如有疑慮，請使用個別的 git 指令以獲得更多控制權。

---
**最後更新日期**：2026 年 4 月 9 日
