# Руководство по HTML5

## Полезные материалы
- [Периодическая таблица HTML-элементов](https://gerardkcohen.github.io/periodic-table-of-semantics.html)

## Структура документа
```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 

      Запрещать пользователю масштабировать страницу плохая практика,
      негативно влияющая на accessibility

    -->
    <title>Заголовок страницы</title>
</head>
<body>
    <!-- Содержимое страницы -->
</body>
</html>
```

## Семантические элементы HTML5

### Основные структурные элементы
- `<header>` - шапка сайта или раздела
- `<nav>` - навигационное меню
- `<main>` - основное содержимое страницы
- `<article>` - независимый контент
- `<section>` - тематическая группа контента
- `<aside>` - дополнительный контент
- `<footer>` - подвал сайта или раздела

### Заголовки (H1-H6)
- `H1` - главный заголовок страницы:
  - Один на страницу
  - Описывает основную тему
  - Важен для SEO
- `H2` - подзаголовки разделов:
  - Может быть несколько
  - Разделяет основной контент
- `H3-H6` - подзаголовки подразделов:
  - Уменьшаются по важности
  - Используются для вложенной структуры

### Формы и интерактивные элементы
```html
<form action="/submit" method="post">
    <fieldset>
        <legend>Личная информация</legend>
        
        <label for="name">Имя:</label>
        <input type="text" id="name" name="name" required>
        
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
        
        <button type="submit">Отправить</button>
    </fieldset>
</form>
```

### Мультимедиа
```html
<!-- Изображения -->
<img src="image.jpg" alt="Описание изображения" loading="lazy">

<!-- Видео -->
<video controls>
    <source src="video.mp4" type="video/mp4">
    Ваш браузер не поддерживает видео.
</video>

<!-- Аудио -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    Ваш браузер не поддерживает аудио.
</audio>
```

### Таблицы
```html
<table>
    <caption>Описание таблицы</caption>
    <thead>
        <tr>
            <th>Заголовок 1</th>
            <th>Заголовок 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Данные 1</td>
            <td>Данные 2</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>Итого</td>
            <td>Сумма</td>
        </tr>
    </tfoot>
</table>
```

## Лучшие практики HTML5

### Доступность (a11y)
1. Используйте семантические теги
2. Добавляйте атрибуты `alt` для изображений
3. Используйте `label` для полей ввода
4. Обеспечьте правильную структуру заголовков
5. Используйте ARIA-атрибуты при необходимости

### Оптимизация
1. Используйте атрибут `loading="lazy"` для изображений
2. Оптимизируйте размеры изображений
3. Используйте современные форматы (WebP, AVIF)
4. Минимизируйте вложенность элементов
5. Используйте кэширование

### SEO
1. Правильная структура заголовков
2. Мета-теги для описания
3. Семантическая разметка
4. Оптимизированные URL
5. Структурированные данные

### Безопасность
1. Валидация форм на стороне сервера
2. Использование HTTPS
3. Защита от XSS-атак
4. Безопасная обработка пользовательского ввода
5. CSP (Content Security Policy)

### Мобильная верстка
1. Используйте viewport meta тег
2. Адаптивные изображения
3. Touch-friendly элементы
4. Оптимизированные размеры шрифтов
5. Медиа-запросы

## Новые возможности HTML5

### API
- Geolocation API
- Web Storage API
- Web Workers
- WebSocket API
- Canvas API
- WebGL API
- History API
- Drag and Drop API
- File API
- Web Speech API

### Элементы
- `<dialog>` - модальные окна
- `<details>` и `<summary>` - раскрывающиеся блоки
- `<datalist>` - выпадающие списки с автодополнением
- `<progress>` - индикатор прогресса
- `<meter>` - измеритель
- `<output>` - результат вычислений
- `<template>` - шаблоны для JavaScript

### Атрибуты
- `contenteditable` - редактируемый контент
- `spellcheck` - проверка орфографии
- `translate` - управление переводом
- `hidden` - скрытие элементов
- `download` - принудительная загрузка
- `autocomplete` - автозаполнение форм

## Советы по верстке
1. Используйте валидатор HTML
2. Следуйте стандартам W3C
3. Тестируйте в разных браузерах
4. Оптимизируйте для поисковых систем
5. Учитывайте доступность
6. Используйте семантическую разметку
7. Минимизируйте использование div
8. Оптимизируйте структуру DOM
9. Используйте современные возможности
10. Следите за производительностью