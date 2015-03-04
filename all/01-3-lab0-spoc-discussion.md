# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- [x]  

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm
有部分语言不理解；inb $0x64, %al；lgdt gdtdesc；

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- [x]  

>   对于其中的io，中断以及异常处理部分不够了解。


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- [x]  

> 1、对ucore中涉及的硬件设计或功能细节不了解；2、对汇编语言理解不够；3、实验源代码理解难度大。

如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- [x]  

>   段机制启动、页机制未启动：逻辑地址->线性地址=物理地址；段机制和页机制启动：逻辑地址->段机制处理->线性地址->物理地址。

了解函数调用栈对lab实验有何帮助？
- [x]  

>   了解函数调用栈可以知道操作系统对于函数的调用分配的具体操作顺序，可以从细节上了解计算机函数调用机制。

你希望从lab中学到什么知识？
- [x]  

>   希望学习到操作系统的设计要点以及对于一个操作系统的设计需要注意的问题和要求，还希望通过lab学习到对于操作系统内核代码的编写技巧以及其与一般软件代码编写的区别。

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- [x]  

> 按照教程搭建基本没有什么问题。

熟悉基本的git命令行操作命令，从github上的[ucore git repo](http://www.github.com/chyyuu/ucore_lab)下载ucore lab实验
- [x]  

> 已经下载在本地。

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- [x]  

> 已尝试调试。

对于如下的代码段，请说明”：“后面的数字是什么含义
```
/* Gate descriptors for interrupts and traps */
struct gatedesc {
    unsigned gd_off_15_0 : 16;        // low 16 bits of offset in segment
    unsigned gd_ss : 16;            // segment selector
    unsigned gd_args : 5;            // # args, 0 for interrupt/trap gates
    unsigned gd_rsv1 : 3;            // reserved(should be zero I guess)
    unsigned gd_type : 4;            // type(STS_{TG,IG32,TG32})
    unsigned gd_s : 1;                // must be 0 (system)
    unsigned gd_dpl : 2;            // descriptor(meaning new) privilege level
    unsigned gd_p : 1;                // Present
    unsigned gd_off_31_16 : 16;        // high bits of offset in segment
};
```
- [x]  

> 后面的数字表示对应的位数。

对于如下的代码段，
```
#define SETGATE(gate, istrap, sel, off, dpl) {            \
    (gate).gd_off_15_0 = (uint32_t)(off) & 0xffff;        \
    (gate).gd_ss = (sel);                                \
    (gate).gd_args = 0;                                    \
    (gate).gd_rsv1 = 0;                                    \
    (gate).gd_type = (istrap) ? STS_TG32 : STS_IG32;    \
    (gate).gd_s = 0;                                    \
    (gate).gd_dpl = (dpl);                                \
    (gate).gd_p = 1;                                    \
    (gate).gd_off_31_16 = (uint32_t)(off) >> 16;        \
}
```
如果在其他代码段中有如下语句，
```
unsigned intr;
intr=8;
SETGATE(intr, 0,1,2,3);
```
请问执行上述指令后， intr的值是多少？
- [x]  

> 43211

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
- [x]  

> 大致含义就是对与自己定义的一个list_entry进行的一系列操作，包括初始化(init)，添加(add)，在特定元素前面添加(add_before)，在特定元素后面添加(add_after)，删除(del)，删除并初始化(del_init)，清空(empty)，链表下一个(next)，前缀(prev)等。

---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  

---
