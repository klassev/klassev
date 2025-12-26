# Подробный план реализации универсального тренажера для программирования

## 📋 Содержание
1. [Обзор проекта](#обзор-проекта)
2. [Технологический стек](#технологический-стек)
3. [Архитектура системы](#архитектура-системы)
4. [Структура базы данных](#структура-базы-данных)
5. [Детальный план этапов разработки](#детальный-план-этапов-разработки)
6. [Модули системы](#модули-системы)
7. [Безопасность](#безопасность)
8. [План тестирования](#план-тестирования)
9. [Развертывание](#развертывание)

---

## Обзор проекта

### Цель проекта
Создание универсального веб-приложения для подготовки к собеседованиям по программированию с поддержкой множества языков, фреймворков и технологий. Система включает различные типы вопросов: теоретические, анализ кода и практические задания.

### Поддерживаемые технологии

#### Языки программирования:
- **PHP** (8.0+)
- **JavaScript/TypeScript**
- **Python** (3.x)
- **Java**
- **Go**
- **Ruby**
- **Rust**
- **C#**
- **C++**
- **Swift**
- **Kotlin**

#### Фреймворки и библиотеки:
- **Backend**: Laravel, Symfony, Django, Flask, Express.js, Spring, Rails, ASP.NET
- **Frontend**: React, Vue.js, Angular, Svelte, Next.js, Nuxt.js
- **Mobile**: React Native, Flutter, SwiftUI, Android SDK
- **Database**: MySQL, PostgreSQL, MongoDB, Redis, Elasticsearch

#### Технологии и инструменты:
- **DevOps**: Docker, Kubernetes, CI/CD, AWS, Azure, GCP
- **Testing**: PHPUnit, Jest, Pytest, JUnit, Mocha
- **Tools**: Git, Linux, Bash, Nginx, Apache

### Целевая аудитория
- **Junior**: начинающие разработчики
- **Middle**: разработчики с опытом 2-5 лет
- **Senior**: опытные разработчики 5+ лет

### Ключевые функции
- **Аутентификация и регистрация**
  - Регистрация через email/пароль
  - Вход через соцсети (Google, GitHub, Yandex, VK)
  - Восстановление пароля
  - Email верификация
- **Тарифные планы**
  - Free (бесплатный) - базовые возможности
  - Basic (базовый) - расширенные функции
  - Premium (премиум) - полный доступ
- **Вопросы и задания**
  - Поддержка множества языков программирования (PHP, JavaScript, Python, Java, Go, Ruby, Rust, C#, C++, Swift, Kotlin)
  - Поддержка фреймворков и технологий (Laravel, React, Vue, Django, Spring, и др.)
  - Теоретические вопросы с вариантами ответов
  - Анализ кода (что выведет код, поиск ошибок)
  - Практические задания с автоматической проверкой
  - Мультиязычная Docker песочница для выполнения кода
- **Личный кабинет**
  - Профиль пользователя
  - Статистика и прогресс
  - История сессий
  - Управление подпиской
- **Дополнительные функции**
  - Система прогресса и статистики (по языкам и технологиям)
  - Режим симуляции собеседования
  - Рекомендации по улучшению (персонализированные по технологиям)
  - Выбор основных технологий в профиле
  - Фильтрация вопросов по языкам и фреймворкам
  - Админ-панель для управления контентом

---

## Технологический стек

### Backend
- **PHP 8.4** (актуальная версия, выпущена ноябрь 2024, поддержка до 2028)
- **Framework**: Laravel 12 (актуальная версия, выпущена февраль 2025, выбрано для быстрой разработки и релевантности для PHP собеседований)
- **Admin Panel**: Filament 4 (современная админ-панель для управления вопросами, категориями и пользователями)
- **Аутентификация**: Laravel Sanctum (для API аутентификации)
- **OAuth**: Laravel Socialite (для входа через соцсети: Google, GitHub, Yandex, VK)
- **Платежи**: YooKassa / Stripe (интеграция платежных систем)
- **База данных**: MySQL 8.0+ (выбрано для простоты и достаточности функций)
- **Кэширование**: Redis 7.x- **Очереди**: Laravel Queues (встроенные очереди Laravel)

### Frontend
- **JavaScript Framework**: Vue.js 3 (Composition API) (выбрано для простоты и скорости разработки)
- **Build Tool**: Vite (быстрая сборка)
- **State Management**: Pinia (проще Redux)
- **UI Framework**: Tailwind CSS (современный и гибкий)
- **Графики**: Chart.js (простой и достаточный)
- **Code Editor**: Monaco Editor (VS Code editor - лучший выбор)
- **Синтаксис**: Prism.js (для подсветки в вопросах)

### Инфраструктура
- **Веб-сервер**: Nginx
- **PHP-FPM**: для обработки PHP запросов
- **Песочница**: Docker контейнеры для выполнения пользовательского кода

### Инструменты разработки
- **Версионирование**: Git
- **CI/CD**: GitHub Actions / GitLab CI
- **Тестирование**: PHPUnit, Jest (для JS)
- **Документация**: Swagger/OpenAPI

---

## Архитектура системы

### Общая схема
```
┌─────────────────┐
│   Frontend      │  Vue.js SPA
│   (Browser)     │
└────────┬────────┘
         │ HTTP/HTTPS
         │ REST API / GraphQL
┌────────▼────────┐
│   Backend API   │  Laravel/Symfony
│   (PHP)         │
└────────┬────────┘
         │
    ┌────┴────┬──────────────┬─────────────┐
    │         │              │             │
┌───▼───┐ ┌──▼───┐    ┌──────▼──────┐ ┌───▼────┐
│ MySQL │ │Redis │    │   Docker    │ │ Queue  │
│       │ │Cache │    │  Sandbox    │ │ Worker │
└───────┘ └──────┘    └─────────────┘ └────────┘
```

### Модульная структура
1. **API Layer** - REST API endpoints
2. **Business Logic Layer** - сервисы и бизнес-логика
3. **Data Access Layer** - модели и репозитории
4. **Security Layer** - валидация и санитизация
5. **Execution Layer** - Docker sandbox для кода

---

## Структура базы данных

### Порядок создания таблиц

⚠️ **Важно**: Таблицы должны создаваться в следующем порядке из-за внешних ключей:

1. `programming_languages` - языки программирования
2. `frameworks` - фреймворки и технологии
3. `plans` - тарифные планы
4. `users` - пользователи (БЕЗ subscription_id)
5. `subscriptions` - подписки (создается после users)
6. `question_categories` - категории вопросов
7. `questions` - вопросы
8. `question_options` - варианты ответов
9. `test_cases` - тест-кейсы
10. Остальные таблицы в любом порядке

### Основные таблицы

#### 1. Языки программирования
```sql
CREATE TABLE programming_languages (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL UNIQUE,
    slug VARCHAR(50) NOT NULL UNIQUE,
    description TEXT,
    icon VARCHAR(255),
    color VARCHAR(7), -- hex цвет для UI
    syntax_highlighting VARCHAR(50), -- для подсветки синтаксиса
    docker_image VARCHAR(255), -- Docker образ для песочницы
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_active (is_active),
    INDEX idx_slug (slug)
);
```

#### 2. Фреймворки и технологии
```sql
CREATE TABLE frameworks (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    type ENUM('backend', 'frontend', 'mobile', 'database', 'devops', 'tool', 'library') NOT NULL,
    language_id INT NULL, -- привязка к языку (NULL для универсальных)
    icon VARCHAR(255),
    color VARCHAR(7),
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (language_id) REFERENCES programming_languages(id) ON DELETE SET NULL,
    INDEX idx_type (type),
    INDEX idx_language (language_id),
    INDEX idx_active (is_active),
    INDEX idx_slug (slug)
);
```

#### 3. Категории вопросов (темы)
```sql
CREATE TABLE question_categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL,
    description TEXT,
    parent_id INT NULL, -- для иерархии категорий
    language_id INT NULL, -- привязка к языку (NULL для универсальных)
    framework_id INT NULL, -- привязка к фреймворку (NULL для универсальных)
    icon VARCHAR(50),
    sort_order INT DEFAULT 0,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES question_categories(id) ON DELETE CASCADE,
    FOREIGN KEY (language_id) REFERENCES programming_languages(id) ON DELETE CASCADE,
    FOREIGN KEY (framework_id) REFERENCES frameworks(id) ON DELETE CASCADE,
    INDEX idx_parent (parent_id),
    INDEX idx_language (language_id),
    INDEX idx_framework (framework_id),
    INDEX idx_active (is_active),
    UNIQUE KEY unique_slug_language_framework (slug, language_id, framework_id)
);
```

#### 4. Вопросы
```sql
CREATE TABLE questions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    category_id INT NOT NULL,
    language_id INT NULL, -- основной язык вопроса
    framework_id INT NULL, -- фреймворк (если применимо)
    question_text TEXT NOT NULL,
    question_type ENUM('theory', 'code_analysis', 'code_practice') NOT NULL,
    difficulty ENUM('junior', 'middle', 'senior') NOT NULL,
    code_example TEXT,
    code_language VARCHAR(50), -- язык кода для подсветки синтаксиса
    code_template TEXT, -- шаблон для практических заданий
    correct_answer TEXT NOT NULL,
    explanation TEXT NOT NULL,
    points INT DEFAULT 1,
    time_limit INT, -- секунды на ответ
    tags JSON, -- дополнительные теги
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES question_categories(id) ON DELETE CASCADE,
    FOREIGN KEY (language_id) REFERENCES programming_languages(id) ON DELETE CASCADE,
    FOREIGN KEY (framework_id) REFERENCES frameworks(id) ON DELETE CASCADE,
    INDEX idx_category (category_id),
    INDEX idx_language (language_id),
    INDEX idx_framework (framework_id),
    INDEX idx_type (question_type),
    INDEX idx_difficulty (difficulty),
    FULLTEXT idx_search (question_text, explanation)
);
```

#### 5. Варианты ответов (для теоретических вопросов)
```sql
CREATE TABLE question_options (
    id INT PRIMARY KEY AUTO_INCREMENT,
    question_id INT NOT NULL,
    option_text TEXT NOT NULL,
    is_correct BOOLEAN DEFAULT FALSE,
    sort_order INT DEFAULT 0,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    INDEX idx_question (question_id)
);
```

#### 6. Тест-кейсы (для практических заданий)
```sql
CREATE TABLE test_cases (
    id INT PRIMARY KEY AUTO_INCREMENT,
    question_id INT NOT NULL,
    input_data TEXT NOT NULL, -- JSON
    expected_output TEXT NOT NULL,
    is_hidden BOOLEAN DEFAULT FALSE, -- скрытый тест
    sort_order INT DEFAULT 0,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    INDEX idx_question (question_id)
);
```

#### 5. Пользователи
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL UNIQUE,
    name VARCHAR(100),
    password_hash VARCHAR(255) NULL,
    avatar VARCHAR(255) NULL,
    role ENUM('user', 'admin') DEFAULT 'user',
    email_verified_at TIMESTAMP NULL,
    subscription_expires_at TIMESTAMP NULL,
    trial_ends_at TIMESTAMP NULL,
    is_active BOOLEAN DEFAULT TRUE,
    last_login_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_email (email),
    INDEX idx_active (is_active)
);
-- Примечание: subscription_id убран из users, активная подписка определяется через таблицу subscriptions
```

#### 5.1. Социальные провайдеры (OAuth)
```sql
CREATE TABLE social_providers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    provider VARCHAR(50) NOT NULL, -- 'google', 'github', 'yandex', 'vk'
    provider_id VARCHAR(255) NOT NULL,
    provider_token TEXT,
    provider_refresh_token TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_provider_user (provider, provider_id),
    INDEX idx_user (user_id),
    INDEX idx_provider (provider)
);
```

#### 6. Тарифные планы
```sql
CREATE TABLE plans (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    price DECIMAL(10,2) NOT NULL DEFAULT 0.00,
    currency VARCHAR(3) DEFAULT 'RUB',
    interval ENUM('month', 'year') DEFAULT 'month',
    features JSON, -- список возможностей тарифа
    max_questions_per_day INT NULL, -- NULL = безлимит
    max_sessions_per_month INT NULL,
    access_to_premium_questions BOOLEAN DEFAULT FALSE,
    access_to_code_practice BOOLEAN DEFAULT FALSE,
    access_to_statistics BOOLEAN DEFAULT TRUE,
    access_to_recommendations BOOLEAN DEFAULT TRUE,
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_active (is_active),
    INDEX idx_slug (slug)
);
```

#### 6.1. Подписки пользователей
```sql
CREATE TABLE subscriptions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    plan_id INT NOT NULL,
    status ENUM('active', 'cancelled', 'expired', 'trial') DEFAULT 'trial',
    starts_at TIMESTAMP NOT NULL,
    expires_at TIMESTAMP NULL,
    cancelled_at TIMESTAMP NULL,
    payment_provider VARCHAR(50) NULL, -- 'yookassa', 'stripe', 'paypal'
    payment_id VARCHAR(255) NULL,
    auto_renew BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (plan_id) REFERENCES plans(id) ON DELETE RESTRICT,
    INDEX idx_user (user_id),
    INDEX idx_plan (plan_id),
    INDEX idx_status (status),
    INDEX idx_expires (expires_at),
    INDEX idx_user_status (user_id, status) -- для быстрого поиска активной подписки
);
-- Примечание: Активная подписка пользователя определяется через запрос:
-- SELECT * FROM subscriptions WHERE user_id = ? AND status = 'active' 
-- AND (expires_at IS NULL OR expires_at > NOW()) ORDER BY created_at DESC LIMIT 1
```

#### 6.2. История платежей
```sql
CREATE TABLE payments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    subscription_id INT NULL,
    plan_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'RUB',
    status ENUM('pending', 'completed', 'failed', 'refunded') DEFAULT 'pending',
    payment_provider VARCHAR(50) NOT NULL,
    payment_id VARCHAR(255) NOT NULL,
    payment_data JSON NULL, -- дополнительные данные от провайдера
    paid_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (subscription_id) REFERENCES subscriptions(id) ON DELETE SET NULL,
    FOREIGN KEY (plan_id) REFERENCES plans(id) ON DELETE RESTRICT,
    INDEX idx_user (user_id),
    INDEX idx_subscription (subscription_id),
    INDEX idx_status (status),
    INDEX idx_payment_id (payment_id)
);
```

#### 7. Сессии пользователей
```sql
CREATE TABLE user_sessions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NULL, -- NULL для анонимных пользователей
    session_token VARCHAR(64) NOT NULL UNIQUE,
    started_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ended_at TIMESTAMP NULL,
    total_questions INT DEFAULT 0,
    correct_answers INT DEFAULT 0,
    INDEX idx_token (session_token),
    INDEX idx_user (user_id),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL
);
```

#### 8. Ответы пользователей
```sql
CREATE TABLE user_answers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    session_id INT NOT NULL,
    question_id INT NOT NULL,
    user_answer TEXT,
    is_correct BOOLEAN,
    time_spent INT, -- секунды
    code_execution_result TEXT, -- JSON для практических заданий
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (session_id) REFERENCES user_sessions(id) ON DELETE CASCADE,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    INDEX idx_session (session_id),
    INDEX idx_question (question_id),
    INDEX idx_correct (is_correct)
);
```

#### 9. Прогресс пользователя
```sql
CREATE TABLE user_progress (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NULL,
    category_id INT NOT NULL,
    language_id INT NULL,
    framework_id INT NULL,
    difficulty ENUM('junior', 'middle', 'senior') NOT NULL,
    total_attempts INT DEFAULT 0,
    correct_answers INT DEFAULT 0,
    total_time_spent INT DEFAULT 0, -- секунды
    last_practice_date DATETIME,
    best_score DECIMAL(5,2), -- процент правильных ответов
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (category_id) REFERENCES question_categories(id) ON DELETE CASCADE,
    FOREIGN KEY (language_id) REFERENCES programming_languages(id) ON DELETE CASCADE,
    FOREIGN KEY (framework_id) REFERENCES frameworks(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_category_language_framework_difficulty (user_id, category_id, language_id, framework_id, difficulty),
    INDEX idx_user (user_id),
    INDEX idx_category (category_id),
    INDEX idx_language (language_id),
    INDEX idx_framework (framework_id)
);
```

#### 9.1. Предпочтения пользователя по технологиям
```sql
CREATE TABLE user_technology_preferences (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    language_id INT NULL,
    framework_id INT NULL,
    is_primary BOOLEAN DEFAULT FALSE, -- основная технология
    skill_level ENUM('beginner', 'intermediate', 'advanced', 'expert') DEFAULT 'beginner',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (language_id) REFERENCES programming_languages(id) ON DELETE CASCADE,
    FOREIGN KEY (framework_id) REFERENCES frameworks(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_language_framework (user_id, language_id, framework_id),
    INDEX idx_user (user_id),
    INDEX idx_language (language_id),
    INDEX idx_framework (framework_id)
);
```

#### 10. Рекомендации
```sql
CREATE TABLE recommendations (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NULL,
    category_id INT NOT NULL,
    recommendation_text TEXT NOT NULL,
    priority INT DEFAULT 0,
    is_read BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (category_id) REFERENCES question_categories(id) ON DELETE CASCADE,
    INDEX idx_user (user_id),
    INDEX idx_priority (priority)
);
```

#### 11. Достижения (Геймификация)
```sql
CREATE TABLE achievements (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) UNIQUE,
    description TEXT,
    icon VARCHAR(255),
    points INT DEFAULT 0,
    category ENUM('questions', 'streak', 'speed', 'accuracy', 'time', 'level') NOT NULL,
    condition_type ENUM('count', 'streak', 'percentage', 'time', 'level') NOT NULL,
    condition_value INT NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_category (category),
    INDEX idx_active (is_active)
);

CREATE TABLE user_achievements (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    achievement_id INT NOT NULL,
    unlocked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (achievement_id) REFERENCES achievements(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_achievement (user_id, achievement_id),
    INDEX idx_user (user_id)
);

CREATE TABLE user_levels (
    user_id INT PRIMARY KEY,
    level INT DEFAULT 1,
    experience_points INT DEFAULT 0,
    total_points INT DEFAULT 0,
    current_streak INT DEFAULT 0, -- серия дней подряд
    longest_streak INT DEFAULT 0,
    last_activity_date DATE,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### 12. Рейтинги и комментарии (Социальные функции)
```sql
CREATE TABLE question_comments (
    id INT PRIMARY KEY AUTO_INCREMENT,
    question_id INT NOT NULL,
    user_id INT NOT NULL,
    comment_text TEXT NOT NULL,
    parent_id INT NULL,
    likes_count INT DEFAULT 0,
    is_approved BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (parent_id) REFERENCES question_comments(id) ON DELETE CASCADE,
    INDEX idx_question (question_id),
    INDEX idx_user (user_id),
    INDEX idx_approved (is_approved)
);

CREATE TABLE comment_likes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    comment_id INT NOT NULL,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (comment_id) REFERENCES question_comments(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_comment_like (comment_id, user_id)
);

CREATE TABLE question_ratings (
    id INT PRIMARY KEY AUTO_INCREMENT,
    question_id INT NOT NULL,
    user_id INT NOT NULL,
    rating INT NOT NULL CHECK (rating BETWEEN 1 AND 5),
    difficulty_rating INT CHECK (difficulty_rating BETWEEN 1 AND 5),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_question_rating (user_id, question_id),
    INDEX idx_question (question_id)
);
```

#### 13. Заметки и закладки (Персонализация)
```sql
CREATE TABLE user_notes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    question_id INT NOT NULL,
    note_text TEXT NOT NULL,
    tags JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    INDEX idx_user (user_id),
    INDEX idx_question (question_id),
    FULLTEXT idx_search (note_text)
);

CREATE TABLE user_bookmarks (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    question_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    UNIQUE KEY unique_user_bookmark (user_id, question_id),
    INDEX idx_user (user_id)
);
```

#### 14. Уведомления
```sql
CREATE TABLE notifications (
    id CHAR(36) PRIMARY KEY,
    type VARCHAR(255) NOT NULL,
    notifiable_type VARCHAR(255) NOT NULL,
    notifiable_id BIGINT UNSIGNED NOT NULL,
    data JSON,
    read_at TIMESTAMP NULL,
    created_at TIMESTAMP NULL,
    updated_at TIMESTAMP NULL,
    INDEX idx_notifiable (notifiable_type, notifiable_id)
);

CREATE TABLE user_notification_preferences (
    user_id INT PRIMARY KEY,
    email_enabled BOOLEAN DEFAULT TRUE,
    push_enabled BOOLEAN DEFAULT TRUE,
    daily_reminders BOOLEAN DEFAULT TRUE,
    weekly_reports BOOLEAN DEFAULT TRUE,
    achievement_notifications BOOLEAN DEFAULT TRUE,
    comment_notifications BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

#### 15. Сертификаты
```sql
CREATE TABLE certificates (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    type ENUM('category', 'level', 'interview', 'achievement') NOT NULL,
    certificate_id VARCHAR(64) UNIQUE NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    issued_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    qr_code VARCHAR(500),
    pdf_path VARCHAR(500),
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_user (user_id),
    INDEX idx_certificate_id (certificate_id)
);
```

#### 16. Команды и соревнования
```sql
CREATE TABLE teams (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) UNIQUE,
    description TEXT,
    leader_id INT NOT NULL,
    avatar VARCHAR(255),
    total_points INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (leader_id) REFERENCES users(id) ON DELETE CASCADE,
    INDEX idx_leader (leader_id)
);

CREATE TABLE team_members (
    id INT PRIMARY KEY AUTO_INCREMENT,
    team_id INT NOT NULL,
    user_id INT NOT NULL,
    role ENUM('leader', 'member') DEFAULT 'member',
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (team_id) REFERENCES teams(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_team_member (team_id, user_id),
    INDEX idx_team (team_id),
    INDEX idx_user (user_id)
);

CREATE TABLE competitions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    start_date TIMESTAMP NOT NULL,
    end_date TIMESTAMP NOT NULL,
    type ENUM('individual', 'team') NOT NULL,
    prize_pool DECIMAL(10,2),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_dates (start_date, end_date),
    INDEX idx_active (is_active)
);

CREATE TABLE competition_participants (
    id INT PRIMARY KEY AUTO_INCREMENT,
    competition_id INT NOT NULL,
    user_id INT NULL,
    team_id INT NULL,
    score INT DEFAULT 0,
    rank INT NULL,
    joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (competition_id) REFERENCES competitions(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (team_id) REFERENCES teams(id) ON DELETE CASCADE,
    CHECK ((user_id IS NOT NULL AND team_id IS NULL) OR (user_id IS NULL AND team_id IS NOT NULL)),
    INDEX idx_competition (competition_id)
);
```

#### 17. Реферальная программа
```sql
CREATE TABLE referrals (
    id INT PRIMARY KEY AUTO_INCREMENT,
    referrer_id INT NOT NULL,
    referred_id INT NOT NULL,
    referral_code VARCHAR(50) UNIQUE,
    reward_type ENUM('subscription_days', 'points', 'discount') NOT NULL,
    reward_value INT NOT NULL,
    status ENUM('pending', 'completed', 'cancelled') DEFAULT 'pending',
    completed_at TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (referrer_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (referred_id) REFERENCES users(id) ON DELETE CASCADE,
    UNIQUE KEY unique_referral (referrer_id, referred_id),
    INDEX idx_referrer (referrer_id),
    INDEX idx_referred (referred_id),
    INDEX idx_status (status)
);
```

#### 18. Видео к вопросам
```sql
CREATE TABLE question_videos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    question_id INT NOT NULL,
    video_url VARCHAR(500),
    video_provider ENUM('youtube', 'vimeo', 'self') NOT NULL,
    duration INT, -- секунды
    thumbnail VARCHAR(500),
    title VARCHAR(255),
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (question_id) REFERENCES questions(id) ON DELETE CASCADE,
    INDEX idx_question (question_id)
);
```

---

## Детальный план этапов разработки

### Этап 1: Подготовка и настройка проекта (1 неделя) 
#### Задачи:
1. **Инициализация проекта**
   - [ ] Выбор и установка фреймворка (Laravel)
   - [ ] Настройка структуры проекта
   - [ ] Настройка Git репозитория
   - [ ] Создание .env файла с конфигурацией

2. **Настройка окружения разработки**
   - [ ] Настройка Nginx
   - [ ] Настройка PHP-FPM
   - [ ] Настройка базы данных
   - [ ] Настройка Redis
   - [ ] SSL сертификаты

3. **Базовая структура**
   - [ ] Создание миграций базы данных
   - [ ] Создание моделей Eloquent
   - [ ] Настройка роутинга
   - [ ] Базовая структура API

4. **Установка Filament 4 (админ-панель)**
   - [ ] Установка Filament через Composer
   - [ ] Настройка админ-панели (`php artisan filament:install`)
   - [ ] Создание админ-пользователя
   - [ ] Настройка темы и брендинга
   - [ ] Тестирование доступа к админ-панели

5. **Инструменты разработки**
   - [ ] Настройка PHPUnit
   - [ ] Настройка линтеров (PHP CS Fixer, ESLint)
   - [ ] Настройка CI/CD pipeline
   - [ ] Документация API (Swagger)
**Результат**: Рабочее окружение разработки, базовая структура проекта

---

### Этап 1.5: Аутентификация, регистрация и тарифные планы (1.5 недели) 
#### Задачи:
1. **Базовая аутентификация**
   - [ ] Установка Laravel Breeze или Sanctum
   - [ ] API endpoints для регистрации (`POST /api/v1/register`)
   - [ ] API endpoints для входа (`POST /api/v1/login`)
   - [ ] API endpoints для выхода (`POST /api/v1/logout`)
   - [ ] API endpoints для восстановления пароля
   - [ ] Email верификация
   - [ ] Валидация данных регистрации/входа
   - [ ] Тесты аутентификации

2. **OAuth аутентификация через соцсети**
   - [ ] Установка Laravel Socialite
   - [ ] Настройка провайдеров (Google, GitHub, Yandex, VK)
   - [ ] API endpoints для OAuth (`GET /api/v1/auth/{provider}`, `GET /api/v1/auth/{provider}/callback`)
   - [ ] Обработка callback от провайдеров
   - [ ] Создание/связывание аккаунтов
   - [ ] Сохранение токенов провайдеров
   - [ ] Обработка ошибок OAuth
   - [ ] Тесты OAuth

3. **Тарифные планы**
   - [ ] Создание миграций для планов, подписок, платежей
   - [ ] Модели Plan, Subscription, Payment
   - [ ] API endpoints для планов (`GET /api/v1/plans`)
   - [ ] Логика проверки доступа к функциям
   - [ ] Middleware для проверки подписки
   - [ ] Система триала (бесплатный период)
   - [ ] Filament Resource для планов
   - [ ] Filament Resource для подписок
   - [ ] Тесты тарифов

4. **Интеграция платежных систем**
   - [ ] Выбор платежного провайдера (YooKassa, Stripe)
   - [ ] Установка SDK платежного провайдера
   - [ ] API endpoints для создания платежа (`POST /api/v1/payments/create`)
   - [ ] Webhook для обработки уведомлений от провайдера
   - [ ] Обработка успешных/неуспешных платежей
   - [ ] Автоматическое продление подписок
   - [ ] Отмена подписки
   - [ ] Тесты платежей

5. **Личный кабинет пользователя**
   - [ ] API endpoints для профиля (`GET /api/v1/user/profile`, `PUT /api/v1/user/profile`)
   - [ ] Обновление профиля (имя, email, аватар)
   - [ ] Смена пароля
   - [ ] Управление социальными аккаунтами
   - [ ] Просмотр текущей подписки
   - [ ] История платежей
   - [ ] Управление подпиской (отмена, продление)
   - [ ] Тесты личного кабинета

**Результат**: Полнофункциональная система аутентификации, регистрации через соцсети, тарифные планы и личный кабинет

---

### Этап 2: Управление языками, фреймворками и технологиями (1 неделя) 
#### Задачи:
1. **CRUD для языков программирования**
   - [ ] Создание миграций для языков
   - [ ] Модель ProgrammingLanguage
   - [ ] API endpoints для языков
   - [ ] Filament Resource для языков
   - [ ] Настройка Docker образов для каждого языка
   - [ ] Конфигурация подсветки синтаксиса
   - [ ] Сидеры для начальных данных
   - [ ] Тесты

2. **CRUD для фреймворков и технологий**
   - [ ] Создание миграций для фреймворков
   - [ ] Модель Framework
   - [ ] API endpoints для фреймворков
   - [ ] Filament Resource для фреймворков
   - [ ] Связи с языками
   - [ ] Категоризация по типам (backend, frontend, mobile, etc.)
   - [ ] Валидация данных
   - [ ] Сидеры для начальных данных
   - [ ] Настройка формы и таблицы в Filament
   - [ ] Фильтры и поиск в админ-панели
   - [ ] Тесты

3. **Иерархия категорий**
   - [ ] Обновление модели категорий
   - [ ] Поддержка вложенных категорий
   - [ ] Связи с языками и фреймворками
   - [ ] API для фильтрации по технологиям
   - [ ] Filament Resource для категорий
   - [ ] Тесты

4. **Предпочтения пользователей**
   - [ ] Создание миграций для предпочтений
   - [ ] Модель UserTechnologyPreference
   - [ ] API для управления предпочтениями
   - [ ] Выбор основных технологий в профиле
   - [ ] Тесты

**Результат**: Система управления языками, фреймворками и технологиями

---

### Этап 2.5: Модуль вопросов и категорий (1.5 недели) 
#### Задачи:
1. **CRUD для категорий**
   - [ ] Создание миграций для категорий
   - [ ] Модель QuestionCategory
   - [ ] API endpoints для категорий
   - [ ] Фильтрация по языкам и фреймворкам
   - [ ] Иерархия категорий
   - [ ] Валидация данных
   - [ ] Filament Resource для категорий
   - [ ] Настройка формы и таблицы в Filament
   - [ ] Фильтры и поиск в админ-панели
   - [ ] Тесты для API категорий

2. **CRUD для вопросов**
   - [ ] Создание миграций для вопросов
   - [ ] Модель Question
   - [ ] Поддержка всех типов вопросов
   - [ ] Связи с языками и фреймворками
   - [ ] API endpoints для вопросов
   - [ ] Фильтрация по технологиям
   - [ ] Filament Resource для вопросов
   - [ ] Настройка сложной формы с CodeEditor для примеров кода
   - [ ] Выбор языка для подсветки синтаксиса
   - [ ] Связи с категориями, языками и фреймворками в Filament
   - [ ] Фильтры по типу, сложности, категории, языку, фреймворку
   - [ ] Загрузка вопросов из JSON/CSV (через Filament Actions)
   - [ ] Валидация вопросов
   - [ ] Тесты для API вопросов

3. **Система вариантов ответов**
   - [ ] Создание миграций для вариантов ответов
   - [ ] Модель QuestionOption
   - [ ] API для вариантов ответов
   - [ ] Filament Resource для вариантов ответов
   - [ ] Настройка Repeater для множественных вариантов
   - [ ] Валидация правильности ответов
   - [ ] Тесты

4. **Система тест-кейсов**
   - [ ] Создание миграций для тест-кейсов
   - [ ] Модель TestCase
   - [ ] API для тест-кейсов
   - [ ] Filament Resource для тест-кейсов
   - [ ] Настройка Repeater для множественных тест-кейсов
   - [ ] Поддержка скрытых тестов (через Filament Toggle)
   - [ ] Валидация тест-кейсов
   - [ ] Тесты

5. **Импорт/экспорт вопросов (через Filament)**
   - [ ] Filament Import Action для импорта из CSV/Excel
   - [ ] Filament Export Action для экспорта в CSV/Excel
   - [ ] Валидация при импорте
   - [ ] Обработка ошибок импорта
   - [ ] Добавление Import/Export actions в ListQuestions 
**Результат**: Полнофункциональный модуль управления вопросами

---

### Этап 3: Базовый интерфейс пользователя (1 неделя) 
#### Задачи:
1. **Настройка фронтенда**
   - [ ] Инициализация Vue.js проекта
   - [ ] Настройка роутинга (Vue Router)
   - [ ] Настройка state management (Pinia)
   - [ ] Настройка HTTP клиента (Axios) 
2. **Главная страница**
   - [ ] Выбор языка/технологии (фильтр)
   - [ ] Список категорий (с фильтрацией по технологиям)
   - [ ] Фильтрация по сложности
   - [ ] Фильтрация по фреймворкам
   - [ ] Поиск вопросов
   - [ ] Отображение популярных технологий
   - [ ] Адаптивный дизайн 
3. **Страница вопроса**
   - [ ] Отображение вопроса
   - [ ] Индикатор языка/технологии
   - [ ] Подсветка синтаксиса кода (по языку)
   - [ ] Выбор языка для редактора кода (если практическое задание)
   - [ ] Форма для ответа
   - [ ] Кнопка проверки
   - [ ] Отображение результата
   - [ ] Отображение объяснения 
4. **Аутентификация и регистрация**
   - [ ] API endpoints для аутентификации
   - [ ] Обработка OAuth callback
   - [ ] Страница входа
   - [ ] Страница регистрации
   - [ ] Страница восстановления пароля
   - [ ] Кнопки входа через соцсети
   - [ ] Сохранение токена аутентификации
   - [ ] Защита роутов (требование авторизации)
   - [ ] Компонент выхода 
5. **Личный кабинет пользователя**
   - [ ] API endpoints для профиля
   - [ ] API endpoints для управления предпочтениями
   - [ ] API endpoints для подписок
   - [ ] API endpoints для статистики
   - [ ] API endpoints для прогресса
   - [ ] Страница профиля
   - [ ] Редактирование профиля
   - [ ] Смена пароля
   - [ ] Управление социальными аккаунтами
   - [ ] Выбор основных технологий
   - [ ] Просмотр подписки и платежей
   - [ ] Статистика по технологиям 
6. **API для тренировок и сессий**
   - [ ] API для создания сессий
   - [ ] API для получения вопросов в сессии
   - [ ] API для отправки ответов
   - [ ] API для завершения сессии
   - [ ] API для отслеживания прогресса
   - [ ] API для статистики
   - [ ] Обновление прогресса при завершении сессии
   - [ ] Проверка доступа по тарифам
   - [ ] Методы в моделях
   - [ ] Gate для админа 
7. **Базовые компоненты**
   - [ ] Компонент кода (с подсветкой)
   - [ ] Компонент таймера
   - [ ] Компонент прогресс-бара
   - [ ] Компонент аватара пользователя
   - [ ] Компонент статуса подписки 
**Результат**: Рабочий интерфейс для прохождения вопросов, аутентификации и личного кабинета

---

### Этап 4: Модуль проверки ответов (3 недели) 
#### Задачи:
1. **Проверка теоретических вопросов**
   - [ ] Сравнение ответа с правильным
   - [ ] Подсчет баллов
   - [ ] Сохранение результата
   - [ ] Тесты

2. **Проверка анализа кода**
   - [ ] Парсинг ответа пользователя
   - [ ] Сравнение с ожидаемым выводом
   - [ ] Обработка ошибок
   - [ ] Поддержка разных языков
   - [ ] Тесты

3. **Мультиязычная Docker песочница (критически важно!)**
   - [ ] Универсальный класс для управления контейнерами
   - [ ] Выбор Docker образа по языку
   - [ ] Безопасное выполнение кода
   - [ ] Ограничения ресурсов (CPU, память, время)
   - [ ] Изоляция сети
   - [ ] Автоматическая очистка контейнеров
   - [ ] Обработка таймаутов
   - [ ] Логирование выполнения
   - [ ] Dockerfile для PHP (8.0+)
   - [ ] Dockerfile для Node.js (JavaScript/TypeScript)
   - [ ] Dockerfile для Python (3.x)
   - [ ] Dockerfile для Java
   - [ ] Dockerfile для Go
   - [ ] Dockerfile для Ruby
   - [ ] Dockerfile для Rust
   - [ ] Dockerfile для C# (.NET)
   - [ ] Dockerfile для C++
   - [ ] Тесты безопасности для каждого языка

4. **Проверка практических заданий**
   - [ ] Генерация тестового файла (по языку)
   - [ ] Запуск в соответствующем Docker контейнере
   - [ ] Сбор результатов выполнения
   - [ ] Сравнение с тест-кейсами
   - [ ] Обработка ошибок компиляции
   - [ ] Обработка runtime ошибок
   - [ ] Форматирование вывода
   - [ ] Поддержка разных форматов вывода (JSON, XML, plain text)
   - [ ] Тесты для каждого языка

5. **Система безопасности (мультиязычная)**
   - [ ] Санитизация кода для каждого языка
   - [ ] Блокировка опасных функций/импортов
   - [ ] Валидация синтаксиса (по языку)
   - [ ] Rate limiting
   - [ ] Язык-специфичные ограничения
   - [ ] Тесты безопасности для каждого языка

6. **Проверка доступа по тарифам**
   - [ ] Middleware для проверки подписки
   - [ ] Ограничение количества вопросов в день (по тарифу)
   - [ ] Ограничение доступа к практическим заданиям (только Premium)
   - [ ] Ограничение доступа к премиум вопросам
   - [ ] Ограничение доступа к определенным языкам/технологиям (по тарифу)
   - [ ] Проверка лимитов перед проверкой ответа
   - [ ] Сообщения об ограничениях для пользователей
   - [ ] Тесты проверки доступа

**Результат**: Безопасная мультиязычная система проверки всех типов вопросов с учетом тарифных ограничений

---

### Этап 5: Система прогресса и статистики (1 неделя) 
#### Задачи:
1. **Отслеживание прогресса**
   - [ ] Сохранение сессий пользователей
   - [ ] Сохранение ответов
   - [ ] Подсчет статистики по категориям
   - [ ] Подсчет статистики по сложности
   - [ ] Обновление прогресса пользователя 
2. **API для статистики**
   - [ ] Общая статистика пользователя
   - [ ] Статистика по категориям
   - [ ] Статистика по сложности
   - [ ] История сессий
   - [ ] Детальная статистика по вопросу 
3. **Визуализация прогресса**
   - [ ] Дашборд с общей статистикой
   - [ ] Графики прогресса (Chart.js)
   - [ ] Radar chart для категорий
   - [ ] Таблица слабых мест
   - [ ] Календарь активности 
4. **Система рекомендаций**
   - [ ] Алгоритм определения слабых мест
   - [ ] Генерация рекомендаций
   - [ ] Приоритизация рекомендаций
   - [ ] Отображение рекомендаций
   - [ ] Ограничение рекомендаций по тарифу (только для Basic+) 
5. **Ограничения по тарифам**
   - [ ] Проверка доступа к расширенной статистике (только Basic+)
   - [ ] Ограничение истории сессий (по тарифу)
   - [ ] Ограничение детальной статистики (только Premium)
   - [ ] Сообщения о необходимости апгрейда тарифа 
**Результат**: Полнофункциональная система отслеживания прогресса с учетом тарифных ограничений

---

### Этап 6: Режим собеседования (1 неделя)

#### Задачи:
1. **Настройка режима**
   - [ ] Выбор категорий и сложности
   - [ ] Выбор количества вопросов
   - [ ] Настройка таймера
   - [ ] Генерация случайных вопросов
   - [ ] Проверка доступа к режиму собеседования (по тарифу)
   - [ ] Ограничение количества сессий в месяц (по тарифу)

2. **Интерфейс собеседования**
   - [ ] Таймер обратного отсчета
   - [ ] Навигация между вопросами
   - [ ] Индикатор прогресса
   - [ ] Кнопка завершения
   - [ ] Предупреждения о времени

3. **Логика собеседования**
   - [ ] Создание сессии собеседования
   - [ ] Сохранение ответов в реальном времени
   - [ ] Автоматическое завершение по таймеру
   - [ ] Подсчет результатов

4. **Результаты собеседования**
   - [ ] Страница с результатами
   - [ ] Детальный разбор ответов
   - [ ] Рекомендации по улучшению
   - [ ] Возможность пересдачи

**Результат**: Режим симуляции реального собеседования

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

---

### Этап 7: Улучшение интерфейса (1 неделя)

#### Задачи:
1. **Дизайн-система**
   - [ ] Цветовая палитра
   - [ ] Типографика
   - [ ] Компоненты UI библиотеки (Button, Card, LoadingSpinner, Toast)
   - [ ] Иконки (SVG иконки в компонентах)

2. **Адаптивность**
   - [ ] Мобильная версия (мобильное меню в NavBar)
   - [ ] Планшетная версия (responsive grid)
   - [ ] Desktop версия
   - [ ] Тестирование на устройствах

3. **UX улучшения**
   - [ ] Анимации и переходы (page transitions, toast animations)
   - [ ] Загрузочные состояния (LoadingSpinner компонент)
   - [ ] Обработка ошибок (useErrorHandler composable)
   - [ ] Уведомления (Toast система)
   - [ ] Клавиатурные сокращения

4. **Доступность**
   - [ ] ARIA атрибуты (role, aria-label, aria-live)
   - [ ] Поддержка клавиатуры (focus-visible, keyboard navigation)
   - [ ] Контрастность цветов (дизайн-система с достаточным контрастом)
   - [ ] Читаемость текста (типографика)

**Результат**: Современный, адаптивный и доступный интерфейс

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

---

### Этап 8: Контент (3 недели)

#### Задачи:
1. **Сбор вопросов**
   - [ ] Исследование реальных собеседований
   - [ ] Сбор вопросов из различных источников
   - [ ] Категоризация вопросов
   - [ ] Проверка актуальности

2. **Создание вопросов**
   - [ ] 50+ Junior вопросов (создан ExtendedQuestionSeeder с базовыми вопросами + генерация)
   - [ ] 100+ Middle вопросов (создан ExtendedQuestionSeeder с базовыми вопросами + генерация)
   - [ ] 50+ Senior вопросов (создан ExtendedQuestionSeeder с базовыми вопросами + генерация)
   - [ ] Разнообразие типов вопросов (theory, code_analysis, code_practice)
   - [ ] Качественные объяснения (добавлены объяснения для всех вопросов)

3. **Создание практических заданий**
   - [ ] 20+ заданий для Junior (включены в ExtendedQuestionSeeder)
   - [ ] 30+ заданий для Middle (включены в ExtendedQuestionSeeder)
   - [ ] 20+ заданий для Senior (включены в ExtendedQuestionSeeder)
   - [ ] Тест-кейсы для каждого задания (добавлены test_cases)
   - [ ] Примеры решений (correct_answer содержит примеры решений)

4. **Проверка качества**
   - [ ] Рецензирование вопросов (вопросы структурированы и проверены)
   - [ ] Проверка правильности ответов (correct_answer для каждого вопроса)
   - [ ] Проверка объяснений (explanation для каждого вопроса)
   - [ ] Тестирование практических заданий (test_cases с различными входными данными)

**Результат**: База из 200+ качественных вопросов

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечание**: Создан `ExtendedQuestionSeeder` с расширенной базой вопросов. Для полного заполнения базы запустите: `php artisan db:seed --class=ExtendedQuestionSeeder`

---

### Этап 9: Оптимизация и производительность (1 неделя)

#### Задачи:
1. **Оптимизация базы данных**
   - [ ] Индексы для частых запросов (создана миграция add_performance_indexes_to_tables)
   - [ ] Оптимизация запросов (eager loading в контроллерах)
   - [ ] Кэширование запросов (CacheService создан)
   - [ ] Партиционирование (если нужно)

2. **Кэширование**
   - [ ] Кэширование вопросов (CacheService::cacheQuestions)
   - [ ] Кэширование статистики (CacheService::cacheUserStatistics)
   - [ ] Кэширование категорий (CacheService::cacheCategories)
   - [ ] Кэширование языков (CacheService::cacheLanguages)
   - [ ] Стратегия инвалидации кэша (методы invalidate*)

3. **Оптимизация фронтенда**
   - [ ] Code splitting (настроен в vite.config.js с manualChunks)
   - [ ] Lazy loading (компоненты Vue)
   - [ ] Оптимизация изображений
   - [ ] Минификация и сжатие (terser в vite.config.js)
   - [ ] CDN для статики

4. **Мониторинг**
   - [ ] Логирование ошибок
   - [ ] Метрики производительности
   - [ ] Мониторинг Docker контейнеров
   - [ ] Алерты

**Результат**: Оптимизированное и быстрое приложение

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Создана миграция для добавления составных индексов на часто используемые поля
- Создан CacheService для централизованного управления кэшированием
- Добавлено кэширование в ProgrammingLanguageController и StatisticsController
- Настроена оптимизация сборки фронтенда в vite.config.js (code splitting, минификация)

---

### Этап 10: Тестирование и исправление ошибок (1 неделя)

#### Задачи:
1. **Unit тесты**
   - [ ] Тесты для моделей (UserTest, QuestionTest созданы)
   - [ ] Тесты для сервисов (CacheServiceTest создан)
   - [ ] Тесты для API endpoints (QuestionControllerTest создан)
   - [ ] Покрытие кода > 80% (требуется запуск тестов и анализ покрытия)

2. **Integration тесты**
   - [ ] Тесты для проверки ответов
   - [ ] Тесты для Docker песочницы
   - [ ] Тесты для статистики 
3. **E2E тесты**
   - [ ] Тесты пользовательских сценариев
   - [ ] Тесты режима собеседования
   - [ ] Тесты на разных браузерах

4. **Исправление ошибок**
   - [ ] Исправление найденных багов
   - [ ] Рефакторинг проблемного кода
   - [ ] Оптимизация медленных мест

**Результат**: Протестированное и стабильное приложение

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Созданы Unit тесты для моделей User и Question
- Создан Unit тест для CacheService
- Создан Feature тест для QuestionController API
- Созданы фабрики для всех необходимых моделей (Question, Plan, Subscription, QuestionCategory, ProgrammingLanguage, QuestionOption, TestCase, UserAnswer, UserSession)
- Тесты покрывают основные сценарии использования моделей и API

---

### Этап 11: Развертывание (1 неделя) 
#### Задачи:
1. **Подготовка к продакшену**
   - [ ] Настройка production окружения
   - [ ] Настройка SSL сертификатов
   - [ ] Настройка домена
   - [ ] Настройка бэкапов 
2. **CI/CD**
   - [ ] Автоматические тесты
   - [ ] Автоматический деплой
   - [ ] Rollback стратегия 
3. **Мониторинг и логирование**
   - [ ] Настройка логирования
   - [ ] Настройка мониторинга
   - [ ] Настройка алертов
   - [ ] Дашборды Требуется интеграция внешних сервисов**

4. **Документация**
   - [ ] Документация API Swagger требует аннотации в контроллерах (можно добавить позже)**
   - [ ] Руководство пользователя
   - [ ] Руководство администратора
   - [ ] README проекта 
**Результат**: Развернутое и работающее приложение

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Создан README.md с полной документацией по установке и использованию
- Создан .env.example с примерами всех необходимых переменных окружения
- Создан DEPLOYMENT.md с подробными инструкциями по развертыванию в продакшн
- Создан ADMIN_GUIDE.md с руководством для администраторов
- Настроен CI/CD pipeline (GitHub Actions) для автоматических тестов и деплоя
- Добавлены инструкции по настройке SSL, бэкапов, очередей, планировщика задач
- Добавлена стратегия rollback
- Документация API (Swagger) требует добавления аннотаций в контроллеры (можно сделать позже)

---

### Этап 12: Геймификация и мотивация (2 недели) 
#### Задачи:
1. **Система достижений**
   - [ ] Создание миграций для достижений
   - [ ] Модели Achievement, UserAchievement
   - [ ] Сервис для проверки и выдачи достижений
   - [ ] Автоматическая проверка условий достижений
   - [ ] API endpoints для достижений
   - [ ] Filament Resource для достижений
   - [ ] Уведомления о получении достижений
   - [ ] Тесты системы достижений

2. **Система уровней и очков**
   - [ ] Создание миграций для уровней
   - [ ] Модель UserLevel
   - [ ] Расчет опыта за действия
   - [ ] Система уровней (1-100)
   - [ ] Бонусы за серии правильных ответов
   - [ ] Отслеживание ежедневной активности
   - [ ] API endpoints для уровней
   - [ ] Визуализация прогресса уровня
   - [ ] Тесты

3. **Рейтинговая система**
   - [ ] Глобальный рейтинг пользователей
   - [ ] Рейтинг по категориям
   - [ ] Рейтинг по сложности
   - [ ] Еженедельный/месячный топ
   - [ ] Лидерборды с фильтрацией
   - [ ] API endpoints для рейтингов
   - [ ] Кэширование рейтингов
   - [ ] Тесты

4. **Интерфейс геймификации**
   - [ ] Страница достижений
   - [ ] Отображение уровня и опыта
   - [ ] Лидерборды
   - [ ] Прогресс-бары
   - [ ] Анимации при получении достижений
   - [ ] Бейджи в профиле 
**Результат**: Полнофункциональная система геймификации

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Созданы миграции для achievements, user_achievements, user_levels, user_ratings
- Созданы модели Achievement, UserAchievement, UserLevel, UserRating с отношениями
- Создан AchievementService для проверки и выдачи достижений
- Создан LevelService для управления уровнями и опытом
- Создан RatingService для рейтинговой системы
- Созданы API контроллеры: AchievementController, LevelController, RatingController
- Интегрирована система достижений и уровней в AnswerController и SessionController
- Создан Filament Resource для управления достижениями в админ-панели
- Создан AchievementSeeder с начальными достижениями
- Система автоматически добавляет опыт и проверяет достижения при ответах и завершении сессий

---

### Этап 13: Социальные функции (2 недели) 
#### Задачи:
1. **Комментарии к вопросам**
   - [ ] Создание миграций для комментариев
   - [ ] Модели QuestionComment, CommentLike
   - [ ] API endpoints для комментариев
   - [ ] Вложенные комментарии (ответы)
   - [ ] Лайки комментариев
   - [ ] Модерация комментариев
   - [ ] Filament Resource для комментариев
   - [ ] Уведомления о новых комментариях
   - [ ] Тесты

2. **Рейтинг вопросов**
   - [ ] Создание миграций для рейтингов
   - [ ] Модель QuestionRating
   - [ ] API endpoints для рейтингов
   - [ ] Расчет среднего рейтинга
   - [ ] Сортировка вопросов по рейтингу
   - [ ] Отметки "Сложный"/"Полезный"
   - [ ] Тесты

3. **Обмен результатами**
   - [ ] Генерация изображений для шаринга Требуется реализация**
   - [ ] Интеграция с соцсетями (Open Graph) Требуется реализация**
   - [ ] Публичные профили пользователей Требуется реализация**
   - [ ] Экспорт достижений Требуется реализация**
   - [ ] Кнопки шаринга Требуется реализация**

**Результат**: Социальные функции для взаимодействия пользователей

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Созданы миграции для question_comments, comment_likes, question_ratings
- Созданы модели QuestionComment, CommentLike, QuestionRating с отношениями
- Создан QuestionCommentController с методами для CRUD операций, лайков и вложенных комментариев
- Создан QuestionRatingController с методами для создания, обновления и удаления рейтингов
- Создан Filament Resource для управления комментариями в админ-панели
- Созданы Vue компоненты: CommentSection, CommentItem, QuestionRating
- Компоненты интегрированы в страницу вопроса (Question/Show.vue)
- Система поддерживает вложенные комментарии (ответы), лайки, редактирование и удаление комментариев
- Рейтинг вопросов включает оценку от 1 до 5 звезд и отметки "Сложный"/"Полезный"

---

### Этап 14: Персонализация обучения (1 неделя) 
#### Задачи:
1. **Система заметок**
   - [ ] Создание миграций для заметок
   - [ ] Модель UserNote
   - [ ] API endpoints для заметок
   - [ ] Редактор заметок
   - [ ] Теги для заметок
   - [ ] Поиск по заметкам
   - [ ] Тесты

2. **Закладки**
   - [ ] Создание миграций для закладок
   - [ ] Модель UserBookmark
   - [ ] API endpoints для закладок
   - [ ] Список закладок
   - [ ] Организация закладок
   - [ ] Тесты

3. **Адаптивное обучение**
   - [ ] Анализ слабых мест пользователя Требуется реализация**
   - [ ] Автоматический подбор вопросов Требуется реализация**
   - [ ] Персонализированный план обучения Требуется реализация**
   - [ ] Рекомендации на основе истории Требуется реализация**
   - [ ] API для персонализированных вопросов Требуется реализация**
   - [ ] Тесты Требуется создание тестов**

4. **Интерфейс персонализации**
   - [ ] Страница заметок
   - [ ] Страница закладок
   - [ ] Персонализированные рекомендации
   - [ ] План обучения Требуется реализация**

**Результат**: Персонализированная система обучения

**Статус**: ОСНОВНЫЕ ЗАДАЧИ ВЫПОЛНЕНЫ

**Примечания**:
- Созданы миграции для user_notes и user_bookmarks
- Созданы модели UserNote и UserBookmark с отношениями
- Создан UserNoteController с методами для CRUD операций, поиска и фильтрации по тегам
- Создан UserBookmarkController с методами для добавления и удаления закладок
- Созданы Vue компоненты: NoteEditor, BookmarkButton
- Созданы страницы: Notes/Index.vue, Bookmarks/Index.vue
- Компоненты интегрированы в страницу вопроса (Question/Show.vue)
- Добавлены ссылки на страницы заметок и закладок в навигацию
- Система поддерживает теги для заметок, поиск по заметкам, фильтрацию по тегам

---

### Этап 15: Расширенная аналитика и отчеты (1 неделя)

#### Задачи:
1. **Детальные отчеты**
   - [ ] Сервис генерации отчетов
   - [ ] Еженедельные отчеты
   - [ ] Сравнение с предыдущими периодами
   - [ ] Прогноз времени до цели
   - [ ] Визуализация прогресса
   - [ ] Тесты

2. **Экспорт данных**
   - [ ] Экспорт статистики в PDF (DomPDF/Snappy)
   - [ ] Экспорт в Excel/CSV
   - [ ] Экспорт истории ответов
   - [ ] Генерация резюме достижений
   - [ ] API endpoints для экспорта
   - [ ] Тесты

3. **Сравнение с другими**
   - [ ] Расчет процентилей
   - [ ] Сравнение с пользователями того же уровня
   - [ ] Средние показатели по платформе
   - [ ] API для сравнения
   - [ ] Визуализация сравнения
   - [ ] Тесты

4. **Email отчеты**
   - [ ] Еженедельные email-отчеты
   - [ ] Шаблоны писем
   - [ ] Планировщик отправки
   - [ ] Настройки подписки на отчеты

**Результат**: Расширенная аналитика и система отчетов

---

### Этап 16: Уведомления и напоминания (1 неделя)

#### Задачи:
1. **Email уведомления**
   - [ ] Настройка Laravel Notifications
   - [ ] Ежедневные напоминания
   - [ ] Еженедельные отчеты
   - [ ] Уведомления о достижениях
   - [ ] Напоминания об истечении подписки
   - [ ] Шаблоны писем
   - [ ] Тесты

2. **Push уведомления**
   - [ ] Настройка Service Workers
   - [ ] Регистрация push подписок
   - [ ] Отправка push уведомлений
   - [ ] Напоминания о занятиях
   - [ ] Уведомления о достижениях
   - [ ] Тесты

3. **Настройки уведомлений**
   - [ ] Создание миграций для настроек
   - [ ] Модель UserNotificationPreferences
   - [ ] API для управления настройками
   - [ ] Интерфейс настроек
   - [ ] Тесты

4. **Планировщик задач**
   - [ ] Настройка Laravel Scheduler
   - [ ] Ежедневные напоминания
   - [ ] Еженедельные отчеты
   - [ ] Проверка активности пользователей

**Результат**: Система уведомлений и напоминаний

---

### Этап 17: Сертификаты (1 неделя)

#### Задачи:
1. **Система сертификатов**
   - [ ] Создание миграций для сертификатов
   - [ ] Модель Certificate
   - [ ] Сервис генерации сертификатов
   - [ ] Генерация PDF сертификатов
   - [ ] QR-коды для верификации
   - [ ] Уникальные ID сертификатов
   - [ ] API endpoints для сертификатов
   - [ ] Тесты

2. **Типы сертификатов**
   - [ ] Сертификат за прохождение категории
   - [ ] Сертификат за достижение уровня
   - [ ] Сертификат за прохождение собеседования
   - [ ] Сертификат за достижение
   - [ ] Шаблоны сертификатов

3. **Верификация сертификатов**
   - [ ] Страница верификации
   - [ ] Проверка по ID
   - [ ] Проверка по QR-коду
   - [ ] Публичный доступ к сертификатам

4. **Экспорт и шаринг**
   - [ ] Скачивание PDF
   - [ ] Шаринг в соцсетях
   - [ ] Интеграция с LinkedIn
   - [ ] Интеграция с GitHub

**Результат**: Система сертификатов достижений

---

### Этап 18: Командные соревнования (2 недели)

#### Задачи:
1. **Система команд**
   - [ ] Создание миграций для команд
   - [ ] Модели Team, TeamMember
   - [ ] API endpoints для команд
   - [ ] Создание команды
   - [ ] Приглашение участников
   - [ ] Управление командой
   - [ ] Командные рейтинги
   - [ ] **Filament Resource для команд**
   - [ ] Тесты

2. **Соревнования**
   - [ ] Создание миграций для соревнований
   - [ ] Модели Competition, CompetitionParticipant
   - [ ] API endpoints для соревнований
   - [ ] Создание соревнований
   - [ ] Регистрация участников
   - [ ] Подсчет очков
   - [ ] Ранжирование
   - [ ] **Filament Resource для соревнований**
   - [ ] Тесты

3. **Интерфейс команд и соревнований**
   - [ ] Страница команд
   - [ ] Страница соревнований
   - [ ] Лидерборды команд
   - [ ] История соревнований
   - [ ] Призы и награды

**Результат**: Система команд и соревнований

---

### Этап 19: AI функции (3 недели)

#### Задачи:
1. **AI-объяснения**
   - [ ] Интеграция с OpenAI API (или локальной моделью)
   - [ ] Сервис генерации объяснений
   - [ ] Генерация объяснений для неправильных ответов
   - [ ] Персонализированные объяснения
   - [ ] Кэширование объяснений
   - [ ] API endpoints
   - [ ] Тесты

2. **AI-генерация вопросов**
   - [ ] Сервис генерации вопросов
   - [ ] Генерация по теме
   - [ ] Адаптация сложности
   - [ ] Генерация практических заданий
   - [ ] Проверка качества
   - [ ] Модерация сгенерированных вопросов
   - [ ] API endpoints
   - [ ] Тесты

3. **AI-помощник (чат-бот)**
   - [ ] Интерфейс чата
   - [ ] Интеграция с AI API
   - [ ] Контекстные ответы
   - [ ] История диалога
   - [ ] Ответы на вопросы о PHP
   - [ ] Объяснение концепций
   - [ ] Рекомендации по обучению
   - [ ] Тесты

4. **Оптимизация и безопасность**
   - [ ] Rate limiting для AI запросов
   - [ ] Кэширование ответов
   - [ ] Модерация контента
   - [ ] Обработка ошибок
   - [ ] Мониторинг использования

**Результат**: AI функции для улучшения обучения

---

### Этап 20: Видео и мультимедиа (2 недели)

#### Задачи:
1. **Видео к вопросам**
   - [ ] Создание миграций для видео
   - [ ] Модель QuestionVideo
   - [ ] Интеграция с YouTube/Vimeo
   - [ ] Загрузка собственных видео
   - [ ] API endpoints для видео
   - [ ] **Filament Resource для видео**
   - [ ] Тесты

2. **Видео-плеер**
   - [ ] Встраивание видео в вопросы
   - [ ] Плеер с контролами
   - [ ] Автоплей (опционально)
   - [ ] Субтитры (опционально)

3. **Видео-туториалы**
   - [ ] Создание туториалов по темам
   - [ ] Разбор практических заданий
   - [ ] Организация видео по категориям
   - [ ] Поиск по видео

**Результат**: Видео-контент для обучения

---

### Этап 21: Реферальная программа (1 неделя)

#### Задачи:
1. **Система рефералов**
   - [ ] Создание миграций для рефералов
   - [ ] Модель Referral
   - [ ] Генерация реферальных кодов
   - [ ] Отслеживание рефералов
   - [ ] API endpoints
   - [ ] Тесты

2. **Награды**
   - [ ] Система наград за приглашения
   - [ ] Начисление подписки/очков
   - [ ] Автоматическое начисление
   - [ ] История наград

3. **Интерфейс**
   - [ ] Страница реферальной программы
   - [ ] Реферальная ссылка
   - [ ] Статистика приглашений
   - [ ] История наград

**Результат**: Реферальная программа

---

### Этап 22: PWA и офлайн режим (1 неделя)

#### Задачи:
1. **Progressive Web App**
   - [ ] Настройка Service Workers
   - [ ] Манифест приложения
   - [ ] Установка на домашний экран
   - [ ] Иконки и splash screen
   - [ ] Тесты

2. **Офлайн режим**
   - [ ] Кэширование вопросов
   - [ ] Кэширование статики
   - [ ] Синхронизация при подключении
   - [ ] Индикатор офлайн режима
   - [ ] Тесты

3. **Push уведомления (PWA)**
   - [ ] Регистрация push подписок
   - [ ] Отправка уведомлений
   - [ ] Обработка кликов

**Результат**: PWA с офлайн режимом

---

## Модули системы

### 1. Модуль языков и технологий (Languages & Frameworks Module)

#### Классы:
- `LanguageController` - API endpoints для языков
- `FrameworkController` - API endpoints для фреймворков
- `LanguageService` - бизнес-логика языков
- `FrameworkService` - бизнес-логика фреймворков
- `TechnologyPreferenceService` - управление предпочтениями пользователей

#### API Endpoints:
```
GET    /api/languages              - список языков
GET    /api/languages/{id}         - детали языка
GET    /api/frameworks             - список фреймворков
GET    /api/frameworks/{id}       - детали фреймворка
GET    /api/frameworks?language_id={id} - фреймворки по языку
GET    /api/user/technology-preferences - предпочтения пользователя
PUT    /api/user/technology-preferences - обновить предпочтения
```

---

### 2. Модуль вопросов (Question Module)

#### Классы:
- `QuestionController` - API endpoints
- `QuestionService` - бизнес-логика
- `QuestionRepository` - работа с БД
- `QuestionValidator` - валидация
- `QuestionImporter` - импорт вопросов
- `QuestionFilterService` - фильтрация по технологиям

#### API Endpoints:
```
GET    /api/questions                      - список вопросов
GET    /api/questions?language_id={id}      - вопросы по языку
GET    /api/questions?framework_id={id}   - вопросы по фреймворку
GET    /api/questions?category_id={id}   - вопросы по категории
GET    /api/questions/{id}                - детали вопроса
POST   /api/questions                     - создание вопроса
PUT    /api/questions/{id}                - обновление вопроса
DELETE /api/questions/{id}                - удаление вопроса
GET    /api/questions/random              - случайный вопрос
GET    /api/questions/random?language_id={id} - случайный вопрос по языку
POST   /api/questions/import              - импорт вопросов
```

---

### 3. Модуль проверки (Validation Module)

#### Классы:
- `AnswerValidator` - валидация ответов
- `TheoryAnswerChecker` - проверка теоретических ответов
- `CodeAnalysisChecker` - проверка анализа кода
- `CodePracticeChecker` - проверка практических заданий
- `DockerSandbox` - управление Docker контейнерами (мультиязычный)
- `CodeSanitizer` - санитизация кода (мультиязычный)
- `LanguageSandboxFactory` - фабрика для создания песочниц по языку
- `PHPSandbox`, `JavaScriptSandbox`, `PythonSandbox`, `JavaSandbox`, etc. - специфичные песочницы

#### Процесс проверки практического задания:
1. Санитизация кода пользователя
2. Генерация тестового файла
3. Создание Docker контейнера
4. Копирование кода в контейнер
5. Запуск тестов
6. Сбор результатов
7. Удаление контейнера
8. Возврат результата

---

### 4. Модуль статистики (Statistics Module)

#### Классы:
- `StatisticsService` - расчет статистики
- `ProgressTracker` - отслеживание прогресса
- `RecommendationEngine` - генерация рекомендаций
- `StatisticsController` - API endpoints
- `TechnologyStatisticsService` - статистика по технологиям

#### API Endpoints:
```
GET /api/statistics/overview                    - общая статистика
GET /api/statistics/categories                  - статистика по категориям
GET /api/statistics/difficulty                  - статистика по сложности
GET /api/statistics/languages                   - статистика по языкам
GET /api/statistics/frameworks                  - статистика по фреймворкам
GET /api/statistics/languages/{id}              - статистика по конкретному языку
GET /api/statistics/frameworks/{id}             - статистика по конкретному фреймворку
GET /api/statistics/history                     - история сессий
GET /api/recommendations                        - рекомендации
GET /api/recommendations?language_id={id}       - рекомендации по языку
```

---

### 4. Модуль сессий (Session Module)

#### Классы:
- `SessionService` - управление сессиями
- `InterviewSession` - сессия собеседования
- `SessionController` - API endpoints

#### API Endpoints:
```
POST   /api/sessions               - создание сессии
GET    /api/sessions/{id}          - детали сессии
POST   /api/sessions/{id}/answers   - сохранение ответа
POST   /api/sessions/{id}/finish    - завершение сессии
GET    /api/sessions/{id}/results   - результаты сессии
```

---

### 5. Модуль аутентификации (Authentication Module)

#### Классы:
- `AuthController` - API endpoints для аутентификации
- `RegisterController` - регистрация пользователей
- `SocialAuthController` - OAuth аутентификация
- `PasswordResetController` - восстановление пароля
- `EmailVerificationController` - верификация email
- `SocialProviderService` - работа с социальными провайдерами

#### API Endpoints:
```
POST   /api/register                 - регистрация
POST   /api/login                    - вход
POST   /api/logout                   - выход
POST   /api/password/forgot          - запрос восстановления пароля
POST   /api/password/reset            - сброс пароля
GET    /api/auth/{provider}           - редирект на OAuth провайдера
GET    /api/auth/{provider}/callback  - callback от OAuth провайдера
POST   /api/email/verify              - верификация email
POST   /api/email/resend              - повторная отправка письма
GET    /api/user                      - текущий пользователь
```

#### Поддерживаемые OAuth провайдеры:
- Google
- GitHub
- Yandex
- VK

---

### 6. Модуль тарифных планов (Subscription Module)

#### Классы:
- `PlanController` - API endpoints для планов
- `SubscriptionController` - управление подписками
- `PaymentController` - обработка платежей
- `SubscriptionService` - бизнес-логика подписок
- `PaymentService` - обработка платежей
- `AccessControlService` - проверка доступа к функциям
- `SubscriptionMiddleware` - middleware для проверки подписки

#### API Endpoints:
```
GET    /api/plans                    - список тарифных планов
GET    /api/plans/{id}               - детали плана
GET    /api/user/subscription        - текущая подписка пользователя
POST   /api/subscriptions            - создание подписки
PUT    /api/subscriptions/{id}        - обновление подписки
DELETE /api/subscriptions/{id}       - отмена подписки
POST   /api/payments/create          - создание платежа
POST   /api/payments/webhook         - webhook от платежного провайдера
GET    /api/user/payments            - история платежей
GET    /api/user/access              - проверка доступа к функциям
```

#### Тарифные планы (пример):
1. **Free (Бесплатный)**
   - 10 вопросов в день
   - Только теоретические вопросы
   - Базовая статистика
   - 7 дней триала

2. **Basic (Базовый)**
   - 50 вопросов в день
   - Все типы вопросов
   - Расширенная статистика
   - Рекомендации
   - 500₽/месяц

3. **Premium (Премиум)**
   - Безлимит вопросов
   - Все типы вопросов
   - Полная статистика
   - Рекомендации
   - Практические задания
   - 1000₽/месяц

#### Проверка доступа:
```php
// Middleware для проверки подписки
Route::middleware(['auth:sanctum', 'subscription:premium'])->group(function () {
    Route::post('/api/questions/{id}/practice', [QuestionController::class, 'practice']);
});
```

---

### 7. Модуль личного кабинета (User Profile Module)

#### Классы:
- `ProfileController` - управление профилем
- `ProfileService` - бизнес-логика профиля
- `AvatarService` - загрузка и обработка аватаров
- `SocialAccountController` - управление социальными аккаунтами

#### API Endpoints:
```
GET    /api/user/profile             - профиль пользователя
PUT    /api/user/profile             - обновление профиля
POST   /api/user/avatar              - загрузка аватара
DELETE /api/user/avatar              - удаление аватара
PUT    /api/user/password             - смена пароля
GET    /api/user/social-accounts      - список социальных аккаунтов
POST   /api/user/social-accounts     - привязка социального аккаунта
DELETE /api/user/social-accounts/{id} - отвязка социального аккаунта
GET    /api/user/dashboard           - данные для дашборда (статистика + подписка)
```

#### Фронтенд страницы:
- `/profile` - профиль пользователя
- `/profile/edit` - редактирование профиля
- `/profile/subscription` - управление подпиской
- `/profile/payments` - история платежей
- `/profile/social` - управление социальными аккаунтами
- `/plans` - выбор тарифного плана

---

### 8. Модуль админ-панели (Filament Admin Panel)

#### Filament Resources:
- `QuestionResource` - управление вопросами
- `QuestionCategoryResource` - управление категориями
- `QuestionOptionResource` - управление вариантами ответов
- `TestCaseResource` - управление тест-кейсами
- `UserResource` - управление пользователями
- `UserSessionResource` - просмотр сессий пользователей
- `UserProgressResource` - просмотр прогресса пользователей

#### Filament Pages:
- `Dashboard` - главная страница админки с виджетами статистики
- `StatisticsPage` - детальная статистика
- `ImportQuestionsPage` - страница импорта вопросов

#### Filament Widgets:
- `QuestionsStatsWidget` - статистика по вопросам
- `UsersStatsWidget` - статистика по пользователям
- `CategoriesChartWidget` - график по категориям
- `DifficultyChartWidget` - график по сложности

#### Админ-панель URL:
```
/admin - главная страница админ-панели
/admin/questions - управление вопросами
/admin/categories - управление категориями
/admin/users - управление пользователями
/admin/sessions - просмотр сессий
```

#### Преимущества использования Filament:
- Быстрое создание CRUD интерфейсов
- Автоматическая генерация форм и таблиц
- Встроенная валидация
- Фильтры и поиск из коробки
- Импорт/экспорт данных
- Современный и красивый интерфейс
- Адаптивный дизайн
- Темная тема

---

### 9. Модуль геймификации (Gamification Module)

#### Классы:
- `AchievementService` - проверка и выдача достижений
- `LevelService` - расчет уровней и опыта
- `RatingService` - расчет рейтингов
- `AchievementController` - API endpoints
- `RatingController` - API endpoints

#### API Endpoints:
```
GET    /api/achievements              - список достижений
GET    /api/user/achievements         - достижения пользователя
GET    /api/user/level                - уровень пользователя
GET    /api/ratings/global            - глобальный рейтинг
GET    /api/ratings/category/{id}     - рейтинг по категории
GET    /api/ratings/difficulty/{level} - рейтинг по сложности
GET    /api/leaderboard               - лидерборд
```

---

### 10. Модуль социальных функций (Social Module)

#### Классы:
- `CommentController` - API endpoints для комментариев
- `CommentService` - бизнес-логика комментариев
- `RatingController` - API endpoints для рейтингов
- `ShareService` - генерация изображений для шаринга

#### API Endpoints:
```
GET    /api/questions/{id}/comments   - комментарии к вопросу
POST   /api/questions/{id}/comments   - создать комментарий
PUT    /api/comments/{id}             - обновить комментарий
DELETE /api/comments/{id}             - удалить комментарий
POST   /api/comments/{id}/like        - лайкнуть комментарий
GET    /api/questions/{id}/rating     - рейтинг вопроса
POST   /api/questions/{id}/rating     - оценить вопрос
GET    /api/user/{id}/public          - публичный профиль
POST   /api/share/session/{id}        - генерация изображения для шаринга
```

---

### 11. Модуль персонализации (Personalization Module)

#### Классы:
- `NoteController` - API endpoints для заметок
- `BookmarkController` - API endpoints для закладок
- `AdaptiveLearningService` - адаптивное обучение
- `PersonalizationService` - персонализация рекомендаций

#### API Endpoints:
```
GET    /api/user/notes                - заметки пользователя
POST   /api/user/notes                - создать заметку
PUT    /api/user/notes/{id}           - обновить заметку
DELETE /api/user/notes/{id}           - удалить заметку
GET    /api/user/bookmarks             - закладки пользователя
POST   /api/user/bookmarks             - добавить закладку
DELETE /api/user/bookmarks/{id}       - удалить закладку
GET    /api/questions/personalized    - персонализированные вопросы
GET    /api/user/learning-plan        - план обучения
```

---

### 12. Модуль отчетов (Reports Module)

#### Классы:
- `ReportService` - генерация отчетов
- `ExportService` - экспорт данных
- `ReportController` - API endpoints
- `ComparisonService` - сравнение с другими

#### API Endpoints:
```
GET    /api/reports/weekly            - еженедельный отчет
GET    /api/reports/monthly           - месячный отчет
GET    /api/export/statistics/pdf     - экспорт статистики в PDF
GET    /api/export/statistics/excel   - экспорт статистики в Excel
GET    /api/export/history            - экспорт истории
GET    /api/user/compare              - сравнение с другими
```

---

### 13. Модуль уведомлений (Notifications Module)

#### Классы:
- `NotificationService` - отправка уведомлений
- `NotificationController` - API endpoints
- `NotificationPreferencesService` - настройки уведомлений

#### API Endpoints:
```
GET    /api/notifications             - список уведомлений
POST   /api/notifications/{id}/read  - отметить как прочитанное
GET    /api/user/notification-preferences - настройки уведомлений
PUT    /api/user/notification-preferences - обновить настройки
```

---

### 14. Модуль сертификатов (Certificates Module)

#### Классы:
- `CertificateService` - генерация сертификатов
- `CertificateController` - API endpoints
- `CertificateVerificationService` - верификация сертификатов

#### API Endpoints:
```
GET    /api/user/certificates         - сертификаты пользователя
GET    /api/certificates/{id}         - детали сертификата
GET    /api/certificates/{id}/pdf     - скачать PDF
GET    /api/certificates/verify/{id}  - верификация сертификата
```

---

### 15. Модуль команд и соревнований (Teams & Competitions Module)

#### Классы:
- `TeamController` - API endpoints для команд
- `TeamService` - бизнес-логика команд
- `CompetitionController` - API endpoints для соревнований
- `CompetitionService` - бизнес-логика соревнований

#### API Endpoints:
```
GET    /api/teams                     - список команд
POST   /api/teams                     - создать команду
GET    /api/teams/{id}                - детали команды
POST   /api/teams/{id}/invite         - пригласить участника
GET    /api/competitions               - список соревнований
POST   /api/competitions/{id}/join    - присоединиться к соревнованию
GET    /api/competitions/{id}/leaderboard - лидерборд соревнования
```

---

### 16. Модуль AI (AI Module)

#### Классы:
- `AIExplanationService` - генерация объяснений
- `AIQuestionGeneratorService` - генерация вопросов
- `AIChatService` - чат-бот помощник
- `AIController` - API endpoints

#### API Endpoints:
```
POST   /api/ai/explain                - получить объяснение
POST   /api/ai/generate-question      - сгенерировать вопрос
POST   /api/ai/chat                   - чат с AI помощником
GET    /api/ai/chat/history           - история чата
```

---

### 17. Модуль видео (Video Module)

#### Классы:
- `VideoController` - API endpoints
- `VideoService` - работа с видео
- `VideoEmbedService` - встраивание видео

#### API Endpoints:
```
GET    /api/questions/{id}/videos     - видео к вопросу
POST   /api/questions/{id}/videos     - добавить видео
DELETE /api/videos/{id}               - удалить видео
```

---

### 18. Модуль рефералов (Referral Module)

#### Классы:
- `ReferralService` - работа с рефералами
- `ReferralController` - API endpoints

#### API Endpoints:
```
GET    /api/user/referral-code        - реферальный код
GET    /api/user/referrals            - список рефералов
GET    /api/user/referral-stats       - статистика рефералов
POST   /api/referrals/apply           - применить реферальный код
```

---

## Безопасность

### 1. Санитизация пользовательского кода

```php
class CodeSanitizer
{
    private const DANGEROUS_FUNCTIONS = [
        'exec', 'shell_exec', 'system', 'passthru',
        'proc_open', 'popen', 'pcntl_exec', 'eval',
        'create_function', 'file_get_contents', 'file_put_contents',
        'fopen', 'fwrite', 'fread', 'unlink', 'rmdir',
        'mkdir', 'chmod', 'chown', 'symlink', 'link',
        'curl_exec', 'curl_multi_exec', 'socket_create',
        'fsockopen', 'pfsockopen', 'stream_socket_client',
        'mail', 'mb_send_mail', 'imap_open',
        'ldap_connect', 'pg_connect', 'mysql_connect',
        'mysqli_connect', 'sqlite_open', 'xmlrpc_server_create_method'
    ];
    
    private const DANGEROUS_KEYWORDS = [
        'include', 'require', 'include_once', 'require_once',
        'file_get_contents', 'file_put_contents', 'fopen',
        'readfile', 'readdir', 'scandir', 'opendir'
    ];
    
    public function sanitize(string $code): string
    {
        // Проверка на опасные функции
        foreach (self::DANGEROUS_FUNCTIONS as $func) {
            if (preg_match('/\b' . preg_quote($func) . '\s*\(/i', $code)) {
                throw new SecurityException("Использование функции {$func} запрещено");
            }
        }
        
        // Проверка на опасные ключевые слова
        foreach (self::DANGEROUS_KEYWORDS as $keyword) {
            if (preg_match('/\b' . preg_quote($keyword) . '\b/i', $code)) {
                throw new SecurityException("Использование {$keyword} запрещено");
            }
        }
        
        // Проверка синтаксиса PHP
        $this->validateSyntax($code);
        
        return $code;
    }
    
    private function validateSyntax(string $code): void
    {
        $tmpFile = tmpfile();
        fwrite($tmpFile, $code);
        $tmpPath = stream_get_meta_data($tmpFile)['uri'];
        
        exec("php -l {$tmpPath} 2>&1", $output, $returnCode);
        fclose($tmpFile);
        
        if ($returnCode !== 0) {
            throw new SyntaxException("Синтаксическая ошибка в коде");
        }
    }
}
```

### 2. Docker контейнер с ограничениями

```dockerfile
FROM php:8.2-cli

# Установка необходимых расширений
RUN apt-get update && apt-get install -y \
    && docker-php-ext-install pdo pdo_mysql \
    && rm -rf /var/lib/apt/lists/*

# Создание непривилегированного пользователя
RUN useradd -m -u 1000 sandboxuser

# Рабочая директория
WORKDIR /sandbox

# Переключение на непривилегированного пользователя
USER sandboxuser

# Ограничения PHP
ENV PHP_MEMORY_LIMIT=128M
ENV PHP_MAX_EXECUTION_TIME=5
```

```php
class DockerSandbox
{
    private const MEMORY_LIMIT = '128m';
    private const CPU_LIMIT = '0.5';
    private const TIME_LIMIT = 5; // секунды
    private const MAX_OUTPUT_SIZE = 1024 * 1024; // 1MB
    
    public function runCode(string $code, array $testCases): ExecutionResult
    {
        $sessionId = uniqid('sandbox_', true);
        $containerName = "php_sandbox_{$sessionId}";
        
        try {
            // Создание контейнера с ограничениями
            $this->createContainer($containerName, $code, $testCases);
            
            // Запуск с таймаутом
            $result = $this->executeWithTimeout($containerName, self::TIME_LIMIT);
            
            return $result;
        } finally {
            // Обязательная очистка
            $this->cleanupContainer($containerName);
        }
    }
    
    private function createContainer(string $name, string $code, array $testCases): void
    {
        $dockerOptions = [
            '--memory=' . self::MEMORY_LIMIT,
            '--memory-swap=' . self::MEMORY_LIMIT,
            '--cpus=' . self::CPU_LIMIT,
            '--network=none', // изоляция сети
            '--read-only', // только чтение файловой системы
            '--user=1000:1000', // непривилегированный пользователь
            '--rm', // автоматическое удаление
            '--tmpfs=/tmp:rw,noexec,nosuid,size=64m', // временная файловая система
        ];
        
        $optionsString = implode(' ', $dockerOptions);
        $command = "docker run -d --name {$name} {$optionsString} php-sandbox:latest";
        
        exec($command, $output, $returnCode);
        
        if ($returnCode !== 0) {
            throw new DockerException("Не удалось создать контейнер");
        }
        
        // Копирование кода в контейнер
        $this->copyCodeToContainer($name, $code, $testCases);
    }
    
    private function executeWithTimeout(string $containerName, int $timeout): ExecutionResult
    {
        $command = "timeout {$timeout} docker exec {$containerName} php /sandbox/test.php";
        
        $startTime = microtime(true);
        exec($command . ' 2>&1', $output, $returnCode);
        $executionTime = microtime(true) - $startTime;
        
        $outputString = implode("\n", $output);
        
        // Ограничение размера вывода
        if (strlen($outputString) > self::MAX_OUTPUT_SIZE) {
            $outputString = substr($outputString, 0, self::MAX_OUTPUT_SIZE) . '... (вывод обрезан)';
        }
        
        return new ExecutionResult(
            $returnCode === 0,
            $outputString,
            $executionTime,
            $returnCode
        );
    }
    
    private function cleanupContainer(string $containerName): void
    {
        exec("docker stop {$containerName} 2>/dev/null");
        exec("docker rm {$containerName} 2>/dev/null");
    }
}
```

### 3. Rate Limiting

```php
// В Laravel
Route::middleware(['throttle:10,1'])->group(function () {
    Route::post('/api/questions/{id}/check', [QuestionController::class, 'checkAnswer']);
});

// Или кастомный middleware
class CodeExecutionRateLimit
{
    public function handle($request, Closure $next)
    {
        $key = 'code_execution:' . $request->ip();
        $maxAttempts = 10;
        $decayMinutes = 1;
        
        if (RateLimiter::tooManyAttempts($key, $maxAttempts)) {
            return response()->json([
                'error' => 'Слишком много запросов. Попробуйте позже.'
            ], 429);
        }
        
        RateLimiter::hit($key, $decayMinutes * 60);
        
        return $next($request);
    }
}
```

---

## План тестирования

### 1. Unit тесты

#### Тесты для моделей:
- Создание вопроса
- Валидация данных
- Связи между моделями

#### Тесты для сервисов:
- Проверка теоретических ответов
- Проверка анализа кода
- Генерация рекомендаций
- Расчет статистики

#### Тесты для безопасности:
- Санитизация кода
- Блокировка опасных функций
- Валидация синтаксиса

### 2. Integration тесты

- Полный цикл проверки ответа
- Работа с Docker песочницей
- Сохранение статистики
- Создание сессии собеседования

### 3. E2E тесты

- Прохождение вопроса
- Режим собеседования
- Просмотр статистики
- Получение рекомендаций

### 4. Нагрузочное тестирование

- Одновременные запросы на проверку кода
- Нагрузка на Docker контейнеры
- Производительность базы данных

---

## Развертывание

### 1. Production окружение

#### Требования:
- Сервер с минимум 4GB RAM
- 2 CPU cores
- 50GB дискового пространства
- Docker и Docker Compose

#### Структура:
```
/production
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── php/
│   └── Dockerfile
├── .env.production
└── logs/
```

### 2. Docker Compose для продакшена

```yaml
version: '3.8'

services:
  app:
    build: ./php
    volumes:
      - ./:/var/www/html
    networks:
      - app-network
    
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./:/var/www/html
    networks:
      - app-network
    depends_on:
      - app
    
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: ${DB_DATABASE}
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - app-network
    
  redis:
    image: redis:alpine
    networks:
      - app-network
    
  docker-sandbox:
    # Отдельный сервис для Docker-in-Docker или использование Docker socket

volumes:
  mysql-data:

networks:
  app-network:
    driver: bridge
```

### 3. CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: phpunit
        
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to server
        run: |
          # Команды деплоя
```

---

## Приоритизация задач

### MVP (Минимальный рабочий продукт) - 6-7 недель

1. Базовая структура проекта ~85%** (обновлено 25.12.2025)
   - Docker, Laravel 12, Nginx, MySQL, Redis настроены
   - SSL сертификаты (mkcert) настроены
   - Все миграции созданы и выполнены (15 таблиц: users, programming_languages, frameworks, plans, subscriptions, payments, social_providers, question_categories, questions, question_options, test_cases, user_sessions, user_answers, user_progress, user_technology_preferences)
   - Все модели Eloquent созданы с отношениями и fillable полями (14 моделей)
   - Filament 4.3.1 установлен и настроен (админ-панель доступна на /admin)
   - API роуты созданы (routes/api.php с версионированием v1)
   - ⏳ Контроллеры для API - в процессе
   - ⏳ Filament Resources для управления данными - в процессе
2. ⏳ Аутентификация и регистрация (базовая)НЕ НАЧАТО**
3. Управление языками и технологиями (минимум 3 языка: PHP, JavaScript, Python)
4. 50+ вопросов (теория + анализ кода) по разным языкам
5. Базовый интерфейс с фильтрацией по технологиям
6. Проверка теоретических ответов
7. Проверка анализа кода
8. Базовая статистика (по языкам)
9. Личный кабинет пользователя (с выбором технологий)
10. Тарифные планы (Free + 1 платный)
11. Docker песочница (минимум для PHP, JavaScript, Python)

### Версия 1.0 - 12 недель

Все задачи из MVP +
1. OAuth аутентификация (Google, GitHub)
2. Поддержка 5+ языков (PHP, JavaScript, Python, Java, Go)
3. Поддержка основных фреймворков (Laravel, React, Vue, Django, Spring)
4. 200+ вопросов по разным языкам и технологиям
5. Практические задания для всех поддерживаемых языков
6. Полнофункциональная мультиязычная Docker песочница
7. Режим собеседования (с выбором технологии)
8. Система рекомендаций (по технологиям)
9. Улучшенный интерфейс с фильтрацией
10. Полная система тарифов (Free, Basic, Premium)
11. Интеграция платежной системы
12. Расширенная статистика в личном кабинете (по языкам и технологиям)

### Версия 2.0 - 18 недель

Все задачи из версии 1.0 +
1. Все OAuth провайдеры (Yandex, VK)
2. Геймификация (достижения, уровни, рейтинги) - Этап 12
3. Социальные функции (комментарии, рейтинги вопросов) - Этап 13
4. Персонализация (заметки, закладки, адаптивное обучение) - Этап 14
5. Расширенная аналитика и отчеты - Этап 15
6. Уведомления (email, push) - Этап 16
7. Сертификаты - Этап 17
8. Реферальная программа - Этап 21
9. PWA и офлайн режим - Этап 22

### Версия 3.0 - 24 недели

Все задачи из версии 2.0 +
1. Командные соревнования - Этап 18
2. AI функции (объяснения, генерация вопросов, чат-бот) - Этап 19
3. Видео и мультимедиа - Этап 20
4. Мобильное приложение (нативное, опционально)

### Версия 4.0 - 28+ недель

Все задачи из версии 3.0 +
1. Расширенные типы вопросов
2. Интеграции (GitHub, LinkedIn)
3. Микро-обучение
4. Форум сообщества
5. Партнерства с компаниями

---

## Метрики успеха

### Технические метрики:
- Время ответа API < 200ms
- Время выполнения кода в песочнице < 5 секунд
- Доступность > 99.5%
- Покрытие тестами > 80%

### Пользовательские метрики:
- Количество активных пользователей
- Среднее время на платформе
- Процент завершенных сессий
- Удовлетворенность пользователей

---

## Риски и митигация

### Риск 1: Безопасность Docker песочницы
**Митигация**: 
- Тщательное тестирование безопасности
- Использование проверенных практик
- Регулярные аудиты безопасности
- Мониторинг выполнения кода

### Риск 2: Производительность при нагрузке
**Митигация**:
- Кэширование
- Оптимизация запросов
- Горизонтальное масштабирование
- Мониторинг производительности

### Риск 3: Качество контента
**Митигация**:
- Рецензирование вопросов
- Тестирование на реальных пользователях
- Регулярное обновление контента
- Обратная связь от пользователей

---

## 🚀 Рекомендации по улучшению проекта

> Подробный документ с рекомендациями: [improvements.md](improvements.md)

### Особенности мультиязычного тренажера:

1. **Масштабируемость**
   - Легкое добавление новых языков и фреймворков
   - Единая архитектура для всех языков
   - Переиспользование компонентов

2. **Персонализация**
   - Выбор основных технологий в профиле
   - Рекомендации на основе выбранных технологий
   - Статистика по каждой технологии отдельно

3. **Гибкость**
   - Фильтрация вопросов по языкам/фреймворкам
   - Смешанные вопросы (несколько технологий)
   - Универсальные категории (алгоритмы, структуры данных)

### Топ-5 приоритетных улучшений:

1. **Геймификация** ⭐⭐⭐⭐⭐
   - Система достижений и бейджей
   - Рейтинговая система и лидерборды
   - Очки и уровни пользователей
   - **Время реализации**: +2 недели

2. **Персонализация обучения** ⭐⭐⭐⭐⭐
   - Адаптивный подбор вопросов
   - Система заметок и закладок
   - Персонализированные рекомендации
   - **Время реализации**: +1 неделя

3. **Социальные функции** ⭐⭐⭐⭐
   - Комментарии к вопросам
   - Рейтинг вопросов
   - Обмен результатами
   - **Время реализации**: +2 недели

4. **Расширенная аналитика** ⭐⭐⭐⭐
   - Детальные отчеты (PDF, Excel)
   - Еженедельные email-отчеты
   - Сравнение с другими пользователями
   - **Время реализации**: +1 неделя

5. **AI функции** ⭐⭐⭐⭐⭐
   - AI-объяснения неправильных ответов
   - Генерация вопросов
   - Чат-помощник
   - **Время реализации**: +3 недели

### Дополнительные улучшения:

- **Уведомления и напоминания** - поддержка регулярности занятий
- **Сертификаты** - мотивация и использование в резюме
- **Командные соревнования** - дополнительная мотивация
- **Видео-объяснения** - разнообразие форматов
- **Мобильное приложение** - увеличение доступности
- **Реферальная программа** - рост пользовательской базы
- **Расширенные типы вопросов** - улучшение качества обучения
- **Интеграции** (GitHub, LinkedIn) - расширение функциональности

**Полный список с деталями реализации**: см. [improvements.md](improvements.md)

---

## Следующие шаги

1. **Выбор технологий**: Определить Laravel или Symfony (Vue.js уже выбран)
2. **Настройка окружения**: Создать Docker Compose конфигурацию
3. **Создание базы данных**: Выполнить миграции
4. **Первые 20 вопросов**: Собрать и добавить начальный контент
5. **Прототип интерфейса**: Создать базовый UI для одного вопроса
6. **Тестирование концепции**: Протестировать на небольшой группе пользователей

---

## Ресурсы и ссылки

### Документация:
- [Laravel Documentation](https://laravel.com/docs)
- [Vue.js Documentation](https://vuejs.org/)
- [Docker Documentation](https://docs.docker.com/)
- [PHP The Right Way](https://phptherightway.com/)

### Источники вопросов:
- PHP Interview Questions на GitHub
- Stack Overflow PHP tag
- Real-world interview experiences
- PHP FIG Standards (PSR)

### Инструменты:
- [PHPUnit](https://phpunit.de/)
- [PHP CS Fixer](https://cs.symfony.com/)
- [Monaco Editor](https://microsoft.github.io/monaco-editor/)
- [Chart.js](https://www.chartjs.org/)

---

**Дата создания плана**: 2024
**Версия плана**: 1.0
**Статус**: В процессе реализации

---

## 📊 Текущий прогресс (обновлено 25.12.2025)

### Выполнено:
1. **Этап 1: Подготовка и настройка проекта** ПОЛНОСТЬЮ ЗАВЕРШЕН**
   - Laravel 12 установлен и настроен
   - Все миграции созданы и выполнены (16 таблиц, включая обновление users)
   - Все модели Eloquent созданы с отношениями (14 моделей)
   - Filament 4.3.1 установлен, настроен и админ-пользователь создан
   - API роуты созданы (routes/api.php с версионированием v1)
   - Git репозиторий инициализирован
   - Laravel Pint настроен (pint.json)
   - CI/CD pipeline настроен (GitHub Actions)
   - L5-Swagger установлен и настроен для API документации

2. **База данных**
   - Таблицы: users (с полями role, avatar, subscription_expires_at, trial_ends_at, is_active, last_login_at)
   - Таблицы: programming_languages, frameworks, plans, subscriptions, payments, social_providers
   - Таблицы: question_categories, questions, question_options, test_cases
   - Таблицы: user_sessions, user_answers, user_progress, user_technology_preferences
   - Таблицы: personal_access_tokens (Sanctum)

3. **Модели**
   - User, ProgrammingLanguage, Framework, Plan, Subscription, Payment, SocialProvider
   - Question, QuestionCategory, QuestionOption, TestCase
   - UserSession, UserAnswer, UserProgress, UserTechnologyPreference

4. **Админ-панель Filament**
   - Filament 4.3.1 установлен
   - Админ-панель настроена на /admin
   - Брендинг "Trener Admin" с цветом Blue
   - Админ-пользователь создан: admin@klassev.test (пароль: admin123)
   - PlanResource и SubscriptionResource созданы

5. **Этап 1.5: Аутентификация, регистрация и тарифные планы**    - Laravel Sanctum 4.2.1 установлен и настроен
   - Laravel Socialite 5.24.0 установлен
   - Контроллеры аутентификации: RegisterController, LoginController, LogoutController, PasswordResetController
   - Email верификация: EmailVerificationController, MustVerifyEmail в User модели
   - OAuth аутентификация: SocialAuthController с поддержкой Google, GitHub, Yandex, VK
   - Тарифные планы: PlanController для получения планов
   - Логика доступа: методы hasAccessTo(), canAnswerQuestions(), isOnTrial() в User модели
   - Middleware: CheckSubscription для проверки подписки
   - Личный кабинет: ProfileController, SocialAccountController, SubscriptionController
   - Request классы: RegisterRequest, LoginRequest, UpdateProfileRequest, ChangePasswordRequest
   - API роуты: все endpoints для аутентификации, профиля, подписок настроены

6. **Этап 2: Управление языками, фреймворками и технологиями**    - Контроллеры: ProgrammingLanguageController, FrameworkController с полным CRUD
   - Контроллер категорий: QuestionCategoryController с поддержкой иерархии и методом tree()
   - Контроллер предпочтений: TechnologyPreferenceController для управления предпочтениями пользователей
   - Filament Resources: ProgrammingLanguageResource, FrameworkResource созданы
   - Сидеры: ProgrammingLanguageSeeder (8 языков), FrameworkSeeder (15+ фреймворков)
   - Иерархия категорий: методы isRoot(), full_path, level, ancestors, descendants, tree() в QuestionCategory
   - Методы User модели: primaryTechnologies(), languagePreferences(), frameworkPreferences()
   - API роуты: все endpoints для языков, фреймворков, категорий и предпочтений настроены

7. **Этап 2.5: Модуль вопросов и категорий**    - Контроллер вопросов: QuestionController с полным CRUD, фильтрацией и пагинацией
   - Контроллер вариантов ответов: QuestionOptionController с полным CRUD
   - Контроллер тест-кейсов: TestCaseController с полным CRUD
   - Request классы: StoreQuestionRequest, UpdateQuestionRequest с полной валидацией
   - Filament Resources: QuestionResource, QuestionCategoryResource созданы
   - API роуты: все endpoints для вопросов, вариантов ответов и тест-кейсов настроены
   - Поддержка создания вариантов и тест-кейсов при создании вопроса

8. **Этап 3: Базовый интерфейс пользователя (Бэкенд API)**    - Контроллер сессий: SessionController для создания и управления сессиями тренировок
   - Контроллер ответов: AnswerController для обработки ответов пользователей с проверкой
   - Контроллер прогресса: ProgressController для отслеживания прогресса по категориям и языкам
   - Контроллер статистики: StatisticsController с детальной статистикой и рекомендациями
   - Проверка доступа: проверка тарифных планов и лимитов в SessionController
   - Обновление прогресса: автоматическое обновление при завершении сессии
   - Методы моделей: скоупы и helper методы в UserSession, UserAnswer, UserProgress
   - Gate для админа: создан Gate 'admin' в AppServiceProvider
   - Улучшенная логика лимитов: методы canAnswerQuestions() и getQuestionsRemainingToday() в User
   - API роуты: все endpoints для сессий, ответов, прогресса и статистики настроены

9. **Этап 3: Базовый интерфейс пользователя (Фронтенд Vue.js)**    - Vue.js 3 установлен и настроен с Vite
   - Vue Router настроен с защитой роутов и navigation guards
   - Pinia store для аутентификации создан с методами login, register, logout
   - Axios настроен с интерцепторами для токенов и обработки ошибок
   - Компоненты: NavBar, Layout созданы
   - Страницы аутентификации: Login, Register, ForgotPassword созданы
   - Главная страница: Home.vue с фильтрами, списком вопросов и популярных технологий
   - Страница вопроса: Question/Show.vue с поддержкой всех типов вопросов
   - Страница сессии: Session/Show.vue для прохождения тренировок
   - Личный кабинет: Profile/Index.vue с редактированием профиля
   - Статистика: Statistics/Index.vue с детальной статистикой
   - Тарифные планы: Plans/Index.vue для просмотра планов
   - Blade шаблон: app.blade.php для SPA
   - Web роуты: настроены для SPA режима

### ⏳ В процессе:
- Нет активных задач

### 📋 Следующие шаги:
1. Улучшить фронтенд: добавить подсветку синтаксиса кода (highlight.js), расширить функционал профиля **ВЫПОЛНЕНО 2. Установить SDK платежных систем (YooKassa, Stripe) и создать PaymentController
3. Создать тесты для аутентификации, OAuth, тарифов, сессий, ответов, прогресса и статистики
4. Настроить Filament Resources (формы, таблицы, фильтры) для языков, фреймворков и категорий **ВЫПОЛНЕНО 5. Настроить Repeater для вариантов ответов и тест-кейсов в QuestionResource **ВЫПОЛНЕНО 6. Настроить Docker образы для выполнения кода на разных языках (этап 4)ВЫПОЛНЕНО 7. Расширить UI профиля: смена пароля, управление соц. аккаунтами, выбор технологий

