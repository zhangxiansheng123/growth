在 GitHub 上闲逛的时候，发现了一个新的项目：**maven-mvnd**，持续霸占 GitHub trending 榜单好几天了。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-01.png)

maven-mvnd，可以读作 Maven Daemon，译作 Maven 守护版，旨在为 Maven 提供更快的构建速度，灵感借鉴了 Gradle 和 Takari（Maven 生命周期优化器）。

>https://github.com/apache/maven-mvnd




Maven 和 Gradle 可以说是项目构建工具中的绝代双骄，我自己的观点是：**Maven 不比 Gradle 好，Gradle 也不比 Maven 好**。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-02.png)


瞧我这该死的观点，足够的圆滑。

Maven 的优点是稳定可靠，在绝大多数的项目上工作良好，社区生态很完善，几乎所有的 Java 开发者都在用。Maven 的缺点是，对于大一点的项目来说，构建太慢了。

Gradle 的优点是足够的灵活，构建速度也会更快一点，因为使用了后台进程和缓存机制。Gradle 的缺点是版本迭代速度太快，社区跟不上，对于初学者来说，学习曲线比较陡峭。

mvnd 并不是 Maven 的重构版，**等于是 Maven ∩ (Gradle & Takari) 部分优点的一个交集**。

mvnd 使用了以下架构方式：

- 内部嵌入了 Maven，所以不需要单独安装 Maven。
- 使用守护进程进行构建，守护进程可以为多个 mvnd 客户端的连续请求提供服务。
- 使用了内置的 [GraalVM](https://www.graalvm.org/reference-manual/native-image/) 虚拟机，和传统的 Java 虚拟机相比，它的启动速度更快，使用内存更少，内部的 JIT 编译器在编译时花费的时间也更少。
- 如果已有的守护进程都在工作中，则可以新建多个守护进程来支撑新的构建请求。

这种架构方式使得 mvnd 的性能优势得到了进一步提升。

好，我们来简单尝试下。

mvnd 像 Maven 一样，可以跨平台，支持 Windows、macOS和 Linux。自动化安装的命令也非常简单，如下所示：

```
# Windows
choco install mvndaemon 
# Linux
sdk install mvnd
# macOS
brew install mvndaemon/homebrew-mvnd/mvnd
```

为了方便演示，我这里采用手动安装的方式，速度也会更快一点。

通过下面的网址下载 mvnd 的 release 版本：

>https://github.com/apache/maven-mvnd/releases

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-03.png)

下载完成后解压，然后把 bin 目录添加到 PATH 路径下。

在终端执行 `mvnd -v` 就可以查看到 mvnd 的配置信息了。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-04.png)

如果出现类似下面这样的错误，未找到 JAVA_HOME，可以按照提示在对应的文件中追加 java.home 属性，也就是 JDK 的安装路径。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-05.png)

刚好之前搭建了一个Spring Boot 项目，我们可以拿 Maven 和 mvnd 来对比一下构建速度。

先执行 `mvn clean package` 命令，一共花费的时间是 5.318 秒。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-06.png)


再执行 `mvnd clean package` 命令，一共花费的时间是 3.225 秒。

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-07.png)

反复多测试几次，发现 mvnd 确实比 Maven 要快上许多！Maven 维持在 5 秒多，mvnd 维持在 3 秒左右。

当然了，我本地这个 Spring Boot 项目本身非常简单，如果是构建时间更长一点的项目，mvnd 的优势会更大。

感受一下 mvnd 在一个 24 核电脑上执行的样子吧，简直就是效率神器！

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-08.gif)

------


本篇已收录至 GitHub 上星标 1.0k+ star 的开源专栏《Java 程序员进阶之路》，该专栏风趣幽默、通俗易懂，对 Java 爱好者极度友好和舒适😄，内容包括但不限于 Java 基础、Java 集合框架、Java IO、Java 并发编程、Java 虚拟机、Java 企业级开发（Git、SSM、Spring Boot）等核心知识点。

该专栏目前也在霸榜 GitHub Trending（Java 类），这让二哥终于体会到了霸榜的快乐！

![](https://cdn.jsdelivr.net/gh/itwanger/toBeBetterJavaer/images/maven/mvnd-09.png)

star 了这个专栏就等于你在浩瀚的 Java 知识海洋里有了一盏再也不会迷路的灯塔。

>[https://github.com/itwanger/toBeBetterJavaer](https://github.com/itwanger/toBeBetterJavaer)


*没有什么使我停留——除了目的，纵然岸旁有玫瑰、有绿荫、有宁静的港湾，我是不系之舟*。



