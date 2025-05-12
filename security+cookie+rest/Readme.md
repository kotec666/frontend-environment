# Безопасность, Cookies и REST API

## Полезные материалы
- [Security headers сканнер №1](https://securityheaders.com/)
- [Security headers сканнер №2](https://domsignal.com/)
- [Security headers сканнер №2 - все тулзы](https://domsignal.com/toolbox)
- [Как настроить политику безопасности контента (CSP) для вашего приложения Next.js](https://nextjs.org/docs/app/guides/content-security-policy)
- [Директивы для CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Content-Security-Policy)
- [Директивы для CSP №2](https://content-security-policy.com/)
- [Строгая настройка CSP Next.js: middleware](https://github.com/vercel/next.js/blob/canary/examples/with-strict-csp/middleware.js)
- [Строгая настройка CSP Next.js: script](https://github.com/vercel/next.js/blob/canary/examples/with-strict-csp/app/page.js)
- [Настройка <br>Cache-Control, CORS, X-DNS-Prefetch-Control, Strict-Transport-Security, X-Frame-Options, Permissions-Policy, X-Content-Type-Options, Referrer-Policy <br> В NEXT.JS](https://nextjs.org/docs/app/api-reference/config/next-config-js/headers#cache-control)
- [Permissions Policy директивы](https://www.validbot.com/header/Permissions-Policy.html)

## Содержание
1. [Безопасность](#безопасность)
2. [Cookies](#cookies)
3. [REST API](#rest-api)
4. [Script теги](#script-теги)
5. [Виды защиты](#виды-защиты)
6. [Виды защиты конфигурация](#виды-защиты-конфигурация)

## Безопасность

### Основные угрозы
1. **XSS (Cross-Site Scripting)**
   - Внедрение вредоносного JavaScript
   - Защита: санитизация данных, CSP

2. **CSRF (Cross-Site Request Forgery)**
   - Подделка межсайтовых запросов
   - Защита: CSRF-токены, SameSite cookies

3. **CORS (Cross-Origin Resource Sharing)**
   - Междоменные запросы
   - Настройка заголовков

### Защита от XSS
```javascript
// Санитизация HTML
const sanitizeHTML = (str) => {
  const div = document.createElement('div')
  div.textContent = str
  return div.innerHTML
}

// Использование
const userInput = '<script>alert("xss")</script>'
const safeHTML = sanitizeHTML(userInput)
```

### Content Security Policy (CSP)
```html
<!-- В заголовке -->
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'

<!-- В мета-теге -->
<meta http-equiv="Content-Security-Policy" content="default-src 'self'">
```

## Cookies

### Атрибуты cookies
| Атрибут    | Описание                                       | Пример                     |
|------------|------------------------------------------------|----------------------------|
| Secure     | Передача только по HTTPS                       | `Secure`                   |
| HttpOnly   | Запрет доступа через JavaScript                | `HttpOnly`                 |
| SameSite   | Ограничение отправки в кросс-доменных запросах | `SameSite=Strict`          |
| Path       | Путь, для которого cookie действителен         | `Path=/`                   |
| Domain     | Домен, для которого cookie действителен        | `Domain=.example.com`      |
| Max-Age    | Время жизни в секундах                         | `Max-Age=3600`             |
| Expires    | Дата истечения срока действия                  | `Expires=Wed, 21 Oct 2021` |

## REST API

### Коды состояния HTTP
| Код  | Категория         | Описание                                            |
|------|-------------------|-----------------------------------------------------|
| 1xx  | Информация        | Получение информации                                |
| 2xx  | Успех             | Запрос успешно обработан                            |
| 3xx  | Перенаправление   | Требуется дополнительное действие                   |
| 4xx  | Ошибка клиента    | Запрос содержит ошибку                              |
| 5xx  | Ошибка сервера    | Сервер не смог обработать запрос                    |

## Script теги

### Атрибуты тега Script
| Атрибут         | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Пример                                               |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------|
| crossorigin     | Настройка CORS: <br>- `anonymous `: запрос скрипта отправляется без кук и аутентификационных данных. <br>- `use-credentials `: запрос отправляется с куками и аутентификацией.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | `crossorigin="anonymous"`                            |
| integrity       | Проверка целостности                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | `integrity="sha256-..."`                             |
| nonce           | Уникальный идентификатор для CSP                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | `nonce="random-string"`                              |
| referrerPolicy  | Управление отправкой реферера: <br>- `no-referrer`: Не отправлять реферер <br>- `no-referrer-when-downgrade`: Не отправлять при переходе на HTTP <br>- `origin`: Отправлять только домен <br>- `origin-when-cross-origin`: Полный URL для того же источника, иначе только домен <br>- `same-origin`: Отправлять только для того же источника <br>- `strict-origin`: Отправлять домен при понижении безопасности <br>- `strict-origin-when-cross-origin`: Полный URL для того же источника, иначе домен <br>- `unsafe-url`: Всегда отправлять полный URL                                                                                                                                                                           | `referrerPolicy="strict-origin"`                     |

### Безопасная загрузка
```html
<!-- С проверкой целостности -->
<script 
  src="https://example.com/script.js"
  integrity="sha256-..."
  crossorigin="anonymous"
></script>

<!-- С поддержкой CSP -->
<script nonce="random-string">
  // Код с поддержкой CSP
</script>
```

## Виды защиты

### Безопасность
| Заголовок                     | Описание                                                                                                                                                                                                                                                                                           |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CSP (Content Security Policy) | Защита от инъекций (XSS, data-утечки)                                                                                                                                                                                                                                                              | 
| HTTPS (HTTP Secure)           | Защита от: MITM-атак.                                                                                                                                                                                                                                                                              | 
| X-XSS-Protection              | Атака XSS (межсайтовый скриптинг) это тип атаки, при котором вредоносный код может быть внедрён в атакуемую страницу.                                                                                                                                                                              | 
| X-Content-Type-Options        | Данный заголовок предотвращает атаки с подменой типов MIME `<script src=«script.txt»>` или несанкционированного хотлинка `<script src=«https://raw.githubusercontent.com/user/repo/branch/file.js»>`                                                                                               | 
| X-Frame-Options               | Для предотвращения страницы быть использованной во фрейме - Кликджекинг                                                                                                                                                                                                                            | 
| Strict-Transport-Security     | Для сайтов работающих по протоколу HTTPS рекомендуется указывать заголовок ответа HSTS для принудительного перенаправления страниц с HTTP на HTTPS.                                                                                                                                                | 
| Permissions-Policy            | Защита от: Доступа к API браузера (камера, микрофон).                                                                                                                                                                                                                                              | 
| Timing-Allow-Origin           | Защита от: Утечки данных через тайминг.                                                                                                                                                                                                                                                            | 
| Referrer-Policy               | Защита от: Утечки referrer-данных. Заголовок позволяет верно отслеживать переходы пользователей между сайтами. Сервисы сбора статистики, например Google Analytics и Яндекс.Метрика, используют реферер для сбора источников перехода.                                                             | 
| Cross-Origin-Opener-Policy    | Защита от атак, связанных с спекулятивными side-channel атаками (например, Spectre), а также предотвращение утечки данных между вкладками/окнами.                                                                                                                                                  | 
| Cross-Origin-Embedder-Policy  | Защита от атак, связанных с утечкой данных через встроенные ресурсы (например, Spectre), для доступа к новым API (SharedArrayBuffer, performance.measureUserAgentSpecificMemory()); Изоляции контента — помогает избежать непреднамеренного взаимодействия между разными источниками.              | 
| CSRF Token (Anti-CSRF Token)  | Совместно с бэкендом. CSRF-токен (token) нужен для защиты сайта от CSRF-атак (межсайтовой подделки запроса). Это вид атаки, при которой злоумышленник выполняет некорректные действия от имени человека, авторизованного на сайте. Например, может изменить пароль или отправить денежный перевод. | 


## Виды защиты конфигурация

### Параметры
| Заголовок                     | Конфигурация                                                                                                                                                                                                                                                                                                                                                          | Пример конфигурации                           | Подробнее                                                                                                                                                                                                                     |                                                                                                                                                                                                                   
|-------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Content-Security-Policy       | <br>• `default-src 'self'` <br>• `script-src 'self' 'unsafe-inline'` <br>• `style-src 'self' 'unsafe-inline'` <br>• `img-src https://*` <br>• `connect-src 'self'` <br>• `frame-ancestors 'none'` <br>• `report-uri /csp-report`                                                                                                                                      | `default-src 'self'; script-src 'self'`       | • [Как настроить политику безопасности контента (CSP) для вашего приложения Next.js](https://nextjs.org/docs/app/guides/content-security-policy) <br>• [Подробнее](https://developer.mozilla.org/ru/docs/Web/HTTP/Guides/CSP) |
| X-XSS-Protection              | 	<br>• `0 (отключено)`<br>• `1 (включено)`<br>• `1; mode=block (блокировка вместо фильтрации)`                                                                                                                                                                                                                                                                        | `1; mode=block`                               | Устарел, но поддерживается браузерами.                                                                                                                                                                                        |
| X-Content-Type-Options        | `nosniff (единственный параметр)`                                                                                                                                                                                                                                                                                                                                     | `nosniff`                                     | Запрещает MIME-sniffing для script/styles.                                                                                                                                                                                    |
| X-Frame-Options               | <br>• `DENY (запрет всех встраиваний)`<br>• `SAMEORIGIN (только свой домен)`<br>• `ALLOW-FROM https://example.com (устарел)`                                                                                                                                                                                                                                          | `DENY`                                        | Защита от кликджекинга.                                                                                                                                                                                                       |
| Strict-Transport-Security     | <br>• `max-age=SECONDS (например, 31536000 для 1 года)` <br>• `includeSubDomains (включая поддомены)` <br>• `preload (добавление в список HSTS браузеров)`                                                                                                                                                                                                            | `max-age=63072000; includeSubDomains`         | Только для HTTPS-сайтов.                                                                                                                                                                                                      |
| Permissions-Policy            | <br>• `camera=(), microphone=() (запрет доступа)` <br>• `geolocation=(self "https://example.com")` <br>• `fullscreen=* (разрешить всем)` <br>• `payment=() (запрет платежных API)`                                                                                                                                                                                    | `geolocation=(), camera=()`                   | [Список всех директив](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Permissions-Policy)                                                                                                                |
| Timing-Allow-Origin           | <br>• `* (разрешить всем доменам)` <br>• `https://example.com (конкретный домен)`                                                                                                                                                                                                                                                                                     | `https://your-site.com`                       | Контроль доступа к Resource Timing API.                                                                                                                                                                                       |
| Referrer-Policy               | <br>• `no-referrer (полный запрет)` <br>• `origin (только домен)` <br>• `strict-origin (домен при переходе с HTTPS→HTTPS)` <br>• `strict-origin-when-cross-origin (умное ограничение)`                                                                                                                                                                                | `strict-origin-when-cross-origin`             | [Все варианты](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Referrer-Policy)                                                                                                                           |
| Cross-Origin-Opener-Policy    | <br>• `unsafe-none (по умолчанию)` <br>• `same-origin (только для страниц с тем же origin)` <br>• `same-origin-allow-popups (Разрешает доступ через window.opener только если открывающая страница имеет тот же origin или была открыта текущей страницей.)` <br>• `restrict-properties (экспериментальный) Ограничивает доступ к некоторым свойствам window.opener.` | `Cross-Origin-Opener-Policy: same-origin`     | [Страница на MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cross-Origin-Opener-Policy)                                                                                                             |
| Cross-Origin-Embedder-Policy  | <br>• `unsafe-none (по умолчанию) — разрешает встраивание любых кросс-доменных ресурсов.` <br>• `require-corp — блокирует все кросс-доменные ресурсы, если у них нет заголовка Cross-Origin-Resource-Policy (CORP) или CORS.`                                                                                                                                         | `Cross-Origin-Embedder-Policy: require-corp`  | [Страница на MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Cross-Origin-Embedder-Policy)                                                                                                           |

- Пример конфигурации
```jsx
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
         headers: [
            // CSP
            {
               key: 'Content-Security-Policy',
               value: "default-src 'self'; script-src 'self' 'unsafe-inline'",
            },
            // Базовые заголовки
            { key: 'X-XSS-Protection', value: '1; mode=block' },
            { key: 'X-Content-Type-Options', value: 'nosniff' },
            { key: 'X-Frame-Options', value: 'DENY' },
            // HSTS
            {
               key: 'Strict-Transport-Security',
               value: 'max-age=63072000; includeSubDomains; preload',
            },
            // Другие политики
            { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
            { key: 'Permissions-Policy', value: 'geolocation=(), camera=()' },
         ],
      },
    ];
  },
};
```

```jsx
// HTTPS (HTTP Secure)
// middleware.js
export function middleware(request) {
   const url = request.nextUrl;
   if (url.protocol === 'http:') {
      url.protocol = 'https:';
      return NextResponse.redirect(url);
   }
}

// X-XSS-Protection
// next.config.js
headers: [
   {
      key: 'X-XSS-Protection',
      value: '1; mode=block',
   },
]

// X-Content-Type-Options
// next.config.js
headers: [
   {
      key: 'X-Content-Type-Options',
      value: 'nosniff',
   },
]

// X-Frame-Options
// next.config.js
headers: [
   {
      key: 'X-Frame-Options',
      value: 'DENY', // или 'SAMEORIGIN'
   },
]

// Referrer-Policy
// next.config.js
headers: [
   {
      key: 'Referrer-Policy',
      value: 'strict-origin-when-cross-origin',
   },
]

// Strict-Transport-Security (HSTS)
// next.config.js
headers: [
   {
      key: 'Strict-Transport-Security',
      value: 'max-age=63072000; includeSubDomains; preload',
   },
]

// Permissions-Policy
// next.config.js
headers: [
   {
      key: 'Permissions-Policy',
      value: "geolocation=(), camera=(), microphone=()",
   },
]

// Timing-Allow-Origin
// next.config.js
headers: [
   {
      key: 'Timing-Allow-Origin',
      value: 'https://your-domain.com',
   },
]
```

### Конфигурация cookie
| Параметр             | Описание                                                    | Пример                                 |
|----------------------|-------------------------------------------------------------|----------------------------------------|
| Name=Value           | Обязательные. Имя и значение куки.                          | sessionId=abc123                       |
| Max-Age=<Секунды>    | Время жизни куки в секундах (приоритетнее Expires).         | Max-Age=3600 (1 час)                   |
| Expires=<Дата>       | Дата истечения срока действия (устаревший формат).          | Expires=Wed, 21 Oct 2025 07:28:00 GMT  |
| Domain=<Домен>       | Указывает домен, для которого кука действительна.           | Domain=example.com                     |
| Path=<Путь>          | URL-путь, для которого кука отправляется.                   | Path=/api                              |
| Secure               | Кука передаётся только по HTTPS.                            | Secure                                 |
| HttpOnly             | Блокирует доступ к куке через JavaScript (document.cookie). | HttpOnly                               |
| SameSite=<Политика>  | Защита от CSRF: Strict, Lax, None.                          | SameSite=Strict                        |
| Priority=<Приоритет> | Приоритет куки: Low, Medium, High (редко используется).     | Priority=High                          |

### SameSite параметры:
| Значение  | Описание                                                                |
|-----------|-------------------------------------------------------------------------|
| Strict    | Кука отправляется только в рамках текущего сайта (защита от CSRF).      |
| Lax       | Кука отправляется при переходе по ссылкам (разрешено для GET-запросов). |
| None      | Кука отправляется при кросс-доменных запросах (требует Secure).         |


### Параметры запроса fetch:
| Параметр       | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Пример использования                                  |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| mode           | Режим запроса: <br>- `no-cors`: Запрос без CORS (ограниченный доступ к ответу) <br>- `cors`: Запрос с CORS (полный доступ к ответу) <br>- `same-origin`: Запрос только к тому же источнику                                                                                                                                                                                                                                                                                   | `{ mode: 'cors' }`                                    |
| cache          | Управление кэшированием: <br>- `default`: Стандартное поведение браузера <br>- `no-cache`: Игнорировать кэш <br>- `reload`: Принудительное обновление <br>- `force-cache`: Использовать кэш <br>- `only-if-cached`: Только из кэша                                                                                                                                                                                                                                           | `{ cache: 'no-cache' }`                               |
| credentials    | Управление отправкой учетных данных: <br>- `include`: Всегда отправлять <br>- `same-origin`: Только для запросов к тому же источнику <br>- `omit`: Не отправлять                                                                                                                                                                                                                                                                                                             | `{ credentials: 'include' }`                          |
| headers        | Заголовки HTTP-запроса в виде объекта. Используется для передачи дополнительной информации серверу                                                                                                                                                                                                                                                                                                                                                                           | `{ headers: { 'Content-Type': 'application/json' } }` |
| redirect       | Обработка перенаправлений: <br>- `manual`: Не следовать перенаправлениям <br>- `follow`: Следовать перенаправлениям <br>- `error`: Ошибка при перенаправлении                                                                                                                                                                                                                                                                                                                | `{ redirect: 'follow' }`                              |
| signal         | Объект AbortSignal для отмены запроса. Позволяет прервать выполнение запроса                                                                                                                                                                                                                                                                                                                                                                                                 | `{ signal: controller.signal }`                       |
| integrity      | Криптографический хеш ресурса для проверки целостности. Используется для защиты от подмены контента                                                                                                                                                                                                                                                                                                                                                                          | `{ integrity: 'sha256-...' }`                         |
| referrer       | URL источника запроса: <br>- URL: Указанный URL <br>- `about:client`: Пустой реферер <br>- `''`: Пустая строка                                                                                                                                                                                                                                                                                                                                                               | `{ referrer: 'https://example.com' }`                 |
| referrerPolicy | Политика отправки реферера: <br>- `no-referrer`: Не отправлять <br>- `no-referrer-when-downgrade`: Не отправлять при переходе на HTTP <br>- `origin`: Только домен <br>- `origin-when-cross-origin`: Полный URL для того же источника <br>- `same-origin`: Только для того же источника <br>- `strict-origin`: Домен при понижении безопасности <br>- `strict-origin-when-cross-origin`: Полный URL для того же источника, иначе домен <br>- `unsafe-url`: Всегда полный URL | `{ referrerPolicy: 'strict-origin' }`                 |
| keepalive      | Флаг для поддержания соединения после закрытия страницы. Полезен для отправки аналитики                                                                                                                                                                                                                                                                                                                                                                                      | `{ keepalive: true }`                                 |
| window         | Указание окна для запроса. Обычно `null` или `undefined`                                                                                                                                                                                                                                                                                                                                                                                                                     | `{ window: null }`                                    |
| next           | Специфичные для Next.js параметры: <br>- `revalidate`: Время в секундах до перевалидации кэша <br>- `tags`: Массив тегов для групповой ревалидации                                                                                                                                                                                                                                                                                                                           | `{ next: { revalidate: 3600, tags: ['products'] } }`  |

### Примеры использования параметров

```javascript
// Базовый запрос с заголовками
fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token'
  },
  body: JSON.stringify({ data: 'value' })
})

// Запрос с CORS и учетными данными
fetch('https://api.example.com/user', {
  mode: 'cors',
  credentials: 'include',
  headers: {
    'Accept': 'application/json'
  }
})

// Запрос с отменой
const controller = new AbortController()
fetch('https://api.example.com/long-request', {
  signal: controller.signal
})
// Отмена запроса
controller.abort()

// Запрос с проверкой целостности
fetch('https://example.com/script.js', {
  integrity: 'sha256-abc123...'
})

// Запрос с Next.js ревалидацией
fetch('https://api.example.com/products', {
  next: {
    revalidate: 3600,
    tags: ['products']
  }
})
```

    

    