# Настройка PHPUnit

## Что нужно сделать

Я настраиваю PHPUnit для написания и запуска тестов проекта. Проверяю конфигурацию, создаю базовые тесты и убеждаюсь, что тестирование работает.

## Почему PHPUnit важен

PHPUnit необходим потому что:
- Автоматическое тестирование кода
- Выявление ошибок на раннем этапе
- Подтверждение корректности функциональности
- Уверенность при рефакторинге
- Документация поведения кода через тесты
- Соответствие best practices разработки

## Пошаговая инструкция

1. Проверяю, установлен ли PHPUnit:
   ```bash
   ./vendor/bin/phpunit --version
   ```
   PHPUnit обычно устанавливается автоматически с Laravel.

2. Проверяю конфигурационный файл:
   ```bash
   cat phpunit.xml
   ```
   Должен быть файл phpunit.xml в корне проекта.

3. Открываю phpunit.xml для проверки настроек:
   ```bash
   nano phpunit.xml
   ```
   Проверяю основные настройки:
   - База данных для тестов
   - Окружение тестирования
   - Пути к тестам

4. Настраиваю тестовую базу данных в phpunit.xml:
   ```xml
   <php>
       <env name="APP_ENV" value="testing"/>
       <env name="DB_CONNECTION" value="sqlite"/>
       <env name="DB_DATABASE" value=":memory:"/>
       <env name="CACHE_DRIVER" value="array"/>
       <env name="SESSION_DRIVER" value="array"/>
   </php>
   ```
   Или использую отдельную тестовую базу данных MySQL.

5. Создаю тестовую базу данных (если использую MySQL):
   ```bash
   mysql -u root -p
   ```
   В MySQL:
   ```sql
   CREATE DATABASE klassev_test_testing;
   EXIT;
   ```

6. Обновляю .env.testing (если создаю):
   ```bash
   cp .env .env.testing
   nano .env.testing
   ```
   Устанавливаю:
   ```
   APP_ENV=testing
   DB_DATABASE=klassev_test_testing
   ```

7. Запускаю существующие тесты:
   ```bash
   ./vendor/bin/phpunit
   ```
   Или:
   ```bash
   php artisan test
   ```
   Должны выполниться базовые тесты Laravel.

8. Проверяю структуру папок тестов:
   ```bash
   ls -la tests/
   ```
   Должны быть:
   - Feature/ - для feature тестов
   - Unit/ - для unit тестов
   - TestCase.php - базовый класс

9. Создаю простой тест для проверки:
   ```bash
   php artisan make:test ExampleTest
   ```

10. Настраиваю тест:
    ```php
    <?php

    namespace Tests\Feature;

    use Tests\TestCase;

    class ExampleTest extends TestCase
    {
        public function test_the_application_returns_a_successful_response(): void
        {
            $response = $this->get('/');

            $response->assertStatus(200);
        }
    }
    ```

11. Запускаю конкретный тест:
    ```bash
    ./vendor/bin/phpunit tests/Feature/ExampleTest.php
    ```

12. Настраиваю coverage (опционально):
    В phpunit.xml добавляю:
    ```xml
    <coverage>
        <include>
            <directory suffix=".php">./app</directory>
        </include>
    </coverage>
    ```

13. Запускаю тесты с coverage:
    ```bash
    ./vendor/bin/phpunit --coverage-html coverage
    ```

14. Проверяю работу тестовой базы данных:
    Создаю тест, который использует базу данных:
    ```php
    public function test_database_connection(): void
    {
        $this->assertDatabaseHas('users', [
            'email' => 'admin@klassev.test',
        ]);
    }
    ```

## Что проверить после выполнения

- [ ] PHPUnit установлен и работает
- [ ] Конфигурация phpunit.xml корректна
- [ ] Тестовая база данных настроена
- [ ] Базовые тесты выполняются успешно
- [ ] Можно создавать новые тесты
- [ ] Тесты изолированы друг от друга

## Структура тестов

- **Feature тесты** - тестируют полный функционал (HTTP запросы, база данных)
- **Unit тесты** - тестируют отдельные классы и методы изолированно

## Важные замечания

- Использую отдельную базу данных для тестов
- Очищаю базу данных перед каждым тестом (RefreshDatabase trait)
- Изолирую тесты друг от друга
- Пишу тесты для критичной функциональности
- Поддерживаю тесты в актуальном состоянии

## Дополнительные настройки

- Настраиваю CI/CD для автоматического запуска тестов
- Настраиваю coverage отчеты
- Настраиваю параллельное выполнение тестов (для больших проектов)

