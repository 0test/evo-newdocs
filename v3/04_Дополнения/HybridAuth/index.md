# HybridAuth

Автор: [Pathologic](https://github.com/Pathologic/EvoHybridAuth)

> Важно: для работы требуется FormLister

## Особенности:

- используется библиотека `HybridAuth`, которая реализует авторизацию через множество соцсетей (провайдеров) без посредников;
- регистрация (в том числе с добавлением в группы) и авторизация пользователей;
- привязка нескольких соцсетей к одному пользователю;
- управление привязанными соцсетями из админки.

## Установка

После установки нужно зайти на страницу веб-пользователя в админке, чтобы создалась таблица.

Затем переименовать файл `config.sample` в `config.php` в папке `assets/plugins/hybridauth/config` и настроить в нем необходимые соцсети.

По настройке придется гуглить, но что-то можно найти в статье [Провайдеры HybridAuth](https://docs.modx.pro/components/hybridauth/providers/).

Также должен быть включен плагин `userHelper`.

Классы для основных провайдеров находятся в папке `assets/plugins/hybridauth/vendor/hybridauth/hybridauth/hybridauth/Hybrid/Providers/` и не требуют дополнительных действий.

В случае дополнительных провайдеров из папки `assets/plugins/hybridauth/vendor/hybridauth/hybridauth/additional-providers/` необходимо добавить в конфигурацию провайдера ключ `wrapper`.

Напримере Vkontakte:

```
"Vkontakte" => array(
        "enabled" => true,
        "keys" => array("id" => "", "secret" => ""),
        "wrapper" => array(
            'class'=>'Hybrid_Providers_Vkontakte',
            'path' => MODX_BASE_PATH.'assets/plugins/hybridauth/vendor/hybridauth/hybridauth/additional-providers/hybridauth-vkontakte/Providers/Vkontakte.php'
        )
    ),
```

Компонент состоит из плагина и сниппета. Cниппет выводит список доступных для использования соцсетей, а плагин обеспечивает взаимодействие с ними.

## Параметры плагина

| параметр                                | описание                          | умолч.           |
| --------------------------------------- | --------------------------------- | ---------------- |
| [registerUsers](#param_registerusers)   | разрешить регистрацию             |
| [debug](#param_debug)                   | включить отладку                  |                  |
| [rememberme](#param_rememberme)         | запомнить после авторизации       |                  |
| [cookieName](#param_cookiename)         | запомнить после авторизации       |                  |
| [cookieLifetime](#param_cookielifetime) | запомнить после авторизации       |                  |
| [redirectUri](#param_redirecturi)       | переход из соцсетей               |                  |
| [loginPage](#param_loginpage)           | переход после логина              | текущая страница |
| [logoutPage](#param_logoutpage)         | переход после выхода              | текущая страница |
| [groups](#param_groups)                 | куда добавить после регистрации   |                  |
| [userModel](#param_usermodel)           | класс для работы с пользователями | `modUsers`       |
| [tabName](#param_tabname)               | название вкладки в админке        |                  |

### <a name="param_registerusers"></a> registerUsers

Разрешает регистрацию новых пользователей.

Если запрещено, то авторизованный пользователей должен привязать в своем профиле соцсети и входить потом через них.

### <a name="param_debug"></a> debug

Включает режим отладки, сообщения пишутся как в лог MODX, так и в файл `assets/cache/ha/error.log`

### <a name="param_rememberme"></a> rememberme, <a name="param_cookiename"></a> cookieName, <a name="param_cookielifetime"></a> cookieLifetime

Параметры для запоминания пользователя после авторизации, описаны в документации `FormLister`

### <a name="param_redirecturi"></a> redirectUri

Адрес страницы, на которую пользователя возвращают из соцсетей, если не заполнять, то будет использоваться главная страница сайта; этот адрес должен совпадать с тем, что указывается в настройках приложений соцсетей

### <a name="param_loginpage"></a> loginPage

id страницы, на которую пользователь должен быть перенаправлен после авторизации.

_По умолчанию:_ если не заполнено, то текущая страница

### <a name="param_logoutpage"></a> logoutPage

id страницы, на которую пользователь должен быть перенаправлен после выхода.

_По умолчанию:_ если не заполнено, то текущая страница

### <a name="param_groups"></a> groups

Список групп, в которые нужно добавить пользователя после регистрации, в виде json-массива

### <a name="param_usermodel"></a> userModel

Класс для работы с пользователями.

_По умолчанию:_ `modUsers`

### <a name="param_tabname"></a> tabName

Заголовок вкладки на странице веб-пользователя в админке.

## Параметры сниппета

| параметр                                      | описание                        |
| --------------------------------------------- | ------------------------------- |
| [langDir](#param_langdir)                     | настройка лексиконов            |
| [lang](#param_lang)                           | настройка лексиконов            |
| [lexicon](#param_lexicon)                     | настройка лексиконов            |
| [tpl](#param_tpl)                             | шаблон списка провайдеров       |
| [providerTpl](#param_providertpl)             | шаблон провайдера               |
| [activeProviderTpl](#param_activeprovidertpl) | шаблон подключенного провайдера |
| [errorTpl](#param_errortpl)                   | шаблон сообщения об ошибке      |
| [registerCss](#param_registercss)             | подключить css                  |

### <a name="param_langdir"></a> langDir, <a name="param_lang"></a> lang, <a name="param_lexicon"></a> lexicon

Настройка лексиконов, см. документацию `FormLister`

### <a name="param_tpl"></a> tpl

Шаблон списка провайдеров, доступны плейсхолдеры `[+providers+]` и `[+error+]`; если значение пустое, то вернется массив с ключами providers и error

### <a name="param_providertpl"></a> providerTpl

Шаблон провайдера, доступны плейсхолдеры `[+url+]` (ссылка для выполнения действия), `[+classNames+]` (имена классов), `[+title+]` (название провайдера)

### <a name="param_activeprovidertpl"></a> activeProviderTpl

Шаблон подключенного провайдера, если не указывать, то используется значение `providerTpl`

### <a name="param_errortpl"></a> errorTpl

Шаблон сообщения об ошибке, доступен плейсхолдер `[+error+]`

### <a name="param_registercss"></a> registerCss

Подключает файл `assets/snippets/hybridauth/css/default.css`
