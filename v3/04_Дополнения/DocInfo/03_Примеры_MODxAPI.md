# Пример DocInfo на базе modResource MODxAPI

Теперь сниппет `DocInfo` не будет нагружать страницу повторными SQL запросами при многократном получении значений из одного и того же документа.

Содержимое репозитория размещается в папке `/assets/lib/MODxAPI/`

Создается плагин на событиях `OnWebPageInit`, `OnManagerPageInit` и `OnPageNotFound` с кодом:

```php
include_once(MODX_BASE_PATH."assets/lib/MODxAPI/modResource.php");
if(!isset($modx->doc)) {
    $modx->doc = new modResource($modx);
}
```

## После чего создается сниппет допустим DocInfo

```php
$id = isset($id) ? (int)$id : $modx->documentObject['id'];
$field = isset($field) ? (string)$field : 'id';
if($field == 'id'){
    $out = $id;
}else{
    if($modx->documentObject['id'] == $id){
        $out = isset($modx->documentObject[$field]) ? $modx->documentObject[$field] : '';
        if(is_array($out)){
           $out = isset($out[1]) ? $out[1] : '';
        }
    }else{
        $out = $modx->doc->edit($id)->get($field);
    }
}
return (string)$out;
```

Profit!

Теперь сниппет `DocInfo` не будет нагружать страницу повторными SQL запросами при многократном получении значений из одного и того же документа.
