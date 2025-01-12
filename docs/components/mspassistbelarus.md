---
title: mspAssistBelarus
description: Оплата заказов miniShop2 через Assist Belarus (Республика Беларусь)
logo: https://modstore.pro/assets/extras/mspassistbelarus/logo-lg.jpg
author: alroniks
modstore: https://modstore.pro/packages/payment-system/mspassistbelarus

dependencies: miniShop2
categories: payment
---

# mspAssistBelarus

## Начало работы

[Скачать модуль mspAssistBelarus][1] можно в Магазине MODX Дополнений Simple Dream.

Для того, чтобы принимать оплату с помощью [Assist Belarus][2] ([belassist.by][2]), Вам необходимо сначала подать [заявку на регистрацию][3].

Помощь по интеграции модуля оплаты на сайт вы можете получить через [службу технической поддержки][4] либо напрямую у [разработчика в Минске][5].

## Настройка модуля оплаты Assist Belarus

_**Важно! Для проверки механизма платежей пользуйтесь только тестовым окружением.**_

Для успешной работы с платежной системой вам понадобится логин и пароль, выданные после регистрации. Для начала необходимо зайти в [панель управления][6] и произвести первичную настройку системы.

[![](https://file.modx.pro/files/2/4/c/24cfc3cdef46304f202438acdeb5b0f4s.jpg)](https://file.modx.pro/files/2/4/c/24cfc3cdef46304f202438acdeb5b0f4.png)

После входа в панель управления необходимо перейти в «Настройки мерчантов» для настройки конкретного магазина.

На вкладке «**Настройки платежей**» имеется важный пункт «**Действие после авторизации**». В зависимости от выбранного варианта пользователь будет либо отправлен обратно на сайт, либо откроется страница с результатами платежа, откуда он тоже сможет вернуться на сайт, но уже по кнопке. В случае первого вариант необходимо вручную указать параметр «**URL_RETURN**», установив значение «site.ru/assets/components/minishop2/payment/assist.php?action=success», где **site.ru** — это адрес вашего сайта.

Я рекомендую выбирать вариант с кнопкой, тогда компонент оплаты сам позаботится о том, какой путь указать.

[![](https://file.modx.pro/files/c/8/c/c8c980536b2b8f2a036aa24429455d6es.jpg)](https://file.modx.pro/files/c/8/c/c8c980536b2b8f2a036aa24429455d6e.png)

Затем необходимо настроить уведомления о проводимых платежах на вкладке «**Настройка отправки результатов платежей**». Это нужно в той ситуации, когда человек совершил оплату, не забыл вернуться на ваш сайт. В таком случае сервис Assist Belarus уведомит ваш сайт о том, что платеж был обработан.

Нужно отметить пункт «**Отправка результатов оплат**», в парметр «**URL для отправки результатов**» указать ссылку вида «site.ru/assets/components/minishop2/payment/assist.php?action=notify», где **site.ru** — это адрес вашего сайта. Сервис позволяет уведомлять о различных изменениях статуса оплаты заказа, но в текущей реализации это актуально только для успешных оплат.

Тип протокола должен быть POST, тип подписи MD5. Секретное слово вы придумываете сами, оно будет использоваться при проверках платежей в дальнейшем.

[![](https://file.modx.pro/files/a/d/7/ad716f1dbd56463e26fac01968eb84b9s.jpg)](https://file.modx.pro/files/a/d/7/ad716f1dbd56463e26fac01968eb84b9.png)

Остальные настройки не должны влиять на работу модуля, но можно настроить кабинет под собственные нужды.

## Установка и настройка пакета в MODX

При установке компонента необходимо ввести реквизиты для начала работы с платежной системой Assist Belarus в MODX. Можно пропустить этот шаг и заполнить эти данные позже в системных настройках.

[![](https://file.modx.pro/files/2/3/2/2329bf0422e1f59b5c387299998ffeacs.jpg)](https://file.modx.pro/files/2/3/2/2329bf0422e1f59b5c387299998ffeac.png)

1. Идентификатор магазина (мерчанта)
2. Логин в системе Assist Belarus — специальный логин для работы с web-сервисами Assist Belarus, выданный после регистрации.
3. Пароль в системе Assist Belarus — пароль от панели управления Assist Belarus, выданный после регистрации.
4. Секретный ключ — придуманный вами сложный ключ, сохраненный в панели управления Assist Belarus.

Так же не забудьте после установке включить метод оплаты в настройках miniShop2 и добавить его к необходимому методу доставки.

[![](https://file.modx.pro/files/1/2/e/12ec4aee8a3154dec4fe5f0fa5183c91s.jpg)](https://file.modx.pro/files/1/2/e/12ec4aee8a3154dec4fe5f0fa5183c91.png)
[![](https://file.modx.pro/files/2/d/f/2df9d57ef3e8595f825a59a2597aadd6s.jpg)](https://file.modx.pro/files/2/d/f/2df9d57ef3e8595f825a59a2597aadd6.png)

Чтобы перевести режим оплат из тестового в «боевой», измените системную настройку **ms2_payment_assistbelarus_developer_mode** с «Да» на «Нет».

## Справочник по системным настройкам

| Ключ                                             | Название                                                   | Значение по умолчанию | Описание                                                                                                                                                                                                                      |
| ------------------------------------------------ | ---------------------------------------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ms2_payment_assistbelarus_merchant_id**        | Идентификатор магазина (мерчанта) в системе Assist Belarus |                       | Cодержит уникальный идентификатор магазина (мерчанта). Данный идентификатор создается при регистрации в системе Assist Belarus и высылается в письме.                                                                         |
| **ms2_payment_assistbelarus_secret_key**         | Секретный ключ                                             |                       | Последовательность случайных символов, задаваемая в панели Assist Belarus (Настройки мерчанта -> Настройка отправки результатов платежей). Участвует в формировании электронной подписи и используется для проверки платежей. |
| **ms2_payment_assistbelarus_login**              | Логин в системе Assist Belarus                             |                       | Специальный логин для работы с web-сервисами. Нужен для проверки платежа.                                                                                                                                                     |
| **ms2_payment_assistbelarus_password**           | Пароль в системе Assist Belarus                            |                       | Пароль, с которым вы входите в панель управления Assist Belarus. Нужен для проверки платежа.                                                                                                                                  |
| **ms2_payment_assistbelarus_checkout_url**       | Адрес для выполнения запросов                              |                       | Адрес, куда будет отправляться пользователь для выполнения оплаты заказа. Выдается тех. поддержкой Assist Belarus.                                                                                                            |
| **ms2_payment_assistbelarus_gate_url**           | Адрес для выполнения проверки платежа                      |                       | Адрес, куда будет отправляться запрос на проверку платежа. Выдается тех. поддержкой Assist Belarus.                                                                                                                           |
| **ms2_payment_assistbelarus_developer_mode**     | Режим совершения тестовых платежей                         | `Да`                  | При значении "Да", все запросы оплаты будут отправляться на тестовую среду обработки платежей Assist Belarus. При включении данного режима настройки checkout_url и gate_url игнорируются.                                    |
| **ms2_payment_assistbelarus_currency**           | Валюта платежа                                             | `USD`                 | Буквенный трехзначный код валюты согласно [ISO 4217][7]. Возможные варианты перечислены в разделе 5.8 документации.                                                                                                           |
| **ms2_payment_assistbelarus_language**           | Язык Assist Belarus                                        | `RU`                  | Укажите код языка, на котором показывать сайт Assist при оплате. Доступны варианты: **RU**, **EN**.                                                                                                                           |
| **ms2_payment_assistbelarus_success_id**         | Страница успешной оплаты Assist Belarus                    | `0`                   | Пользователь будет отправлен на эту страницу после завершения оплаты. Рекомендуется указать id страницы с корзиной, для вывода заказа.                                                                                        |
| **ms2_payment_assistbelarus_failure_id**         | Страница отказа от оплаты Assist Belarus                   | `0`                   | Пользователь будет отправлен на эту страницу при неудачной оплате. Рекомендуется указать id страницы с корзиной, для вывода заказа                                                                                            |
| **ms2_payment_assistbelarus_success_payment_id** | Статус заказа при успешной оплате                          | `2`                   | При успешной оплате заказа ему будет установлен указанный номер статуса. Сами статусы редактируются в настройках miniShop2.                                                                                                   |
| **ms2_payment_assistbelarus_cancel_payment_id**  | Статус заказа при отмененной оплате                        | `4`                   | При отмене оплаты заказа ему будет установлен указанный номер статуса. Сами статусы редактируются в настройках miniShop2.                                                                                                     |

[1]: https://modstore.pro/packages/payment-system/mspassistbelarus
[2]: http://belassist.by/
[3]: http://belassist.by/toclients/connect/register.php
[4]: https://modstore.pro/office/support
[5]: http://klimchuk.by/about.html
[6]: https://account.paysec.by/
[7]: http://www.iso.org/iso/home/standards/currency_codes.htm
