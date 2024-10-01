# DLRequest

Запуск сниппетов с параметрами из get/post.

Автор: [Pathologic](https://github.com/Pathologic/DLRequest)

Сниппет разработан для замены `Ditto` с экстендером `request` по просьбе _Extremum_ и на его же средства, за что большое спасибо.

Для работы требуется DocLister.

Сниппет принимает значения из `post` или `get`-массива и использует их в качестве параметров для запуска другого сниппета. Изначально планировалась работа только с `DocLister` (отсюда и название), но в итоге можно вызывать любой сниппет. Тем не менее, наличие установленного DocLister'а обязательно.

DLRequest умеет не только передавать параметры, но и строить форму для управления этими самыми параметрами, без костылей типа:

```php
<?php
// [!selected? &field=`ditto_id1_sortDir` &value=`DESC`!]
if ($_REQUEST[$field] == $value) {
    echo "selected";
}
```

Важный момент – возможные значения параметров задаются разработчиком, то есть если в форме для выбора количества документов на странице указано «10, 20, 50, 100», то подставить руками `url?display=100500` не получится – такой параметр будет просто проигнорирован.

Предусмотрена также возможность сохранять значения параметров, которые передаются из браузера для других сниппетов, например, для `DocLister` это будет параметр `page`. То есть если в списке документов вы находитесь на странице 5, то после смены, например, направления сортировки, все равно останетесь на странице 5.

Теперь приведу пример вызова, из которого понятно, как это все работает:

```
[+paramsForm+]
<div>
  [!DLRequest?
    &runSnippet=`DocLister`
    &parents=`2`
    &tpl=`@CODE:[+id+]. [+pagetitle+]`
    &paginate=`pages`
    &display=`0`
    &rqParams=`{
        "sortBy": {
            "id":"По id",
            "pagetitle":"По pagetitle"
        },
        "sortDir":{
            "asc":"По возрастанию",
            "desc":"По убыванию"
        },
        "display":{
            "1":"1 документ",
            "3":"3 документа",
            "5":"5 документов"
        }
    }`
    &rqParamsNames=`{
        "sortBy":"Сортировать по",
        "sortDir":"Порядок",
        "display":"Результатов на странице"
    }`
    &selectedClassName=`selected`
    &paramsForm=`paramsForm`
    &keepParams=`page`
    &paramsOwnerTPL=`@CODE:
        <form method="get" action="%5B~%5B*id*%5D~%5D.html">
        [+keepParams+] [+params+]
        <button type="submit">Отправить</button>
        </form>`
    &param.ownerTPL=`@CODE:
        <label>[+description+]</label>
        <select name="[+paramName+]">[+values+]</select>`
    &param.tpl=`@CODE:
        <option value="[+value+]" [+selectedClass+]>[+description+]</option>`
    &sortBy.tpl=`@CODE:
        <option value="[+value+]" [+selectedClass+]>[+description+]</option>`
    &display.ownerTPL=`@CODE:
        <label style="color:red;">[+description+]</label>
        <select name="[+paramName+]">[+values+]</select>`
    &keepTpl=`@CODE:
        <input name="[+paramName+]" value="[+value+]" type="hidden" />`
!]
</div>
[+pages+]
```
