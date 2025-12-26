# Создание миграций базы данных

## Что нужно сделать

Я создаю миграции базы данных для всех основных таблиц проекта согласно схеме из плана реализации. Создаю миграции для пользователей, планов, подписок, платежей, языков, фреймворков, категорий, вопросов и других сущностей.

## Почему миграции важны

Миграции базы данных необходимы потому что:
- Версионирование структуры базы данных
- Возможность отката изменений
- Единообразие структуры на всех окружениях
- Автоматическое применение изменений
- Документация структуры БД в коде
- Упрощение работы в команде

## Пошаговая инструкция

1. Изучаю схему базы данных из плана реализации:
   Открываю `docs/implementation_plan.md` и нахожу раздел "Структура базы данных" для понимания всех таблиц и их связей.

2. Создаю миграцию для таблицы users (если еще не создана):
   ```bash
   php artisan make:migration create_users_table
   ```
   Laravel может уже иметь базовую миграцию users, проверяю это.

3. Создаю миграцию для таблицы plans:
   ```bash
   php artisan make:migration create_plans_table
   ```

4. Создаю миграцию для таблицы subscriptions:
   ```bash
   php artisan make:migration create_subscriptions_table
   ```

5. Создаю миграцию для таблицы payments:
   ```bash
   php artisan make:migration create_payments_table
   ```

6. Создаю миграцию для таблицы programming_languages:
   ```bash
   php artisan make:migration create_programming_languages_table
   ```

7. Создаю миграцию для таблицы frameworks:
   ```bash
   php artisan make:migration create_frameworks_table
   ```

8. Создаю миграцию для таблицы question_categories:
   ```bash
   php artisan make:migration create_question_categories_table
   ```

9. Создаю миграцию для таблицы questions:
   ```bash
   php artisan make:migration create_questions_table
   ```

10. Создаю миграцию для таблицы question_options:
    ```bash
    php artisan make:migration create_question_options_table
    ```

11. Создаю миграцию для таблицы test_cases:
    ```bash
    php artisan make:migration create_test_cases_table
    ```

12. Создаю миграцию для таблицы social_providers:
    ```bash
    php artisan make:migration create_social_providers_table
    ```

13. Создаю миграцию для таблицы user_technology_preferences:
    ```bash
    php artisan make:migration create_user_technology_preferences_table
    ```

14. Заполняю каждую миграцию согласно схеме:
    Открываю каждую миграцию и заполняю метод `up()` с полями таблицы согласно плану реализации.

15. Пример заполнения миграции plans:
    ```php
    public function up(): void
    {
        Schema::create('plans', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('slug')->unique();
            $table->text('description')->nullable();
            $table->decimal('price', 10, 2);
            $table->string('interval'); // monthly, yearly
            $table->integer('trial_days')->default(0);
            $table->json('features')->nullable();
            $table->boolean('is_active')->default(true);
            $table->integer('sort_order')->default(0);
            $table->timestamps();
        });
    }
    ```

16. Проверяю синтаксис миграций:
    ```bash
    php artisan migrate:status
    ```
    Показывает статус всех миграций.

17. Выполняю миграции:
    ```bash
    php artisan migrate
    ```
    Это создаст все таблицы в базе данных.

18. Проверяю, что таблицы созданы:
    ```bash
    php artisan db:show
    ```
    Или подключаюсь к MySQL и проверяю:
    ```sql
    SHOW TABLES;
    ```

## Что проверить после выполнения

- [ ] Все миграции созданы
- [ ] Миграции заполнены согласно схеме
- [ ] Миграции выполнены без ошибок
- [ ] Все таблицы созданы в базе данных
- [ ] Связи между таблицами настроены (foreign keys)
- [ ] Индексы созданы для часто используемых полей

## Важные замечания

- Создаю миграции в правильном порядке (сначала таблицы без foreign keys)
- Использую правильные типы данных для полей
- Добавляю индексы для полей, по которым будут поиски
- Использую timestamps() для created_at и updated_at
- Проверяю, что все nullable поля помечены правильно

## Порядок создания миграций

Рекомендуемый порядок (с учетом зависимостей):
1. users
2. programming_languages
3. frameworks
4. plans
5. question_categories
6. questions
7. subscriptions (зависит от users и plans)
8. payments (зависит от users и subscriptions)
9. question_options (зависит от questions)
10. test_cases (зависит от questions)
11. social_providers (зависит от users)
12. user_technology_preferences (зависит от users, languages, frameworks)

