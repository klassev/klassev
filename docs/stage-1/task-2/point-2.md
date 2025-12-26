# Настройка PHP-FPM

## Что нужно сделать

Я настраиваю PHP-FPM (FastCGI Process Manager) для обработки PHP запросов от Nginx. Настраиваю PHP 8.4-FPM с оптимальными параметрами для разработки.

## Почему PHP-FPM

PHP-FPM необходим потому что:
- Nginx не может напрямую выполнять PHP код
- PHP-FPM обеспечивает эффективную обработку PHP запросов
- Поддерживает пул процессов для лучшей производительности
- Позволяет настраивать ограничения ресурсов
- Стандартное решение для связки Nginx + PHP

## Пошаговая инструкция

1. Проверяю, установлен ли PHP-FPM:
   ```bash
   php-fpm -v
   ```
   Или:
   ```bash
   php -v
   ```
   Если PHP 8.4 не установлен, устанавливаю его.

2. На macOS с Homebrew устанавливаю PHP 8.4:
   ```bash
   brew install php@8.4
   brew link php@8.4
   ```

3. Проверяю конфигурационный файл PHP-FPM:
   ```bash
   php-fpm -i | grep "Loaded Configuration File"
   ```
   Обычно это:
   - macOS: `/usr/local/etc/php/8.4/php-fpm.conf`
   - Linux: `/etc/php/8.4/fpm/php-fpm.conf`

4. Открываю основной конфигурационный файл:
   ```bash
   nano /usr/local/etc/php/8.4/php-fpm.conf
   ```

5. Проверяю и настраиваю пул процессов (обычно в отдельном файле):
   ```bash
   nano /usr/local/etc/php/8.4/php-fpm.d/www.conf
   ```
   Или на Linux:
   ```bash
   sudo nano /etc/php/8.4/fpm/pool.d/www.conf
   ```

6. Настраиваю параметры пула для разработки:
   ```ini
   [www]
   user = klassev
   group = staff
   listen = 127.0.0.1:9000
   listen.owner = klassev
   listen.group = staff
   pm = dynamic
   pm.max_children = 10
   pm.start_servers = 3
   pm.min_spare_servers = 2
   pm.max_spare_servers = 5
   pm.max_requests = 500
   ```

7. Настраиваю PHP.ini для разработки:
   ```bash
   nano /usr/local/etc/php/8.4/php.ini
   ```
   Устанавливаю:
   ```ini
   memory_limit = 256M
   upload_max_filesize = 20M
   post_max_size = 20M
   max_execution_time = 300
   date.timezone = Europe/Moscow
   ```

8. Проверяю, что PHP-FPM может запуститься:
   ```bash
   php-fpm -t
   ```
   Должно быть "test is successful".

9. Запускаю PHP-FPM:
   ```bash
   brew services start php@8.4  # macOS
   sudo systemctl start php8.4-fpm  # Linux
   ```

10. Проверяю, что PHP-FPM слушает порт 9000:
    ```bash
    lsof -i :9000
    ```
    Должен быть процесс php-fpm.

11. Проверяю работу связки Nginx + PHP-FPM:
    Создаю тестовый файл в public:
    ```bash
    echo "<?php phpinfo(); ?>" > public/test.php
    ```
    Открываю в браузере: http://klassev.test/test.php
    Должна отобразиться информация о PHP.

12. Удаляю тестовый файл:
    ```bash
    rm public/test.php
    ```

## Что проверить после выполнения

- [ ] PHP 8.4 установлен и работает
- [ ] PHP-FPM запущен и слушает порт 9000
- [ ] Конфигурация PHP-FPM корректна
- [ ] Nginx может обрабатывать PHP файлы через PHP-FPM
- [ ] Laravel приложение открывается в браузере

## Настройка автозапуска

Чтобы PHP-FPM запускался автоматически при загрузке системы:
```bash
brew services start php@8.4  # macOS
sudo systemctl enable php8.4-fpm  # Linux
```

## Возможные проблемы

- Если порт 9000 занят, меняю порт в конфигурации PHP-FPM и Nginx
- Если PHP-FPM не запускается, проверяю логи: `tail -f /usr/local/var/log/php-fpm.log`
- Если права доступа неверны, проверяю user и group в конфигурации пула

