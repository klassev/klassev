# Настройка базы данных

## Что нужно сделать

Я настраиваю базу данных MySQL/MariaDB для проекта, создаю базу данных и пользователя, проверяю подключение из Laravel.

## Почему MySQL/MariaDB

Выбрано MySQL/MariaDB потому что:
- Надежная и проверенная СУБД
- Отличная поддержка в Laravel
- Достаточно функций для проекта
- Простота настройки и использования
- Хорошая производительность
- Широко используется в production

## Пошаговая инструкция

1. Проверяю, установлен ли MySQL/MariaDB:
   ```bash
   mysql --version
   ```
   Или:
   ```bash
   mariadb --version
   ```
   Если не установлен, устанавливаю через пакетный менеджер.

2. Запускаю MySQL/MariaDB сервис:
   ```bash
   brew services start mysql  # macOS
   sudo systemctl start mysql  # Linux
   ```

3. Подключаюсь к MySQL:
   ```bash
   mysql -u root -p
   ```
   Ввожу пароль root (если установлен) или просто нажимаю Enter.

4. Создаю базу данных для проекта:
   ```sql
   CREATE DATABASE klassev_test CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```

5. Создаю пользователя для приложения (опционально, для безопасности):
   ```sql
   CREATE USER 'klassev_user'@'localhost' IDENTIFIED BY 'strong_password';
   GRANT ALL PRIVILEGES ON klassev_test.* TO 'klassev_user'@'localhost';
   FLUSH PRIVILEGES;
   ```
   Заменяю 'strong_password' на надежный пароль.

6. Проверяю, что база данных создана:
   ```sql
   SHOW DATABASES;
   ```
   Должна быть в списке klassev_test.

7. Выхожу из MySQL:
   ```sql
   EXIT;
   ```

8. Обновляю .env файл с данными для подключения:
   ```bash
   nano .env
   ```
   Устанавливаю:
   ```
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=klassev_test
   DB_USERNAME=klassev_user
   DB_PASSWORD=strong_password
   ```
   Или использую root, если не создавал отдельного пользователя.

9. Проверяю подключение из Laravel:
   ```bash
   php artisan tinker
   ```
   В tinker выполняю:
   ```php
   DB::connection()->getPdo();
   ```
   Должно вернуться объект PDO без ошибок.

10. Альтернативная проверка через команду:
    ```bash
    php artisan db:show
    ```
    Должна отобразиться информация о подключении к базе данных.

## Что проверить после выполнения

- [ ] MySQL/MariaDB установлен и запущен
- [ ] База данных klassev_test создана
- [ ] Пользователь создан (если использовал отдельного пользователя)
- [ ] Настройки в .env корректны
- [ ] Laravel может подключиться к базе данных
- [ ] Нет ошибок при проверке подключения

## Дополнительные настройки

1. Настраиваю права доступа к файлам базы данных (если нужно):
   ```bash
   sudo chown -R mysql:mysql /var/lib/mysql
   ```

2. Настраиваю автозапуск MySQL:
   ```bash
   brew services start mysql  # macOS
   sudo systemctl enable mysql  # Linux
   ```

3. Проверяю производительность и настройки:
   ```sql
   SHOW VARIABLES LIKE 'max_connections';
   SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
   ```

## Возможные проблемы

- Если не могу подключиться, проверяю, запущен ли MySQL: `brew services list`
- Если ошибка доступа, проверяю права пользователя в MySQL
- Если база данных не создается, проверяю права root пользователя
- Если Laravel не подключается, проверяю правильность данных в .env

## Безопасность

- Использую отдельного пользователя для приложения, а не root
- Устанавливаю надежный пароль
- Ограничиваю права пользователя только на нужную базу данных
- Регулярно делаю резервные копии базы данных

