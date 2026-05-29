---
name: api-documentation-generator
description: 從原始碼產生全面且準確的 API 文件。在建立或更新 API 文件、產生 OpenAPI 規格，或使用者提到 API 文件、端點或文件時使用。
---

# API 文件產生器 Skill

## 產生項目

- OpenAPI/Swagger 規格
- API 端點文件
- SDK 使用範例
- 整合指南
- 錯誤碼參考
- 認證指南

## 文件結構

### 對於每個端點

```markdown
## GET /api/v1/users/:id

### 說明
此端點功能的簡要說明

### 參數

| 名稱 | 類型 | 必填 | 說明 |
|------|------|------|------|
| id | string | 是 | 使用者 ID |

### 回應

**200 成功**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 找不到**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### 範例

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```
```
