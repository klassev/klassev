# Настройка темы и брендинга

## Что нужно сделать

Я настраиваю тему и брендинг админ-панели Filament: изменяю название, логотип, цвета, настраиваю внешний вид согласно бренду проекта.

## Почему это важно

Настройка темы и брендинга важна потому что:
- Уникальный внешний вид админ-панели
- Соответствие бренду проекта
- Профессиональный вид
- Улучшение пользовательского опыта
- Легкость идентификации проекта

## Пошаговая инструкция

1. Нахожу провайдер Filament:
   ```bash
   ls -la app/Providers/
   ```
   Обычно это `FilamentServiceProvider` или `AdminPanelProvider`.

2. Открываю провайдер для настройки:
   ```bash
   nano app/Providers/Filament/AdminPanelProvider.php
   ```
   Или нахожу соответствующий файл.

3. Настраиваю базовый брендинг в методе panel():
   ```php
   public function panel(Panel $panel): Panel
   {
       return $panel
           ->default()
           ->id('admin')
           ->path('admin')
           ->brandName('Trener Admin')
           ->brandLogo(asset('images/logo.svg'))
           ->brandLogoHeight('2rem')
           ->favicon(asset('images/favicon.ico'))
           ->colors([
               'primary' => Color::Blue,
           ])
           ->discoverResources(in: app_path('Filament/Resources'), for: 'App\\Filament\\Resources')
           ->discoverPages(in: app_path('Filament/Pages'), for: 'App\\Filament\\Pages')
           ->navigationGroups([
               'Content',
               'Users',
               'Settings',
           ]);
   }
   ```

4. Настраиваю цвета темы:
   ```php
   ->colors([
       'primary' => Color::Blue,
       'gray' => Color::Slate,
   ])
   ```
   Или использую кастомные цвета:
   ```php
   ->colors([
       'primary' => '#3B82F6',
       'danger' => '#EF4444',
       'success' => '#10B981',
       'warning' => '#F59E0B',
   ])
   ```

5. Настраиваю логотип:
   Создаю папку для изображений:
   ```bash
   mkdir -p public/images
   ```
   Размещаю логотип в `public/images/logo.svg` или `public/images/logo.png`.
   Обновляю путь в провайдере.

6. Настраю favicon:
   Размещаю favicon в `public/images/favicon.ico`.
   Обновляю путь в провайдере.

7. Настраиваю навигационные группы:
   ```php
   ->navigationGroups([
       'Content Management',
       'User Management',
       'System Settings',
   ])
   ```
   Это организует ресурсы в меню.

8. Настраиваю темную тему (опционально):
   ```php
   ->darkMode(true)
   ```
   Или позволяю пользователю переключать:
   ```php
   ->darkMode(false) // только светлая
   ->darkMode(true)  // только темная
   ->darkMode()      // переключение
   ```

9. Настраиваю дополнительные опции:
   ```php
   ->sidebarCollapsibleOnDesktop()
   ->maxContentWidth('full')
   ->navigation(false) // скрыть навигацию
   ```

10. Очищаю кэш:
    ```bash
    php artisan config:clear
    php artisan view:clear
    php artisan route:clear
    ```

11. Проверяю изменения:
    Открываю https://klassev.test/admin
    Проверяю, что название, логотип и цвета применились.

12. Настраиваю кастомные стили (если нужно):
    Создаю файл `resources/css/filament/admin/custom.css`:
    ```css
    /* Кастомные стили для админ-панели */
    ```
    Подключаю в провайдере:
    ```php
    ->viteTheme('resources/css/filament/admin/theme.css')
    ```

## Что проверить после выполнения

- [ ] Название админ-панели изменено
- [ ] Логотип отображается (если добавлен)
- [ ] Favicon отображается (если добавлен)
- [ ] Цвета темы применены
- [ ] Навигационные группы настроены
- [ ] Изменения видны в браузере

## Доступные цвета в Filament

Filament предоставляет предустановленные цвета:
- Color::Amber
- Color::Blue
- Color::Cyan
- Color::Emerald
- Color::Fuchsia
- Color::Gray
- Color::Green
- Color::Indigo
- Color::Orange
- Color::Pink
- Color::Purple
- Color::Red
- Color::Rose
- Color::Sky
- Color::Slate
- Color::Violet
- Color::Yellow
- Color::Zinc

## Кастомизация

Могу дополнительно настроить:
- Шрифты
- Размеры элементов
- Отступы и margins
- Анимации
- Иконки
- Язык интерфейса

## Важные замечания

- Логотип должен быть в формате SVG или PNG
- Размеры логотипа не должны быть слишком большими
- Цвета должны соответствовать бренду
- Проверяю отображение на разных устройствах
- Тестирую темную тему (если включена)

## Следующий шаг

После настройки темы проверяю доступ к админ-панели и тестирую функциональность (следующий пункт).

