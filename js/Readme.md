# JavaScript: Основы и Современные Практики

## Содержание
1. [Типы данных](#типы-данных)
2. [Функции](#функции)
3. [Область видимости](#область-видимости)
4. [События DOM](#события-dom)
5. [Работа с данными](#работа-с-данными)
6. [Асинхронное программирование](#асинхронное-программирование)
7. [ООП в JavaScript](#ооп-в-javascript)
8. [Продвинутые концепции](#продвинутые-концепции)

## Типы данных

### Примитивные типы (8 шт.)
- `number` - числа (целые и с плавающей точкой)
- `string` - строки
- `boolean` - логические значения
- `null` - отсутствие значения
- `undefined` - неопределенное значение
- `symbol` - уникальные идентификаторы
- `bigint` - большие целые числа

### Truthy и Falsy значения
```javascript
// Falsy значения
false
0
""
null
undefined
NaN

// Фильтрация falsy значений
const arr = [false, 0, '', null, undefined, NaN, 1];
const filtered = arr.filter(Boolean); // [1]
```

## Функции

### Способы объявления
```javascript
// Function Declaration
function sum(a, b) {
    return a + b;
}

// Function Expression
const sum = function(a, b) {
    return a + b;
};

// Arrow Function
const sum = (a, b) => a + b;

// IIFE (Immediately Invoked Function Expression)
(function() {
    console.log('Выполняется сразу');
})();
```

### Особенности функций
| Тип функции           | Всплытие (Hoisting) | Контекст (this) | Аргументы (arguments) | Доступность до объявления |
|-----------------------|---------------------|-----------------|-----------------------|---------------------------|
| Function Declaration  | Да                  | Да              | Да                    | Да                        |
| Function Expression   | Нет                 | Да              | Да                    | Нет                       |
| Arrow Function        | Нет                 | Нет             | Нет                   | Нет                       |
| IIFE                  | Нет                 | Да              | Да                    | Нет                       |

### Замыкания
```javascript
Определение: замыкание - это ф-я, которая захватывает переменные из своей области видимости
в момент создания, и может использовать их позже, даже после того,
как выполнение функции, создавшей эти переменные, завершено

function createCounter() {
    let count = 0;
    return function() {
        return ++count;
    };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

## Область видимости

### var vs let/const
```javascript
// var - функциональная область видимости
function example() {
    if (true) {
        var x = 5;
    }
    console.log(x); // 5
}

// let/const - блочная область видимости
function example() {
    if (true) {
        let x = 5;
    }
    console.log(x); // ReferenceError
}
```

### Сравнение переменных
| Тип переменной | Всплытие (Hoisting) | Область видимости | Повторное объявление | Изменение значения |
|---------------|-------------------|------------------|---------------------|-------------------|
| var           | Да (undefined)    | Функциональная   | Да                  | Да                |
| let           | Нет (TDZ)         | Блочная          | Нет                 | Да                |
| const         | Нет (TDZ)         | Блочная          | Нет                 | Нет               |

> TDZ (Temporal Dead Zone) - временная мертвая зона, период между созданием переменной и её инициализацией, когда к ней нельзя обратиться

## События DOM

### Всплытие и Погружение
```javascript
// Погружение (capturing)
parent.addEventListener('click', () => {
    console.log('Parent (capture)');
}, true);

// Всплытие (bubbling)
child.addEventListener('click', () => {
    console.log('Child (bubble)');
});

// Остановка всплытия
element.addEventListener('click', (event) => {
    event.stopPropagation();
    // event.stopImmediatePropagation() – также предотвращает вызов других обработчиков на этом элементе.
});
```

### Делегирование событий
```javascript
document.querySelector('.parent').addEventListener('click', (event) => {
    if (event.target.matches('.child')) {
        console.log('Child clicked');
    }
});
```

## Работа с данными

### HTTP-статусы
- 100-199: Информационные
- 200-299: Успешные
- 300-399: Перенаправления
- 400-499: Ошибки клиента
- 500-599: Ошибки сервера

### Хранение данных
```javascript
// LocalStorage
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');

// SessionStorage
sessionStorage.setItem('key', 'value');

// Cookies
document.cookie = 'name=value; expires=Fri, 31 Dec 2023 23:59:59 GMT';
```

## Асинхронное программирование

### Promise
```javascript
// Создание Promise
const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve('Success'), 1000);
});

// Promise.withResolvers()
const { promise, resolve, reject } = Promise.withResolvers();

// Обработка Promise
promise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => console.log('Done'));

// Promise.all
Promise.all([promise1, promise2])
    .then(results => console.log(results));

// Promise.race
Promise.race([promise1, promise2])
    .then(result => console.log(result));
```

### Event Loop и типы задач
```javascript
// Тело промиса выполняется синхронно
new Promise((resolve) => {
    console.log('Синхронный код');
    resolve();
}).then(() => {
    console.log('Микротаска');
});

// Микротаски (выполняются сразу после текущего макрозадания)
- Promise
- IntersectionObserver
- MutationObserver
- queueMicrotask()

// Макротаски (выполняются в следующей итерации event loop)
- XMLHTTPRequest
- setTimeout
- setInterval
- События DOM
```

### Сравнение методов Promise
| Метод                 | Описание                                 | Обработка ошибок | Возвращаемое значение                          |
|-----------------------|------------------------------------------|------------------|------------------------------------------------|
| Promise.all           | Ждет выполнения всех промисов            | Прерывается при первой ошибке | Массив результатов                |
| Promise.race          | Возвращает первый выполненный промис     | Обрабатывает ошибку первого завершенного | Результат первого промиса|
| Promise.any           | Возвращает первый успешный промис        | Обрабатывает все ошибки | Результат первого успешного промиса      |
| Promise.allSettled    | Ждет завершения всех промисов            | Обрабатывает все ошибки | Массив объектов с результатами и статусами|
| Promise.withResolvers | Создает промис с методами resolve/reject | Стандартная обработка ошибок | Объект { promise, resolve, reject }  |

### async/await
```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

## ООП в JavaScript

### Основные принципы
- **Абстракция**: Сокрытие деталей реализации
- **Инкапсуляция**: Защита данных
- **Наследование**: Переиспользование кода
- **Полиморфизм**: Разные реализации одного интерфейса

### Классы
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a sound`);
    }
}

class Dog extends Animal {
    speak() {
        console.log(`${this.name} barks`);
    }
}
```

## Продвинутые концепции

### SOLID принципы
1. **Single Responsibility**:  Единственная ответственность, класс должен отвечать за работу с одной сущностью
2. **Open/Closed**: Открыт для расширения, закрыт для модификации
3. **Liskov Substitution**: Подклассы должны быть заменяемы базовыми классами
4. **Interface Segregation**: Много интерфейсов лучше одного общего
5. **Dependency Inversion**: Зависимость от абстракций, а не от конкретных реализаций

### Работа с контекстом
Контекст биндится один раз .bind({ age: 24 }).bind({ age: 25 }) // Итог: { age: 24 }
```javascript
const obj = {
    name: 'Object',
    getName: function() {
        return this.name;
    }
};

// call
obj.getName.call({ name: 'New Object' });

// apply
obj.getName.apply({ name: 'New Object' });

// bind
const boundGetName = obj.getName.bind({ name: 'New Object' });
```

### Сравнение методов работы с контекстом
| Метод | Описание                                       | Аргументы                                              | Момент вызова            | Возвращаемое значение                  |
|-------|------------------------------------------------|--------------------------------------------------------|--------------------------|----------------------------------------|
| call  | Вызывает функцию с указанным контекстом        | Принимает контекст и список аргументов через запятую   | Сразу                    | Результат выполнения функции           |
| apply | Вызывает функцию с указанным контекстом        | Принимает контекст и массив аргументов                 | Сразу                    | Результат выполнения функции           |
| bind  | Создает новую функцию с привязанным контекстом | Принимает контекст и список аргументов через запятую   | При вызове новой функции | Новая функция с привязанным контекстом |

### Глубокое копирование
```javascript
// JSON
const deepCopy = JSON.parse(JSON.stringify(original));

// structuredClone
const deepCopy = structuredClone(original);

// Lodash
const deepCopy = _.cloneDeep(original);
```

### Ключевое слово **new** нужно, чтобы:
1. **Создать новый объект.**
2. **Настроить его прототипную связь.**
3. **Привязать контекст this к этому объекту.**

Без **new** функция-конструктор ведёт себя как обычная функция, что обычно ломает логику.

### Сборка мусора
- Автоматическое освобождение памяти
- Удаление недостижимых объектов
- Предотвращение утечек памяти
- garbage collector - отслеживает достижимость, если на объект больше нет ссылок (прямых или косвенных), он считается "мусором". GC удаляет недостижимые объекты, чтобы предотвратить утечки памяти.

### Процесс загрузки страницы (после ввода URL в адресную строку)
1. Браузер обращается в локальный кэш, известен ли этот сайт
2. Если пусто, проверяется /etc/host, где мы указываем соответствие доменов ip-адресам
3. Обращение к DNS-серверу (глобальный/региональный)
4. Установка TCP-соединения
5. HTTP запрос на получение контента
6. Загрузка ресурсов, рендеринг страницы
