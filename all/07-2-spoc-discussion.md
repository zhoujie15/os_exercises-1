# 同步互斥(lec 18) spoc 思考题


- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 基本理解
 - 什么是信号量？它与软件同步方法的区别在什么地方？
 - 什么是自旋锁？它为什么无法按先来先服务方式使用资源？
 - 下面是一种P操作的实现伪码。它能按FIFO顺序进行信号量申请吗？
```
 while (s.count == 0) {  //没有可用资源时，进入挂起状态；
        调用进程进入等待队列s.queue;
        阻塞调用进程;
}
s.count--;              //有可用资源，占用该资源； 
```

> 参考回答： 它的问题是，不能按FIFO进行信号量申请。
> 它的一种出错的情况
```
一个线程A调用P原语时，由于线程B正在使用该信号量而进入阻塞状态；注意，这时value的值为0。
线程B放弃信号量的使用，线程A被唤醒而进入就绪状态，但没有立即进入运行状态；注意，这里value为1。
在线程A处于就绪状态时，处理机正在执行线程C的代码；线程C这时也正好调用P原语访问同一个信号量，并得到使用权。注意，这时value又变回0。
线程A进入运行状态后，重新检查value的值，条件不成立，又一次进入阻塞状态。
至此，线程C比线程A后调用P原语，但线程C比线程A先得到信号量。
```

### 信号量使用

 - 什么是条件同步？如何使用信号量来实现条件同步？
 - 什么是生产者-消费者问题？
 - 为什么在生产者-消费者问题中先申请互斥信息量会导致死锁？

### 管程

 - 管程的组成包括哪几部分？入口队列和条件变量等待队列的作用是什么？
 - 为什么用管程实现的生产者-消费者问题中，可以在进入管程后才判断缓冲区的状态？
 - 请描述管程条件变量的两种释放处理方式的区别是什么？条件判断中while和if是如何影响释放处理中的顺序的？

### 哲学家就餐问题

 - 哲学家就餐问题的方案2和方案3的性能有什么区别？可以进一步提高效率吗？

### 读者-写者问题

 - 在读者-写者问题的读者优先和写者优先在行为上有什么不同？
 - 在读者-写者问题的读者优先实现中优先于读者到达的写者在什么地方等待？
 
## 小组思考题

1. （spoc） 每人用python threading机制用信号量和条件变量两种手段分别实现[47个同步问题](07-2-spoc-pv-problems.md)中的一题。向勇老师的班级从前往后，陈渝老师的班级从后往前。请先理解[]python threading 机制的介绍和实例](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab7/semaphore_condition)

我的问题为35。

桔子汁生产线问题 现有三个生产者P1 、P2 、P3，他们都要生产水，每个生产者都已分别购得两种不同原料，待购得第三种原料后就可配制成桔子水，装瓶出售。有一供应商能源源不断地供应糖、水、桔子精，但每次只拿出一种原料放入容器中供给生产者。当容器中有原料时需要该原料的生产者可取走，当容器空时供应商又可放入一种原料。假定：生产者P1已购得糖和水；生产者P2 已购得水和桔子精；生产者P3已购得糖和桔子精；试用：信号量与P、V操作，写出供应商和三个生产者之间能正确同步的程序。

使用信号量
```
import threading
import random
import time

critical = threading.Semaphore(1)
critical_product = -1

class Consumer(threading.Thread):
    def __init__(self, number):
        self.number = number
        self.ingredient = [1, 1, 1]
        self.ingredient[number] = 0
        threading.Thread.__init__(self)

    def run(self):
        #print "I am %s %d" % (self.name,self.number)
        # random sleep
        time.sleep(2)
        global critical
        global critical_product
        while True:
            critical.acquire()
            print "I am %s in critical!" % (self.name)
            if critical_product == self.number:
                print "My require ingredient is %d and I get %d now!" % (self.number, critical_product)
                critical_product = -1
                critical.release()
                break
            critical.release()
            time.sleep(random.randrange(1, 5))
        print "I am %s, I'm finished." % (self.name)

class Producer(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
        global critical
        global critical_product
        count = 0
        while (count < 3):
            critical.acquire()
            print "I am %s in critical!" % (self.name)
            if critical_product == -1:
                critical_product = count
                count += 1
            critical.release()
            time.sleep(random.randrange(1, 5))


if __name__ == '__main__':
    my = Producer()
    my.start()

    for i in range(0, 3):
        my = Consumer(i)
        my.start()
        time.sleep(1)
```
使用条件变量
```
import threading
import random
import time

condition = threading.Condition()
product = -1

class Consumer(threading.Thread):
    def __init__(self, number):
        self.number = number
        #self.ingredient = [1, 1, 1]
        #self.ingredient[number] = 0
        threading.Thread.__init__(self)

    def run(self):
        #print "I am %s %d" % (self.name,self.number)
        # random sleep
        while True:
            global condition
            global product
            if condition.acquire():
                if product == self.number:
                    print "My require ingredient is %d and I get %d now!" % (self.number, product)
                    product = -1
                    condition.notify()
                    condition.release()
                    break
                else:
                    condition.wait()
                condition.release()
        
        print "I am %s, I'm finished." % (self.name)

class Producer(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
        global condition
        global product
        count = 0
        while (count < 3):
            if condition.acquire():
                if product == -1:
                    product = count
                    print "I an producing ingredient %d now!" % (product)
                    count += 1
                    condition.notify()
                else:
                    condition.wait()
                condition.release()


if __name__ == '__main__':
    my = Producer()
    my.start()

    for i in range(0, 3):
        my = Consumer(i)
        my.start()
        time.sleep(1)
```
