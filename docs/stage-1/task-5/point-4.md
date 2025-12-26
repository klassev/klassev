# Документация API (Swagger)

## Что нужно сделать

Я настраиваю Swagger/OpenAPI документацию для API используя L5-Swagger, чтобы автоматически генерировать документацию из аннотаций в контроллерах.

## Почему Swagger важен

Swagger документация необходима потому что:
- Автоматическая генерация документации API
- Интерактивное тестирование API
- Единая точка входа для всех endpoints
- Упрощение интеграции для фронтенда
- Документация всегда актуальна (синхронизирована с кодом)
- Профессиональный вид документации

## Пошаговая инструкция

1. Устанавливаю L5-Swagger:
   ```bash
   composer require darkaonline/l5-swagger
   ```

2. Публикую конфигурацию:
   ```bash
   php artisan vendor:publish --provider "L5Swagger\L5SwaggerServiceProvider"
   ```

3. Проверяю конфигурационный файл:
   ```bash
   cat config/l5-swagger.php
   ```
   Должен быть создан файл конфигурации.

4. Настраиваю конфигурацию:
   ```bash
   nano config/l5-swagger.php
   ```
   Проверяю основные настройки:
   - Путь к документации
   - Версия API
   - Базовый URL

5. Генерирую документацию:
   ```bash
   php artisan l5-swagger:generate
   ```
   Это создаст файл документации из аннотаций.

6. Проверяю доступность документации:
   Открываю в браузере: https://klassev.test/api/documentation
   Должна отобразиться Swagger UI с документацией.

7. Добавляю аннотации в контроллер:
   Пример для HealthController:
   ```php
   <?php

   namespace App\Http\Controllers\Api\V1;

   /**
    * @OA\Info(
    *     title="Trener API",
    *     version="1.0.0",
    *     description="API для тренажера программирования"
    * )
    * @OA\Server(
    *     url="https://klassev.test/api/v1",
    *     description="API Server"
    * )
    * @OA\SecurityScheme(
    *     securityScheme="bearerAuth",
    *     type="http",
    *     scheme="bearer",
    *     bearerFormat="JWT"
    * )
    */
   class HealthController extends BaseController
   {
       /**
        * @OA\Get(
        *     path="/health",
        *     summary="Health check",
        *     tags={"System"},
        *     @OA\Response(
        *         response=200,
        *         description="Service is healthy",
        *         @OA\JsonContent(
        *             @OA\Property(property="success", type="boolean", example=true),
        *             @OA\Property(property="message", type="string", example="Service is healthy"),
        *             @OA\Property(
        *                 property="data",
        *                 type="object",
        *                 @OA\Property(property="status", type="string", example="ok"),
        *                 @OA\Property(property="timestamp", type="string", example="2025-01-XX..."),
        *                 @OA\Property(property="version", type="string", example="1.0.0")
        *             )
        *         )
        *     )
        * )
        */
       public function check()
       {
           return $this->success([
               'status' => 'ok',
               'timestamp' => now()->toIso8601String(),
               'version' => '1.0.0',
           ], 'Service is healthy');
       }
   }
   ```

8. Создаю базовый файл с общей информацией:
   ```bash
   touch app/Http/Controllers/Api/V1/SwaggerController.php
   ```
   Или добавляю аннотации в BaseController.

9. Добавляю аннотации для аутентификации:
   В контроллерах аутентификации добавляю:
   ```php
   /**
    * @OA\Post(
    *     path="/auth/login",
    *     summary="User login",
    *     tags={"Authentication"},
    *     @OA\RequestBody(
    *         required=true,
    *         @OA\JsonContent(
    *             @OA\Property(property="email", type="string", example="user@example.com"),
    *             @OA\Property(property="password", type="string", example="password")
    *         )
    *     ),
    *     @OA\Response(
    *         response=200,
    *         description="Login successful"
    *     )
    * )
    */
   ```

10. Регенерирую документацию:
    ```bash
    php artisan l5-swagger:generate
    ```

11. Проверяю обновленную документацию:
    Обновляю страницу https://klassev.test/api/documentation
    Проверяю, что новые endpoints отображаются.

12. Настраиваю автогенерацию при деплое (опционально):
    Добавляю в CI/CD pipeline:
    ```yaml
    - name: Generate Swagger documentation
      run: php artisan l5-swagger:generate
    ```

13. Тестирую API через Swagger UI:
    В Swagger UI нажимаю "Try it out" на любом endpoint.
    Заполняю параметры и нажимаю "Execute".
    Проверяю ответ.

14. Настраиваю экспорт документации:
    Swagger позволяет экспортировать документацию в:
    - JSON формат
    - YAML формат
    - PDF (через дополнительные инструменты)

## Что проверить после выполнения

- [ ] L5-Swagger установлен
- [ ] Конфигурация настроена
- [ ] Документация генерируется
- [ ] Swagger UI доступен по /api/documentation
- [ ] Endpoints отображаются в документации
- [ ] Можно тестировать API через Swagger UI
- [ ] Аннотации корректны

## Структура аннотаций

Основные аннотации:
- `@OA\Info` - общая информация об API
- `@OA\Server` - серверы API
- `@OA\Path` - пути endpoints
- `@OA\Parameter` - параметры запросов
- `@OA\RequestBody` - тело запроса
- `@OA\Response` - ответы
- `@OA\Schema` - схемы данных

## Важные замечания

- Регулярно обновляю документацию при изменении API
- Использую правильные типы данных в аннотациях
- Описываю все параметры и ответы
- Тестирую документацию через Swagger UI
- Храню документацию в актуальном состоянии

## Дополнительные настройки

- Настраиваю версионирование документации
- Добавляю примеры запросов и ответов
- Настраиваю авторизацию в Swagger UI
- Экспортирую документацию для внешних разработчиков

## Альтернативы

Если L5-Swagger не подходит, могу использовать:
- Scribe (Laravel)
- API Blueprint
- Postman Collections
- OpenAPI Generator

