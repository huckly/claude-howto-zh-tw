# Planning Mode 範例

展示在 Claude Code 中有效使用 planning mode 的實際案例。

## 範例 1：建立 REST API

### 沒有使用 Planning Mode

```
User: Build a REST API for a blog

Claude: I'll create the API...
[立即開始寫程式，可能會遺漏需求]
```

### 使用 Planning Mode

```
User: /plan Build a REST API for a blog

Claude: Let me create a comprehensive plan for the blog API.

## Blog REST API Implementation Plan

### Phase 1: Project Setup (15 min)
1. Initialize Node.js project with Express
2. Set up TypeScript configuration
3. Install dependencies (express, pg, jwt, joi)
4. Configure environment variables
5. Set up project structure

### Phase 2: Database Layer (30 min)
6. Design database schema
   - users table (id, email, password_hash, created_at)
   - posts table (id, user_id, title, content, published, created_at, updated_at)
   - comments table (id, post_id, user_id, content, created_at)
7. Create migration files
8. Set up database connection pool
9. Implement database models

### Phase 3: Authentication (45 min)
10. Implement user registration endpoint
11. Implement login endpoint with JWT
12. Create authentication middleware
13. Add password hashing with bcrypt
14. Implement token refresh logic

### Phase 4: Posts API (40 min)
15. GET /api/posts - List all published posts (with pagination)
16. GET /api/posts/:id - Get single post
17. POST /api/posts - Create new post (auth required)
18. PUT /api/posts/:id - Update post (auth required, ownership check)
19. DELETE /api/posts/:id - Delete post (auth required, ownership check)
20. GET /api/users/:id/posts - Get user's posts

### Phase 5: Comments API (30 min)
21. GET /api/posts/:id/comments - Get post comments
22. POST /api/posts/:id/comments - Add comment (auth required)
23. DELETE /api/comments/:id - Delete comment (auth required, ownership check)

### Phase 6: Validation & Error Handling (30 min)
24. Add request validation with Joi
25. Implement global error handler
26. Add input sanitization
27. Implement rate limiting

### Phase 7: Testing (60 min)
28. Write unit tests for models
29. Write integration tests for auth endpoints
30. Write integration tests for posts endpoints
31. Write integration tests for comments endpoints
32. Achieve 80%+ code coverage

### Phase 8: Documentation & Deployment (30 min)
33. Generate API documentation with Swagger
34. Create README with setup instructions
35. Add Docker configuration
36. Set up CI/CD pipeline

**Estimated Time**: 4.5 hours
**Files to Create**: ~25
**Key Technologies**: Express, PostgreSQL, JWT, TypeScript

**API Endpoints Summary**:
- Authentication: 2 endpoints
- Posts: 6 endpoints
- Comments: 3 endpoints
- Total: 11 endpoints

Ready to proceed? (yes/no/modify)

User: yes

Claude: Starting Phase 1: Project Setup...
[系統性地執行每個階段]
```

## 範例 2：資料庫遷移

### 使用規劃模式 (Planning Mode)

```
User: /plan 從 MongoDB 遷移至 PostgreSQL

Claude: 這是一個複雜的遷移任務。讓我建立一個詳細的計畫。

## MongoDB 遷移至 PostgreSQL 計畫

### 第一階段：分析與準備 (第 1 天)
1. 分析目前的 MongoDB schema
2. 識別資料關聯性與約束條件
3. 記錄目前的資料量與成長率
4. 設計對應的 PostgreSQL schema
5. 規劃資料類型映射 (mapping)
6. 識別潛在的遷移挑戰

### 第二階段：PostgreSQL 設定 (第 1 天)
7. 設定 PostgreSQL 資料庫
8. 建立包含資料表、索引、約束條件的 schema
9. 設定連線池 (connection pooling)
10. 配置備份策略

### 第三階段：雙寫 (Dual-Write) 實作 (第 2-3 天)
11. 建立資料庫抽象層
12. 實作同時寫入兩個資料庫的雙寫機制
13. 加入資料一致性檢查
14. 將雙寫機制部署至 staging 環境
15. 監控問題

### 第四階段：資料遷移 (第 4 天)
16. 建立 MongoDB 的資料匯出腳本
17. 建立 PostgreSQL 的資料匯入腳本
18. 實作資料轉換邏輯
19. 在生產資料的副本上執行初步遷移
20. 驗證遷移後的資料完整性

### 第五階段：逐步切換 (第 5-7 天)
21. 在生產環境啟用雙寫機制
22. 分批遷移歷史資料
23. 持續驗證資料一致性
24. 同時監控兩個資料庫
25. 修復任何資料同步問題

### 第六階段：將 PostgreSQL 設為主資料庫 (第 8 天)
26. 將讀取操作切換至 PostgreSQL
27. 監控查詢效能
28. 優化慢查詢
29. 為求安全，繼續維持雙寫機制

### 第七階段：停用 MongoDB (第 9-10 天)
30. 停止寫入 MongoDB
31. 將 MongoDB 作為備份保留 2 週
32. 從程式碼中移除 MongoDB 的依賴
33. 封存 MongoDB 資料
34. 停用 MongoDB 實例

**預估時間**：10 天
**風險等級**：高
**回滾策略**：維持 MongoDB 運行並保持雙寫機制
**成功標準**：
- 零資料遺失
- 效能退化 <5%
- 所有測試通過

**關鍵風險**：
1. 遷移過程中的資料不一致
2. 查詢效能下降
3. 應用程式停機時間
4. 資料遺失

**緩解措施**：
- 使用雙寫模式確保安全
- 逐步切換
- 持續監控
- 具備快速回滾能力

是否繼續執行此計畫？(yes/no/modify)

User: Modify - 為每個階段增加回滾計畫

Claude: 已更新計畫，包含每個階段的回滾程序...
```

## 範例 3：前端重構

```
User: /plan 將我們的 React app 從 class components 重構為 hooks

Claude: 我將建立一個系統性的重構計畫。

## React Class to Hooks 遷移計畫

### 第一階段：準備與分析 (2 小時)
1. 審查所有 class components (尋找所有 .jsx/.tsx 檔案)
2. 依複雜度分類：
   - 簡單 (僅含 state): ~15 個組件
   - 中等 (含生命週期方法): ~23 個組件
   - 複雜 (多個生命週期、refs): ~8 個組件
3. 識別需要更新的第三方函式庫
4. 建立組件依賴圖
5. 規劃遷移順序 (從葉節點開始)

### 第二階段：設定與工具 (1 小時)
6. 將 React 更新至 v18+
7. 設定 hooks 的 ESLint 規則
8. 建立轉換範本
9. 設定自動化測試
10. 在開始前建立檢查點

### 第三階段：遷移簡單組件 (4 小時)
11. 轉換簡單的有狀態組件 (15 個組件)
12. 將 this.state 替換為 useState
13. 將 this.setState 替換為 state setters
14. 轉換後測試每個組件
15. 更新測試

### 第四階段：遷移中等組件 (8 小時)
16. 轉換含有生命週期方法的組件 (23 個組件)
17. 將 componentDidMount 替換為 useEffect
18. 將 componentDidUpdate 替換為 useEffect
19. 將 componentWillUnmount 替換為 useEffect cleanup
20. 進行徹底測試

### 第五階段：遷移複雜組件 (12 小時)
21. 轉換複雜組件 (8 個組件)
22. 將 refs 替換為 useRef
23. 提取自定義 hooks 以處理共用邏輯
24. 處理邊際情況 (多個 useEffect)
25. 廣泛測試

### 第六階段：共用邏輯提取 (6 小時)
26. 識別組件間重複的邏輯
27. 建立自定義 hooks：
    - useAuth
    - useFetch
    - useLocalStorage
    - useDebounce
28. 重構組件以使用自定義 hooks
29. 獨立測試自定義 hooks

### 第七階段：效能優化 (4 小時)
30. 在需要時加入 React.memo
31. 使用 useCallback 進行優化
32. 使用 useMemo 進行優化
33. 分析並修復效能退化

### 第八階段：清理與文件化 (2 小時)
34. 移除未使用的 class component 工具函式
35. 更新文件
36. 更新編碼規範
37. 為團隊建立遷移指南

**預估時間**: 5 天 (39 小時)
**需修改的檔案**: 46 個組件
**風險等級**: 中等

**遷移順序**:
1. 葉組件 (無依賴項)
2. 中層組件
3. 高層容器組件
4. App.jsx (最後)

**測試策略**:
- 每個組件的單元測試
- 使用者流程的整合測試
- 視覺回歸測試
- 效能基準測試

**成功指標**:
- 所有測試通過
- 無視覺回歸
- 效能維持或提升
- Bundle size 未顯著增加

是否繼續？(yes/no/modify)
```

## 範例 4：安全性實作

```
User: /plan 為我們的 Web 應用程式實作全面的安全性

Claude: 讓我建立一個詳盡的安全性實作計畫。

## Web 應用程式安全性實作計畫

### 第一階段：安全性稽核 (第 1 天)
1. 執行自動化安全性掃描器 (npm audit, Snyk)
2. 審查依賴項是否存在漏洞
3. 檢查是否符合 OWASP Top 10
4. 稽核身分驗證流程
5. 審查資料處理實務
6. 記錄目前的安全性缺口

### 第二階段：身分驗證強化 (第 2-3 天)
7. 實作強密碼要求
8. 加入密碼強度計量器
9. 實作登入速率限制 (rate limiting)
10. 加入登入失敗後的帳號鎖定機制
11. 實作 2FA (TOTP)
12. 加入會話管理改進
13. 實作安全的密碼重設流程

### 第三階段：授權與存取控制 (第 3-4 天)
14. 實作 RBAC (基於角色的存取控制)
15. 在所有端點加入權限檢查
16. 實作最小權限原則
17. 為敏感操作加入稽核日誌
18. 實作資源層級的權限控制

### 第四階段：資料保護 (第 4-5 天)
19. 實作靜態加密 (敏感欄位)
20. 確保所有通訊皆使用 TLS/SSL
21. 實作安全的會話儲存
22. 在日誌中加入 PII 資料遮罩
23. 實作資料保留政策
24. 加入安全的檔案上傳處理

### 第五階段：輸入驗證與清理 (第 5-6 天)
25. 在所有端點實作輸入驗證
26. 加入 XSS 防護 (內容安全政策 Content Security Policy)
27. 實作 SQL 注入防護 (參數化查詢)
28. 加入 CSRF 防護
29. 實作請求大小限制
30. 加入檔案上傳類型驗證

### 第六階段：安全性標頭與配置 (第 6 天)
31. 加入安全性標頭：
    - Strict-Transport-Security
    - X-Content-Type-Options
    - X-Frame-Options
    - X-XSS-Protection
    - Content-Security-Policy
32. 正確配置 CORS
33. 停用不必要的 HTTP 方法
34. 移除伺服器版本標頭

### 第七階段：監控與日誌 (第 7 天)
35. 實作安全性事件日誌記錄
36. 加入異常檢測
37. 設定入侵檢測
38. 實作即時告警
39. 建立安全性儀表板

### 第八階段：測試與文件 (第 8-9 天)
40. 進行滲透測試
41. 執行安全性掃描器
42. 測試所有安全性功能
43. 建立安全性文件
44. 對團隊進行安全性實務培訓
45. 建立事件應變計畫

### 第九階段：合規性與政策 (第 10 天)
46. GDPR 合規性檢查
47. 建立隱私權政策
48. 實作資料主體權利
49. 加入 Cookie 同意機制
50. 建立安全性政策文件

**預估時間**：10 天
**風險等級**：極高 (Critical)
**優先級**：高 (High)

**安全性改進摘要**：
- 身分驗證：7 項增強
- 授權：5 項增強
- 資料保護：6 項增強
- 輸入驗證：6 項增強
- 基礎設施：9 項增強
- 監控：5 項增強

**合規性**：OWASP Top 10, GDPR

**成功標準**：
- 零個關鍵漏洞
```

- 已緩解所有 OWASP Top 10 風險
- 安全評分 >90/100
- 所有滲透測試問題已解決

是否繼續？(yes/no/modify)
```

## Example 5: Performance Optimization

```
User: /plan 優化我們緩慢的電子商務網站

Claude: 讓我建立一個全面的效能優化計畫。

## 電子商務效能優化計畫

### Phase 1: Performance Audit (4 hours)
1. 執行 Lighthouse 稽核
2. 分析 Chrome DevTools 效能設定檔
3. 測量 Core Web Vitals：
   - LCP (Largest Contentful Paint)
   - FID (First Input Delay)
   - CLS (Cumulative Layout Shift)
4. 識別效能瓶頸
5. 建立基準效能指標

**Current Metrics**:
- LCP: 4.2s (target: <2.5s)
- FID: 280ms (target: <100ms)
- CLS: 0.25 (target: <0.1)
- Page Load: 8.3s (target: <3s)

### Phase 2: Image Optimization (6 hours)
6. 將圖片轉換為 WebP 格式
7. 實作響應式圖片
8. 為圖片加入延遲載入 (lazy loading)
9. 優化圖片大小（壓縮）
10. 為圖片實作 CDN
11. 加入圖片佔位符 (placeholders)

**Expected Impact**: -40% load time

### Phase 3: Code Splitting & Lazy Loading (8 hours)
12. 實作基於路由的程式碼分割 (code splitting)
13. 延遲載入非關鍵組件
14. 分割 vendor bundles
15. 優化 chunk 大小
16. 實作動態匯入 (dynamic imports)
17. 為關鍵資源加入預載 (preloading)

**Expected Impact**: -30% initial bundle size

### Phase 4: Caching Strategy (6 hours)
18. 實作瀏覽器快取 (Cache-Control)
19. 加入 service worker 以支援離線功能
20. 實作 API 回應快取
21. 為資料庫查詢加入 Redis 快取
22. 實作 stale-while-revalidate
23. 設定 CDN 快取

**Expected Impact**: -50% API response time

### Phase 5: Database Optimization (8 hours)
24. 加入資料庫索引 (indexes)
25. 優化慢查詢 (>100ms)
26. 實作查詢結果快取
27. 加入連線池 (connection pooling)
28. 在適當情況下進行反正規化 (Denormalize)
29. 實作資料庫讀取副本 (read replicas)

**Expected Impact**: -60% database query time

### Phase 6: Frontend Optimization (10 hours)
30. 最小化並壓縮 JavaScript
31. 最小化並壓縮 CSS
32. 移除未使用的 CSS (PurgeCSS)
33. 實作關鍵 CSS (critical CSS)
34. 延遲執行非關鍵 JavaScript
35. 減少 DOM 大小
36. 優化 React 渲染 (memo, useMemo)
37. 為長列表實作虛擬捲動 (virtual scrolling)

**Expected Impact**: -35% JavaScript execution time

### Phase 7: Network Optimization (4 hours)
38. 啟用 HTTP/2
39. 實作資源提示 (preconnect, prefetch)
40. 減少 HTTP 請求數量
41. 啟用 Brotli 壓縮
42. 優化第三方腳本

**Expected Impact**: -25% network time

### Phase 8: Monitoring & Testing (4 hours)
43. 設定效能監控 (Datadog/New Relic)
44. 加入真實使用者監測 (RUM)
45. 建立效能預算 (performance budgets)
46. 設定自動化 Lighthouse CI
47. 在真實裝置上進行測試

**Estimated Time**: 50 hours (2 weeks)

**Target Metrics** (90th percentile):
- LCP: <2.0s (from 4.2s) ✅
- FID: <50ms (from 280ms) ✅
- CLS: <0.05 (from 0.25) ✅
- Page Load: <2.5s (from 8.3s) ✅

**Expected Revenue Impact**:

- 100ms 加快 = 1% 轉換率提升
- 目標：改善 5.8s = 約 58% 轉換率提升
- 預估額外營收：顯著

**優先順序**：
1. 圖片優化 (快速獲益)
2. Code splitting (高影響力)
3. Caching (高影響力)
4. 資料庫優化 (關鍵)
5. 前端優化 (完善)

是否按照此計畫進行？(yes/no/modify)

## 重點摘要

### Planning Mode 的優點

1. **清晰度**：在開始前擁有明確的路線圖
2. **估算**：時間與工作量估算
3. **風險評估**：及早識別潛在問題
4. **優先順序**：任務的邏輯順序
5. **審核**：在執行前進行審查與核准
6. **修改**：根據回饋調整計畫

### 何時使用 Planning Mode

✅ **務必使用於**：
- 多日期的專案
- 團隊協作
- 關鍵系統變更
- 學習新概念
- 複雜的重構

❌ **不要使用於**：
- Bug fixes
- 微小的調整
- 簡單的查詢
- 快速實驗

### 最佳實務

1. 在核准前**仔細審查計畫**
2. 當發現問題時**修改計畫**
3. 將複雜任務**拆解**
4. **估算現實的**時間範圍
5. **包含回滾**策略
6. **加入成功**標準
7. 為每個階段**規劃測試**

---
**最後更新日期**：2026 年 4 月 9 日
