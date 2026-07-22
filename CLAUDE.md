# CLAUDE.md

此檔案為 Claude Code (claude.ai/code) 在此儲存庫中處理程式碼時提供指導。

## 專案概觀

Claude How To 是一個針對 Claude Code 功能的教學儲存庫。這是 **documentation-as-code**（文件即程式碼）—— 主要產出是整理成編號學習模組的 Markdown 檔案，而非可執行的應用程式。

**架構**：每個模組 (01-10) 涵蓋特定的 Claude Code 功能，並包含可直接複製貼上的範本、Mermaid 圖表與範例。建置系統會驗證文件品質並生成 EPUB 電子書。

## 常用命令

### Pre-commit 品質檢查

所有文件在提交（commit）之前必須通過四項品質檢查（這些檢查透過 pre-commit 鉤子自動執行）：

```bash
# 安裝 pre-commit 鉤子（在每次提交時執行）
pre-commit install

# 手動執行所有檢查
pre-commit run --all-files
```

這四項檢查分別為：
1. **markdown-lint** — 透過 `markdownlint` 檢查 Markdown 結構與格式
2. **cross-references** — 檢查內部連結、錨點、程式碼區塊語法（Python 腳本）
3. **mermaid-syntax** — 驗證所有 Mermaid 圖表是否能正確解析（Python 腳本）
4. **link-check** — 檢查外部 URL 是否可連通（Python 腳本）
5. **build-epub** — 確保 EPUB 生成過程無錯誤（在 `.md` 檔案變更時執行）

### 開發環境設定

```bash
# 安裝 uv (Python 套件管理員)
pip install uv

# 建立虛擬環境並安裝 Python 依賴項目
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# 安裝 Node.js 工具 (markdown linter 與 Mermaid 驗證器)
npm install -g markdownlint-cli
npm install -g @mermaid-js/mermaid-cli

# 安裝 pre-commit 鉤子
uv pip install pre-commit
pre-commit install
```

### 測試

`scripts/` 中的 Python 腳本包含單元測試：

```bash
# 執行所有測試
pytest scripts/tests/ -v

# 帶有覆蓋率報告執行
pytest scripts/tests/ -v --cov=scripts --cov-report=html

# 執行特定測試
pytest scripts/tests/test_build_epub.py -v
```

### 程式碼品質

```bash
# 檢查並格式化 Python 程式碼
ruff check scripts/
ruff format scripts/

# 安全掃描
bandit -c scripts/pyproject.toml -r scripts/ --exclude scripts/tests/

# 型別檢查
mypy scripts/ --ignore-missing-imports
```

### EPUB 建置

```bash
# 生成電子書 (透過 Kroki.io API 渲染 Mermaid 圖表)
uv run scripts/build_epub.py

# 使用選項
uv run scripts/build_epub.py --verbose --output custom-name.epub --max-concurrent 5
```

## 目錄結構

```
├── 01-slash-commands/      # 使用者呼叫的捷徑
├── 02-memory/              # 持久化上下文範例
├── 03-skills/              # 可重複使用的能力
├── 04-subagents/           # 專業化的 AI 助手
├── 05-mcp/                 # Model Context Protocol 範例
├── 06-hooks/               # 事件驅動自動化
├── 07-plugins/             # 組合功能
├── 08-checkpoints/         # 會話快照
├── 09-advanced-features/   # 規劃、思考、背景
├── 10-cli/                 # CLI 參考
├── scripts/
│   ├── build_epub.py           # EPUB 生成器 (透過 Kroki API 渲染 Mermaid)
│   ├── check_cross_references.py   # 驗證內部連結
│   ├── check_links.py          # 檢查外部 URL
│   ├── check_mermaid.py        # 驗證 Mermaid 語法
│   └── tests/                  # 腳本的單元測試
├── .pre-commit-config.yaml    # 品質檢查定義
└── README.md               # 主指南 (同時也是模組索引)
```

## 內容指南

### 模組結構
每個編號資料夾皆遵循以下模式：
- **README.md** — 功能概覽與範例
- **範例檔案** — 可直接複製貼上的範本（`.md` 用於指令、`.json` 用於設定、`.sh` 用於鉤子）
- 檔案依據功能複雜度與依賴關係進行組織

### Mermaid 圖表
- 所有圖表必須能成功解析（由 pre-commit 鉤子檢查）
- EPUB 建置透過 Kroki.io API 渲染圖表（需要網路連線）
- 使用 Mermaid 繪製流程圖、時序圖與架構視覺圖

### 交叉引用
- 內部連結請使用相對路徑（例如：`(01-slash-commands/README.md)`）
- 程式碼區塊必須指定語言（例如：` ```bash `, ` ```python `）
- 錨點連結使用 `#heading-name` 格式

### 連結驗證
- 外部 URL 必須可以存取（由 pre-commit 鉤子檢查）
- 避免連結至暫時性的內容
- 盡可能使用永久連結 (permalinks)

## 關鍵架構重點

1. **數字資料夾表示學習順序** — 01-10 前綴代表學習 Claude Code 功能的建議順序。此編號是刻意設計的；請勿按字母順序重新排列。

2. **腳本是工具，而非產品本身** — `scripts/` 中的 Python 腳本是用於支援文件品質與 EPUB 生成。實際內容位於帶有數字編號的模組資料夾中。

3. **Pre-commit 是守門員** — 在 PR 被接受之前，必須通過所有四項品質檢查。CI pipeline 會進行第二次檢查，執行相同的檢查流程。

4. **Mermaid 渲染需要網路** — EPUB 建置過程會呼叫 Kroki.io API 來渲染圖表。此處的建置失敗通常是網路問題或無效的 Mermaid 語法。

5. **這是一個教學，而非函式庫** — 在新增內容時，請專注於清晰的解釋、可直接複製貼上的範例以及視覺化圖表。其價值在於教學概念，而非提供可重複使用的程式碼。

## Commit 慣例

請遵循 conventional commit 格式：
- `feat(slash-commands): Add API documentation generator`
- `docs(memory): Improve personal preferences example`
- `fix(README): Correct table of contents link`
- `refactor(hooks): Simplify hook configuration examples`

在適用情況下，Scope 應與資料夾名稱一致。
