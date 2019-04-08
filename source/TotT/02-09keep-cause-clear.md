## 确保原因与影响清晰

下面的这个测试写的正确吗？

```java
208: @Test public void testIncrement_existingKey() {
209:   assertEquals(9, tally.get("key1"));
210: }
```

事实上，如果不知道tally这个对象是如何准备的，你就根本不可能确认它是否正确：

```java
1:   private final Tally tally = new Tally();
2:   @Before public void setUp() {
3:      tally.increment("key1", 8);
4:      tally.increment("key2", 100);
5:      tally.increment("key1", 0);
6:      tally.increment("key1", 1);
7:   }
// 200 lines away
208: @Test public void testIncrement_existingKey() {
209:   assertEquals(9, tally.get("key1"));
210: }
```

上面这段代码存在的问题在于：对`key1`这个值进行断言与对其值的修改之间远隔200多行。或者说，**原因就隐藏在远离影响的地方**。

相反，编写测试时，*影响*应该能够立即追踪到*原因*。这就是我们用自然语言说话的方式：“如果你超过了速度限制（*原因*），你会得到一张交通罚单（*效果*）。”当我们将这两段代码放在一起，我们就很容易看到发生了什么：

```java
1:   private final Tally tally = new Tally();
2:   @Test public void testIncrement_newKey() {
3:     tally.increment("key", 100);
5:     assertEquals(100, tally.get("key"));
6:   }
7:   @Test public void testIncrement_existingKey() {
8:     tally.increment("key", 8);
9:     tally.increment("key", 1);
10:    assertEquals(9, tally.get("key"));
11:  }
12:  @Test public void testIncrement_incrementByZeroDoesNothing() {
13:    tally.increment("key", 8);
14:    tally.increment("key", 0);
15:    assertEquals(8, tally.get("key"));
16:  }
```

这种方式可能会需要更多的代码。**每个测试设置自己的输入并验证自己的预期输出**。回报是更可读的代码和更低的维护成本。