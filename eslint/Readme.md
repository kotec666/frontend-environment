# Плагины eslint

## Основные плагины
- [eslint-plugin-jsx-a11y - плагин для работы с доступностью в JSX](https://www.npmjs.com/package/eslint-plugin-jsx-a11y)
- [eslint-plugin-react – правила для React (хуки, JSX и т. д.).](https://www.npmjs.com/package/eslint-plugin-react)
- [eslint-plugin-react-hooks – проверка правил хуков (useEffect, useMemo и др.).](https://www.npmjs.com/package/eslint-plugin-react-hooks)
- [eslint-plugin-import – проверка импортов (порядок, ошибки в путях и т. д.).](https://www.npmjs.com/package/eslint-plugin-import)
- [eslint-plugin-next – официальный плагин для Next.js (оптимизации, ошибки в next/link и др.).](https://www.npmjs.com/package/@next/eslint-plugin-next)

## Стилистические и форматирующие
- [eslint-plugin-prettier – интеграция Prettier в ESLint.](https://www.npmjs.com/package/eslint-plugin-prettier)
- [eslint-config-prettier – отключает правила ESLint, которые конфликтуют с Prettier.](https://www.npmjs.com/package/eslint-config-prettier)

## Типизация (TypeScript)
- [@typescript-eslint/eslint-plugin – правила для TypeScript.](https://www.npmjs.com/package/@typescript-eslint/eslint-plugin)
- [@typescript-eslint/parser – парсер TS для ESLint.](https://www.npmjs.com/package/@typescript-eslint/parser)

## Тестирование (Jest, Testing Library)
- [eslint-plugin-jest – правила для тестов Jest.](https://www.npmjs.com/package/eslint-plugin-jest)
- [eslint-plugin-testing-library – лучшие практики для Testing Library.](https://www.npmjs.com/package/eslint-plugin-testing-library)

## Безопасность и производительность
- [eslint-plugin-security – поиск уязвимостей в коде.](https://www.npmjs.com/package/eslint-plugin-security)
- [eslint-plugin-sonarjs – обнаружение проблем (дублирование, сложность кода).](https://www.npmjs.com/package/eslint-plugin-sonarjs)

```jsx
    const funcName = () => {}

    useEffect(() => {

    }, [funcName]) // please wrap function in useCallback first
```
    