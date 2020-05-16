## 包含尊重的CR，才是有用的CR

虽然代码评审被认为是提高软件项目质量的一个有价值的工具。但是，如果反馈评论被认为描述不清晰，或者是过于苛刻，都可能会产生不良后果，如：慢吞吞的评审、被阻滞的代码评审，负面情绪，或其他贡献者或同事的负面看法。

请考虑以下这些技巧，以相互尊重地方式代码评论。

### 作为评审人，或代码作者：

**应当**：假设有能力。一个作者的实现或者一个评论者的推荐可能是由于另一方与你有不同的背景。先问问题以获得理解。
**应当**：提供基本原理或上下文，如最佳实践文档、样式指南或设计文档。这可以帮助其他人理解你的决定或提供指导。
**应当**：考虑如何解释评论。注意不同的方式夸张，笑话，和情绪化可能被感知。


<table class="my-bordered-table">
  <tbody>
<tr width=‘50%’>
      <td style="text-align: center;"><em>作者不应当</em></td>
      <td style="text-align: center;"><em>作者应当</em></td>
</tr>
<tr>
<td bgcolor=LightPink>
我喜欢短名字，所以我就不改。除非你命令我？:)
</td>
<td bgcolor=LIGHTGREEN>
在最佳实践指南中，建议省略明显的或通用的术语。<br/>
我不知道该如何改，才能使指南与你的这一要求保持一致。
</td>
  </tr>
</tbody></table>


**不应当**：批评或指责那个人。相反，要就代码进行讨论。甚至，在评论中由于指代具体人（例如，由于使用了“you”或“your”）也会偏离改进代码的目标。
**不应当**：用过于严厉的语言。带有否定和反问语气的代码评论不太可能有什么好作用。例如，先前的研究发现，57%的作者认为非常负面的评论是有用的，而79%的作者认为更中性的评论是有用的。

<table class="my-bordered-table">
  <tbody>
<tr width=‘50%’>
      <td style="text-align: center;"><em>评审人不应当</em></td>
      <td style="text-align: center;"><em>评审人应当</em></td>
</tr>
<tr>
<td bgcolor=LightPink>
你为什么要用这种方法？<br/>
你增加了不必要的复杂性。
</td>
<td bgcolor=LIGHTGREEN>
这种并发模型似乎增加了系统的复杂性，<br>
但似乎没有什么明显增加性能优势。
</td>
  </tr>
</tbody></table>


### 作为评审人：

**应当**：提供具体可行的反馈。如果你没有具体的建议，有时询问作者为什么做出决定是有益的。


<table class="my-bordered-table">
  <tbody>
<tr width=‘50%’>
      <td style="text-align: center;"><em>评审人不应当</em></td>
      <td style="text-align: center;"><em>评审人应当</em></td>
</tr>
<tr>
<td bgcolor=LightPink>
我不明白。
</td>
<td bgcolor=LIGHTGREEN>
如果这是为了优化目的，你能增加一些注释吗？
</td>
  </tr>
</tbody></table>

**应当**：使用前缀（如“Nit”或“optional”）清楚地标记“”nitpick和“可选”的注解。这使作者能够更好地评估评审人的期望。

### 作为作者：

**应当**：在回复反馈时，应该对代码进行澄清，或回复评审人提出的意见。如果不这样做，可能让人觉得缺乏对实现代码改进的意愿和能力。


<table class="my-bordered-table">
  <tbody>
<tr width=‘50%’>
      <td style="text-align: center;"><em>作者不应当</em></td>
      <td style="text-align: center;"><em>作者应当</em></td>
</tr>
<tr>
<td bgcolor=LightPink>
你说的这些在某些情况下是有道理的，但在这段代码里并不适用。
</td>
<td bgcolor=LIGHTGREEN>
我添加了一条代码注释，说明为什么在这段代码中要这样实现。
</td>
</tr>
</tbody></table>
