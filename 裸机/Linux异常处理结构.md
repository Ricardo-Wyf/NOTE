# Linux异常处理结构

内核在`start_kernel`函数中调用`trap_init` `init_IRQ`来设置异常处理函数。

1. `trap_init`

此函数用来设置各种异常的处理向量，Linux内核使用0xffff0000作为异常向量的基址。

`trap_init`函数将异常向量复制到**0xffff0000**处，发生异常时，根据异常向量的代码跳转至**相对地址执行对应的代码**，比如保存被中断程序的执行环境，调用异常处理函数，恢复被中断程序的执行环境并重新运行。这些更复杂的代码被复制到**0xffff0200**处。

**stubs_offset**用来重定位跳转的位置，在编译阶段的`b vector_dabt+stubs_offset`是没有具体的实际意义，

只是利用相对跳转的功能“记录”在执行代码时所需的偏移量，**在被复制之后才有意义**。

 <img src="http://images.cnitblog.com/blog/513893/201304/08231648-b85f904298d14f7a89d470d9f0a56e49.gif" alt="img" style="zoom:80%;" /> 

2. 发生异常时，跳转至相应的地址执行代码。比如`vector_und`，这些都通过`vector_stub`宏来定义。

   根据被中断程序的工作模式，跳转到不同的分支。（_und_usr . _und_svc）随后执行异常处理函数。

   - 未定义指令异常：do_undefinstr
   - 内存访问异常：1. 数据访问中止异常：do_DataAbort  2. 指令预取中止异常:do_PrefetchAbort
   - 中断：asm_do_IRQ
   - swi异常：swi指令机器码的位[23:0]被用来作为索引，通过不同的swi index调用不同的处理函数，称作系统调用，sys_open，sys_read，sys_write .etc

3. `init_IRQ`

此函数用来初始化中断的处理框架，设置各种中断的默认处理函数。