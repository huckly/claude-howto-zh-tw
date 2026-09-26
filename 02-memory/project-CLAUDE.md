# 專案設定

## 專案概觀
- **名稱**: 電商平台
- **技術堆疊**: Node.js, PostgreSQL, React 18, Docker
- **團隊規模**: 5 位開發人員
- **截止日期**: 2025 年第四季

## 架構
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## 開發標準

### 程式碼樣式
- 使用 Prettier 進行格式化
- 使用 ESLint 搭配 airbnb 設定
- 最大行長度：100 個字元
- 使用 2 個空格縮排

### 命名規則
- **檔案**: kebab-case (user-controller.js)
- **類別**: PascalCase (UserService)
- **函式/變數**: camelCase (getUserById)
- **常數**: UPPER_SNAKE_CASE (API_BASE_URL)
- **資料庫表格**: snake_case (user_accounts)

### Git 工作流程
- 分支名稱：`feature/description` 或 `fix/description`
- 提交訊息：遵循 conventional commits
- 合併前必須建立 PR
- 所有 CI/CD 檢查都必須通過
- 最低需要 1 項核准

### 測試需求
- 最低 80% 程式碼覆蓋率
- 所有關鍵路徑都必須有測試
- 使用 Jest 進行單元測試
- 使用 Cypress 進行 E2E 測試
- 測試檔案名稱：`*.test.ts` 或 `*.spec.ts`

### API 標準
- 僅限 RESTful 端點
- JSON 請求/回應
- 正確使用 HTTP 狀態碼
- API 端點版本化：`/api/v1/`
- 所有端點均須附範例說明文件

### 資料庫
- schema 變更須使用遷移腳本
- 永遠不要將憑證寫死在程式碼中
- 使用連線池
- 在開發環境中啟用查詢記錄
- 必須定期備份

### 部署
- 基於 Docker 的部署
- Kubernetes 容器編排
- 藍綠部署策略
- 失敗時自動回滾
- 部署前先執行資料庫遷移

## 常用命令

| 指令 | 用途 |
|---------|---------|
| `npm run dev` | 啟動開發伺服器 |
| `npm test` | 執行測試套件 |
| `npm run lint` | 檢查程式碼樣式 |
| `npm run build` | 建置正式環境版本 |
| `npm run migrate` | 執行資料庫遷移 |

## 團隊聯絡人
- 技術負責人: Sarah Chen (@sarah.chen)
- 產品經理: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## 已知問題與解決方案
- PostgreSQL 連線池在尖峰時段限制為 20
- 解決方案：實作查詢佇列
- Safari 14 與非同步產生器相容性問題
- 解決方案：使用 Babel 轉譯器

## 相關專案
- 分析儀表板: `/projects/analytics`
- 行動應用程式: `/projects/mobile`
- 管理面板: `/projects/admin`

---
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/memory
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
