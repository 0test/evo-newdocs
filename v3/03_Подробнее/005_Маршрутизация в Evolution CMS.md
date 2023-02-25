# Маршрутизация в Evolution CMS

1. [Маршрутизация движка](#section1)
2. [Маршрутизация Laravel](#section2)

## Маршрутизация движка <a name="section1"></a>

Все маршруты к ресурсам на сайте управляются движком Evolution CMS.

За эту функциональность отвечают настройки из раздела `Дружественные URL` (Системная конфигурация -> Дружественные URL). Возможные настройки хорошо описаны в интерфейсе системы.

![Конфигурация](/assets/images/s16.png)

Каждый раз при сохранении ресурса сбрасывается кэш и перестраивается "дерево" сайта.

Настройка кэша AliasListing отвечает за хранение кэша алиасов ресурсов, что позволяет уменьшить количество запросов в базу при создании страниц. Есть три варианта настройки:

- хранить в кеше алиасы всех ресурсов - подходит для небольших сайтов и сайтов среднего размера (примерно до 10 тыс ресурсов)
- хранить в кеше алиасы только для папок (подходит для огромных сайтов)
- не хранить в кеше алиасы

Для огромных сайтов рекомендуется включать хранение AliasListing только для папок, так как иначе кеш становится таким огромным, что не помещается в оперативную память.

Поле "псевдоним" задаётся при создании ресурса. Можно не заполнять это поле - система сама сделает валидный адрес.

![Alias](/assets/images/s15.png)

## Маршрутизация Laravel <a name="section2"></a>

Использование маршрутизации Laravel позволяет создавать свои ссылки, для которых не было создано ресурсов в движке, например для реализации backend.

Также это может пригодиться для обработки форм, вывода данных из своих таблиц и/или моделей.

### Маршруты по-умолчанию

В файл с маршрутами `core/custom/routes.php` пишем свои маршруты используя фасад `use Illuminate\Support\Facades\Route;`

```php
<?php
use EvolutionCMS\Main\Controllers\ArticleController;
use Illuminate\Support\Facades\Route;

Route::get('/articles', [ArticleController::class, 'index'] );
Route::get('/articles/{id}', [ArticleController::class, 'show'] );
```

Если файл маршрутов становится очень большим, то можно его разделить на логические блоки и подключать их в основной файл.

В основной файл с машрутами `core/custom/routes.php` дописать:

```php
foreach (File::allFiles(__DIR__ . '/routes') as $route_file) {
    require $route_file->getPathname();
}
```

Например, добавим отдельный файл с маршрутами `core/custom/routes/news.php`:

```php
<?php
use EvolutionCMS\Main\Controllers\NewsController;
use Illuminate\Support\Facades\Route;

Route::get('/news', [NewsController::class, 'index'] );
Route::get('/news/{id}', [NewsController::class, 'show'] );
```

Также доступны все другие параметры маршрутизации из Laravel.

### Роуты в своём пакете

Создать файл `core/custom/packages/NAME/routes.php`:

```php
use Illuminate\Support\Facades\Route;

Route::get('test/{id}', [ArticleController::class, 'show']);
```

В сервис-провайдере вашего пакета загрузить их, задав middleware.

`core/custom/packages/Name/src/NameServiceProvider.php`

```php
Route::group(['middleware' => 'bindings'], function(){
    $this->loadRoutesFrom(__DIR__ . '/../routes.php');
});

```

Либо задать middleware сразу в файле routes.php

```php
Route::get('test/{id}', [ArticleController::class, 'show'])->middleware('bindings');
```
