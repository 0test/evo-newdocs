# LikeDislike

LikeDislike - возможность ставить оценки на Evolution CMS.

Автор: [Pathologic](https://github.com/Pathologic/LikeDislike)

В пакете:

- `LikeDislike` (сниппет) - который и дает возможность ставить оценки
- `ldController` (сниппет) для запуска DocLister с расширенным контроллером `site_content`
- `LikeDislike` (модуль) чтобы видеть оценки в админке

Защита от накруток <s>никакая</s> простая – куки, ip, также можно разрешить оценивать только зарегистрированным пользователям.

## Установка

После установки нужно запустить модуль, чтобы создались таблицы.

На странице документа нужно подключить скрипт `jGrowl`:

```html
<script
  src="assets/js/jGrowl/jquery.jgrowl.min.js"
  type="text/javascript"
></script>
<link href="assets/js/jGrowl/jquery.jgrowl.min.css" rel="stylesheet" />
```

И скрипт для ajax-обработчика:

```html
<script
  src="assets/snippets/LikeDislike/likedislike.js"
  type="text/javascript"
></script>
<link href="assets/snippets/LikeDislike/likedislike.css" rel="stylesheet" />
```

Вызов сниппета выглядит так:

```
[!LikeDislike?
    &enabledTpl=`@CODE:
        <div class="likedislike" data-id="[+rid+]">
            <a href="#" class="like">
                <i class="fa fa-lg fa-thumbs-up"></i>
                <span>[+like+]</span>
            </a>
            <a href="#" class="dislike">
                <i class="fa fa-lg fa-thumbs-down"></i>
                <span>[+dislike+]</span>
            </a>
        </div>`
    &disabledTpl=`@CODE:
        <div class="likedislike">
            <span class="like">За: <span>[+like+]</span></span>
            <span class="dislike">Против: <span>[+dislike+]</span></span>
        </div>`
!]
```

Скрипт `likedislike.js` написан под верстку в этом примере.

Кроме вывода шаблонов сниппет задает плейсхолдеры `[+modResource.like.{id}+]` и `[+modResource.dislike.{id}+]`.

## Параметры сниппета LikeDislike

| параметр      | описание                                                                                            | умолч.        |
| ------------- | --------------------------------------------------------------------------------------------------- | ------------- |
| `rid`         | id оцениваемого ресурса, если параметр не задан, то по возможности используется id текущего ресурса |               |
| `classKey`    | параметр позволяющий разделять оцениваемые сущности                                                 | `modResource` |
| `action`      | действие: like, dislike, stat                                                                       | `stat`        |
| `enabledTpl`  | шаблон, если разрешено оценивать                                                                    |               |
| `disabledTpl` | шаблон, если запрещено оценивать                                                                    |               |
| `onlyUsers`   | разрешено оценивать только зарегистрированным пользователям                                         |               |

Параметр `classKey` сделан на будущее, вдруг понадобится ставить оценки пользователям или еще чему-нибудь.

Если не задавать шаблоны, то сниппет вернет массив с ключами `like` и `dislike`.

## Параметры сниппета ldController

| параметр      | описание                                                    | умолч.        |
| ------------- | ----------------------------------------------------------- | ------------- |
| `allowLD`     | разрешить оценивать в списке                                | `0`           |
| `enabledTpl`  | шаблон, если разрешено оценивать                            |               |
| `disabledTpl` | шаблон, если запрещено оценивать                            |               |
| `onlyUsers`   | разрешено оценивать только зарегистрированным пользователям |               |
| `classKey`    | параметр позволяющий разделять оцениваемые сущности         | `modResource` |

Параметр `classKey` сделан на будущее, вдруг понадобится ставить оценки пользователям или еще чему-нибудь.

Для вывода в основном шаблоне `&tpl` нужно использовать плейсхолдер `[+likedislike+]`.

Имена полей в параметрах для выборки и сортировки лучше задавать с префиксом таблицы (`c` для `site_content` и `ld` для `likedislike`).

Поле `like` обязательно должно быть в обратных кавычках – _`_, иначе поломаются запросы.
