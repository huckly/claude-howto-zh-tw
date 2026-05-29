# Функція: `functionName`

## Опис
Короткий опис що робить функція.

## Сигнатура
```typescript
function functionName(param1: Type1, param2: Type2): ReturnType
```

## Параметри

| Параметр | Тип | Обов'язковий | Опис |
|----------|-----|-------------|------|
| param1 | Type1 | Так | Опис param1 |
| param2 | Type2 | Ні | Опис param2 |

## Повертає
**Тип**: `ReturnType`

Опис того, що повертається.

## Кидає виключення
- `Error`: При невалідному введенні
- `TypeError`: При неправильному типі

## Приклади

### Базове використання
```typescript
const result = functionName('value1', 'value2');
console.log(result);
```

### Просунуте використання
```typescript
const result = functionName(
  complexParam1,
  { option: true }
);
```

## Нотатки
- Додаткові нотатки або попередження
- Міркування щодо продуктивності
- Найкращі практики

## Див. також
- [Пов'язана функція](#)
- [API-документація](#)
