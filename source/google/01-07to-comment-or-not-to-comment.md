# 使用注释，还是不用呢?

当阅读代码时， **没有什么比更好的注释有帮助的了。** 然而， **注释并不总是好事情。** 有些情况下，需要写注释就说明，这段代码应该被重构了。

**如果你无法让你的代码意图不言自明，再使用注释好了.** 如果你认为需要用注释来解释一段代码在做什么，那么，还是先试一试下面的方法吧：

- 引入一个解释性的变量。

  <table>
    <tr>
      <td width="50%" bgcolor=LightPink>
        // Subtract discount from price.<br/>
        finalPrice = (numItems * itemPrice) <br/>
        &nbsp;&nbsp;&nbsp;&nbsp;- min(5, numItems) * itemPrice * 0.1;
      </td>
      <td bgcolor=DarkSeaGreen>
        price = numItems * itemPrice;<br/>
        discount = min(5, numItems) * itemPrice * 0.1; <br/>
        finalPrice = price - discount;<br/>
      </td>
    </tr>
  </table>

- 抽取成一个方法。

  <table>
    <tr>
      <td bgcolor=LightPink width ="50%">
        // Filter offensive words.<br/>
        for (String word : words) {...}
      </td>
      <td bgcolor=DarkSeaGreen>
        filterOffensiveWords(words);
      </td>
    </tr>
  </table>


- 使用一个更有描述性的命名。

  <table>
    <tr>
      <td bgcolor=LightPink width ="50%">int width = ...; // Width in pixels.</td>
      <td bgcolor=DarkSeaGreen>int widthInPixels = ...;</td>
    </tr>
  </table>

- 万一你的代码有假设条件，就增加一个检查点，而不是注释。

  <table>
    <tr>
      <td bgcolor=LightPink width ="50%">
        // Safe since height is always > 0.<br/>
        return width
      </td>
      <td bgcolor=DarkSeaGreen >
        checkArgument(height > 0)   ;<br/>
        return width / height;
      </td>
    </tr>
  </table>

**有一些情况下，注释是有益的:**

- 展示你的意图：解释为什么写这段代码，而不是它在做什么。

  `// Compute once because it’s expensive. `

- 保护未来那个善意的想修改你代码的人，以免它错误地“修复”了你的代码。

  ` // Create a new Foo instance because Foo is not thread-safe.`

- 声明式:代码审查期间出现的问题或代码的读者可能有的问题。
  ` // Note that order matters because…`

- 如果看起来你写了一段违反软件工程实践的糟糕代码，那就解释一下你的理由。

   `@SuppressWarnings("unchecked") // The cast is safe because…`

另一方面，**避免注释只是重复了代码表达的内容** 。这种做法令人生厌，如下所示:

  <table>
    <tr>
      <td bgcolor=LightPink width ="50%">
        // Get all users.<br/>
        userService.getAllUsers();
      </td>
      <td bgcolor=LightPink >
        // Check if the name is empty.<br/>
        if (name.isEmpty()) { ... }
      </td>
    </tr>
  </table>

