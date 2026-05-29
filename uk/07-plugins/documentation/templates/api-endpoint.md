# [METHOD] /api/v1/[endpoint]

## Опис
Короткий опис що робить цей ендпоінт.

## Автентифікація
Необхідний метод автентифікації (напр., Bearer token).

## Параметри

### Параметри шляху
| Назва | Тип | Обов'язковий | Опис |
|-------|-----|-------------|------|
| id | string | Так | ID ресурсу |

### Параметри запиту
| Назва | Тип | Обов'язковий | Опис |
|-------|-----|-------------|------|
| page | integer | Ні | Номер сторінки (за замовч.: 1) |
| limit | integer | Ні | Елементів на сторінку (за замовч.: 20) |

### Тіло запиту
```json
{
  "field": "value"
}
```

## Відповіді

### 200 OK
```json
{
  "success": true,
  "data": {
    "id": "123",
    "name": "Example"
  }
}
```

### 400 Bad Request
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input"
  }
}
```

### 404 Not Found
```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Resource not found"
  }
}
```

## Приклади

### cURL
```bash
curl -X GET "https://api.example.com/api/v1/endpoint" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

### JavaScript
```javascript
const response = await fetch('/api/v1/endpoint', {
  headers: {
    'Authorization': 'Bearer token',
    'Content-Type': 'application/json'
  }
});
const data = await response.json();
```

### Python
```python
import requests

response = requests.get(
    'https://api.example.com/api/v1/endpoint',
    headers={'Authorization': 'Bearer token'}
)
data = response.json()
```

## Обмеження частоти запитів
- 1000 запитів на годину для автентифікованих користувачів
- 100 запитів на годину для публічних ендпоінтів

## Пов'язані ендпоінти
- [GET /api/v1/related](#)
- [POST /api/v1/related](#)
