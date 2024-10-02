# Сниппет evoSortBlock

Сниппет для вывода блока с настройков сортировки и выбора количества товаров на странице.

Сниппет для формирования блока сортировки.

Содержание

- [Параметры](#params)
- [Шаблоны](#tpls)
- [Классы](#classes)
- [По умолчанию](#defaults)
- [без eFilterResult](#noefilterresult)
- [Логика работы js](#js)
- [Пример](#example)

## <a name="params"></a> Параметры

| параметр                                                | описание                    | умолч |
| ------------------------------------------------------- | --------------------------- | ----- |
| [displayConfig](#param_displayconfig)                   | настройка количества        |       |
| [sortConfig](#param_sortconfig)                         | настройка сортировки        |       |
| [ajax](#param_ajax)                                     | использовать ajax           | `0`   |
| [changeSortByClickField](#param_changesortbyclickfield) | изменять порядок сортировки | `0`   |

### <a name="param_displayconfig"></a> displayConfig

Настройка селекта или ссылок для указания количества товаров на странице.

_Пример:_ `20||30||40||все==all`.

### <a name="param_sortconfig"></a> sortConfig

Настройка ссылок для указания поля по которому сортируются товары.

_Пример:_ `По название==pagetitle||По индексу==menuindex||Цена от маленькой==price:asc||Цена от большой==price:desc`.

### <a name="param_ajax"></a> ajax

Использовать ли ajax.

_Значения:_ `0`, `1`

_По умолчанию:_ `0`

### <a name="param_changesortbyclickfield"></a> changeSortByClickField

Изменять направление сортировки при повторном клике по параметру сортировки.

Параметр необходим если у нас нет блока для выбора направление, а надо изменять направление при клике второй раз по параметру поля.

_Значения:_ `1`, `0`.

_По умолчанию:_ `0`.

## <a name="tpls"></a> Шаблоны

- [Основная обертка блока](#tpls_main)
- [Выбор количества товаров](#tpls_amount)
- [Выбор поля для сортировки](#tpls_sort)
- [Выбор направления сортировки](#tpls_sortdir)

### <a name="tpls_main"></a> Основная обертка блока

#### ownerTpl

|                      |                                                                                       |
| -------------------- | ------------------------------------------------------------------------------------- |
| `[+class+]`          | класcы для обертки, которые необходимы для работы `js`, а именно `sort-wrap` и `ajax` |
| `[+display.block+]`  | блок выбора количества товаров                                                        |
| `[+sort.block+]`     | блок выбора поля для сортировки                                                       |
| `[+sort.direction+]` | блок выбора направления сортировки                                                    |

_Пример:_ `<div class="[+class+]">[+display.block+][+sort.block+]</div>`

### <a name="tpls_amount"></a> Выбор количества товаров

#### displayOwnerTpl

Обертка блока для выбора количества элементов на странице.

|               |                |
| ------------- | -------------- |
| `[+class+]`   | классы обёртки |
| `[+wrapper+]` | данные         |

_Пример:_ `<select class="[+class+]">[+wrapper+]</select>`

_Пример:_ `<div class="[+class+]">[+wrapper+]</div>`

#### displayRowTpl

Шаблон вывода строки, `option` для селекта или тег `a` для блока.

|                |                 |
| -------------- | --------------- |
| `[+value+]`    | значение        |
| `[+selected+]` |                 |
| `[+data+]`     |                 |
| `[+class+]`    | классы элемента |
| `[+caption+]`  | текст элемента  |

_Пример:_ ` <option value="[+value+]" [+selected+] >[+caption+]</option>`

_Пример:_ `<a [+data+] class="[+class+]">[+caption+]</a>`

### <a name="tpls_sort"></a> Выбор поля для сортировки

#### sortOwnerTpl

Обертка блока для выбора поля по которому элементы сортируются на странице.

|               |        |
| ------------- | ------ |
| `[+wrapper+]` | данные |

_Пример:_ `<ul>[+wrapper+]</ul>`

#### sortRowTpl

Шаблон вывода ссылки для выбора поля.

|               |                 |
| ------------- | --------------- |
| `[+class+]`   | классы элемента |
| `[+data+]`    |                 |
| `[+caption+]` | текст элемента  |

_Пример:_ `<a class="[+class+]" [+data+]>[+caption+]</a>`

### <a name="tpls_sortdir"></a> Выбор направления сортировки

#### sortDirectionTpl

Обертка блока выбора направления.

|            |                 |
| ---------- | --------------- |
| `[+up+]`   | сортировка asc  |
| `[+down+]` | сортировка desc |

#### sortDirectionUpTpl

Шаблон ссылки для выбора направления сортировки `asc`

|             |                                                                                                          |
| ----------- | -------------------------------------------------------------------------------------------------------- |
| `[+class+]` | css класс                                                                                                |
| `[+data+]`  | дата атрибут data-value в котором хранится текущее поле а направление сортировки asc. Пример `price:asc` |

#### sortDirectionDescTpl

Шаблон ссылки для выбора направления сортировки `desc`

|             |                                                                                                           |
| ----------- | --------------------------------------------------------------------------------------------------------- |
| `[+class+]` | css класс                                                                                                 |
| `[+data+]`  | дата атрибут data-value в котором хранится текущее поле а направление сортировки asc. Пример `price:desc` |

## <a name="classes"></a> Классы

| класс                      | описание                                                          | умолч.           |
| -------------------------- | ----------------------------------------------------------------- | ---------------- |
| `displayActiveClass`       | активный пункт в выборе количества элементов на странице          | `active`         |
| `sortActiveClass`          | активный пункт в выборе поля для сортировки элементов на странице |                  |
| `sortUpClass`              | ссылка выбора поля когда сортировка от маленького к большому      | `up`             |
| `sortDownClass`            | ссылка выбора поля когда сортировка от большого к маленькому      | `down`           |
| `sortFieldClass`           | ссылка выбора поля и направления сортировки                       | `set-sort-field` |
| `sortDirectionActiveClass` | активная ссылка выбранного направления сортировки                 | `active`         |

## <a name="defaults"></a> Значения по умолчанию

| значение           | описание                                      |
| ------------------ | --------------------------------------------- |
| `displayDefault`   | количество элементов на странице по умолчанию |
| `sortFieldDefault` | поле сортировки по умолчанию                  |
| `sortOrderDefault` | направление сортировки по умолчанию           |

## <a name="noefilterresult"></a> Устанавливаемые плейсхолдеры для работы без eFilterResult

| плейсхолдер    | описание               |
| -------------- | ---------------------- |
| `sort_display` | количество товаров     |
| `sort_field`   | поле для сортировки    |
| `sort_order`   | направление сортировки |

Также если использовать снипет без eFilter необходимо подключить `js` файл `eFilter.js` вручную.

А товары обернуть в обертку `eFilterResult`

```html
<div id="eFiltr_results_wrapper">
  <div class="eFiltr_loader"></div>
  <div id="eFiltr_results"></div>
</div>
```

## <a name="js"></a> Логика работы js

### Для выбора количества товаров

- Обрабатывается клик по элементу с класом `set-display-field`
- Если это тег `a` (ссылка) и событие `click`, то информация про количество берётся из дата атрибута.
- Если это событие `change`, то информация берется из атрибута `value` элемента `option`.
- Далее `ajax` запрос из параметра `sortDisplay` со значением.
- Если `ajax` отключен, то обновление страницы.

### Для выбора поля и направления сортировки.

- Обрабатывается клик по элементу c класом `set-sort-field`.
- Если это тег `a` (ссылка) и событие `click`, то информация про поле и направление берется из дата атрибута.
- Если это событие `change`, то информация берется из атрибута `value` элемента `option`.
- Далее `ajax` запрос из параметра `sortBy` со значением.
- Если `ajax` отключен, то обновление страницы.

## <a name="example"></a> Пример

```
[!evoSortBlock?
    &ownerTpl=`@CODE:
        <div class="sorting-block__filters [+class+]"><form action="#">[+display.block+][+sort.block+]</form></div>`
    &displayOwnerTpl=`@CODE:
        <div class="sorting-block__filters-amount">
            <span class="sorting-block__filters-label">Показывать:</span>
            <div class="sorting-block__select">
                <div class="inline-select">
                    <select class="decor-select js-select[+class+]">[+wrapper+]</select>
                </div>
            </div>
        </div>`
    &sortOwnerTpl=`@CODE:
        <div class="sorting-block__filters-type">
            <span class="sorting-block__filters-label sorting-block__filters-label--type">Сортировать:</span>
            <div class="sorting-block__filters-block">
                <span class="sorting-block__filters-mobile-active">
                    <span class="sorting-block__filters-mobile-active-inner">По популярности</span>
                </span>
                <ul class="sorting-block__filters-list">[+wrapper+]</ul>
            </div>
        </div>`
    &sortRowTpl=`@CODE:
        <li class="sorting-block__filters-item">
            <a href="#"  [+data+] [+selected+]  class="sorting-block__filters-link [+class+]">[+caption+]</a>
        </li>`
    &sortActiveClass=`is-active`
    &sortConfig=`Название==pagetitle||Дата поступления==menuindex||Цена==price`
    &ajax=`1`
!]
```
