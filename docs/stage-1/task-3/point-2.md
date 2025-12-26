# Создание моделей Eloquent

## Что нужно сделать

Я создаю модели Eloquent для всех сущностей проекта, настраиваю отношения между моделями, fillable поля, скоупы и другие методы.

## Почему модели важны

Модели Eloquent необходимы потому что:
- Абстракция работы с базой данных
- Упрощение запросов к БД
- Автоматическая работа с отношениями
- Валидация и бизнес-логика
- Типобезопасность через IDE
- Соответствие паттерну Active Record

## Пошаговая инструкция

1. Создаю модель User (если еще не создана):
   ```bash
   php artisan make:model User
   ```
   Laravel может уже иметь базовую модель User, проверяю это.

2. Создаю модель Plan:
   ```bash
   php artisan make:model Plan
   ```

3. Создаю модель Subscription:
   ```bash
   php artisan make:model Subscription
   ```

4. Создаю модель Payment:
   ```bash
   php artisan make:model Payment
   ```

5. Создаю модель ProgrammingLanguage:
   ```bash
   php artisan make:model ProgrammingLanguage
   ```

6. Создаю модель Framework:
   ```bash
   php artisan make:model Framework
   ```

7. Создаю модель QuestionCategory:
   ```bash
   php artisan make:model QuestionCategory
   ```

8. Создаю модель Question:
   ```bash
   php artisan make:model Question
   ```

9. Создаю модель QuestionOption:
   ```bash
   php artisan make:model QuestionOption
   ```

10. Создаю модель TestCase:
    ```bash
    php artisan make:model TestCase
    ```

11. Создаю модель SocialProvider:
    ```bash
    php artisan make:model SocialProvider
    ```

12. Создаю модель UserTechnologyPreference:
    ```bash
    php artisan make:model UserTechnologyPreference
    ```

13. Настраиваю каждую модель:
    Открываю каждую модель и настраиваю:
    - `$fillable` или `$guarded` для массового присвоения
    - Отношения (relationships) с другими моделями
    - Касты типов (casts)
    - Скоупы (scopes) для часто используемых запросов

14. Пример настройки модели Plan:
    ```php
    class Plan extends Model
    {
        protected $fillable = [
            'name',
            'slug',
            'description',
            'price',
            'interval',
            'trial_days',
            'features',
            'is_active',
            'sort_order',
        ];

        protected $casts = [
            'price' => 'decimal:2',
            'trial_days' => 'integer',
            'features' => 'array',
            'is_active' => 'boolean',
            'sort_order' => 'integer',
        ];

        public function subscriptions()
        {
            return $this->hasMany(Subscription::class);
        }
    }
    ```

15. Настраиваю отношения в модели User:
    ```php
    public function subscriptions()
    {
        return $this->hasMany(Subscription::class);
    }

    public function currentSubscription()
    {
        return $this->hasOne(Subscription::class)
            ->where('status', 'active')
            ->latest();
    }

    public function socialProviders()
    {
        return $this->hasMany(SocialProvider::class);
    }

    public function technologyPreferences()
    {
        return $this->hasMany(UserTechnologyPreference::class);
    }
    ```

16. Настраиваю отношения в модели Question:
    ```php
    public function category()
    {
        return $this->belongsTo(QuestionCategory::class);
    }

    public function language()
    {
        return $this->belongsTo(ProgrammingLanguage::class);
    }

    public function framework()
    {
        return $this->belongsTo(Framework::class);
    }

    public function options()
    {
        return $this->hasMany(QuestionOption::class);
    }

    public function testCases()
    {
        return $this->hasMany(TestCase::class);
    }
    ```

17. Добавляю скоупы для часто используемых запросов:
    ```php
    // В модели Question
    public function scopeActive($query)
    {
        return $query->where('is_active', true);
    }

    public function scopeByLanguage($query, $languageId)
    {
        return $query->where('programming_language_id', $languageId);
    }

    public function scopeByDifficulty($query, $difficulty)
    {
        return $query->where('difficulty', $difficulty);
    }
    ```

18. Проверяю модели через tinker:
    ```bash
    php artisan tinker
    ```
    Тестирую:
    ```php
    Plan::all();
    Question::with('category', 'language')->first();
    User::with('currentSubscription')->first();
    ```

## Что проверить после выполнения

- [ ] Все модели созданы
- [ ] Fillable/guarded поля настроены
- [ ] Отношения между моделями настроены
- [ ] Касты типов настроены (для JSON, dates и т.д.)
- [ ] Скоупы работают корректно
- [ ] Модели можно использовать в tinker без ошибок

## Важные замечания

- Использую `$fillable` для безопасности (защита от массового присвоения)
- Настраиваю отношения в обе стороны (belongsTo и hasMany/hasOne)
- Использую касты для JSON полей и дат
- Добавляю скоупы для часто используемых фильтров
- Проверяю работу отношений через tinker

## Дополнительные методы

Могу добавить в модели:
- Accessors и mutators для форматирования данных
- Методы для бизнес-логики
- События модели (creating, created, updating и т.д.)
- Локальные скоупы для сложных запросов

