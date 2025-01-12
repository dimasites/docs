---
author: alexsoin
head:
  - - link
    - rel: canonical
      href: https://zencod.ru/articles/moxi-settings/
---

# Кастомизация

Данный инструмент можно изменить под индивидуальные задачи. Для этого скопируем себе репозиторий проекта и приступим к кастомизации.

Открываем папку `src`, в ней находятся папки `content`(сами файлы) и `data`(данные для настройки).

Всё что помещается в папку `src/content/core` скопируется в папку `core` на сайте.

```bash
./src/
├── content/
│   ├── core/
│   │   └── elements/
│   │       ├── chunks/
│   │       ├── templates/
│   │       └── zoomx/
│   │           ├── controllers/
│   │           ├── plugins/
│   │           ├── snippets/
│   │           └── templates/
│   ├── pages/
│   ├── plugins/
│   ├── snippets/
│   └── templates/
└── data/
    ├── addons.php
    ├── clientConfig.php
    ├── plugins.php
    ├── providers.php
    ├── resources.php
    ├── settings.php
    ├── snippets.php
    ├── templates.php
    └── tvs.php
```

## Дополнения

Для редактирования списка дополнений открываем файл `src/data/addons.php`. В нём видим массив с провайдерами и дополнениями:

::: code-group

```php [src/data/addons.php]
<?php
return [
  "modx.com" => [
    'FormIt',
    'ClientConfig',
    // ...
  ],
  "modstore.pro" => [
    'Ace',
    'autoRedirector',
    'pdoTools',
    // ...
  ]
];
```

:::

Для изменения списка дополнений ищем нужное нам дополнение на сайте провайдера (например на [modstore.pro](https://modstore.pro/)), копируем название дополнения и добавляем по аналогии с остальными дополнениями в массив к нужному провайдеру.

## Системные настройки

Для редактирования списка дополнений открываем файл `src/data/settings.php`. В нём видим массив с ключами системных настроек и их значениями:

::: code-group

```php [src/data/settings.php]
<?php
return [
  'log_deprecated' => 0,
  'locale' => 'ru_RU.utf8',
  'allow_multiple_emails' => 0,
  'server_protocol' => 'http',
  'friendly_alias_realtime' => 1,
  'friendly_alias_restrict_chars' => 'alphanumeric',
  'friendly_alias_translit' => 'russian-fixed',
  // ...
];
```

:::

Для того чтобы настройка была со значением `Нет` записываем в её значение `0`, соответственно если нужно `Да` то ставим `1`.

Если нужной настройки нет в списке, то открываем на сайте системные настройки, копируем название ключа, добавляем в массив и указываем нужное значение.

![settings-manager](https://file.modx.pro/files/2/6/9/269df522808381d4a5532aca0c278a1e.png)

## Плагины/Сниппеты

Для редактирования списка дополнений открываем файл `src/data/plugins.php`. В нём видим массив плагинов:

::: code-group

```php [src/data/plugins.php]
<?php
return [
  'ignore' => [
    'name' => 'ignore',
    'description' => 'Обертывание выводимых данных в тег ignore',
    'events' => [
      'pdoToolsOnFenomInit' => []
    ]
  ],
  // ...
];
```

:::

Ключами являются названия контента плагина, размещенного в папке `src/content/plugins/` без расширения `.php`.

![plugins](https://file.modx.pro/files/5/8/9/589d5587407a00572a3e55b793cc3c18.png)

Если нужно добавить плагин, то создаём по аналогии с другими элементами массива новый плагин, в качестве ключа указываем название файла контента плагина, в `name` указываем название плагина, в `description` добавляем описание, либо оставляем пустым, в массив `events` добавляем события на которые подписываем плагин. Затем создаём в папке `src/content/plugins/` файл с расширением `.php` с контентом нового плагина.

Со сниппетами всё точно также:

::: code-group

```php [src/data/snippets.php]
<?php
return [
  'version' => [
    'name' => 'version',
    'description' => 'Вывод гет параметра времени создания у подключаемого скрипта/стиля',
  ]
];
```

:::

## ClientConfig

Для редактирования списка дополнений открываем файл `src/data/clientConfig.php`. В нём видим массив категориями и её элементами:

::: code-group

```php [src/data/clientConfig.php]
<?php
return [
  [
    'label' => 'Основное',
    'description' => '',
    'items' => [
      ['key' => 'policy', 'xtype' => 'modx-panel-tv-file', 'label' => 'Политика конфиденциальности', 'value' => '#'],
      ['key' => 'year_start', 'xtype' => 'numberfield', 'label' => 'Год начала в копирайте', 'value' => date('Y')],
      ['key' => 'emailto', 'xtype' => 'textfield', 'label' => 'E-mail для заявок', 'value' => ''],
    ],
  ],
  // ...
];
```

:::

В `label` указываем название вкладки категории, в `items` добавляем список полей данной вкладки.

- `xtype` - тип поля
- `key` - ключ по которому будет вызываться данное поле
- `label` - название поля
- `value` - значение поля(можно оставить пустым)

## Добавление кастомных операций

Операция - это функция запускаемые при настройке нового сайта, например установка списка дополнений или добавление плагинов.

Чтобы добавить свою операцию, которой нет в списке шагов в функции `steps` нужно добавить новую функцию в класс `MoxiPack`, находящийся в файле `./app.php` назовём её `exampleStep`:

::: code-group

```php [app.php]
<?php
// ...
class MoxiPack extends MoxiModx
{
  // ...

  /**
   * Пример кастомной операции
   *
   * @return void
   */
  public function exampleStep()
  {
    $this->log("Кастомная операция завершена");
  }

  // ...
}
```

:::

В данной функции доступен экземпляр объекта `modx` через обращение к `$this->modx`.

Также для логирования операции доступна функция `$this->log` принимающая текст сообщения и уровень лога, по умолчанию уровень `info`, также доступны уровни `error` и `warning`.

Например, для записи в лог сообщения об ошибки нужно вызвать функцию следующим образом:

```php
$this->log("Текст ошибки", "error");
```

Затем, чтобы данная операция отработала при настройке, её нужно добавить в массив вызовов шагов операций, находящимся в функции `steps` класса `MoxiHelp` в файле `./app.php`:

::: code-group

```php [app.php]
<?php
// ...
class MoxiHelp
{
  // ...

  /**
   * Возвращает массив порядка выполнения операций.
   *
   * @return array Массив порядка выполнения операций, включает в себе имя и описание.
   */
  static function steps()
  {
    return [
      ["name" => "providers", "desc" => "Добавление поставщиков дополнений", ],
      ["name" => "addons", "desc" => "Установка дополнений", ],
      ["name" => "copyCore", "desc" => "Копирование папки core", ],
      ["name" => "templates", "desc" => "Добавление шаблонов", ],
      ["name" => "resources", "desc" => "Добавление ресурсов", ],
      ["name" => "settings", "desc" => "Изменение настроек", ],
      ["name" => "snippets", "desc" => "Добавление сниппетов", ],
      ["name" => "plugins", "desc" => "Добавление плагинов", ],
      ["name" => "tvs", "desc" => "Добавление дополнительных полей", ],
      ["name" => "clientConfig", "desc" => "Настройка clientConfig", ],
      ["name" => "exampleStep", "desc" => "Кастомная операция", ], // [!code ++]
      ["name" => "managerCustomize", "desc" => "Настройка панели администрирования", ],
      ["name" => "renameHtaccess", "desc" => "Переименовывание ht.access в .htaccess", ],
      ["name" => "removeChangelog", "desc" => "Удаление changelog", ],
      ["name" => "clearCache", "desc" => "Очистка кэша", ],
    ];
  }

  // ...
}
```

:::

- `name` - название функции, которая будет запущена
- `desc` - описание данной функции

Операции из данного массива выполняются поочередно начиная с самого первого, если кастомная операция должна выполнятся после окончания какого-либо события, то её нужно добавлять после данной операции. В данном примере `exampleStep` выполнится после операции `clientConfig`.

На этом добавление кастомной операции закончено.
