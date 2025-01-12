# События

Доступны следующие события:

- `PasOnBeforeChangeStatus`
- `PasOnChangeStatus` - смена статуса сущности
  - `instance` - сущность объект
  - `status` - идентификатор статуса
- `PasOnBeforeChangeTerm`
- `PasOnChangeTerm` - смена срока подписки
  - `subscription` - подписка объект
  - `action` - действие
  - `term` - срок
- `PasOnClientBeforeSave`
- `PasOnClientSave` - сохранение клиента
  - `client` - клиент объект
- `PasOnSubscriptionBeforeSave`
- `PasOnSubscriptionSave` - сохранение подписки
  - `subscription` - подписка объект
- `PasOnRateBeforeSave`
- `PasOnRateSave` - сохранение тарифа
  - `rate` - тариф объект
- `PasOnGetRateCost` - получение стоимости тарифа
  - `rate` - тариф объект
  - `data` - данные массив
- `PasOnContentBeforeSave`
- `PasOnContentSave` - сохранение контента
  - `content` - контент объект
- `PasOnGetContentRate` - получение тарифа контента
  - `content` - контент объект
  - `rate` - тариф объект
  - `data` - данные массив
- `PasOnBeforeAddToOrder`
- `PasOnAddToOrder` - добавление поля заказа
  - `key` - ключ поля
  - `value` - значение поля
- `PasOnBeforeValidateOrderValue`
- `PasOnValidateOrderValue` - валидация поля заказа
  - `key` - ключ поля
  - `value` - значение поля
- `PasOnBeforeRemoveFromOrder`
- `PasOnRemoveFromOrder` - удаление поля заказа
  - `key` - ключ поля
  - `order` - заказ объект
- `PasOnBeforeEmptyOrder`
- `PasOnEmptyOrder` - очистка заказа
  - `order` - заказ объект
- `PasOnBeforeGetOrderCost`
- `PasOnGetOrderCost` - получение стоимости заказа
  - `order` - заказ объект
  - `cost` - стоимость
- `PasOnSubmitOrder` - обработка заказа
  - `order` - заказ объект
  - `data` - данные массив
- `PasOnBeforeCreateOrder`
- `PasOnCreateOrder` - создание заказа
  - `msOrder` - заказ объект
  - `order` - заказ объект

## Примеры

Создать тариф для контента сроком 2 дня и стоимость 100 руб.

```php
<?php

switch ($modx->event->name) {

  case 'PasOnGetContentRate':
    /** @var PasContent $content */
    /** @var PasRate $rate */
    if ($content AND !$rate) {
      $rate = $modx->newObject('PasRate');
      $rate->fromArray(array(
        'content'    => $content->get('id'),
        'cost'       => '100',
        'term_value' => '2',
        'term_unit'  => 'd',
        'active'     => 1,
      ));
      $scriptProperties['rate'] = $rate;
    }

    break;
}
```
