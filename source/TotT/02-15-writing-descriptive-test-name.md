## 使用描述性的测试名称

(https://testing.googleblog.com/2014/10/testing-on-toilet-writing-descriptive.html)

**猜一猜你会花多长时间才能了解下面这段测试代码用来验证什么样的行为?**

```java
@Test public void isUserLockedOut_invalidLogin() {
  authenticator.authenticate(username, invalidPassword);
  assertFalse(authenticator.isUserLockedOut(username));

  authenticator.authenticate(username, invalidPassword);
  assertFalse(authenticator.isUserLockedOut(username));

  authenticator.authenticate(username, invalidPassword);
  assertTrue(authenticator.isUserLockedOut(username));
}
```

**你可能要从头到尾读完每一行代码才能知道** (甚至不至读一遍) 并理解每一行在做什么. 但是， **假如我们把这个测试用例名改成下面的样子，你会花多长时间呢?**

`isUserLockedOut_lockOutUserAfterThreeInvalidLoginAttempts`

现在，你只需要读一下测试用例的名字，就应该知道被测试的行为是什么了，甚至不需要读测试代码。在第一个代码示例中的测试用例名中一个提示，那就是“invalidLogin”，但它并没有真正地告诉你，我们期望的结果到底是什么，所以，你不得不读完所有的代码才能知道。

在测试用例名字中包含将测试场景和期望的结果还有几个其它的好处：

- 如果你想了解某个类所有可能的行为，你所需要做的只是在它对应的那些测试用例类的测试方法名，而不是花数十分钟或者几个小时反复翻看测试代码，甚至生产代码本身，才能知道它的行为。在 Code Review 的时候，这种做法也会很有用，因为你在 Review 这段代码时，就会很快知道这些测试是否覆盖了所有的用例。

- 通过给测试用例一个更加明确的名称，会迫使你将测试不同的行为拆分到单独的测试用例中。否则，您可能会尝试将不同行为的断言都放在一个测试用例中。而随着时间的推移，这可能导致测试用例不断增长，变得难以理解和维护。

- 在某些情况下，我们很可能无法通过测试代码本身非常清晰地就表达打算测试哪一个确切行为。此时，如果测试名称没有明确说明这一点，你很可能要去猜测测试实际上在测什么。

- 你能很容易地判断出，是否还有哪些功能没有被测试到。比如，当你没发现那个描述你正在寻找的代码行为的测试名时，你就知道代码的作都没有写全测试。

- 当测试失败时，你能立即看到哪个功能被破坏了。

有几种常见的测试名称的命名结构模式，其中一种就是将测试名写成一个句子，并在名称中包含关键字 “should” ，例如, `shouldLockOutUserAfterThreeInvalidLoginAttempts`。无论你用什么模式，有个建议是通用的： 确保测试用例名字中包括被测试的场景和期望的输出。

当然，有时只是指定被测方法的名字就足够了，特别是当这个方法非常简单，并且只有一个行为，而这个行为就是这个方法名本身。