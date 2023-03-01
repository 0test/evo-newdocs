# DocumentManager

```php
/*
* namespace
*/
use EvolutionCMS\DocumentManager\Facades\DocumentManager;
```

Методы, связаные с управлением ресурсами в EvolutionCMS.
Все действия, связанные с документами проходят через эти методы.

## Доступные функции и примеры использования

- [ get ](#get) - получение документа
- [ create ](#create) - создание документа
- [ edit ](#edit) - редактирование документа
- [ delete ](#delete) - удаление документа
- [ undelete ](#undelete) - восстановление удаленного документа
- [ duplicate ](#duplicate) - копирование документа
- [ setGroups ](#setGroups) - установка групп документа
- [ publish ](#publish) - опубликовать документ
- [ unpublish ](#unpublish) - снять с публикации документ
- [ clearCart ](#clearCart) - очистить корзину с удалёнными документами

## **get** - получение документа<a name="get"></a>

```php
SiteContent \DocumentManager::get(integer $documentId)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentId` - id документа, который желаем получить

Пример получения документа

```php
$document = \DocumentManager::get(1);
dd($document->toArray());
```

## **create** - создание документа<a name="create"></a>

```php
SiteContent \DocumentManager::create(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив содержащий поля документа и TV-шки, для ключей TV названия этих TV. Обязательные поля pagetitle, template
- `$events` - указатель вызываем ли мы события связанные с созданием документа
- `$cache` - указатель сбрасываем ли кэш после создания документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции создания документа

```php
$document = ['pagetitle' => 'test', 'template' => '1', 'test'=>'value']; //test - название tv
try {
    $document = \DocumentManager::create($document);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
     dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **edit** - редактирование документа<a name="edit"></a>

```php
SiteContent \DocumentManager::edit(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив содержащий поля документа. Обязательные поля: `id`
- `$events` - указатель вызываем ли мы события связанные с редактированием документа
- `$cache` - указатель сбрасываем ли кэш после редактирования документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции редактирования документа

```php
$data = ['id'=> 1, 'pagetitle' => 'new title', 'test'=>'new value']; //test - название tv
try {
    $document = \DocumentManager::edit($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
     dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **delete** - удаление документа<a name="delete"></a>

```php
SiteContent \DocumentManager::delete(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив, содержащий id документа.
- `$events` - указатель, вызываем ли мы события связанные с удалением документа
- `$cache` - указатель, сбрасываем ли кэш после удаления документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции удаления документа

```php
$data = ['id'=> 1];
try {
    $document = \DocumentManager::delete($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **undelete** - восстановление удаленного документа<a name="undelete"></a>

```php
SiteContent \DocumentManager::undelete(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив, содержащий id документа.
- `$events` - указатель вызываем ли мы события связанные с восстановлением документа
- `$cache` - указатель сбрасываем ли кэш после восстановления документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример восстановления удалённого документа

```php
$data = ['id'=> 1];
try {
    $document = \DocumentManager::undelete($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **duplicate** - копирование документа<a name="duplicate"></a>

```php
SiteContent \DocumentManager::duplicate(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив, содержащий id копируемого документа.
- `$events` - указатель вызываем ли мы события связанные с копированием документа
- `$cache` - указатель сбрасываем ли кэш после копирования документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции копирования документа

```php
$data = ['id'=> 1];
try {
    $document = \DocumentManager::duplicate($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **setGroups** - назначению документу его группы документа<a name="setGroups"></a>

```php
SiteContent \DocumentManager::setGroups(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив содержащий `id` документа и `document_groups` - массив групп документа. Оба поля обязательны.
- `$events` - указатель вызываем ли мы события связанные с назначением группы документа
- `$cache` - указатель сбрасываем ли кэш после назначения группы документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции назначения группы документов

```php
$data = ['id'=> 1, 'document_groups' => [1,2]];
try {
    $document = \DocumentManager::setGroups($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **publish** - опубликовать документ<a name="publish"></a>

```php
SiteContent \DocumentManager::publish(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив содержащий `id` документа. Поле является обязательным.
- `$events` - указатель вызываем ли мы события связанные публикацией документа
- `$cache` - указатель сбрасываем ли кэш после публикации документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции публикации документа

```php
$data = ['id'=> 1];
try {
    $document = \DocumentManager::publish($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **unpublish** - снять с публикации документ<a name="unpublish"></a>

```php
SiteContent \DocumentManager::unpublish(array $documentData, bool $events = true, bool $cache = true)
```

Функция возвращает объект модели документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - массив содержащий `id` документа. Поле является обязательным.
- `$events` - указатель, вызываем ли мы события связанные со снятием с публикации документа
- `$cache` - указатель, сбрасываем ли кэш после снятия с публикации документа

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции снятия документа с публикации

```php
$data = ['id'=> 1];
try {
    $document = \DocumentManager::unpublish($data);
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```

## **clearCart** - очистить корзину с удалёнными документами<a name="clearCart"></a>

```php
SiteContent \DocumentManager::clearCart(array $documentData => [], bool $events = true, bool $cache = true)
```

Функция возвращает объект модели первого документа SiteContent

Параметры, которые принимает функция:

- `$documentData` - Необязательное поле
- `$events` - указатель вызываем ли мы события связанные очисткой корзины
- `$cache` - указатель сбрасываем ли кэш после очистки корзины

**ВНИМАНИЕ**
Функция может бросить два различных исключения

- **\EvolutionCMS\Exceptions\ServiceValidationException** исключение срабатывает в случае если мы передали плохие данные в $documentData.
- **\EvolutionCMS\Exceptions\ServiceActionException** исключение срабатывает в ситуации когда возникла ошибка в процессе обработки данных.

Пример функции очистки корзины

```php
try {
    $document = \DocumentManager::clearCart();
} catch (\EvolutionCMS\Exceptions\ServiceValidationException $exception) {
    $validateErrors = $exception->getValidationErrors(); //Получаем все ошибки валидации
    dd($validateErrors); //Выводим все ошибки валидации
} catch (\EvolutionCMS\Exceptions\ServiceActionException $exception) {
    dd($exception->getMessage()); //Выводим ошибку процесса обработки данных
}
```
