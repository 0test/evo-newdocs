# Модели в Evolution CMS #

Модель - это класс, который предоставляет удобные методы для получения данных из таблиц.

Предполагается, что вы уже прочитали и попробовали код из [вводного материала](/v3/02_%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D1%81%D0%B0%D0%B9%D1%82%D0%B0/006_%D0%9C%D0%BE%D0%B4%D0%B5%D0%BB%D0%B8.md) про модели.

1. [Пример](#section1)
2. [Наследование](#section2)
3. [Получение моделей](#section3)
4. [Связи](#section4)
5. [Жадная загрузка](#section5)
6. [Преобразование значений ](#section6)
7. [Пример мутатора для изменения размеров изображений](#section7)



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
## Получение моделей <a name="section3"></a> ##

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
// Используем id документа
// Заодно получим ТВ-параметр news_image

$id =  $this->evo->documentObject['id'];
$result = News::
withTVs(['news_image'])
->find($id)
;
$this->data['item'] = $result;
return $this->data['item'];

// Получить первую модель, соответствующую условиям
$article = Article::where('active', 1)->first();

// Комбинируем
$article = App\Models\Article::firstWhere('active', 1);
```

## Связи <a name="section4"></a> ##

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

## Жадная загрузка (eager loading) <a name="section5"></a> ##

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


## Преобразование значений (аксессоры, мутаторы) <a name="section6"></a> ##


### Аксессор  ###

Аксессор преобразует значение атрибутов модели.

Пример
> Колонка published в базе данных отдаёт цифровое значение: 1 для опубликованного документа и 0 для неопубликованного. А в разметке на сайте вы хотите ставить css-классы `news-pub` и `news-unpub`.

Конечно, можно использовать директивы типа @if в шаблонах блейд. Но зачастую логика подобных преобразований сложнее, чем использование условных операторов.

Чтобы определить аксессор, создайте метод `get{Attribute}Attribute` в вашей модели, где {Attribute} – это имя столбца, к которому вы хотите получить доступ, в верхнем регистре.

```php
public function getPublishedTextAttribute()
{
    $classes = [
        'news-unpub',
        'news-pub',
    ];
    return $classes[$this->published];
}
```


Теперь, чтобы получить доступ к значению, вы можете просто получить доступ к атрибуту `publishedText` экземпляра модели:

 ```html
<article class="{{ $article->publishedText }}">
    <!-- content -->
</article>
```

Разумеется, вы не ограничены взаимодействием с одним атрибутом в аксессоре. Вы можете использовать аксессор для возврата новых  значений из существующих атрибутов:
```php
/**
 * Получить полное имя пользователя.
 *
 * @return string
 */
public function getFullNameAttribute()
{
    return "{$this->first_name} {$this->last_name}";
}
```



###  Мутатор ###

Мутатор преобразует значение атрибута в момент их присвоения экземпляру Eloquent. Чтобы определить мутатор, определите метод `set{Attribute}Attribute` в вашей модели, где {Attribute} – это имя столбца, к которому вы хотите получить доступ, в «верхнем» регистре.

Определим мутатор для атрибута first_name. Этот мутатор будет автоматически вызываться, когда мы попытаемся присвоить значение атрибута first_name модели:

```php
public function setFirstNameAttribute($value)
{
    $this->attributes['first_name'] = strtolower($value);
}
```

Этот механизм работает исключительно при создании модели средствами Eloquent и не сработает при создании документа из админ-панели.


## Пример мутатора для изменения размеров изображений <a name="section7"></a> ##

Давайте добавим модели News возможность отдать ТВ news_image, преобразованный с помощью [хелпера phpThumb](/v3/03_%D0%9F%D0%BE%D0%B4%D1%80%D0%BE%D0%B1%D0%BD%D0%B5%D0%B5/008_DI_%D0%93%D0%BB%D0%BE%D0%B1%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5%20%D0%BF%D0%BE%D0%BC%D0%BE%D1%89%D0%BD%D0%B8%D0%BA%D0%B8.md).

В классе модели необходимо добавить атрибут, который и будет содержать метод для изменения и возвращения значения ТВ.

```php 
<?php 
namespace EvolutionCMS\Main\Models;
use EvolutionCMS\Facades\HelperProcessor;
use EvolutionCMS\Models\SiteContent;
use Illuminate\Database\Eloquent\Builder;

class News extends SiteContent {
	protected $table = 'site_content';
	protected static function booted()
    {
        //  объявляем, что новость это ресурсы, имеющие шаблон номер 3
        static::addGlobalScope('custom_template', function (Builder $builder) {
            $builder->where('template', 3);
        });
    }
    public function getResizedImageAttribute()
    {
        //  используем "помощник"
        return HelperProcessor::phpThumb($this->news_image,'w=300,zc=1');
    }
}
```
> Обратите внимание, сверху мы подключаем `HelperProcessor` для использования хелпера и `Builder` для изменения запроса.

Выводим параметр в нужном нам шаблоне
```html
<img src="/{{ $item->resizedImage }}" alt="">
```