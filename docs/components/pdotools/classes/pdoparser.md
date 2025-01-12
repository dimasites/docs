# pdoParser

pdoParser является заменой класса modParser.

## Обработка плейсхолдеров

Его задача - стараться быстро разобрать теги MODX без создания объектов, как это делает оригинальный парсер.
pdoParser умеет работать только с простыми тегами, без фильтров и условий, то есть:

- `[[%tag]]` - строка лексикона
- `[[~id]]` - ссылка
- `[[+tag]]` - обычные плейсхолдеры
- `[[++tag]]` - системные плейсхолдеры
- `[[*tag]]` - плейсхолдеры ресурса
- `[[#tag]]` - плейсхолдеры FastField

Специальные теги FastField [были предложены Виталием Киреевым](http://habrahabr.ru/post/161843/) в одноимённом дополнении.
[После согласованию с автором](https://github.com/argnist/fastField/issues/5), pdoParser был обучен работе с ними.

Он умеет:

- Выводить поля ресурсов: `[[#15.pagetitle]]`, `[[#20.content]]`
- Выводить ТВ параметры ресурсов: `[[#15.date]]`, `[[#20.some_tv]]`
- Выводить поля товаров miniShop2: `[[#21.price]]`, `[[#22.article]]`
- Выводить массивы ресурсов и товаров: `[[#12.properties.somefield]]`, `[[#15.size.1]]`
- Выводить глобальные массивы: `[[#POST.key]]`, `[[#SESSION.another_key]]`
- Распечатывать массивы для отладки: `[[#15.colors]]`, `[[#GET]]`, `[[#12.properties]]`

Цифра после решетки - это id ресурса, от которого нужно выбрать данные.

Все эти теги pdoTools обрабатывает без создания объектов modElement, поэтому работает немного быстрее чем родные методы MODX.
Если же плейсхолдер вызван с какими-то параметрами, то он уйдёт в родной modParser.

## Шаблонизатор Fenom

**С версии 2.0** в состав pdoTools входит [шаблонизатор Fenom](https://github.com/fenom-template/fenom/tree/master/docs/ru#readme) [анонс от автора на Хабре](http://habrahabr.ru/post/169525/).

Работает только при включенном pdoParser и если разрешен в системных параметрах.

### Возможности

- Компилируется в нативный PHP код, который выполняется гораздо быстрее, чем теги MODX. Прирост, в среднем **30% - 50%**.
- Может работать и в чанках и на страницах сайта
- Теги Fenom и MODX никак не мешают друг другу и работают одновременно
- Если в чанке нет плейсхолдеров MODX, то парсер MODX не запускается
- Если в чанке нет тегов Fenom, то он тоже не запускается
- Поддерживаются даже @INLINE чанки
- В отличии от других решений, вам не нужно никаким образом менять или переписывать свои сниппеты - всё работает через методы `pdoTools::getChunk()` и `pdoTools::parseChunk()` автоматически.
- Все ошибки компиляции пишутся в системный журнал

### Настройки

![Настройки](https://file.modx.pro/files/0/9/0/0902b411c53ef2417f09a03828820b69.png)

- **pdotools_fenom_default** включает обработку синтаксиса Fenom во всех чанках сайта.
- **pdotools_fenom_parser** включает обработку синтаксиса Fenom на страницах сайта. Контент ресурсов, шаблоны - везде. По умолчанию отключено.
- **pdotools_fenom_php** включает возможность выполнения произвольных функций PHP в шаблонах через `{$.php.функция()}`. Опция эта очень опасная, так что тоже отключена.
- **pdotools_fenom_modx** - чуть менее опасная опция, но во многих случаях, пока, необходимая - работа с объектами modX и pdoTools через переменные `{$modx}` и `{$pdoTools}`. Если вы не доверяете своим менеджерам - выключите её от греха подальше, потому что через объект modX можно удалить начисто весь сайт.
- **pdotools_fenom_cache** - включает кэшированние чанков (только чанков, не страниц сайта) через кэшер MODX (а не как раньше). Стоит использовать только на продакшн сайтах при больших и сложных чанках.

### Порядок запуска шаблонизатора

Если включен pdoParser и системная опция **pdotools_fenom_parser**, то шаблонизатор запускается ровно [вот здесь](https://github.com/modxcms/revolution/blob/6ab36a4742cde928e03a7ccf8d4e57190c70a08a/core/model/modx/modresponse.class.php#L83).

В этот момент все кэшированные чанки и сниппеты на странице обработаны (или загружены из кэша) и вы можете использовать вот такие конструкции:

```fenom
{if $.get.test == 1}
  [[!pdoResources?parents=`0`]]
{else}
  [[!pdoMenu?parents=`0`]]
{/if}
```

То есть, в зависимости от `$_GET['test']` на странице будет запущен **или** один сниппет **или** другой. Парсер MODX же запустил бы оба и результат выполнения одного неподходящего просто не показал.

Таким образом, вы можете реализовывать гораздо более сложную логику работы сайта даже с отключенными опциями `pdotools_fenom_php` и `pdotools_fenom_modx`. Понятное дело, что **вызов тегов Fenom на страницах сайта никак не кэшируется**.

Внутри чанков Fenom **всегда** выполняется в первую очередь, позволяя также разделять их содержимое для MODX, в зависимости от условий.

### Кэширование чанков Fenom

По умолчанию этот функционал Fenom отключен, потому что по моим тестам, толку от него в MODX нет. Но, это на моих мелких и простых чанках, а у вас может быть что-то посложнее.

Поэтому вы можете включить системную настройку **pdotools_fenom_cache** и тогда скомпилированные шаблоны будут сохранены в `/cache/default/fenom/` в зависимости от своего типа.

Чанки из БД кэшируются под своими id, а INLINE именуются как хэш от своего содержимого, то есть - путь к обычному чанку будет выглядеть как `cache/default/fenom/**chunk/90.cache.php`, а к INLINE уже как `cache/default/fenom/inline/35e115c27fdc3814b6f41a1015aa67e6.cache.php`.

Отсюда следует, что нормальные чанки из БД кэшируются намертво, и обновляются только при очистке системного кэша, а INLINE чанки при изменении контента сохраняются под новым именем и весь кэш чистить не нужно.

Как это работает дальше?

При первом запуске с пустым кэшем pdoTools получает нужный чанк, определяет его тип и отдаёт в Fenom.
Тот компилирует шаблон и сохраняет его во внутренний кэш pdoTools методом `setStore()`. Этот кэш находится в ОЗУ и сохраняется только на время выполнения скрипта, он нужен чтобы не компилировать 10 раз один и тот же чанк при выводе pdoResources.

А вот если включена опция `pdotools_fenom_cache`, то исходный код скомпилированного шаблона сохраняется на HDD сервера, и при следующем запуске Fenom уже не нужно его компилировать. Кэшер MODX отдаёт исходный код, из него получается объект `Fenom\Render` который передаётся в `setStore()` и оттуда уже работает.

Собственно, вопрос в том, что для вас будет быстрее - поднять скомпилированный шаблон из кэша, или скомпилировать его заново.

Обычно выходит, что на маленьких и простых чанках (как у сниппетов pdoTools) выигрыша нет, а лишних файлов много, а вот на больших и сложных чанках (которые вы наверняка создадите, используя возможности Fenom) разница уже может быть. Время компиляции и работы с кэшем выводится в ```&showLog=`1` ```, так что каждый может проверить сам.

### Примеры

Стандартный чанк `tpl.Tickets.comments.wrapper` из компонента Tickets

```modx
<div class="comments">
  [[+modx.user.id:isloggedin:is=`1`:then=`
  <span class="comments-subscribe pull-right">
    <label for="comments-subscribe" class="checkbox">
      <input type="checkbox" name="" id="comments-subscribe" value="1" [[+subscribed:notempty=`checked`]] />
      [[%ticket_comment_notify]]
    </label>
  </span>
  `:else=``]]

  <h4 class="title">[[%comments]] (<span id="comment-total">[[+total]]</span>)</h4>

  <div id="comments-wrapper">
    <ol class="comment-list" id="comments">[[+comments]]</ol>
  </div>

  <div id="comments-tpanel">
    <div id="tpanel-refresh"></div>
    <div id="tpanel-new"></div>
  </div>
</div>
```

Он же, переписанный для работы с Fenom

```fenom
<div class="comments">
  {if $modx->user->isAuthenticated($modx->context->key)}
    <span class="comments-subscribe pull-right">
      <label for="comments-subscribe" class="checkbox">
        <input type="checkbox" name="" id="comments-subscribe" value="1" {$subscribed != '' ? 'checked' : ''} />
        {$modx->lexicon('ticket_comment_notify')}
      </label>
    </span>
  {/if}

  <h4 class="title">{$modx->lexicon('comments')} (<span id="comment-total">{$total}</span>)</h4>

  <div id="comments-wrapper">
    <ol class="comment-list" id="comments">{$comments}</ol>
  </div>

  <div id="comments-tpanel">
    <div id="tpanel-refresh"></div>
    <div id="tpanel-new"></div>
  </div>
</div>
```
