# 利用"提交注释”和缺陷报告提供上下文

你掉入了陷阱。很多机关在你周围旋转，但它们毫无意义。你拼命搜索房间却找到建筑师的原始设计图纸！陷阱就在你的工作指令描述中“*杂项修复*。” 哦，我的天啊~

阅读其他工程师的代码有时会感觉像是考古调察，充满了难以破译的奇怪而精彩的描述。**写代码一定是有目的的，然而有时候，代码本身并没有清楚地表达它的目的。**你可以通过编写为什么修改这段代码的上下文来解决这种知识鸿沟。代码注释可以提供上下文，但仅有注释有时候无法提供足够的信息。

有两种关键的方法来表明上下文：

**提交注释信息**

- **一个提交注释信息是提供上下文的最简单，最易发现的方法之一**。当有些代码的目的不是很清楚时，可以看一下提交注释信息，它可能对代码做了很好的解释，可以深入了解代码的用途。

- **提交注释信息的第一行应该独立成行，因为像GitHub这样的工具会在提交列表页面中显示第一行。** 独立第一行让你更快地浏览代码历史记录，快速建立对源文件随时间演变的理解。例如下面这一行：  

  > `Add Frobber to the list of available widgets.  This allows consumers to easily discover the new Frobber widget and add it to their application.`

**缺陷报告单**

- **你可以使用缺陷报告单跟踪一个缺陷或特性，或者是一次重构的完整历史，**这个缺陷报告单包括原始问题的描述，团队间的设计讨论，以及用来解决这个问题的注解。这让你很容易在一个地方看到所有相关的提交，让其他人容易跟踪某个具体问题的状态。
- **大多数提交应该与某个缺陷报告相关联，**而相对独立的提交 **(例如一次性的清除，或者未计划的小变更)并不需要有一个它们自己的缺陷报告单，因为在描述或源代码中通常包含它们所有的上下文。

**信息丰富的提交注释信息和缺陷报告单是相辅相成的，** 它们从不同的角度提供是上下文。请记信，**这样的上下文甚至对你自己也有用的**，它可以很容易提醒你，你上周做了什么，甚至是去年做了什么。未来的某一天，你一定会感谢今天写下清晰上下文的你!