# SSL сертификаты

## Что нужно сделать

Я настраиваю SSL сертификаты для локальной разработки используя mkcert, чтобы иметь доверенные HTTPS соединения на klassev.test.

## Почему SSL сертификаты

SSL сертификаты нужны потому что:
- Современные браузеры требуют HTTPS для многих функций
- OAuth провайдеры часто требуют HTTPS для callback URL
- Безопасность передачи данных
- Соответствие production окружению
- Избежание проблем с mixed content (HTTP/HTTPS)

## Почему mkcert

mkcert выбран потому что:
- Простота использования
- Автоматическое создание локального CA (Certificate Authority)
- Доверенные сертификаты в системе
- Не требует настройки сложных инструментов
- Работает на macOS, Linux, Windows

## Пошаговая инструкция

1. Устанавливаю mkcert:
   ```bash
   brew install mkcert
   ```
   Или на Linux:
   ```bash
   sudo apt install mkcert  # Ubuntu/Debian
   ```

2. Устанавливаю локальный CA (Certificate Authority):
   ```bash
   mkcert -install
   ```
   Это создаст локальный CA и добавит его в доверенные корневые сертификаты системы.

3. Создаю SSL сертификат для домена:
   ```bash
   cd /Users/klassev/Sites/klassev.test
   mkdir -p ssl
   mkcert -key-file ssl/klassev.test-key.pem -cert-file ssl/klassev.test.pem klassev.test "*.klassev.test" localhost 127.0.0.1
   ```
   Это создаст сертификат для klassev.test и всех поддоменов.

4. Проверяю, что файлы созданы:
   ```bash
   ls -la ssl/
   ```
   Должны быть:
   - klassev.test.pem (сертификат)
   - klassev.test-key.pem (приватный ключ)

5. Обновляю конфигурацию Nginx для HTTPS:
   ```bash
   sudo nano /usr/local/etc/nginx/servers/klassev.test.conf
   ```
   Добавляю блок для HTTPS:
   ```nginx
   server {
       listen 443 ssl http2;
       server_name klassev.test;
       root /Users/klassev/Sites/klassev.test/public;
       index index.php index.html;

       ssl_certificate /Users/klassev/Sites/klassev.test/ssl/klassev.test.pem;
       ssl_certificate_key /Users/klassev/Sites/klassev.test/ssl/klassev.test-key.pem;

       ssl_protocols TLSv1.2 TLSv1.3;
       ssl_ciphers HIGH:!aNULL:!MD5;
       ssl_prefer_server_ciphers on;

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

   # Редирект с HTTP на HTTPS
   server {
       listen 80;
       server_name klassev.test;
       return 301 https://$server_name$request_uri;
   }
   ```

6. Проверяю конфигурацию Nginx:
   ```bash
   sudo nginx -t
   ```

7. Перезагружаю Nginx:
   ```bash
   sudo nginx -s reload
   ```

8. Обновляю .env файл:
   ```bash
   nano .env
   ```
   Меняю APP_URL:
   ```
   APP_URL=https://klassev.test
   ```

9. Проверяю доступность HTTPS:
   ```bash
   curl -k https://klassev.test
   ```
   Или открываю в браузере: https://klassev.test

10. Проверяю, что сертификат доверенный:
    Открываю https://klassev.test в браузере и проверяю, что нет предупреждения о недоверенном сертификате.

11. Добавляю ssl/ в .gitignore:
    ```bash
    echo "ssl/" >> .gitignore
    ```
    Сертификаты не должны попадать в репозиторий.

## Что проверить после выполнения

- [x] mkcert установлен
- [x] Локальный CA установлен в систему
- [x] SSL сертификаты созданы
- [x] Nginx настроен для HTTPS
- [x] Сайт доступен по https://klassev.test
- [x] Сертификат доверенный (нет предупреждений в браузере)
- [x] Редирект с HTTP на HTTPS работает
- [ ] ssl/ добавлен в .gitignore

## Дополнительные настройки

1. Настраиваю безопасные заголовки в Nginx:
   ```nginx
   add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
   add_header X-Frame-Options "SAMEORIGIN" always;
   add_header X-Content-Type-Options "nosniff" always;
   add_header X-XSS-Protection "1; mode=block" always;
   ```

2. Настраиваю OCSP Stapling (опционально):
   ```nginx
   ssl_stapling on;
   ssl_stapling_verify on;
   ```

## Возможные проблемы

- Если mkcert не устанавливается, проверяю наличие Homebrew или использую альтернативный способ установки
- Если сертификат не доверенный, проверяю, что локальный CA установлен: `mkcert -CAROOT`
- Если Nginx не запускается, проверяю синтаксис конфигурации: `sudo nginx -t`
- Если сайт не открывается по HTTPS, проверяю, что порт 443 не занят: `lsof -i :443`

## Безопасность

- Храню приватные ключи в безопасности
- Не коммичу сертификаты в репозиторий
- Для production использую Let's Encrypt или коммерческие сертификаты
- Регулярно обновляю сертификаты (хотя для локальной разработки это не критично)

## Альтернативные способы

Если mkcert не подходит, могу использовать:
- OpenSSL для создания самоподписанных сертификатов
- Let's Encrypt для локальной разработки (сложнее)
- Коммерческие сертификаты (для production)

