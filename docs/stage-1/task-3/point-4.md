# Базовая структура API

## Что нужно сделать

Я создаю базовую структуру API: контроллеры, Form Requests для валидации, базовые ответы API, обработку ошибок и единый формат ответов.

## Почему это важно

Базовая структура API необходима потому что:
- Единообразие ответов API
- Централизованная обработка ошибок
- Валидация данных на уровне запросов
- Легкость расширения и поддержки
- Соответствие RESTful принципам
- Готовность к дальнейшей разработке

## Пошаговая инструкция

1. Создаю базовый API контроллер:
   ```bash
   php artisan make:controller Api/V1/BaseController
   ```

2. Настраиваю BaseController с общими методами:
   ```php
   <?php

   namespace App\Http\Controllers\Api\V1;

   use App\Http\Controllers\Controller;
   use Illuminate\Http\JsonResponse;

   class BaseController extends Controller
   {
       protected function success($data = null, string $message = 'Success', int $code = 200): JsonResponse
       {
           return response()->json([
               'success' => true,
               'message' => $message,
               'data' => $data,
           ], $code);
       }

       protected function error(string $message = 'Error', int $code = 400, $errors = null): JsonResponse
       {
           $response = [
               'success' => false,
               'message' => $message,
           ];

           if ($errors) {
               $response['errors'] = $errors;
           }

           return response()->json($response, $code);
       }
   }
   ```

3. Создаю базовый Form Request:
   ```bash
   php artisan make:request Api/V1/BaseFormRequest
   ```

4. Настраиваю BaseFormRequest:
   ```php
   <?php

   namespace App\Http\Requests\Api\V1;

   use Illuminate\Foundation\Http\FormRequest;
   use Illuminate\Contracts\Validation\Validator;
   use Illuminate\Http\Exceptions\HttpResponseException;

   class BaseFormRequest extends FormRequest
   {
       protected function failedValidation(Validator $validator)
       {
           throw new HttpResponseException(
               response()->json([
                   'success' => false,
                   'message' => 'Validation error',
                   'errors' => $validator->errors(),
               ], 422)
           );
       }
   }
   ```

5. Создаю контроллер для Health Check:
   ```bash
   php artisan make:controller Api/V1/HealthController
   ```

6. Настраиваю HealthController:
   ```php
   <?php

   namespace App\Http\Controllers\Api\V1;

   class HealthController extends BaseController
   {
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

7. Обновляю роут для health check:
   В routes/api/v1/public.php:
   ```php
   Route::get('/health', [HealthController::class, 'check']);
   ```

8. Создаю Trait для обработки исключений API:
   ```bash
   mkdir -p app/Traits
   touch app/Traits/ApiResponse.php
   ```

9. Настраиваю ApiResponse trait (опционально, если хочу использовать в других контроллерах):
   ```php
   <?php

   namespace App\Traits;

   use Illuminate\Http\JsonResponse;

   trait ApiResponse
   {
       protected function success($data = null, string $message = 'Success', int $code = 200): JsonResponse
       {
           return response()->json([
               'success' => true,
               'message' => $message,
               'data' => $data,
           ], $code);
       }

       protected function error(string $message = 'Error', int $code = 400, $errors = null): JsonResponse
       {
           $response = [
               'success' => false,
               'message' => $message,
           ];

           if ($errors) {
               $response['errors'] = $errors;
           }

           return response()->json($response, $code);
       }
   }
   ```

10. Настраиваю обработку исключений в app/Exceptions/Handler.php:
    ```php
    public function render($request, Throwable $exception)
    {
        if ($request->is('api/*')) {
            return $this->handleApiException($request, $exception);
        }

        return parent::render($request, $exception);
    }

    protected function handleApiException($request, Throwable $exception)
    {
        if ($exception instanceof ValidationException) {
            return response()->json([
                'success' => false,
                'message' => 'Validation error',
                'errors' => $exception->errors(),
            ], 422);
        }

        if ($exception instanceof ModelNotFoundException) {
            return response()->json([
                'success' => false,
                'message' => 'Resource not found',
            ], 404);
        }

        return response()->json([
            'success' => false,
            'message' => $exception->getMessage() ?: 'Server error',
        ], 500);
    }
    ```

11. Создаю структуру папок для будущих контроллеров:
    ```bash
    mkdir -p app/Http/Controllers/Api/V1
    mkdir -p app/Http/Requests/Api/V1
    ```

12. Тестирую health check endpoint:
    ```bash
    curl http://klassev.test/api/v1/health
    ```
    Должен вернуться JSON:
    ```json
    {
        "success": true,
        "message": "Service is healthy",
        "data": {
            "status": "ok",
            "timestamp": "2025-01-XX...",
            "version": "1.0.0"
        }
    }
    ```

13. Создаю базовые контроллеры-заглушки для будущей разработки:
    ```bash
    php artisan make:controller Api/V1/Auth/RegisterController
    php artisan make:controller Api/V1/Auth/LoginController
    php artisan make:controller Api/V1/ProfileController
    ```

14. Настраиваю структуру ответов для всех API endpoints:
    Все ответы должны иметь формат:
    ```json
    {
        "success": true/false,
        "message": "Описание",
        "data": {...} // для успешных запросов
        "errors": {...} // для ошибок валидации
    }
    ```

## Что проверить после выполнения

- [ ] BaseController создан и работает
- [ ] BaseFormRequest создан для валидации
- [ ] Health check endpoint возвращает правильный формат
- [ ] Обработка исключений работает для API
- [ ] Структура папок создана
- [ ] Единый формат ответов применяется

## Формат ответов API

Успешный ответ:
```json
{
    "success": true,
    "message": "Success",
    "data": {...}
}
```

Ошибка валидации:
```json
{
    "success": false,
    "message": "Validation error",
    "errors": {
        "field": ["Error message"]
    }
}
```

Ошибка сервера:
```json
{
    "success": false,
    "message": "Error message"
}
```

## Важные замечания

- Использую единый формат ответов для всех endpoints
- Обрабатываю все исключения в едином месте
- Использую Form Requests для валидации
- Возвращаю правильные HTTP коды статуса
- Документирую формат ответов (буду использовать Swagger)

## Дополнительные настройки

- Настраиваю пагинацию для списков
- Настраиваю фильтрацию и сортировку
- Настраиваю rate limiting для API
- Настраиваю версионирование через заголовки

