# Debugger Tracy в Evolution CMS #

Tracy — инструмент, предназначенный для облегчения отладки PHP. Помогает с визуализацией и протоколированием ошибок, дампом переменных и многим другим ([демо](https://nette.github.io/tracy/tracy-debug-bar.html)).


## Как включить ##

Создать файл
```
core\custom\config\tracy\active.php
```

```php
<?php 
return 'adminfrontonly';
```
## Параметры ##
Возможные значения:
```
manager
admin
adminfrontonly
managerfrontonly
```
## Пример ##

Если вы хотите отключать Tracy на боевом сайте, но держать включенным на локальном:

```php
if(env('APP_ENV') && env('APP_ENV') == 'devel'){
  return 'adminfrontonly';
}
else{
  return false;
}
```
Где `APP_ENV` - директива из файла `.env` (см. [развёртывание](/v3/01_%D0%9D%D0%B0%D1%87%D0%B0%D0%BB%D0%BE%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B/04_%D0%A0%D0%B0%D0%B7%D0%B2%D0%B5%D1%80%D1%82%D1%8B%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5.md)).