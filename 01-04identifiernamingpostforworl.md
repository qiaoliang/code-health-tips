# 小心使用过长的命名

创建长标识符很容易被带飞。虽然较长的命名常常让代码更加具有可读性，但是，**如果名字过长，也能降低可读性。**在GitHub和其它地方，你很容易找到长达60个字符的变量名。**在58个字符以内，我们可以让你看到下面的俳句**:

​                     **Name variables**

​                     **Using these simple guidelines**

​                     **Beautiful source code**

**命名应该符合两个条件:** ***清晰*** （clear，知道它是什么) 并且 ***准确*** (precise，知道它不是什么)。下面有一些有用的指导方针：

**• 省略那些变量类型非常明确的描述语。**

```java
// 不好的, 类型声明已经告诉我们，它是String了： 
String nameString; List<datetime> holidayDateList; 
// 好的: 
String name; List<datetime> holidays; 
```

**• 省略无关的细节。**

```java
// 过于具体的名字，很难读: 
Monster finalBattleMostDangerousBossMonster; 
Payments nonTypicalMonthlyPayments; 
// 好的，假如没有会引起歧义的其它monsters或payments: 
Monster boss; 
Payments payments;
```

**• 省略那些从上下文中可以得到的描述信息。**

```java
// 不好的，只是重复了上下文: 
class AnnualHolidaySale {int annualSaleRebate; boolean promoteHolidaySale() {...}} 
// 好的: 
class AnnualHolidaySale {int rebate; boolean promote() {...}} 
```

**• 省略那些可以到处使用的标识符。**

常常有嫌疑的命名是：*data, state, amount, number, value, manager, engine, object, entity, instance, helper, util, broker, metadata, process, handle, context*. 剔除它们吧。

当然也有一些例外情况，此时要加入你自己的判断力了。过长的命名仍旧比太短的命名好。然而，**遵循这些指导原则, 您的代码将保持明确并且更易于阅读**。读者，包括"未来的你” 都会对你现在代码的清晰整洁大加赞赏!