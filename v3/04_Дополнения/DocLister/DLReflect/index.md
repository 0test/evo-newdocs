# Описание сниппета

DLReflect - построение списка дат.

## Параметры

Можно использовать параметры `DocLister`, а также добавлены свои параметры.

| параметр                                            | описание                                     | умолч.       |
| --------------------------------------------------- | -------------------------------------------- | ------------ |
| [reflectType](#param_reflecttype)                   | Тип фильтрации                               | `month`      |
| [wrapTpl](#param_wraptpl)                           | Шаблон обёртки                               | см. описание |
| [reflectTpl](#param_reflecttpl)                     | Шаблон даты.                                 | см. описание |
| [activeReflectTpl](#param_activereflecttpl)         | Шаблон активной даты.                        | см. описание |
| [currentReflect](#param_currentreflect)             | Текущая дата                                 |              |
| [selectCurrentReflect](#param_selectcurrentreflect) |                                              | `1`          |
| [appendCurrentReflect](#param_appendcurrentreflect) |                                              | `1`          |
| [activeReflect](#param_activereflect)               |                                              | `current`    |
| [reflectSource](#param_reflectsource)               | Источник даты.                               | `content`    |
| [reflectField](#param_reflectfield)                 | Поле, из которого берется дата документа.    |              |
| [targetID](#param_targetid)                         | ID документа на котором настроена фильтрация | текущий      |
| [limitBefore](#param_limitbefore)                   | Число элементов до                           | `0`          |
| [limitAfter](#param_limitafter)                     | Число элементов после                        | `0`          |

### <a name="param_reflecttype"></a> reflectType

Тип фильтрации.

_Значения:_

- `month` - по месяцам
- `year` - по годам

_По умолчанию:_ `month`

### <a name="param_wraptpl"></a> wrapTpl

Шаблон обёртки.

_По умолчанию:_ `<div class="reflect-list"><ul>[+wrap+]</ul></div>`

### <a name="param_reflecttpl"></a> reflectTpl

Шаблон даты.

_По умолчанию:_ `<li><a href="[+url+]" title="[+title+]">[+title+]</a></li>`

Поддерживается плейсхолдеры:

| плейсхолдер           | описание                                                                                                         |
| --------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `[+url+]`             | ссылка на страницу где настроена фильтрация по документам за выбранную дату                                      |
| `[+monthName+]`       | Название месяца. Плейсхолдер доступен только в режиме `reflectType` = `month`                                    |
| `[+monthNum+]`        | Номер месяца с ведущим нулем (01, 02, 03, ..., 12). Плейсхолдер доступен только в режиме `reflectType` = `month` |
| `[+year+]`            | Год                                                                                                              |
| `[+title+]`           | Дата (числовое представление месяц + год или просто год в зависиомсти от `reflectType`)                          |
| `[+reflects+]`        | Общее число уникальных дат, которые возможно отобразить в списке                                                 |
| `[+displayReflects+]` | Число уникальных дат отображаемых в общем списке                                                                 |

### <a name="param_activereflecttpl"></a> activeReflectTpl

Шаблон активной даты.

Поддерживается такие же плейсхолдеры, как и в шаблоне `reflectTpl`

_По умолчанию:_ `<li><span>[+title+]</span></li>`

### <a name="param_currentreflect"></a> currentReflect

Текущая дата (месяц в формате `00-0000` или год в формате `0000`), где:

- `00` - номер месяца с ведущим нулем (01, 02, 03, ..., 12)
- `0000` - год

Если не указан в параметре, то генерируется автоматически текущая дата

### <a name="param_selectcurrentreflect"></a> selectCurrentReflect

_По умолчанию:_ `1`

### <a name="param_appendcurrentreflect"></a> appendCurrentReflect

Если в списке дат не встречается указанная через параметр `currentReflect`, то этот параметр определяет - стоит ли добавлять дату или нет

_Значения:_

- `0` - не добавлять,
- `1` - добавлять

Этот параметр тесно связан с параметром `activeReflect`

_По умолчанию:_ 1

### <a name="param_activereflect"></a> activeReflect

Дата которую выбрал пользователь.

Если параметр не задан, то в качестве значения по умолчанию используется значение параметра `currentReflect`.
При наличии GET параметра month/year (в зависимости от значения параметра `reflectType`), приоритет отдается ему.

При отсутствии выбранной даты в общем списке дат и совпадении значений параметров `currentReflect` и `activeReflect`, дата будет автоматически добавлена в общий список. Тем самым значение параметра `appendCurrentReflect` будет расцениваться как `1`.

_Значения:_ Текущая дата (месяц в формате `00-0000` или год в формате `0000`)

_По умолчанию:_ `current`

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

### <a name="param_targetid"></a> targetID

ID документа на котором настроена фильтрация по месяцам

_По умолчанию:_ ID текущего документа

### <a name="param_limitbefore"></a> limitBefore

Число элементов до месяца указанного в `activeMonth` параметре

_Значения:_ Любое число. `0` расценивается как все доступные месяцы

_По умолчанию:_ `0`

### <a name="param_limitafter"></a> limitAfter

Число элементов после месяца указанного в `activeMonth` параметре.

_Значения:_ Любое число. `0` расценивается как все доступные месяцы

_По умолчанию:_ `0`

## Пример использования

```
[[DLReflect?
&idType=`parents`
&parents=`87`
&reflectType=`year`
&reflectSource=`tv`
&reflectField=`date`
&limitBefore=`1`
&limitAfter=`3`
]]
```
