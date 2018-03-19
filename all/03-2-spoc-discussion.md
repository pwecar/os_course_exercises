# lec6 SPOC思考题


NOTICE
- 有"w3l2"标记的题是助教要提交到学堂在线上的。
- 有"w3l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。

## 与视频相关思考题

### 6.1	非连续内存分配的需求背景
 1. 为什么要设计非连续内存分配机制？

 > 提高分配的灵活性

 > 提高内存的利用效率：方便共享、充分利用内存空间
 
 1. 非连续内存分配中内存分块大小有哪些可能的选择？大小与大小是否可变?

 > 大块好管理，小块更灵活

 1. 为什么在大块时要设计大小可变，而在小块时要设计成固定大小？小块时的固定大小可以提供多种选择吗？

 > 固定大小好管理，多种大小比一种大小灵活

 > 可变大小更灵活，通常可变大小也会通过对齐来减少管理难度
 
### 6.2	段式存储管理
 1. 什么是段、段基址和段内偏移？

 > 段表示访问方式和存储数据等属性相同的一段地址空间
 
 1. 段式存储管理机制的地址转换流程是什么？为什么在段式存储管理中，各段的存储位置可以不连续？这种做法有什么好处和麻烦？
 
 > 段式存储管理中，地址转换是段基址（段号）加段内偏移
 
 > 段反映了程序的存储逻辑结构，程序不会从一个段的基址去访问另一个段，于是不同的段可以不连续。
 
 > 好处是可以不连续，方便内存管理；麻烦是地址转换稍微复杂了一些。

### 6.3	页式存储管理
 1. 什么是页（page）、帧（frame）、页表（page table）、存储管理单元（MMU）、快表（TLB, Translation Lookaside Buffer）和高速缓存（cache）？
 1. 页式存储管理机制的地址转换流程是什么？为什么在页式存储管理中，各页的存储位置可以不连续？这种做法有什么好处和麻烦？

 > 页式存储管理中，地址转换是页号加页内偏移
 
 > CPU使用连续的逻辑地址，存储访问时，逻辑地址先分成逻辑页号和页内偏移，然后通过页表定义的对应关系，把逻辑页面转换成物理页号，最后再把物理页号加页内偏移得到物理地址；于是不同的段可以不连续。
 
 > 好处是可以不连续，方便内存管理中的存储分配和回收；麻烦是地址转换比较复杂（页表项访问开销和页表存储开销），并且频繁进行（每次存储访问会变成两次或更多）。

### 6.4	页表概述
 1. 每个页表项有些什么内容？有哪些标志位？它们起什么作用？
 1. 页表大小受哪些因素影响？

 >  页大小、地址空间大小、进程数目

### 6.5	快表和多级页表
 1. 快表（TLB）与高速缓存（cache）有什么不同？
 TLB是内存中页表的cache。
 1. 为什么快表中查找物理地址的速度非常快？它是如何实现的？为什么它的的容量很小？
 TLB中保存着虚拟地址(前20位)和页框号的对映关系,如果匹配到虚拟地址就可以迅速找到页框号(页框号可以理解为页表项),通过页框号与虚拟地址后12位的偏移组合得到最终的物理地址.
 1. 什么是多级页表？多级页表中的地址转换流程是什么？多级页表有什么好处和麻烦？

### 6.6	反置页表
 1. 页寄存器机制的地址转换流程是什么？
 
 > 页寄存器的内容
 
 > 逻辑地址进行hash，然后查相应页寄存器

 1. 反置页表机制的地址转换流程是什么？

 > 反置页表项的内容
 
 > 逻辑地址和进程号共同进行hash，然后查相应页寄存器
 
 1. 反置页表项有些什么内容？

 >  PID、逻辑页号、标志位

### 6.7	段页式存储管理
 1. 段页式存储管理机制的地址转换流程是什么？这种做法有什么好处和麻烦？
 1. 如何实现基于段式存储管理的内存共享？
 1. 如何实现基于页式存储管理的内存共享？

## 个人思考题
---

（1） (w3l2) 请简要分析64bit CPU体系结构下的分页机制是如何实现的
```
  + 采分点：说明64bit CPU架构的分页机制的大致特点和页表执行过程
  - 答案没有涉及如下3点；（0分）
  - 正确描述了64bit CPU支持的物理内存大小限制（1分）
  - 正确描述了64bit CPU下的多级页表的级数和多级页表的结构或反置页表的结构（2分）
  - 除上述两点外，进一步描述了在多级页表或反置页表下的虚拟地址-->物理地址的映射过程（3分）
 ```
- [x]  

>  

## 小组思考题
---

（1）(spoc) 某系统使用请求分页存储管理，若页在内存中，满足一个内存请求需要150ns (10^-9s)。若缺页率是10%，为使有效访问时间达到0.5us(10^-6s),求不在内存的页面的平均访问时间。请给出计算步骤。 

- [x]  

> 500=0.9\*150+0.1\*x

（2）(spoc) 有一台假想的计算机，页大小（page size）为32 Bytes，支持32KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries
PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

PTE格式（8 bit） :
```
  VALID | PFN6 ... PFN0
```
PDE格式（8 bit） :
```
  VALID | PT6 ... PT0
```
其
```
VALID==1表示，表示映射存在；VALID==0表示，表示映射不存在。
PFN6..0:页帧号
PT6..0:页表的物理基址>>5
```
在[物理内存模拟数据文件](./03-2-spoc-testdata.md)中，给出了4KB物理内存空间的值，请回答下列虚地址是否有合法对应的物理内存，请给出对应的pde index, pde contents, pte index, pte contents。
```
1) Virtual Address 6c74
   Virtual Address 6b22
2) Virtual Address 03df
   Virtual Address 69dc
3) Virtual Address 317a
   Virtual Address 4546
4) Virtual Address 2c03
   Virtual Address 7fd7
5) Virtual Address 390e
   Virtual Address 748b
```

比如答案可以如下表示： (注意：下面的结果是错的，你需要关注的是如何表示)
```
Virtual Address 7570:
  --> pde index:0x1d  pde contents:(valid 1, pfn 0x33)
    --> pte index:0xb  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)
      
Virtual Address 21e1:
  --> pde index:0x8  pde contents:(valid 0, pfn 0x7f)
      --> Fault (page directory entry not valid)

Virtual Address 7268:
  --> pde index:0x1c  pde contents:(valid 1, pfn 0x5e)
    --> pte index:0x13  pte contents:(valid 1, pfn 0x65)
      --> Translates to Physical Address 0xca8 --> Value: 16
```

[链接](https://piazza.com/class/i5j09fnsl7k5x0?cid=664)有上面链接的参考答案。请比较你的结果与参考答案是否一致。如果不一致，请说明原因。

（3）请基于你对原理课二级页表的理解，并参考Lab2建页表的过程，设计一个应用程序（可基于python、ruby、C、C++、LISP、JavaScript等）可模拟实现(2)题中描述的抽象OS，可正确完成二级页表转换。

[链接](https://piazza.com/class/i5j09fnsl7k5x0?cid=664)有上面链接的参考答案。请比较你的结果与参考答案是否一致。如果不一致，提交你的实现，并说明区别。

（4）假设你有一台支持[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)的机器，请问你如何设计操作系统支持这种类型计算机？请给出设计方案。

 (5)[X86的页面结构](http://os.cs.tsinghua.edu.cn/oscourse/OS2015/lecture06#head-1f58ea81c046bd27b196ea2c366d0a2063b304ab)
--- 

## 扩展思考题

阅读64bit IBM Powerpc CPU架构是如何实现[反置页表](http://en.wikipedia.org/wiki/Page_table#Inverted_page_table)，给出分析报告。

--- 


## interactive　understand VM

- [Virtual Memory with 256 Bytes of RAM](http://blog.robertelder.org/virtual-memory-with-256-bytes-of-ram/)

