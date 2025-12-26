# Настройка Nginx

## Что нужно сделать

Я настраиваю Nginx как веб-сервер для проекта, создаю конфигурационный файл для домена klassev.test и настраиваю обработку PHP запросов.

## Почему Nginx

Nginx выбран потому что:
- Высокая производительность и низкое потребление ресурсов
- Отличная поддержка PHP через PHP-FPM
- Гибкая конфигурация
- Поддержка HTTP/2 и SSL
- Широко используется в production окружениях

## Пошаговая инструкция

1. Проверяю, установлен ли Nginx:
   ```bash
   nginx -v
   ```
   Если не установлен, устанавливаю через пакетный менеджер (brew на macOS, apt на Ubuntu).

2. Создаю конфигурационный файл для проекта:
   ```bash
   sudo nano /usr/local/etc/nginx/servers/klassev.test.conf
   ```
   Или в зависимости от системы:
   - macOS (Homebrew): `/usr/local/etc/nginx/servers/`
   - Ubuntu/Debian: `/etc/nginx/sites-available/`

3. Создаю конфигурацию для HTTP:
   ```nginx
   server {
       listen 80;
       server_name klassev.test;
       root /Users/klassev/Sites/klassev.test/public;
       index index.php index.html;

       charset utf-8;

       location / {
           try_files $uri $uri/ /index.php?$query_string;
       }

       location ~ \.php$ {
           fastcgi_pass 127.0.0.1:9000;
           fastcgi_index index.php;
           fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include fastcgi_params;
       }

       location ~ /\.(?!well-known).* {
           deny all;
       }
   }
   ```

4. Создаю симлинк (для Ubuntu/Debian):
   ```bash
   sudo ln -s /etc/nginx/sites-available/klassev.test /etc/nginx/sites-enabled/
   ```

5. Добавляю домен в /etc/hosts:
   ```bash
   sudo nano /etc/hosts
   ```
   Добавляю строку:
   ```
   127.0.0.1 klassev.test
   ```

6. Проверяю конфигурацию Nginx:
   ```bash
   sudo nginx -t
   ```
   Должно быть сообщение "syntax is ok" и "test is successful".

7. Перезагружаю Nginx:
   ```bash
   sudo nginx -s reload
   ```
   Или перезапускаю:
   ```bash
   sudo brew services restart nginx  # macOS
   sudo systemctl restart nginx      # Linux
   ```

8. Проверяю, что сайт доступен:
   ```bash
   curl http://klassev.test
   ```
   Должна вернуться страница Laravel или ошибка (если еще не настроен PHP-FPM).

## Что проверить после выполнения

- [x] Nginx установлен и запущен
- [x] Конфигурационный файл создан и синтаксически корректен
- [x] Домен добавлен в /etc/hosts
- [x] Сайт отвечает на запросы (хотя бы ошибка, но не "connection refused")
- [x] Логи Nginx не показывают критических ошибок

## Настройка HTTPS (будет в следующем пункте)

После настройки SSL сертификатов добавлю конфигурацию для HTTPS на порту 443.

## Возможные проблемы

- Если порт 80 занят, проверяю другие сервисы: `sudo lsof -i :80`
- Если Nginx не запускается, проверяю логи: `sudo tail -f /var/log/nginx/error.log`
- Если сайт не открывается, проверяю права доступа к папке проекта

