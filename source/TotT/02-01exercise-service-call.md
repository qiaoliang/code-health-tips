## 在测试用例中对服务调用进行契约检查

下面的测试将对CloudService的服务调用进行模拟替代(mocks）。 **这个测试真的提供了足够的信心，让你相信这个服务调用是可以工作的了么？**

```java
@Test public void uploadFileToCloudStorage() {
  when(mockCloudService.write(
          WriteRequest.newBuilder().setUserId(“testuser”).setFileType(“plain/text”)...))
    .thenReturn(WriteResponse.newBuilder().setUploadId(“uploadId”).build());

  CloudUploader cloudUploader = new CloudUploader(mockCloudService);

  Uri uri = cloudUploader.uploadFile(new File(“/path/to/foo.txt”));
  // The uploaded file URI contains the user ID, file type, and upload ID. (Or does it?)
  assertThat(uri).isEqualTo(new Uri(“/testuser/text/uploadId.txt”));
```

**很多地方可能出错，** 尤其是当服务契约变得复杂时。例如，plain/text可能不是一个有效的文件类型，而你验证上传文件的URI是正确的。

**如果被测代码依赖于某个服务的契约，建议检查这个服务调用，**而不是对其进行模拟替代（mocking it out）。这让你对能够正确使用这个外部服务更有信心：

<table>
  <tr>
    <td bgcolor=DarkSeaGreen>
      @Test public void uploadFileToCloudStorage() {<br>
      &nbsp;&nbsp;&nbsp;&nbsp;
      CloudUploader cloudUploader = new CloudUploader(cloudService);</br>
      &nbsp;&nbsp;&nbsp;&nbsp;
      Uri uri = cloudUploader.uploadFile(”/path/to/foo.txt”);</br>
      &nbsp;&nbsp;&nbsp;&nbsp;
      assertThat(cloudService.retrieveFile(uri)).isEqualTo(readContent(“/path/to/foo.txt));</br>
}</br>
    </td>
  </tr>
</table>

你如何检查这个服务调用呢？

1. **使用一个fake服务**。一个fack对象是某个真实服务的快速且轻量级的实现，它的行为就象它模仿的真实服务一样。一个fake服务通常是由服务的提供者负责维护；除非你能保证Fake服务的行为与真实实现一直保持一致，否则不要自己创建自己的fake服务。关于更多的Fake服务，参见 [testing.googleblog.com/2013/06/testing-on-toilet-fake-your-way-to.html](http://testing.googleblog.com/2013/06/testing-on-toilet-fake-your-way-to.html).
2. **使用一个密闭的(hermetic）服务器**。这是一个真实的服务，它是由测试用例在其运行的同一台机器上启动的。使用密闭服务器的缺点是启动它并与其交互会让测试运行比较慢。关于更多的密闭服务器，参见 [testing.googleblog.com/2012/10/hermetic-servers.html](http://testing.googleblog.com/2012/10/hermetic-servers.html).

如果众多使用的服务并没有fake服务或者密闭服务器，模拟器（mock）可有是你的唯一选择了。但是，如果**你的测试并没有使用真正的调用契约来请求真正的服务，那你要格外小心，确保那个测试所调用的真正服务是正常工作的，例如可以通过一个全面的端到端的测试用例，或者求助于手工测试(但这可能效率低，而且不容易扩展规模)。