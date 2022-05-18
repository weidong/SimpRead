> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [insights.thoughtworks.cn](https://insights.thoughtworks.cn/zhongtai-ddd-eventstorming/)

> 我们提到业务中台规划的时候，总是会涉及到 DDD 和 EventStorming，并把他们作为核心方法。

白话中台战略系列文章：

*   [白话中台战略 1：中台是个什么鬼？](https://insights.thoughtworks.cn/what-is-zhongtai/)
*   [白话中台战略 2：中台到底长啥样？](https://insights.thoughtworks.cn/what-is-zhongtai-2/)
*   [白话中台战略 3：中台的定义](https://insights.thoughtworks.cn/what-is-zhongtai-definition/)
*   [白话中台战略 4：中台与组织](https://insights.thoughtworks.cn/zhongtai-and-organization/)
*   [白话中台战略 5：中台的第一性原理](https://insights.thoughtworks.cn/zhongtai-first-principle/)
*   [白话中台战略番外篇：从互联网巨头变阵看中台战略](https://insights.thoughtworks.cn/zhongtai-strategy/)
*   [白话中台战略番外篇：DDD、EventStorming 与业务中台](https://insights.thoughtworks.cn/zhongtai-ddd-eventstorming/)

* * *

刚刚结束的 2019 年领域驱动设计峰会（[DDD China Conference 2019](https://mp.weixin.qq.com/s?__biz=MzU0MDg2MDg4Mw==&mid=2247484672&idx=1&sn=b9cc84a40a77e691895a8dff4ca52188&chksm=fb338dbccc4404aa7a47d98a12d0a01db95eb2da4435a9e15f528e6c114090421a0600af08df&token=1038621279&lang=zh_CN#rd)），已经是 DDD-China 的第三年了，也是我参加的第二年，还记得去年分享的是[《当我们谈中台时我们在谈些什么》](https://mp.weixin.qq.com/s?__biz=MzI1MzcwMDg2MA==&mid=2247486154&idx=1&sn=1f6c7ef3636448e1842d21daea929c6e&chksm=e9d13559dea6bc4f3f6aa5d87d715daef66c8fd3b1da703487eb3fe6610c12bc63b80d6f81df&token=530304999&lang=zh_CN#rd)，讲的更多是中台的 Why 和 What，转眼间一年就过去了，弹指一挥间。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/%E7%8E%8B%E5%81%A5-2018-2019-.png)

幸运的是，这一年个人的主旋律仍然还是围绕着中台这个既火热又充满争议的主题，只不过关注点逐渐从 Why 和 What 逐渐过渡到 How，也就是对于如何构建中台的通用方法论思考、研究与应用上。所以今年分享的主题是《中台规划的七种武器》，就当是再给大家做一个年终总结汇报，分享这一年的所思所得。

提到中台（尤其是业务中台）的构建方法论，就不得不提另两个同样伴随着微服务和中台概念兴起的工具：Domain-Driven Design（DDD，领域驱动设计）和 EventStorming（事件风暴）。

在各种讲中台落地规划，尤其是业务中台的共性能力识别和微服务划分的时候，总是能看到这两位的身影。不过相信好多朋友对于这两个相对陌生的面孔还是感觉云里雾里，搞不清楚到底是什么，以及与中台的关系。

本篇就以我个人的经历和视角，为大家讲述一下我对这二位的理解。

### DDD（领域驱动设计）

回想一下，第一次接触 DDD 应该还是十多年前，那还是每天刷着 JavaEye，看一群神仙打架的年代。

依稀记得那时候社区里讨论的最热的也还是 Hibernate 和 OR-Mapping，RoR 也还在蓄势待发，记忆中那也是我最开始接触 DDD 的时候。

所以，现在很多人以为 DDD 是个新冒出来的东西，其实并不是，这东西已经有了 10 多年的历史了，豆瓣上还有第一版领域驱动设计的蓝色版本的封面（不知道谁还有这个版本，反正我的是早就丢了）。而第一版的出版年份被定格在 2006 年 3 月 1 日，距今已经过去了 13 年。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/1-%E9%A2%86%E5%9F%9F%E9%A9%B1%E5%8A%A8%E8%AE%BE%E8%AE%A1%E7%AC%AC%E4%B8%80%E7%89%88.png)

DDD 虽然诞生的早，但直到现在，和我们日常工作的距离也并不远，例如我们代码中经常出现的 XxxEntity，XxxRepository ，XxxService，XxxFactory…… 这些类，这里的 Entity、Repository、Service 和 Factory，以及我们现在大家早已习以为常的分层架构，都是出自 DDD 之手，只不过很多人已经不知道这些都是拜 DDD 所赐而已。

我们中的大多数人就像 [“猴子定律”](https://baike.baidu.com/item/%E7%8C%B4%E5%AD%90%E5%AE%9A%E5%BE%8B/6268365) 里后来的猴子们一样，习惯了约定俗成，日复一日重复着（Controller-Service-Repository-Entity => CRUD）这些看似枯燥的操作，而早已忘了为什么一开始架构和这些所谓的最佳实践被设计成这个样子，少了一份溯本求源的精神。

以致于我经常在面试的时候，问候选人，你这里为什么要一个 Service 呀？大多数的回复都是：这是规范呀！这不是基本常识么？我们公司规定都得这么写的……

那说来说去，DDD 到底是什么呢？

在我看来，**DDD 其实就是面向对象的一套 “方言”，提供了一种基于业务领域的对象划分和分类方法，其最大的价值就在于对于软件开发过程全生命周期使用语言的统一！**

怎么讲呢？

在面向对象的世界，只告诉了我们万事万物皆是对象。但并没有告诉我们对象究竟应该怎么组织，该以什么角度进行划分。

而作为技术人员的我们，早已习惯了从技术的角度出发思考，自然就出现了 “用技术的语言” 定义对象的习惯，例如最常见的 DAO（Data Access Objects），DTO（Data Transfer Object）这类对象的划分方式就是使用技术视角看待和分类对象的典型案例。

这样从技术视角分类对象的问题，就在于在软件的开发过程中，会涉及到多 “语言” 间的对应和翻译，例如最常见的就是业务语言和技术语言的相互翻译。想想周围经常出现的研发和产品之间各种痛苦的沟（吵）通（架）场景，应该就知道我说的意思了。

这个过程其实和两种不同语言（例如中文和英文）的人之间的沟通是一样的，而且更加隐蔽，因为说的明明都是中文，但是彼此就是听不懂彼此说的到底是什么意思，而且都觉得对方像个傻子，听不懂人话，殊不知其实两个人说的本身就不是同一种语言。

而 DDD 的出现，就是为了解决这个问题。它通过一套面向对象的分类方法或是方言体系，从领域出发，实现软件开发过程中各个角色和环境的 **“统一语言”（UBIQUITOUS LANGUAGE）**。例如使用 “仓库（Repository）” 替代“数据访问对象（DAO）”，就更能让业务和技术同时理解这个对象的作用。

就像车同轨，书同文一样。

好了，了解了 DDD 的来历和价值，那你肯定还有疑问，既然 DDD 这个概念已经快被人们淡忘了 10 多年了（虽然我们一直还在不知不觉中僵硬地应用着），那为什么最近一两年又突然重出江湖，重新被大家关注了呢？

答案其实就两个：**微服务和中台**。

说到这里还有个段子：

其实 DDD 一开始的时候，就把领域分析设计分为了战略（Domain、Bounded Context）和战术（Entity、VO、Repository、Service……）两个部分。按道理应该从战略入手，再下沉到战术部分。但是 Eric Evans 在写《Domain-Driven Design》这本书时，可能是基于当时的环境，却是先写的战术部分，书的后半部分才开始展开 DDD 的战略部分。

而又因为战术部分本身就很复杂很枯燥（各种图各种代码），所以很多人并没有坚持到读后边的战略部分，就读不下去（比如我）。导致的结果就是这么多年战术部分被大家充分的讨论和应用，而战略部分的影响则相较之下非常有限，讨论和应用的也不多。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/2-DDD.png)

直到微服务的横空出世……

随着微服务架构的兴起，微服务到底如何拆分就成了人们最最关注的问题。这时候一些 “老人儿” 们突然想起来，这不是正是应用 DDD 中战略部分（问题域，限界上下文）应用和施展拳脚的场合么？！

所以随着微服务的爆炸式发展，DDD 这位退隐已久的老江湖，又再次被请出了山，站到了大家的面前。

而此时，他身边还多了一个年轻的新搭档，他正是：事件风暴。

### EventStorming（事件风暴）

现在，当很多人谈到 DDD 都会同时谈到 EventStorming，甚至有人误认为这两个名词本身指代的就是同一个概念。

但其实这是两个完全独立的工具。

DDD 是一套基于领域的分析和建模方法，而 EventStorming 则是一套 Workshop（可以理解成一个类似于头脑风暴的工作坊）方法。DDD 出现要比 EventStorming 早了 10 多年，而 EventStorming 的设计虽然参考了 DDD 的部分内容，但是并不是只为了 DDD 而设计的，是一套独立的通过协作基于事件还原系统全貌，从而快速分析复杂业务领域，完成领域建模的方法。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/3-EventStorming.png)

EventStorming 最初由 Alberto Brandolini（曾受邀参加了 DDD-China 2017）开发，经过 DDD 社区和 ThoughtWorks 的很多团队实践验证后，于 2015 年 11 月进入 [ThoughtWorks 技术雷达](https://www.thoughtworks.com/radar/techniques/event-storming)，开始被更多人知晓和应用，从此成为 DDD 的最佳拍档。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/5-EventStorming.png)

关于 EventStorming 的具体内容和细节，已经有很多文章都介绍了，不过还是强烈建议先看一下 eventstorming.com 官网的那本作者 Alberto Brandolini 自己写的《Introducing EventStorming》，应该是最官方最正宗的对于 EventStorming 的介绍。

而书中的下面这个图，我认为是对于 EventStorming 最清晰、完整且简单的概括。完整的阐释了从系统外部与用户的交互，到系统内（事件 - 策略 - 命令 - 聚合 - 事件）的事件传递涟漪，以及通过事件影响到读模型从而给予用户动作的响应，从而形成完整闭环的全过程。对我们了解还原系统的全貌非常有帮助。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/4-EventStorming-1.png)

前面提到了，EventStorming 和 DDD 两个独立的工具，形式不同、解决的也是不同的问题，那为什么这两者总是搭档出现呢？

关键就在于下图这条红线，就像是月老手中的红线一样，将两个概念牵在了一起。

DDD 提供了一套面向对象的 “方言”，给出了一套面向对象的分类框架和架构指引，但是在 DDD 中并没有明确给出如何为一个系统识别出这些不同种类的对象的过程和方法。

而 EventStorming 的出现正好弥补了这个空白，通过 EventStorming 工作坊的方式，正好给我们提供了一个还原和分析系统的方法，并最终通过 “聚合” 这条红线，穿越时空，无缝切入到 DDD 的领域范畴之内，以 “聚合” 为支点，向上可以进一步做问题域和限界上下文的战略分析，向下则可以通过聚合的进一步展开进行实体、值对象等相关的战术分析，引导落地。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/6-EventStormingDDD.png)

而 DDD 战略设计中问题域和限界上下文的识别，则为我们划分微服务提供了非常强有力的支撑，至此我们就通过 EventStorming 和 DDD 的这一对强力组合的联袂辅助下，找到了解决了微服务架构下最困难问题之一的，既服务该如何划分问题的解题思路和落地方法。

那说了半天微服务，DDD 和 EventStorming 到底又与业务中台有什么关系呢？

### DDD、EventStorming 与业务中台

我之前的文章曾提到过业务中台与微服务的关系，而当时我的观点很简单，就是：没有直接关系。

业务中台解决的是企业级能力复用的问题，而微服务解决的是运行时解耦的问题。业务中台不一定是微服务的，采用微服务架构的也不一定就是业务中台，这些之前都提到过，这里就不赘述了。

所以很多人以为使用 DDD 和 EventStorming 规划业务中台，是因为微服务，在我看来，基于以上的分析这也是站不住脚的，难道我不用微服务架构来实现业务中台，就无法使用 DDD 和 EventStorming 了么？

DDD 和 EventStorming 的结合，形成的是一套 “领域分析方法”，可以想象成一套双剑合璧的剑阵。其目的是透过现象看本质，透过表面的业务流程来分析背后的核心领域问题和概念，而微服务架构只是使用了这套能力，来指导和帮助进行服务划分而已，并且也不是唯一的指导原则，其他还需要考虑像团队组成、变更频率、技术异构边界、SLA 要求等等因素……

而对于业务中台，这套领域分析方法，则可以指导我们探究与分析业务中台规划过程中的一个最困难的问题，既：识别不同的业务线，到底有哪些业务是可以复用的？

因为如果只分析表面的功能和流程，我们总是会发现虽然不同的业务看似都差不多，但总是有不同的地方，很难抽取共性。

这就像我们看每个人，乍一看都长得差不多，一个脑袋一个身子，俩个胳膊俩条腿。但是仔细观察，又会发现每个人都是不一样的，细节上还是有很大的差别，长相不同，性格不同，做事方式也不一样。

而领域分析就像是给人照个 B 超一样或是做一个性格分析一样，透过外表和行为，分析背后的本质和机理，寻找不同背后的相同，找寻变化背后的不变。

而这种通过领域分析和抽象，找寻不同业务线背后面对的相同的问题域，并从中提取共性的业务模型、提取共性的业务功能、提取共性的业务流程、甚至是提取共性的业务模式，加工并予以复用的过程，也正是业务中台的规划与建设过程的关键所在。

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/7-EventStormingDDD%E4%B8%AD%E5%8F%B0.png)

这也是为什么当我们提到业务中台规划的时候，总是会涉及到 DDD 和 EventStorming，并把他们作为核心方法的原因。

### 总结

最后，我们用武侠小说中的一段场景来收尾做个总结：

> 江湖上流传着一个人的传说：
> 
> 十年前，他一身白衣飘飘，仗双剑叱咤江湖，但只用一剑，已天下无敌，从来没有人看到过他的另一把剑出鞘。
> 
> 十年间，江湖上早已不见了白衣人的踪迹，人们虽然还在不断地传承着这流传下来的无名剑法，但忘已忘却了这个剑法背后的那个人。
> 
> 十年后，江湖又是一场大乱，白衣人又重现江湖，虽然已没有几个人认识他，但是认识的人看到他之后都无不大吃一惊。
> 
> 这十年，他的容貌没有一点变化。
> 
> 但让人吃惊的不是他的容貌，而是他的剑：此时，双剑都已出鞘！
> 
> 而他的身旁，还站着另一个持剑的少年，一样的白衣飘飘，一样的冷峻与潇洒，让人们不禁想起了白衣人十年前的样子。
> 
> 两个人，三把剑，一套剑阵，能否能平息这场江湖大乱？
> 
> 或是否还会有其他的英雄会横空出世？
> 
> 棋至中盘，一切都还未成定论，让我们一起拭目以待！

![](https://insights.thoughtworks.cn/wp-content/uploads/2019/12/ddd%E6%BC%94%E8%AE%B2PPT%E4%B8%8B%E8%BD%BD-banner.png)

* * *

白话中台战略系列文章：

*   [白话中台战略 1：中台是个什么鬼？](https://insights.thoughtworks.cn/what-is-zhongtai/)
*   [白话中台战略 2：中台到底长啥样？](https://insights.thoughtworks.cn/what-is-zhongtai-2/)
*   [白话中台战略 3：中台的定义](https://insights.thoughtworks.cn/what-is-zhongtai-definition/)
*   [白话中台战略 4：中台与组织](https://insights.thoughtworks.cn/zhongtai-and-organization/)
*   [白话中台战略 5：中台的第一性原理](https://insights.thoughtworks.cn/zhongtai-first-principle/)
*   [白话中台战略番外篇：从互联网巨头变阵看中台战略](https://insights.thoughtworks.cn/zhongtai-strategy/)
*   [白话中台战略番外篇：DDD、EventStorming 与业务中台](https://insights.thoughtworks.cn/zhongtai-ddd-eventstorming/)