# 操作系统概述思考题

## 个人思考题

---

分析你所认识的操作系统（Windows、Linux、FreeBSD、Android、iOS）所具有的独特和共性的功能？
- Windows提供了出色的图形界面和丰富的应用，让用户非常容易上手使用，同时平台上也有很多大型的应用，满足用户的多种需求，在软件数量和质量上占据优势。
- Linux具有很好的开放性，能够适应多种不同场合的需要，同时linux开放源代码，对于编程人员和研究人员提供很好的环境支持，并且体积轻巧，运行速度快。
- FreeBSD系统纯净，性能高，和linux相比具有更好的安全性，适用于在服务器部署。
- Android是开放免费的移动端系统，应用数量较多，系统可以根据不同的需要进行定制，具有很好的兼容性。
- iOS是封闭的移动端系统，运行效率高，应用的质量高，系统安全性较好。
- 共性：都提供了硬件的管理功能，为上层应用提供接口，控制程序的运行，进行资源的分配。都提供了一定的用户界面或者命令行。实现对于多种不同硬件的管理和访问（如网卡、内存等等）。

>  

请总结你认为操作系统应该具有的特征有什么？并对其特征进行简要阐述。
- 并发。并发是指在操作系统中会有多个应用程序同时运行，操作系统需要对其进行管理和调度。
- 共享。共享是指多个应用在宏观上可能会表现出“同时”使用资源，但是在微观上需要实现它们的互斥访问。
- 虚拟。利用多道程序技术，对于多用户使用的计算机，让用户感觉到有一台专门的计算机在对其提供服务。
- 异步。异步是指实际上程序是间断执行的，只要用户的输入不变，运行环境不变，那么程序的结果要应该保持一致。

>   

请给出你觉得的更准确的操作系统的定义？
- 操作系统是一种对计算机的硬件资源进行管理，为用户提供接口和服务的软件。它是控制其他程序运行,管理系统资源并为用户提供操作界面的系统软件的集合。

>   

你希望从操作系统课学到什么知识？
- 操作系统的一些基本概念，包括其历史、特征、结构、演变等等。
- 操作系统的工作原理和运行机制，深入了解操作系统背后所蕴含的知识和道理。
- 现有操作系统的优缺点，以及现在比较前沿的一些研究方向。
- 了解操作系统的编写方法和编程细节，提高自己的代码能力。

>   

---

## 小组讨论题

---

目前的台式PC机标准配置和价格？

- 目前PC机子的标配: 
- CPU：Intel Core i5-Quad Core； 
- 内存：4GB； 
- 处理器速度：3.20GHz-Turbo to 3.6； 
- 显示器：20"LED Flat Panel 1600*900； 
- 显卡：Intel HD 4600 Dual Monitor Capable； 
- 硬盘：500GB ； 
- 价格：$749 ￥4698 

你理解的命令行接口和GUI接口具有哪些共性和不同的特征？

- GUI和Shell(CLI)比较 CLI比较难上手，对用户难度较大，GUI由于用图形化接口，比较直观，好上手；
- CLI对文件和操作系统控制性更强，比如从一个地方把文件拷贝到另一个地方只需要一行，GUI提供大量的接口，但对比较高级的操作，不如CLI; 
- CLI只需要使用键盘，GUI需要使用鼠标+键盘，一般情况下，CLI比GUI更快；
- CLI占用较少的系统资源，GUI占用较多的系统资源，因为需要加载很多如按钮之类的其他东西；
- CLI可以很简单的用指令直接运行程序或者开始进程，GUI相比较CLI可获取的资源少一些；
- CLI可以比较容易做到远程操作，部分GUI不支持远程操作;
- CLI比较单一，简洁，GUI种类繁多，比较复杂；
- CLI可以让手一直停留在键盘完成所有操作，GUI即使有快捷键也不可避免用到鼠标;
- 共同点都是为用户提供接口，使用计算机系统资源 

为什么现在的操作系统基本上用C语言来实现？

>  因为目前所有语言的开发环境里，C语言能做到编译成不依赖操作系统的形式二进制代码，C语言的各种脱离系统的库最丰富，最完整，C语言用来开发操作系统的工具最多。 语言运行所依赖的东西决定了C语言在现今是比较适合写操作系统的语言。

为什么没有人用python，java来实现操作系统？

>  操作系统有一个很重要的功能是内存管理，Java,Python之类的不能直接操纵内存的语言要做内存管理是很困难的事情。不过有人确实用Java写过操作系统（JNode），链接： http://zh.wikipedia.org/wiki/JNode 。 不过他的内存管理部分使用的是汇编写的，相当一部分代码需要使用L2编译器编译，编译出来的是X86原生代码，不是JVM的bytecode。 

请评价用C++来实现操作系统的利弊？

>  可以把操作系统看做一个多层结构，C++适合比较高层的编写，不太适合比较底层的编写。C++需要运行时刻库来实现很多功能（比如异常处理），比较底层的时候可能无法用到运行时刻库，C在这时就显得比较合适。 不过比较高层的服务，比较抽象的系统服务，运行时刻库可以获取到的情况下，C++这类高级语言就更加合适，Windows中就有很多部分用C++编写。

---

## 开放思考题

---

请评价微内核、单体内核、外核（exo-kernel）架构的操作系统的利弊？

- 微内核：模块化的方式设计操作系统，模块的设计者只需要关注自己的功能模块。与分层结构相比能够提高效率，安全性和可扩展性有很大提高，但是用户模块间的通讯性能会下降。
- 单体内核：简单和高性能。
- 外核架构：主要应用在科研系统中，实现了操作系统抽象，达到了保护与控制分离。

请评价用LISP,OCcaml, GO, D，RUST等实现操作系统的利弊？

- [x]

>  

进程切换的可能实现思路？


>  可以采用中断技术来实现，操作系统对当前正在运行的程序应用软中断来中止运行，保存好现场，构建新程序的运行环境，然后从中断返回，运行将要运行的程序。

计算机与终端间通过串口通信的可能实现思路？

- [x]

>  

为什么微软的Windows没有在手机终端领域取得领先地位？

>  微软的windows phone起步较晚，没有占据用户市场，这就导致平台上的应用和其他平台相比较少，UI的样式比较小众，在用户体验方面和其他平台相比不够好，而且开放性也不够好。

你认为未来（10年内）的操作系统应该具有什么样的特征和功能？

>  应该支持分布式资源的利用和管理，提高虚拟化程度，根据用户的需要在云端申请适当的硬件资源，在用户不需要时释放，提高资源的利用率。

---
