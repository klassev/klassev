# Настройка Redis

## Что нужно сделать

Я настраиваю Redis для кэширования данных и сессий в Laravel. Устанавливаю Redis, запускаю сервис и настраиваю подключение в Laravel.

## Почему Redis

Redis выбран потому что:
- Быстрое хранение данных в памяти
- Отличная поддержка в Laravel
- Поддержка различных структур данных
- Используется для кэширования, сессий, очередей
- Широко используется в production окружениях
- Простота настройки и использования

## Пошаговая инструкция

1. Проверяю, установлен ли Redis:
   ```bash
   redis-cli --version
   ```
   Если не установлен, устанавливаю:
   ```bash
   brew install redis  # macOS
   sudo apt install redis-server  # Ubuntu/Debian
   ```

2. Запускаю Redis сервис:
   ```bash
   brew services start redis  # macOS
   sudo systemctl start redis  # Linux
   ```

3. Проверяю, что Redis запущен:
   ```bash
   redis-cli ping
   ```
   Должен вернуться ответ "PONG".

4. Проверяю версию Redis:
   ```bash
   redis-cli info server | grep redis_version
   ```
   Должна быть версия 7.x или выше.

5. Настраиваю автозапуск Redis:
   ```bash
   brew services start redis  # macOS (уже делает автозапуск)
   sudo systemctl enable redis  # Linux
   ```

6. Проверяю конфигурацию Redis (опционально):
   ```bash
   cat /usr/local/etc/redis.conf  # macOS
   cat /etc/redis/redis.conf      # Linux
   ```
   Для разработки стандартные настройки обычно подходят.

7. Обновляю .env файл с настройками Redis:
   ```bash
   nano .env
   ```
   Проверяю/устанавливаю:
   ```
   CACHE_DRIVER=redis
   SESSION_DRIVER=redis
   REDIS_HOST=127.0.0.1
   REDIS_PASSWORD=null
   REDIS_PORT=6379
   ```

8. Устанавливаю пакет predis для работы с Redis в Laravel:
   ```bash
   composer require predis/predis
   ```
   Или использую встроенный phpredis, если установлен.

9. Очищаю конфигурацию Laravel:
   ```bash
   php artisan config:clear
   php artisan config:cache
   ```

10. Проверяю подключение к Redis из Laravel:
    ```bash
    php artisan tinker
    ```
    В tinker выполняю:
    ```php
    Cache::put('test', 'Redis works!', 60);
    Cache::get('test');
    ```
    Должно вернуться "Redis works!".

11. Проверяю работу Redis через redis-cli:
    ```bash
    redis-cli
    ```
    В Redis CLI:
    ```redis
    KEYS *
    GET laravel_cache:test
    EXIT
    ```

12. Тестирую кэширование в Laravel:
    ```bash
    php artisan cache:clear
    php artisan cache:put test_key "test_value" 60
    php artisan cache:get test_key
    ```

## Что проверить после выполнения

- [ ] Redis установлен и запущен
- [ ] Redis отвечает на команду PING
- [ ] Настройки в .env корректны
- [ ] Laravel может подключиться к Redis
- [ ] Кэширование работает (тест через tinker)
- [ ] Нет ошибок при работе с Redis

## Дополнительные настройки

1. Настраиваю пароль для Redis (для production):
   ```bash
   nano /usr/local/etc/redis.conf
   ```
   Добавляю:
   ```
   requirepass your_strong_password
   ```
   И обновляю REDIS_PASSWORD в .env.

2. Настраиваю персистентность данных (опционально):
   В redis.conf настраиваю RDB или AOF для сохранения данных на диск.

3. Настраиваю ограничения памяти:
   ```
   maxmemory 256mb
   maxmemory-policy allkeys-lru
   ```

## Использование Redis в Laravel

Redis будет использоваться для:
- Кэширования запросов к базе данных
- Хранения сессий пользователей
- Очередей задач (если настрою)
- Временного хранения данных

## Возможные проблемы

- Если Redis не запускается, проверяю логи: `tail -f /usr/local/var/log/redis.log`
- Если не могу подключиться, проверяю, что Redis слушает правильный порт: `lsof -i :6379`
- Если Laravel не подключается, проверяю правильность настроек в .env
- Если ошибка "Connection refused", проверяю, что Redis запущен: `brew services list`

## Мониторинг Redis

Проверяю статистику Redis:
```bash
redis-cli info stats
redis-cli info memory
```

