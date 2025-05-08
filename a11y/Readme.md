# Руководство по доступности (a11y)

## Полезные материалы
- [Learn Accessibility - Full a11y Tutorial](https://www.youtube.com/watch?v=e2nkq3h1P68)
- [The Only Accessibility Video You Will Ever Need](https://www.youtube.com/watch?v=2oiBKSjOOFE)
- [Доступность элементов через клавиатуру, Aria, семантика](https://www.youtube.com/watch?v=VyWRmepESoQ)
- [Введение в ARIA](https://doka.guide/a11y/aria-intro/)
- [Возможные дополнения ARIA атрибутов к html элементам](https://russmaxdesign.github.io/html-elements-names/)
- [Периодическая таблица семантики](https://gerardkcohen.github.io/periodic-table-of-semantics.html)
- [Всевозможные aria-* и области их применения](https://docs.google.com/spreadsheets/d/1Q4qjn8v6xiHIb0Gv2kI3LF-dp-eiJBo0zLwBJCK7gVI/edit?usp=sharing)
- [react-aria библиотека для создания accessibility компонентов](https://www.npmjs.com/package/react-aria)
- [react-aria-components библиотека с accessibility компонентами](https://www.npmjs.com/package/react-aria-components)
- [react-aria/dialog для создания accessibility модалок](https://www.npmjs.com/package/@react-aria/dialog)
- [focus-trap-react библиотека для создания accessibility модальных окон](https://www.npmjs.com/package/focus-trap-react)

## Инструменты разработчика
- Дерево доступности: F12 -> accessibility
- Эмуляция плохого зрения: CTRL + SHIFT + P в F12 -> Rendering

## Инструменты проверки доступности
- Accessibility tree
- Contrast checkers
- Screen readers
- Автоматизированные инструменты:
  - Lighthouse
  - Accessibility inspector
  - aXe

## Основные принципы доступности
1. **Контраст цветов**
2. **Адекватные alt-атрибуты** у изображений
   - Для декоративных изображений описание не требуется
   - Можно добавить точку в конец текста alt для паузы
3. **Ссылки** должны быть семантически корректными
4. **Фокус** должен работать через TAB для кнопок, ссылок, инпутов
5. **Семантический HTML**:
   - Использование списков через `ul li`
   - `label` для инпутов
6. **Размер текста** в rem
7. **Заголовки** h1-h6:
   - Последовательная нумерация
   - Один h1 на странице
   - Использование для структуры, а не оформления
8. **ARIA атрибуты**:
   - Предпочтительно использовать семантику HTML
   - Применять только при необходимости (кастомные чекбоксы/свичи)
9. **Aria live**:
   - `off` - изменения по умолчанию
   - `polite` - вежливое изменение без прерываний
   - `assertive` - срочные уведомления с прерыванием
10. **Управление фокусом** после действий (форма, модальное окно)

## Примеры использования

### Пустые ссылки
```html
<a href="https://" aria-label="Перейти на страницу с каталогом продуктов"></a>
```

### Кастомные элементы
```html
<!-- Кнопка -->
<button type="button">Обычная кнопка</button>
<div role="button" tabindex="0">Click Me</div>

<!-- Свитчер -->
<div role="switch" aria-checked="false" tabindex="0">Light mode</div>
```

### Aria live
```html
<button aria-controls="submitMessage">Отправить</button>
<p id="submitMessage" aria-live="polite">Форма отправлена</p>
```

## Доступность модальных окон
- `role="dialog"` или `aria-modal="true"`
- Возврат фокуса к кнопке открытия
- Запрет фокуса на элементах под модалкой (`inert`)

## ARIA роли и атрибуты
- `role="dialog"`
- `role="button"` (лучше использовать `<button>`)
- `role="list"`
- `role="listitem"`
- `tabindex="0"` (использовать только 0)
- `tabindex="-1"` (для программного управления фокусом через js)
- `type="button"`

## Примеры aria-labelledby и aria-describedby
```html
<!-- aria-labelledby -->
<div id="te">Заполните имя</div>
<input aria-labelledby="te" type="text" />

<section aria-labelledby="products-heading">
  <h2 id="products-heading">Products</h2>
</section>

<!-- aria-describedby -->
<input aria-describedby="sort-ascending-desc" />
<span id="sort-ascending-desc" hidden>sort products in ascending order by price</span>
```

## Таблица Атрибуты состояния
| Атрибут            | Описание                                                                                                                                                                                   | Пример                                   |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| aria-hidden        | Указывает, что элемент и его дочерние элементы не должны быть доступны для вспомогательных технологий (не путать с display: none, который полностью удаляет элемент из дерева доступности) | aria-hidden="true"                       |
| aria-disabled      | Указывает, что элемент недоступен для взаимодействия                                                                                                                                       | aria-disabled="true"                     |
| aria-checked       | Состояние чекбокса/радиокнопки (true/false/mixed)                                                                                                                                          | aria-checked="true"                      |
| aria-selected      | Указывает, выбран ли элемент (вкладка, пункт списка)                                                                                                                                       | aria-selected="true"                     |
| aria-expanded      | Показывает, раскрыт ли элемент (выпадающее меню, аккордеон)                                                                                                                                | aria-expanded="false"                    |
| aria-pressed       | Состояние кнопки-переключателя (true/false/mixed)                                                                                                                                          | aria-pressed="true"                      |
| aria-invalid       | Указывает на ошибку ввода (true/false/grammar/spelling)                                                                                                                                    | aria-invalid="true"                      |
| aria-errormessage  | Описывает ошибку ввода                                                                                                                                                                     | aria-errormessage="id на span с ошибкой" |
| aria-busy          | Элемент обновляется (например, при загрузке данных)                                                                                                                                        | aria-busy="true"                         |
| aria-modal         | Указывает, что элемент является модальным (блокирует взаимодействие с остальной частью страницы)                                                                                           | aria-modal="true"                        |
| aria-grabbed       | Используется в drag-and-drop для обозначения, что элемент захвачен (устаревший, но иногда встречается)                                                                                     | aria-grabbed="true"                      |

## Таблица Атрибуты свойств
| Атрибут                                     | Описание                                                                              | Пример                           |
|---------------------------------------------|---------------------------------------------------------------------------------------|----------------------------------|
| aria-label                                  | Задаёт текстовую метку для элемента (если видимого текста нет)                        | aria-label="Закрыть"             |
| aria-labelledby                             | Связывает элемент с другим, который его описывает (по id)                             | aria-labelledby="title-id"       |
| aria-describedby                            | Указывает на дополнительное описание элемента                                         | aria-describedby="hint-id"       |
| aria-live                                   | Определяет, как скринридер объявляет динамические изменения (off/polite/assertive)    | aria-live="polite"               |
| aria-atomic                                 | В сочетании с aria-live указывает, читать ли весь контейнер или только изменения      | aria-atomic="true"               |
| aria-controls                               | Указывает, какой элемент контролируется (например, кнопка меню → контейнер)           | aria-controls="menu-id"          |
| aria-haspopup                               | Указывает, что у элемента есть всплывающее окно (true/dialog/menu/listbox/tree)       | aria-haspopup="menu"             |
| aria-current                                | Указывает текущий элемент (страницу, шаг и т. д.) (true/page/step/location/date/time) | aria-current="page"              |
| aria-required                               | Поле обязательно для заполнения                                                       | aria-required="true"             |
| aria-multiline                              | Указывает, поддерживает ли textarea многострочный ввод                                | aria-multiline="true"            |
| aria-multiselectable                        | Можно ли выбрать несколько элементов (в списке, таблице)                              | aria-multiselectable="true"      |
| aria-orientation                            | Ориентация элемента (horizontal/vertical)                                             | aria-orientation="horizontal"    |
| aria-placeholder                            | Подсказка в поле ввода (аналог placeholder)                                           | aria-placeholder="Введите имя"   |
| aria-readonly                               | Элемент только для чтения                                                             | aria-readonly="true"             |
| aria-sort                                   | Указывает тип сортировки в таблице (none/ascending/descending/other)                  | aria-sort="ascending"            |
| aria-valuemin, aria-valuemax, aria-valuenow | Минимальное, максимальное и текущее значение (для прогрессбаров, слайдеров)           | aria-valuenow="50"               |
| aria-valuetext                              | Текстовое описание текущего значения                                                  | aria-valuetext="Средний уровень" |
| aria-roledescription                        | Кастомное описание роли элемента (например, "карусель" вместо "группа")               | aria-roledescription="carousel"  |
| aria-keyshortcuts                           | Указывает клавиатурные сочетания для элемента                                         | aria-keyshortcuts="Ctrl+Enter"   |
| aria-details                                | Ссылка на элемент с дополнительной информацией (например, развёрнутое описание)       | aria-details="more-info-id"      |

## Таблица Атрибуты отношений
| Атрибут                      | Описание                                                                                                   | Пример                           |
|------------------------------|------------------------------------------------------------------------------------------------------------|----------------------------------|
| aria-owns                    | Указывает, что элемент "владеет" другим (например, выпадающий список)                                      | aria-owns="dropdown-id"          |
| aria-activedescendant        | Указывает активный дочерний элемент (в списке, меню)                                                       | aria-activedescendant="item1"    |
| aria-colindex                | Позиция столбца в таблице (если не все загружены)                                                          | aria-colindex="2"                |
| aria-rowindex                | Позиция строки в таблице                                                                                   | aria-rowindex="5"                |
| aria-colcount, aria-rowcount | Общее количество столбцов/строк в таблице                                                                  | aria-rowcount="100"              |
| aria-posinset                | Позиция в наборе (например, вкладка 2 из 5)                                                                | aria-posinset="3"                |
| aria-setsize                 | Общий размер набора                                                                                        | aria-setsize="5"                 |
| aria-flowto                  | Указывает следующий логический элемент в последовательности (если порядок чтения должен отличаться от DOM) | aria-flowto="next-section-id"    |
| aria-relevant                | В сочетании с aria-live указывает, какие изменения должны объявляться (additions, removals, text, all)     | aria-relevant="additions text"   |

## Советы по доступности
- ARIA состоит из трёх частей: ролей, состояний и свойств
- Для элементов с `overflow: auto` добавлять `tabindex="0"` для взаимодействия с прокруткой через клавиатуру
- Включать субтитры для видео по умолчанию
- Использовать `autoFocus` для главного инпута
- Использовать `eslint-plugin-jsx-a11y` для проверки доступности в JSX

