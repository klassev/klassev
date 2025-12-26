# Настройка линтеров (PHP CS Fixer, ESLint)

## Что нужно сделать

Я настраиваю линтеры для автоматического форматирования и проверки кода: Laravel Pint для PHP и ESLint для JavaScript. Это обеспечит единообразие стиля кода в проекте.

## Почему линтеры важны

Линтеры необходимы потому что:
- Единообразие стиля кода в проекте
- Автоматическое исправление форматирования
- Выявление потенциальных ошибок
- Соответствие стандартам кодирования (PSR для PHP)
- Улучшение читаемости кода
- Упрощение code review

## Пошаговая инструкция

### Настройка Laravel Pint (PHP)

1. Проверяю, установлен ли Laravel Pint:
   ```bash
   ./vendor/bin/pint --version
   ```
   Pint обычно устанавливается автоматически с Laravel 11+.

2. Создаю конфигурационный файл pint.json:
   ```bash
   touch pint.json
   ```

3. Настраиваю pint.json:
   ```json
   {
       "preset": "laravel",
       "rules": {
           "array_syntax": {
               "syntax": "short"
           },
           "ordered_imports": {
               "sort_algorithm": "alpha"
           }
       }
   }
   ```

4. Запускаю Pint для проверки кода:
   ```bash
   ./vendor/bin/pint --test
   ```
   Показывает, какие файлы будут изменены.

5. Запускаю Pint для исправления кода:
   ```bash
   ./vendor/bin/pint
   ```
   Автоматически исправляет форматирование.

6. Запускаю Pint для конкретной папки:
   ```bash
   ./vendor/bin/pint app/
   ```

7. Проверяю результат:
   ```bash
   git diff
   ```
   Вижу изменения, которые внес Pint.

### Настройка ESLint (JavaScript)

1. Проверяю, установлен ли Node.js:
   ```bash
   node --version
   npm --version
   ```

2. Проверяю package.json:
   ```bash
   cat package.json
   ```
   ESLint может быть уже установлен.

3. Устанавливаю ESLint (если не установлен):
   ```bash
   npm install --save-dev eslint
   ```

4. Инициализирую ESLint:
   ```bash
   npx eslint --init
   ```
   Отвечаю на вопросы:
   - How would you like to use ESLint? (To check syntax, find problems, and enforce code style)
   - What type of modules? (JavaScript modules (import/export))
   - Which framework? (None или Vue.js, если использую)
   - Does your project use TypeScript? (No)
   - Where does your code run? (Browser)
   - What format do you want your config file? (JSON)

5. Создаю .eslintrc.json (если не создан автоматически):
   ```json
   {
       "env": {
           "browser": true,
           "es2021": true
       },
       "extends": "eslint:recommended",
       "parserOptions": {
           "ecmaVersion": "latest",
           "sourceType": "module"
       },
       "rules": {
           "indent": ["error", 2],
           "quotes": ["error", "single"],
           "semi": ["error", "always"]
       }
   }
   ```

6. Запускаю ESLint для проверки:
   ```bash
   npx eslint resources/js/
   ```

7. Запускаю ESLint с автоматическим исправлением:
   ```bash
   npx eslint resources/js/ --fix
   ```

8. Добавляю скрипты в package.json:
   ```json
   {
       "scripts": {
           "lint": "eslint resources/js/",
           "lint:fix": "eslint resources/js/ --fix"
       }
   }
   ```

9. Запускаю через npm:
   ```bash
   npm run lint
   npm run lint:fix
   ```

### Интеграция с Git

10. Настраиваю pre-commit hook (опционально):
    Создаю .git/hooks/pre-commit:
    ```bash
    #!/bin/sh
    ./vendor/bin/pint --test
    npm run lint
    ```
    Делаю исполняемым:
    ```bash
    chmod +x .git/hooks/pre-commit
    ```

11. Добавляю .gitignore для coverage и других файлов:
    ```bash
    echo "coverage/" >> .gitignore
    echo ".phpunit.result.cache" >> .gitignore
    ```

## Что проверить после выполнения

- [ ] Laravel Pint установлен и работает
- [ ] pint.json настроен
- [ ] Pint исправляет форматирование PHP кода
- [ ] ESLint установлен и работает
- [ ] .eslintrc.json настроен
- [ ] ESLint проверяет JavaScript код
- [ ] Скрипты в package.json работают

## Конфигурация Pint

Pint использует PHP CS Fixer под капотом. Могу настроить:
- Preset (laravel, psr12, symfony)
- Правила форматирования
- Исключения для определенных файлов/папок

## Конфигурация ESLint

ESLint позволяет настроить:
- Правила стиля кода
- Поддержку различных фреймворков (Vue, React)
- Поддержку TypeScript
- Интеграцию с Prettier (если использую)

## Важные замечания

- Запускаю линтеры перед коммитом кода
- Использую единые настройки для всей команды
- Обновляю конфигурацию при необходимости
- Не коммичу автоматически исправленные файлы без проверки

## Дополнительные инструменты

Могу также настроить:
- Prettier для форматирования JavaScript
- PHPStan для статического анализа PHP
- Psalm для анализа PHP кода

