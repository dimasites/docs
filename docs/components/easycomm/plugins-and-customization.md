# Плагины и кастомизация

## Плагины (добавление полей)

easyComm позволяет вам добавить дополнительные поля к объекту ecMessage, в том случае, если стандартных полей мало.

Механизм работы идентичен таковому в miniShop2 (тот, который использовался в версии 2.2 и ранее).

Рассмотрим добавление полей **field1** и **field2**. Для этого необходимо:

1. Создать папку "myplugin" (можете задать любое имя) в 2 каталогах:
    - `/core/components/easycomm/plugins/myplugin/`
    - `/assets/components/easycomm/plugins/myplugin/`

2. В каталоге `/core/components/easycomm/plugins/myplugin/` создать файлы:

    ::: code-group

    ```php [index.php]
    <?php

    return array(
      'xpdo_meta_map' => array(
        'ecMessage' => require_once dirname(__FILE__) .'/ecmessage.map.inc.php'
      )
      ,'manager' => array(
        'ecMessage' => MODX_ASSETS_URL . 'components/easycomm/plugins/myplugin/ecmessage.js'
      )
    );
    ```

    ```php [ecmessage.map.inc.php]
    <?php

    return array(
      'fields' => array(
        'field1' => NULL,
        'field2' => NULL,
      )
      ,'fieldMeta' => array(
        'field1' => array(
          'dbtype' => 'varchar'
          ,'precision' => '50'
          ,'phptype' => 'string'
          ,'null' => true
          ,'default' => NULL
        ),
        'field2' => array(
          'dbtype' => 'varchar'
          ,'precision' => '50'
          ,'phptype' => 'string'
          ,'null' => true
          ,'default' => NULL
        )
      )
      ,'indexes' => array(

      )
    );
    ```

    :::

3. В каталоге `/assets/components/easycomm/plugins/myplugin/` создать файл:

    ::: code-group

    ```js [ecmessage.js]
    easyComm.plugin.myplugin = {
      getFields: function (config) {
        return {
          field1: { xtype: 'textfield', fieldLabel: _('ec_message_field1'), anchor: '99%' },
          field2: { xtype: 'textfield', fieldLabel: _('ec_message_field2'), anchor: '99%' },
        }
      }
      ,getColumns: function () {
        return {
          field1: { width:50, sortable:true, name: 'field1' },
          field2: { width:50, sortable:true, name: 'field2' }
        }
      }
    };
    ```

    :::

4. Создать поля в таблице `modx_ec_messages`.
5. Добавить записи в словари системы `ec_message_field1` и `ec_message_field2` (пространство имен easycomm).
6. В системных настройках `ec_message_grid_fields` и `ec_message_window_layout` прописать добавленные поля. Про это ниже.
7. Организовать работу с новыми полями на сайте, к примеру добавить в чанк с формой.
    ::: warning
    Не забудьте про параметр allowedFields сниппета ecForm, необходимо добавить новые поля в этот параметр.
    :::

## Кастомизация внешнего вида

Для управления отображением сообщений и цепочек в админке предусмотрены системные настройки:

- **ec_message_grid_fields** - список полей, доступных в таблице сообщений
- **ec_message_window_layout** - разметка окна редактирования сообщения
- **ec_thread_grid_fields** - список полей, доступных в таблице цепочек
- **ec_thread_window_fields** - список полей, доступных в окне редактирования цепочки

Те, что списки - это просто перечесление полей через запятую, например `thread, subject, date, user_name, user_email, user_contacts, rating, text, reply_author, reply_text, ip`.

Настройки **ec_message_window_layout** задается в более сложном формате. Значение по-умолчанию следующее:

```json
{
  "main": {
    "name": "main",
    "columns": {
      "column0": ["user_name", "user_email"],
      "column1": ["date","user_contacts"]
    },
    "fields": ["subject", "rating","text", "published" ]
  },
  "reply": {
    "name": "reply",
    "columns": { },
    "fields": ["reply_author", "reply_text", "notify", "notify_date"]
  },
  "settings": {
    "name": "settings",
    "columns": { },
    "fields": [ "thread",  "ip",  "extended"]
  }
}
```

Здесь мы располагаем поля на 3-х вкладках: main, reply, settings, а внутри вкладки main у нас еще есть 2 колонки. Механизм обработки этой настройки позволяет создать несколько вкладок и поместить поля на них, дополнительно поля можно разбить на колонки один раз на одной вкладке.

Если вы хотите добавить еще одну вкладку - не забудьте добавить ее заголовок в словари системы **ec_message_tab_XXX**.
