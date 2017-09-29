---
title: Java 9 – The Ultimate Feature List
date: 2017-09-27 19:42:01
thumbnailImage: openjdk.png
categories:
- Translation
- Java
tags:
- Java 9
- 新特性
- Java
---

{% blockquote   Java 9 的发布在历经多次跳票之后，终于正式发布。从这个版本开始，Java 将每半年发布一个版本。
                本文翻译自Java 9 Ultimate Feature List, 让我们看看这次发布我们将拥有哪些高级特性。 http://openjdk.java.net/projects/jdk9/ openjdk.java.net %}

{% endblockquote %}
<!-- more -->
[![Java 9 Telescope](http://384uqqh5pka2ma24ild282mv.wpengine.netdna-cdn.com/wp-content/uploads/2014/09/Java-9-Telescope.png)](http://384uqqh5pka2ma24ild282mv.wpengine.netdna-cdn.com/wp-content/uploads/2014/09/Java-9-Telescope.png)

**This post will be updated with new features targeted at the upcoming Java 9 release** 

The OpenJDK development is picking up speed: after the Java 8 launch in March 2014, we’re expecting to enter a 2 year release cycle. Java 9 will reportedly be released in 2016, and the JEPs (JDK Enhancement Proposals) that target the release keep getting published. Moreover, some JSRs (Java Specification Requests) are already being worked on and we’ve also added a hint of other features that might be included.


The flagship features are the Jigsaw project, significant performance improvements and long awaited APIs including: Process API updates, JSON as part of java.util and a money handling API. For those of you who want to be on the bleeding edge, JDK 9 early access builds are already available [here](https://jdk9.java.net/).

In this post we’ll keep updating around the main new features for Java 9 and what they’re all about. So stay tuned for additional updates!

## Table of contents

<span style="color: #339966;"><span style="color: #000000;">1.</span> [Accepted]</span> [Project Jigsaw – Modular Source Code](#jigsaw)  
<span style="color: #339966;"><span style="color: #000000;">2.</span> [Accepted]</span> [Process API Updates](#processapi)  
<span style="color: #339966;"><span style="color: #000000;">3.</span> [Accepted]</span> [Light Weight JSON API](#jsonapi)  
<span style="color: #339966;"><span style="color: #000000;">4.</span> [Accepted]</span> [Money and Currency API](#moneyapi)  
<span style="color: #339966;"><span style="color: #000000;">5.</span> [Accepted]</span> [Improved Contended Locking](#locking)  
<span style="color: #339966;"><span style="color: #000000;">6.</span> [Accepted] </span>[Segmented Code Cache](#codecache)  
<span style="color: #339966;"><span style="color: #000000;">7.</span> [Accepted] </span>[Smart Java Compilation – Phase Two](#sjavac)  
<span style="color: #339966;"><span style="color: #ff9900;"><span style="color: #000000;">8.</span> [Expected]</span> </span>[HTTP 2 Client](#http2)  
<span style="color: #ff9900;"><span style="color: #000000;">9.</span> [</span><span style="color: #ff9900;">Expected</span><span style="color: #ff9900;">] </span>[REPL in Java](#repl)

### Update 20/11/2014:

<span style="color: #339966;"><span style="color: #000000;">10.</span> [Accepted]</span> [Unified JVM Logging](#unifiedlogging)  
<span style="color: #339966;"><span style="color: #000000;">11.</span> [Accepted]</span> [Compiler Control](#compilercontrol)  
<span style="color: #339966;"><span style="color: #000000;">12.</span> [Accepted]</span> [Datagram Transport Layer Security (DTLS)](#dtls)  
<span style="color: #339966;"><span style="color: #000000;">13.</span> [Accepted]</span> [HTML5 Javadoc](#javadoc)

### Additional fixes / cleanup:

<span style="color: #339966;"><span style="color: #000000;">14.</span> [Accepted]</span> [Elide Deprecation Warnings on Import Statements](#elideimport)  
<span style="color: #339966;"><span style="color: #000000;">15.</span> [Accepted]</span> [Resolve Lint and Doclint Warnings](#doclint)  
<span style="color: #339966;"><span style="color: #000000;">16.</span> [Accepted]</span> [Milling Project Coin](#millingcoin)  
<span style="color: #339966;"><span style="color: #000000;">17.</span> [Accepted]</span> [Remove GC Combinations Deprecated in JDK 8](#removegc)  
<span style="color: #339966;"><span style="color: #000000;">18.</span> [Accepted]</span> [Process Import Statements Correctly](#processimport)
<span style="color: #000000;">19.</span> [Where do new features come from?](#newfeatures)

## Accepted features

<a name="jigsaw"></a>

### 1\. Project Jigsaw – Modular Source Code

[Project Jigsaw](http://openjdk.java.net/projects/jigsaw/ "Project Jigsaw")’s goal is to make Java modular and break the [JRE](http://www.oracle.com/technetwork/java/javase/tech/index.html) to interoperable components, one of the most hyped features for Java 9\. This JEP is the first out of [4 steps](http://mail.openjdk.java.net/pipermail/jigsaw-dev/2014-July/003417.html) towards Jigsaw and will not change the actual structure of the JRE and JDK. The purpose of this step is to reorganize the JDK source code into modules, enhance the build system to compile modules, and enforce module boundaries at build time. The project was originally intended for Java 8 but was delayed since and retargeted at Java 9.
Jigsaw项目是为了模块化Java代码、将JRE分成可相互协作的组件，这也是Java 9 众多特色种的一个。JEP是迈向Jigsaw四步中的第一步，它不会改变JRE和JDK的真实结构。JEP是为了模块化JDK源代码，让编译系统能够模块编译并在构建时检查模块边界。这个项目原本是随Java 8发布的，但由于推迟，所以将把它加到Java 9.

Once it’s finished, it would allow creating a scaled down runtime Jar (rt.jar) customised to the components a project actually needs. The JDK 7 and JDK 8 rt.jar’s have about 20,000 classes that are part of the JDK even if many of them aren’t really being used in a specific envrionment (although a partial solution is included in the Java 8 [compact profiles](http://www.oracle.com/technetwork/java/embedded/resources/tech/compact-profiles-overview-2157132.html "Java 8 Compact Features") feature). The motivation behind this is to make Java easily scalable to small computing devices (Internet of Things), improve security and performance, and make it easier for developers to construct and maintain libraries.
一旦它完成，它可能允许根据一个项目需求自定义组件从而减少rt.jar的大小。在JDK 7 和JDK 8的rt.jar包中有大约20,000个类，但有很多类在一些特定的环境里面并没有被用到(即使在Java 8的紧凑分布特性中已经包含了一部分解决方法也存在着类冗余)。这么做是为了能让Java能够容易应用到小型计算设备(比如网络设备)中，提高它的安全和性能，同时也能让开发者更容易构建和维护这些类库。

[More about JEP 201](http://openjdk.java.net/jeps/201 "JEP 201")

<a name="processapi"></a>




### 2\. Process API Updates

So far there has been a limited ability for controlling and managing operating system processes with Java. For example, in order to do something as simple as get your process PID today, you would need to either access native code or use some sort of a workaround. More than that, it would require a different implementation for each platform to guarantee you’re getting the right result.
截止到目前，Java控制与管理系统进程的能力是有限的。举个例子，现在为了简便获取你程序的进程PID，你要么调用本地程序要么要自己使用一些变通方案。更多的是，每个（系统）平台需要有一个不同实现来确保你能获得正确的结果。
In Java 9, expect the code for retrieving Linux PIDs, that now looks like this:

  {% codeblock lang:java  %}
      public static void main(String[] args) throws Exception
      {
          Process proc = Runtime.getRuntime().exec(new String[]{ "/bin/sh", "-c", "echo $PPID" });
      
          if (proc.waitFor() == 0)
          {
              InputStream in = proc.getInputStream();
              int available = in.available();
              byte[] outputBytes = new byte[available];
      
          in.read(outputBytes);
          String pid = new String(outputBytes);
      
          System.out.println("Your pid is " + pid);
          }
      }
    {% endcodeblock %}

To turn into something like this (that also supports all operating systems):
  {% codeblock lang:java  %}
  System.out.println("Your pid is " + Process.getCurrentPid());
   {% endcodeblock %}

The update will extend Java’s ability to to interact with the operating system: New direct methods to handle PIDs, process names and states, and ability to enumerate JVMs and processes and more.
这次更新将会扩展Java与操作系统的交互能力：新增一些新的直接明了的方法去处理PIDs，进程名字和状态以及枚举多个JVM和进程以及更多事情。


[More about JEP 102](http://openjdk.java.net/jeps/102 "JEP 102")  
<a name="jsonapi"></a>

### 3\. Light-Weight JSON API

Currently there are alternatives available for handling JSON in Java, what’s unique about this API is that it would be part of the language, lightweight and would use the new capabilities of Java 8\. And will be delivered right through java.util (Unlike [JSR 353](https://jsonp.java.net/ "JSONP") which uses an external package or other [alternatives](https://code.google.com/p/google-gson/ "google-gson")).

目前有多种处理JSON的Java工具，但JSON API 独到之处在于JSON API将作为Java语言的一部分，轻量并且运用Java 8的新特性。它将放在java.util包里一起发布(但在JSR 353里面的JSON是用第三方包或者其他的方法处理的).
** Code samples coming soon!

[More about JEP 198](http://openjdk.java.net/jeps/198 "JEP 198")  
<a name="moneyapi"></a>

### 4\. Money and Currency API

After the new [Date and Time API](http://blog.takipi.com/5-features-in-java-8-that-will-change-how-you-code/#datetime "Date and Time API") introduced in Java 8, Java 9 brings with it a new and official API for representing, transporting, and performing comprehensive calculations with Money and Currency. To find out more about the project, you can visit JavaMoney on Github. Code and usage examples are already available right here . Here are a few highlights:
在Java 8引进了日期和时间的API之后, Java 9引入了新的货币API, 用以表示货币, 支持币种之间的转换和各种复杂运算. 关于这个项目的具体情况, 请访问https://github.com/JavaMoney,里面已经给出了使用说明和示例, 以下是几个重要的例子:
 {% codeblock lang:java  %}
          Money amt1 = Money.of(10.1234556123456789, "USD"); // Money is a BigDecimal
          FastMoney amt2 = FastMoney.of(123456789, "USD"); // FastMoney is up to 5 decimal places
          Money total = amt1.add(amt2);      
   {% endcodeblock %}
   
_The new money types: Money & FastMoney_

 {% codeblock lang:java  %}
         MonetaryAmountFormat germanFormat = MonetaryFormats.getAmountFormat( Locale.GERMANY);
         System.out.println(germanFormat.format(monetaryAmount)); // 1.202,12 USD
   {% endcodeblock %}
   

_Formatting money according to different countries_

[More about JSR 354](https://jcp.org/en/jsr/detail?id=354 "JSR 354")  
<a name="locking"></a>

### 5\. Improve Contended Locking

Lock contention is a performance bottleneck for many multithreaded Java applications. The enhancement proposal looks into improving the performance of Java object monitors as measured by different benchmarks. One of the these tests is [Volano](http://www.volano.com/benchmarks.html "Volano"). It simulates a chat server with huge thread counts and client connections, many of them trying to access the same resources and simulate a heavy duty real world application.

These kind of stress tests push JVMs to the limit and try to determine the maximum throughput they can achieve, usually in terms of messages per second. The ambitious success metric for this JEP is a significant improvement over 22 different benchmarks. If the effort will succeed, these performance improvements will be rolling out in Java 9.

锁争用是限制许多Java多线程应用性能的瓶颈. 新的机制在改善Java对象监视器的性能方面已经得到了多种基准(benchmark)的验证, 其中包括Volano. 测试中通讯服务器开放了海量的进程来连接客户端, 其中有很多连接都申请同一个资源, 以此模拟重负荷日常应用.

通过诸如此类的压力测试我们可以估算JVM的极限吞吐量(每秒的消息数量). JEP在22种不同的测试中都得到了出色的成绩, 新的机制如果能在Java 9中得到应用的话, 应用程序的性能将会大大提升.
[More about JEP 143](http://openjdk.java.net/jeps/143 "JEP 143")  
<a name="codecache"></a>

### 6\. Segmented Code Cache

Another performance improvement for Java 9 is coming from the JIT compiler angle. When certain areas of code are executed rapidly, the VM compiles them to native code and stores them in the code cache. This update looks into segmenting the code cache to different areas of compiled code in order to improve the compiler’s performance.

Instead of a single area, the code cache will be segmented into 3 by the code’s lifetime in the cache:  
– Code that will stay in the cache forever (JVM internal / non-method code)  
– Short lifetime (Profiled code, specific to a certain set of conditions)  
– Potentially long lifetime (Non-profiled code)  
The segmentation would allow for several performance improvements to happen. For example, the method sweeper would be able to skip non-method code and act faster.

Java 9的另一个性能提升来自于JIT(Just-in-time)编译器. 当某段代码被大量重复执行的时候, 虚拟机会把这段代码编译成机器码(native code)并储存在代码缓存里面, 进而通过访问缓存中不同分段的代码来提升编译器的效率.

和原来的单一缓存区域不同的是, 新的代码缓存根据代码自身的生命周期而分为三种:

- 永驻代码(JVM 内置 / 非方法代码)
- 短期代码(仅在某些条件下适用的配置性(profiled)代码)
- 长期代码(非配置性代码)

缓存分段会在各个方面提升程序的性能, 比如做垃圾回收扫描的时候可以直接跳过非方法代码(永驻代码), 从而提升效率.

[More about JEP 197](http://openjdk.java.net/jeps/197 "JEP 197")  
<a name="sjavac"></a>

### 7\. Smart Java Compilation, Phase Two

The Smart Java Compilation tool, or sjavac, was first worked on around [JEP 139](http://openjdk.java.net/jeps/139 "JEP 139") in order to improve JDK build speeds by having the javac compiler run on all cores. With JEP 199, it enters Phase Two, where it will be improved and generalized so that it can be used by default and build other projects than the JDK.

智能Java编译工具sjavac的第一阶段开始于JEP 139这个项目, 用于在多核处理器上提升JDK的编译速度. 现在这个项目已经进入第二阶段(JEP 199), 目的是改进sjavac并让其成为取代目前JDK编译工具javac的Java默认的通用编译工具.
[More about JEP 199](http://openjdk.java.net/jeps/199 "JEP 199")

## What else to expect?

<a name="http2"></a>

### 8\. HTTP 2 Client

HTTP 2.0 hasn’t been released yet as a standard but it will be submitted for final review soon and it’s expected to be finalized before the release of Java 9\. JEP 110 will define and implement a new HTTP client for Java that will replace HttpURLConnection, and also implement HTTP 2.0 and websockets. It wasn’t published as an accepted JEP yet but its targeting Java 9 and we expect it to be included.

The official HTTP 2.0 RFC release date is currently set to February 2015, building on top of Google’s SPDY algorithm. SPDY has already shown great speed improvements over HTTP 1.1 ranging between 11.81% to 47.7% and its implementation already exists in most modern browsers.

HTTP 2.0标准虽然还没正式发布, 但是已经进入了最终审查阶段, 预计可以在Java 9发布之前审查完毕. JEP 110将会重新定义并实现一个全新的Java HTTP客户端, 用来取代现在的HttpURLConnection, 同时也会实现HTTP 2.0和网络接口(原文websockets). 它现在还没被JEP正式认可但我们希望在Java 9中包含这一项目的内容.

官方的HTTP 2.0 RFC(Request for Comments, 官方技术讨论/会议记录等等的一系列文档记录)预订于2015年2月发布, 它是基于Google发布的SPDY(Speedy, 快速的)协议. 基于SPDY协议的网络相对于基于HTTP 1.1协议的网络有11.81%到47.7%之间的显著提速, 现在已经有浏览器实现了这个协议.
[More about JEP 110](http://openjdk.java.net/jeps/110 "JEP 110")  
<a name="repl"></a>

### 9\. Project Kulla – REPL in Java

Recently announced, a bit unlikely to hit Java 9 but might make it on time with a targeted integration date set in April 2015\. Today there’s no “native” Java way to REPL (Read-Eval-Print-Loop). Meaning, if you want to run a few lines of Java to check out them quickly on their own you will have to wrap it all in a separate project or method. There are REPL add-ons to popular IDEs and some other solutions like Java REPL, but no official way to do this so far – Project Kulla might be the answer.

这个取名为Kulla的项目最近宣布将于2015年4月整合测试, 虽然已经不太有希望能赶上Java 9的发布, 但如果进度快的话或许刚好能赶上. 现在Java并没有来自官方的REPL(Read-Eval-Print-Loop)方式, 也就是说现在如果你想要跑几行Java代码做一个快速的测试, 你仍然需要把这几行代码封装在项目或者方法里面. 虽然在一些流行的IDE里面有Java REPL工具, 但它们并没有官方支持, 而Kulla项目或许就能成为Java官方发布的REPL解决方案. 
[More about Project Kulla](http://mail.openjdk.java.net/pipermail/announce/2014-August/000181.html "Project Kulla")


<a name="unifiedlogging"></a>

### 10\. Unified JVM Logging

Today it’s hard to make sense of the root cause for performance issues and crashes of the JVM. One way to tackle this is introducing one single system for all JVM components that would allow fine-grained, and easy-to-configure JVM logging. Currently, different components of the JVM use different mechanisms and conventions for logging, making it harder to debug.

[More about JEP 158](http://openjdk.java.net/jeps/158)  
<a name="compilercontrol"></a>

### 11\. Compiler Control

Taking on the HotSpot JVM and extending the controls for compiler options down to the method level. After this update, the JIT compiler options could be modified even during runtime depending on the specific method that’s being compiled.

[More about JEP 165](http://openjdk.java.net/jeps/165)  
<a name="dtls"></a>

### 12\. Datagram Transport Layer Security (DTLS)

Adding DTLS support, a secure way to communicate using datagram protocols such as UDP.

[More about JEP 219](http://openjdk.java.net/jeps/219)  
<a name="javadoc"></a>

### 13\. HTML5 Javadoc

Bringing Javadoc up to speed with the HTML standard: Generating modern HTML5 documentation.

[More about JEP 224](http://openjdk.java.net/jeps/224)

## **Fixes / Cleanup:**

<a name="elideimport"></a>

### 14\. Elide Deprecation Warnings on Import Statements

[More about JEP 211](http://openjdk.java.net/jeps/211)  
<a name="doclint"></a>

### 15\. Resolve Lint and Doclint Warnings

[More about JEP 212](http://openjdk.java.net/jeps/212)  
<a name="millingcoin"></a>

### 16\. Milling Project Coin

[More about JEP 213](http://openjdk.java.net/jeps/213)  
<a name="removegc"></a>

### 17\. Remove GC Combinations Deprecated in JDK 8

[More about JEP 214](http://openjdk.java.net/jeps/214)  
<a name="processimport"></a>

### 18\. Process Import Statements Correctly

[More about JEP 216](http://openjdk.java.net/jeps/216)
<a name="newfeatures"></a>

## 19\. Bonus: Where do new features come from?

JEPs and JSRs don’t usually pop out of nowhere, here’s the structure that holds it all together:

**Groups** – Individuals and organisations with a mutual interest around a broad subject or a specific body of code. Some examples are Security, Networking, Swing, and HotSpot.

**Projects** – Efforts to produce a body of code, documentation or other effort. Must be sponsored by at least one group. Recent examples are Project Lambda, Project Jigsaw, and Project Sumatra.

**JDK Enhancement Proposal** ([JEP](http://openjdk.java.net/jeps/0)) – Allows promoting a new specification informally before or in parallel to the JCP, when further exploration is needed. Accepted JEPs become a part of the JDK roadmap and assigned a version number.

**Java Specification Request** ([JSR](https://jcp.org/en/jsr/all)) – The actual specification of the feature happens in this stage, can be either coming through Groups/Projects, JEPs or from individual JCP (Java Community Process) members. An umbrella JSR is usually opened for each Java version, this has yet to happen with Java 9\. Individual members of the community can also propose new Java specification requests.

JEP和JSR并不是无中生有, 下面就介绍一下Java发展的生态环境:

小组 - 对特定技术内容, 比如安全, 网络, Swing, HotSpot, 有共同兴趣的组织和个人

项目 - 编写代码, 文档以及其他工作, 至少由一个小组赞助和支持, 比如最近的Lambda计划, Jigsaw计划和Sumatra计划.

JDK改进提案(JEP) - 每当需要有新的尝试的时候, JEP可以在JCP(Java Community Process)之前或者同时提出非正式的规范(specification). 被正是认可的JEP正式写进JDK的发展路线图并分配版本号.

Java规范提案(JSR) - 新特性的规范出现在这一个阶段, 可以来自于小组 / 项目, JEP, JCP成员或者Java社区(community)成员的提案. 每个Java版本都由相应的JSR支持, Java 9暂时还没有.