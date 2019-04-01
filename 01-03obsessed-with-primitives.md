# 沉迷于原始类型？



编程语言提供基本类型，例如整数，字符串和映射，这些类型在许多上下文中都很有用。 例如，字符串可用于保存从人名到网页URL的所有内容。 但是，**过于依赖基本类型而非自定义抽象的代码可能难以理解和维护**。

**原始类型的痴迷是指过度使用基本（“原始”）类型来表示更高层次的概念**。 例如，下面的代码使用基本类型来表示形状：

```java
vector<pair<int, int>> polygon = ...
pair<pair<int, int>, pair<int, int>> bounding_box = GetBoundingBox(polygon);
int area = (bounding_box.second.first  - bounding_box.first.first) *
           (bounding_box.second.second - bounding_box.first.second);
```

`pair` 在这里并不是一个正确的抽象层次，因为它的一般命名 `first` 和 `second`两个参数被用于表示 *X* 和 *Y*，其中一处被用做标识 *lower-left* (或者, *upper-left*?) 而另外一处用作 *upper-right* (或, *lower-right*?) 。更糟糕的是，**基本类型并没有对领域专属的代码进行封装**，例如**边框**（bounding box）和**面积**（area）。

**用更高级别的抽象可以产生更清晰且封装性更好的代码：**

```java
Polygon polygon = ...
int area = polygon.GetBoundingBox().GetArea();
```

下面是一些原始类型痴迷的例子：

- maps, lists, vectors等类型可以很方便地成为一个容器集，将值合并到自定义的更高级抽象中。

  | map<UserId, string> id_to_name; <br/> map<UserId, int> id_to_age; | map<UserId, Person> id_to_person; |
  | ------------------------------------------------------------ | ----------------------------------- |
  |                                                              |                                     |

- 包含魔术键值的vector或 map ，例如 用0，1和2分别代表*name*, *address*, and *phone #*，应该将其放入到更高一级的抽象中。

  | `person_data[kName] = "Foo";` | `person.SetName("Foo");` |
  | ----------------------------- | ------------------------ |
  |                               |                          |

- 用一个字符串表示复杂或结构化的文本（例如一个日期）。更好的做法是使用更高层的抽象(如`Date`类型) ，它还提供自我说明的访问函数(例如 `GetMonth` ) ，并且还保证正确性。

  | `string date = "01-02-03";` | `Date date(Month::Feb, Day(1), Year(2003));` |
  | --------------------------- | -------------------------------------------- |
  |                             |                                              |

- 用一个整型或浮点数表过一个时间值，例如多少秒。更好的做法是使用结构化的时间戳(timestamp)或 时间段类型（duration）。

  | `int timeout_secs = 5;` | `Duration timeout = Seconds(5);` |
  | ----------------------- | -------------------------------- |
  |                         |                                  |

- **对于某个任务来说，任何类型都可能过于原始，从最原始的int，到复杂的红黑树。**如果你看到代码使用了很多的基础类型，那么，使用更高级别的抽象更能够表达清楚，封装性也会更好。你应该把它重构了，或者礼貌地提醒作者要 ***保持优雅***！

