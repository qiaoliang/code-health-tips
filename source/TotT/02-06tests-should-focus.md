## 测试应该聚焦

下面的代码在测试什么场景？

```C++
TEST_F(BankAccountTest, WithdrawFromAccount) {
  Transaction transaction = account_.Deposit(Usd(5));
  clock_.AdvanceTime(MIN_TIME_TO_SETTLE);
  account_.Settle(transaction);


  EXPECT_THAT(account_.Withdraw(Usd(5)), IsOk());
  EXPECT_THAT(account_.Withdraw(Usd(1)), IsRejected());
  account_.SetOverdraftLimit(Usd(1));
  EXPECT_THAT(account_.Withdraw(Usd(1)), IsOk());
}
```

翻译成中文：“***(1)*** *我有5美元可以取出来;* ***(2)*** *当透支1美元时，就会被拒绝* ***(3)*** 但是，*如果我但如果我允许透支1美元的限额，我就可以提取1美元。”* 这个测试是否有点难以理解呢？因为**它测试了三个场景，而不是一个场景。**

更好的方法是**每个场景都应该在独自的测试用例中进行验证**：

```C++
TEST_F(BankAccountTest, CanWithdrawWithinBalance) {
  DepositAndSettle(Usd(5));  // Common setup code is extracted into a helper method.
  EXPECT_THAT(account_.Withdraw(Usd(5)), IsOk());
}
TEST_F(BankAccountTest, CannotOverdraw) {
  DepositAndSettle(Usd(5));
  EXPECT_THAT(account_.Withdraw(Usd(6)), IsRejected());
}
TEST_F(BankAccountTest, CanOverdrawUpToOverdraftLimit) {
  DepositAndSettle(Usd(5));
  account_.SetOverdraftLimit(Usd(1));
  EXPECT_THAT(account_.Withdraw(Usd(6)), IsOk());
}
```

以这种方式写测试有很多好处：

- 逻辑更容易理解，因为在每个测试方法中需要理解的代码更少。
- 每个测试中的准备代码都比较简单，因为它只需要服务于一个场景。
- 一个场景的副作用不会意外地使后一个场景的假设失效或被覆盖。
- 如果一个测试中的一个场景失败，其他场景仍可以运行，因为它们不受失败的影响。
- 测试名称清楚地描述了每个场景，这使得理解有哪些场景更加容易。

有一个迹象能够表明您可能正在测试多个场景：在对一个被测系统调用的输出进行断言之后，又对被测系统进行了另一个调用。

**虽然单元测试的场景通常由对被测试系统的单个调用组成，但是对于集成和端到端测试来说，它的范围可能更大**。例如，Web UI在做发送电子邮件的测试时可能要打开收件箱，单击撰写按钮，编写一些文本，然后按发送按钮。