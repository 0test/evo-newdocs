# Сниппет eFilterResult

Снипет является оберткой `Doclister`. Если нет выбранных тв параметров ведет себя как обычный `DocLister` и получает товары в режиме `idType = parents`, если форма фильтрации не пустая то в режиме documents получает плейсхолдер из списка `ids` от `eFilter`.

Содержание:

- [Параметры](#params)
- [ajax](#ajax)
- [Другое](#other)

## <a name="params"></a> Параметры

| параметр            | описание           | умолч. |
| ------------------- | ------------------ | ------ |
| [lang](#param_lang) | язык для склонения | `ru`   |
| [pid](#param_pid)   | аналог `parents`   |        |

### <a name="param_lang"></a> lang

Код языка для склонения слова "товар".

_По умолчанию:_ `ru`.

### <a name="param_pid"></a> pid

Аналог параментра `parents` из `DocLister`.

## <a name="ajax"></a> ajax

Для работы ajax необходима следующая структура обертки:

```html
<div id="eFiltr_results_wrapper">
  <div class="eFiltr_loader"></div>
  <div id="eFiltr_results">[+dl.wrap+][+pages+]</div>
</div>
```

Если для `eFilter` задан параметр `ajax = 1`, то стандартная пагинация переопределяется.

Обертка пагинации должна иметь класс `pagination`, пример шаблонов:

```
&TplNextP=`@CODE:
    <a data-prefix="" data-page="[+num+]">&gt;</a>`
&TplPrevP=`@CODE:
    <a data-prefix="" data-page="[+num+]">&lt;</a>`
&TplPage=`@CODE:
    <a data-prefix="" data-page="[+num+]" class="page">[+num+]</a>`
&TplWrapPaginate=`@CODE:
    <div class="paginate">[+wrap+]</div>`
```

Можно задать свои шаблоны, главное вместо `href` использовать `data-page="[+num+]"` и если задан параметр `id` для `eFilterResult` в `data-prefix` необходимо записать `id` + `_`.

## <a name="other"></a> Другое

### Склонение слова "Товар" по количеству

`[+параметр_id.plural+]` Склонение слова "Товар" по количеству

Для переопределения склоняемого слова необходимо прописать параметры.

- `phrase1` - значение "Товар"
- `phrase2` - значение "Товара"
- `phrase3` - значение "Товаров"

### Подгрузка товаров через ajax

Пример html шаблона для блока "Показать ещё"

```
[!if?
    &is=`[+параметр_id.isstop+]:!=:1`
    &then=`
        <div class="amount eFilter_more_wrap">
            <a data-page="[+параметр_id.pages_next+]" data-prefix="[+параметр_id.isstop+]_" class="eFilter_more">Показать ещё</a>
        </div>`
!]
```

- `eFilter_more_wrap` - класс для обертки
- `eFilter_more` - класс для ссылки

Если `id` для `eFilter` не задан `data-prefix` пустой.

При использовании "Показать еще" в блоке из классом `eFiltr_results` должны быть только товары.

### Количество товаров и склонение

Для замены количества товаров на странице и склоняемого слова товар нужно задать класы

- `filter_display` - количество товаров
- `filter_plural` - склоняемое слово
- `[+параметр_id.pages_next+]` - номер следующей страницы
