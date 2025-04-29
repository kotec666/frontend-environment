# TypeScript

## Полезные материалы
- [Всё о TypeScript (часть 1)](https://www.youtube.com/watch?v=dVrVY1nbFn0)
- [Всё о TypeScript (часть 2)](https://www.youtube.com/watch?v=YNlhuwRlT5U)
- [TypeScript Roadmap](https://roadmap.sh/typescript)
- [Эти задачи по TypeScript точно попадутся вам на собеседовании!](https://www.youtube.com/watch?v=rLwVcc0-WBU)

## Основные концепции

### 1. Типы и интерфейсы

#### Различия между type и interface
- **type**:
  - Не может быть объявлен повторно
  - Используется для создания псевдонимов типов
  - Поддерживает union и intersection типы
  - Не поддерживает объединение через extends

- **interface**:
  - Может быть объявлен несколько раз (объединяются)
  - Используется для определения контрактов объектов
  - Поддерживает extends
  - Лучше подходит для ООП

Пример объединения интерфейсов:
```typescript
interface User {
  name: string;
  age: number;
}

interface User {
  position: string;
}

// Конечный интерфейс User будет иметь все три свойства
const user: User = {
  name: 'John',
  age: 30,
  position: 'Developer'
};
```

### 2. Утилитарные типы (Utility Types)

#### Основные утилитарные типы:
- **Partial<T>** - делает все свойства необязательными
- **Pick<T, K>** - выбирает указанные свойства
- **Omit<T, K>** - исключает указанные свойства
- **Record<K, T>** - создает тип с указанными ключами и значениями
- **Required<T>** - делает все свойства обязательными
- **Readonly<T>** - делает все свойства только для чтения
- **ReturnType<T>** - получает тип возвращаемого значения функции
- **Awaited<T>** - получает тип значения из Promise
- **Exclude<T, U>** - исключает типы из union
- **Extract<T, U>** - извлекает типы из union
- **NonNullable<T>** - исключает null и undefined
- **Parameters<T>** - получает типы параметров функции

Примеры использования:
```typescript
// Partial
type PartialUser = Partial<User>; // Все свойства становятся необязательными

// Pick
type UserName = Pick<User, 'name'>; // { name: string }

// Omit
type UserWithoutAge = Omit<User, 'age'>; // { name: string, position: string }

// Record
type UserRoles = Record<string, boolean>; // { [key: string]: boolean }
```

### 3. Перегрузка функций

Пример перегрузки функции:
```typescript
function greet(name: string): string;
function greet(age: number): string;
function greet(value: string | number): string {
  if (typeof value === 'string') {
    return `Hello, ${value}!`;
  } else {
    return `You are ${value} years old!`;
  }
}
```

### 4. Условные типы и маппинг

```typescript
// Условный тип
type IsNumber<T> = T extends number ? true : false;

// Кортежи
const arr: [number, string] = [4, 'test'];

// Своя реализация Partial
type PartialOwn<T> = {
  [P in keyof T]?: T[P]
};

// Своя реализация Required
type RequiredOwn<T> = {
  [P in keyof T]-?: T[P]
};
```

### 5. Типы any и unknown

- **any**:
  - Отключает проверку типов
  - Следует избегать использования
  - Похож на обычный JavaScript

- **unknown**:
  - Безопасная альтернатива any
  - Требует явного приведения типов
  - Используется для работы с внешними данными

Пример:
```typescript
function processData(data: unknown): string {
  if (typeof data === 'string') {
    return data.toUpperCase();
  }
  return 'Unknown data type';
}
```

### 6. Модификаторы доступа в классах

| Модификатор | Доступность | Описание |
|-------------|-------------|----------|
| public      | Везде       | По умолчанию, доступен везде |
| protected   | Класс + наследники | Не доступен из экземпляров |
| private     | Только класс | Не доступен даже в наследниках |
| readonly    | -           | Только для чтения |
| static      | -           | Принадлежит классу, а не экземплярам |

### 7. Type Guards

```typescript
// Пользовательский type guard
function isString(value: string | number): value is string {
  return typeof value === 'string';
}

function printValue(value: string | number): void {
  if (isString(value)) {
    console.log(value.toUpperCase());
  } else {
    console.log(value);
  }
}
```

### 8. Типы свойств класса

```typescript
class User {
  constructor(name: string, age: number) {
    // ...
  }
}

// Получение типов параметров конструктора
type UserParams = ConstructorParameters<typeof User>; // [string, number]
```

### 9. Awaited

```typescript
// Получение типа из Promise
type UserId = Awaited<Promise<{ id: number }>>; // { id: number }
```

## Лучшие практики

1. **Избегайте any**:
   - Используйте unknown для внешних данных
   - Используйте конкретные типы везде, где возможно

2. **Используйте интерфейсы для объектов**:
   - Они лучше подходят для ООП
   - Поддерживают объединение
   - Более читаемы

3. **Используйте type guards**:
   - Для безопасной работы с union типами
   - Для проверки типов в runtime

4. **Применяйте утилитарные типы**:
   - Для создания производных типов
   - Для работы с существующими типами

5. **Используйте readonly**:
   - Для неизменяемых свойств
   - Для предотвращения случайных изменений

6. **Документируйте сложные типы**:
   - Используйте комментарии
   - Объясняйте сложные конструкции

7. **Тестируйте типы**:
   - Используйте утилиты для проверки типов
   - Проверяйте edge cases

