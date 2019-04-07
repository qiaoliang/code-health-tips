## 避免Flakey Tests

不稳定的测试会让你的生活更加困难。您会收到失败通知，便也没什么帮助。你甚至可能因对失败已麻木而错过了一个真实的失败条件。你对代码的修改可能会因导致测试失败而受到不公平的指责。

不幸的是，**很多因素都会使测试变得不稳定**。今天，我们举一个简单的例子：从单元测试访问文件。下面是这个功能及其测试代码：

```py
def CreateGapLease(self):
  data_file = open('/leases/gap', 'w+')
  data_file.write(contract_data)
  data_file.close()
def testCreateGapLease(self):
  contract_writer.CreateGapLease()
  self.assertEqual(ReadFileContents('/leases/gap'),
         contract_data)
```

如果 `/leases/gap`已经存在，并且已经包含了一些数据呢？ `testCreateGapLease`这个测试会失败。这是一个常见问题，即前置条件被假定为正确的。同样，假设一个数据库包含正确的信息，也是容易发生的。**如果使用该文件的另一个测试也同时运行，会怎么样呢？**

如果您真的想使用实时资源来测试你的代码，请始终检查您的假设。在这种情况下，在测试开始时清除文件可以降低其脆弱性：

```py
def testCreateGapLease(self):
  if os.path.exists(lease_file):
    RemoveFile(lease_file)
  ...
```

不幸的是，这并不能完全消除我们测试中的不稳定因素。如果`/leases/gap`是一个NFS路径，或者可以被另一个测试写入内容，那么，我们的测试仍然可能意外失败。测试最好使用唯一的资源。这可以通过对“`creategaplease`”的重构来完成：

```python
def CreateGapLease(self, lease_path=None):
  if lease_path is None:
    lease_path = '/leases/gap'
  ...
```

对 `CreateGapLease`的真实调用可以像平时一样调用它，而单元测试可以传入一个不同的文件路径：

```python
def testCreateGapLease(self):
  lease_file = os.path.join(FLAGS.test_tmpdir, 'gap')
  contract_writer.CreateGapLease(lease_path=lease_file)
  self.assertEqual(ReadFileContents(lease_file),contract_data)
```

当然，为了让测试尽可能运行快速，最好使用模拟文件系统，从而完全屏蔽掉对磁盘的访问。