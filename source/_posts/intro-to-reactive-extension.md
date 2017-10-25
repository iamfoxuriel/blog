---
title: ReactiveX
date: 2016-3-22 18:33:46
thumbnailImage: Rx.png
tags:
---
{% blockquote  %}
ReactiveX 简称 Rx，全称 Reactive Extensions，最初是LINQ的一个扩展，由微软的架构师Erik Meijer领导的团队开发，在2012年11月开源，Rx是一个编程模型，目标是提供一致的编程接口，帮助开发者更方便的处理异步数据流。
{% endblockquote %}
<!-- more -->

ReactiveX 是一个基于一系列可观察的异步和基础事件编程组成的一个库。
它继承观察者模式，支持序列数据或者事件。更高级的用法允许你将如下的一些抽象概念操作一起联合使用，比如低线程，同步，线程安全，数据并发，非阻塞I/O流。
它通常被称为“函数响应式编程”，这是用词不当的。ReactiveX 可以是函数式的，可以是响应式的，但是和“函数响应式编程”是不同的概览。一个主要的不同点是“函数响应式编程”是对随着时间不停变化的值进行操作的，而ReactiveX是对超时提交产生的离散值上。

# 为什么使用Observables？

ReactiveX 可见模式允许你使用数组等数据项的集合来进行些异步事件流组合操作。它使你从繁琐的web式回调中解脱，从而能使得代码可读性大大提高，同时减少bug的产生。

## Observables是可以进行一系列组合的

它技术上和[Java Futures](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html)一样简单是直接当着一个异步执行使用的，但是当Futures被嵌套使用复杂度会大大增加。
很难直接使用Futures去优化条件异步执行流程。然而Observables可以。使用Future.get()马上使其变得复杂或者提前进入阻塞状态，使的异步执行的优点淡然无存。
ReactiveX Observables 是为了处理组合流和异步序列数据设计的。


## Observables 数据是灵活的
ReactiveX Observables不单单支持单个标量的值（想Futures）的提交,同时还支持多值甚至是很多的数据流。Observables是一个单个抽像，能被用到一些用户场景中。Observable和Iterable一样运用灵活并且可以进行更加高雅的操作。

| event | Iterable (pull) |  Observable (push) |
| --------   | -----:   | :----: |
| event | T next() | onNext(T) |
| retrieve data | throws Exception | onError(Exception) |
| complete | !hasNext() | onCompleted() |


## Observables 是类型多变

并发、异步资源可以应用在ReactiveX中，Reactiviex可以当着一个线程池使用，甚至loops，非阻塞I/O线程使用，无论什么能够更好符合你的需要，风格、专长。客户端代码处理交互使用Observables进行异步处理，无论你潜在的操作是阻塞或者非阻塞都可以选择它来实现。

&nbsp;Observable 如何执行呢?

* * *
{% codeblock lang:java  %}
  public Observable getData();
{% endcodeblock %}
>.作为调用者是否异步运行在同一个线程中?
>.是否异步运行在不同的线程中?
>.是否被划分在多种线程中并且顺序返回数据对于调用者?
>.是否使用 Actor (or multiple Actors) 代替线程池?
>.是否使用网络NIO(网络接口对象)通过loop异步进行网络异步访问？
>.使用使用loop 来将工作线程从回调线程中剥离开？

* * *

## 回调函数存在问题

回调函数为了解决 Future.get()过早的阻塞问题是通过不允许执行任何阻塞的行为。这能处理很好因为执行的时候响应数据已经就绪了。

未来，回调函数能够简单通过单个级别的异步执行，然而通过嵌套就显得不灵活。

## ReactiveX 是跨语言的

ReactiveX 当前被多种编程语言实现，它遵守各种语言惯用语，同时多种语言正在加入其中。

# 交互式编程

ReactiveX 提供操作符集合，你可以使用 filter, select, transform, combine, and compose Observables。这将形成有效执行和结构。

你可以将Observable类的“push”行为等价于iterable的“pull”。使用Iterable消费者拉取数据从生产者此时线程阻塞直到数据到达。相比之下Observable生产者上传数据给消费者无论何时只要数据可用。这个方式是非常灵活的，因为数据到达可以使用同步或者异步的方式。

高阶函数在 Iterator/Observable应用的相似处
{% codeblock lang:java  Iterable%}
  getDataFromLocalMemory()
    .skip(10)
    .take(5)
    .map({ s -> return s + " transformed" })
    .forEach({ println "next => " + it })
{% endcodeblock %}

{% codeblock lang:java Observable %}
getDataFromNetwork()
  .skip(10)
  .take(5)
  .map({ s -> return s + " transformed" })
  .subscribe({ println "onNext => " + it })
{% endcodeblock %}

Observable 类型 相对于the Gang of Four’s Observer pattern增加了2个缺失的语法，去匹配可用的Iterable 类型。

&nbsp; &nbsp; &nbsp; &nbsp;1.当前没有跟多可用数据时候生产者通知消费者的能力（onCompleted方法）

&nbsp; &nbsp; &nbsp; &nbsp;2.当前发生错误的时候生产者通知消费者的能力（onErro方法）

通过以上这些，ReactiveX使得Iterable和Observable类型一致。唯一的不同是数据流动的方向。这是非常重要的因为现在你在Iterable上面的一系列操作同样可以用Oberservable达到效果。




## RX 目前支持的语言和平台
* 语言
    *   Java: [RxJava](https://github.com/ReactiveX/RxJava)
    *   JavaScript: [RxJS](https://github.com/ReactiveX/rxjs)
    *   C#: [Rx.NET](https://github.com/Reactive-Extensions/Rx.NET)
    *   C#(Unity): [UniRx](https://github.com/neuecc/UniRx)
    *   Scala: [RxScala](https://github.com/ReactiveX/RxScala)
    *   Clojure: [RxClojure](https://github.com/ReactiveX/RxClojure)
    *   C++: [RxCpp](https://github.com/Reactive-Extensions/RxCpp)
    *   Lua: [RxLua](https://github.com/bjornbytes/RxLua)
    *   Ruby: [Rx.rb](https://github.com/Reactive-Extensions/Rx.rb)
    *   Python: [RxPY](https://github.com/ReactiveX/RxPY)
    *   Go: [RxGo](https://github.com/ReactiveX/RxGo)
    *   Groovy: [RxGroovy](https://github.com/ReactiveX/RxGroovy)
    *   JRuby: [RxJRuby](https://github.com/ReactiveX/RxJRuby)
    *   Kotlin: [RxKotlin](https://github.com/ReactiveX/RxKotlin)
    *   Swift: [RxSwift](https://github.com/kzaher/RxSwift)
    *   PHP: [RxPHP](https://github.com/ReactiveX/RxPHP)
    *   Elixir: [reaxive](https://github.com/alfert/reaxive)
    *   Dart: [RxDart](https://github.com/ReactiveX/rxdart)
* 平台
    *   [RxNetty](https://github.com/ReactiveX/RxNetty)
    *   [RxAndroid](https://github.com/ReactiveX/RxAndroid)
    *   [RxCocoa](https://github.com/kzaher/RxSwift)

## 1. 用于创建Observable的操作符：

*   filter() —输出和输入相同的元素，并且会过滤掉那些不满足检查条件的。
*   take() —输出最多指定数量的结果。
*   Delay() —让发射数据的时机延后一段时间
*   Create — 通过调用观察者的方法从头创建一个Observable
*   Defer — 在观察者订阅之前不创建这个Observable，为每一个观察者创建一个新的Observable
*   Empty/Never/Throw — 创建行为受限的特殊Observable
*   From — 将其它的对象或数据结构转换为Observable
*   Interval — 创建一个定时发射整数序列的Observable
*   Just — 将对象或者对象集合转换为一个会发射这些对象的Observable
*   Range — 创建发射指定范围的整数序列的Observable
*   Repeat — 创建重复发射特定的数据或数据序列的Observable
*   Start — 创建发射一个函数的返回值的Observable
*   Timer — 创建在一个指定的延迟之后发射单个数据的Observable

## 2. 用于对Observable发射的数据进行变换的操作符：

*   Buffer — 缓存，可以简单的理解为缓存，它定期从Observable收集数据到一个集合，然后把这些数据集合打包发射，而不是一次发射一个
*   FlatMap — 扁平映射，将Observable发射的数据变换为Observables集合，然后将这些Observable发射的数据平坦化的放进一个单独的Observable，可以认为是一个将嵌套的数据结构展开的过程。
*   GroupBy — 分组，将原来的Observable分拆为Observable集合，将原始Observable发射的数据按Key分组，每一个Observable发射一组不同的数据
*   Map — 映射，通过对序列的每一项都应用一个函数变换Observable发射的数据，实质是对序列中的每一项执行一个函数，函数的参数就是这个数据项
*   Scan — 扫描，对Observable发射的每一项数据应用一个函数，然后按顺序依次发射这些值
*   Window — 窗口，定期将来自Observable的数据分拆成一些Observable窗口，然后发射这些窗口，而不是每次发射一项。类似于Buffer，但Buffer发射的是数据，Window发射的是Observable，每一个Observable发射原始Observable的数据的一个子集

## 3. 线程切换和控制相关操作符：

*   subscribeOn(Scheduler) — 指定事件的call方法以及以前的操作到一个线程中
*   observeOn(Scheduler) — 指定事件的call方法之后的操作（如：map(),onNext(),onCompleted(),onError()）到一个线程中【注意：不包括Subscriber.onStart()方法，该方法在默认它所在的线程中执行】
*   参数Scheduler有：
    *   Schedulers.immediate():默认模式。直接使用当前线程运行。
    *   Schedulers.newThread():总是启动新线程，并在新线程中运行。
    *   Schedulers.io():I/O 操作（读写文件、读写数据库、网络信息交互等）所使用的 Scheduler。行为模式和 newThread() 差不多，区别在于 io() 的内部实现是是用一个无数量上限的线程池，可以重用空闲的线程，因此多数情况下 io() 比 newThread() 更有效率。不要把计算工作放在 io() 中，可以避免创建不必要的线程
    *   Schedulers.computation(): 计算所使用的 Scheduler。这个计算指的是 CPU 密集型计算，即不会被 I/O 等操作限制性能的操作，例如图形的计算。这个 Scheduler 使用的固定的线程池，大小为 CPU 核数。不要把 I/O 操作放在 computation() 中，否则 I/O 操作的等待时间会浪费 CPU。
    *   AndroidSchedulers.mainThread():它指定的操作将在 Android 主线程运行。

## 4. Single相关方法汇总：

*   `compose()` -- 创建一个自定义的操作符
*   `concat()/concatWith()` -- 连接多个Single和Observable发射的数据
*   `create()` -- 调用观察者的create方法创建一个Single
*   `error()` -- 返回一个立即给订阅者发射错误通知的Single
*   `map()`
*   `flatMap()`
*   `flatMapObservable():Observable` -- 返回一个Observable，它发射对原Single的数据执行flatMap操作后的结果
*   `from():Single` -- 将Future转换成Single
*   `just(V)` -- 返回一个发射一个指定值V的Single
*   `merge()` -- 将一个Single(它发射的数据是另一个Single，假设为B)转换成另一个Single(它发射来自另一个Single(B)的数据)
*   `merge()/mergeWith():Observable` -- 合并发射来自多个Single的数据
*   `observeOn()` -- 指示Single在指定的调度程序上调用订阅者的方法
*   `subscribeOn()` -- 指示Single在指定的调度程序上执行操作
*   `onErrorReturn()` -- 将一个发射错误通知的Single转换成一个发射指定数据项的Single
*   `timeout()` -- 它给原有的Single添加超时控制，如果超时了就发射一个错误通知
*   `toSingle()` -- 将一个发射单个值的Observable转换为一个Single
*   `zip()/zipWith():Single` -- 将多个Single转换为一个，后者发射的数据是对前者应用一个函数后的结果


# 参考资料：

[http://reactivex.io/intro.html](http://reactivex.io/intro.html)