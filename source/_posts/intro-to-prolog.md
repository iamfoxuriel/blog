---
title: Prolog初窥
date: 2015-09-25 13:29:33
categories:
- Tech
- Prolog
tags:
- Prolog
---
{% pullquote %}
事实是我们对这个世界直接观察的结果。规则是关于现实世界的逻辑推论。
{% endpullquote %}
Prolg(Programing in logic)是其实属于DSL,并不算是完整意义上的编程语言，是一阶谓词为基础的逻辑性语言
在Prolog中变量的命名是有特殊要求的，如果一个词以小写字母开头，它就是一个原子(atom)，类似于其他语言中的符号(symbol)，如果一个词以大写或下划线开头，那么它就是一个变量，和其他语言一样变量值可以改变，可以赋值（不过更灵活）。
<!-- excerpt -->

最近在一个语义相似度分析的项目中接触到Prolog，对Prolog有了一个感性的认识，想要用Prolog解决一些实际问题之前必须要先了解它们。


Prolg(Programing in logic)是其实属于DSL,并不算是完整意义上的编程语言，是一阶谓词为基础的逻辑性语言
在Prolog中变量的命名是有特殊要求的，如果一个词以小写字母开头，它就是一个原子(atom)，类似于其他语言中的符号(symbol)，如果一个词以大写或下划线开头，那么它就是一个变量，和其他语言一样变量值可以改变，可以赋值（不过更灵活）。

符号组成一些事实：
{% codeblock %}
likes(Jack,Bob).
likes(Sam,Bob).
likes(John,Scott).
{% endcodeblock %}
符号和变量在一起可以用来定义规则：
{% codeblock %}
friend(X,Y):- \+(X = Y),likes(X,Z),likes(Y,Z).
{% endcodeblock %}

{% pullquote %}
事实是我们对这个世界直接观察的结果。规则是关于现实世界的逻辑推论。
{% endpullquote %}

<b>事实 + 规则 = 知识库</b>

上面的规则可以叫做friend/2因为它有两个参数，:-读作“如果”，“如果”后面是由一系列“子目标”组成，子目标之间可以是且的关系，用“,”分割，也可以是或者的关系，用“.”表示。Prolog就是通过验证规则来回到我们yes或no的，如果参数能满足所有子目标就是yes。
{% codeblock %}
| ?- friend(Jack,Sam)
yes
{% endcodeblock %}
合一是Prolog中一个非常重要的概念。简单的来说合一就相当于其它语言中的赋值：
{% codeblock %}
cat(lion).
cat(tiger).

dorothy(X,Y,Z) :- X = lion,Y = tiger,Z = bear.
twin_cast(X,Y) :- cat(X),cat(Y).
{% endcodeblock %}
{% pullquote %}
合一的意思是：找出那些使规则匹配的值。
{% endpullquote %}
所以执行dorothy(lion,tiger,bear).这句，Prolog会返回yes:

dorothy/3规则的右侧，Prolog将lion赋值给X,tiger赋值给Y,bear赋值给Z，就像在命令式语言中这样
{% codeblock %}
var X = lion;
var Y = tiger;
var Z = bear;
{% endcodeblock %}
这些值和左侧（也就是dorothy(lion,tiger,bear)）对应的值相匹配，所以合一成功。

不过真正让合一发挥作用的是因为它在规则的两侧都能工作，在GNU Prolog中执行下面的语句：
{% codeblock %}
dorothy(One,Two,Three).
{% endcodeblock %}
会得到这样的结果：
{% codeblock %}
| ?- dorothy(One,Two,Three)
One = lion
Three = bear
Two = tiger
{% endcodeblock %}
在规则右侧Prolog还是分别将X,Y,Z和lion,tiger,bear进行绑定，而在规则的右侧，Prolog是的One,Two,Three分别和X,Y,Z进行合一了：
{% codeblock %}
One = X = lion;
Two = Y = tiger;
Three = Z = bear;
{% endcodeblock %}
并且合一的情况有时候不是唯一的，我们如下执行上面的twin_cast规则：
{% codeblock %}
| ?-twin_cast(One,Two).
One = lion
Two = lion?;

One = lion
Two = tiger?;

One = tiger
Two = lion?;

One = tiger
Two = tiger
{% endcodeblock %}

进阶写出能够应用在实际项目中的代码还需要了解更多奇技淫巧
对于递归的熟练应用是写好Prolog的关键之一
