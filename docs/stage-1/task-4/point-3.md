# Создание админ-пользователя

## Что нужно сделать

Я создаю админ-пользователя для доступа к админ-панели Filament. Создаю сидер или использую команду для создания пользователя с правами администратора.

## Почему это важно

Админ-пользователь необходим потому что:
- Доступ к админ-панели требует аутентификации
- Нужен пользователь с правами администратора
- Безопасность - только авторизованные пользователи могут управлять контентом
- Разделение прав доступа между обычными пользователями и администраторами

## Пошаговая инструкция

1. Проверяю структуру таблицы users:
   ```bash
   php artisan tinker
   ```
   В tinker:
   ```php
   Schema::getColumnListing('users');
   ```
   Проверяю, какие поля есть в таблице users.

2. Создаю сидер для админ-пользователя:
   ```bash
   php artisan make:seeder AdminUserSeeder
   ```

3. Настраиваю AdminUserSeeder:
   ```php
   <?php

   namespace Database\Seeders;

   use Illuminate\Database\Seeder;
   use Illuminate\Support\Facades\Hash;
   use App\Models\User;

   class AdminUserSeeder extends Seeder
   {
       public function run(): void
       {
           User::firstOrCreate(
               ['email' => 'admin@klassev.test'],
               [
                   'name' => 'Admin',
                   'email' => 'admin@klassev.test',
                   'password' => Hash::make('admin123'),
                   'email_verified_at' => now(),
               ]
           );
       }
   }
   ```

4. Обновляю DatabaseSeeder для вызова AdminUserSeeder:
   ```bash
   nano database/seeders/DatabaseSeeder.php
   ```
   Добавляю:
   ```php
   public function run(): void
   {
       $this->call([
           AdminUserSeeder::class,
       ]);
   }
   ```

5. Выполняю сидер:
   ```bash
   php artisan db:seed --class=AdminUserSeeder
   ```
   Или все сидеры:
   ```bash
   php artisan db:seed
   ```

6. Проверяю, что пользователь создан:
   ```bash
   php artisan tinker
   ```
   В tinker:
   ```php
   User::where('email', 'admin@klassev.test')->first();
   ```
   Должен вернуться объект пользователя.

7. Настраиваю права доступа для админ-пользователя:
   Если использую систему ролей, добавляю роль администратора. Или проверяю доступ через Filament policies.

8. Проверяю вход в админ-панель:
   Открываю https://klassev.test/admin
   Ввожу:
   - Email: admin@klassev.test
   - Password: admin123
   Должен произойти вход в админ-панель.

9. (Опционально) Создаю команду для создания админ-пользователя:
   ```bash
   php artisan make:command CreateAdminUser
   ```
   Это позволит создавать админ-пользователя через команду в будущем.

10. Настраиваю команду CreateAdminUser (если создал):
    ```php
    <?php

    namespace App\Console\Commands;

    use Illuminate\Console\Command;
    use App\Models\User;
    use Illuminate\Support\Facades\Hash;

    class CreateAdminUser extends Command
    {
        protected $signature = 'admin:create {email} {name} {password}';
        protected $description = 'Create an admin user';

        public function handle()
        {
            $user = User::create([
                'name' => $this->argument('name'),
                'email' => $this->argument('email'),
                'password' => Hash::make($this->argument('password')),
                'email_verified_at' => now(),
            ]);

            $this->info("Admin user created: {$user->email}");
        }
    }
    ```

## Что проверить после выполнения

- [ ] Админ-пользователь создан в базе данных
- [ ] Пароль захеширован (не хранится в открытом виде)
- [ ] Email верифицирован (email_verified_at установлен)
- [ ] Можно войти в админ-панель с этими данными
- [ ] Пользователь имеет доступ к админ-панели

## Безопасность

- Использую надежный пароль (не "admin123" в production)
- Храню пароль в безопасном месте (не в коде)
- Регулярно меняю пароль администратора
- Использую двухфакторную аутентификацию (если доступна в Filament)
- Ограничиваю доступ к админ-панели по IP (для production)

## Важные замечания

- Пароль должен быть захеширован через Hash::make()
- Email должен быть уникальным
- В production использую более сложный пароль
- Храню учетные данные в безопасном месте
- Не коммичу реальные пароли в репозиторий

## Следующий шаг

После создания админ-пользователя настраиваю тему и брендинг админ-панели (следующий пункт).

