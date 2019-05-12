## 测试公有API（Public APIs）而不是测试实现的细节

(https://testing.googleblog.com/2015/01/testing-on-toilet-prefer-testing-public.html)

**下面的这个类需要测试么?**

```java
class UserInfoValidator {
  public void validate(UserInfo info) {
    if (info.getDateOfBirth().isInFuture()) { throw new ValidationException()); }
  }
}
```

它的`Validate()`方法是包含一些业务逻辑的，所以，也许应该对它进行测试。但是， **如果它的调用者是以下面的方式来使用它的呢？**

```java
public class UserInfoService {
  private UserInfoValidator validator;
  public void save(UserInfo info) {
    validator.validate(info); // Throw an exception if the value is invalid.
    writeToDatabase(info);   
  }
}
```

答案可能是：**它也许不需要测试** ，因为所有的路径都可以通过 `UserInfoService`被测试到。关键点在于：**这个类是一种实现细节，而不是Public API**.

**公有的API可能被任何调用者所使用**，而调用者可能会将从任意组合的参数引入到该方法中。**你希望所有可能性都被测试覆盖，** 从而在调用者使用这个API时，不会发生任何问题。公共API包括两类：一是在代码仓库中其它地方的代码调用的类 (例如，一个服务类，会被客户端代码所调用)，二是公共的工具类，可能被整个代码仓库所用。

**一个被称为""实现细节"的类，其存在的意义就是为了支持某个公共API**，并且只有有限个数的调用者(通常只有一个)。 有时候，**这些类可能通过测试那些调用它们的API而被间接测试到。**

**在有些情况下，测试这种细节实现类也是有用的，**例如，如果这个类非常复杂，或者对公共API的一些测试用例非常难写。当你做这些测试时，**通常并不需要象测试公共API那样全面且深入，**因为有一些看上去可能出现的输入参数根本不可能被传到这个方法中来( 例如，在上面的代码样例中，如果`UserInfoService`已经确保 `UserInfo` 不可能为 `null`，那么就没有必要测试当把`null`作为参数传入 `UserInfoValidator.validate`时会发生什么，因为这根本不可能发生)。

细节实现类有时可能被认为是私有方法（private methods）被放到了另外的一个类中了，因为你想遵守"不应该测试私有方法"的原则。 **你也应该严格限制这种细节实现类的可见性，**例如，在Java中可能应该是在包的范围可见即可（package-private）。

**测试细节实现类常常导致一些耦合问题**:

\- **由于你经常需要更新测试，所以代码很难维护**，例如，当修改一个细节实现类的方法签名时，或者在进行一个重构操作时。如果只通过公共API来进行测试，那么这类变更就根本不会影响测试代码。

\- **如果只通过细节实现类测试某个行为，你可能会对你的代码得到假信心（false confidence）**，因为当使用公共API时，同样的代码路径也许并不能按预期方式工作。**你也不得不在重构时更加小心**，因为，如果并不是所有的路径都能通过公共API被测试到，那么，就很难确保公共API的所有行为会得到预期结果。