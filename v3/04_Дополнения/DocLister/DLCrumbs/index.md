# DLCrumbs

Сниппет для создания навигации хлебные крошки (breadcrumbs) с помощью `DocLister`.

Возможно использовать любой из параметров `DocLister` в вызове `DLCrumbs`.

Автор: [Agel_Nash](https://github.com/AgelxNash)

## Параметры

| параметр                          | описание                          | по умолч.        |
| --------------------------------- | --------------------------------- | ---------------- |
| [id](#param_id)                   | Страница для построение навигации | текущая страница |
| [hideMain](#param_hidemain)       | Без домашней страницы             | `0`              |
| [showCurrent](#param_showcurrent) | Показывать текущую страницу       | `0`              |
| [minDocs](#param_mindocs)         | Сколько показывать                | `0`              |
| [tpl](#param_tpl)                 | Шаблон элемента                   | см. описание     |
| [tplFirst](#param_tplfirst)       | Шаблон первого элемента           | см. описание     |
| [tplCurrent](#param_tplcurrent)   | Шаблон текущей страницы           | см.описание      |
| [ownerTpl](#param_ownertpl)       | Шаблон обёртки                    | см.описание      |

### <a name="params_id"></a> id

ID страницы, для которой надо построить навигацию, по умолчанию текущая страница.

_Значение:_ id существующего ресурса.

_По умолчанию:_ `$modx->documentIdentifier`

### <a name="params_hidemain"></a> hideMain

Убирать ссылку на домашнюю страницу (значение `0`).

_Значения:_ `0`, `1`.

_По умолчанию:_ `0`.

### <a name="params_showcurrent"></a> showCurrent

Включить текущую страницу (значение `1`).

_Значения:_ `0`, `1`.

_По умолчанию:_ `0`.

### <a name="params_mindocs"></a> minDocs

Минимальное количество отображаемых элементов.

_По умолчанию:_ `0`.

### <a name="params_tpl"></a> tpl

Шаблон вывода элемента.

_По умолчанию:_

```
@CODE:<li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem"><meta itemprop="position" content="[+iteration+]" />
    <a href="[+url+]" title="[+e.title+]" itemprop="item">
        <span itemprop="name">[+title+]</span>
    </a>
</li>
```

### <a name="params_tplfirst"></a> tplFirst

Шаблон вывода первого пункта.

_По умолчанию:_ нет значения

_Пример шаблона:_

```
@CODE:<li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem" class="home-link">
    <meta itemprop="position" content="[+iteration+]" />
    <a href="[+url+]" title="[+longtitle+]" itemprop="item" class="icon icon-home"><i class="fa fa-home"></i></a>
</li>
```

### <a name="params_tplcurrent"></a> tplCurrent

Шаблон вывода текущей страницы.

_По умолчанию:_

```
@CODE:<li class="active" itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
    <meta itemprop="position" content="[+iteration+]" />
    <span itemprop="item">[+title+]</span>
</li>
```

### <a name="params_ownertpl"></a> ownerTpl

Шаблон обертки.

_По умолчанию:_

```
@CODE:<nav class="breadcrumbs">
    <ul class="breadcrumb" itemscope itemtype="http://schema.org/BreadcrumbList">
        [+crumbs.wrap+]
    </ul>
</nav>
```
