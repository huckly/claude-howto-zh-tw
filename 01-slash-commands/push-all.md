---
description: 階段性地提交所有變更，建立 commit，並推送到遠端 (謹慎使用)
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git push:*), Bash(git diff:*), Bash(git log:*), Bash(git pull:*)
---

# 提交與推送所有內容

⚠️ **謹慎提醒**: 階段性地提交所有變更，建立 commit，並推送到遠端。僅在確信所有變更都應該一起提交時才使用。

## 工作流程

### 1. 分析變更
平行執行：
- `git status` - 顯示已修改/新增/刪除/未追蹤的檔案
- `git diff --stat` - 顯示變更統計資訊
- `git log -1 --oneline` - 顯示最近的 commit 以供訊息樣式參考

### 2. 安全性檢查

**❌ 停止並發出警告如果偵測到：**
- 密碼：`.env*`, `*.key`, `*.pem`, `credentials.json`, `secrets.yaml`, `id_rsa`, `*.p12`, `*.pfx`, `*.cer`
- API 金鑰：任何 `*_API_KEY`, `*_SECRET`, `*_TOKEN` 變數具有實際值（而不是佔位符，例如 `your-api-key`、`xxx`、`placeholder`）
- 大型檔案：`>10MB` 而沒有 Git LFS
- 構建工件：`node_modules/`, `dist/`, `build/`, `__pycache__/`, `*.pyc`, `.venv/`
- 臨時檔案：`.DS_Store`, `thumbs.db`, `*.swp`, `*.tmp`

**API 金鑰驗證：**
檢查已修改的檔案是否包含以下模式：
```bash
OPENAI_API_KEY=sk-proj-xxxxx  # ❌ 偵測到實際金鑰！
AWS_SECRET_KEY=AKIA...         # ❌ 偵測到實際金鑰！
STRIPE_API_KEY=sk_live_...    # ❌ 偵測到實際金鑰！

# ✅ 可接受的佔位符：
API_KEY=your-api-key-here
SECRET_KEY=placeholder
TOKEN=xxx
API_KEY=<your-key>
SECRET=${YOUR_SECRET}
```

**✅ 驗證：**
- `.gitignore` 已正確設定
- 沒有合併衝突
- 正確的分支 (如果為主要/主分支，則發出警告)
- API 金鑰僅為佔位符

### 3. 請求確認

呈現摘要：
```
📊 變更摘要：
- X 個檔案已修改，Y 個新增，Z 個刪除
- 總數：+AAA 插入，-BBB 刪除

🔒 安全性：✅ 沒有密碼 | ✅ 沒有大型檔案 | ⚠️ [警告]
🌿 分支：[名稱] → origin/[名稱]

我將：git add . → commit → push

輸入 'yes' 以繼續或 'no' 以取消。
```

**在繼續之前等待明確的 "yes"。**

### 4. 執行 (確認後)

順序執行：
```bash
git add .
git status  # 驗證階段性提交
```

### 5. 產生 commit 訊息

分析變更並建立符合慣例的 commit：

**格式：**
```
[類型]: 簡短摘要 (最多 72 個字元)

- 主要變更 1
- 主要變更 2
- 主要變更 3
```

**類型：** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`, `build`, `ci`

**範例：**
```
docs: 更新概念 README 檔案，包含全面的文件

- 新增架構圖表和表格
- 包含實用範例
- 擴充最佳實務部分
```

### 6. 提交與推送

```bash
git commit -m "$(cat <<'EOF'
[生成的 commit 訊息]
EOF
)"
git push  # 如果失敗：git pull --rebase && git push
git log -1 --oneline --decorate  # 驗證
```

### 7. 確認成功

```
✅ 成功推送至遠端！

Commit: [雜湊值] [訊息]
Branch: [分支] → origin/[分支]
已變更的檔案：X (+插入，-刪除)
```

## 錯誤處理

- **git add 失敗**: 檢查權限、鎖定檔案、驗證是否已初始化 Git 倉庫
- **git commit 失敗**: 修正預提交鉤子、檢查 git 設定 (user.name/email)
- **git push 失敗**:
  - 非快速前進：`git pull --rebase && git push`
  - 沒有遠端分支：`git push -u origin [branch]`
  - 保護分支：使用 PR 工作流程

## 何時使用

✅ **適合：**
- 多檔案文件更新
- 包含測試和文件的功能
- 跨檔案的錯誤修正
- 專案範圍的格式化/重構
- 設定變更

❌ **避免：**
- 不確定要提交什麼
- 包含機密/敏感資料
- 保護分支且未經過審查
- 存在合併衝突
- 想要粒度化的提交歷史
- 預提交鉤子失敗

## 替代方案

如果使用者想要控制，建議：
1. **選擇性暫存**: 審查/暫存特定檔案
2. **互動式暫存**: `git add -p` 用於修補程式選擇
3. **PR 工作流程**: 建立分支 → 提交 → PR (使用 `/pr` 斜線命令)

**⚠️ 提醒**: 提交前請務必審查變更。 如果猶豫不決，請使用個別 git 命令以獲得更多控制。

---
**上次更新**: 2026 年 4 月 9 日
