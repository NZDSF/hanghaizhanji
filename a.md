开篇





大家好，本次Spring源码专栏相比于原来的专栏，几乎做了一次颠覆式的重置，核心着重于Spring源码的分析，而非项目实战性质。

本次专栏的核心，是以Spring容器初始化、bean的加载以及Spring AOP这三条主线贯穿整个专栏，我们会从零开始，带大家一步步搭建Spring源码环境，采用一步一图的方式带大家分析Spring源码，相信大家通过本专栏的学习，对Spring的一些核心机制以及常见的面试题，都能轻松搞定了。

* * *




作为本专栏的开篇文章，主要就是带大家先搭建一下Spring的源码环境，包括以下几个步骤：

1.从Spring官网一步步找到Spring源码在github上的位置，并拉取Spring源码

2.安装和配置Gradle，用于构建Spring的源码

3.将Spring源码导入到IDEA中，IDEA结合Gradle来构建Spring的源码

* * *

# 从github拉取Spring的源码




好了，在开始源码分析前，我们先搭建下Spring的源码环境，Spring源码目前是在github上托管的，我们通过链接：<https://spring.io/projects/spring-framework>，到spring官网看一下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d00ba18ea6b44c8687e3b1f2b2dacd2d~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=1024&s=222486&e=png&b=ffffff)

通过点击图片右上角的猫头图标，我们可以定位到spring源码在github上的位置：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e93ef2e4dd434c1499c12f5892295f83~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=910&s=160392&e=png&b=ffffff)

Spring源码默认是位于main分支上的，本次专栏采用的是v5.2.6.RELEASE这个版本的代码，所以，大家可以先切换到分支5.2.x：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8a6be12efe6e4aa89b3f282cd253354d~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=890&s=166402&e=png&b=fefefe)

然后基于分支5.2.x再切换到v5.2.6.RELEASE这个tag上：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5c8120e128bb470aa70047599af26561~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=867&s=167809&e=png&b=fefefe)

然后，我们可以下载这个tag下对应的Spring源码ZIP包：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2181b17036f84bccaa2544c7ade5b649~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=892&s=190878&e=png&b=ffffff)




或者，大家可以像我一样，直接在本地的git上拉取spring的源码：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a2f5ebd23d2d48a7b960296751a2bcf3~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=920&h=150&s=69859&e=png&b=000000)

git的搭建这里就不赘述了，大家可以网上找些资料，不过，由于Spring源码是在国外，下载的速度可能会慢一点。

* * *

当我们成功从github拉取源码到本地之后，再通过checkout命令，切换到v5.2.6.RELEASE这个tag中：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a1290ad3bd5f453ca94cd6f37235d6dd~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1613&h=362&s=59296&e=png&b=000000)

到现在为止，Spring源码我们已经准备好了，但是，因为Spring源码的构建并不是通过Maven来构建的，而是在国外比较受欢迎的Gradle，所以，接下来我们还得要在本地安装一下Gradle以便构建Spring源码。

* * *

# Gradle的下载和环境配置




我们可以通过链接 <https://gradle.org/releases/> ，到Gradle官网看下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c70202e8ae8d4c9e9584781e18245f21~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=642&s=85312&e=png&b=ffffff)

我们选择下载v6.5这个版本，然后在解压到本地目录中：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fd5623e681cb4455ab901fd3c6f0e8e6~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=840&h=411&s=40161&e=png&b=ffffff)

* * *




然后，我们还需要在电脑中配置下Gradle的环境变量，并将Gradle的bin目录添加到Path路径中：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/078cdc63c91740869f95922e17998fa6~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=847&h=449&s=31408&e=png&b=f3f3f3)

接着，我们打开命令窗口，输入命令“gradle -version”再回车，如果看到如下图一样的Gradle版本信息，就说明Gradle在本地安装成功了：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a7d315be0d64616b8484db41303e031~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1252&h=623&s=62381&e=png&b=0c0c0c)

# 将Spring源码导入IDEA中

接下来，我们可以准备将下载好的Spring源码导入IDEA中了，导入IDEA之前，我们需要修改一下Spring源码中，关于构建Gradle的一些配置，方便后续Spring源码的构建。

* * *

我们在spring-framework源码目录下，可以找到gradle.properties、settings.gradle和build.gradle这三个配置文件，我们需要调整下这些配置的参数，方便Gradle编译Spring源码。




其中，gradle.properties配置文件调整后如下：

```
version=5.2.6.RELEASE ## Gradle编译时，会下载很多东西，占用内存较大，建议适当调大点 org.gradle.jvmargs=-Xmx2048M ## 开启Gradle的缓存 org.gradle.caching=true ## 开启Gradle并行编译 org.gradle.parallel=true ## 开启Gradle守护进程模式 org.gradle.daemon=true
```

* * *




而在settings.gradle配置文件中的repositories配置项，需要再添加阿里云的仓库地址：

maven { url "[https://maven.aliyun.com/repository/public"](https://maven.aliyun.com/repository/public%22) }

这样可以加快Gradle构建Spring源码的速度：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/866c057fc3544094b401985e99ea96e0~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=864&s=142944&e=png&b=fefefe)

* * *

而在build.gradle配置文件中的repositores配置项中，也需要添加阿里云仓库的配置：

maven { url 'https://maven.aliyun.com/nexus/content/groups/public/' }  
       maven { url 'https://maven.aliyun.com/nexus/content/repositories/jcenter'}

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cdac3543b00642199fa148f58dabc3af~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=535&s=103361&e=png&b=fefefe)

同时，我们需要注释以下的配置，因为在Gradle构建v5.2.6.RELEASE版本的Spring源码时，相应的jar包可能下载不到了，如果不注释掉的话可能会导致Gradle构建失败，这个坑大家需要注意下：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a19552e15b8847b2badee14274882934~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=600&s=121809&e=png&b=fefefe)

* * *

最后，我们只需要将spring源码导入到IDEA中即可，Gradle默认就会启动后台的进程来构建Spring源码了，如下图所示：

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56ccdea406e2413b9b606c89fa343292~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=991&s=158546&e=png&b=fafafa)

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2ea3da90d954241b2f933d544eb86c4~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1920&h=976&s=188972&e=png&b=f9f9f9)

* * *

# 总结




好了，今天的知识点，我们就讲到这里了，我们来总结一下吧。




这一节主要就是带大家搭建了Spring的源码环境，包括从github上拉取Spring源码、下载和配置Gradle、调整Spring源码中的Gradle配置，最终将Spring源码导入到IDEA中。

从下一节开始，我们就开始着手准备Spring源码的分析了。
