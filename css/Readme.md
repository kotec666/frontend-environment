# Руководство по CSS

## Полезные материалы
- [Плейлист с описанием свойств CSS](https://www.youtube.com/watch?v=PEQ3i9q3ez8&list=PL0MUAHwery4o9I7QQVj_RP4ZVpmdx6evz&index=1)
- [5 новых CSS и JavaScript фич](https://www.youtube.com/watch?v=BjqepzUA-o4)
- [ТОП-5 новинок CSS](https://www.youtube.com/watch?v=5-FD__mbjls)
- [CSS Свойства для улучшения верстки](https://www.youtube.com/watch?v=jLJU27MNdQs)
- [5 фишек CSS, которые надо знать](https://www.youtube.com/watch?v=ljSpwVyw4yU)
- [ТОП-10 новинок CSS в 2023](https://www.youtube.com/watch?v=cz7IkmyKFHU)
- [Слайдер на чистом CSS](https://www.youtube.com/watch?v=TvniJS45Lgo&list=PL2zhi-fIpnXA6Rnwy-kshdQJ4W27wLFpN)
- [Grid CSS за 13 минут](https://www.youtube.com/watch?v=xI9G0Zh5DVA)
- [Расчет line-height](https://youtu.be/qxJEcHjk8Xs?t=14m16s)
- [Font-face генератор, WOFF2](https://transfonter.org/)
- [roadmap](https://roadmap.sh/r/css-pbthg)
- [roadmap #2](https://roadmap.sh/r/css)
- [Clamp calculator](https://www.marcbacon.com/tools/clamp-calculator/)

## Основные свойства

### Display
- `none` - элемент полностью удаляется со страницы
- `block` - блочный элемент:
  - Располагаются вертикально
  - Занимает всю доступную ширину
  - Можно задавать размеры
- `inline` - строчный элемент:
  - Располагаются на одной строке
  - Размеры определяются содержимым
  - Нельзя задавать width/height
- `inline-block` - строчно-блочный элемент:
  - Располагаются на одной строке
  - Можно задавать размеры
- `flex` - флекс-контейнер:
  - По умолчанию `flex-direction: row`
  - Позволяет управлять направлением и выравниванием
- `grid` - грид-контейнер:
  - Создает сетку из элементов
  - Позволяет управлять расположением в двух измерениях

### Position
- `static` - стандартное позиционирование
- `relative` - относительное позиционирование:
  - Сдвиг относительно обычного положения
  - Управление через `top`, `left`, `right`, `bottom`
- `absolute` - абсолютное позиционирование:
  - Элемент "вырывается" из потока
  - Позиционируется относительно ближайшего `relative` родителя
  - Если нет `relative` родителя - относительно документа
- `fixed` - фиксированное позиционирование:
  - Привязка к определенной точке экрана
  - Не зависит от прокрутки
- `sticky` - "липкое" позиционирование:
  - Привязка к определенной точке после прокрутки
  - Работает только внутри родительского контейнера

### Box Model
```css
/* Сокращенная запись */
inset: 0 0 0 0; /* = top: 0; right: 0; bottom: 0; left: 0; */

/* Box-sizing */
box-sizing: border-box; /* Размер включает padding и border */
box-sizing: content-box; /* Размер не включает padding и border */
```

## Современные возможности CSS

### Grid Layout
```css
/* Автоматическое заполнение с минимальной шириной */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));

/* Подсетка */
grid-template-rows: subgrid;

/* Масонри-сетка */
grid-template-rows: masonry;
masonry-auto-flow: ordered;
align-tracks: end;
justify-tracks: stretch;
```

### Текст и типографика
```css
/* Балансировка текста */
text-wrap: balance; /* Равномерное распределение */
text-wrap: pretty; /* Более естественное распределение */

/* Ограничение строк */
display: -webkit-box;
-webkit-line-clamp: 3;
-webkit-box-orient: vertical;
overflow: hidden;

/* Вариативные шрифты */
font-variation-settings: "wght" 400, "wdth" 100;
```

### Размеры и отступы
```css
/* Адаптивные размеры */
width: min(200px, 100%); /* Выбирает меньшее значение */
width: max(250px, 50%); /* Выбирает большее значение */
width: clamp(200px, 30vw, 400px); /* Минимум, рекомендуемое, максимум */

/* Единицы измерения viewport */
svh - относительно самого меньшего состояния вьюпорта /* когда показывается тулбар */
lvh - относительно самого большого состоянию вьюпорта /* когда тулбар скрыт */
dvh - относительно состояния экрана /* сам пересчитывает скрытие тулбара */

/* Логические свойства */
padding-inline-start: 16px; /* Адаптируется под направление письма (i18 вёрстка) */
padding-block-start: 16px; /* Адаптируется под направление письма (i18 вёрстка) */

/* Размеры контента */
width: min-content; /* Задание ширины по самому длинному элементу в блоке */
width: max-content; /* Занять столько сколько нужно пространства, но не больше */
width: fit-content; /* Когда контейнер большой - max-content, когда мелкий - min-content */

/* Наследование цвета */
currentColor: /* Наследует цвет от родителя */
button {
  color: #0d47a1;
}
svg {
  fill: currentColor; /* Наследует цвет от кнопки */
}
```

### Контейнерные запросы
```css

.box {
  container: blog / inline-size;
}

@container blog(max-width: 670px) {
  article {
    flex-direction: row;
    min-width: 100%;
  }
}

@container (max-width: 600px) {
  .card {
    flex-direction: column;
  }
}

@container style(--theme: dark) {
  .card {
    background: #333;
  }
}
```

### Селекторы и псевдоклассы
```css
/* Новые селекторы */
:has(.card__image) { /* Выбор родителя по дочернему элементу */ }
:nth-child(2 of .highlight) { /* Выбор по порядку среди элементов с классом */ }
:first-letter {
  initial-letter: 3.6 6; /* (lines, offset) */
}

/* Псевдоклассы */
:focus-visible { /* Стили для фокуса через клавиатуру */ }
:marker { /* Стилизация маркеров списка */ }
:where(header, footer) > p { /* Группировка селекторов */ }
```

### Анимации и переходы
```css
/* Плавный скролл */
scroll-behavior: smooth;

/* Ограничение скролла */
overscroll-behavior: contain;

/* Анимации с учетом предпочтений пользователя */
@media (prefers-reduced-motion: no-preference) {
  .element {
    transition: transform 0.3s ease;
  }
}

/* Линейная анимация */
linear(0, 1); /* Линейная анимация через функцию, без ускорения, замедления */

/* Snap scroll */
.container {
  scroll-snap-type: y mandatory;
  scroll-snap-points-y: repeat(300px);
}
.child {
  scroll-snap-align: start;
}
```

### Цвета и градиенты
```css
/* Современные цветовые пространства */
color: oklch(70% 0.1 120);
background: conic-gradient(from 45deg, red, yellow, green);

/* Смешивание цветов */
color: color-mix(in srgb, blue 30%, white);

/* Системные цвета */
color-scheme: light dark;

/* Фильтры */
filter: blur(5px) contrast(120%); /* Блюр, контраст и прочее */
backdrop-filter: blur(5px); /* Фильтр к фону элемента */

/* Смешивание цветов */
mix-blend-mode: multiply; /* Различные режимы наложения */

/* Цвета контролов */
accent-color: #0d47a1; /* Задаёт дефолтные цвета для контролов, чекбоксы, прогрессбар */
```

### Оптимизация производительности
```css
/* Оптимизация рендеринга */
content-visibility: auto; /* Браузер не будет тратить ресурсы на отрисовку вне зоны видимости */

/* Оптимизация изображений */
object-view-box: 0 0 100 100; /* Позволяет работать с вьюбоксом как в svg, отрезать контент */

/* Оптимизация шрифтов */
font-display: swap; /* Оптимизация загрузки шрифтов */

/* Оптимизация скролла */
scrollbar-gutter: stable; /* Предотвращает скачки контента при появлении скроллбара */
```

### Медиа-запросы
```css
/* Новый синтаксис диапазонов */
@media (100px <= width <= 1900px) {
  /* Стили */
}

/* Предпочтения пользователя */
@media (prefers-reduced-motion: reduce) {
  /* Уменьшенная анимация */
}

/* Принудительные цвета */
@media (forced-colors: active) {
  .button {
    border: 2px solid CanvasText;
  }
  
/* Проверка поддержки */
@supports (display: grid) {
  .main {
    display: grid;
  }

@media (prefers-contrast: more) {
  /* Увеличенный контраст */
}

@media (prefers-reduced-data: reduce) {
  /* Экономия трафика */
}
```

### Новые API
- **Popover API** - нативные всплывающие окна
- **View Transition API** - анимации переходов между страницами
- **Scroll-driven Animations** - анимации на основе скролла
- **Container Queries** - адаптация на основе размера контейнера
- **Style Queries** - адаптация на основе CSS-переменных


## Советы и рекомендации
1. Используйте логические свойства для интернациональных сайтов
2. Применяйте `content-visibility: auto` для оптимизации производительности
3. Используйте `scrollbar-gutter: stable` для предотвращения скачков контента
4. Добавляйте `font-display: swap` для оптимизации загрузки шрифтов
5. Используйте `aspect-ratio` для сохранения пропорций изображений
6. Применяйте `object-view-box` для кадрирования изображений
7. Используйте `touch-action` для оптимизации мобильных взаимодействий