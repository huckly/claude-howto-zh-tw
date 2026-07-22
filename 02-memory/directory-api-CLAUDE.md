# API Module Standards

此檔案會覆蓋根目錄的 CLAUDE.md，適用於 /src/api/ 中的所有內容

## API 特定標準

### Request Validation
- 使用 Zod 進行 schema 驗證
- 務必驗證輸入內容
- 若驗證失敗，回傳 400 錯誤
- 需包含欄位層級的錯誤細節

### Authentication
- 所有端點皆需 JWT token
- Token 放置於 Authorization header 中
- Token 有效期為 24 小時
- 實作 refresh token 機制

### Response Format

所有回應必須遵循此結構：

```json
{
  "success": true,
  "data": { /* actual data */ },
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
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### Pagination
- 使用基於游標（cursor-based）的分頁方式（而非 offset）
- 包含 `hasMore` 布林值
- 最大分頁大小限制為 100
- 預設分頁大小：20

### Rate Limiting
- 已驗證使用者每小時限額 1000 次請求
- 公開端點每小時限額 100 次請求
- 超過限額時回傳 429 錯誤
- 需包含 retry-after header

### Caching
- 使用 Redis 進行 session 快取
- 快取時長：預設 5 分鐘
- 進行寫入操作時失效快取
- 為快取鍵（cache keys）加上資源類型的標籤

---
**最後更新日期**：2026 年 4 月 9 日
