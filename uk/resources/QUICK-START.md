# Швидкий старт — Брендові ресурси

## Копіювання ресурсів у ваш проєкт

```bash
# Copy all resources to your web project
cp -r resources/ /path/to/your/website/

# Or just the favicons for web
cp resources/favicons/* /path/to/your/website/public/
```

## Додавання в HTML (копіювання та вставка)

```html
<!-- Favicons -->
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-32.svg" sizes="32x32">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-16.svg" sizes="16x16">
<link rel="apple-touch-icon" href="/resources/favicons/favicon-128.svg">
<link rel="icon" type="image/svg+xml" href="/resources/favicons/favicon-256.svg" sizes="256x256">
<meta name="theme-color" content="#000000">
```

## Використання в Markdown/документації

```markdown
# Claude How To

![Claude How To Logo](resources/logos/claude-howto-logo.svg)

![Icon](resources/icons/claude-howto-icon.svg)
```

## Рекомендовані розміри

| Призначення | Розмір | Файл |
|-------------|--------|------|
| Заголовок сайту | 520×120 | `logos/claude-howto-logo.svg` |
| Іконка додатку | 256×256 | `icons/claude-howto-icon.svg` |
| Вкладка браузера | 32×32 | `favicons/favicon-32.svg` |
| Домашній екран мобільного | 128×128 | `favicons/favicon-128.svg` |
| Десктопний додаток | 256×256 | `favicons/favicon-256.svg` |
| Малий аватар | 64×64 | `favicons/favicon-64.svg` |

## Значення кольорів

```css
/* Use these in your CSS */
--color-primary: #000000;
--color-secondary: #6B7280;
--color-accent: #22C55E;
--color-bg-light: #FFFFFF;
--color-bg-dark: #0A0A0A;
```

## Значення дизайну іконки

**Компас з кодовою дужкою**:
- Кільце компасу = Навігація, структурований навчальний шлях
- Зелена північна стрілка = Напрямок, прогрес, керівництво
- Чорна південна стрілка = Основа, міцний фундамент
- Дужка `>` = Промпт терміналу, код, контекст CLI
- Позначки = Точність, структуровані кроки

Це символізує "пошук шляху через код з чітким керівництвом".

## Що і де використовувати

### Сайт
- **Заголовок**: Логотип (`logos/claude-howto-logo.svg`)
- **Фавікон**: 32px (`favicons/favicon-32.svg`)
- **Соціальний превʼю**: Іконка (`icons/claude-howto-icon.svg`)

### GitHub
- **Бейдж README**: Іконка 64-128px
- **Аватар репозиторію**: Іконка

### Соціальні мережі
- **Аватар**: Іконка 256×256px
- **Банер**: Логотип 520×120px
- **Мініатюра**: Іконка 256×256px

### Документація
- **Заголовки розділів**: Логотип або іконка (масштабовані)
- **Іконки навігації**: Фавікон 32-64px

---

Див. [README.md](README.md) для повної документації.
