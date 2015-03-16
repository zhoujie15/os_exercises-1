# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- [x]  

>  

## 小组思考题

请参考ucore lab2代码，采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)来实现，给出你的设思路，并给出测试用例。 (spoc)

我的学号2012011394，mod 4值为2，所以我实现的算法是最先匹配。

我使用python进行编写，用数字模拟内存分配，维护两个列表，一个是emptyList，记录了空闲空间的起始地址和大小，另一个是runList，记录了正在运行的进程占用的空间大小，占用空间的起始地址和进程号。

主要算法通过最先匹配进行分配。空闲内存列表按照地址排序，每次分配时扫描数组，找到第一个能够分配的空间后进行分配，把进程加入到运行列表中，如果分配后空间还有剩余，那么把剩余的空间保留。释放某个进程时，在列表中找到空间的位置，然后分别检查它之前和之后的内存块，如果能够合并的话进行合并。

算法使用了python中list数据结构进行模拟，实现比较简单，对于内存的操作并没有真正的进行，而是仅仅使用数字进行了模拟，主要目的是体会内存分配算法，这是本程序的局限之处。

```
#coding:utf-8

class pmm_manager:
    emptyList = []      # 空闲空间表 元素为list ［起始地址，大小］
    runList = []        # 运行进程表 元素为list ［起始地址，占用内存大小，进程号］
    
    def init(self, size):   # 初始化操作
        self.emptyList = []
        self.runList = []
        self.emptyList.append([0, size])    # 根据输入参数设置初始空间

    def allocate(self, size, id):   # 最先匹配算法，输入进程需要的空间大小size和进程id
        for item in self.emptyList:
            if item[1] >= size:     # 找到第一个满足条件的空间
                self.runList.append([item[0], size, id])    # 加入运行进程表
                print "Allocate process " + str(id) + " on start memory address " + str(item[0])
                if (item[1] > size):
                    self.emptyList.append([item[0] + size, item[1] - size]) # 如果该空间还有剩余
                self.emptyList = sorted(self.emptyList, key = lambda x:x[0]) # 根据起始地址排序
                self.emptyList.remove(item)
            break
    
    def release(self, size, id):        # 释放内存方法 输入进程占用的内存大小size和进程id
        for item in self.runList:
            if (item[2] == id and item[1] == size):     # 找到相应的进程
                print "Release process " + str(id) + " on start memory address " + str(item[0])
                self.runList.remove(item)   # 在进程表中删除
                blockAddr = item[0]
                blockSize = item[1]
                for i in range(0, len(self.emptyList)):
                    if self.emptyList[i][0] > item[0]:
                        if item[0] + item[1] == self.emptyList[i][0]:   # 如果和后一段空间相连，合并
                            blockSize += self.emptyList[i][1]
                            blockAddr = item[0]
                            del(self.emptyList[i])
                        if  i - 1 >= 0 and self.emptyList[i-1][0] + self.emptyList[i-1][1] == item[0]:  # 如果和前一段空间相连，合并
                            blockSize += self.emptyList[i-1][1]
                            blockAddr = self.emptyList[i-1][0]
                            del(self.emptyList[i - 1])
                        self.emptyList.append([blockAddr, blockSize])
                        self.emptyList = sorted(self.emptyList, key = lambda x:x[0])
                        break
                break
```

测试样例如下：

```
    pmm = pmm_manager()
    pmm.init(4096)          # 初始化过程

    pmm.allocate(512, 0)    # 自己编写的测试样例
    pmm.allocate(128, 1)
    pmm.allocate(128, 2)
    pmm.allocate(64, 3)
    print pmm.emptyList
    print pmm.runList
    pmm.release(512, 0)
    print pmm.emptyList
    print pmm.runList
    pmm.allocate(128, 4)
    print pmm.emptyList
    print pmm.runList
    pmm.release(128, 1)
    pmm.release(64, 3)
    pmm.release(128,2)
    pmm.release(128, 4)
    print pmm.emptyList
    print pmm.runList
```

测试样例中考虑了对于多个地址块的选择情况， 以及释放内存之后的进行合并的情况。经过测试，程序实现正确。


--- 

## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。

## “连续内存分配”与视频相关的课堂练习

### 5.1 计算机体系结构和内存层次
MMU的工作机理？

- [x]  

>  http://en.wikipedia.org/wiki/Memory_management_unit

L1和L2高速缓存有什么区别？

- [x]  

>  http://superuser.com/questions/196143/where-exactly-l1-l2-and-l3-caches-located-in-computer
>  Where exactly L1, L2 and L3 Caches located in computer?

>  http://en.wikipedia.org/wiki/CPU_cache
>  CPU cache

### 5.2 地址空间和地址生成
编译、链接和加载的过程了解？

- [x]  

>  

动态链接如何使用？

- [x]  

>  


### 5.3 连续内存分配
什么是内碎片、外碎片？

- [x]  

>  

为什么最先匹配会越用越慢？

- [x]  

>  

为什么最差匹配会的外碎片少？

- [x]  

>  

在几种算法中分区释放后的合并处理如何做？

- [x]  

>  

### 5.4 碎片整理
一个处于等待状态的进程被对换到外存（对换等待状态）后，等待事件出现了。操作系统需要如何响应？

- [x]  

>  

### 5.5 伙伴系统
伙伴系统的空闲块如何组织？

- [x]  

>  

伙伴系统的内存分配流程？

- [x]  

>  

伙伴系统的内存回收流程？

- [x]  

>  

struct list_entry是如何把数据元素组织成链表的？

- [x]  

>  


