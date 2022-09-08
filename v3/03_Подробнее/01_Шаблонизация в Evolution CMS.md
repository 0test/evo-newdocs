# Шаблонизация в Evolution CMS #
Тут прям подробно и с примерами, 

Блейд


### Оператор If ### 
```
@if (count($records) === 1)
    Один
@elseif (count($records) > 1)
    Много
@else
   Пусто
@endif
```

### Оператор isset ### 
```
@isset($records)
    // Переменная $records определена и не равна `null` ...
@endisset

@empty($records)
    // Переменная $records считается «пустой» ...
@endempty
```

###  Операторы Switch ### 
```html
@switch($i)
    @case(1)
        Первое
        @break
    @case(2)
        второе
        @break
    @default
        По-умолчанию
@endswitch
```

###  Циклы ### 
```html
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor
```

```html
@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach
```

```html
@forelse ($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse
```

```html
@while (true)
    <p>I'm looping forever.</p>
@endwhile
```