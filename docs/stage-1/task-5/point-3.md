# Настройка CI/CD pipeline

## Что нужно сделать

Я настраиваю CI/CD pipeline используя GitHub Actions для автоматического запуска тестов, проверки кода и деплоя при изменениях в репозитории.

## Почему CI/CD важен

CI/CD pipeline необходим потому что:
- Автоматическое тестирование при каждом коммите
- Выявление ошибок до деплоя
- Единообразие окружения для тестов
- Автоматизация рутинных задач
- Уверенность в качестве кода
- Ускорение процесса разработки

## Пошаговая инструкция

1. Создаю структуру для GitHub Actions:
   ```bash
   mkdir -p .github/workflows
   ```

2. Создаю базовый workflow файл:
   ```bash
   touch .github/workflows/ci.yml
   ```

3. Настраиваю CI workflow:
   ```yaml
   name: CI

   on:
     push:
       branches: [ main, develop ]
     pull_request:
       branches: [ main, develop ]

   jobs:
     tests:
       runs-on: ubuntu-latest

       services:
         mysql:
           image: mysql:8.0
           env:
             MYSQL_ROOT_PASSWORD: password
             MYSQL_DATABASE: testing
           ports:
             - 3306:3306
           options: --health-cmd="mysqladmin ping" --health-interval=10s --health-timeout=5s --health-retries=3

         redis:
           image: redis:7
           ports:
             - 6379:6379
           options: --health-cmd="redis-cli ping" --health-interval=10s --health-timeout=5s --health-retries=3

       steps:
         - name: Checkout code
           uses: actions/checkout@v4

         - name: Setup PHP
           uses: shivammathur/setup-php@v2
           with:
             php-version: '8.4'
             extensions: mbstring, xml, bcmath, pdo, pdo_mysql
             coverage: none

         - name: Copy .env
           run: php -r "file_exists('.env') || copy('.env.example', '.env');"

         - name: Install Dependencies
           run: composer install -q --no-ansi --no-interaction --no-scripts --no-progress --prefer-dist

         - name: Generate key
           run: php artisan key:generate

         - name: Directory Permissions
           run: chmod -R 755 storage bootstrap/cache

         - name: Create Database
           run: |
             mkdir -p database
             touch database/database.sqlite

         - name: Execute tests (Unit and Feature tests) via PHPUnit
           env:
             DB_CONNECTION: sqlite
             DB_DATABASE: database/database.sqlite
           run: vendor/bin/phpunit

         - name: Code Style Check
           run: vendor/bin/pint --test

         - name: Setup Node
           uses: actions/setup-node@v4
           with:
             node-version: '20'

         - name: Install NPM Dependencies
           run: npm ci

         - name: Lint JavaScript
           run: npm run lint
   ```

4. Создаю workflow для деплоя (опционально):
   ```bash
   touch .github/workflows/deploy.yml
   ```

5. Настраиваю deploy workflow (если нужно):
   ```yaml
   name: Deploy

   on:
     push:
       branches: [ main ]

   jobs:
     deploy:
       runs-on: ubuntu-latest
       if: github.ref == 'refs/heads/main'

       steps:
         - name: Checkout code
           uses: actions/checkout@v4

         - name: Deploy to server
           # Добавляю шаги для деплоя на сервер
   ```

6. Коммичу workflow файлы:
   ```bash
   git add .github/workflows/
   git commit -m "Add CI/CD pipeline"
   ```

7. Пушаю в репозиторий:
   ```bash
   git push origin main
   ```

8. Проверяю выполнение workflow:
   Открываю GitHub репозиторий.
   Перехожу во вкладку "Actions".
   Проверяю, что workflow запустился и выполнился успешно.

9. Тестирую локально (опционально):
   Устанавливаю act для локального запуска GitHub Actions:
   ```bash
   brew install act
   ```
   Запускаю:
   ```bash
   act
   ```

10. Настраиваю секреты (если нужны):
    В GitHub репозитории:
    Settings → Secrets and variables → Actions
    Добавляю необходимые секреты (токены, пароли и т.д.)

11. Добавляю бейджи в README (опционально):
    ```markdown
    ![CI](https://github.com/username/repo/workflows/CI/badge.svg)
    ```

## Что проверить после выполнения

- [ ] Workflow файлы созданы
- [ ] Workflow запускается при push
- [ ] Тесты выполняются в CI
- [ ] Проверка стиля кода работает
- [ ] Нет ошибок в выполнении workflow
- [ ] Уведомления о статусе приходят (если настроены)

## Структура workflow

Workflow состоит из:
- **Triggers** - когда запускается (push, pull_request)
- **Jobs** - задачи для выполнения
- **Steps** - отдельные шаги в задаче
- **Services** - дополнительные сервисы (MySQL, Redis)

## Дополнительные настройки

Могу добавить:
- Кэширование зависимостей для ускорения
- Параллельное выполнение тестов
- Отправка уведомлений (email, Slack)
- Автоматический деплой при успешных тестах
- Проверка безопасности (dependabot, security scanning)

## Важные замечания

- Храню секреты в GitHub Secrets, а не в коде
- Использую правильные версии PHP и зависимостей
- Настраиваю правильные права доступа для деплоя
- Тестирую workflow перед использованием в production
- Мониторю выполнение workflow

## Альтернативы

Если не использую GitHub, могу настроить:
- GitLab CI/CD
- Jenkins
- Bitbucket Pipelines
- CircleCI
- Travis CI

