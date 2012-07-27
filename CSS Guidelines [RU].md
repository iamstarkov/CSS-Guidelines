# Общие CSS замечания, советы и рекомендации

## CSS файлы

В начале каждого CSS файла создаётся и поддерживается список содержимого в, указывающий на разделы файла стилей. Название разделов имеет префикс из `$`, что означает поиск `$[раздел]` выдаст только блок относящийся к секции.

### Синтакс и форматирование

Мы используем многострочную запись для улучшения последующей работы с системой контроля версий (выявление различий в сделанных в одной строке это тихий ужас) и мы упорядочиваем логические войства и селекторы по логическим группам, а **не** по алфавиту.

Мы пишем их селекторы в нижнем регистре и разделяем слова дефисом: `.thisIsBad{}`, `.this_is_also_bad{}` but `.this-is-correct{}`.

Всегда пишите последнюю точку с запятой в последнем правиле для селектора, чтобы избежать возможных конфликтов и синтаксических ошибок во время всего использования документа.

Для примера предпочитаемого форматирования и структуры CSS файла просто посмотрите файл [github.com/csswizardry/vanilla/&hellip;/style.css](http://github.com/csswizardry/vanilla/blob/master/css/style.css)

**К прочтению:**

* [coding.smashingmagazine.com/&hellip;/writing-css-for-others](http://coding.smashingmagazine.com/2011/08/26/writing-css-for-others)
* [jasoncale.com/&hellip;/5-dont-format-your-css-onto-one-line](http://jasoncale.com/articles/5-dont-format-your-css-onto-one-line)


## Комментарии

Комментируйте так много и так часто, насколько вы вообще можете это делать. Если это может понадобиться, то включите в комментарий блок кода разметки, помогающий понять в каком контексте находятся стили.

Будьте многословны, не стесняйтесь: CSS будет уменьшен при выкладывании на рабочий сервер.


## Отступы

Для каждого уровня вложенности разметки, пытайтесь делать соотвественные отступы в стилях. Например: 

    .nav{}
        .nav li{}
            .nav a{}
            
    .promo{}
        .promo p{}

Также же пишите вендорные префиксы, так чтобы значения были в одном столбике, то есть:

    -webkit-border-radius: 4px;
       -moz-border-radius: 4px;
            border-radius: 4px;

Это позволяет нам сразу увидеть, что все свойства установлены в 4px, но что более важно — так это то, что если наш редактор поддерживает режим редактирования столбцов, то мы можем менять значение сразу во всей колонке значений в один момент.

## Создание компонента

При создании нового компонента пишите разметку **до того**, как напишите хоть одну строчку CSS. Это позволяет увидеть какие свойства наследовались и избежать повторного применения избыточных стилей.

## OOCSS

При создании компонента старайтесь не повторять себя также не упускайте из виду принципы OOCSS.

Вместо того, чтобы плодить множество уникальных компонентов, попытайтесь распознать повторяющиеся шаблоны дизайна и создать из них абстракции; Сверстайте эти абстракции, а затем используйте уточняющие классы для расширения их внешнего оформления.

Если вы вынуждены создать новый компонент, то разделите его на структура и декоративное оформление; сверстайте структуру используя общие классы, тем самым давая возможность использования структуры компонента в других местах вашего проекта и затем используя более специфичные классы оформите компонент в соответствии с требованиями дизайна.

**К прочтению:**

* [csswizardry.com/&hellip;/the-nav-abstraction](http://csswizardry.com/2011/09/the-nav-abstraction)
* [stubbornella.org/&hellip;/the-media-object-saves-hundreds-of-lines-of-code](http://stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code)


## Раскладка

Все компоненты должны быть полностью независимы от ширины; ваши компоненты должны оставаться резиновыми(fluid) и их ширина должна контролироваться сеточной системой (grid system)

Высота **никогда** не должна назначаться элементам. Высота применяется только на сущности, имевшие размеры *до того*, как попали на сайт (например, картинки и спрайты). Никогда не устанавливайте высоту на `p`, `ul`, `div`, что угодно. Вы можете добить желаемого эффекта с помощью гораздо более гибкого `line-height`.

(Grid systems should be thought of as shelves.) Они содержат контент, но не являются им самим. (You put up your shelves then fill them with your stuff.)

Вы никогда не должны применять никаких стилей на ячейку сетки, так как они служат только целям разметки. (Never, under any circumstances, apply box-model properties to a grid item.)

## Размеры

Мы используем различные методы для задания размеров интерфейса. Проценты, пиксели, `ems`, `rems` и (nothing at all.)

**К прочтению:**

* [csswizardry.com/&hellip;/measuring-and-sizing-uis-2011-style](http://csswizardry.com/2011/12/measuring-and-sizing-uis-2011-style)


## Размеры текста

Мы используем `rems` (c запасным решением с помошью пикселей только для старых браузеров). Мы категорически не хотим определять размеры текста в пикселях как это делают обычно. Мы определяем `line-height` без определения размерностей везде, **кроме** тех случаев когда позиционируем текст в заранее известной высоте.

Мы избегаем многоразового определения `font-size`; для достижения той же цели мы используем предопределённый размер шрифтов разбитые по классым (to achieve this we have a predefined scale of font sizes tethered to classes). Мы можем переделать их вместо определения `font-size` снова и снова.

Перед тем как задать элементу `font-size`, убедитесь что класс с заданным значеним ещё не существует.

**К прочтению:**

* [csswizardry.com/&hellip;/pragmatic-practical-font-sizing-in-css](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)


## Сокращённая запись

Это может показаться (tempting) использовать правила похожие на `background: red;` но делая это, мы на самом деле говорим: «Я хочу, чтобы фоном была не одна картинка скролящуюся, спозиционированную вверх и влево и повторяющуюся по X и Y и чтобы цвет фона был красный». В девяти случаях из десяти это не высовет никаких проблем, но в 10% обязательно доставит достаточно неприятностей, чтобы не использовать сокращенные записи. Вместо этого используйте `background-color: red;`.

Например, ситуация с правилом `margin: 0;` — оно ясное и короткое, но черезчур (**be explicit**). Если вы на самом деле хотите сделать отступ снизу от элемента, то гораздо лучше (appropriate) использовать `margin-bottom: 0;`.

Будьте (explicit) в свойствах которые вы устанавливаете и следите за тем, чтобы (inadvertently) не сбросить свойства других элементов используя сокращенную запись. Например, если вы хотите сбросить нижний отступ, то нет никакой необходимости в сбрасывании (blitzing) всех отступов с помощью `margin: 0;`.

Сокращённая запись сама по себе хороша, но легко используется неправильно.

## Селекторы

Сохраняйте селекторы (efficient) и переносимыми.

Тяжелые, основанные на расположении внутри DOM-дерева (location-based), селекторы никуда не годятся по ряду причин. Например возьмем `.sidebar h3 span{}`. Этот селектор основан на вложении (location-based) и поэтому мы не можем переместить тот `span` из `h3` и из `.sidebar` — не можем обеспечить поддержку стилей на должном уровне (maintain styling.).

Слишком длинные селекторы также вызывают проблемы производительности; чем больше проверок в селекторе (например селекторе `.sidebar h3 span` имеет три проверки, а `.content ul p a` — четыре), тем больше работы должен выполнять браузер.

Старайтесь следить, чтобы ваши стили не зависели от вложенности, где это возможно (Make sure styles aren’t dependent on location where possible), а также что ваши селекторы короткие и легко воспринимаемые.

**Запомните:** Классы на самом деле ни семантичны, ни не семантичны; Они применимы или нет! Перестаньте беспокоиться о «семантике» имён классов и выберите что-нибудь удобное в применении, с расчётом на дальнейшее использование.

**К прочтению:**

* [speakerdeck.com/&hellip;/breaking-good-habits](http://speakerdeck.com/u/csswizardry/p/breaking-good-habits)
* [csswizardry.com/&hellip;/writing-efficient-css-selectors](http://csswizardry.com/2011/09/writing-efficient-css-selectors)

### Слишком специфичные селекторы

Гиперспецифичный селектор это один из ряда `div.promo`. Скорее всего мы можем достичь тот же самый эффект используя лишь `.promo`. Конечно, иногда мы *хотим* определить класс в зависимости от элемента (например, если у вас есть общий класс `.error`, который должен выглядеть по разному на разных элементах (например, `.error{ color: red; }` `div.error{ padding: 14px; }`)), но по возможности избегайте этого, где это только возможно.
 
Другим примером слишком специфичного селектора может быть `ul.nav li a{}`. Как описано выше мы сразу можем выкинуть `ul`, и так как мы знаем, что `.nav` это список, то ссылка будет вложена только в `li`, поэтому мы можем сократить `ul.nav li a {}` до `.nav a`.

### Производительность

Это правда (Whilst it is true), что браузеры только улучшают свои показатели в скорости рендеринга CSS, «Эффективность» — это то, на чём мы можем быть сфокусированы всегда. Короткие селекторы, неиспользование универсального (`* {}`) селектора и избегание больших комбинаций CSS3 селекторов должно помочь ( circumvent these problems).

**К прочтению:**

* [csswizardry.com/…/writing-efficient-css-selectors](http://csswizardry.com/2011/09/writing-efficient-css-selectors)

## Будьте точны, не делайте обобщений и предположений (assumptions)

Вместо использования селекторов спускающихся по всему DOM-дереву(Instead of using selectors to drill down the DOM to an element), чаще удобнее добавить класс элементу, для которого вы хотите написать стили. Давайте рассмотрим конкретный пример.

Представьте себе промо баннер с классом `.promo` с текстом внутри и ссылкой призывающей к действию. Если будет только один `a` во всем `.promo` то можно стилизовать «призывную» ссылку с помощью `.promo a {}`.

Проблема станет очевидно так скоро, как вы добавите простую текстовую ссылку (или любую другую ссылку) в `.promo` контейнер; только что добавленная ссылка унаследует стили от «призывной» ссылки в независимости от того, хотите вы этого или нет. В этой ситуации будет лучше добавить точный класс (например `.cta` — abbr. from call-to-action) ссылке, которую вы хотите стилизовать.

Будьте точны, конкретны; указывайте именно тот элемент, который вам нужен, не его родитель. Никогда не предполагайте, что разметка не будет меняться.

### Ключевой селектор никогда (обычно) не должен быть селектором тега или основным классом компонента/абстракции (Key selectors should (typically) never be a type selector or an object/abstraction class)

Вы никогда не должны обнаружить себя пишущим (You should never find yourself writing) селектор, в котором ключевое значение имеет селектор по тегу (например, `.header ul {}`) или базовый компонент (например, `.header .nav {}`). Так как вы никогда не сможете гарантировать, что будет только один `ul` или один `.nav` в контейнере `.header`, ключевой селектор (selector is too loose—too broad).

Будет правильнее задать элементу класс, отвечающий на вопрос, что это за элемент и указывающий на него и только на него (It would be more appropriate to give the element in question an explicit class targeting that one and that one only), итак `.header .nav {}` может быть заменено на `.site-nav`, например.

Единственное, когда селектор по тегу может быть востребован, это когда у вас ситуация похожа на ту, что описана ниже: (The only time where a type selector may be appropriate is if you have a situation like this:)

    a{
        color:red;
    }
    .promo{
        background-color:red; 
        color:white;
    }
        .promo a{
            color:white;
        }

В данном случае вы *знаете*, что каждый `a` в `.promo` нуждается в пустом правиле, так как в противном случае будет нечитабельным.

**Пишите селекторы, указывающие на желаемые элементы,(Write selectors that target what you want, not what happens to be there already.) **

## Идентификаторы (ID) и классы

Никогда не используйте ID в CSS, **абсолютно никогда**. Они могут быть использованы в вашей разметке, только для джаваскрипта или обозначения секций документа (fragment-identifiers). Для стилизации используйте только классы. Мы не хотим увидеть хоть один ID в этой или любой другой таблице стилей.

Классы предоставляют возможность повторного использования (даже если мы не хотим, но всё равно мы можем) и имеют прелестную низкую специфичность.


**К прочтению:**

* [csswizardry.com/&hellip;/when-using-ids-can-be-a-pain-in-the-class](http://csswizardry.com/2011/09/when-using-ids-can-be-a-pain-in-the-class)


## `!important`

Допустимо использовать `!important` только на вспомогательных классах. Добавляя `!important` (preemptively) круто, например `.error { color: red !important; }`, (as you know you will **always** want this rule to take precedence).

Использование `!important` (reactively), например чтобы помочь выбраться себе из ситуации с запутанной специфичностью, не приветствуется (not advised). Переработайте ваш CSS и старайтесь избежать этих проблем рефакторингом ваших селекторов. Сохраняя ваши селекторы короткими и неиспользование ID сослужит вам хорошую службу.


## Magic numbers and absolutes

A magic number is a number which is used because ‘it just works’. These are bad because they rarely work for any real reason and are not usually very futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only works here because in this particular scenario the `.dropdown-nav` happens to be 37px tall.

Instead we should use `.dropdown-nav li:hover ul{ top:100%; }` which means no matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from the top.

Every time you hard code a number think twice; if you can avoid it by using keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’) or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily want to keep.


## Conditional stylesheets

IE stylesheets can, by and large, be totally avoided. The only time an IE stylesheet may be required is to circumvent blatant lack of support (e.g. PNG fixes).

As a general rule, all layout and box-model rules can and _will_ work without an IE stylesheet if you refactor and rework your CSS. This means we never want to see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such CSS that is clearly using arbitrary styling to just ‘make stuff work’.


## Debugging

If you run into a CSS problem **take code away before you start adding more** in a bid to fix it. The problem exists in CSS that is already written, more CSS isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects of a layout quirk, but overflow was probably never the problem; **fix the problem, not its symptoms.**


## Preprocessors

By following the above advice you should typically find the need for a preprocessor decreases dramatically. If you still wish to use a preprocessor then by all means do so, but only as en extension of the above, not an alternative.

For example, preprocessors’ nesting abilities often lead to overly specific and location dependent selectors. Let’s use our `. nav a{}` example again:

    .nav{
        li{
            a{}
        }
    }

Will compile to:


    .nav {}
    .nav li {}
    .nav li a {}

Whilst this is a very timid example, it does help illustrate how a lot of preprocessors’ built in ‘helpful’ aspects actually go against our ideals; `.nav li a{}` could (and should) just be `.nav a{}`.

Also, with mixins and the like, preprocessors teach you how to recognise abstractions&mdash;which is great&mdash;but not necessarily how to use them properly; there’s no point writing an abstracted mixin when you proceed to repeat it a dozen times in a stylesheet.

Be sure to know the ins-and-outs of excellent vanilla CSS and where a preprocessor can _aid_ that, not hinder or undo it. Learn the downsides of preprocessors inside-out and then fuse the best aspects of the two with the bad bits of neither.