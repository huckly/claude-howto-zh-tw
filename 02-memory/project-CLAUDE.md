# Project Configuration

## Project Overview
- **名稱**: E-commerce Platform
- **技術棧**: Node.js, PostgreSQL, React 18, Docker
- **團隊規模**: 5 位開發人員
- **截止日期**: 2025 年 Q4

## Architecture
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## Development Standards

### Code Style
- 使用 Prettier 進行格式化
- 使用帶有 airbnb 設定的 ESLint
- 最大行長：100 字元
- 使用 2 個空格縮排

### Naming Conventions
- **檔案**: kebab-case (user-controller.js)
- **類別**: PascalCase (UserService)
- **函式/變數**: camelCase (getUserById)
- **常數**: UPPER_SNAKE_CASE (API_BASE_URL)
- **資料庫資料表**: snake_case (user_accounts)

### Git Workflow
- 分支名稱：`feature/description` 或 `fix/description`
- Commit 訊息：遵循 conventional commits
- 合併前須提交 PR
- 所有 CI/CD 檢查必須通過
- 至少需要 1 個審核通過 (approval)

### Testing Requirements
- 最低 80% 程式碼覆蓋率
- 所有關鍵路徑必須包含測試
- 使用 Jest 進行單元測試
- 使用 Cypress 進行 E2E 測試
- 測試檔案名稱：`*.test.ts` 或 `*.spec.ts`

### API Standards
- 僅限使用 RESTful 端點
- JSON 請求/回應
- 正確使用 HTTP 狀態碼
- API 端點版本化：`/api/v1/`
- 為所有端點提供包含範例的說明文件

### Database
- 使用 migrations 進行結構變更
- 絕不將憑證寫死在程式碼中
- 使用連線池 (connection pooling)
- 在開發環境中啟用查詢日誌 (query logging)
- 需要定期備份

### Deployment
- 基於 Docker 的部署
- Kubernetes 編排
- Blue-green 部署策略
- 失敗時自動回滾 (rollback)
- 在部署前執行資料庫 migrations

## 常用命令

| 命令 | 用途 |
|---------|---------|
| `npm run dev` | 啟動開發伺服器 |
| `npm test` | 執行測試套件 |
| `npm run lint` | 檢查程式碼風格 |
| `npm run build` | 建置生產版本 |
| `npm run migrate` | 執行資料庫遷移 |

## 團隊聯絡人
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## 已知問題與解決方案
- PostgreSQL 連線池在尖峰時段限制為 20
- 解決方案：實作查詢佇列
- Safari 14 與 async generators 的相容性問題
- 解決方案：使用 Babel transpiler

## 相關專案
- Analytics Dashboard: `/projects/analytics`
- Mobile App: `/projects/mobile`
- Admin Panel: `/projects/admin`

---
**最後更新日期**：2026 年 4 月 9 日
