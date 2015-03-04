# lab0 SPOC思考题

## 个人思考题

---

能否读懂ucore中的AT&T格式的X86-32汇编语言？请列出你不理解的汇编语言。
- 基本能够读懂，但是需要了解AT&T格式和Intel格式的主要区别。

>  http://www.imada.sdu.dk/Courses/DM18/Litteratur/IntelnATT.htm

虽然学过计算机原理和x86汇编（根据THU-CS的课程设置），但对ucore中涉及的哪些硬件设计或功能细节不够了解？
- 实机制是如何转换为保护机制的。
- 页机制和段机制是如何完成不同地址间的转换的
- 硬件中寄存器的具体用法，以及实验中需要用到哪些寄存器

>   


哪些困难（请分优先级）会阻碍你自主完成lab实验？
- 优先级从高到低：
-     实验环境和配置有问题，无法找到解决方案
-     对实验内容、实验相关原理不理解


如何把一个在gdb中或执行过程中出现的物理/线性地址与你写的代码源码位置对应起来？
- [x]  

>   

了解函数调用栈对lab实验有何帮助？
- 有利于更好地理解操作系统的实现机制，更容易理解内存分配和进程控制中的问题。

>   

你希望从lab中学到什么知识？

>   学习到操作系统的具体实现，实现中的一些细节和编程技巧，掌握编写大型软件的能力。了解操作系统原理的实际应用。

---

## 小组讨论题

---

搭建好实验环境，请描述碰到的困难和解决的过程。
- 我采用了直接在Linux系统中配置环境的过程。
- 我在系统中安装了gdb make diff meld git qemu eclipsecdt

>  在安装过程中，使用sudo apt-get install diff失败，采用提示的方法安装了diffutils后解决了问题。

熟悉基本的git命令行操作命令，从github上的[ucore git repo](http://www.github.com/chyyuu/ucore_lab)下载ucore lab实验
- [x]  

> http://www.cnblogs.com/cspku/articles/Git_cmds.html

尝试用qemu+gdb（or ECLIPSE-CDT）调试lab1
- 使用了make debug命令进行了qemu + gdb调试。
- 直接使用eclipsecdt也可以进行调试。

> 

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

> “：”后面的数字是定义位域的宽度。表示这个变量占的bit位数。比如unsigned gd_p : 1;  代表变gd_p占1个bit。

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
- 261683767476226  

> 

请分析 [list.h](https://github.com/chyyuu/ucore_lab/blob/master/labcodes/lab2/libs/list.h)内容中大致的含义，并能include这个文件，利用其结构和功能编写一个数据结构链表操作的小C程序
- list.h中实现了双向链表这样的数据结构，实现了添加、删除、初始化等操作。

> 
```
#include "list.h"
#include <cstdio>
int main() {
    list_entry *a = new list_entry();
    list_entry *b = new list_entry();
    list_entry *c = new list_entry();
    list_init(a);
    list_add(a, b);
    list_add_before(a, c);
    list_del(c);
    list_del_init(b);
    return 0;
}
```
---

## 开放思考题

---

是否愿意挑战大实验（大实验内容来源于你的想法或老师列好的题目，需要与老师协商确定，需完成基本lab，但可不参加闭卷考试），如果有，可直接给老师email或课后面谈。
- [x]  

>  

---
