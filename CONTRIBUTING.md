<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# 參與 Claude How To 的貢獻

感謝您有興趣參與此專案！本指南將幫助您了解如何進行有效的貢獻。

## 關於此專案

Claude How To 是一個視覺化、範例驅動的 Claude Code 指南。我們提供：
- **Mermaid 圖表**：解釋功能運作方式
- **生產就緒的範本**：您可以立即使用的範本
- **真實世界的範例**：包含上下文與最佳實務
- **漸進式學習路徑**：從初學者到進階學習者

## 貢獻類型

### 1. 新範例或範本
為現有功能（斜線命令、技能、鉤子等）添加範例：
- 可直接複製貼上的程式碼
- 對於運作方式的清晰解釋
- 使用案例與優點
- 除錯技巧

### 2. 文件改進
- 釐清令人困惑的章節
- 修復打字錯誤與語法問題
- 補充缺失的資訊
- 改進程式碼範例

### 3. 功能指南
為新的 Claude Code 功能建立指南：
- 逐步教學
- 架構圖
- 常見模式與反模式
- 真實世界的工作流程

### 4. Bug 回報
回報您遇到的問題：
- 描述您的預期結果
- 描述實際發生的情況
- 包含重現步驟
- 加上相關的 Claude Code 版本與 OS

### 5. 回饋與建議
協助改進本指南：
- 提出更好的解釋建議
- 指出內容覆蓋範圍的缺口
- 建議新的章節或重新組織架構

## 入門指南

### 1. Fork 與 Clone
```bash
git clone https://github.com/luongnv89/claude-howto.git
cd claude-howto
```

### 2. 建立分支
使用具描述性的分支名稱：
```bash
git checkout -b add/feature-name
git checkout -b fix/issue-description
git checkout -b docs/improvement-area
```

### 3. 設定您的環境

Pre-commit 鉤子會在每次提交前，於本地執行與 CI 相同的檢查。所有四項檢查都必須通過，PR 才會被接受。

**必要的依賴項目：**

```bash
# Python 工具（uv 是此專案的套件管理員）
pip install uv
uv venv
source .venv/bin/activate
uv pip install -r scripts/requirements-dev.txt

# Markdown linter (Node.js)
npm install -g markdownlint-cli

# Mermaid 圖表驗證器 (Node.js)
npm install -g @mermaid-js/mermaid-cli

# 安裝 pre-commit 並啟用鉤子
uv pip install pre-commit
pre-commit install
```

**驗證您的設定：**

```bash
pre-commit run --all-files
```

每次提交時執行的鉤子如下：

| 鉤子 | 檢查內容 |
|------|---------------|
| `markdown-lint` | Markdown 格式與結構 |
| `cross-references` | 相對連結、錨點、程式碼區塊 |
| `mermaid-syntax` | 所有 ` ```mermaid ` 區塊是否正確解析 |
| `link-check` | 外部 URL 是否可存取 |
| `build-epub` | EPUB 生成是否無錯誤（於 `.md` 變更時） |

## 目錄結構

```
├── 01-slash-commands/      # 使用者呼叫的捷徑
├── 02-memory/              # 持久化上下文範例
├── 03-skills/              # 可重複使用的能力
├── 04-subagents/           # 專業的 AI 助手
├── 05-mcp/                 # Model Context Protocol 範例
├── 06-hooks/               # 事件驅動自動化
├── 07-plugins/             # 綑綁功能
├── 08-checkpoints/         # 會話快照
├── 09-advanced-features/   # 規劃、思考、背景
├── 10-cli/                 # CLI 參考
├── scripts/                # 建置與工具腳本
└── README.md               # 主指南
```

## 如何進行貢獻範例

### 新增斜線命令
1. 在 `01-slash-commands/` 中建立一個 `.md` 檔案
2. 內容須包含：
   - 功能的清晰描述
   - 使用情境
   - 安裝說明
   - 使用範例
   - 自訂技巧
3. 更新 `01-slash-commands/README.md`

### 新增技能
1. 在 `03-skills/` 中建立一個目錄
2. 內容須包含：
   - `SKILL.md` - 主要文件
   - `scripts/` - 若有需要，包含輔助腳本
   - `templates/` - 提示詞範本
   - README 中的使用範例
3. 更新 `03-skills/README.md`

### 新增子代理
1. 在 `04-subagents/` 中建立一個 `.md` 檔案
2. 內容須包含：
   - 代理的目的與能力
   - 系統提示詞結構
   - 範例使用情境
   - 整合範例
3. 更新 `04-subagents/README.md`

### 新增 MCP 配置
1. 在 `05-mcp/` 中建立一個 `.json` 檔案
2. 內容須包含：
   - 配置說明
   - 必要的環境變數
   - 設定說明
   - 使用範例
3. 更新 `05-mcp/README.md`

### 新增鉤子
1. 在 `06-hooks/` 中建立一個 `.sh` 檔案
2. 內容須包含：
   - Shebang 與描述
   - 解釋邏輯的清晰註解
   - 錯誤處理
   - 安全性考量
3. 更新 `06-hooks/README.md`

## 寫作指南

### Markdown 風格
- 使用清晰的標題（H2 用於章節，H3 用於子章節）
- 保持段落簡短且集中
- 使用項目符號列出清單
- 包含帶有語言指定的程式碼區塊
- 在章節之間加入空白行

### 程式碼範例
- 確保範例可以被直接複製貼上使用
- 為非顯而易見的邏輯加上註解
- 同時包含簡單與進階版本
- 展示真實世界的使用情境
- 標示出潛在問題

### 文件說明
- 解釋「為什麼」而不僅僅是「是什麼」
- 包含前置條件
- 加入疑難排解章節
- 連結至相關主題
- 保持對初學者友善

### JSON/YAML
- 使用正確的縮排（一致使用 2 或 4 個空格）
- 加入解釋配置的註解
- 包含驗證範例

### 圖表
- 盡可能使用 Mermaid
- 保持圖表簡單且易讀
- 在圖表下方加入描述
- 連結至相關章節

## Commit 指引

遵循 Conventional Commit 格式：
```
type(scope): description

[optional body]
```

類型：
- `feat`: 新功能或範例
- `fix`: Bug 修復或更正
- `docs`: 文件變更
- `refactor`: 程式碼重構
- `style`: 格式變更
- `test`: 新增或修改測試
- `chore`: 建置、依賴項目等

範例：
```
feat(slash-commands): Add API documentation generator
docs(memory): Improve personal preferences example
fix(README): Correct table of contents link
docs(skills): Add comprehensive code review skill
```

## 提交前

### Checklist
- [ ] 程式碼符合專案風格與慣例
- [ ] 新範例包含清晰的文件說明
- [ ] README 檔案已更新（包含本地與根目錄）
- [ ] 無敏感資訊（API keys、憑證）
- [ ] 範例已測試且運作正常
- [ ] 連結已驗證且正確
- [ ] 檔案具有正確的權限（腳本具備執行權限）
- [ ] Commit 訊息清晰且具描述性

### 本地測試
```bash
# 執行所有 pre-commit 檢查（與 CI 檢查相同）
pre-commit run --all-files

# 查看你的變更
git diff
```

## Pull Request 流程

1. **建立具有清晰描述的 PR**：
   - 這增加了什麼或修復了什麼？
   - 為什麼需要這個變更？
   - 相關的 issue（若有）

2. **包含相關細節**：
   - 新功能？請包含使用情境
   - 文件？請說明改進之處
   - 範例？請展示修改前後的對比

3. **連結至 issue**：
   - 使用 `Closes #123` 來自動關閉相關的 issue

4. **耐心等待審核**：
   - 維護者可能會提出改進建議
   - 根據回饋進行迭代
   - 最終決定權歸維護者所有

## Code Review 流程

審閱者將檢查：
- **準確性**：是否如描述般運作？
- **品質**：是否已達生產就緒標準？
- **一致性**：是否遵循專案模式？
- **文件**：是否清晰且完整？
- **安全性**：是否存在任何漏洞？

## 回報問題

### Bug 報告
請包含：
- Claude Code 版本
- 作業系統
- 重現步驟
- 預期行為
- 實際行為
- 若適用，請附上螢幕截圖

### 功能需求
請包含：
- 使用案例或正在解決的問題
- 建議的解決方案
- 您已考慮過的替代方案
- 額外的上下文

### 文件問題
請包含：
- 哪些部分令人困惑或缺失
- 建議的改進事項
- 範例或參考資料

## 專案政策

### 敏感資訊
- 切勿提交 API keys、tokens 或憑證
- 在範例中使用佔位符數值
- 為設定檔包含 `.env.example`
- 記錄所需的環境變數

### 程式碼品質
- 保持範例集中且具備可讀性
- 避免過度設計解決方案
- 為非顯而易見的邏輯加入註解
- 在提交前進行徹底測試

### 智慧財產權
- 原創內容歸作者所有
- 專案使用教育授權
- 尊重現有的版權
- 在需要時提供歸屬說明

## 尋求協助

- **問題**：在 GitHub Issues 中發起討論
- **一般協助**：查閱現有文件
- **開發協助**：檢視類似的範例
- **Code Review**：在 PR 中標記維護者

## 鳴謝

貢獻者將在以下地方獲得認可：
- README.md 的 Contributors 區塊
- GitHub contributors 頁面
- Commit 歷史紀錄

## Security

在貢獻範例與文件時，請遵循安全編碼實務：

- **絕不將密鑰或 API keys 硬編碼** - 請使用環境變數
- **提醒安全影響** - 強調潛在風險
- **使用安全預設值** - 預設啟用安全功能
- **驗證輸入** - 展示正確的輸入驗證與清理（sanitization）
- **包含安全筆記** - 記錄安全考量事項

關於安全問題，請參閱 [SECURITY.md](SECURITY.md) 以了解我們的漏洞回報流程。

## Code of Conduct

我們致力於提供一個友善且具包容性的社群。請閱讀 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) 以了解完整的社群準則。

簡而言之：
- 保持尊重與包容
- 優雅地接受回饋
- 幫助他人學習與成長
- 避免騷擾或歧視
- 向維護者回報問題

所有貢獻者皆應遵守此準則，並以友善與尊重對待彼此。

## License

透過向本專案進行貢獻，即表示您同意您的貢獻將根據 MIT License 進行授權。詳情請參閱 [LICENSE](LICENSE) 檔案。

## Questions?

- 查看 [README](README.md)
- 審閱 [LEARNING-ROADMAP.md](LEARNING-ROADMAP.md)
- 查看現有的範例
- 開啟一個 issue 以進行討論

感謝您的貢獻！ 🙏

---
**最後更新日期**：2026 年 4 月 9 日
