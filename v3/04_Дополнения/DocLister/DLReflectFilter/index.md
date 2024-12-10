# Описание сниппета

DLReflectFilter - фильтрация документов по датам.

## Параметры

Можно использовать параметры `DocLister`, а также добавлены свои параметры.

| параметр                                            | описание                                 | умолч.    |
| --------------------------------------------------- | ---------------------------------------- | --------- |
| [reflectType](#param_reflecttype)                   | Тип фильтрации                           | `month`   |
| [reflectSource](#param_reflectsource)               | Источник даты.                           | `content` |
| [reflectField](#param_reflectfield)                 | Поле, из которого берется дата документа |           |
| [selectCurrentReflect](#param_selectcurrentreflect) |                                          | `1`       |
| [currentReflect](#param_currentreflect)             | Текущая дата                             |           |
| [activeReflect](#param_activereflect)               |                                          | `current` |

### <a name="param_reflecttype"></a> reflectType

Тип фильтрации.

_Значения:_

- `month` - по месяцам
- `year` - по годам

_По умолчанию:_ `month`

### <a name="param_reflectsource"></a> reflectSource

Источник даты.

_Значения:_

- `tv` - ТВ параметр
- `content` или любое другое значение: Основные параметры документа

_По умолчанию:_ `content`

### <a name="param_reflectfield"></a> reflectField

Имя поля из которого берется дата документа.

_Значения:_ Любое имя существующего ТВ параметра или поля документа

_По умолчанию:_ Если не указана дата публикации, то использовать дату создания документа (`if(pub_date=0,createdon,pub_date)`). Актуально только для таблицы `site_content`.

### <a name="param_selectcurrentreflect"></a> selectCurrentReflect

_По умолчанию:_ `1`

### <a name="param_currentreflect"></a> currentReflect

Текущая дата (месяц в формате `00-0000` или год в формате `0000`), где:

- `00` - номер месяца с ведущим нулем (01, 02, 03, ..., 12)
- `0000` - год

Если не указан в параметре, то генерируется автоматически текущая дата

### <a name="param_activereflect"></a> activeReflect

Дата которую выбрал пользователь.

Если параметр не задан, то в качестве значения по умолчанию используется значение параметра `currentReflect`.
При наличии GET параметра month/year (в зависимости от значения параметра `reflectType`), приоритет отдается ему.

При отсутствии выбранной даты в общем списке дат и совпадении значений параметров `currentReflect` и `activeReflect`, дата будет автоматически добавлена в общий список. Тем самым значение параметра `appendCurrentReflect` будет расцениваться как `1`.

_Значения:_ Текущая дата (месяц в формате `00-0000` или год в формате `0000`)

_По умолчанию:_ `current`

## Пример использования

```
[[DLReflectFilter?
&idType=`parents`
&parents=`87`
&tpl=`listNews`
&paginate=`pages`
&display=`2`
&reflectSource=`tv`
&reflectField=`date`
&reflectType=`month`
&tvList=`date`
&sortDir=`DESC`
]]
[+pages+]
```
