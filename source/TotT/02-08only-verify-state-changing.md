## 只验证发生状态变化的方法调用

下面的哪一行代码可以被安全地移除？

```java
@Test public void addPermissionToDatabase() {
  new UserAuthorizer(mockUserService, mockPermissionDb).grantPermission(USER, READ_ACCESS);

  // The test will fail if any of these methods is not called.
  verify(mockUserService).isUserActive(USER);
  verify(mockPermissionDb).getPermissions(USER);
  verify(mockPermissionDb).isValidPermission(READ_ACCESS);
  verify(mockPermissionDb).addPermission(USER, READ_ACCESS);
}
```

答案是：**可以删除用于验证非状态更改方法的调用。**

**对另一个对象的方法调用分为两类**：

- **引起状态变化的**（state-changing）：有副作用，并且改变了被测代码之外的信息，例如sendemail（）、saverecord（）、logaccess（）。
- **未引起状态变化的**（non-state-changing）：只是返回被测代码之外的信息，但并不修改任何内容的方法，例如getuser（）、findresults（）、readfile（）。

通常应**避免验证那些并未修改状态的方法是否被调用了**：

- *它通常是多余的*：**一个不改变外部世界状态的方法调用，对其本身来说是没有什么意义的**。测试中的代码将使用方法调用的返回值来执行您可以断言的其他工作。
- *它使测试变得脆弱*：只要方法调用发生变化，就要更新测试代码。例如，如果一个测试期望的是调用mockuserservice.isUserActive（user）`，那么，当将被测代码被修改为调用user.isActive（）时，这个测试就会失败。
- *它使测试的可读性降低*：测试中附加的断言让“确认到底是哪个方法调用真正影响了世界状态”变得更加困难。
- *它给出了一种错误的安全感*：仅仅因为被测试的代码调用了一个方法，并不意味着被测试的代码用该方法的返回值做了正确的事情。

不要验证是否调用了它们，而是使用非状态更改方法来模拟测试中的不同条件，例如，`when(mockUserService.isUserActive(USER)).thenReturn(false)`。然后为被测代码的返回值编写断言，或者验证那个引起状态变化的方法被调用了。

假如没有可以断言的其他输出，验证非状态更改方法的调用可能很有用。例如，如果您的代码应该缓存一个RPC结果，您可以验证使用该RPC的方法只被调用一次。

删除没必要的代码之后，测试应该如下所示：

```java
@Test public void addPermissionToDatabase() {
  new UserAuthorizer(mockUserService, mockPermissionDb).grantPermission(USER, READ_ACCESS);

  // Verify only the state-changing method.
  verify(mockPermissionDb).addPermission(USER, READ_ACCESS);
}
```

这样是不是简单多啦！但是要记住，与其使用模拟来验证是否调用了方法，不如使用一个真实的或者[**冒充对象**](02-03fake-you-way-to.md)来实际执行该方法，并检查其是否正常工作。例如，上面的测试可以使用一个假数据库来检查权限是否存在于数据库中，而不只是验证是否调用了`addPermission()`。 

*你可以在《* [*Growing Object-Oriented Software, Guided by Tests*](http://www.growing-object-oriented-software.com/)*》一书中找到更多的相关信息。要注意的是，这本书中使用的术语“command” 和 “query”，而不是“state-changing”或 “non-state-changing”.*