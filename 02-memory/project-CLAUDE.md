# 專案配置

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
- PR 才能合併
- 所有 CI/CD 檢查都必須通過
- 最低需要 1 項批准

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
- 版本 API 端點：`/api/v1/`
- 使用範例記錄所有端點

### 資料庫
- 對於 schema 變更，使用遷移
- 永遠不要硬編碼憑證
- 使用連線池
- 在開發環境中啟用查詢記錄
- 定期備份必要

### 部署
- 基於 Docker 的部署
- Kubernetes 協調
- 藍綠部署策略
- 失敗時自動回滾
- 在部署前執行資料庫遷移

## 常用命令

| 指令 | 目的 |
|---------|---------|
| `npm run dev` | 啟動開發伺服器 |
| `npm test` | 執行測試套件 |
| `npm run lint` | 檢查程式碼樣式 |
| `npm run build` | 為生產環境建置 |
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
**上次更新**: 2026 年 4 月 9 日
