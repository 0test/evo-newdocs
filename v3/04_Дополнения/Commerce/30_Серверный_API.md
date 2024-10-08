# Серверный API

Содержание:

- [Корзина, общее](#cart)
- [Корзина, товары](#cart_product)
- [Заказ](#order)
- [Платёж](#payment)
- [Доставка](#delivery)

## <a name="cart"></a> Корзина, общее

Получить корзину товаров в своем коде довольно просто:

```php
// получить объект корзины:
$cart = $modx->commerce->getCart();
// или так:
$cart = ci()->carts->getCart('products');

// получить товары из корзины:
$items = $cart->getItems();
```

При создании корзины можно передать ее название вторым аргументом, и зарегистрированное хранилище - третьим:

```php
new Commerce\Carts\ProductsCart($modx, $instance = 'products', $store = 'session');
```

`Commerce` по умолчанию использует корзину товаров и списки сравнения и избранного. Любой из них может быть заменен в момент инициализации, например:

```php
if ($modx->event->name == 'OnInitializeCommerce') {
    $cart = new Commerce\Carts\ProductsCart($modx);
    ci()->carts->addCart('products', $cart);
}
```

По умолчанию товары в корзине хранятся в сессии пользователя, списки сравнения и избранного - в `cookies` (только идентификаторы товаров). Способ хранения также может быть изменен при инициализации:

```php
if ($modx->event->name == 'OnInitializeCommerce') {
    class DatabaseCartStore extends Commerce\Carts\SessionCartStore {
        // ...
    }

    ci()->carts->registerStore('database', DatabaseCartStore::class);

    $cart = new Commerce\Carts\ProductsCart($modx, 'products', 'database');
    ci()->carts->addCart('products', $cart);
}
```

### setTitleField

Задание имени поля для использования в качестве названия товаров.

```php
$cart->setTitleField($field);
```

### setPriceField

Задание имени поля для использования в качестве цены товаров

```php
$cart->setPriceField($field);
```

### getSubtotals

Расчет подитогов и итоговой стоимости товаров в корзине

```php
$cart->getSubtotals(array &$rows, &$total);
```

Вызывается событие `OnCollectSubtotals`, подитоги записываются в `$rows`, итоговая сумма - в `$total`.

## <a name="cart_product"></a> Корзина, товары

| Команда                          | Описание                             |
| -------------------------------- | ------------------------------------ |
| [add](#cart_add)                 | Добавить товар в корзину             |
| [addMultiple](#cart_addmultiple) | Добавить несколько товаров           |
| [update](#cart_update)           | Обновить корзину                     |
| [remove](#cart_remove)           | Удалить товар из корзины по названию |
| [removeById](#cart_removebyid)   | Удалить товар из корзины по id       |
| [clean](#cart_clean)             | Очистить корзину                     |

### <a name="cart_add"></a> add

Добавление товара в корзину

```php
$cart->add(array $item, $isMultiple = false);
```

В массиве `$item` передается информация о товаре, должны быть как минимум `id` и `count`. Параметр `isMultiple` означает множественное добавление, если равен `true`, в случае успеха не вызывается событие `OnCartChanged`.

### <a name="cart_addmultiple"></a> addMultiple

Пакетное добавление товаров в корзину

```php
$cart->addMultiple(array $items = []);
```

### <a name="cart_update"></a> update

Изменение товара в корзине

```php
$cart->update($row, array $attributes = [], $isAdded = false);
```

В параметре `$row` передается символьный идентификатор строки в корзине, в `$attributes` - поля, которые нужно изменить. `$isAdded` означает, было ли действие изначально <i>добавлением</i> товара или нет.

### <a name="cart_remove"></a> remove

Удаление товара из корзины

```php
$cart->remove($row);
```

Удаляет товар из корзины по символьному идентификатору строки.

### <a name="cart_removebyid"></a> removeById

Удаление товаров по идентификатору товара

```php
$cart->removeById($id);
```

Удаляет товары из корзины по уникальному идентификатору товара/ресурса в дереве.

### <a name="cart_clean"></a> clean

Очистка корзины

```php
$cart->clean();
```

## <a name="order"></a> Заказ

Для получения обработчика заказа в своем коде нужно вызвать специальный метод Commerce:

```php
$processor = $modx->commerce->loadProcessor();
```

| Команда                                   | Описание                  |
| ----------------------------------------- | ------------------------- |
| [createOrder](#order_createorder)         | создать заказ             |
| [addOrderHistory](#order_addorderhistory) | добавить в историю заказа |
| [changeStatus](#order_changestatus)       | изменить статус           |
| [updateOrder](#order_updateorder)         | обновить заказ            |
| [deleteOrder](#order_deleteorder)         | удалить заказ             |
| [getOrder](#order_getorder)               | получить текущий заказ    |
| [loadOrder](#order_loadorder)             | загрузить заказ           |
| [loadOrderByHash](#order_loadorderbyhash) | загрузить заказ по хешу   |
| [isOrderStarted](#order_isorderstarted)   | начато ли оформление      |
| [updateRawData](#order_updaterawdata)     | обновить сырые данные     |

### <a name="order_createorder"></a> createOrder

Создание заказа.

```php
$order = $processor->createOrder(array $items, array $fields);
```

### <a name="order_addorderhistory"></a> addOrderHistory

Добавление пункта в историю заказа.

```php
$processor->addOrderHistory($order_id, &$status_id, &$comment = '', &$notify = false);
```

### <a name="order_changestatus"></a> changeStatus

Изменение статуса заказа.

```php
$processor->changeStatus($order_id, $status_id, $comment = '', $notify = false, $template = null);
```

### <a name="order_updateorder"></a> updateOrder

Изменение данных заказа.

```php
$processor->updateOrder($order_id, $data = []);
```

### <a name="order_deleteorder"></a> deleteOrder

Удаление заказа.

```php
$processor->deleteOrder($order_id);
```

### <a name="order_getorder"></a> getOrder

Получение текущего заказа.

```php
$processor->getOrder();
```

### <a name="order_loadorder"></a> loadOrder

Получение заказа по идентификатору.

```php
$processor->loadOrder($order_id, $force = false);
```

### <a name="order_loadorderbyhash"></a> loadOrderByHash

Получение заказа по хэшу.

```php
$processor->loadOrderByHash($order_hash);
```

### <a name="order_isorderstarted"></a> isOrderStarted

Начато ли оформление заказа.

```php
$processor->isOrderStarted();
```

### <a name="order_updaterawdata"></a> updateRawData

Обновление сырых данных заказа (используется в процессе заполнения формы).

```php
$processor->updateRawData($data);
```

## <a name="payment"></a> Платёж

| Команда                                                   | Описание                       |
| --------------------------------------------------------- | ------------------------------ |
| [payOrderByHash](#payment_payorderbyhash)                 | создание платежа по хешу       |
| [loadPayment](#payment_loadpayment)                       | загрузить платёж               |
| [loadPaymentByHash](#payment_loadpaymentbyhash)           | загрузить платёж по хешу       |
| [createPayment](#payment_createpayment)                   | создать платёж                 |
| [savePayment](#payment_savepayment)                       | сохранить платёж               |
| [getOrderPaymentsAmount](#payment_getorderpaymentsamount) | получить сумму платежей заказа |
| [getCurrentPayment](#payment_getcurrentpayment)           | получить текущий способ оплаты |

### <a name="payment_payorderbyhash"></a> payOrderByHash

Создание платежа для заказа по хэшу платежа.

```php
$processor->payOrderByHash($hash);
```

### <a name="payment_loadpayment"></a> loadPayment

Получение платежа по идентификатору.

```php
$processor->loadPayment($payment_id);
```

### <a name="payment_loadpaymentbyhash"></a> loadPaymentByHash

Получение платежа по хэшу.

```php
$processor->loadPaymentByHash($hash);
```

### <a name="payment_createpayment"></a> createPayment

Создание платежа.

```php
$processor->createPayment($order_id, $amount);
```

### <a name="payment_savepayment"></a> savePayment

Сохранение данных платежа.

```php
$processor->savePayment($payment);
```

### <a name="payment_getorderpaymentsamount"></a> getOrderPaymentsAmount

Получение суммы всех платежей для заказа.

```php
$processor->getOrderPaymentsAmount($order_id);
```

### <a name="payment_getcurrentpayment"></a> getCurrentPayment

Получение текущего способа оплаты.

```php
$processor->getCurrentPayment();
```

## <a name="delivery"></a> Доставка

| Команда                                            | Описание                         |
| -------------------------------------------------- | -------------------------------- |
| [getCurrentDelivery](#delivery_getcurrentdelivery) | получить текущий способ доставки |

### <a name="delivery_getcurrentdelivery"></a> getCurrentDelivery

Получение текущего способа доставки

```php
$processor->getCurrentDelivery();
```
