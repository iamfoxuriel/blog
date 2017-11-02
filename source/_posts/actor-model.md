---
title: Actor 模型
date: 2015-7-01 17:04:07
categories:
- Tech
- Actor
tags: 
- Actor
- 并发模型
---

* * *
我们的CPU没有变得更快，但是我们现在的CPU拥有多个内核。 如果我们想要利用我们现在可用的所有这些硬件，我们需要一种并发运行代码的方法。 几十年的难以追踪的错误和开发者的沮丧表明，`threads`不是要走的路。 但是不要害怕，那里有很好的选择，今天向你展示其中之一：Actor 模型。
<!-- more -->
### Actor模型
Actor模型是处理并发计算的概念模型。 它定义了一些关于系统组件如何表现和相互交互的一般规则。 使用这种模式的最有名的语言可能是`Erlang`。 我们将尝试更多地关注模型本身，而不是在不同的语言或库中实现它。

### Actor概念
Actor是原始的计算单位。 这是接收消息的 _载体_，并且会基于消息进行某种计算。

这个概念与我们在面向对象语言中是非常相似的：一个对象接收一个消息（一个方法调用），并根据接收到的消息做什么（我们调用哪个方法）。
主要区别是Actor彼此之间完全隔离(isolated)，永远不会共享内存。 还值得一提的是，一个Actor可以保持一个永远不能被另一个Actor直接改变的私有状态。

##### Actors是成体系存在的

`One actor is no actor`正如`One ant is no ant`。
他们是一个完整的系统。 在Actor模型中，一切都是Actor，他们需要有指向（addresses），这样以来一个Actor可以发送一个消息给另一个Actor。

##### Actors have mailboxes

我们可以假设每个Actor都有一个自己的邮箱。
虽然多个Actor可以同时运行， 但是对于单个Actor是按顺序处理给定的消息， 这对我们理解至关重要。

消息是通过异步发方式被发送到Actor的， ，当处理另一个消息时，需要将它们存储在某处。 邮箱是存储这些邮件的地方。

{% asset_img actors.png%}

Actors之间通信是通过发送异步的消息， 这些异步的消息会被存到消息目标对象Actor的邮箱中，直到被处理。

* * *

##### Actors 做哪些事

当一个Actor接受到一个消息， 它可以做以下三类事情

*   创建更多的actors;
*   发送消息到其他的actors;
*   指定如何处理下一条消息.

前两个点非常容易理解，我们来谈最后一条。
就像之前说过的那样， 一个Actor会维护自身的私有状态。  “指定如何处理下一条消息” 的意思就是定义当对于接收到的下一个消息的时候，这个私有状态会变成什么


举个例子，我们有一个像计算器Actor，它的初始状态只是数字`0`。 当这个actor接收到`add(1)`消息时，它不会突变(mutating)它的原始状态，而是定义对于接收到的下一个消息，状态将为`1`。

### 错误容忍机制 

`Erlang` 推崇 “let it crash” 的哲学. 换句话说，您不需要写试图预测可能出现的所有可能的问题并找到一种方法来处理它们的防御性程序，因为没有人可以考虑每一个故障点。
`Erlang` 所做的是直接让程序崩溃，但是将关键代码的责任交给一段程序， 这段程序会监管上面那段代码， 当遇到道崩溃的时候该如何处理（例子：就像把这个单元的代码重置为一个稳定的状态）。然而能让所有变成可能的就是Actor 模型。

每一行代码都在 `process`中运行（`Erlang` 称之为Actors）。这些`process` 是完全隔绝的，也就是说它们的状态不会被其他的`process`所影响。在此之外我们有一个监管程序，它实际上是另外一个`process` (记得吗?everything is an actor)，这些监管程序当遇到被监管`process`崩溃的时候会被唤醒去处理。

这让一个自我修复的系统变成可能。如果一个Actor出现异常状态并且崩溃，无论什么原因，监管程序的Actor可以做一些事情来尝试将其置于一个一致的状态（有多种策略可以实现，最常见的只是用初始状态重新初始化被监管的Actor）。

### 分布式

Actor模型的另一个有趣的方面是，我发送消息的Actor是在本地运行还是在另一个节点运行并不重要。

假设一下，如果一个Actor只是一个具有邮箱和内部私有状态的代码单元。
当它返回消息的时候，谁在乎具体是哪台机器正真在运行这段程序? 只要我们能正确的到**消息**的返回。
这个单位的邮箱和一个内部状态的代码，它只是回应消息，谁在乎哪台机器它正在运行？
这个机制，允许我们建立分布式计算的系统，当任意一个节点失败，我们也不是可以从其他的计算单元的到返回信息。

### 其他资源

这是概念模型的简要概述，它是诸如`Erlang` 和`Elixir`等伟大语言的基础，同样对于`Akka`（`JVM`）和`Celluloid` （`Ruby`）这样的类库Actor也是基础。

如果我成功地让你好奇这个模型是如何在现实世界中实现和使用的，这是我阅读或正在阅读的关于这个话题推荐的书籍清单：

*   [Seven Concurrency Models in Seven Weeks: When Threads Unravel](https://pragprog.com/book/pb7con/seven-concurrency-models-in-seven-weeks)
*   [Programming Elixir](https://pragprog.com/book/elixir/programming-elixir)
*   [Elixir in Action](http://www.manning.com/juric/)

如果您有兴趣了解概念本身的更多细节，我强烈推荐这个讨论视频
{% youtube 7erJ1DV_Tlo %}