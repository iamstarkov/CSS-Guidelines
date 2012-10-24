
# CSS рекомендации

Большие, долгоживущие проекты с большим количеством разработчиков требуют от нас выполнять нашу работу опреленным образом для достижения следующих целей:

* Сохранять стили легко поддерживаемыми
* Сохранять прозрачность и читаемость кода
* Сохранять стили масштабируемыми

Несколько техник помогут нам достичь этих целей.

Первая часть этого документа расскажет про синтаксис, форматирование и анатомию CSS, вторая часть раскроет темы метода, способа мышления и отношения к написанию и созданию CSS архитектуры. Звучит, многообещающе?

## Оглавление

* [Анатомия CSS документа](#анатомия-css-документа)
  * [Общие моменты](#общие-моменты)
  * [Один файл vs. много файлов](#один-файл-vs-много-файлов)
  * [Оглавление](#оглавление)
  * [Названия секций](#section-titles)
* [Организация правил](#организация-правил)
* [Анатомия CSS правила](#анатомия-css-правила)
* [Соглашения об именовании](#соглашения-об-именовании)
  * [Джаваскрипт классы](#джаваскрипт-классы)
* [Комментарии](#комментарии)
  * [Комментарии на стероидах](#комментарии-на-стероидах)
    * [Квази-селекторы](#квази-селекторы)
    * [Теги в коде](#теги-в-коде)
    * [Ссылки между компонентом и его расширением](#ссылки-между-компонентом-и-его-расширением)
* [Напиcание CSS](#напиcание-css)
* [Создание новых компонентов](#создание-новых-компонентов)
* [OOCSS](#oocss)
* [Разметка](#layout)
* [Единицы измерения](#единицы-измерения)
  * [Типографские единицы измерения](#font-sizing)
* [Сокращенная запись](#сокращенная-запись)
* [Идентификаторы](#идентификаторы)
* [Селекторы](#селекторы)
  * [Перенасыщеный селекторы](#перенасыщеный-селекторы)
  * [Производительность селекторов](#производительность-селекторов)
* [Ключевой элемент селектора](#ключевой-элемент-селектора)
* [`!important`](#important)
* [Магические числа и абсолютные значения](#магические-числа-и-абсолютные значения)
* [Conditional stylesheets](#conditional-stylesheets)
* [Отладка](#отладка)
* [Препроцессоры](#Препроцессор)

---

## Анатомия CSS документа

Мы всегда должны стараться сохранять принятое форматирование. Это означает согласованные комментарии, синтаксис и соглашение об именовании.

### Общие моменты

Ограничьте строки 80 символами, где это только возможно. Исключениями могут быть синтаксис градиентов и ссылки в комментариях. Это нормально, мы ничего не можем с этим сделать.

Я предпочитаю отступы в четыре пробела и мы пишем многострочный CSS.

### Один файл vs. много файлов

Некоторые предпочитают работать с одним большим файлом. Это хорошо и применяя данное руководство у вас не будет проблем. Начав использовать Sass, я начал разбивать мои стили на множество маленьких, подключаемых через `include`(TODO: проверить команду инклуда в SASS). Это тоже хорошо… Правила и рекомендации, приведённые ниже применимы к обоим способам. Единственное замечание относится к «Оглавлению» и «Заголовкам секций».(TODO: проверить соотносятся ли эти термины со структурой документа). Продолжайте читать, чтобы узнать подробности.

### Оглавление

В начале CSS файла я храню таблицу содержимого, которая уточняет какие секции содержатся в этом файле, например:

    /*------------------------------------*\
        $CONTENTS
    \*------------------------------------*/
    /**
     * CONTENTS............You’re reading it!
     * RESET...............Set our reset defaults
     * FONT-FACE...........Import brand font files
     */

Оглавление подскажет следующему разработчику, что ему можно ожидать в этом файле. Каждый пункт оглавления совпадает с названием секции. (TODO: ‘map’ перевод)

Если вы работаете с одним большим файлом стилей, то секция попавшая в оглавление также находится в этом файле. Если вы работаете со стилями разнесёнными в несколько файлов, то каждый пункт оглавления адресует к команде подключения файла с этой секцией.

### Названия секций

Оглавление будет бесполезно пока не будет соотноситься с названими секций. Оформляйте секцию таким способом:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

Знак `$` является префиксно дополняет название секции, позволяя искать по файлу паттерн `$[НАЗВАНИЕ-СЕКЦИИ]` и тем самым **поиск ограничивается только по названиям секций**.

Работая с большим файлом стилей, оставляейте отступ в пять строк между секциями

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Our
    reset
    styles]
    
    
    
    
    
    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

Этот участок пустого пространство легко и быстро бросается в глаза при пролистывании большого файла.

При работе с несколькими, подключаемыми файлами стилей, начинайте каждый файл с названия секции и в этом случае нет необходимости в пустых строках.

## Организация правил

Возьмите за правило писать стили в порядке специфичности. Это обеспечит гарантию того, что вы используете все преимущества вложенности и каскада.

Порядок правильно организованного файла стилей похож на это:

1. **Reset** – Платформа.
2. **Элементы** – `h1`, `ul` и другие общие селекторы.
3. **Объекты и абстракции** — Общие конструкции без дизайна.
3. **Объекты и абстракции** — generic, underlying design patterns.
4. **Компоненты** – компоненты построенные на объектах и их расширениях.
5. **Полезные стили** – состояния ошибок и другое.

Такая структура означает, что секция написанная ниже строится и наследует правила описанные в предыдущих секциях. Это приведёт в к меньшему количеству проблем специфичности и отменяющих переопределений, а также к более качественной CSS архитектуре.

For further reading I cannot recommend Jonathan Snook’s
[SMACSS](http://smacss.com) highly enough. (TODO: to translate)

## Анатомия CSS правила

    [Селектор] {
        [Свойство]:[Значение]
        [<-   Объявление  ->] (TODO: DECLARATION)
    }    

Я руководствуюсь несколькими правилами при написании правила.

* Дефис как разделитель (Исключая БЭМ запись,
  [смотрите ниже](#cоглашения-об-именовании))
* Отступ в 4 пробела
* Многострочность
* Свойства сортируются в порядке релевантности (**не** в алфавитном)
* Выравнивайте свойства с вендорными префиксами так, чтобы их значения были друг под другом.
* Используйте отступы в стилях, чтобы отразить структуру HTML.
* Всегда заканчивайте объявление знаком точки с запятой

И сразу пример:

    .widget{
        padding:10px;
        border:1px solid #BADA55;
        background-color:#C0FFEE;
        -webkit-border-radius:4px;
           -moz-border-radius:4px;
                border-radius:4px;
    }
        .widget-heading{
            font-size:1.5rem;
            line-height:1;
            font-weight:bold;
            color:#BADA55;
            margin-right:-10px;
            margin-left: -10px;
            padding:0.25em;
        }

Наглядно видно, что `.widget-heading` является дочерним элементом `.widget`, так как мы `.widget-heading` имеет дополнительный отступ относительное селектора `.widget`. Эта полезное информация позволяет разработчикам, которые теперь знают структуру DOM дерева просто взглянув на отступы в файле стилей.

Также мы видим, что объявления селектора `.widget-heading` отсортированы в порядке релевантности; `.widget-heading` вероятнее всего текстовый элемент, поэтому мы начинаем правило со типографских свойств, за которыми уже следуют все остальные.

Единственное исключение в многосточном CSS может быть в таком случае:

    .t10    { width:10% }
    .t20    { width:20% }
    .t25    { width:25% }       /* 1/4 */
    .t30    { width:30% }
    .t33    { width:33.333% }   /* 1/3 */
    .t40    { width:40% }
    .t50    { width:50% }       /* 1/2 */
    .t60    { width:60% }
    .t66    { width:66.666% }   /* 2/3 */
    .t70    { width:70% }
    .t75    { width:75% }       /* 3/4*/
    .t80    { width:80% }
    .t90    { width:90% }

В этом примере (из [сеточной системы inuit.css](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88))
гораздо больше смысла использовать однострочные правила.

## Соглашения об именовании

В большинстве случаев я использую дефис как разделитель слов в классах (например `.foo-bar`, не `.foo_bar` и не `.fooBar`), но в некоторых случаях я использую БЭМ запись.

<abbr title="Блок, Элемент, Модификатор">БЭМ</abbr> — Методология именования и категоризации CSS селекторов для приведения их в чёткий порядок, придания прозрачности и информативности.

Соглашение об именовании выражается следующим шаблоном:

    .block{}
    .block__element{}
    .block--modifier{}

* `.block` представляет собой наивысший уровень абстракции или компонента.
* `.block__element` является вложенным элементом `.block` формирующим `.block` как сущность.
* `.block--modifier` — класс отражающий другое состояние или версию `.block`.

**Аналогия** иллюстрирующая работу БЭМ может быть такой:

    .person{}
    .person--woman{}
        .person__hand{}
        .person__hand--left{}
        .person__hand--right{}

В этом примере приведен базовый объект описывающий человека, и существует отличный от базового тип человека — женщина. Также наглядно видно, что у людей есть руки; они являются частями человека, и рука может быть как левой, так и правой.

Теперь мы можем создавать селекторы опираясь на базовые объекты и также без проблем понимаем, что делает каждый конкретный селектор; является ли он частью компонента (`__`) или же модификацией (`--`)?

Итак, `.page-wrapper` является отдельным селектором; он не формирует часть компонента и значит является валидным селектором. В противовес этому, селектор `.widget-heading` относится к компоненту; он является дочерним элементом `.widget` и поэтому мы вынуждены переименовать этот класс в `.widget__heading`.

БЭМ непригляднее и перегружен, но тем не менее он предоставляет огромные возможности, с помощью которых мы можем определить назначение и отношения элементов просто посмотрев на их классы. Также, БЭМ обычно сжимается c помощью gzip, после использования минификаторов, которые первоклассно работают с повторениями.

Используете ли вы БЭМ или нет, всегда проверяйте наличие правильность классов; классы должны быть короткими, насколько это возможно, но настолько длинными, насколько это необходимо. Убедитесь, что абстракции имеют очень общие классы (например `.ui-list`, `.media`) для возможного переиспользования. Расширения абстракций должны иметь гораздо более конкретное классы (например `.user-avatar-link`). Не беспокойтесь о длинных классах; gzip выполнит свою работу потрясающе хорошо.

### Классы в HTML

Для большей читаемости разделяйте классы в разметке двумя (2) пробелами:

    <div class="foo--bar  bar__baz">

Увеличенные отступы между классами позволяют легко вычленять отдельные классы.

### Джаваскрипт классы

**Никогда не используйте обычные CSS классы для привязывания JavaScript логики.** Связывание логики скриптов с оформлением ведёт к тому, что мы не сможем использовать одно без другого.

Если вам требуется привязать к вёрстке какую-то логику используйте специальный JS класс — обычный класс, дополненный префиксом `.js-`, например `.js-toggle`, `.js-drag-and-drop`. Данный приём позволяет добавлять JS и CSS классы, без создания самому себе проблем в будущем, а также разделяет логику поведедения и оформление страницы друг от друга.

    <th class="is-sortable  js-is-sortable">
    </th>

Приведенная в пример разметка имеет два класса; один предназначен для оформления сортируемых табличных колонок и другой для того, чтобы вы могли добавить возможность сортировки.

## Комментарии

Я использую комментарии в стиле docBlock, которые я ограничиваю в 80 символов:

    /**
     * Пример комментария в стиле docBlock
     *
     * Более длинное описание комментария, описывающего код в больших
     * подробностях. Комментарии ограничиваются в 80 символов.
     * 
     * Можно вставлять разметку в комментарии. Делается это так:
     * 
       <div class=foo>
           <p>Lorem</p>
       </div>
     * 
     * Разметка префиксно не дополняется звездочками, чтобы оставить возможность
     * легкого копипаста.
     * 
     */

Вы должны документировать и комментирвоать так много, насколько вы можете это делать; всё, что кажется вам прозрачным и само-объясняемым может не быть таковым для другого разработчика. Пишите кусок кода и сразу после этого объясняйте его.

### Комментарии на стероидах

Существуют несколько продвинутых техник, связанных с комментариями:

* Квази-селекторы
* Теги в коде
* Ссылка на расширяемый компонент

#### Квази-селекторы

Вы никогда не должны перегружать селектор; скажем так, вы никогда не должны писать `ul.nav{}`, если вы можете написать просто `.nav{}`. Перегрузка селекторов приводит к уменьшению производительности селекторов (увеличению времени рендеринга страницы), исключает возможность потенциального переиспользования селектора для элемента другого типа и увеличивает специфичность селектора. Это все те вещи, которые должны избегаться при любых условиях.

Иногда бывает полезно сообщить следующему разработчику, в каком контексте вы предполагаете должен использовать селектор. Давайте рассмотрим `.product-page`; Этот класс выглядит так, как будто должен использоваться на корневых контейнерах, таких как `html` и `body`, но селектор `.product-page` не имеет возможности сказать на какой именно контейнер он был использован.

Используя смешанные селекторы (например, комментирование первого простого селектора по типу), мы можем сообщить на какой контейнер следует использовать этот класс:

    /*html*/.product-page{}

В этом примере мы чётко видим на какой элемент применён данный класс, без проблем со специфичносью и переиспользованием этого кода;

Другие примеры могут быть такими:

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}


Мы можем увидеть к какому типу элемента применён класс и также недопустили увеличения специфичности селектора.

#### Теги в коде

При написаниии нового компонента, оставляйте теги основнные на функиональности компонента:

    /**
     * ^navigation ^lists
     */
    .nav{}
    
    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

Теги позволяют разаработчикам находить сниппеты поиском по названию тега. Если 
разработчику понадобится поработать со списками, он может начать искать `^color` и найдёт объекты `.nav` и `.matrix`

#### Ссылки между компонентом и его расширением

Используя методы OOCSS, вы часто будете иметь два участка кода (один — скелет приложения и второй — темизация) и при этом расширение (TODO: extension в терминах ООЦСС) сильно связано с классом-родителем; но часто бывает так, что они находятся в разных файлах. Для установления чёткой связи между объектом и его расширением мы применяем <i>ссылки между компонентом и его расширением</i>. Данные ссылки являются просто комментариями.

В вашей основном файле стилей:

Эти теги позволят другим разработчикам искать код, используя сниппеты как этот:

    /**
     * Extend `.foo` in theme.css
     * (Дополнен классом `.foo` в theme.css) (TODO: translate)
     */
     .foo {}

In your theme stylesheet:

    /**
     * Extends `.foo` in base.css
     * (Дополняет класс `.foo` из base.css)
     */
     .bar{}

Тем самым мы получили устойчивую связь между двумя связаными логически, но физически разбитыми на части кусками кода.

---

## Напиcание CSS

Предыдущая часть рассказывает какую соблюдать структуру и как писать наш CSS.

Предыдущая часть рассказывает как структурировать и формировать наш CSS; здесь существуют более-менее чёткие правила. Следущая глава будет немного более теоретической и повествует про способ мышления и подход. (TODO: attitude and approach)

## Создание новых компонентов

При создании нового компонента пишите разметку **до того**, как напишите хоть одну строчку CSS. Это позволяет увидеть какие свойства наследовались и избежать повторного применения избыточных стилей.

Если вы будете писать сначала разметку, то вы сможете сфокусироваться на информации, контенте и семантике и **только после** этого применить необходимые стили.

## OOCSS

Я использую OOCSS подход; Я разделяю компоненты на структуру (объекты) и оформления (расширения). Как **аналогия** (не пример) можно рассмотреть следующий сниппет:

    .room{}
    
    .room--kitchen{}
    .room--bedroom{}
    .room--bathroom{}

Мы располагаем комнатами нескольких типов, но все комнаты имеют похожие свойства; В каждой комнате есть пол, потолок, стены и двери. Мы можем обобщить эту информацию в общем классе `.room{}`. Тем не менее у нас есть специальные отличающиеся друг от друга типы комнат; Кухня должна иметь плитку на полу, спальня — ковёр; В ванной не должно быть окна, а в спальне, напротив, должно. В каждой комнате скорее всего стены покрашены в свой цвет. OOCSS учит нас абстрагироть похожие свойства в базовый объект и в дальнейшем дополнять его с помощью расширяющих классов для добавления особенных (TODO: treatment)

Вместо того, чтобы плодить множество уникальных компонентов, попытайтесь распознать повторяющиеся шаблоны дизайна и создать из них абстракции; cверстайте эти абстракции, а затем используйте уточняющие классы для расширения их внешнего оформления.

Если вы вынуждены создать новый компонент, то разделите его на структуру и декоративное оформление; сверстайте структуру используя общие классы, тем самым давая возможность использования структуры компонента в других местах вашего проекта, и затем, используя более специфичные классы, оформите компонент в соответствии с требованиями дизайна.

## Разметка

Все компоненты должны быть полностью независимы от ширины; ваши компоненты должны оставаться резиновыми и их ширина должна контролироваться системой модульных сеток.

Высота **никогда** не должна назначаться элементам. Высота применяется только на сущности, имевшие размеры *до того*, как попали на сайт (например, картинки и спрайты). Никогда не устанавливайте высоту на `p`, `ul`, `div`, вообще что угодно. Вы можете добить желаемого эффекта с помощью гораздо более гибкого `line-height`.

Систему модульных сеток следует рассматривать, как структуру сайта. Вы создаёте структуру сайта, а затем наполняете её информацией. Отделяя систему модульных сеток от созданных нами компонентов, вы можете менять расположение компонентов на сайте намного проще, если бы ширина и высота были бы установлены прямо на компоненты; это делает наш фронтэнд более гибким и быстрым в работе.

Вы никогда не должны применять никаких стилей на ячейку сетки, так как они служат только целям разметки. Применяйте стили только на *содержание ячейки*. Никогда, *ни при каких обстоятельствах* не применяйте свойства меняющие поведение `box-model` к ячейкам сетки.

## Единицы измерения

I use a combination of methods for sizing UIs. Percentages, pixels, ems, rems
and nothing at all.

Grid systems should, ideally, be set in percentages. Because I use grid systems
to govern widths of columns and pages, I can leave components totally free of
any dimensions (as discussed above).

Font sizes I set in rems with a pixel fallback. This gives the accessibility
benefits of ems with the confidence of pixels. Here is a handy Sass mixin to
work out a rem and pixel fallback for you (assuming you set your base font
size in a variable somewhere):

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

I only use pixels for items whose dimensions were defined before the came into
the site. This includes things like images and sprites whose dimensions are
inherently set absolutely in pixels.

### Типографские единицы измерения

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Cокращенная запись

**Shorthand CSS needs to be used with caution.**

It might be tempting to use declarations like `background:red;` but in doing so
what you are actually saying is ‘I want no image to scroll, aligned top-left,
repeating X and Y, and a background colour of red’. Nine times out of ten this
won’t cause any issues but that one time it does is annoying enough to warrant
not using such shorthand. Instead use `background-color:red;`.

Similarly, declarations like `margin:0;` are nice and short, but
**be explicit**. If you actually only really want to affect the margin on
the bottom of an element then it is more appropriate to use `margin-bottom:0;`.

Be explicit in which properties you set and take care to not inadvertently unset
others with shorthand. E.g. if you only want to remove the bottom margin on an
element then there is no sense in setting all margins to zero with `margin:0;`.

Shorthand is good, but easily misused.

## Идентификаторы

A quick note on IDs in CSS before we dive into selectors in general.

**NEVER use IDs in CSS.**

They can be used in your markup for JS and fragment identifiers but use only
classes for styling. You don’t want to see a single ID in any stylesheets!

Classes come with the benefit of being reusable (even if we don’t want to, we
can) and they have a nice, low specificity. Specificity is one of the quickest
ways to run into difficulties in projects and keeping it low at all times is
imperative. An ID is **255** times more specific than a class, so never ever use
them in CSS _ever_.

## Cелекторы

Keep selectors short, efficient and portable.

Heavily location-based selectors are bad for a number of reasons. For example,
take `.sidebar h3 span{}`. This selector is too location-based and thus we
cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain
styling.

Selectors which are too long also introduce performance issues; the more checks
in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has
four), the more work the browser has to do.

Make sure styles aren’t dependent on location where possible, and make sure
selectors are nice and short.

Selectors as a whole should be kept short (e.g. one class deep) but the class
names themselves should be as long as they need to be. A class of `.user-avatar`
is far nicer than `.usr-avt`.

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

### Перенасыщеные селекторы

As discussed above, qualified selectors are bad news.

An over-qualified selector is one like `div.promo`. You could probably get the
same effect from just using `.promo`. Of course sometimes you will _want_ to
qualify a class with an element (e.g. if you have a generic `.error` class that
needs to look different when applied to different elements (e.g.
`.error{ color:red; }` `div.error{ padding:14px; }`)), but generally avoid it
where possible.

Another example of an over-qualified selector might be `ul.nav li a{}`. As
above, we can instantly drop the `ul` and because we know `.nav` is a list, we
therefore know that any `a` _must_ be in an `li`, so we can get `ul.nav li a{}`
down to just `.nav a{}`.

### Производительность селекторов

Whilst it is true that browsers will only ever keep getting faster at rendering
CSS, efficiency is something you could do to keep an eye on. Short, unnested
selectors, not using the universal (`*{}`) selector as the key selector, and
avoiding more complex CSS3 selectors should help circumvent these problems.

## Ключевой элемент селектора

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul{}`…

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul{}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

For a full write up please see my article
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will
**always** want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

## Магические числа и абсолютные значения

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

## Conditional stylesheets

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

## Отладка

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

## Препроцессоры

Sass is my preprocessor of choice. **Use it wisely.** Use Sass to make your CSS
more powerful but avoid nesting like the plague! Nest only when it would
actually be necessary in vanilla CSS, e.g.

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Would be wholly unnecessary in normal CSS, so the following would be **bad**
Sass:

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

If you were to Sass this up you’d write it as:

    .header{}
    .site-nav{
        li{}
        a{}
    }