## 使用描述性的测试名称

(https://testing.googleblog.com/2014/10/testing-on-toilet-writing-descriptive.html)

**How long does it take you to figure out what behavior is being tested in the following code?**

```java
@Test public void isUserLockedOut_invalidLogin() {
  authenticator.authenticate(username, invalidPassword);
  assertFalse(authenticator.isUserLockedOut(username));

  authenticator.authenticate(username, invalidPassword);
  assertFalse(authenticator.isUserLockedOut(username));

  authenticator.authenticate(username, invalidPassword);
  assertTrue(authenticator.isUserLockedOut(username));
}
```

**You probably had to read through every line of code** (maybe more than once) and understand what each line is doing. But **how long would it take you to figure out what behavior is being tested if the test had this name?**

`isUserLockedOut_lockOutUserAfterThreeInvalidLoginAttempts`

You should now be able to understand what behavior is being tested by reading just the test name, and you don’t even need to read through the test body. The test name in the above code sample hints at the scenario being tested (“invalidLogin”), but it doesn’t actually say what the expected outcome is supposed to be, so you had to read through the code to figure it out.

Putting both the scenario and the expected outcome in the test name has several other benefits:

- If you want to know all the possible behaviors a class has, all you need to do is read through the test names in its test class, compared to spending minutes or hours digging through the test code or even the class itself trying to figure out its behavior. This can also be useful during code reviews since you can quickly tell if the tests cover all expected cases.

- By giving tests more explicit names, it forces you to split up testing different behaviors into separate tests. Otherwise you may be tempted to dump assertions for different behaviors into one test, which over time can lead to tests that keep growing and become difficult to understand and maintain.

- The exact behavior being tested might not always be clear from the test code. If the test name isn’t explicit about this, sometimes you might have to guess what the test is actually testing.

- You can easily tell if some functionality isn’t being tested. If you don’t see a test name that describes the behavior you’re looking for, then you know the test doesn’t exist.

- When a test fails, you can immediately see what functionality is broken without looking at the test’s source code.

There are several common patterns for structuring the name of a test (one example is to name tests like an English sentence with “should” in the name, e.g., `shouldLockOutUserAfterThreeInvalidLoginAttempts`). Whichever pattern you use, the same advice still applies: Make sure test names contain both the scenario being tested and the expected outcome.

Sometimes just specifying the name of the method under test may be enough, especially if the method is simple and has only a single behavior that is obvious from its name.