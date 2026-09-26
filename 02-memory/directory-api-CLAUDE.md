# API 模組標準

此檔案補充根 CLAUDE.md，適用於 /src/api/ 中的所有內容。記憶檔案是串接而非覆寫——根 CLAUDE.md 仍然適用，而 Claude Code 會在讀取此子樹中的檔案時按需載入此檔案。

## API 專屬標準

### 請求驗證
- 使用 Zod 進行結構描述驗證
- 務必驗證輸入
- 當驗證發生錯誤時，回傳 400 狀態碼
- 包含欄位層級的錯誤詳細資料

### 驗證
- 所有端點都需要 JWT token
- Token 位於 Authorization 標頭中
- Token 於 24 小時後逾期
- 實作重新整理 token 機制

### 回應格式

所有回應都必須遵循此結構：

```json
{
  "success": true,
  "data": { /* 實際資料 */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

錯誤回應：
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "使用者訊息",
    "details": { /* 欄位錯誤 */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### 分頁
- 使用游標式分頁（不要使用 offset）
- 包含 `hasMore` 布林值
- 將最大頁面大小限制為 100
- 預設頁面大小：20

### 速率限制
- 經過驗證的使用者每小時 1000 請求
- 公共端點每小時 100 請求
- 超出限制時回傳 429 狀態碼
- 包含 retry-after 標頭

### 快取
- 使用 Redis 進行工作階段快取
- 快取時長：預設 5 分鐘
- 在寫入操作時使快取失效
- 使用資源類型標記快取金鑰

---
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/memory
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
