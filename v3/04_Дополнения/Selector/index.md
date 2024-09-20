# Selector

**Важно!** Для работы требуется `DocLister`.

`Selector` является заменой для `mm_ddSelectDocuments` (устаревший ManagerManager), это custom TV для составления списка документов в Evolution CMS.

В отличие от `mm_ddSelectDocuments`:

- уживается с `SimpleGallery`;
- благодаря `DocLister` позволяет выбирать данные для списка как угодно и откуда угодно;
- список результатов поиска можно оформлять по-всякому (реализовано с помощью `prepare` в `DocLister`);
- код полностью открыт.

После установки дополнения из `Extras` необходимо у нужного TV параметра выбрать тип ввода `selector`.

Компонент требует некой настройки под себя.

Допустим, есть tv-параметр c названием `related` для хранения списка похожих товаров. Чтобы его поведение отличалось от базового, нужно создать в файле `assets/tvs/selector/lib/related.controller.class.php` класс:

```php
<?php namespace Selector;
include_once(MODX_BASE_PATH.'assets/tvs/selector/lib/controller.class.php');
class RelatedController extends SelectorController {
... // переопределяем что нужно
}
```

В большинстве случаев будет достаточно изменить свойство `dlParams`, в котором хранятся параметры по умолчанию для запуска `DocLister`:

```php
<?php namespace Selector;
include_once(MODX_BASE_PATH.'assets/tvs/selector/lib/controller.class.php');
class RelatedController extends SelectorController {
    public function __construct($modx) {
        parent::__construct($modx);
        $this->dlParams['parents'] = 5;
        $this->dlParams['addWhereList'] = 'c.published = 1';
    }
}
```

Следует обратить внимание на правильные названия, параметр – `related`, название файла – `related.controller.class.php`, а название класса – `RelatedController` (с большой буквы).

Можно подгружать нужный класс вручную с помощью плагина на `OnManagerPageInit`, в этом случае значение имеет только правильное название класса.

**Важно!** есть ограничение на название tv-параметра: допускаются только латинские буквы и символ подчеркивания.
