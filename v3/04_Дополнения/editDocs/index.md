# editDocs

Автор: [Grinyaha](https://github.com/Grinyaha/editDocs)

Модуль для Evolution CMS. Для работы необходим `DocLister`. В работе модуля используется библиотека [phpSpreadSheet](https://github.com/PHPOffice/phpspreadsheet/).

Данный модуль предназначен для:

- быстрого редактирования основных полей документа (pagetitle, longtitle и тд.) и своих TV
- импорта данных из таблицы Excel (Calc) или csv
- экспорта данных по основным полям и ТВ в .xlsx и csv
- массового переноса документов от одного родителя (папки) к другому

Модуль доступен для установки из extras

Документация с примерами https://editdocs.grishin.net/

Модуль умеет:

- Редактировать основные поля и TV группы документов разной вложенности, а также изменять ID категорий для плагина MultiCategories
- Импортировать/обновлять таблицы из Excel или Calc и CSV и производить сравнение по выбранному полю( или ТВ). Также можно включить интеграцию с таблицей значений для категорий плагина MultiCategories
- Экспортировать в XLSX и CSV с выбранным уровнем вложенности.
- Массово переносить документы от одного родителя к другому.

Видео-урок по использованию модуля [https://youtu.be/6c_Tg9eGc2g](https://youtu.be/6c_Tg9eGc2g).

> ВАЖНО!

- При импорте обязательно должен быть столбец со стандартным полем `pagetitle`!
- Список полей и ТВ можно регулировать в настройках модуля.
- Внимательно проверяйте названия полей или TV-параметров при импорте.
- При импорте сразу в несколько родительских документов указываем для каждого документа поле parent, тогда основной родитель при импорте можно ставить любой, отличный от 0. Он роли играть не будет.
- Для EVO 3 нужно будет установить `ModxAPI` для `evo3` через `composer`, будет сообщение при запуске модуля.
