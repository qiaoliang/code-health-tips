## Don't Put Logic in Tests

(https://testing.googleblog.com/2014/07/testing-on-toilet-dont-put-logic-in.html)

**Programming languages give us a lot of expressive power.** Concepts like operators and conditionals are important tools that allow us to write programs that handle a wide range of inputs. But **this flexibility comes at the cost of increased complexity**, which makes our programs harder to understand.

Unlike production code, **simplicity is more important than flexibility in tests**. Most unit tests verify that a single, known input produces a single, known output. **Tests can avoid complexity by stating their inputs and outputs directly rather than computing them**. Otherwise it's easy for tests to develop their own bugs.

Let's take a look at a simple example. **Does this test look correct to you?**



```java
@Test public void shouldNavigateToPhotosPage() {
  String baseUrl = "http://plus.google.com/";
  Navigator nav = new Navigator(baseUrl);
  nav.goToPhotosPage();
  assertEquals(baseUrl + "/u/0/photos", nav.getCurrentUrl());
}
```



The author is trying to avoid duplication by storing a shared prefix in a variable. Performing a single string concatenation doesn't seem too bad, but **what happens if we simplify the test by inlining the variable?**

```java
@Test public void shouldNavigateToPhotosPage() {
  Navigator nav = new Navigator("http://plus.google.com/");
  nav.goToPhotosPage();
  assertEquals("http://plus.google.com//u/0/photos", nav.getCurrentUrl()); // Oops!
}
```

**After eliminating the unnecessary computation from the test, the bug is obvious**—we're expecting two slashes in the URL! This test will either fail or (even worse) incorrectly pass if the production code has the same bug. We never would have written this if we stated our inputs and outputs directly instead of trying to compute them. And this is a very simple example—**when a test adds more operators or includes loops and conditionals, it becomes increasingly difficult to be confident that it is correct.**

Another way of saying this is that, **whereas production code describes a general strategy for computing outputs given inputs, tests are concrete examples of input/output pairs** (where output might include side effects like verifying interactions with other classes). It's usually easy to tell whether an input/output pair is correct or not, even if the logic required to compute it is very complex. For instance, it's hard to picture the exact DOM that would be created by a Javascript function for a given server response. So the ideal test for such a function would just compare against a string containing the expected output HTML.

**When tests do need their own logic, such logic should often be moved out of the test bodies and into utilities and helper functions**. Since such helpers can get quite complex, it's usually a good idea for any nontrivial test utility to have its own tests.

