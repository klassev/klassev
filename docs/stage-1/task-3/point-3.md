# Настройка роутинга

## Что нужно сделать

Я настраиваю роутинг для API, создаю структуру версионирования API (v1), настраиваю middleware и базовые маршруты.

## Почему роутинг важен

Правильная настройка роутинга обеспечивает:
- Организацию endpoints приложения
- Версионирование API для обратной совместимости
- Применение middleware для аутентификации и валидации
- RESTful структуру API
- Легкость поддержки и расширения

## Пошаговая инструкция

1. Открываю файл routes/api.php:
   ```bash
   nano routes/api.php
   ```
   Или использую любой редактор.

2. Создаю структуру для версионирования API:
   В `routes/api.php` создаю базовую структуру:
   ```php
   <?php

   use Illuminate\Http\Request;
   use Illuminate\Support\Facades\Route;

   Route::get('/user', function (Request $request) {
       return $request->user();
   })->middleware('auth:sanctum');

   // API v1 routes
   Route::prefix('v1')->group(function () {
       // Public routes
       Route::get('/health', function () {
           return response()->json(['status' => 'ok']);
       });

       // Auth routes (будут добавлены позже)
       // Route::prefix('auth')->group(function () {
       //     Route::post('/register', [AuthController::class, 'register']);
       //     Route::post('/login', [AuthController::class, 'login']);
       // });

       // Protected routes (будут добавлены позже)
       // Route::middleware('auth:sanctum')->group(function () {
       //     Route::get('/user/profile', [ProfileController::class, 'show']);
       // });
   });
   ```

3. Создаю отдельные файлы для организации роутов:
   ```bash
   mkdir -p routes/api/v1
   ```

4. Создаю файл для публичных роутов:
   ```bash
   touch routes/api/v1/public.php
   ```

5. Создаю файл для роутов аутентификации:
   ```bash
   touch routes/api/v1/auth.php
   ```

6. Создаю файл для защищенных роутов:
   ```bash
   touch routes/api/v1/protected.php
   ```

7. Обновляю routes/api.php для подключения подфайлов:
   ```php
   <?php

   use Illuminate\Http\Request;
   use Illuminate\Support\Facades\Route;

   Route::prefix('v1')->group(function () {
       // Public routes
       require __DIR__.'/v1/public.php';

       // Auth routes
       require __DIR__.'/v1/auth.php';

       // Protected routes (require authentication)
       Route::middleware('auth:sanctum')->group(function () {
           require __DIR__.'/v1/protected.php';
       });
   });
   ```

8. Настраиваю базовые публичные роуты в routes/api/v1/public.php:
   ```php
   <?php

   use Illuminate\Support\Facades\Route;

   // Health check
   Route::get('/health', function () {
       return response()->json([
           'status' => 'ok',
           'timestamp' => now()->toIso8601String(),
       ]);
   });

   // Public information endpoints (будут добавлены позже)
   // Route::get('/languages', [ProgrammingLanguageController::class, 'index']);
   // Route::get('/frameworks', [FrameworkController::class, 'index']);
   ```

9. Настраиваю базовые роуты аутентификации в routes/api/v1/auth.php:
   ```php
   <?php

   use Illuminate\Support\Facades\Route;

   // Auth routes (будут реализованы позже)
   // Route::post('/register', [RegisterController::class, 'register']);
   // Route::post('/login', [LoginController::class, 'login']);
   // Route::post('/logout', [LogoutController::class, 'logout'])
   //     ->middleware('auth:sanctum');
   ```

10. Настраиваю базовые защищенные роуты в routes/api/v1/protected.php:
    ```php
    <?php

    use Illuminate\Support\Facades\Route;

    // Protected routes (будут реализованы позже)
    // Route::get('/user/profile', [ProfileController::class, 'show']);
    // Route::put('/user/profile', [ProfileController::class, 'update']);
    ```

11. Проверяю, что роуты загружаются:
    ```bash
    php artisan route:list
    ```
    Должны отобразиться все зарегистрированные роуты.

12. Тестирую health check endpoint:
    ```bash
    curl http://klassev.test/api/v1/health
    ```
    Или открываю в браузере: http://klassev.test/api/v1/health
    Должен вернуться JSON с статусом "ok".

13. Настраиваю CORS (если нужно):
    Проверяю config/cors.php и настраиваю разрешенные домены для API запросов.

14. Настраиваю rate limiting:
    В app/Http/Kernel.php или через middleware проверяю настройки throttle для API.

## Что проверить после выполнения

- [ ] Структура роутов создана
- [ ] API версионирование настроено (v1)
- [ ] Health check endpoint работает
- [ ] Роуты отображаются в `php artisan route:list`
- [ ] Middleware настроен для защищенных роутов
- [ ] Нет ошибок при загрузке роутов

## Структура роутов

Планируемая структура API:
- `/api/v1/health` - проверка работоспособности
- `/api/v1/auth/*` - роуты аутентификации
- `/api/v1/languages` - публичные данные о языках
- `/api/v1/frameworks` - публичные данные о фреймворках
- `/api/v1/user/*` - данные пользователя (защищено)
- `/api/v1/questions/*` - вопросы (частично защищено)
- `/api/v1/sessions/*` - сессии тренировок (защищено)

## Важные замечания

- Использую префикс v1 для возможности будущего версионирования
- Разделяю роуты на файлы для лучшей организации
- Применяю middleware для защиты роутов
- Использую RESTful соглашения для именования endpoints
- Документирую роуты (буду использовать Swagger позже)

## Дополнительные настройки

- Настраиваю fallback роут для 404 ошибок API
- Настраиваю формат ответов (JSON по умолчанию)
- Настраиваю валидацию запросов через Form Requests

