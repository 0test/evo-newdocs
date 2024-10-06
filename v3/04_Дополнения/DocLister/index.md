# DocLister

- [Описание](01_Описание.md)
- [Параметры выборки](02_Параметры_выборки.md)
- [Обработка данных перед выводом](03_Обработка_данных_перед_выводом.md)
- [Вывод данных](04_Вывод_данных.md)
- [Фильтры](05_Фильтры.md)
- [Лексиконы](06_Лексиконы.md)
- [Примеры](07_Примеры.md)
- [Отладка](08_Отладка.md)
- [Разработчикам](09_Разработчикам.md)
- [MODxAPI](10_MODxAPI.md)

На базе класса DocLister сформировано 12 сниппетов:

- DocLister - основной сниппет для вывода информации по принципу сниппетов Ditto и CatalogView
- [DLcrumbs](DLCrumbs/index.md) - для формирования хлебных крошек по принципу сниппета Breadcrumbs
- DLglossary - для фильтрации документов по первому символу в определенном поле
- DLvaluelist - для замены сниппета DropDownDocs
- DLTemplate - для замены `$modx->parseChunk()`
- DLFirstChar - выборка документов и группировках в блоках по первой букве
- [DLPrevNext](DLPrevNext/index.md) - цикличная навигация вперед/назад между соседними документами
- [DLMenu](DLMenu/index.md) - Построение меню неограниченой вложенности
- [DLSitemap](DLSitemap/index.md) - Построение xml-карты сайта
- DLReflect - Построение списка дат
- DLReflectFilter - Фильтрация документов по датам
- DLBeforeAfter - Пагинация по прошедшим и предстоящим событиями с учетом текущей даты

Компоненты на базе DocLister:

- [SimpleGallery](../SimpleGallery/index.md) – вывод галереи на странице
- [SimpleTube](../SimpleTube/index.md) – плагин и сниппет для создания видеогалерей
- [SimpleFiles](../SimpleFiles/index.md) – прикрепляем к странице файлы
- [SimplePolls](../SimplePolls/index.md) - голосования и опросы
- [LikeDislike](../LikeDislike/index.md) – возможность ставить оценки
- [FormLister](../FormLister/index.md) - cниппет для работы с формами
- [FastImageTV](../FastImageTV/index.md) - custom TV для загрузки картинки к документу
- [DLRequest](../DLRequest/index.md) - запуск сниппетов с параметрами из get/post
- [evoSearch](../evoSearch/index.md) - поиск по сайту
- [eFilter](../eFilter/index.md) - фильтр и сортировка товаров
- [Selector](../Selector/index.md) - custom TV для составления списка документов
- [Commerce](../Commerce/index.md) - интернет-магазин
- [EditDocs](../editDocs/) - загрузка контента из файла
