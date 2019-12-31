---
layout: post
title:  "all about simple design of kent beck, and beyond"
---

# Kent Beck's simple design rule
https://martinfowler.com/bliki/BeckDesignRules.html

# 许式伟的架构课

Below is high quality comments regarding programming paradigm.

```
对象范式的原始概念其实根本不包括类和继承，只有1，程序由对象组成，2，对象之间互相发送消息，协作完成任务。最初世界上第一个面向对象的语言是Simula-67,第二个面向对象的语言是Smalltalk-71。Smalltalk受到了Simula-67的启发，基本出发点相同，但是最大的不同是Smalltalk是通过发消息来实现对象方法调用，而Simula是直接调用目标对象的方法。Bjarne Stroustrup在博士期间深入研究过Simula，非常欣赏其思想，C++的面向对象思路直接受其影响，因为调用目标对象的方法来“传递消息”需要事先知道这个对象有哪些方法，因此，定义对象本身有哪些方法的“类”和“继承“的概念，一下超越了对象本身，而对象不过是类这个模子里造出来的东西，反而不重要。随着C++大行其道，继承和封装变成了面向对象世界的核心概念，OOP至此被扭曲为COP（Class Oriented Programming)。

但是COP这套概念本身是有缺陷的：每个程序员似乎都要先成为领域专家，然后成为领域分类学专家，然后构造一个完整的继承树，然后才能new出对象，让程序跑起来。

到了1990年代中期，问题已经十分明显。UML有个一个对象活动图，其描述的就是运行时对象之间相互传递消息的模型。1994年Robert C.Martin 在《Object-Oriented C++ Desing Using Booch Method》中，曾建议面向对象设计从对象活动图入手，而不是从类图入手。而1995年出版的经典作品《Design Pattern》中，建议优先考虑组合而不是继承，这也是人尽皆知的事情。这些迹象表明，在那个时候，面向对象社区里的思想领袖们，已经意识到“面向类的设计”并不好用。只可惜他们的革命精神还不够，delphi之父在创建.Net Framework的时候，曾经不想要继承，在微软内部引起了很大争议，最后是向市场低头，加上了继承。

2000年后，工程界明确的提出：“组合比继承重要，而且更灵活”，Go和Rust也许是第一批明确的对这种思路进行回应的语言，他们的对象根本不需要类本身来参与，也能完成对象范式的多态组合。历史让C++走上了舞台，历史也终将让COP重新回到了OOP的本来面目。

```

