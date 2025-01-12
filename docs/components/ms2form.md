---
title: ms2form
description: Выводит форму для создания продукта minishop2 пользователем из фронтэнда
logo: https://modstore.pro/assets/extras/ms2form/logo-lg.jpg
author: gvozdb
modstore: https://modstore.pro/packages/users/ms2form
repository: https://github.com/me6iaton/ms2form

dependencies: miniShop2
---

# ms2form

Выводит форму для создания продукта minishop2 пользователем из фронтэнда.

![ms2form](https://file.modx.pro/files/c/3/a/c3a73249165844c116d0463006c6272c.png)

[Поcледние версии][releases]

## Возможности

- Создание продуктов minishop2 из фронтэнда.
- Редактирование существующих minishop2 продуктов с фронтенда, с проверкой прав.
- Поддержка нескольких редакторов [quill] и [bootstrap-markdown].
- Автоматическое создание новой категории в которой будет опубликован продукт, интеграция с msearch2 для автодополнения.
- Загрузка изображений в галерею продукта перетаскиванием, запись превью на диск, настройка параметров через источник файлов.
- Поддержка мультикатегорий
- Поддержка тегов
- Поддержка дополнительных TV
- Возможность выбора шаблона из фронтэнда.
- Фильтрация контента с помощью HTML Purifier.

## Системные настройки

| Название                   | По умолчанию                                     | Описание                                                                                                                                                                         |
|----------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ms2form_assets_url**     | `/assets/components/ms2form/`                    | Url к файлам фронтенда                                                                                                                                                           |
| **ms2form_core_path**      | `{core_path}components/ms2form/`                 | Путь к компоненту                                                                                                                                                                |
| **ms2form_frontend_css**   | `/assets/components/ms2form/css/web/ms2form.css` | Путь к файлу со стилями магазина. Если вы хотите использовать собственные стили - укажите путь к ним здесь, или очистите параметр и загрузите их вручную через шаблон сайта.     |
| **ms2form_frontend_js**    | `/assets/components/ms2form/js/web/ms2form.js`   | Путь к файлу со скриптами магазина. Если вы хотите использовать собственные скрипты - укажите путь к ним здесь, или очистите параметр и загрузите их вручную через шаблон сайта. |
| **ms2form_mail_bcc**       | `1`                                              | Укажите через запятую список **id** администраторов, которым нужно отправлять сообщения о создании нового ресурса.                                                               |
| **ms2form_mail_createdby** | `Да`                                             | Отправлять уведомление создателю ресурса                                                                                                                                         |
| **ms2form_mail_from**      |                                                  | Адрес для отправки почтовых уведомлений. Если не заполнен - будет использована настройка `emailsender`.                                                                          |
| **ms2form_mail_from_name** |                                                  | Имя, от которого будут отправлены все уведомления. Если не заполнен - будет использована настройка `site_name`.                                                                  |

## Параметры вызова сниппета

| Название              | По умолчанию                                                      | Описание                                                                                                                                                                                                     |
|-----------------------|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **allowFiles**        | `Да`                                                              | Разрешить пользователю загружать файлы на сервер.                                                                                                                                                            |
| **allowedFields**     | `parent, pagetitle, content, published, template, hidemenu, tags` | Поля продукта, которые разрешено заполнять пользователю. Можно указывать имена TV параметров.                                                                                                                |
| **editor**            | `quill`                                                           | Тип редактора контента. `0` - отключить редактор, `quill`                                                                                                                                                    |
| **parent**            |                                                                   | Id основной категории для публикации ресурса. Обязательно для заполнения                                                                                                                                     |
| **parentMse2form**    |                                                                   | Разрешить создавать новые категории и использовать форму Msearch2 для автодополнения. Json строка с параметрами сниппета mSearchForm [json]                                                                  |
| **parents**           |                                                                   | Список id, через запятую, ресурсов для поиска категорий в которых будет опубликован ресурс одновременно с основной категорией. По умолчанию выводятся все доступные категории с фронтенда, с проверкой прав. |
| **parentsIncludeTVs** |                                                                   | Список названий TV, через запятую, которые будут выводиться вместе с дополнительными категориями                                                                                                             |
| **parentsSortby**     | `pagetitle`                                                       | Поле для сортировки дополнительных категорий, можно использовать TV                                                                                                                                          |
| **parentsSortdir**    | `ASC`                                                             | Направление сортировки дополнительных категорий                                                                                                                                                              |
| **resources**         |                                                                   | Список id, через запятую, категорий в которых будет опубликован ресурс одновременно с основной категориией. Альтернатива parents                                                                             |
| **permissions**       | `section_add_children`                                            | Проверка прав на публикацию в раздел. По умолчанию проверяется разрешение `section_add_children`.                                                                                                            |
| **redirectPublished** | `new`                                                             | На какой документ отправлять пользователя при создании ресурса? new - на вновь созданный, `0` - не перенаправлять, `id` - на ресурс с указанным id                                                           |
| **redirectScheme**    | `-1`                                                              | Схема создания url для перенаправления [modx.makeurl]                                                                                                                                                        |
| **requiredFields**    | `parent, pagetitle, content`                                      | Обязательные поля ресурса, которые пользователь должен заполнить для отправки формы.                                                                                                                         |
| **source**            |                                                                   | Id источника медиа для загрузки файлов. По умолчанию будет использован источник, указанный в системной настройке `ms2_product_source_default`.                                                               |
| **template**          |                                                                   | id шаблона для публикации ресурса, эта настройка отключает настройку templates                                                                                                                               |
| **templates**         | `1`                                                               | Список id шаблонов для публикации ресурсов формата `1==Базовый,2==Дополнительный`, можно указать только один id шаблона, по умолчанию используется шаблон с id равным 1                                      |
| **tags**              | `true`                                                            | Разрешить или запретить вывод тегов                                                                                                                                                                          |
| **tagsNew**           | `true`                                                            | Разрешить или запретить добавлять новые теги                                                                                                                                                                 |
| **tplCreate**         | `tpl.ms2form.create`                                              | Чанк для создания нового ресурса                                                                                                                                                                             |
| **tplEmailBcc**       | `tpl.ms2form.email.bcc`                                           | Чанк для уведомления администраторов сайта о новом ресурсе.                                                                                                                                                  |
| **tplFile**           | `tpl.ms2form.file`                                                | Чанк оформления загруженного файла, который не является изображением.                                                                                                                                        |
| **tplFiles**          | `tpl.ms2form.files`                                               | Контейнер для вывода загрузчика и списка уже загруженных файлов.                                                                                                                                             |
| **tplImage**          | `tpl.ms2form.image`                                               | Чанк оформления загруженного изображения.                                                                                                                                                                    |
| **tplSectionRow**     | `tpl.ms2form.section.row`                                         | Чанк для оформления вывода дополнительной категории                                                                                                                                                          |
| **tplTagRow**         | `tpl.ms2form.tag.row`                                             | Чанк для оформления вывода тега                                                                                                                                                                              |
| **tplUpdate**         | `tpl.ms2form.update`                                              | Чанк для обновления существующего ресурса                                                                                                                                                                    |

## Использование компонента

Для создания нового товара вызовите не кешируемый сниплет ms2form с необходимыми параметрами.

Для редактирования существующего товара с помощью формы, на ресурс с формой должна вести ссылка
с `GET` параметром `?&pid=[id товара]`.

## Политики доступа

Все необходимы права находятся в политике доступа `ms2formUserPolicy`,
примените эту политику к группе пользователей чтобы у них была возможность пользоваться формой

## Способы вызова

*Сниппет вызывается не кэшированным.*

```modx
[[!ms2form?
  &parent=`54`
  &parentMse2form=`{"parents": "114"}`
  &parents=`54,58`
  &editor=`bootstrapMarkdown`
  &templates=`1==Базовый,2==Дополнительный`
  &allowedFields=`parent,pagetitle,content,published,template,hidemenu,tags,tv1`
  &requiredFields=`parent,pagetitle,content`
]]
```

Свойства ресурса и дополнительные поля нужно добавить в параметр `allowedFields` и в чанки:

::: code-group

```modx [tpl.ms2form.create]
<input type="hidden" name="hidemenu" value="0"/>

<div class="form-group">
  <label>[[%ms2form_pagetitle]]</label>
  <span class="text-danger">*</span>
  <input type="text" class="form-control" placeholder="[[%ms2form_pagetitle]]" name="pagetitle" value="" maxlength="50" id="ms2formPagetitle"/>
</div>

<div class="form-group">
  <label>Пример TV </label>
  <br/>
  <input type="text" name="tv1" class="form-control">
</div>
```

```modx [tpl.ms2form.update]
<input type="hidden" name="hidemenu" value="0"/>

<div class="form-group">
  <label>[[%ms2form_pagetitle]]</label>
  <input type="text" class="form-control" placeholder="[[%ms2form_pagetitle]]" name="pagetitle" value="[[+pagetitle]]" maxlength="50" id="ms2form-pagetitle"/>
</div>

<div class="form-group">
  <label>Пример TV </label>
  <br/>
  <input type="text" value="[[+tv1]]" name="tv1" class="form-control">
</div>
```

:::

Внешний вид дополнительных TV и полей ресурса кроме шаблона, тегов, мультикатегорий, редактора контента
и галереи нужно определять вручную с помощью сторонних скриптов или использовать
скрытое поле:

```html
<input type="hidden" name="hidemenu" value="0"/>
```

## Разработка дополнения

О предложениях и ошибках в работе ms2Form сообщайте на [Github][issues].

[quill]: http://quilljs.com/
[bootstrap-markdown]: http://www.codingdrama.com/bootstrap-markdown/
[modx.makeurl]: https://docs.modx.com/current/en/extending-modx/modx-class/reference/modx.makeurl
[releases]: https://github.com/me6iaton/ms2form/releases
[issues]: https://github.com/me6iaton/ms2form/issues
