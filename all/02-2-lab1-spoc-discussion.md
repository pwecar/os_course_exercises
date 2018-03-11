# lab1 SPOC思考题

##**提前准备**
（请在上课前完成）

 - 完成lec4的视频学习和提交对应的在线练习
 - git pull ucore_os_lab, v9_cpu, os_course_spoc_exercises in github repos。这样可以在本机上完成课堂练习。
 - 了解x86的保护模式，段选择子，全局描述符，全局描述符表，中断描述符表等概念，以及如何读写，设置等操作
 - 了解Linux中的ELF执行文件格式
 - 了解外设:串口，并口，时钟，键盘,CGA，已经如何对这些外设进行编程
 - 了解x86架构中的mem地址空间和io地址空间
 - 了解x86的中断处理过程（包括硬件部分和软件部分）
 - 了解GCC内联汇编
 - 了解C语言的可函数变参数编程
 - 了解qemu的启动参数的含义
 - 在piazza上就lec3学习中不理解问题进行提问
 - 学会使用 qemu
 - 在linux系统中，看看 /proc/cpuinfo的内容

## 个人思考题

NOTICE
- 有"w2l2"标记的题是助教要提交到学堂在线上的。
- 有"w2l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。
---

- 请描述ucore OS配置和驱动外设时钟的准备工作包括哪些步骤？ (w2l2)

ANSWER:

```
  - 对IDT的初始化，包了针时钟中断的中断描述符的设置
  - 对8259中断控制器进行初始化
  - 对8253时钟外设的初始化
  - 对EFLAG操作使能中断
 ```

- lab1中完成了对哪些外设的访问？ (w2l2)

ANSWER:

```
- 时钟,串口,并口，CGA，键盘
```

- lab1中的cprintf函数最终通过哪些外设完成了对字符串的输出？

ANSWER:

```
- 串口 并口 CGA
```

---

## 小组思考题

---

- lab1中printfmt函数用到了可变参，请参考写一个小的linux应用程序，完成实现定义和调用一个可变参数的函数。(spoc)
 
  - C可变参数函数实现 http://blog.csdn.net/weiwangchao_/article/details/4857567 
  - va_list & va_start & va_arg & va_end http://blog.csdn.net/skymingst/article/details/36690081

```
#include <stdio.h>
#include <stdarg.h>
void simple_va_fun(int start, ...)
{
       va_list arg_ptr;
       int nArgValue =start;
       int nArgCout=0;     //可变参数的数目
       va_start(arg_ptr,start); //以固定参数的地址为起点确定变参的内存起始地址。
       do {
          ++nArgCout;
          printf("the %d th arg: %d\n",nArgCout,nArgValue);//输出各参数的值
          nArgValue = va_arg(arg_ptr,int);                 //得到下一个可变参数的值
       } while(nArgValue != -1);
       va_end(arg_ptr);     
       return;
}

int main(int argc, char* argv[])
{
       simple_va_fun(100,-1);
       simple_va_fun(100,200,-1);
       return 0;
}
```



- 如果让你来一个阶段一个阶段地从零开始完整实现lab1（不是现在的填空考方式），你的实现步骤是什么？（比如先实现一个可显示字符串的bootloader（描述一下要实现的关键步骤和需要注意的事项），再实现一个可加载ELF格式文件的bootloader（再描述一下进一步要实现的关键步骤和需要注意的事项）...） (spoc)


- 如何能获取一个系统调用的调用次数信息？如何可以获取所有系统调用的调用次数信息？请简要说明可能的思路。(spoc)

- 如何修改lab1, 实现一个可显示字符串"THU LAB1"且依然能够正确加载ucore OS的bootloader？如果不能完成实现，请说明理由。


- 对于ucore_lab中的labcodes/lab1，我们知道如果在qemu中执行，可能会出现各种稀奇古怪的问题，比如reboot，死机，黑屏等等。请通过qemu的分析功能来动态分析并回答lab1是如何执行并最终为什么会出现这种情况？

- 对于ucore_lab中的labcodes/lab1,如果出现了reboot，死机，黑屏等现象，请思考设计有效的调试方法来分析常在现背后的原因。


- 在X86中设置了4种特权级，ring0~ring3，用户态是ring3，内核态是ring0，对于这两种状态来说，切换是非常常见的。但是既然设置了ring1和ring2，那么肯定是可以切换到的。请给出一个例子来验证这个问题，包括4个特权级之间的切换，并显示。[challenge]

---

## 开放思考题

---

- 如何修改lab1, 实现在出现除零错误异常时显示一个字符串的异常服务例程的lab1？



- 在lab1/bin目录下，通过`objcopy -O binary kernel kernel.bin`可以把elf格式的ucore kernel转变成体积更小巧的binary格式的ucore kernel。为此，需要如何修改lab1的bootloader, 能够实现正确加载binary格式的ucore OS？ (hard)
 

- GRUB是一个通用的bootloader，被用于加载多种操作系统。如果放弃lab1的bootloader，采用GRUB来加载ucore OS，请问需要如何修改lab1, 能够实现此需求？ (hard)


- 如果没有中断，操作系统设计会有哪些问题或困难？在这种情况下，能否完成对外设驱动和对进程的切换等操作系统核心功能？

---

## 各知识点的小练习

### 4.1 启动顺序

- 段寄存器的字段含义和功能有哪些？

代码段寄存器CS存放当前正在运行的程序代码所在段的段基址，表示当前使用的指令代码可以从该段寄存器指定的存储器段中取得，相应的偏移量则由IP提供。
数据段寄存器DS指出当前程序使用的数据所存放段的最低地址，即存放数据段的段基址。
堆栈段寄存器SS指出当前堆栈的底部地址，即存放堆栈段的段基址。
附加段寄存器ES指出当前程序使用附加数据段的段基址，该段是串操作指令中目的串所在的段。

- 描述符特权级DPL、当前特权级CPL和请求特权级RPL的含义是什么？在哪些寄存器中这些字段？对应的访问条件是什么？

CPL是当前正在执行的代码所在的段的特权级，存在于cs寄存器的低两位。
RPL是进程对段访问的请求权限，是对于段选择子而言的，每个段选择子有自己的RPL，它说明的是进程对段访问的请求权限，不同访问的RPL可以不同。
RPL可能会削弱CPL的作用，例如当前CPL=0的进程要访问一个数据段，它把段选择符中的RPL设为3，这样虽然它对该段仍然只有特权为3的访问权限。
DPL存储在段描述符中，规定访问该段的权限级别，每个段的DPL固定。
当进程访问一个段时，需要进行特权级检查，一般要求DPL>=max{CPL, RPL}

- 分析可执行文件格式elf的格式。

---

### 4.2 C函数调用的实现

### 4.4 x86中断处理过程
---

4.4 x86中断处理过程


- 中断处理中硬件压栈内容？用户态中断和内核态中断的硬件压栈有什么不同？


- 为什么在用户态的中断响应要使用内核堆栈？
  
  为了要保护中断服务例程代码的安全。


- trap类型的中断门与interrupt类型的中断门有啥设置上的差别？如果在设置中断门上不做区分，会有什么可能的后果?

---

### 4.5 练习一 ucore编译过程
---

- gcc编译、ld链接和dd生成两个映像对应的makefile脚本行？


- 函数print_stackframe中要调用函数print_debuginfo(uintptr_t eip)来打印函数源码位置信息

```
  print_stackframe(void)
   eip = read_eip();
   #option 1
   print_debuginfo(eip - 1);
   #option 2
   print_debuginfo(eip );
```


- 对于如下5条语句执行后得到的5个eip（类型为uint32_t）的结果的数值关系是什么？
```
  eip = ((uint32_t *)ebp)[2];
  eip = ((uint32_t *)ebp)[1];
  eip = ((uint32_t *)(ebp+4);
  eip = ((uint32_t *)(ebp+2);
  eip = ((uint32_t *)(ebp+1);
```        

---


- 跳转到elf的代码？


- 函数调用栈获取？


---
