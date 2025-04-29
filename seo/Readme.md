# SEO (Search Engine Optimization)

## Полезные материалы
- [Микроразметка Schema.org. Зачем нужна schema и как её использовать](https://www.youtube.com/watch?v=BkP-a3J-yTg)
- [How to Add Schema Markup to Your Website (SEO For Beginners)](https://www.youtube.com/watch?v=ZIOQOMhZiY8)
- [Список поддерживаемых свойств и схем Яндекс](https://yandex.ru/support/webmaster/schema-org/what-is-schema-org.html)
- [Конструктор schema.org в JSON №1 (больше типов)](https://webcode.tools/structured-data-generator)
- [Конструктор schema.org в JSON №2](https://tools.100zona.com/schema-generator-jsonld.html)
- [Валидатор микроразметки от schema.org](https://validator.schema.org/)
- [Валидатор микроразметки yandex](https://webmaster.yandex.ru/tools/microtest/)
- [Введение в schema.org](https://yandex.ru/support/webmaster/schema-org/intro-schema-org.html)
- [Проверка расширенных результатов google](https://search.google.com/test/rich-results)
- [Можно спросить у нейросети какая schema.org разметка требуется по ссылке на страницу](https://gemini.google.com/app?hl=ru)


- **Конструктор не вносит массу полезных полей. Лучше сделать это самим или использовать другой конструктор**

## Порядок действий при работе со schema.org
1. Воспользоваться конструктором / нейросетью
2. Исправить даты / ссылки
3. Воспользоваться валидатором микроразметки (HTML)
4. Самим дополнить схему, описывая её так, чтобы это не было избыточно
5. Проверить проблемы разметки через проверку расширенных результатов google

## Основные компоненты SEO

### 1. Метаданные
- **Meta теги** - базовые метаданные для поисковых систем
- **Open Graph** - метаданные для социальных сетей
- **Twitter Cards** - метаданные для Twitter

Пример базовых метаданных:
```typescript
// Пример из https://github.com/kotec666/cadexchanger
export const generateBasicMetadata = {
  title: 'Cadexchanger - Обмен криптовалют',
  description: 'Безопасный обмен криптовалют с лучшими курсами',
  keywords: 'криптовалюта, обмен, биткоин, ethereum'
}
```

### 2. Структура заголовков
- **H1** - главный заголовок страницы (используется 1 раз)
- **H2** - подзаголовки основных разделов
- **H3** - подзаголовки подразделов

### 3. Семантическая разметка
- **Schema.org** - структурированные данные для поисковых систем (прямо в HTML / в теге Script)
- **Microdata** - семантическая разметка в HTML5
- **JSON-LD** - структурированные данные в формате JSON
- **RDFa** - расширяемая разметка для семантического веба
- **лучше выбрать что-то одно: JSON-LD**

#### Примеры JSON-LD

##### Для организации:
```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Cadexchanger",
  "url": "https://cadexchanger.com",
  "logo": "https://cadexchanger.com/logo.png",
  "contactPoint": {
    "@type": "ContactPoint",
    "telephone": "+7-999-123-45-67",
    "contactType": "customer service"
  }
}
</script>
```

##### Для статьи:
```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Как безопасно обменять криптовалюту",
  "image": "https://cadexchanger.com/images/safe-exchange.jpg",
  "author": {
    "@type": "Person",
    "name": "Иван Петров"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Cadexchanger",
    "logo": {
      "@type": "ImageObject",
      "url": "https://cadexchanger.com/logo.png"
    }
  },
  "datePublished": "2024-03-15",
  "dateModified": "2024-03-16"
}
</script>
```

##### Для продукта:
```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Обменник криптовалют",
  "image": "https://cadexchanger.com/images/exchange.jpg",
  "description": "Безопасный обмен криптовалют с лучшими курсами",
  "brand": {
    "@type": "Brand",
    "name": "Cadexchanger"
  },
  "offers": {
    "@type": "AggregateOffer",
    "priceCurrency": "RUB",
    "lowPrice": "1000",
    "highPrice": "1000000",
    "offerCount": "10"
  }
}
</script>
```

#### Примеры Schema.org с Microdata

##### Для хлебных крошек:
```html
<nav aria-label="Breadcrumb">
  <ol itemscope itemtype="https://schema.org/BreadcrumbList">
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="https://cadexchanger.com">
        <span itemprop="name">Главная</span>
      </a>
      <meta itemprop="position" content="1" /> <!-- Могут создаваться не только в head -->
    </li>
    <li itemprop="itemListElement" itemscope itemtype="https://schema.org/ListItem">
      <a itemprop="item" href="https://cadexchanger.com/exchange">
        <span itemprop="name">Обмен</span>
      </a>
      <meta itemprop="position" content="2" />
    </li>
  </ol>
</nav>
```

```html
itemscope указывает на родительский элемент
itemtype указывает на ссылку схемы, которую мы используем (копируем из URL браузера)
itemprop указывает на определенное свойство в родительском блоке - их можно получить по ссылке из itemscope, в первом столбце
Ценник товара и валюта это уже другая схема - Offer
Наличие товара это уже другая схема - InStock
```

##### Для FAQ:
```html
<div itemscope itemtype="https://schema.org/FAQPage">
  <div itemscope itemprop="mainEntity" itemtype="https://schema.org/Question">
    <h2 itemprop="name">Как обменять криптовалюту?</h2>
    <div itemscope itemprop="acceptedAnswer" itemtype="https://schema.org/Answer">
      <div itemprop="text">
        Выберите пару обмена, введите сумму и следуйте инструкциям.
      </div>
    </div>
  </div>
</div>
```

### 4. Техническое SEO
- **robots.txt** - правила для поисковых роботов
- **sitemap.xml** - карта сайта для поисковых систем
- **Canonical URLs** - предотвращение дублирования контента
- **Скорость загрузки** - оптимизация производительности

### 5. Контент и ключевые слова
- **Качество контента** - уникальный и полезный контент
- **Ключевые слова** - естественное использование в тексте
- **Альтернативные тексты** - описания для изображений
- **Внутренняя перелинковка** - связывание страниц сайта

### 6. Мобильная оптимизация
- **Адаптивный дизайн**
- **Mobile-first подход**
- **Core Web Vitals**

### 7. Мониторинг и аналитика
- **Google Search Console**
- **Google Analytics**
- **Яндекс.Метрика**
- **Яндекс.Вебмастер**

## Полезные ресурсы
- [Open Graph Protocol](https://ogp.me/)
- [Schema.org](https://schema.org/)
- [Google Search Console](https://search.google.com/search-console)
- [Яндекс.Вебмастер](https://webmaster.yandex.ru/)
