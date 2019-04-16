## 测试行为（Behaviors），不要测试方法(Methods）

在编写一个method()之后，只需编写一个测试，就可以轻松地验证方法的所有功能。但是**认为测试与public method方法应该是一对一的关系，是不对的**。我们真正想要测试的是软件的行为，而一个方法可能展现出很多种行为，而且，一种行为有时会跨越多个方法。

让我们来看看现面这个不好的测试，它验证整个方法：

```java
@Test public void testProcessTransaction() {
  User user = newUserWithBalance(LOW_BALANCE_THRESHOLD.plus(dollars(2));
  transactionProcessor.processTransaction(
      user,
      new Transaction("Pile of Beanie Babies", dollars(3)));
  assertContains("You bought a Pile of Beanie Babies", ui.getText());
  assertEquals(1, user.getEmails().size());
  assertEquals("Your balance is low", user.getEmails().get(0).getSubject());
}
```



显示所购买商品的名称，并发送一封关于余额不足的电子邮件是两种不同的行为，但是，这个测试同时检查这两种行为，因为它们恰好是在同一个方法中触发的。**像这样的测试通常会变得非常庞大，并且随着时间的推移很难维护，**这是因为很容易让人产生在其中不断添加额外行为的冲动——最终很难确定输入的哪些部分负责哪些断言。测试的名字是方法名的直接镜像，这也是一个错误信号。

**最好使用单独的测试来验证单独的行为**：

```java
@Test public void testProcessTransaction_displaysNotification() {
  transactionProcessor.processTransaction(
      new User(), new Transaction("Pile of Beanie Babies"));
  assertContains("You bought a Pile of Beanie Babies", ui.getText());
}
@Test public void testProcessTransaction_sendsEmailWhenBalanceIsLow() {
  User user = newUserWithBalance(LOW_BALANCE_THRESHOLD.plus(dollars(2));
  transactionProcessor.processTransaction(
      user,
      new Transaction(dollars(3)));
  assertEquals(1, user.getEmails().size());
  assertEquals("Your balance is low", user.getEmails().get(0).getSubject());
}
```

现在，当有人为该方法添加新的行为时，他们就要为这个行为编写新的测试了。无论添加多少行为，每个测试都要保持聚焦和易于理解。**这将使您的测试更有弹性，因为添加新的行为不太可能破坏现有的测试，而且更清晰，因为每个测试都包含只执行一种行为的代码**。