# SWI异常模式程序

> swi软件中断：Software Interrupt

ARM的CPU有7种工作模式，**除了用户模式外**，其他6种都是特权模式，特权模式可以直接修改**CPSR(Current Program State Register**)进入其他模式。

> USR用户模式不能修改CPSR进入其他模式
>
> Linux应用程序一般运行于用户模式(不可访问硬件)

SO，访问硬件必须切换模式，切换模式的方法：**发生异常**。

发生异常的3种模式：

1. 中断
2. und
3. **swi+某个值（使用软件中断切换模式，切换到SVC模式）**

```Start.S
SWI示例：
1.复位之后，CPU处于SVC模式
  将其切换到USR模式
  设置栈
  跳转执行
2.添加SWI异常处理
3.引入一条SWI指令
```

查看异常向量表，SWI异常的向量地址为**0x8**

1. **切换至USR模式**

   USR模式下的M0~M4为10000

   ![1584414554407](C:\Users\Ricardo\AppData\Roaming\Typora\typora-user-images\1584414554407.png)

   ![1584415156341](C:\Users\Ricardo\AppData\Roaming\Typora\typora-user-images\1584415156341.png)

   ```assembly
   /*先进入USR模式*/
   mrs r0, cpsr /*读出CPSR 读到r0*/
   bic r0, r0, #0xf /*使用bic命令 bitclean 把低4位清零*/
   msr cpsr, r0 /*清零后M[4:0]=10000,进入USR模式*/
   
   /*设置栈：sp_usr 只要不发生冲突就行*/
   ldr sp, =0x33f00000
   
   ```

先编译运行，看是否能处理und的指令。

2. 添加SWI异常处理：

```assembly
/*添加SWI指令 直接将地址写入PC，使用绝对地址，在SDRAM执行命令*/
_start:
	b reset			 /*vector 0 : reset */	
	ldr pc, und_addr /*vector 4 : und */
	ldr pc, swi_addr /*vector 8 : swi */
	
/* swi异常处理 */
swi_addr:
	.word do_swi
	
/* 以4字节对齐 */
.align 4

do_swi:
	/* 在跳转到该异常中断处理程序之前：
     * lr_svc保存被中断模式中（此示例中为USR）的下一条即将执行的指令地址
     * SPSR_svc保存被中断模式的CPSR
     * CPSR中的M4~M0被设置为10011，进入SVC模式
     * 跳到0x08的地方执行程序
     */
     /* 每个模式都有自己专属的栈，故先设置SP_SVC */
     ldr sp, =0x33e0000
     
     /* 在SWI异常处理函数中可能会修改r0-r12，所以先保存 
      * lr是异常处理完后的返回地址，在异常处理函数中可能会调用函数而改变lr，故需保存
      */
      stmdb sp!, {r0-r12, lr}
      
      /* 处理SWI异常 该示例只是打印 */
      mrs r0, cpsr
      ldr r1, =swi_string
      bl printException
      
      /* 恢复现场 */
      ldmia sp!, {r0-r12 ,pc}^ /* ^会把spsr的值恢复到cpsr里 */
      
swi_string:
	.string "swi exception"
	
.align 4
/**************
表明下面的标号要放在4字节对齐的地方
*/
```

3. **引入SWI指令：**

   ```assembly
   swi 0x123 	/* 触发SWI异常，跳到0x08执行 ldr pc, swi_addr  //vector 8 : swi  */
   ```

   打印"swi exception"



**读取SWI传入的val**

> 在执行异常处理函数前，lr_svc保存有被中断模式的下一条指令的地址
>
> 将lr_svc里的地址读出来再 -4 就是 “指令swi 0x123” 的地址 0x123就保存在此地址

在do_swi中：

```assembly
bl printException
```

在**printSWIVal**函数的前面，会改变lr的值，在执行此函数前需要保存lr

```assembly
mov r4, lr /* 根据ATPCS规则，r4~r11在C函数里会被保存，不会被C语言破坏 */
...
bl printException
...
sub r0, r4, #4
bl printSWIVal   
```

**在此示例中，如果先调用printSWIVal函数，就不用考虑lr会被其他函数破坏的事情**

> **isnn't it?**

在Uart.c中添加函数：

```c
void printSWIVal(unsigned int *val)
{
    puts("SWI val = ");
    printHEX(*val & ~0xff000000); /* 高8位忽略 */
    puts("\n\r");
}
```



