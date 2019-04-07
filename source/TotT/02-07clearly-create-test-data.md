## 清晰创建测试数据

**Helper方法让创建测试数据变得更容易。但随着时间的推移，它们会变得更难理解。**因为您需要测试数据的更多变形，来满足不断变化的新测试要求：

```java
// This helper method starts with just a single parameter:
Company company = newCompany(PUBLIC);

// But soon it acquires more and more parameters.
// Conditionals creep into the newCompany() method body to handle the nulls,
// and the method calls become hard to read due to the long parameter lists:
Company small = newCompany(2, 2, null, PUBLIC);
Company privatelyOwned = newCompany(null, null, null, PRIVATE);
Company bankrupt = newCompany(null, null, PAST_DATE, PUBLIC);

// Or a new method is added each time a test needs a different combination of fields:
Company small = newCompanyWithEmployeesAndBoardMembers(2, 2, PUBLIC);
Company privatelyOwned = newCompanyWithType(PRIVATE);
Company bankrupt = newCompanyWithBankruptcyDate(PAST_DATE, PUBLIC);
```

相反，**应该使用Test Data Builder模式：创建一个帮助方法，它返回一个被部分构造的对象**（例如，Java语言中的一个Builder，或者一个可变对象），其状态可以在测试中被重写。helper方法将逻辑上必需的字段初始化为合理的默认值，因此**每个测试仅指定与测试用例相关的字段**：

```java
Company small = newCompany().setEmployees(2).setBoardMembers(2).build();
Company privatelyOwned = newCompany().setType(PRIVATE).build();
Company bankrupt = newCompany().setBankruptcyDate(PAST_DATE).build();
Company arbitraryCompany = newCompany().build();

// Zero parameters makes this method reusable for different variations of Company.
// It also doesn’t need conditionals to ignore parameters that aren’t set (e.g. null
// values) since a test can simply not set a field if it doesn’t care about it.
private static Company.Builder newCompany() {
  return Company.newBuilder().setType(PUBLIC).setEmployees(100); // Set required fields
}
```

还请注意，**测试不应依赖于由helper方法中指定的默认值**，因为这会强制读者读取helper方法的实现细节，以便能理解测试。

```java
// This test needs a public company, so explicitly set it.
// It also needs a company with no board members, so explicitly clear it.
Company publicNoBoardMembers = newCompany().setType(PUBLIC).clearBoardMembers().build();
```

更多相关内容请参见*http://www.natpryce.com/articles/000714.html*。