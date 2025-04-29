# Next.js: Современный React-фреймворк

## Полезные материалы
- [Конфигурация SSG / SSR / ISR](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)

## Содержание
1. [Основные концепции](#основные-концепции)
2. [Роутинг](#роутинг)
3. [Рендеринг](#рендеринг)
4. [Динамический импорт](#динамический-импорт)
5. [Архитектурные подходы](#архитектурные-подходы)
6. [Оптимизация](#оптимизация)
7. [API Routes](#api-routes)
8. [Middleware](#middleware)
9. [Лучшие практики](#лучшие-практики)
10. [Инструменты разработчика](#инструменты-разработчика)

## Основные концепции

### Что такое Next.js?
Next.js - это React-фреймворк, который предоставляет:
- Серверный рендеринг (SSR)
- Статическую генерацию (SSG)
- Инкрементальную статическую регенерацию (ISR)
- API роуты
- Встроенную оптимизацию изображений
- Автоматическое разделение кода

### Преимущества Next.js
1. **SEO-оптимизация**
   - Серверный рендеринг для лучшей индексации
   - Метаданные для каждой страницы
   - Динамическая генерация sitemap.xml

2. **Производительность**
   - Автоматическое разделение кода
   - Оптимизация изображений
   - Предварительная загрузка страниц


## Роутинг

### Файловая система маршрутизации
```
app/
  ├── page.tsx           # Главная страница (/)
  ├── layout.tsx         # Общий layout
  ├── loading.tsx        # Компонент загрузки
  ├── error.tsx          # Компонент ошибки
  ├── not-found.tsx      # Страница 404
  ├── api/               # API роуты
  │   └── route.ts
  ├── blog/              # Динамический роут
  │   ├── [slug]/
  │   │   └── page.tsx
  │   └── page.tsx
  └── dashboard/         # Группировка роутов
      ├── layout.tsx
      ├── page.tsx
      └── settings/
          └── page.tsx
```

### Типы роутов
| Тип роута    | Описание                   | Пример              |
|--------------|----------------------------|---------------------|
| Статический  | Фиксированный путь         | `/about`            |
| Динамический | Переменный сегмент         | `/blog/[slug]`      |
| Catch-all    | Любое количество сегментов | `/docs/[...slug]`   |
| Опциональный | Необязательный сегмент     | `/shop/[[...slug]]` |

### Навигация
```jsx
// Клиентская навигация
import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()
  
  return (
    <button onClick={() => router.push('/dashboard')}>
      Перейти в дашборд
    </button>
  )
}
```

## Рендеринг

### Стратегии рендеринга
| Стратегия | Описание                                     | Когда использовать                      |
|-----------|----------------------------------------------|-----------------------------------------|
| SSR       | Рендеринг на сервере при каждом запросе      | Динамический контент                    |
| SSG       | Статическая генерация при сборке             | Контент, не требующий частых обновлений |
| ISR       | Периодическое обновление статических страниц | Контент, требующий обновления           |
| CSR       | Рендеринг на клиенте                         | Интерактивные приложения                |

### Конфигурация кэширования fetch 

#### Опции кэширования
```jsx
// Статическая генерация (SSG)
fetch(url, { 
  cache: 'force-cache' // Данные кэшируются на этапе сборки
})

// Динамический рендеринг (SSR)
fetch(url, { 
  cache: 'no-store' // Каждый запрос получает свежие данные
})

// Инкрементальная статическая регенерация (ISR)
fetch(url, { 
  next: { 
    revalidate: 3600 // Пересборка каждые 3600 секунд (1 час)
  }
})
```

#### Дополнительные опции fetch
```jsx
fetch(url, {
  // Кэширование
  cache: 'force-cache' | 'no-store',
  
  // Ревалидация
  next: {
    revalidate: number, // В секундах
    tags: ['collection'] // Теги для ревалидации
  },
  
  // Заголовки
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  
  // Метод запроса
  method: 'GET' | 'POST' | 'PUT' | 'DELETE',
  
  // Тело запроса
  body: JSON.stringify(data)
})
```

#### Примеры использования

##### Статическая генерация с ISR
```jsx
// app/products/page.tsx
export default async function ProductsPage() {
  const products = await fetch('https://api.example.com/products', {
    next: { revalidate: 3600 } // Обновление каждый час
  }).then(res => res.json())
  
  return <ProductsList products={products} />
}
```

##### Динамический рендеринг
```jsx
// app/dashboard/page.tsx
export default async function DashboardPage() {
  const stats = await fetch('https://api.example.com/stats', {
    cache: 'no-store' // Всегда свежие данные
  }).then(res => res.json())
  
  return <Dashboard stats={stats} />
}
```

##### Условное кэширование
```jsx
// app/blog/[slug]/page.tsx
export default async function BlogPost({ params }) {
  const post = await fetch(`https://api.example.com/posts/${params.slug}`, {
    cache: params.slug === 'latest' ? 'no-store' : 'force-cache'
  }).then(res => res.json())
  
  return <BlogPost post={post} />
}
```

##### Ревалидация по тегам
```jsx
// app/api/revalidate/route.ts
export async function POST() {
  // Ревалидация всех запросов с тегом 'products'
  revalidateTag('products')
  return NextResponse.json({ revalidated: true })
}

// app/products/page.tsx
export default async function ProductsPage() {
  const products = await fetch('https://api.example.com/products', {
    next: { tags: ['products'] }
  }).then(res => res.json())
  
  return <ProductsList products={products} />
}
```

#### Лучшие практики
1. **Выбор стратегии кэширования**
   - `force-cache` для статического контента
   - `no-store` для динамических данных
   - `revalidate` для периодически обновляемого контента

2. **Оптимальное время ревалидации**
   - Часто обновляемый контент: 60-300 секунд
   - Умеренно обновляемый: 3600 секунд (1 час)
   - Редко обновляемый: 86400 секунд (24 часа)

3. **Теги ревалидации**
   - Используйте для группировки связанных данных
   - Позволяют точечно обновлять кэш
   - Удобны для управления контентом

4. **Обработка ошибок**
   ```jsx
   try {
     const data = await fetch(url, options)
     if (!data.ok) throw new Error('Ошибка загрузки')
     return data.json()
   } catch (error) {
     // Обработка ошибок
   }
   ```

### Конфигурация рендеринга
```jsx
export const dynamic = 'auto'; // 'auto' | 'force-dynamic' | 'error' | 'force-static'
export const revalidate = 3600; // Опционально: ISR (Incremental Static Regeneration)
```

### Примеры рендеринга

#### SSR (Server Side Rendering)
```jsx
// app/page.tsx
export default async function Page() {
  const data = await fetchData()
  return <div>{data}</div>
}
```

#### SSG (Static Site Generation)
```jsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await getPosts()
  return posts.map((post) => ({
    slug: post.slug,
  }))
}
```

#### ISR (Incremental Static Regeneration)
```jsx
// app/products/[id]/page.tsx
// для часто меняющегося контента: 60-300 сек
// для относительно статичного: 3600-86400 сек
export const revalidate = 3600 // Пересборка каждый час 

export default async function Page({ params }) {
  const product = await getProduct(params.id)
  return <div>{product.name}</div>
}
```

### Динамический импорт

#### Базовое использование
```jsx
// app/page.tsx
import dynamic from 'next/dynamic'

// Ленивая загрузка компонента
const DynamicComponent = dynamic(() => import('../components/HeavyComponent'))

export default function Page() {
  return (
    <div>
      <h1>Главная страница</h1>
      <DynamicComponent />
    </div>
  )
}
```

#### С загрузчиком
```jsx
const DynamicComponent = dynamic(
  () => import('../components/HeavyComponent'),
  {
    loading: () => <p>Загрузка...</p>,
    ssr: false // Отключение серверного рендеринга
  }
)
```

#### С именованным экспортом
```jsx
const DynamicComponent = dynamic(
  () => import('../components/HeavyComponent').then(mod => mod.NamedExport)
)
```

#### Примеры использования

##### Ленивая загрузка библиотек
```jsx
const Map = dynamic(() => import('react-map-gl'), {
  ssr: false,
  loading: () => <p>Загрузка карты...</p>
})
```

##### Условная загрузка компонентов
```jsx
const DynamicComponent = dynamic(() => {
  if (condition) {
    return import('../components/ComponentA')
  }
  return import('../components/ComponentB')
})
```

##### Лучшие практики
1. **Оптимизация загрузки**
   - Используйте для тяжелых компонентов
   - Применяйте для компонентов, не критичных для SEO
   - Используйте для компонентов с внешними зависимостями

2. **Производительность**
   - Добавляйте loading состояние
   - Используйте preload для критических компонентов
   - Оптимизируйте размер бандла

3. **SEO**
   - Учитывайте влияние на индексацию
   - Используйте ssr: true для важного контента
   - Предоставляйте fallback контент

### Архитектурные подходы

#### Component-Based Architecture (CBA)
Компонентная архитектура - это подход, при котором приложение строится из независимых, переиспользуемых компонентов.

##### Принципы CBA
1. **Инкапсуляция**
   ```jsx
   // components/Button.tsx
   interface ButtonProps {
     children: React.ReactNode;
     variant?: 'primary' | 'secondary';
     onClick?: () => void;
   }
   
   export const Button = ({ children, variant = 'primary', onClick }: ButtonProps) => {
     return (
       <button className={`button button--${variant}`} onClick={onClick}>
         {children}
       </button>
     )
   }
   ```

2. **Композиция**
   ```jsx
   // components/Card.tsx
   import { Button } from './Button'
   
   export const Card = ({ title, content }) => {
     return (
       <div className="card">
         <h2>{title}</h2>
         <p>{content}</p>
         <Button variant="secondary">Подробнее</Button>
       </div>
     )
   }
   ```

#### Feature-Sliced Design (FSD)
FSD - это методология организации кода, основанная на бизнес-логике и функциональности.

##### Структура FSD
```
src/
  ├── app/              # Инициализация приложения
  ├── pages/            # Страницы
  ├── widgets/          # Сложные компоненты
  ├── features/         # Бизнес-логика
  ├── entities/         # Бизнес-сущности
  ├── shared/           # Переиспользуемый код
  └── lib/              # Утилиты и хелперы
```

##### Пример реализации
```jsx
// features/auth/model.ts
export const authModel = {
  state: {
    user: null,
    isAuthenticated: false
  },
  reducers: {
    setUser: (state, user) => ({ ...state, user }),
    setAuth: (state, isAuthenticated) => ({ ...state, isAuthenticated })
  }
}

// features/auth/ui/LoginForm.tsx
export const LoginForm = () => {
  const { user, setUser } = useAuth()
  // ...
}
```

#### Atomic Design
Atomic Design - это методология, основанная на принципах химии, где компоненты делятся на атомы, молекулы, организмы и т.д.

##### Уровни Atomic Design
1. **Атомы**
   ```jsx
   // atoms/Button.tsx
   export const Button = ({ children }) => (
     <button className="button">{children}</button>
   )
   ```

2. **Молекулы**
   ```jsx
   // molecules/SearchForm.tsx
   import { Button } from '../atoms/Button'
   import { Input } from '../atoms/Input'
   
   export const SearchForm = () => (
     <form>
       <Input placeholder="Поиск..." />
       <Button>Найти</Button>
     </form>
   )
   ```

3. **Организмы**
   ```jsx
   // organisms/Header.tsx
   import { SearchForm } from '../molecules/SearchForm'
   import { Navigation } from '../molecules/Navigation'
   
   export const Header = () => (
     <header>
       <Navigation />
       <SearchForm />
     </header>
   )
   ```

4. **Шаблоны**
   ```jsx
   // templates/MainLayout.tsx
   import { Header } from '../organisms/Header'
   import { Footer } from '../organisms/Footer'
   
   export const MainLayout = ({ children }) => (
     <div>
       <Header />
       <main>{children}</main>
       <Footer />
     </div>
   )
   ```

5. **Страницы**
   ```jsx
   // pages/HomePage.tsx
   import { MainLayout } from '../templates/MainLayout'
   
   export const HomePage = () => (
     <MainLayout>
       <h1>Добро пожаловать!</h1>
     </MainLayout>
   )
   ```

##### Преимущества Atomic Design
1. **Масштабируемость**
   - Легко добавлять новые компоненты
   - Четкая структура проекта
   - Простое тестирование

2. **Переиспользование**
   - Компоненты можно использовать в разных контекстах
   - Уменьшение дублирования кода
   - Единый стиль интерфейса

3. **Поддержка**
   - Легко находить нужные компоненты
   - Простое обновление дизайна
   - Четкая документация

## Оптимизация

### Оптимизация изображений
```jsx
import Image from 'next/image'

export default function Page() {
  return (
    <Image
      src="/profile.jpg"
      alt="Profile"
      width={500}
      height={500}
      priority // Предзагрузка
      placeholder="blur" // Размытие при загрузке
    />
  )
}
```

### Оптимизация шрифтов
```jsx
// app/layout.tsx
import { Inter } from 'next/font/google'

const inter = Inter({ subsets: ['latin'] })

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

### Оптимизация скриптов
```jsx
import Script from 'next/script'

export default function Page() {
  return (
    <Script
      src="https://example.com/script.js"
      strategy="afterInteractive" // Загрузка после интерактивности
      onLoad={() => console.log('Script loaded')}
    />
  )
}
```

## API Routes

### Создание API эндпоинта
```typescript
// app/api/hello/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  return NextResponse.json({ message: 'Hello World' })
}

export async function POST(request: Request) {
  const body = await request.json()
  return NextResponse.json({ received: body })
}
```

### Обработка параметров
```typescript
// app/api/users/[id]/route.ts
export async function GET(
  request: Request,
  { params }: { params: { id: string } }
) {
  const user = await getUser(params.id)
  return NextResponse.json(user)
}
```

## Middleware

### Что такое Middleware?
Middleware в Next.js - это функция, которая выполняется перед обработкой запроса. Она позволяет:
- Модифицировать запрос/ответ
- Перенаправлять запросы
- Проверять аутентификацию
- Устанавливать заголовки
- И многое другое

### Примеры использования Middleware

#### Базовая структура
```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  return NextResponse.next()
}

// Конфигурация путей
export const config = {
  matcher: '/about/:path*',
}
```

#### Аутентификация
```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  const token = request.cookies.get('token')
  
  if (!token) {
    return NextResponse.redirect(new URL('/login', request.url))
  }
  
  return NextResponse.next()
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/protected/:path*']
}
```

#### Локализация
```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

const locales = ['en', 'ru']

export function middleware(request: NextRequest) {
  const pathname = request.nextUrl.pathname
  const pathnameIsMissingLocale = locales.every(
    (locale) => !pathname.startsWith(`/${locale}/`) && pathname !== `/${locale}`
  )

  if (pathnameIsMissingLocale) {
    const locale = 'ru'
    return NextResponse.redirect(
      new URL(`/${locale}${pathname}`, request.url)
    )
  }
}

export const config = {
  matcher: ['/((?!api|_next/static|_next/image|favicon.ico).*)']
}
```

#### Аналитика и логирование
```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Логирование запросов
  console.log(`[${request.method}] ${request.nextUrl.pathname}`)
  
  // Добавление заголовков
  const response = NextResponse.next()
  response.headers.set('x-request-time', new Date().toISOString())
  
  return response
}
```

## Лучшие практики

### Оптимизация производительности
1. **Используйте Image Component**
   - Автоматическая оптимизация
   - Ленивая загрузка
   - Предотвращение CLS

2. **Оптимизация шрифтов**
   - Используйте next/font
   - Предзагрузка критических шрифтов
   - Оптимизация подмножеств

3. **Кэширование**
   - Настройка revalidate
   - Использование ISR
   - Кэширование API запросов

### Безопасность
1. **Защита API роутов**
   - Валидация входных данных
   - Ограничение CORS
   - Rate limiting

2. **Аутентификация**
   - Использование middleware
   - Безопасное хранение токенов
   - Защита от CSRF

### SEO
1. **Метаданные**
   - Использование Metadata API
   - Динамические метаданные
   - Open Graph теги

2. **Структура данных**
   - JSON-LD
   - Структурированные данные
   - Карта сайта

## Инструменты разработчика

### Отладка
```typescript
// next.config.js
module.exports = {
  reactStrictMode: true,
  swcMinify: true,
  compiler: {
    removeConsole: process.env.NODE_ENV === 'production',
  },
}
```

### Анализ производительности
```bash
# Установка
npm install @next/bundle-analyzer

# Использование
ANALYZE=true npm run build
```

### Мониторинг
```typescript
// middleware.ts
export function middleware(request: NextRequest) {
  const start = Date.now()
  const response = NextResponse.next()
  const duration = Date.now() - start
  
  // Отправка метрик
  console.log(`Request duration: ${duration}ms`)
  
  return response
}
```


