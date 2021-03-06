# 前言

## 前言

        RUST是一门系统级别的编程语言。

        作为一门系统编程语言，对于大部分的程序员来说是没有经验的，然而，它确实是我们做任何开发的基础。

        假如你合上笔记本电脑的盖子，操作系统会感知到这个动作，它会挂起所有正在运行的程序，然后息屏，最后让这个系统进入休眠状态。过会，你再打开笔记本电脑：屏幕再一次点亮，每一个被挂起的程序从挂起点启动。我们同意系统这样执行。为了操作系统实现这样的功能，系统开发工程师写了很多代码。

系统程序是指如下的程序：

* 操作系统
* 所有类型的硬件驱动程序
* 文件系统
* 数据库
* 非常简易设备上运行的程序，或者要求非常稳定的设备上运行的程序
* 加密程序
* 多媒体编解码（读写音视频和图片文件的软件）
* 多媒体编辑程序（例如：语音识别，图像编辑软件）
* 内存管理程序（例如：对内存对象的垃圾回收管理）
* 文字的渲染程序（文字和字体转换成像素显示）
* 编写高级编程语言（例如：JavaScript，python）
* 网络程序
* 虚拟化和容器程序
* 系统的模拟器
* 游戏程序

       总而言之，系统程序是使用资源被严格限制的程序。系统程序在运行时，它使用的每个字节和每个CPU都被统计记录的。

      系统代码为了支持一个基本的应用程序所写的代码量是惊人的。

       本书并不会教你怎么写系统程序。实际上，如果你以前没有做过系统程序相关的开发，而本书涵盖了许多看似没必要，而且晦涩难懂的内存管理方面的内容。但是如果你是有经验的系统程序开发人员，你会发现RUST是一门很特别的系统语言：一个全新的开发工具，它消除了在系统软件开发领域众人皆知而且困扰数十年的主要开发问题。

### 什么人应该读这本书

        如果你已经是一位开发系统程序的程序员，并且，准备选择替代C++的开发语言，本书适合你阅读。如果你是有经验的程序员，无论使用的是：C\#，JAVA，Python，JavaScript或者其它语言，本书也适合你。

        然而，你不必立即学习RUST，充分利用这门语言，你也需要获得系统编程的应验。我们推荐阅读本书，同时用RUST也可以编写一些系统程序方面的工程。利用RUST的快速，并发和安全方面的特点尝试开发一些你以前没有开发过的程序。系统编程的范围在前言的开始已经列出了，可以参考。

### 我们为什么写这本书

        我们写这本书的初衷是：当我们开始学习RUST的时候，有这样的一本书供我们学习参考。我们的目标：抓住RUST广泛的，新颖的超前编程理念，有深度的，而且清晰的呈现给读者，通过练习和试错，以最小的学习代价掌握RUST精髓。

### 快速浏览这本书章节的要点

        本书在开始3，4，5章节学习RUST基本类型和所有权，引用等核心功能前，前2章节先介绍了RUST以及提供了一个简单的体验开发。我们建议按顺序读完前5章节。

        第6章节，通过10个简单的例子涵盖了RUST 5个方面的基础知识：表达式（第6章节），错误处理（第7章节），包装库和模块（第8章节），结构（第9章节）以及枚举和模式匹配（第10章节）。先简略的浏览这些基础功能，这是很好的，但是不要跳过关于错误处理的章节。相信我们。

第11章节主要讲了 特性和泛型，最后2个大的语言特性你一定要知道。特性类似Java和C\#的接口，特性和泛型也是RUST扩展自身类型的主要方式。第12章节展示了如何用特性实现操作符重载，第13章节涵盖了更多实用的特性。

理解了特性和泛型之后，我们将开始本书其余章节的学习。闭包和迭代器这两个关键而且强力的工具，相信你不会错过，它们分别在第14章节和第15章节学习。你可以不按顺序学习剩下的章节，按照你的需要，进入相关章节的学习。余下章节涵盖了RUST剩下的知识点：集合（第16章节），字符串和文本（第17章节），输入输出（第18章节），并发（第19章节），宏（第20章节）以及不安全代码（第21章节）。

### 这本书的适用约定规范

本书使用了下列排版规范：

_斜体_

       表示新的术语，URLS，Email地址，文件名和文件扩展名_。_

等宽字体

        用于程序列表，和在段落中一样，涉及到的元素有：变量或者函数名称，数  据库，数据类型，环境变量，状态以及关键字。

**等宽粗体**

        显示命令或者用户定义的类型字面量。

_等宽斜体_

        显示使用用户提供的值或者通过上下文推断的值来被替换的文本。

![&#x8FD9;&#x4E2A;&#x56FE;&#x6807;&#x8868;&#x793A;&#x4E00;&#x4E2A;&#x5C0F;&#x7A8D;&#x95E8;&#x6216;&#x8005;&#x5EFA;&#x8BAE;](.gitbook/assets/1.jpg)

![&#x8FD9;&#x4E2A;&#x56FE;&#x6807;&#x8868;&#x793A;&#x4E00;&#x4E2A;&#x5E38;&#x89C4;&#x7684;&#x6CE8;&#x91CA;](.gitbook/assets/2.jpg)

![&#x8FD9;&#x4E2A;&#x56FE;&#x6807;&#x663E;&#x793A;&#x4E86;&#x4E00;&#x4E2A;&#x8B66;&#x544A;&#x6216;&#x8B66;&#x793A;](.gitbook/assets/3.jpg)

### 使用示例代码

        补充内容（代码示例，练习题等）可在[此处](https://github.com/oreillymedia/programming_rust)。

        这本书可以帮助你完成工作。通常来说，本书提供的示例代码可以直接用到你的代码或者文档里。除非你改写了示例中很重要部分的代码，否则你没必要联系我们获得我们的授权。例如：你写了一个程序，直接用了这本书示例中的几块代码，没必要获得我们的授权。以CD-ROM的方式销售或者分发O’Reilly书中的示例代码需要获得我们的授权。回应一个问题而引用了这本书或者示例无需获得我们的授权。合并大量本书中的示例代码到你的产品文档中需要获得我们的授权。

        引用我们的代码并表明出处我们很感激，但这样做不是必须的。出处通常包含：标题，作者，出版商和ISBN。例如：“RUST 程序开发 作者 Jim Blandy 和Jason Orendorff \(O’Reilly\)。著作权2018 Jim Blandy 和 Jason Orendorff, 978-1-491-92728-1。“

## O’Reilly Safari



![Safari &#x662F;&#x4E00;&#x4E2A;&#x4E3A;&#x4F01;&#x4E1A;&#xFF0C;&#x653F;&#x5E9C;&#xFF0C;&#x6559;&#x80B2;&#x548C;&#x4E2A;&#x4EBA;&#x63D0;&#x4F9B;&#x57F9;&#x8BAD;&#x53CA;&#x53C2;&#x8003;&#x4E66;&#x76EE;&#x7684;&#x5E73;&#x53F0;&#xFF0C;&#x5E73;&#x53F0;&#x91C7;&#x7528;&#x4F1A;&#x5458;&#x5236;&#x3002;](.gitbook/assets/4.jpg)

  
        会员可以访问数千本书，培训视频，学习路线，交互式教程，来自250多家出版商的播放列表，这些出版商包括：奥莱利媒体，哈佛商业评论， 普伦蒂斯霍尔出版社，爱迪生韦斯利出版社，微软出版社， 萨姆斯出版社，科友出版社，派奇彼得出版社，adobe，焦点出版社，思科出版社，约翰威立国际出版社，桑戈斯出版社，摩根考夫曼出版社，IBM红皮书出版社，派克特出版社，adobe出版社，FT出版社，培训课程出版社，曼宁出版社，新骑手出版社，麦格劳希尔出版社，琼斯&巴莱特出版社和专业课程出版社，这只是期中的一部分出版社。

        想获取更多出版社信息，请访问 [这里](http://oreilly.com/safari)。

### 怎么联系我们

 请向出版社提出有关这本书的意见和问题：

         O’Reilly Media, Inc. 

         1005 Gravenstein Highway North 

          Sebastopol, CA 95472 

          800-998-9938 \(in the United States or Canada\) 

          707-829-0515 \(international or local\) 

          707-829-0104 \(fax\)

我们有一个关于这本书的网页，网页中我们罗列了勘误表，示例和附加信息，你可以在[这里](http://bit.ly/programming-rust)访问它。

发邮件到 bookquestions@ oreilly.com ，可以评论或者提问关于本书的技术问题。

想获得更多关于我们出版的书，课程，学术会议和新闻资讯，请访问我们的网站：[http://www.oreilly.com](http://www.oreilly.com)  。

Facebook里发现我们：[http://facebook.com/oreilly](http://facebook.com/oreilly)

Twitter里关注我们     ：[http://twitter.com/oreillymedia](http://twitter.com/oreillymedia)

YouTube里观看我们  ：[http://www.youtube.com/oreillymedia](http://www.youtube.com/oreillymedia)

### 感谢

        你们手中的这本书从我们的正式技术评审员那里获得了极大的关注，评审员包括：Anderson, Matt Brubeck, J. David Eisenberg, 和Jack Moffitt。

        许多非官方的评审员早期读了本书的草稿，反馈了许多非常宝贵的意见。我们非常感谢：Eddy Bruel, Nick Fitzgerald, Michael Kelly, Jeffrey Lim, Jakob Olesen, Gian-Carlo Pascutto, Larry Rabinowitz, Jaroslav Šnajdr, 和Joe Walker提出深思熟虑的评论。尤其是Jeff Walden 和 Nicolas Pierron 付出了大量的时间和精力，修订了几乎整本书的内容。 与任何编程企业一样，编程书也以高质量的bug报告为基础。谢谢。

        Mozilla非常配合我们在这个项目上的工作， 尽管这不属于我们的职责范围，并且与他们竞争关注度。我们非常感谢我们的经理，Dave Camp, Naveed Ihsanullah, Tom Tromey, 和Joe Walker 对这本书的鼎力支持。 他们从长远来看Mozilla是做什么的；而且 我们希望这些结果证明他们对我们的信任是正确的。

        我们也要感谢O 'Reilly的每一个人，他们帮助完成了这个项目， 特别感谢我们的编辑Brian MacDonald和Jeff Bleiel。 

        最重要的是，我们衷心感谢我们的妻子和孩子，感谢他们坚定不移的爱、热情和耐心

