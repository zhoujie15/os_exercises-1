#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 UEFT不同于BIOS的是：UEFT是统一的可扩展固件接口，它是用模块化、C语言风格的参数堆栈传递方式、动态链接的形式构建系统；它比BIOS更加快速安全，更加容易实现，容错和纠错能力较强，运行与32位或者64位，而BIOS是16位，这样就能够提高启动速度。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 当主引导记录被装入内存后，程序开始运行，活动分区和引导扇区被装入内存，此时，NTLDR从引导扇区被装入并初始化，NTLDR开始运行适当的小文件系统驱动程序，然后NTLDR读boot.ini文件，然后NTLDR开始装载系统。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 NTLDR是一个系统启动时需要调用的文件，而GRUB是一个多操作系统启动程序，NTLDR功能是系统启动时装载系统功能的一个只读文件，GURB是一个多系统管理程序，用于当计算机有多个系统时允许用户选择使用的系统。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？
 可屏蔽中断：I/O设备发出的所有中断请求；
 非屏蔽中断：硬件故障或者断电；
 处理器探测异常：溢出，除0错误；
 编程异常：有int类指令出发的异常。
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (2wl1)
 1. 以ucore lab8为例，uCore的系统调用有哪些？大致的功能分类有哪些？(2wl1)
 
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(2wl1)

 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(2wl1)
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
