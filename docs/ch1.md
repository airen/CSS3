# 揭开CSS的面纱

如果您对前端方面有所关注，那么对CSS一定不会陌生，你也肯定听说过一些CSS的新特性。在使用CSS新特性之前，你应该对这个新一代样式表语言的来龙去脉有个基本了解。在本章节中，你将知道一个CSS属性的制定将会经历哪些过程，为什么会有浏览器的私有前缀以及如何更好的处理这些私有前缀，在文章最后简单的介绍了开发人员如何对CSS新特性做一些渐进增强，优雅降级的处理，给你的用户有一个更好的，更佳的体验。

## Web标准

从维基百科中，我们可以得知。Web标准又被称为网页标准，一般是指有关于万维网各个方面的定义和说明的正式标准以及技术规范。近年来，这个术语也时常和一套建立网站的标准化的最佳实践方法、网页设计的原理、以及上述方法的衍生物连续在一起。

Web标准不是一种单一的标准规范，而是由一些规范共同组成的标准集合，是由W3C和其它的标准化组织共同制定，用来创建和解释基于Web的内容。这些规范是专门为了那些在网上发布的可向后兼容的文档所设计，使其能够被大多数人所访问。

这些标准和规范往往彼此相依，其中一部分甚至延伸到互联网，而不仅限于万维网，并直接或间接的影响到网站以及Web服务的发展和管理。同时也考量到网页或网站的协同工作能力、无障碍性、易用性。

网页主要由三部分组成：结构（Structure）、表现（Presentation）和行为（Behavior）。

![HTML和CSS验证标记](images/ch1/figure-6.png)

如果有网站或网页宣称遵循网页标准，通常表示他们的网页符合HTML（或HTML5，或XHTML）、CSS和JavaScript等标准。对应的标准也分三方面：结构化标准语言主要包括XHTML和XML，表现标准语言主要包括CSS，行为标准主要包括对象模型（如W3C DOM）、ECMAScript等。而我们要讲的CSS就是Web标准中的表现标准语言。

CSS是Cascading Style Sheets层叠样式表的缩写。W3C创建CSS标准的目的是以CSS取代HTML表格布局、帧和其他表现的语言。

### 标准的制定过程

W3C并不直接产生标准。W3C以工作组的方式，把某项技术的相关各方聚集在一起，最终由他们来产生标准。当然，W3C并不只是一个观察者，他设定了整个平台的规则，也会监督整个进程。但这些技术规范，基本上并不是由W3C工作组的工作人员编写完成的。而我们所说的CSS规范通常是由W3C工作组中的CSS工作组的成员来编写。其人员组成主要由来自W3C会员公司的成员（比如浏览器厂商、主流网站、研究机构和常规技术公司等）、特邀专家（被邀请参与标准制定的Web开发者）和W3C工作人员三个部分组成。

很多Web开发者普遍认为W3C手握标准，主宰Web的生杀大权，而浏览器厂端们则不敢不遵从。事实上并非如此，哪些东西该进入Web标准，浏览器厂商比W3C有更多的发言权，因为浏览器实不实现制定下来的标准，还是由他们说了算。

另外很多Web开发者普遍都会认为，Web标准与自己无关，自己也无法参与。事实上制定Web标准并不是闭门造车。CSS工作组一直坚持透明原则，它内部所有的交流都是公开的，并邀请公众来关注和参与讨论：

- 绝大多数的讨论都发生在工作组的邮件列表中：[www-style](lists.w3.org/Archives/Public/www-style)）。这个邮件列表是公开布档的，欢迎任何人的参与
- 每周都会召开一次电话会议，时长一小时。该会议并不向非工作组成员开放，但会议会被记录在[W3C的IRC服务器](//irc.w3.org/)）上的#css频道。这些会议也会整理出来发布到邮件列表中
- 还有每个季度会有一次面对面会议，也会记录下来。在获得工作组主席的许可之后，这类会议也通常会对观察员开放（就是旁听）

所有这些都是W3C进程的一部分，任何决定都是通过这样的方式来产生的。此外，那些真正负责把这些决定写成规范的人员叫作规范编辑。规范编辑可能是W3C的工作人员、浏览器开发者、相关专业的特邀专家，也可能是会员公司的职员，他们全职从事此项工作，为了共同利益去推进标准。

CSS的每项规范大致都会经历下面这几个过程：

- **编辑草案(ED)**：这是一项规范的初始阶段，可能非常粗糙。对这个阶段没有什么要求，也不保证它会被工作组批准。但它也是各个修订版本的必经阶段，每次变更都是先从一个 ED 中产 生的，然后才会发布出来。
- **首个公开工作草案(FPWD)**：一项规范的首个公开发布版本，它应该准备就绪，以接受工作组的公开反馈。
- **工作草案(WD)**：在第一个 WD 之后，还会有更多的 WD 出来。 这些 WD 会吸收来自工作组和更广阔的社区的反馈，一版接着一版小幅改进。浏览器的早期实现通常是从这个阶段开始的，厂商基本不太可能对更早阶段的草案提供实验性的支持。
- **候选推荐规范(CR)**：这可以认为是一个相对稳定的版本。此时比较适合实现和测试。一项规范只有具备一套完整的测试套件和两个独立的实现之后，才有可能继续推进到下一阶段。
- **提名推荐规范(PR)**：这是 W3C 会员公司对这项规范表达反对意见的最后机会。实际上他们很少在这个阶段提出异议，因此每个 PR 推进到下一阶段(也是最后一个阶段)只是时间问题。
- **正式推荐规范(REC)**：一项 W3C 技术规范的最终阶段。

用W3C上的一张图来向大家展示一下一个CSS属生诞生的历程：

![标准的诞生历程](/images/ch1/figure-7.jpg)

### 采用Web标准的好处

对于Web页面或者Web应用程序，采用Web标准是有一定的好处。首行对于访问者：

- 文件下载与页面显示速度更快。 
- 内容能被更多的用户所访问（包括失明、视弱、色盲等残障人士）。 
- 内容能被更广泛的设备所访问（包括屏幕阅读机、手持设备、搜索机器人、打印机、电冰箱等等）。 
- 用户能够通过样式选择定制自己的表现界面。 
- 所有页面都能提供适于打印的版本。

对于网站所有者： 

- 更少的代码和组件，容易维护。 
- 带宽要求降低（代码更简洁），成本降低。
- 更容易被搜寻引擎搜索到。 
- 改版方便，不需要变动页面内容。 
- 提供打印版本而不需要复制内容。 
- 提高网站易用性。在美国，有严格的法律条款（Section 508）来约束政府网站必须达到一定的易用性，其他国家也有类似的要求。

早期的Web的开发者对于自己开发的Web页面，都会通过W3C验证服务来验证自己开发的项目。比如[HTML的验证](//validator.w3.org/）和[CSS的验证](//jigsaw.w3.org/css-validator/)，以在页脚附上下图（图1-1）引以为荣，从而向用户展示自己的Web网页是结得住考验的。

![HTML和CSS验证标记](images/ch1/figure-1.jpg)

*图1-1：HTML和CSS验证标记*

## CSS版本之争

要了解CSS版本之间的故事，就得对CSS的起源有所了解。

CSS开始发展于1994年。最初的CSS提案计划于1994年11月在芝加哥举行的Web会议上提出。而实际上CSS的第一个版本，也就是CSS 1 的规范是1996年由 Håkon Wium Lie 和 Bert Bos发表。CSS1的规范非常的短，而且比较简单。它的内容少到用一个HTML页面就足以呈现，即使用A4的纸打印出来也只需要68页。

1997年2月，CSS成立W3C旗下自己的工作小组，新的工作小组去解决CSS1中没有解决的问题。工作小组有Chris Lilley领导，他是来自曼彻斯特大学的爱尔兰人。CSS2在1998年5月成为推荐标准，它的定义更加严格，囊括了更多的功能。

在 CSS 2 之后，CSS 工作组意识到这门语言已经变得非常庞大，再也无 法把它塞进单个规范中了。这样不仅阅读和编辑极其困难，而且限制了 CSS 本身的快速发展。别忘了，**一项规范如果要推进到最终阶段，其中的每项特 性都必须具备两个独立的实现和全面的测试**。为了促进CSS能得到更好的发展以规范推进速度。所以W3C组织决定将CSS以不同的功能模块细分出去，而且每个功能模块都可以独立更新版本。这其中，那些延续 CSS 2.1 已有特性的模块 会升级到 3 这个版本号。比如以下模块:

- CSS 语法 【[http://w3.org/TR/css-syntax-3](http://w3.org/TR/css-syntax-3)】
- CSS 层叠与继承 【[http://w3.org/TR/css-cascade-3](http://w3.org/TR/css-cascade-3))】
- CSS 颜色 【[http://w3.org/TR/css3-color](http://w3.org/TR/css3-color)】
- CSS 选择器 【[http://w3.org/TR/selectors](http://w3.org/TR/selectors))】
- CSS 背景与边框 【[http://w3.org/TR/css3-background](http://w3.org/TR/css3-background)】
- CSS 值与单位 【[http://w3.org/TR/css-values-3](http://w3.org/TR/css-values-3))】
- CSS 文本排版 【[http://w3.org/TR/css-text-3](http://w3.org/TR/css-text-3))】
- CSS 文本装饰效果 【[http://w3.org/TR/css-text-decor-3](http://w3.org/TR/css-text-decor-3)】
- CSS 字体 【[ttp://w3.org/TR/css3-fonts](ttp://w3.org/TR/css3-fonts)】
- CSS 基本 UI 特性 【[http://w3.org/TR/css3-ui](http://w3.org/TR/css3-ui)】

此外，如果某个模块是前所未有的新概念，那它的版本号将从 1 开始。 比如下面这些:

- CSS 变形 【[http://w3.org/TR/css-transforms-1](http://w3.org/TR/css-transforms-1))】
- 图像混合效果 【[http://w3.org/TR/compositing-1](http://w3.org/TR/compositing-1)】
- 滤镜效果 【[http://w3.org/TR/filter-effects-1](http://w3.org/TR/filter-effects-1))】
- CSS 遮罩 【[http://w3.org/TR/css-masking-1](http://w3.org/TR/css-masking-1))】
- CSS 伸缩盒布局 【[http://w3.org/TR/css-flexbox-1](http://w3.org/TR/css-flexbox-1)】
- CSS 网格布局 【[http://w3.org/TR/css-grid-1](http://w3.org/TR/css-grid-1)】

> **提醒作者：上面list需要去验证，更新，并且提供CSS所有功能模块的清单列表**

广大开发者习惯性的延续了CSS 2.1之样的称呼，所以大家喜欢把CSS的新特性称之为CSS3，甚至称之为CSS4（比如最新版本的选择器，很多地方就称为CSS4选择器）。尽管CSS3这个词非常流行，但实际上并没有在任何规范中定义过CSS3。事实上，绝大多数编辑在提到这个词时，指的是一个非正式的集合，它包括CSS规范第三版（Level 3）再加上一些版本号还是Level 1的新规范，比如，CSS网格布局。尽管在哪些规范应该归入 CSS3 的问题上，编辑们达成了一定的共识，但我们也不得不面对现实:

> 由于CSS 的各个模块在近些年里以不同的速度在推进，我们已经越来越难以把这些规范以CSS3、CSS4这样的方式来划分了，而且这样也难以被大众理解和接受。

**所以，大家以后不要再把CSS按CSS3或者CSS4来称谓了，我们应该改变以前的习惯，按功能模块发布的版本号来称呼他们。这样才不会给别人造成误解或困惑！**

## 浏览器前缀

CSS中的很多属性并不是一下子就确定的，确定使用是需要一个很漫长的过程，正如前面提到的一样，会经过`ED`、`FPWD`、`WD`、`CR`、`PR`和`REC`几个过程，但有一些实验性阶段或非标准的阶段的CSS属性也会得到浏览器厂商们的支持。开发者在使用这些试验性阶段的CSS属性需要添加各浏览器引擎的前缀，这样开发者就可以在使用这些试验阶段的代码 时能够确保不会被其他标准代码所依赖而导致破坏标准Web代码的问题。开发人员应该等到浏览器厂商们取消前缀。

### CSS前缀

到目前为止，主流浏览器引擎的前缀常见的主要有：

- `-webkit-`：Chrome、Safari和新版Opera浏览器，国内的UC浏览器使用的也是`-webkit-`前缀
- `-moz-`：Firefox浏览器
- `-ms-`：IE和现在微软新浏览器Edge

很多同学应该还看到过`-o-`前缀，Opdera旧内核对应的前缀，不过现在不怎么使用了。

### 如何处理浏览器前缀

对于发开者而言，很多时候他们是不知道哪些浏览器还需要添加私有前缀，哪些属性已经不需要添加私有前缀了。往往会因为前缀的问题，在部分浏览器上产生异常的样式渲染。于时开始有人在讨论“**如何在编写CSS时不需要添加浏览器私有前缀，又能让浏览器识别**”？

为了解决手工书写前缀的问题，最早的一个解决方案是由[@Lea Verou](//lea.verou.me/)提供的一个[`-prefix-free`脚本](//leaverou.github.io/prefixfree/)。

![Prefix free](/images/ch1/figure-2.png)

你只需要在你的`.html`文件中引入一个`prefixfree.js`文件即可，建议把这个脚本文件放在样式表之后。添加这个脚本之后，使用CSS属性时，只需要书写标准样式即可。但是这种做法将所有压力交给了客户端来处理。如此一来页面解析压力就大了，性能会打一定的折扣，并且一旦脚本加载失败，那么就会出现浏览器无法正常渲染需要带有私有前缀的CSS样式风格。

这种方式到目前为止来说有点过时了，但在一些在线编辑器还是提供了相应的选项，比如[Codepen](//codepen.io/)在线编辑器就提供了相应的选择：

![Codepen with Prefixfree](/images/ch1/figure-3.png)

随着CSS处理器越来越普及，部分同学开始采用CSS处理器来处理浏览器私有前缀。比如早期的Sass中，通过处理器中的混合宏来处理私有前缀，其中最有名的应该是Compass了，他就是使用Sass的`mixin`为CSS需要带有前缀的属性定制了一些`mixin`。还有类似于Stylus中的nib等。这些混合宏确实可以解决很多问题，但也产生了新的总是，就是它所使用的语法是全新的，如果要使用就必须重新学习，另外这类工具的演进速度通常都会跟不上浏览器的发展速度，这样也会造成其产生的CSS有过时的问题，有时候为了解决一些问题，还需要自己去写`mixin`，比如下图所示：

![Sass mixins](/images/ch1/figure-4.png)

使用Sass、LESS、Stylus或者其他类似的工具可以处理CSS属性的私有前缀，但都不是最佳的方式。具体原因前面也简单的描述过了，这里不再累述。时至今天，在社区中处理CSS属性的私有前缀最受欢迎的应该是PostCSS的插件[Autoprefixer](//github.com/postcss/autoprefixer)了。

![Autoprefixer](/images/ch1/figure-5.jpg)

Autoprefixer是PostCSS有一个插件，也称得算是一个处理器程序，只不过大家都喜欢把他称为后处理程序。它可以同Sass、LESS或Stylus等CSS处理器一起使用。使用Autoprefixer之后，你只需要关注怎么书写CSS，不再需要关注为哪些属性添加私有前缀。

Autoprefixer是以 [Rework](//github.com/visionmedia/rework) 这种架构所发展的CSS后处理器，他是将CSS代码解析后转成JavaScript的资料结构，进行处理后再产生新的CSS代码。

Autoprefixer会分析CSS代码，并且根据[Can I Use](//caniuse.com/)所提供的资料来决定要加上哪些浏览器前缀，而你要做的事情就是把他加入自己的自动化开发工具中（比如Gulp或Webpack），然后就可以直接使用W3C的标准来写CSS，不需要加上任何浏览器的私有前缀。

Autoprefixer使用也非常的简单，可以通过下载相应的插件配置到你自己的IDE编辑器上，比如Sublime、VS Code或Atom等。除此之外还可以很简单、有效地同现有的打包工具，比如Gulp、Webpack等一起使用，来完成对项目中所有的`.css`文件中的属性添加私有前缀。

Autoprefixer如何和构建工具一起使用，在官方上已有相应的配置说明，比如Gulp环境之下，你可以这样配置：

    gulp.task('autoprefixer', function () {
        var postcss      = require('gulp-postcss');
        var sourcemaps   = require('gulp-sourcemaps');
        var autoprefixer = require('autoprefixer');

        return gulp.src('./src/*.css')
            .pipe(sourcemaps.init())
            .pipe(postcss([ autoprefixer() ]))
            .pipe(sourcemaps.write('.'))
            .pipe(gulp.dest('./dest'));
    });

而在流行的Webpack中，配置也不复杂。

    npm install --save-dev css-loader style-loader postcss-loader autoprefixer

执行上面的命令，安装所需的包，然后在项目中的`webpack.config.js`中添加以下配置：

    module.exports = {
        module: {
            rules: [
                {
                    test:/\.css$/,
                    use:[
                        {
                            loader:'style-loader'
                        },
                        {
                            loader:'css-loader',
                            options:{
                                modules:true,
                                importLoaders:1,
                                minimize: true,
                                localIdentName: '[local]_[hash:base64:5]'
                            }
                        },
                        {
                            loader:'postcss-loader'
                        }
                    ]
                }
            ]
        }
    }

在项目根目录下创建`postcss.config.js`，并且设置支持哪些浏览器：

    module.exports = {
        plugins: [
            require('autoprefixer')({
                "browsers": [
                    "defaults",
                    "not ie < 11",
                    "last 2 versions",
                    "> 1%",
                    "iOS 7",
                    "last 3 iOS versions"
                ]
            })
        ]
    };

个人更建议把`browserslist`放在项目的`package.json`文件中或者单独在根目录下创建一个`.browserslistrc`的配置文件。这样做可以允许Autoprefixer和eslint-plugin-compat共享此配置。

`browserslist`有很多配置参数，具体的[可以查阅其官方文档](//github.com/browserslist/browserslist)。这里例举一下其常见的参数说明：

- `>5%`: 浏览器版本的全球占有率(这里指兼容浏览器版本全球占有率超过`5%`的) `>=`, `<` 和 `<=`
- `iOS 7`: 直接兼容iOS 7的浏览器
- `last 2 versions`: 兼容每个浏览器最新的两个版本
- `not ie <= 8`: 去兼容这些浏览器版本之外的其他版本

有了Autoprefixer这样的工具对于处理CSS属性前缀来说就不再是头痛的事情了。当然，如果你正在使用CSS预处理器编写代码，那么也可以很完美的结合Autoprefixer去处理。

## 渐进增强

第一次听到“渐进增强”(Progressive Enhancement)一词是在前端重构交流会上。渐进 增强并不是一种技术，而是一种开发的方式，更是一种 Web 设计理念。首先思考一个问题:“网站是否需要在每个浏览器中看起来都一样?”带着这个问题来看渐进增强。

### 渐进增强与优雅降级

正如前面所言，渐进增强是一种开发方式，更是一种设计理念。在编写 Web 页面时， 首先保证最核心的功能实现，让任何低端的浏览器都能看到站点内容，然后考虑使用高级 但非必要的 CSS 和 JavaScript 等增强功能，为当前和未来的浏览器提供更好的支持，给用 户带来更好的体验。

在设计的时候，先考虑低端设备用户能否看到所有内容，然后在此基础之上为高端用户进行设计。不仅为高端设备用户提供一个完美的应用，也要为不同性能级别设备的用户设计不同级别的不那么完美的应用。这称为“优雅降级”。

目前而言，虽对“渐进增强”有所了解的人很多，但是要说普及或深入还远远没到时 候。在大家平时的设计思维中有一种极强的固定思维，也就是想让网站在各个浏览器下表 现一致。这种出发点本身并没有什么问题，但是这样会让领先的浏览器的优势无法充分显 示出来。

因此，从今天开始要改变制作 Web 站点的思维，让网站能优雅降级，目标应该是—— 向尽可能多的用户提供尽可能优质的用户体验。这跟用户访问网站使用方式无关，无论通过iPhone、高端的桌面系统、Kindle，还是阅读器，用户都能得到尽可能独特且完美的体验。

### 渐进增强的优点

向尽可能多的用户提供尽可能优质的用户体验”这一目标听起来相当不错。有的人制作 Web 站点时报怨，怎样才能使用CSS新特性，使用了CSS新特性，怎么才能让不支持的浏览器也能正常渲染。诚然，我们不使用渐进增强也可以实现， 如为一些旧浏览器提供一套兼容方案，确保页面与现代浏览器保持一致。简单来说就是在支持CSS新特性的浏览器中使用新特性，在不支持的浏览器中另写一套样式来模拟新特性的效果，实现让网站在所有浏览器都一样。

可以说，通过这种方法只是让低版本的浏览器渲染页面更好看一点，并没有得到实质性的提高。

因此，如果网站始终无法做到一模一样，为什么不使用 CSS新特性，使它在现代浏览器上看起来更靓丽呢?当然，某些CSS特性在不支持的浏览器中是“无法模拟”的，但使用渐进增强，就无须为了让网站适合所有人而放弃这些技术。

CSS 的渐进增强有别于 CSS 的 Hack。Hack 是浏览器厂商的一种手法，用来增强自己的 竞争，而渐进增强起到锦上添花之效。所以前者应该尽量避免使用，而后者应该适当使用。

在CSS中，我们可以使用CSS的新特性`@supports`来帮助我们做渐进增强和优雅降级相关的处理。比如在iPhone X这样的浏海处理，我们就可以借助该特性来做：

    :root { 
        --navBarHeight: 88px; 
        --footerBarHeight: 150px; 
        --safe-area-inset-bottom: env(safe-area-inset-bottom); 
        --safe-area-inset-top: env(safe-area-inset-top); 
    } 
    .nav-bar{ 
        height: var(--navBarHeight, 88px); 
        box-sizing: content-box; 
    } 
    .footer-bar { 
        hegiht: var(--footerBarHeight, 150px); 
        box-sizing: content-box; 
    } 
    .nav-bar ~ .content { 
        padding-top: var(--navBarHeight, 88px) 
    } 
    .footer-bar ~ .content { 
        padding-bottom: var(--footerBarHeight, 150px) 
    } 

    @supports (padding-top:env(safe-area-inset-top)){ 
        .nav-bar { 
            padding-top: var(--safe-area-inset-top); 
        } 
        .footer-bar { 
            padding-bottom: var(--safe-area-inset-bottom) 
        } 
        .content { 
            padding-top: var(--safe-area-inset-top); 
            padding-bottom: var(--safe-area-inset-bottom); 
        } 
        .nav-bar ~ .content { 
            padding-top: calc(var(--safe-area-inset-top) + var(--navBarHeight)); 
        } 
        .footer-bar ~ .content { 
            padding-bottom: calc(var(--safe-area-inset-bottom) + var(--footerBarHeight)) 
        } 
    }

有关于`@supports()`特性更详细的介绍，我们后续将会专门用一个章节来进行阐述。这里只是抛个引子，告诉大家，如何使用`@supports()`可以做一些渐近增强，优雅降级的处理。

## 小结

本文主要介绍了一个CSS属性是怎么制定而来，从提出到成为规范需要经历哪些过程。同时CSS将再无大版本一说，只会按各功能模块进行划分。介绍了为什么会有浏览器的私有前缀出现，以及有哪些方式可以更好的帮助我们更好的处理CSS属性的私有前缀。最后简单的聊了一下下如何对新特性做一些降级处理。在接下来的一章中，我们将开启CSS中另一个重要概念的学习，即CSS层叠和继承！感兴趣的同学，欢迎持续关注相关的更新。




