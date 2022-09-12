# DLCrumbs #

Сниппет для создания навигации breadcrumbs с помощью DocLister. Вы можете использовать любой из параметров DocLister в вызове DLCrumbs.

Автор: <a href="https://github.com/AgelxNash" rel="nofollow" target="_blank">Agel_Nash</a>

## Параметры ##

### id ###
ID текущей страницы.

Значение по умолчанию:

```
$modx->documentIdentifier
```
### hideMain ###
Установите значение 1, если необходимо спрятать ссылку на домашнию страницу.

Значение по умолчанию: 0.

### showCurrent ###
Установите значение 1, чтобы включить текущую страницу.

Значение по умолчанию: 0.

### minDocs ###
Этот параметр определяет минимальное количество отображаемых элементов.

Значение по умолчанию: 0.


<h2 class="page-header">Шаблоны</h2>
### tpl ###
Шаблон вывода крошки.

Значение по умолчанию: 

```
@CODE:<li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem"><meta itemprop="position" content="[+iteration+]" />
	<a href="[+url+]" title="[+e.title+]" itemprop="item">
		<span itemprop="name">[+title+]</span>
	</a>
</li>
```
### tplFirst ###
Шаблон вывода первого пункта.

Значение по умолчанию: нету

Пример шаблона для домашней страницы

```
@CODE:<li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem" class="home-link">
	<meta itemprop="position" content="[+iteration+]" />
	<a href="[+url+]" title="[+longtitle+]" itemprop="item" class="icon icon-home"><i class="fa fa-home"></i></a>
</li>
```
### tplCurrent ###
Шаблон вывода текущей страницы.

Значение по умолчанию:

```
@CODE:<li class="active" itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
	<meta itemprop="position" content="[+iteration+]" />
	<span itemprop="item">[+title+]</span>
</li>
```
### ownerTPL ###
Шаблон обертки.

Значение по умолчанию:

```
@CODE:<nav class="breadcrumbs">
	<ul class="breadcrumb" itemscope itemtype="http://schema.org/BreadcrumbList">
		[+crumbs.wrap+]
	</ul>
</nav>
```
