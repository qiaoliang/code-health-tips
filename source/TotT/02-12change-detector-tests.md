## 像变更检测器一样的测试代码，是有害的！

想像一下，你刚刚完成一次重构（没有改变该应用的外部行为），正打算提交这次变更。但是，为了安全起见，你打算在提交代码前运行一次自动化测试。然而运行之后发现，一堆单元测试失败了。可能你只是给一个方法增加了一个参数，而现在必须更新自动化测试用例中，所有调用这个方法的测试代码（可能有100多处代码引用了它），只是向其中传入一个空字符串。**你一边修复着这些失败的测试，一边心里嘀咕：“这种机械式重复修改真是浪费时间啊。"**

**机械地写测试，看起来是什么样子的呢？** 下面就是 个荒谬但却显而易见的方式：

```python
// Production code:
def abs(i: Int)
  return (i < 0) ? i * -1 : i

// Test code:
for (line: String in File(prod_source).read_lines())
  switch (line.number)
    1: assert line.content equals def abs(i: Int)
    2: assert line.content equals   return (i < 0) ? i * -1 : i
```

**这个测试显然没什么用**：它就是被测代码的一份拷贝，其测试的行为就像一个校验码。 **无论这个程序正确与否，都会正常通过**，因为它就是被测代码的衍生物。现实中，没有人这么写自动化测试用例，那么，它与下面的这个例子有什么区别吗？

```python
// Production code:
def process(w: Work)
  firstPart.process(w)
  secondPart.process(w)

// Test code:
part1 = mock(FirstPart)
part2 = mock(SecondPart)
w = Work()
Processor(part1, part2).process(w)
verify_in_order
  was_called part1.process(w)
  was_called part2.process(w)
```

这种测试代码很诱惑人，因为它不需要思考，并且会很快运行。 **这就是所谓的“变化检测器（change-detector）”的测试用例**——它只是对被测代码中相同信息进行了一个转换——而且， **源代码的任何变化，它都会坏掉，而且并没有验证任何东西**，即不是原来的生产代码，也不是变化后的生产代码。

**变化检测器并不提供价值，相反，它会降低价值，**因为它并不能发现任何bug，还增加了维护成本，也就阻碍了开发速度。这类测试应该被重写或者删除。