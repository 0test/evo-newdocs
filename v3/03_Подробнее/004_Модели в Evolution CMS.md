# Модели в Evolution CMS #

Модель - это класс, который предоставляет удобные методы для получения данных из таблиц.

Предполагается, что вы уже прочитали и попробовали код из [вводного материала](/v3/02_%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D1%81%D0%B0%D0%B9%D1%82%D0%B0/06_%D0%9C%D0%BE%D0%B4%D0%B5%D0%BB%D0%B8.md) про модели.

1. [Пример](#section1)
2. [Наследование](#section2)

## Пример модели <a name="section1"></a> ##



**Модель для своей таблицы**

Вы можете создавать модели для любых таблиц, доступных Evolution CMS. Префикс указывать не нужно.

```php
<?php 
namespace EvolutionCMS\Main\Models;
use Illuminate\Database\Eloquent;
class Example extends Eloquent\Model{
    protected $table = 'example';
}
```
Пространство имён модели связано с её местоположением.
`EvolutionCMS\Main\Models` означает, что модель  находится в папке `core\custom\packages\main\src\Models\`. 

## Наследование <a name="section2"></a> ##

Вы можете наследовать методы и свойства любой другой модели, как созданной вами, так и уже имеющейся в составе Evolution CMS.

**Модель, наследующая  модель SiteContent:**

```php 
<?php 
namespace EvolutionCMS\Main\Models;

use EvolutionCMS\Models\SiteContent;
class Example extends SiteContent {
	protected $table = 'site_content';
}
```
## Получение моделей ##

Все методы Eloquent, которые возвращают более одного результата модели, будут возвращать экземпляры класса Illuminate\Database\Eloquent\Collection, включая результаты, полученные с помощью метода get или доступные через отношения.

Метод `all` получит все записи из связанной с моделью таблицы:

```php
<?php
use EvolutionCMS\Main\Models\News;
foreach (News::all() as $news) {
    echo $news->pagetitle;
}
```
Метод `find` для извлечения отдельных моделей:
```php
<?php
use EvolutionCMS\Main\Models\Article;

// Получить модель по ее первичному ключу
$article = Article::find(1);

// Получить первую модель, соответствующую условиям
$article = Article::where('active', 1)->first();

// Комбинируем
$article = App\Models\Article::firstWhere('active', 1);
```

## Связи ##

Модели могут быть связаны друг с другом разными способами. Динамические свойства позволяют получить доступ к методам отношений, как если бы они были свойствами, определенными в модели


### Один к одному (hasOne)

Например, модель User может быть связана с одной моделью Phone. Поместим метод phone в модель User.
```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Получить телефон, связанный с пользователем.
     */
    public function phone()
    {
        return $this->hasOne(Phone::class);
    }
}
```
Eloquent определяет внешний ключ отношения на основе имени родительской модели. В этом случае автоматически предполагается, что модель Phone имеет внешний ключ user_id.

Получим телефон:

```php
$phone = User::find(1)->phone;
```
### Обратная связь Один к одному (belongsTo) ###

```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Phone extends Model
{
    /**
     * Получить пользователя, владеющего телефоном.
     */
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```
Получим юзера:

```php
$user = Phone::find(1)->user;
```

### Один ко многим (hasMany)

Например, пост в блоге может содержать бесконечное количество комментариев

```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * Получить комментарии к посту блога.
     */
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
}
```
> Eloquent предполагает, что столбец внешнего ключа в модели Comment именуется post_id

Получим все комментарии к посту:

```php
use App\Models\Post;
$comments = Post::find(1)->comments;
foreach ($comments as $comment) {
    //
}
```


### Обратная связь Один ко многим (belongsTo)

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * Получить пост, которому принадлежит комментарий.
     */
    public function post()
    {
        return $this->belongsTo(Post::class);
    }
}
```
Найдём "родительский" пост для комментария

```php 	
<?php
use App\Models\Comment;
$comment = Comment::find(1);
return $comment->post;
```

### Отношения Многие ко многим
Пример: у пользователя много ролей, а у роли много пользователей.

## Агрегирование связанных моделей ##
 Подсчет связанных моделей
 Другие агрегатные функции
 Подсчет связанных моделей отношений Morph To
 ## Жадная загрузка (eager loading) ##

 При построении запроса вы можете указать, какие отношения должны быть загружены с помощью метода with:
```php

$books = Book::with('author')->get();
foreach ($books as $book) {
    echo $book->author->name;
}
``` 
### Жадная загрузка по-умолчанию
Иногда требуется постоянная загрузка некоторых отношений при извлечении модели. Для этого вы можете определить свойство `$with` в модели:
```php
<?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Book extends Model
{
    protected $with = ['author'];
    public function author()
    {
        return $this->belongsTo(Author::class);
    }
}
```


## Преобразование значений (аксессоры, мутаторы) ##

