#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？

7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？

9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？

13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象

虚拟页数量为M，访问的页序列为b(t)，t = 1,2,3...，b(t)表示在t时刻的访问页号，0< b(t) < M+1。在时刻t，物理页帧的集合为S(t)，其元素范围在[0..N]中，物理页帧的集合大小为N(N < M)。

如果b(t)属于S(t)，则表示命中，且物理页帧在t+1时刻的集合

S(t + 1) = S(t)

如果b(t)不属于S(t)，则表示缺页，这时有一个块要被换出，换出的页为v(t)。则物理页帧在t+1时刻的集合

S(t + 1) = (S(t) - {v(t)}) + {b(t)}

设S(1) = {1, 2, ..., N} S'(1) = {1, 2, ..., N} 即S(1) = S'(1)
 
证明LRU算法在物理页帧集合S'的大小为N+k（0 < k < M-N）时的缺页率比在物理页帧集合S大小为N的缺失率要低。

证明：使用数学归纳法，假设在时间t-1时，s(t－1)包含于s'(t－1)中。证明s(t)包含于s'(t)。考虑四种情况

1. b(t)属于s(t-1), b(t)属于s'(t-1)
  这种情况下s(t)和s‘(t)没有发生改变，包含关系依然存在。
2. b(t)不属于s(t-1), b(t)不属于s'(t-1)
   这种情况下两个集合都会发生替换，替换掉栈底元素，将b(t)插入栈顶。是否有s'(t)的栈底元素被替换，而s(t)中仍然有这个元素存在的情况？这种情况不会存在，设s(t-1)的栈底为a，而s'(t-1)的栈底为b，则b不会在s(t-1)中存在。可知a在s'(t-1)中也存在，那么在s'(t-1)中有b排在a的后面，则b的访问时间比a的访问时间长，而如果b在s(t-1)中存在，则不会是栈底，那么就有a的访问时间比b长，这样产生了一个矛盾。则这种情况不存在。
3. b(t)不属于s(t-1), b(t)属于s'(t-1)
   这种情况下s(t)会发生替换，替换掉栈底元素，将b(t)插入栈顶，因为本来就有b(t)属于s'(t-1)，这样也有两个集合的包含关系成立。
4. b(t)属于s(t-1), b(t)不属于s'(t-1)
  根据归纳假设，这种情况不存在，不予考虑。

综上所述，命题成立。则LRU不存在belady现象。

(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。


学号为2012011394，实现了工作集置换算法。
```
if __name__ == '__main__':
    address = raw_input()
    addrList = address.strip('\r\n').split(',')
    window = 4
    time = 0
    workSet = {}
    for item in addrList:
        n = int(item)
        time += 1
        print "Access %d at time %d:" % (n, time)
        if n in workSet:
            print "hit"
            workSet[n] = time
            for item in workSet.items():
                if time - item[1] >= window:
                    workSet.pop(item[0])
        else:
            print "miss"
            workSet[n] = time
        print "WorkSet Now:"
        for item in workSet.items():
            print item[0]
```

在工作集中记录一个内存的访问时间，存储二元组（n, time)
 - 当页访问命中时，遍历工作集中的每一个项，检查其访问时间和当前时间之差是否超过窗口，如果超过则移除。
 - 当页访问缺失时，直接将页加入到工作集中，此时可能导致工作集增大。


 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
 
## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
