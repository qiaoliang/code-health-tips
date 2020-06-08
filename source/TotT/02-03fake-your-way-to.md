## 使用冒充者(faked)，让测试做得更好

在写了多年博客之后，你决定尝试一下所用的博客平台的API。你开始练习使用它，但你意识到一个问题：你的测试在没有与远程博客服务器进行交互的情况下，如何判断你的代码代码是可以工作的呢？

```java
public void deletePostsWithTag(Tag tag) {
  for (Post post : blogService.getAllPosts()) {
    if (post.getTags().contains(tag)) { blogService.deletePost(post.getId()); }
  }
}
```

为了营救而冒充！ **一个冒充对象（A [fake](http://xunitpatterns.com/Fake%20Object.html)）是API的一个轻量级实现，其行为类似于真正的实现，但不能用于生产环境。**对于博客服务而言，你所关心的只是检索和删除文章的能力。虽然真正的博客服务需要一个数据库并支持多个前端服务器，但是，在测试你的代码时，你并不需要它们中的任何一个，你所需要的只是博客服务API的某种实现即可。你可以通过一个简单的内存实现来实现这一点：

```java
public class FakeBlogService implements BlogService {  
  private final Set<Post> posts = new HashSet<Post>(); // Store posts in memory
  public void addPost(Post post) { posts.add(post); }
  public void deletePost(int id) {
    for (Post post : posts) {
      if (post.getId() == id) { posts.remove(post); return; }
    }
    throw new PostNotFoundException("No post with ID " + id);
  }
  public Set<Post> getAllPosts() { return posts; }
}
```

现在你的测试可以用假代码替换真正的博客服务，测试中的代码甚至根本不知道它的区别在哪里。

**当你不能在测试中使用真正的实现时，冒充对象是很有用的，** 例如，如果真正实现代码的访问速度太慢（例如，启动需要几分钟），或者它的返回不具备确定性(例如，它需要与外部的机器进行通信，但测试代码运行时无法访问外部资源)。

通常，您不需要自己编写冒充对象，因为**冒充对象应该由拥有真正实现**的人员或团队创建和维护。如果您使用的API不提供冒充实现，那么通常很容易自己创建一个：为在测试中不能使用的那部分代码编写一个包装器，并为该包装器创建一个冒充对象。**请记住在尽可能低的级别上创建伪数据库**（例如，如果您不能在测试中使用数据库，则应伪造数据库，而不是伪造所有与数据库对话的类），这样您的维护工作量较少，并且，测试代码也能对系统的重要部分执行更多真实代码。

**冒充对象应该有自己的代码**，以确保它的行为象真实的实现一样(例如，如果在给定某个输入时实际实现抛出异常，则在给定相同的输入时，假实现也应抛出异常）。实现这一点的一种方法是针对API的公共接口编写测试，并针对真实和虚假实现运行这些测试。）例如，如果在给定某个输入时，真实的实现代码会抛出异常，那么，给定相同的输入时，也应抛出异常）。实现这一点的一种方法是针对API的那些公共接口（ public interface ）编写测试，并针对真实API和冒充对象运行这些测试。

如果你的所有测试都使用了一早冒充代码，所以您仍然不完全相信您的代码可以在生产环境中正常工作，那么您可以编写少量集成测试，以确保您的代码能够与外部真实的代码一起工作。