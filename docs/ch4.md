# 条件 CSS

在CSS的世界中，总是有很多实验性的属性先行，正因为这些先行者在不断的探索新的特性，才让CSS越来越强大。而这些实验性的特性并没有立马得到众多浏览器的支持，为了能让这些实验性特性能在部分支持的浏览器上运行，同时又能让不支持的浏览器做相应的降级处理。那么我们就会需要根据相关的条件进行判断。这也就是条件CSS的由来。

## 条件CSS的简介

条件CSS（Conditional CSS）现在被纳入到 **[CSS Conditional Rules Module Level 3](//www.w3.org/TR/css3-conditional/)** 模块中。事实上，条件CSS的开发源于在多数浏览器上修正CSS渲染bug的需求，以确保尽量多的用户看到正确的网站设计。核心思想是建立在IE条件注释方法，并扩展到包含其他的浏览器，而且将条件声明内联到CSS定义里面。

但随着技术不断的革新，条件CSS现在很少使用条件注释这样的方式来做条件判断，而是提供一些CSS特性（比如，CSS的`@`规则）及其相关的JavaScript API允许我们在满足特定条件时应用样式或行为。

需要注意的是，如果所有浏览器都能正确地执行W3C发布的CSS标准，那么条件CSS就没有需求了。但是，CSS在不同浏览器渲染总是会有或多或少的bug存在，而且往往都及其让人沮丧。条件CSS给我们提供了一个简单的方法来解决这些问题。加上文章开头也提到过了，在CSS的社区中总是有很多先驱者在不断的探索和创造一些实验性CSS特性。我们也可以通过条件CSS在一些已得到的浏览器上先用起来。

## 条件CSS分类

到目前为止，条件CSS主要有三个`@`规则：

- **`@media`**
- **`@supports`**
- **`@viewport`**

其中`@media`和`@supports`两个规则是我们常见的规则，也是真正的条件CSS，而`@viewport`并不常见，但也不是真正的条件。

## CSS中的`@`规则

既然条件CSS运用到的也是CSS的`@`规则，那么我们很有必要先简单的了解一下CSS的`@`规则。

> CSS的`@`规则（`at-rule`）是一条语句，它为CSS提供了执行或如何执行的指令。

`@`规则的每个语句都是以`@`开头，后面直接跟着相应的关键词，这些关键词充当CSS应该做什么的标识符。尽管每个`@`规则都有它的变体，但这也是最常见的语法规则。

CSS的`@`规则主要分为**常规规则**和**嵌套规则**两大类。

### 常规规则

常规规则的语法较为简单，类似下面这样：

    @[关键词](规则)

常规规则常见的主要：

#### `@charset`

大家不知道有没有印象，早期在创建`.scss`或`.less`文件时都会要求在第一行中使用`@charset`来声明字符集，比如：

    @charset 'utf-8'

在某些CSS属性（比如`content`）中使用非`ASCII`字符或样式表包含`UTF-8`等非`ASCII`字符时，`@charset`规则非常有用。另外，`@charset`规则必须是样式表中的第一个元素，并且前面不能有任何字符。用户代理必须忽略样式表开头之外的任何`@charset`规则。如果定义了几个`@charset`规则，则只使用第一个。

#### `@import`

`@import`允许用户从其他样式表导入样式规则。比如：

    @import url("https://fonts.googleapis.com/css?family=Libre+Baskerville");
    @import url("print.css") print;
    @import url("tv.css") projection, tv;
    @import 'custom.css';
    @import "common.css" screen, projection;
    @import url('landscape.css') screen and (orientation:landscape);

导入样式规则时，就好像文件的内容就在规则所在的位置一样。这些规则必须先于所有其他类型的规则，`@charset`规则除外，否则`@import`规则会不生效。

> 在实际项目中不建议使用`@import`来引用其他CSS样式文件。这样做不但请求多，还会造成阻塞。

#### `@namespace`

`@namespace`规则对于很多同学而言会感到陌生。从词面上来了解，它是用来声明一个命名空间前缀，并将其与给定的命名空间关联起来。然后可以在命名空间限定的名称中使用此命名空间前缀。该规则对于将CSS应用到XHTML中特别有用，这样一来，XHTML元素就可以用作CSS中的选择器。可以使用定义的命名空间来限制泛型、类型和属性选择器，只选择该命名空间中的元素。`@namespace`规则通常只在处理包含多个命名空间的文档时有用，比如包含内联SVG或MathML的HTML，或者包含多个词汇表的XML。

    @namespace url(http://www.w3.org/1999/xhtml);
    @namespace svg url(http://www.w3.org/2000/svg);

    /* 和所有XHTML中的a元素匹配，因为XHTML是默认的命名空间 */
    a {
        color: red;
    }

    /* 和所有SVG中的a元素匹配 */
    svg|a {
        color: blue;
    }

    /* 同时匹配XHTML和SVG中的a元素 */
    *|a {
        color: orange;
    }

使用`@namespace`规则有几点要注意：

- 任何`@namespace`规则都必须遵循所有`@charset`和`@import`规则，并位于样式表中所有其他`@`规则和样式声明之前
- `@namespace`规则可以用于定义样式表的默认命名空间。定义默认命名空间时，所有通用选择器和类型选择器（不包含属性选择器）仅应用于该命名空间中的元素
- `@namespace`规则还可以用于定义命名空间的前缀。如果泛型、类型和属性选择器的前缀是命名空间的前缀，那么该选择器只在元素或属性的命名空间和名称匹配时才匹配

### 嵌套规则

嵌套规则和常规规则不同的是，在规则后面会带一个花括号`{}`，括号中会嵌套一些样式规则：

    @[关键词] {
        /* 样式规则 */
    }

CSS的嵌套规则主要有：

#### `@font-face`

CSS的`@font-face`规则允许我们引用自定义的字体，该规则消除了依赖于计算机上安装的有限字体数量的需求。在使用自定义定体时，需要先使用该规则来声明：

    @font-face { 
        [ font-family: <family-name>; ] || 
        [ src: [ <url> [ format(<string>#) ]? | <font-face-name> ]#; ] || 
        [ unicode-range: <urange>#; ] || 
        [ font-variant: <font-variant>; ] || 
        [ font-feature-settings: normal | <feature-tag-value>#; ] || 
        [ font-stretch: <font-stretch>; ] || 
        [ font-weight: <weight>; ] || 
        [ font-style: <style>; ] 
    } 

每个`@font-face`规则为每个字体描述符（隐式或显式）指定一个值。规则中没有给出显式值的部分使用每个描述符列出的初始值。这些描述符仅适用于定义它们的`@font-face`规则的上下文中，而不适用于文档语言元素。没有关于描述符应用于哪些元素或这些值是否由子元素继承的概念。当给定的描述符在给定的`@font-face`规则中多次出现时，只使用最后一个描述符声明，并忽略该描述符所有先前声明。

另外，该规则允许选择与设计目标密切匹配的字体，而不是将字体选择限制为给定平台上可用的字体。一组字体描述符定义字体资源的位置，包括本地或外部的位置，以及单个外观的样式特征。多个`@font-face`规则可用于构造具有多种字体的字体族。使用CSS字体匹配规则，用户代理可以选择性地只下载所需的字体。

在使用`@font-face`时也有些细节需要注意：

- 作用域的限制。Web字体受到作用域的限制，因此`@font-face`规则中引用的字体资源必须与使用它们的页面位于相同的作用域，除非使用`HTTP`访问控制来放宽这一限制
- 不考虑指定文件的`MIME`类型，因为没有为`TrueType`、`OpenType`和`WOFF`字体定义`MIME`类型
- `@font-face`不能在CSS选择器中声明

> 在项目中使用`@font-face`引用自定义定体涉及很多细节，有关于这方面的细节，后续我们将会花费一个章节来专门介绍。

#### `@keyframes`

`@keyframes`规则主要用来声明一个动画，在嵌套的规则中指定了动画各个节点（帧）的样式规则。

    @keyframes <keyframes-name> { 
        <keyframe-block-list> 
    }

`@keyframes`只是声明了一个动画，如果没有被`animation-name`属性调用的话，那么该规则中的样式并不会起任何的作用。另外要使用关键帧列表有效，它必须包含动画开始和结束状态的规则，即`0%`（`from`）和`100%`（`to`）。如果没有指定这两个时间偏移量，那么`@keyframes`声明就会无效，解析器将会忽略它，并且不能用于`animation`中。

如果我们通过JavaScript来操作`@keyframes`中的规则，可以使用CSSOM中的`CSSKeyframesRule`。

> 特别注意：在`@keyframes`规则中声明的样式规则会覆盖元素中的样式规则，哪怕是带有`!important`加强权重的样式规则。

有关于`@keyframes`更深入的介绍，我们将会放到CSS动画相关的章节来阐述。

#### `@media`

`@media`规则是条件CSS中的一种，其条件是一个媒体查询。它由一个媒体查询列表（可以是空的）和一组规则组成。规则的条件是媒体查询的结果。

    @media <media-query-list> { 
        <group-rule-body> 
    }

#### `@supports`

`@supports`规则是条件CSS中的另一种，也是一条件组规则，其条件测试用户代理是否支持CSS属性/值对。它可以用于编写样式表，这些样式表在可用时使用新特性，但在不支持这些特性时将可以优雅地降级。

    @supports <supports-condition> { 
        <group-rule-body> 
    }

#### `@viewport`

`@viewport`规则事实上不是条件CSS中的一种。该规则在CSS中定义了一组嵌套的描述符，这些描述符主要用来控制移动设备上的`viewport`设置。

    @viewport { 
        <group-rule-body> 
    }

比如：

    @viewport {
        min-width: 640px;
        max-width: 800px;
    }

#### `@page`

`@page`规则主要用于打印文档时候修改一些CSS属性。使用`@page`我们只能改变部分CSS属性，例如间距属性`margin`, 打印相关的`orphans`, `widows`, 以及`page-break-*`, 其他CSS属性会被忽略。

    @page <page-selector-list> { 
        <page-body> 
    }

#### `@document`

`@document`规则指定应用于特定页面的样式的条件。该规则可以指定一个或多个匹配函数，如果其中任何一个函数应用于`URL`，则该规则将对具有该`URL`的文档生效。比如说，这个CSS文件被子站`A`调用，和被子站`C`调用，我们可以通过域名匹配来执行不同的CSS样式。这样，我们可以有效避免冲突，或者防止外链之类。

    @document 
        /* 页面URL需要是 */
        url(https://www.w3cplus.com/),
        
        /* 页面URL的开头必须是... */
        url-prefix(www.w3cplus.com/blog/),
        
        /* 该域上的所有页面 */
        domain(w3cplus.com),

        /* 所有https协议页面 */
        regexp("https:.*")
        {
        
            /* 开始样式 */
            body { 
                color: #444;
            }

    }

#### `@font-feature-values`

`@font-feature-values`规则主要用于给定字体家族的替代符号的索引定义命名值。它允许在`font-variant-alternates`中使用一个公共名称来替换`OpenType`中不可激活的特性，从而在使用多种字体时简化CSS。

    @font-feature-values <family-name># { 
        <feature-value-block-list> 
    } 

来看一个小示例：

    /* 在Font One中激活 cool-style 风格的字体 */
    @font-feature-values Font One {
        @styleset {
            cool-style: 12;
        }
    }

    /* 在Font Two中激活 cool-style 风格的字体 */
    @font-feature-values Font Two {
        @styleset {
            cool-style: 4;
        }
    }

    /* 与字体无关 */
    .cool-look {
        font-variant-alternates: styleset(cool-style);
    }

#### `@counter-style`

`@counter-style`规则可以允许我们定义自定义的计数器的样式。计数器样式由`@counter-style`规则中的描述符来指定，主要由`system`、`symbols`、`additive-symbols`、`negative`、`prefiex`、`suffix`和`range`等组成。该规则的一般形式是：

    @counter-style <counter-style-name> { 
        [ system: <counter-system>; ] || 
        [ symbols: <counter-symbols>; ] || 
        [ additive-symbols: <additive-symbols>; ] || 
        [ negative: <negative-symbol>; ] || 
        [ prefix: <prefix>; ] || 
        [ suffix: <suffix>; ] || 
        [ range: <range>; ] || 
        [ pad: <padding>; ] || 
        [ speak-as: <speak-as>; ] || 
        [ fallback: <counter-style-name>; ] 
    } 

具体使用的时候可以像下面这样：

    @counter-style circled-alpha {
        system: fixed;
        symbols: Ⓐ Ⓑ Ⓒ;
        suffix: " ";
    }

    li {
        list-style: circled-alpha;
    }

上面列出了CSS的`@`规则，其中`@charset`、`@import`、`@font-face`、`@keyframes`、`@media`、`@supports`是我们常见或已在项目中有见过的`@`规则；而`@namespace`、`@viewport`、`@page`、`@document`、`@font-feature-values`和`@counter-style`等规则是我们不怎么常见。

在众多CSS的`@`规则中，`@media`、`@supports`和`@viewport`又被称为是条件CSS。这也是我们这一章节中重点。那么接下来，我们详细的来聊聊这三个规则。

## 条件CSS之`@media`

就前端开发者而言都避免不了面对众多设备终端的适配。而设备终端可谓是绫罗满目，用下图来形容一点不为过：

![](/images/ch4/figure-1.gif)

面对这样的场景，早在2010年社区就提出了**响应式设计**（Responsive Design）的概念。

> **响应式设计**指的是，你的Web应用程序或Web页面应该从宽屏显示器到手机终端屏幕的所有东西上都显示得一样的好。

这是一种Web设计和开发的方法，他的最初目的就是在可限的空间最好方式展示最全、最优内容（布局）。它削除了网站在移动端和桌面端之间的差别。换句话说，用户在拖拉浏览器改变视口时能以最佳的方式实现Web应用程序或Web页面的布局。

而在响应式设计中，最为关键的就是条件CSS中的**媒体查询**，即`@media`。**媒体查询可以有条件的应用CSS规则**，它告诉浏览器应该忽略或应用哪些CSS规则，而这些都取决于用户的设备终端。

![](/images/ch4/figure-2.png)

媒体查询让我们将相同的HTML内容在不同的设备终端运用不同的CSS规则，最终向用户呈现不同的布局效果。因此，与其为智能手机维护一个网站，为笔记本电脑或台式机维护另一个网站，还不如借助媒体查询的特性来为不同的终端设备维护相同的HTML结构，然后再利用不同的规则展示不同的布局风格。这从维护成本上来说，还是有利的。

既然媒体查询这么强大，那么我们应该怎么来使用媒体查询呢？或者说媒体查询包含了哪些知识呢？这也是我们接下来要了解和学习的东西。

### 媒体查询是什么？

媒体查询是一种条件CSS，它提供了一种规则，让我们在符合条件的情况之下调用正确的CSS规则。而这个条件可以根据使用的**设备类型**、**视口大小**、**屏幕像素密度**甚至**设备方向**。

简单地说，媒体查询使用`@media`规则，后面跟着一个**媒体类型**、零个或多个**媒体特性**，或者一个或多个**媒体类型**和一个或多个**媒体特性**。然后再符合条件的特定范围输出正确的CSS规则。

### 媒体查询语法

媒体查询包含一个可选的**媒体类型**和**媒体特性**表达式（零个或多个）最终会被解析为`true`（符合条件规则）或`false`（不符合条件规则）。如果媒体查询中指定的媒体类型匹配展示文档所使用的设备类型，并且所有的表达式的值都是`true`，那么该媒体查询的结果为`true`。

![](/images/ch4/figure-3.png)

*先忽略此图的具体意思，随着后续内容的完善，我们都能看懂此图的含义。*

在实际使用`@media`规则时，不管是使用哪种方式，都是可以使用的，比如：

    <!-- link元素中的CSS媒体查询 -->
    <link rel="stylesheet" media="screen and (max-width: 800px)" href="example.css" />

    <!-- import导入CSS中的媒体查询 -->

    @import url(example.css) screen and (color), projection and (color);

    <!-- 样式表中的CSS媒体查询 -->
    <style>
        @media screen and (max-width: 600px) {
            :root {
                color: green;
            }
        }
    </style>

当媒体查询中的条件规则为`true`时，其对应的样式表或样式规则就会遵循正常的级联规则进行应用。即使媒体查询的规则返回的是`false`，`<link>`标签和`@import`指向的样式规则也将会被下载，但是它们不会被应用到页面上。

针对媒体查询的语法规则，我们可以用一张细化的图来描述：

![](/images/ch4/figure-4.png)

从上图可以看出，整个媒体查询规则中主要包含了三个部分：**媒体类型**、**媒体特性**和**逻辑操作符**。接下来，我们主要围绕着这三个方面进行展开。

### 媒体查询类型

媒体查询类型简称为**媒体类型**，它是媒体查询条件中的重要部分之一。媒体类型允许你在不同的媒体类型上指定相应的样式文件。原始媒体类型集是在HTML4中定义的，主要用于`<link>`元素上的媒体属性，比如`screen`和`print`等。比如下面的代码，大家应该不会感到陌生：

    <link href="style.css" media="screen" />
    <link href="print.css" media="print" />

而事实上，除了我们常见的`all`、`screen`和`print`媒体类型之外，还有其他的一些媒体类型：

| 媒体类型 | 描述 |
| ----- | ----- |
| `all` | 所有设备 |
| `screen` | 电脑屏幕 |
| `print` | 文档打印或打印预览模式 |
| `braille` | 盲文 |
| `embossed` | 盲文打印 |
| `handheld` | 手持设备 |
| `speech` | ‘听觉’类似的媒体类型 |
| `tty` | 用于使用固定密度字母栅格的媒体，比如电传打字机和终端 |
| `tv` | 用于电视机类型的设备 |
| `projection` | 用于方案展示，比如幻灯片 |

不幸的是，媒体类型作为区分具有不同样式需求的设备的一种方式已经是不够的。一些原本非常不同的类别，比如屏幕（`screen`）和手持设备（`handheld`）已经显著地融合在一起。其他类型，比如`tty`和`tv`，暴露了与全功能计算机显示器的标准的有用差异，因此对使用不同样式的目标有用，但是媒体类型的互斥定义使它们难以以合理的方式使用；相反，它们独有的方面可以用媒体特性来处理。

对于媒体类型，我们常见的使用方式主要有：

    <link href="style.css" media="screen print" />

    @import url("style.css") screen;

    <style media="screen">
        @import url("style.css");
    </style>

    @media screen{
        selector{rules}
    }

### 媒体特性

刚才也提到过了，目前就媒体类型（设备终端）有一定的缺陷，因此为了更好的使用媒体查询规则，还会借助媒体特性来加强条件规则方面的判断。比如下表所列：

| 媒体特性 | 是否接受`min`和`max`的前缀 | 描述 |
| ----- | ----- | ----- |
| `width` | 是 | 输出设备渲染区域（可视区域的宽度或打印机纸盒的宽度）的宽度 |
| `height` | 是 | 输出设备渲染区域（可视区域的高度或打印机纸盒的高度）的高度 |
| `device-width` | 是 | 输出设备的宽度（整个屏幕或页的宽度，而不仅仅像文档窗口一样的渲染区域） |
| `device-height` | 是 | 输出设备的高度（整个屏幕或页的高度，而不仅仅像文档窗口一样的渲染区域） |
| `aspect-ratio` | 是 | 输出设备目标显示区域的宽高比 |
| `device-aspect-ratio` | 是 | 输出设备的宽高比 |
| `orientation` | 否 | 指定了设备处于横屏（宽度大于高度）模式还是竖屏（高度大于宽度）模式 |
| `resolution` | 是 | 指定输出设备的分辨率（像素密度）|

上面这些是我们常见的一些媒体特性，但还有一些我们不常见的媒体特性，比如：

| 媒体特性 | 是否接受`min`和`max`的值 | 描述 |
| ----- | ----- | ----- |
| `color` | 是 | 指定输出设备每个像素单元的比例值，如果设备不支持输出颜色，则该值为`0` |
| `color-index` | 是 | 指定输出设备中颜色查询表中的条目数量 |
| `grid` | 否 | 判断输出设备是网格设备还是位图设备，如果设备是基于网格的，该值为`1`，否则为`0` |
| `monochrome` | 是 | 指定了一个黑白（灰度）设备每个像素的比例数。如果不是黑白设备，该值为`0` |
| `scan` | 否 | 描述了电视输出设备的扫描过程 |
| `update` | 否 | 指定输出设备是否具有内容渲染后修改外观的能力，接受`none`（无）、`slow`（慢）和`fast`（快）三个值 |
| `overflow-block` | 否 | 指设备处理块内容溢出的处理行为，主要包含`none`、`scroll`、`optional-paged`和`paged`四个值 |
| `overflow-inline` | 否 | 指设备处理内联内容溢出的处理行为，主要包含`none`和`scroll`两个值 |
| `color-gamut` | 否 | 指设备可以显示的颜色的大致范围 |
| `pointer` | 否 | 指设备（如鼠标）的存在性和准确性。如果存在多个指针设备，则指针媒体特性必须反映由用户代理决定的“主”指向设备的特征 |
| `hover` | 否 | 指设备是否具有指针悬浮在元素上的能力 |
| `any-pointer` | 否 | 确定是否有可用的设备指针与请求的条件匹配 |
| `any-hover` | 否 | 确定是否有可用的设备指针可以悬停 |

上面两个表格中是 **[Media Queries Level 4](//www.w3.org/TR/mediaqueries-4/#mq-features)** 所列出的媒体特性，有常用的，也有少见的，但具体何时使用什么样的媒体查询特性来增强`@media`规则条件，需要根据具体的需求来判断。稍后我们会举一些简单的示例，来增强大家对媒体特性的理解。

除此之外，**[Media Queries Level 5](//drafts.csswg.org/mediaqueries-5/#mq-features)**草案在媒体特性方面还进行了增强，纳入到Level 5的媒体查询特性主要有：

| 媒体特性 | 是否支持`min`或`max`前缀 | 描述 |
| ----- | ----- | ----- |
| `light-level` | 否 | 用于查询设备所使用的环境光级，允许作者相应地调整文档的样式 |
| `environment-blending` | 否 | 用于查询用户的显示特征，从而调整文档的样式。作者可以根据显示技术调整页面的视觉效果或布局，以增加吸引力或提高可读性 |
| `scripting` | 否 | 用于查询当前文档是否支持脚本语言，比如JavaScript |
| `nverted-colors` | 否 | 指示内容是否正常显示，或颜色是否被反色 |
| `prefers-reduced-motion` | 否 | 用于检测用户是否请求系统将其使用的动画或运动的数量减少到最小 |
| `prefers-reduced-transparency` | 否 | 用于检测用户是否请求系统使用的最小化透明或半透明层效果 |
| `prefers-contrast` | 否 | 用于检测用户是否请求系统增加或减少相邻颜色之间的对比度 |
| `prefers-color-scheme` | 否 | 用于检测用户是否请求系统使用浅颜色或深颜色主题 |

> 特别声明，Level 5中提到的媒体特性都和系统设置有所关系，另外这些特性还处于草案阶段，随时都有可能会更改或删除。这里所列仅供参考。

就上面所列的几个表格，大家都会觉得媒体特性类型众多，为了能更好的帮助大家理解相关的使用，接下来以常用的媒体特性为例，给大家列几个示例。

不知道大家有没有发现，在上面的几个表格中，有一个选项 **是否接受`min`或`max`的前缀**。因为在媒体特性中，大多数的媒体特性都可以带有`min`或`max`的前缀，比如说`min-width`和`max-width`，用于表达 **“最小的...”** 或者 **最大的...**。用两张图来帮助大家来理解`min`和`max`的实际含义。

![](/images/ch4/figure-5.png)

> **使用`min`或`max`前缀是主要为了避免与HTML或XML中的`<`或`>`字符相冲突。如果你觉得使用`<`或`>`符更易于理解的话，那么可以使用[postcss-media-minmax插件](//github.com/postcss/postcss-media-minmax)来帮助你。**

有了这些基础，我们来看看示例。

如果你想向最小宽度的`20em`的手持设备或屏幕应用样式表，你可以使用下面这样的媒体查询规则：

    @media handheld and (min-width: 20em), screen and (min-width: 20em) {
        /* 样式规则 */
    }

如果你想大宽度在`20em`和`36em`之间的屏幕运用不同的样式规则，则可以使用下面这样的媒体查询规则：

    @media screen and (min-width: 20em) and (max-width: 36em) {
        /* 样式规则 */
    }

如果你想大最小宽度为`375px`和最小高度为`812px`的屏幕上运用相应的样式规则，则可以像下面这样写：

    @media only screen and (min-width: 375px) and (min-height: 812px) {
        /* 样式规则 */
    }

另外你要是想通过横屏或竖屏来区分，则可以像下面这样：

    @media only screen and (min-width: 812px) and (orientation: landscape) {
        /* 横屏样式规则 */
    }

    @media only screen and (min-width: 375px) and (orientation: portrait) { 
        /* 竖屏样式规则 */
    }

甚至还可以根据屏幕分辨率来写：

    @media
        only screen and (min-device-pixel-ratio: 3),
        only screen and (min-resolution: 384dpi),
        only screen and (min-resolution: 3dppx) { 
            /* Retina屏幕下的样式规则 */
    }

上面看到的仅仅是其中的一部分，也是我们常见的一些。对于不常见的媒体特性的规则，这里只向大家展示Level 5中的`prefers-reduced-motion`特性。为什么要特意介绍这个特性呢？主要是因为这个特性特别有意思。因为该特性可能通过特性检测区分并对一些配置较差或主动开启系统**减弱动态效果**的用户进行体验优化。

**减弱动态效果**设置是系统的一个设置，无论是在MacOS还是iOS时都隐藏的比较深。

对于MacOS系统，可以根据下面这个操作路径进行操作：

> 进入 **系统偏好设置** => **辅助功能** => **显示器**

开启 **减弱动态效果**，如下图所示：

![](/images/ch4/figure-6.png)

对于iOS系统，可以根据下面这个操作路径进行操作：

> 进入 **设置** => **通用** => **辅助功能**

开启 **减弱动态效果**， 如下图所示：

![](/images/ch4/figure-7.png)

开启「减弱动态效果」可以有效地降低 MacOS/iOS 系统糟糕的晕眩效果性能开销，从而达到系统更流畅的功效。具体使用的时候，就可以借助媒体特性的规则来处理：

    @keyframes aniName {
        / *声明动画，在@规则中有介绍过 * /
    }
    
    .background {
        animation: aniName 10s infinite alternate;
    }
    
    /* 开启 减弱动态效果 的设备会禁用 aniName动画 */
    @media screen and (prefers-reduced-motion) {
        / * 禁用不必要的动画 * /
        .element {
            animation: none;
        }
    }

如果借助CSS的自定义属性（后面会花一个章节专门介绍），可以让上面的代码变得更为简单：

    :root {
        --anim-duration: 10s
    }

    @media screen and (prefers-reduced-motion) { 
        :root {
            --anim-duration: 0s;
        }
    }

    .element {
        animation: aniName var(--anim-duration) infinite alternate;
    }

> 如果你看不懂这段代码并不要紧，随着你接触或学习了CSS自定义属性之后，你会觉得它们原来是这么的简单（^_^） !

这个功能非常适合为低配置设备的用户以及追求性能的用户做体验优化，因为很多用户会开启 **减弱动态效果** 来在旧设备上提升系统流畅度。

### 逻辑操作符

从上面示例代码我们不难发现，在`@media`规则中有出现过`and`这样的关键词。而这个关键词在媒体查询中被称为**逻辑操作符**。在`@media`可用的逻辑操作符，除了`and`之外，还有`not`和`only`等，这些逻辑操作符构建复杂的媒体查询规则。这几个逻辑操作符有点类似于JavaScript中的**与**、**或**和**非**。其中：

- `and`操作符用来把多个媒体规则组合成一条媒体查询规则，对成链式的特征进行请求，只有当每个规则都为真时，结果才为真（有点类似于**与**的概念）。
- `not`操作符用来对一条媒体查询规则的结果进行取反（有点类似于**非**的概念）
-  `only`操作符仅在媒体查询规则匹配成功的情况下被应用于一个样式，这对于防止让选中的样式在老式浏览器中被应用到

> **若使用了`not`或`only`操作符，必须明确指定一个媒体类型！**

另外，可以将多个媒体查询规则以**逗号（`,`）**分隔放在一起，只要其中任何一个为**真**，整个媒体查询规则语句返回就是**真**。相当于**`or`**操作符。

接下来，我们分别来看看每个操作符具体使用的细节。

#### `and`

`and`逻辑符主要是让你将多个媒体查询规则（多个媒体属性或媒体属性与媒体类型）合并在一起。一个基本的媒体查询规则，即**一个媒体属性与默认指定的`all`媒体类型**，就像下面这样子：

    @media (min-width: 20em) {
        :root {
            --font-size: 100%;
        }
    }

如果你只想在横屏时应用这个规则，你可以使用`and`逻辑符，加上`orientation`特性，比如：

    @media (min-width: 20em) and (orientation: landscape) {
        :root {
            --font-size: 100%;
        }
    }

上面的规则是查询仅在可视区域宽度不小于`20em`并在横屏的设备下有效。如果，你仅想在电视媒体上应用，那么可以继续使用`and`逻辑符来合并媒体类型：

    @media tv and (min-width: 20em) and (orientation: landscape) {
        :root {
            --font-size: 100%;
        }
    }

#### `not`

`not`逻辑符应用于整个媒体查询规则，在媒体查询规则为**假**时返回**真**。比如`monochrome`就用于彩色显示设备上或一个`600px`的屏幕应用于`min-width: 700px`属性查询上。在逗号媒体查询列表中`not`仅会否定它应用到它应用到的媒体查询上而不影响其它的媒体查询。`not`逻辑符仅能应用于整个媒体查询规则，而不能单独地应用于一个独立的媒体查询规则。例如，`not`在下面的媒体查询中最后被计算：

    @media not all and (monochrome) {
        /* 样式规则 */
    }

上面的规则等价于：

    @media not (all and (monochrome)) {
        /* 样式规则 */
    }

而不是：

    @media (not all) and (monochrome) {
        /* 样式规则 */
    }

再来看一个示例：

    @media not screen and (color), print and (color) {
        /* 样式规则 */
    }

等价于：

    @media (not (screen and (color))), print and (color) {
        /* 样式规则 */
    }

#### `or` 和 逗号分隔符

`or`逻辑操作符相当于JavaScript逻辑运符中的**或**，即，当你的媒体查询规则中有多个的时候，只要有一个规则符合条件，其结果就是`true`。另外，在媒查询中，还有一个特殊规则，那就是**逗号（`,`）**分隔符，其效果等同于`or`逻辑操作符。当使用逗号分隔的媒体查询规则时，如果任何一个媒体查询规则返回的是真，那对应的样式规则就会生效。逗号分隔的列表中每个媒体查询规则都是独立的，一个媒体查询规则中的操作符并不会影响其它的媒体查询规则。也就是说，**逗号分隔的媒体查询规则列表能够作用于不同的媒体属性、类型和状态**。

例如，如果你想在最小宽度为`760px`或横屏的手持设备上应用一组样式，可以这样写：

    @media (min-width: 760px), handheld and (orientation: landscape) {
        /* 样式规则 */
    }

正如上面代码所示，如果一个`800px`宽度的屏幕设备，将返回真，逗号前一部分规则相当于`@media all and (min-width: 760px)`将会应用于该设备并且返回真，尽管我的屏幕媒体类型并不与第二部分（逗号后面一部分规则）的手持媒体类型相符合。同样的，如果我是一个`500px`宽的横屏手持设备，尽管第一部分因为宽度并不匹配，但第二部分仍匹配，因此整个媒体查询也会返回值。

#### `only`

`only`逻辑操作符有点特殊，它隐藏了旧浏览器的整个查询（防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式）。换句话说，旧的浏览器不理解`only`逻辑操作符，因此会忽略整个媒体查询规则。否则只会无效：

    @media only all and (min-width: 320px) and (max-width: 480px) {
        /* 忽略老的浏览器样式规则 */
    }

和`not`逻辑操作符类似，`only`逻辑操作符对于使用媒体类型是不可选的。

> 不支持媒体查询Level 3规则的浏览器已经非常少见了，所以在大多数情况之下，使用`only`逻辑操作符是不必要的。

### 标准设备的媒体查询规则

前面也提到过，媒体查询规则`@media`是为响应式设计而生，它是响应式设计的一个主要组成部分，主要作用就是为**正确的设备提供最佳的样式规则，确保最好的用户体验**。但市场上有着大量不同的设备终端，要为所有设备终端提供最匹配的媒体查询规则，匹配最佳样式规则，决不是一件简单的事情。这里罗列了一些媒体查询规则，这些媒体查询规则是针对于众多标准和流行的设备终端而匹配的。非常值得大家收藏，在关键时刻的时候能派上用场。

简单地说，根据您的设计而不是特定的设备选择断点是一种明智的方法。但有时你只需要用一点点代码就可以为一个特定的场景提供相应的样式规则。

#### 手机和手持设备

    /* iPhone 4 和 4s */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 480px)
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 480px)
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: portrait) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 480px)
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: landscape) {
        /* 样式规则 */
    }

    /* iPhone 5, 5S, 5C 和 5SE  */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 568px)
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 568px)
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: portrait) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 568px)
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: landscape) {
        /* 样式规则 */
    }

    /* iPhone 6, 6S, 7 和 8  */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 667px) 
        and (-webkit-min-device-pixel-ratio: 2) { 
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 667px) 
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: portrait) { 
        /* 样式规则 */
    }
    /* 模屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 667px) 
        and (-webkit-min-device-pixel-ratio: 2)
        and (orientation: landscape) { 
        /* 样式规则 */
    }

    /* iPhone 6+, 7+ 和 8+  */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 414px) 
        and (max-device-width: 736px) 
        and (-webkit-min-device-pixel-ratio: 3) { 
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 414px) 
        and (max-device-width: 736px) 
        and (-webkit-min-device-pixel-ratio: 3)
        and (orientation: portrait) { 
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 414px) 
        and (max-device-width: 736px) 
        and (-webkit-min-device-pixel-ratio: 3)
        and (orientation: landscape) { 
        /* 样式规则 */
    }

    /*  iPhone X  */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 812px) 
        and (-webkit-min-device-pixel-ratio: 3) { 
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 812px) 
        and (-webkit-min-device-pixel-ratio: 3)
        and (orientation: portrait) { 
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 375px) 
        and (max-device-width: 812px) 
        and (-webkit-min-device-pixel-ratio: 3)
        and (orientation: landscape) { 
        /* 样式规则 */
    }

    /* iPhone XS Max和 XR  */
    /*横屏和竖屏*/
    @media only screen 
        and (min-device-width: 414px)
        and (max-device-width: 896px)
        and (-webkit-device-pixel-ratio: 3) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 414px) 
        and (max-device-height: 896px) 
        and (orientation : portrait) 
        and (-webkit-device-pixel-ratio: 3){
        /* 样式规则 */
    }
    /*横屏*/
    @media only screen 
        and (min-device-width: 414px) 
        and (max-device-height: 896px) 
        and (orientation : landscape) 
        and (-webkit-device-pixel-ratio: 3){
        /* 样式规则 */
    }

    /*  Google Pixel  */
    /* 横屏和竖屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 3) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 3) 
        and (orientation: portrait) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 3) 
        and (orientation: landscape) {
        /* 样式规则 */
    }

    /*  Google Pixel XL  */
    /* 竖屏和横屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 4) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 4) 
        and (orientation: portrait) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media screen 
        and (device-width: 360px) 
        and (device-height: 640px) 
        and (-webkit-device-pixel-ratio: 4) 
        and (orientation: landscape) {
        /* 样式规则 */
    }

#### 平板电脑

    /*  iPad 1, 2, Mini 和 Air  */
    /* 横屏和竖屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (-webkit-min-device-pixel-ratio: 1) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (orientation: portrait) 
        and (-webkit-min-device-pixel-ratio: 1) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (orientation: landscape) 
        and (-webkit-min-device-pixel-ratio: 1) {
        /* 样式规则 */
    }

    /*  iPad 3, 4 和 Pro 9.7"  */
    /* 竖屏 和 横屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (orientation: portrait) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 768px) 
        and (max-device-width: 1024px) 
        and (orientation: landscape) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }

    /*  iPad Pro 10.5"  */
    /* 竖屏 和 横屏 */
    @media only screen 
        and (min-device-width: 834px) 
        and (max-device-width: 1112px)
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 竖屏 */
    /* 最小设备宽度和最大设备宽度设置相同的值，以免与PC端相 */
    @media only screen 
        and (min-device-width: 834px) 
        and (max-device-width: 834px) 
        and (orientation: portrait) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 横屏 */
    /* 最小设备宽度和最大设备宽度设置相同的值，以免与PC端相 */
    @media only screen 
        and (min-device-width: 1112px) 
        and (max-device-width: 1112px) 
        and (orientation: landscape) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }

    /*  iPad Pro 12.9"  */
    /* 竖屏 和 横屏 */
    @media only screen 
        and (min-device-width: 1024px) 
        and (max-device-width: 1366px)
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 竖屏 */
    /* 最小设备宽度和最大设备宽度设置相同的值，以免与PC端相 */
    @media only screen 
        and (min-device-width: 1024px) 
        and (max-device-width: 1024px) 
        and (orientation: portrait) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }
    /* 横屏 */
    /* 最小设备宽度和最大设备宽度设置相同的值，以免与PC端相 */
    @media only screen 
        and (min-device-width: 1366px) 
        and (max-device-width: 1366px) 
        and (orientation: landscape) 
        and (-webkit-min-device-pixel-ratio: 2) {
        /* 样式规则 */
    }

#### 笔记本电脑

对于笔记本电脑的媒体查询规则，相对来说更为复杂一些。我们应该不要针对特定的设备来设计媒体查询规则，而应该针对屏幕的大小范围来设计，然后再区分是否为视网膜屏幕（Retina）：

    /*  非Retina屏  */
    @media screen 
        and (min-device-width: 1200px) 
        and (max-device-width: 1600px) 
        and (-webkit-min-device-pixel-ratio: 1) { 
        /* 样式规则 */  
    }

    /* Retina屏 */
    @media screen 
        and (min-device-width: 1200px) 
        and (max-device-width: 1600px) 
        and (-webkit-min-device-pixel-ratio: 2)
        and (min-resolution: 192dpi) { 
        /* 样式规则 */
    }

> 上面罗列的是一些常见设备的媒体查询规则，如果你想获取更多的设备媒体查询规则，可以在**[VIZ DEVICES](//vizdevices.yesviz.com/viewport.php)**网站上查询。

### 媒体查询API

在DOM中有一个特性，可以通过JavaScript来获取媒体查询的结果。可以使用`MediaQueryList`接口和它的方法来实现。一旦创建了`MediaQueryList`对象，咱们就可以通过它来检查查询结果，或者也可以设置一些属性，来实现当查询结果变化时，自动接收到通知。

#### 创建媒体查询列表

在获取媒体查询结果之前，首先要创建`MediaQueryList`对象，用来存储媒体查询。为了实现这个目的，需要使用**`window.matchMedia()`**方法。

举个例子，比如你想设置一个查询列表用来判定设备屏幕处于横屏还是竖屏，那你可以像下面这样编码：

    window.matchMedia("(orientation: portrait)");

当设备屏幕是横屏时，`MediaQueryList`对象中的`matches`属性返回的值为`false`，如下图所示：

![](/images/ch4/figure-8.png)

反之，当你的设备是竖屏时，`MediaQueryList`对象中的`matches`属性返回的值为`true`，如下图所示：

![](/images/ch4/figure-9.png)

从上面的示例也可以看出来，`matchMedia()`方法的使用很简单，只需要给这个方法传一个`mediaQueryString`参数，该参数是一个字符串，表示即将返回一个新`MediaQueryList`对象的媒体查询。返回来的`MediaQueryList`对象包含两个属性：

- `media`，是一个`DOMString`类型，返回一个序列化的媒体查询列表
- `matches`，返回的是一个布尔值，匹配则为`true`，否则为`false`

比如下面这个示例：

    window.matchMedia("(min-width: 400px)")
    // => MediaQueryList {media: "(min-width: 400px)", matches: true, onchange: null}
    window.matchMedia("(min-width: 400px)").media
    // => "(min-width: 400px)"
    window.matchMedia("(min-width: 400px)").matches
    // => true

那么在一些情况之下，就可以借助新返回的`MediaQueryList`对象的`matches`属性做一些事情，比如：

    if (window.matchMedia("(min-width: 400px)").matches) {
        // 如果视窗宽度大于或等于400px，返回的值为true
        // 可以针对符合条件的情况下做一些想做的事情
    } else {
        // 视窗小于400px的情况下，做一些自己想做的事情
    }

#### 接收和终止媒体查询的通知

如果想要接收媒体查询的提醒，我们就需要注册一个监听器来帮助我们，这样做要比手动查询更为有效。可以在`MediaQueryList`对象上使用`addListener()`方法，这样就通过实现`MediaQueryListListener`接口来指定一个监听器。比如下面这个示例：

    let receiveMediaQueryMessage = window.matchMedia('(orientation: portrait)')
    receiveMediaQueryMessage.addListener(handleOrientationChange)

    function handleOrientationChange (receiveMediaQueryMessage) {
        if (receiveMediaQueryMessage.matches) {
            console.log('现在处在竖屏')
        } else {
            console.log('现在处在横屏')
        }
    }
    handleOrientationChange(receiveMediaQueryMessage)

当你的手持设备不断的在横屏与竖屏之间切换时，`console.log`打印出来的值也会随之变化，如下图所示：

![](/images/ch4/figure-10.gif)

上面的示例，咱们通过`window.matchMedia()`方法创建了一个屏幕方向检测的查询列表`receiveMediaQueryMessage`，并且添加了一个事件监听。需要注意的是，当我们添加监听之后，我们其实直接调用了一次监听。这会让我们的监听器以目前设备的屏幕方向来初始化判定代码。也就是说，如果我们代码中设定的设备处理竖屏模式，而实际上它在启动时处理横屏模式，那么我们在后面的判定就会出现矛盾。然后我们可以在`handleOrientationChange()`函数中来查看媒体查询结果，并且可以设置屏幕方向变化后的逻辑处理代码。

上面示例演示的是，使用`addListener()`来接收媒体查询的通知，如果你不想再要接收媒体查询值变化的相关通知时，可以在`MediaQueryList`上调用`removeListener()`方法来移除监听，比如：

    receiveMediaQueryMessage.removeListener(handleOrientationChange)

### 使用CSS媒体查询常的缺点

CSS的`@media`规则（媒体查询）是为某些东西设计的，虽然很多人都说媒体查询是响应式Web设计的基石，但事实上那不是响应式Web设计。听起来是不是有些矛盾，那么根据我的经验，我来说说CSS媒体查询时常碰到的问题。

#### 不直观

直到现在为止还没有哪位Web开发者说**CSS媒体查询是直观的**。虽然定义媒体查询规则非常简单，但是并不总是非常清楚，媒体查询规则在真实的浏览器，真实的设备和无数的场景中情况又是如何？

比如：

    @media only screen and (min-width: 320px) and (max-width: 480px) {
        /** 样式规则*/
    }

上面的媒体查询规则的意思是：**当浏览器视窗的宽度是`320px`至`480px`之间时符合媒体查询规则，返回的值是`true`，就会调用对应的样式规则**。事实上，当你想做一些更具体的事情时，比如设备是平板电脑的横屏操作中，应用一个样式规则，这并不完全是确定的或直观的。设置一个媒体查询来做到这一点并不是不可能，但它绝对不是直观的。

#### 限制条件

CSS媒体查询是动态的，允许你在CSS中定义条件语句。例如，如果视口位于这个和那个之间，那么执行另一段样式规则。然而，你这只是考虑了视口方面的限制，但事实上许多Web设计中还会考虑使用的场景。比如说，移动端的TabBar，对于不同的系统场景，他们的布局是有所不同的。例如，iOS设备，系统TabBar常常通栏位于屏幕底部，而Android设备却刚好相反，位于屏幕顶部。

![](/images/ch4/figure-11.jpg)

那么问题来了，CSS媒体查询规则又怎么才能根据系统对某些UI元素设置不同的样式呢？

就算是你绕着走，可以使用CSS媒体查询来做到这一点，但也不能这么做，因为CSS媒体查询不是用于任何特性构建的。除此之外，你可能还需要通过CSS进行许多其他定制，但是当你需要不同程度的简单到高级条件时，媒体查询就不是一个很好的解决方案了。

#### 不是本地扩展

CSS媒体查询是嵌入在浏览器中的一个功能特性。这也意味着它不是本机可扩展的。也就是说，不能通过CSS接口为本机添加额外的CSS媒体查询规或增加其功能。即使新的CSS媒体查询特性得到了Web标准流程的认可，要达到普实性，也还需要较长的一段时间。此外，并不是所有添加的特性都对你有用，因此，如果你没有得到你想要的东西，那么你需要找到其他方法来解决你的问题。

当然，有一种方法可以扩展CSS，但是你必须对JavaScript相关的知识要有深入的了解，而对于大多数Web开发人员来说，这不是一个实用的过程。

#### 开发效率低

使用媒体查询为不同的场景实现不同的Web效果（响应式Web设计）,有可能造成你的代码量成倍增加，因为你要为不同的断点添加单独的样式规则。比如：

    :root {
        --font-size: 100%;
    }

    body {
        font-size: var(--font-size);
    }
    
    /* 竖屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 480px) 
        and (-webkit-min-device-pixel-ratio: 2) 
        and (orientation: portrait) {
        :root {
            font-size: 80%;
        }
    }
    /* 横屏 */
    @media only screen 
        and (min-device-width: 320px) 
        and (max-device-width: 480px) 
        and (-webkit-min-device-pixel-ratio: 2) 
        and (orientation: landscape) {
        :root {
            --font-size: 120%;
        }
    }

除了代码量的增加之外，还会增加工作流程的复杂性。为什么这么说呢？

了解响应Web设计的同学都应该知道，CSS媒体查询大部分在处理布局上的调整。因此，要做更多的事情，甚至还有的时候需要借助JavaScript来弥补其不足之处。同时为你后其的测试也增加了倍数量的工作。

#### 响应Web性能

由于CSS媒体查询的工作方式，你最终需要更多的CSS代码，才能满足你的Web设计需求。根据HTTPArchive.org过去对响应式Web设计的一个数据统计可以得知，CSS文件的大小在过去五年中增加了114%，HTML文件大小的增长在同一时期达到了`53%`的峰值。

这种特殊的情况对您的网站的性能是会有一定的影响，因为在实现CSS媒体查询之后，它的速度肯定比以前慢，特别是对于使用不太理想的移动宽带网络的移动设备。而且，除了文件大小增加的问题之外，CSS媒体查询中没有任何内部机制可以真正提高Web页面的性能。

既然CSS媒体查询存在这么多的不足，那么为什么还有很多Web开发人员使用该技术来实现响应式的Web设计呢？其实这也是有一定的历史原因的。

最初的CSS媒体查询是用来辅助你在Web浏览器中实现一些特殊的效果，比如说，在高屏和短屏的设备中做一些元素上距离调整等。但后来，CSS媒体查询被社区用来承担整个响应式Web设计的重任。这好比，你只能吃一碗饭，然后店家非得让让吃十确碗饭，你说冤屈不。

因此，在一些Web的特殊场景，CSS媒体查询规则，还是具有一定的特殊能力，能帮助众多Web开发人员解决一些痛点。这也是CSS媒体查询规则最初设计的初衷。因此大家不要迷信CSS媒体查询规则是万能的，它只是在最适合的场景使用才是最佳的。

## 条件CSS之`@supports`

Web开发者都知道，不同的浏览器（不管是现代浏览器还是老版本的IE浏览器）对Web页面的解析都会有所不一样，为了让Web页面在这些浏览器下渲染达到基本一致的效果，给用户更好的体验，开发者有时候需要为他们提供不同的样式代码。

而早期的时候，大多都是依赖于JavaScript来检测，然后提供不同的样式规则，很多时候为了节省时间和成本，会直接使用[Modernizr](//modernizr.com/)这样的第三方JavaScript库来完成。但这样做真的有用吗？除了要懂怎么检测之外，还需要了解更多的有关于浏览器渲染机制，这样一来对于很多开发人员而言是较为痛苦的。

不过，幸运的是，条件CSS除了`@meida`之外，还提供了另一个条件CSS规则，即 **`@supports`** 规则，它允许我们可以根据浏览器对CSS特性的支持情况来定义不同的样式。这对于我们来说是非常重要的。

### `@supports`规则作用是什么

用一句话来说，`@supports`规则的作用是：

> **用来查询浏览器是否支持CSS的特性！**

前面也提到过，前端社区有很多大神一直在致力于CSS技术的革新和新特性的推进。所以每一年都会有不少的新特性或CSS的实验性特性的出现，但由于各大浏览器厂商对于这些新特性的支持度是有所差异，甚至支持的友好度也是不一致的。换句话说，或许你知道新出现的CSS特性，但又担心浏览器不支持，从而又不敢使用。那么这个时候就可以借助于`@supports`特性来完成对浏览器的检测。从而可以放心的在支持的浏览器中使用这些新特性CSS，对于不支持的浏览器可以轻易的提供一些降级处理。让你的作品在用户面前展示最佳的效果。

### `@supports`规则

`@supports`规则和`@media`规则有点类似。在`@`规则（`@supports`）后面会紧跟一个或多个条件语句`<supports-condition>`，再紧跟着`{}`，在`{}`中嵌套着你想要的CSS规则。如果条件符合，即返回的值是`true`，浏览器就会运用`{}`中的CSS样式规则；反之，条件不符合，返回的值是`false`，浏览器即会忽略`{}`中的样式规则。用一张图来描述，大家会更易于理解：

![](/images/ch4/figure-12.png)

`@supports`中的条件规则是由一个或多个由不同的逻辑操作符组成的表达式声明组合而成的。使用小括号`()`可以调整这些表达式之间的运算优先级。而其中的条件表达式就是**CSS声明**，也就是一个CSS属性后跟一个属性值，比如`property:value`。比如说，`display: flex`这样的一个表达式，对于支持的浏览器将会返回`true`，反之不支持的浏览器则会返回`false`。

### `@supports`的使用

就使用方面，`@supports`要比`@media`简单和直观的多。比如下面这个小示例：

    <article class="artwork">
        <img src="myimg.jpg" alt="cityscape">
    </article>

HTML的结构非常的简单。一个`article`标签套了一个`img`元素，没啥特殊之处，主要来看CSS方面的运用：

    /* 支持mix-blend-mode 浏览器将会运用的CSS规则 */
    @supports (mix-blend-mode: overlay) { 
        .artwork img { 
            mix-blend-mode: overlay; 
        } 
    } 

    /* 不支持mix-blend-mode 浏览器将会运用的CSS规则 *
    @supports not(mix-blend-mode: overlay) { 
        .artwork img { 
            opacity: 0.5; 
        } 
    }

上面的示例代码将会告诉浏览器，如果支持`mix-blend-mode:overlay`样式规则，将会执行`@supports (...) {...}`中的CSS:

    .artwork img { 
        mix-blend-mode: overlay; 
    } 

对于不支持的浏览器则会执行`@supports not(...) {...}`中的CSS：

    .artwork img { 
        opacity: 0.5; 
    } 

对应的渲染效果如下图所示：

![](/images/ch4/figure-13.png)

### 逻辑操作符

对于逻辑操作符方面`@supports`和`@media`非常类似，它具有的逻辑操作符有`and`、`or`和`not`。

#### `and`逻辑操作符

`and`操作符相当于JavaScript逻辑操作符中的**与**。在CSS中的`@supports`逻辑符`and`，其意思是将两个或多个表达式连在一起，如果每个表达式返回的值都是`true`，则`@supports`的表达式就为**真**；如果其中有一个为假，表达式都将返回为**假**。比如下面这个示例，当且仅当两个原始表达式同时为真时，整个表达式才为真：

    @supports (display:table-cell) and (display: list-item) {
        /* 样式规则 */
    }

#### `or`逻辑操作符

`or`操作符相当于JavaScript逻辑操作符中的**或**。在CSS中的`@supports`逻辑符`or`,其主要用来将两个原始的表达式做逻辑或后生成一个新的表达式，如果两个原始表达式的值有一个为真或者都为真，则生成的表大家式也为真。简单地说，只要表达式中有一个为真，最终的值就是为值。比如：

    @supports (display: -webkit-flex) or (display: -moz-flex) or (display: flex) {
        /* 样式规则 */
    }

#### `not`逻辑操作符

`not`操作符相当于JavaScript逻辑符中的**非**。在CSS中的`@supports`逻辑符`not`可以放在任何表达式的前面来产生一个新的表达式，新的表达式为原表达式的值的否定（即非）。也就是说，如果表达式的值为真，加上`not`其新的表达式的值就是假；如果表达式的值为假，加上`not`其新的表达多的值就是真。

    @supports not(mix-blend-mode: overlay) { 
        /* 样式规则 */
    }

在CSS中的`@supports`的逻辑符除了单个使用之外，还可以混合在一起使用，比如：

    @supports ((margin-left: 0px) or (float:left)) and (background-color: yellow) {
        /* 样式规则 */
    }

> 注意: 在使用`and`和`or`操作符时，如果是为了定义多个表达式的执行顺序，则必须使用小括号。如果不这样做的话，该条件就是无效的，会导致整个规则失效。

### `@supports`对应的JavaScript API

一般情况之下，我们可以使用JavaScript来检测CSS属性是否被浏览器支持，经典的做法是使用`in`操作符来检测：

    if("backgroundColor" in document.body.style){
        document.body.style.backgroundColor = "red";
    }

`@supports`出现之后，我们可以使用另一个API来检测浏览器是否支持CSS。而这个API就是`CSS.supports`，每个支持`@supports`规则的浏览器也将支持这个函数：

    if(CSS.supports("(background-color: red) and (color:white")){
        document.body.style.color = "white";
        document.body.style.backgroundColor = "red";
    }

**`CSS.supports()`**静态方法返回一个布尔值，用来检测浏览器是否支持一个给定的CSS特性。在使用当中，可以通过两种方式来调用`CSS.supports()`：

    // 第一种方式 
    CSS.supports('mix-blend-mode', 'overlay'); // => true 

    // 第二种方式 
    CSS.supports('(mix-blend-mode: overlay)'); // => true

如果浏览器支持`@supports`中的条件则返回`true`，否则返回`false`。

这样一来，就可以很容易根据`.supports()`来做一个判断，做一些事情。比如说，如果浏览器支持`mix-blend-mode: luminosity` 我就给目标元素添加`luminosity-blend`类名，否则就给目标元素添加`noluminosity`类名。

    var init = function() { 
        var test = CSS.supports('mix-blend-mode', 'luminosity'), 
            targetElement = document.querySelector('img'); 
        if (test) { 
            targetElement.classList.add('luminosity-blend'); 
        } else { 
            targetElement.classList.add('noluminosity'); 
        } 
    }; 
    window.addEventListener('DOMContentLoaded', init, false);

## 条件CSS之`@viewport`

先要声明一点，其实`@viewport`并不是真正的条件CSS规则，时至今日，`@viewport`规则被纳入到[CSS Device Adaptation Module Level 1](//www.w3.org/TR/css-device-adapt-1/)模块。估计大部分同学都并未接触过该规则，但对于HTML中`<meta>`元素的`viewport`应该并不会感到太陌生，即 **用来设置视口**。

    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, user-scalable=no">

上面的代码会指示浏览器如何对网页尺寸和缩放比例进行控制：

- 使用元视口代码控制浏览器视口的宽度和缩放比例
- 添加`width=device-width`以便与屏幕宽度（以设备无关像素为单位）进行匹配
- 添加`initial-scale=1` 以便将CSS像素与设备无关像素的比例设为`1:1`
- 确保在不停用用户缩放功能的情况下，你的网页也可以访问

除了设置`initial-scale`之外，你还可以在视口上设置以下属性：

- `minimum-scale`
- `maximum-scale`
- `user-scalable`

但是，设置后，这些属性可以停用用户缩放视口的功能，可能会造成网页访问方面的问题。

而在CSS中，提供了一个`@viewport`规则，可以让我们在CSS中设置类似于`<meta>`标签中的`viewport`。简单地说，`@viewport`规则让我们可以对文档的大小进行设置`viewport`。这个特性主要被用于移动设备，但是也可以用在支持类似“固定到边缘”等特性的桌面浏览器，比如微软的Edge浏览器。

比如：

    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0, user-scalable=no" />

我们就可以使用CSS来描述，而且起到同等的效果：

    @viewport {
        width: device-width;
        initial-scale: 1;
        zoom: 1;
        min-zoom: 1;
        max-zoom: 3;
        user-zoom: fixed;
    }

CSS中的`@viewport`规则中描述符是针对每个文档的，不涉及继承。因此，使用`inherit`关键词的声明将被删除。而且它的工作方式类似于`@page`规则，并遵循CSS的级联顺序。因此，`@viewport`规则中描述符将覆盖前面规则中的描述符。

正如上面的示例代码，`@viewport`规则中包含的规则符常见的主要有：

| 规则属性 | 描述 |
| ----- | ----- |
| `min-width` | 设置`viewport`的最小宽度 |
| `max-width` | 设置`viewport`的最大宽度 |
| `width` | 同时设置`min-width`和`max-width` |
| `min-height` | 设置`viewport`的最小高度 |
| `max-height` | 设置`viewport`的最大高度 |
| `height` | 同时设置`min-height`和`max-height` |
| `zoom` | 设置初始缩放系数 |
| `min-zoom` | 设置最小缩放系数 |
| `max-zoom` | 设置最大缩放系数 |

## 小结

通过上面内容的学习，我想大家对于CSS中的`@`规则应该有了一个基础的了解。但该文花费更多的篇幅是来阐述条件CSS。简单的说，条件CSS主要有`@media`和`@supports`规则，他们都是根据一定的条件表达式，提供相应的样式规则。当条件表达式返回的值是`true`，对应嵌套的CSS规则就会被浏览器渲染；反之即会忽略。而`@media`大多数在为响应式Web设计服务，而`@supports`能更好的为你给浏览器提供不同的样式规则，特别是面对于一些CSS新特性的时候，你可以借这个规则为先进的设备提供最佳、最优的样式规则；而对于不支持新特性的浏览器又能轻易的提供相应的降级样式，从而能更好的为用户提供最佳的样式规则。对于`@viewport`规则，也是CSS`@`规则中的一种，有些人也将其称为条件CSS规则的一部分，但事实它并不是真正的CSS规则。不过，它能让你在CSS样式中更好的设置视口相应的参数。

最后再提一句，不管是条件CSS还是CSS的`@`规则，大特定的场合下都能起到意想不到的功效。当然，大家在使用的时候，应该根据自己的场景选择最适合的方案。比如在响应式设计的时候，选择`@media`就较好，但对于处理刘海设备适配的时候，`@supports`就比较好；而对于纵向适配的时候，`@supports`和`@media`结合可能会更好。所以说，没有最好的，只有最合适的。