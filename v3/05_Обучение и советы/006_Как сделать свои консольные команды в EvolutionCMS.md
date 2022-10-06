# Как сделать свои консольные команды в EvolutionCMS #

Для примера сделаем команду, которая будет менять пароль пользователю.

Синтаксис будет следующий:
```
php artisan user:resetpass 1 123
```
Где первый аргумент это id пользователя, второй - новый пароль.

![Console](/assets/images/s18.png)

* [Регистрируем функции](#section1)
* [Пишем класс](#section2)
* [Подробности](#section3)


## Регистрируем функции в провайдере своего пакета <a name="section1"></a> ##

В нашем случае это main.

```
core/custom/packages/main/src/MainServiceProvider.php
```

```php
<?php
namespace EvolutionCMS\Main;
use EvolutionCMS\Facades\Console;
use EvolutionCMS\Main\Console\UserResetpass;
use EvolutionCMS\ServiceProvider;

class MainServiceProvider extends ServiceProvider
{
    protected $namespace = 'main';
    protected $commands = [
        //  сюда можно писать новые команды
        'UserResetpass' => 'user:resetpass',
    ];
    public function register()
    {
      $this->registerCommands($this->commands );

      $this->app->singleton('Console', function ($app) {
        return new Console($app, $app['events'], '0.1');
      });
    }
    protected function registerCommands(array $commands)
    {
      foreach (array_keys($commands) as $command) {
        //  Придёт значение из массива команд
        //  "UserResetpass"
        call_user_func_array([$this, "register{$command}"], []);
        //  Вызываем "registerUserResetpass
      }
      $this->commands(array_values($commands));
    }
    protected function registerUserResetpass()
    {
      $this->app->singleton('user:resetpass', function ($app) {
        return new UserResetpass($app);
      });
    }
}

```
Пример содержит функционал для создания массива команд, а не одной-единственной. 

Создаём функцию `registerCommands`, которая принимает массив будущих команд, конструирует имя функции и вызывает эту функцию для регистрации соответствующей команды.

Если мы хотим создать ещё команду, нужно добавить соответствующую запись в массив 
```php
protected $commands = [
	'MyNewCommand' => 'mycommand',
];
```
И добавить функцию её вызова, которая бы вернула нужный класс, где описана команда.

```php
protected function registerMyNewCommand()
{
	$this->app->singleton('user:resetpass', function ($app) {
	return new MyNewCommandClass($app);
	});
}
```
## Пишем класс команды <a name="section2"></a> ##

Создаём класс для команды.
Обратите внимание на имена класса, файла и функции в провайдере (код выше, где этот класс был вызван в функции `registerUserResetpass`).

```
core/custom/packages/main/src/Console/UserResetpass.php
```

```php
<?php 
namespace EvolutionCMS\Main\Console;
use EvolutionCMS\Models\User;
use Illuminate\Console\Command;
class UserResetpass extends Command{

    protected $name = 'user:resetpass';
    protected $description = 'Смена пароля пользователя';
    protected $signature  = 'user:resetpass {id : Идентификатор пользователя}{pass : Новый пароль}';
    public function __construct()
    {
        parent::__construct();
    }
    public function handle()
    {
        $id = (int)$this->argument('id');
        $pass = $this->argument('pass');
        $user = User::find($id);
        try {
            if($user){
                $this->info('Нашли пользователя ' . $user->username);
                $user->update(['password' => EvolutionCMS()->getPasswordHash()->HashPassword($pass)]);
                $this->newLine();
                $this->info('Пароль изменен');
            }
            else{
                throw new \Exception("Пользователь не найден");
            }
        } catch (\Exception $th) {
            $this->alert( $th->getMessage() );
        }
        
    }

}
```
Теперь вы можете вызывать эту команду, используя командную строку терминала:
```
php artisan user:resetpass 1 123456
```
Команда использует модель `User`, имеющуюся в EvolutionCMS, ищет пользователя по переданному id, и, если находит, меняет пароль при помощи методов, имеющихся в cms.


## Подробности <a name="section3"></a> ##


После создания команды следует заполнить свойства класса `$signature` и `$description`.   

Свойство  `$signature` позволяет определить имя, аргументы и параметры команды в едином  синтаксисе, схожим с синтаксисом маршрутов.

### Аргументы ###

Все  аргументы и параметры заключаются в фигурные скобки. В примере команда определяет два обязательных аргумента:

```php
protected $signature  = 'user:resetpass {id : Идентификатор пользователя}{pass : Новый пароль}';
```

Можно сделать аргументы необязательными или определить значения по умолчанию:

```php
// Необязательный аргумент 
user:resetpass {id?}
// Необязательный аргумент с заданным по умолчанию значением
user:resetpass {id=1}
```

### Параметры ###

Параметры, как и аргументы, являются разновидностью пользовательского ввода. Параметры должны иметь префикс в виде двух дефисов (--), при использовании их в командной строке. Существует два типа параметров: получающие значение, и те, которые его не получают. 


#### Параметры без значений #### 

Параметры, которые не получают значение, возвращают `true` или `false`. 

```php
protected $signature = 'user:resetpass {id} {--myparam}';
```

Вызов:

```
php artisan user:resetpass --myparam
```
В этом примере при вызове команды может быть указан параметр  `--myparam`. Если `--myparam` передан, то значение этого параметра будет `true`. В противном случае значение будет `false`. Это может быть полезно для задания каких-то опций вашей команде.


#### Параметры со значениями #### 

```php
protected $signature = 'user:resetpass {--myparam=}';
````

В этом примере пользователь может передать значение для параметра. Если параметр не указан при вызове команды, то его значение будет null:

```
php artisan user:resetpass --user=1
```

Параметру можно присвоить значение по умолчанию, указав его после имени. Если значение параметра не передано, то будет использовано значение по умолчанию:

```php
user:resetpass {--user=1}
```

> Использование параметров со значениями более наглядно и удобно для пользователя в некоторых случаях. Вы можете переписать вывод нашей команды с использованием параметров. Тогда она выглядела бы примерно так:

```
php artisan user:resetpass --user=1 --pass=123456
```

### Вывод данных ###


Для вывода данных есть методы `line`, `info`, `comment`, `question`, `warn` и `error`. 

```php
$this->line('Строка текста');
$this->error('Ошибка');
$this->newLine();//Пустая линия
$this->table(
    ['Name', 'Email'],
    User::all(['name', 'email'])->toArray()
);
```

Больше подробностей про команды, вывод данных и прочие атрибуты консоли вы можете найти в [официальной документации Ларавел](https://laravel.com/docs/9.x/artisan).