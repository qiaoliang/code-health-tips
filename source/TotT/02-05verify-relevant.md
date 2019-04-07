## 只验证相关的方法参数

什么会让测试不稳定？

```java
@Test public void displayGreeting_showSpecialGreetingOnNewYearsDay() {
  fakeClock.setTime(NEW_YEARS_DAY);
  fakeUser.setName("Fake User”);
  userGreeter.displayGreeting();
  // The test will fail if userGreeter.displayGreeting() didn’t call  
  // mockUserPrompter.updatePrompt() with these exact arguments.
  verify(mockUserPrompter).updatePrompt(
      "Hi Fake User! Happy New Year!", TitleBar.of("2018-01-01"), PromptStyle.NORMAL);
}
```

上面这个测试代码为`MockUserPrompter`的所有参数指定了精确值。当被测试的代码发生更改时，这些参数就可能需要更新，即使这些参数与被测试的行为无关。例如，如果向标题栏添加了额外的文本，则需要对所用使用了这个参数的测试代码进行修改。

此外，验证过多的参数会让理解正在测试的行为变得困难，因为不清楚哪些参数对测试很重要，哪些参数无关紧要。

相反，只应用验证影响被测试特定行为正确性的那些参数。可以使用参数匹配器（例如mockito中的`any()`和`contains()`）忽略不需要验证的参数：

```java
@Test public void displayGreeting_showSpecialGreetingOnNewYearsDay() {
  fakeClock.setTime(NEW_YEARS_DAY);
  userGreeter.displayGreeting();
  verify(mockUserPrompter).updatePrompt(contains("Happy New Year!"), any(), any()));
}
```

可以在其他测试用例中来验证在该测试中被忽略的参数。遵循这个模式，我们可以对每个测试只验证一个行为，这使得测试更可读，更能适应变化。例如，下面这个可能是我们需要编写的另一个单独的测试：

```java
@Test public void displayGreeting_renderUserName() {
  fakeUser.setName(“Fake User”);
  userGreeter.displayGreeting();
  // Focus on the argument relevant to showing the user's name.
  verify(mockUserPrompter).updatePrompt(contains("Hi Fake User!"), any(), any());
}
```

