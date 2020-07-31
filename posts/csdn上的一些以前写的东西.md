---
title: csdn上的一些以前写的东西
date: 2020-07-07 14:25:10
tags: csdn
---

csdn以前的一些东西

## 计算机系统基础（一）

1.关于计算机基础方面的一些理解

	对于这门课，我在刚接触的时候，跟我刚接触C语言一样，这到底讲的是个什么啊。但是经过这半个学期的学习，我其实还是懂了一些了，这门课其实是为了写出更加优美的代码，其实也就是去理解计算机的底层到底是怎样去运行的。这些底层的东西，对于我们来说其实是一个通往一个更高的水平的路，我们需要的就是去理解，去学习。

好，现在从第一章开始。

**第一章 计算机系统漫游**
		
		这一部分其实我还真的不理解，这一部分要求我们学习更多的知识才能理解，但我自己对这一部分的理解也就是一个总览，看看就好，不必深究。

**第二章 信息的表示和处理**
**2.1	信息存储**
			一个字节由8位组成		十六进制由0x开头，由0-F十六个字符表示，可大小写混合。
		**进制转换：**
		十六进制转换为二进制，直接把相应的字符转换为对应的4位二进制就行。
		十进制转换为十六进制，直接十进制数除以16，取其剩下的余数，再用那个16的倍数再次除以16继续取其余数，再反向排序余数
		十进制转换为二进制与上面也差不多，只是除数变为2。
		**字数据大小**
		![在这里插入图片描述](https://img-blog.csdnimg.cn/20190501200525609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)对于一个w位的机器，程序最多访问2^w个字节。也就是0 - 2^w
		64位机器可以运行32位的程序，这是一种向后兼容。
		我们将程序成为32位 64位 只是区别于程序是如何编译的，而不是其运行的机器类型。
		ps：所有的指针类型都是8个字节。
小端模式：低位数据放在低地址中。如0x01234567 		67就是低位数据放在低地址中。大端模式与其相反。
		**布尔代数：**~		&		|		^四种运算。运算时按位相运算就是了。
		要保持原数不变，就是与1相与，十六进制的布尔运算先转换为二进制在按位相运算。
		**移位运算**：左移直接移就是了，右移有算术和逻辑两种，算数补符号位，逻辑补0。用’<<’表示左移            
							        ‘>>’表示右移。
		
**2.2 整数表示**
		整数有有符号和无符号两种，有符号数的最大值要比最小值的绝对值小1。|Tmin| = |Tmax|+1.
		有符号数的第一位用来表示正负，用来计算十进制数时，第一位用负数参与计算，之后的数以正数加至第一个负数，最后得到其值。
		有符号数和无符号数一起参与运算时的时候，要将有符号数转化为无符号数。
		无符号数和补码的转换就是一个大小的适配。也就是加或减2^w。
		**扩展**：无符号数添0，有符号数添符号位。
		**截断**：将一个w位的数字截断为k位，就是丢弃高w-k位。
		**整数运算**：
		无符号数加法：当两个数相加之后的结果没有超过相对应的范围时，就不去管；如果超过了，就减去最大范围，剩下的就是最后的结果。
		补码加法：有正溢出和负溢出两种，也是相应的对2^w相对应运算。
当要求一个负数表示的二进制代码时，可以先将它的绝对值用二进制表示出来，之后再将其取补就是了。
		**取补**：二进制数从后向前数的第一个1之前的数全部取反就是取补。ps：补码的非也就是取补。
		ps：一般两数相加超过最大的表示范围时，就直接截断超过的那些位，留下该表示的位数。
		**无符号和有符号数的乘法**：
		![在这里插入图片描述](https://img-blog.csdnimg.cn/20190501213026781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
		**2.4   浮点数**
		符号数的小数点左移相当于除法，小数点右移相当于乘法
对于数来说，左移相当于乘法，右移相当于除法![在这里插入图片描述](https://img-blog.csdnimg.cn/20190501213145130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
		对于规格数，需要的是M=1+frac；E=Exp−Bias
而非规格数，就只是M=frac；而且E=1-bias。![在这里插入图片描述](https://img-blog.csdnimg.cn/20190501213200636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
		![在这里插入图片描述](https://img-blog.csdnimg.cn/20190501213209871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
		当阶码全为1，小数域全为0时，表示无穷 	称为‘NaN’。
		**舍入**：
		舍去：多余位小于最低位的一半
		+一半：多余位大于最低位的一半
		当多余位和最低位的一半时：
		舍：最低位为0
 +一半：最低位不为0

浮点数的细节舍入和溢出问题：
从int变为float，数字不会溢出，但可能被舍入
从int或float变为double，因为double有更大的范围，所以能精确保留数值
从double变为float，因为float的范围小，可能会溢出为无穷，也可能舍入
从float或double变为int，值会向0舍入


**第三章：程序的机器级表示**

程序计数器（也称pc）：给出将要执行的下一条指令在内存中的地址。
寄存器：有的寄存器用来保存某些重要的程序状态，而其他的用来保存临时数据，例如过程的参数和局部变量，以及函数的返回值。

gcc -s test.c		产生汇编代码
gcc -c test.c 		产生目标代码文件test.o（二进制）
objdump -d test.o 反汇编二进制文件成汇编代码
所有‘.’开头的行都是伪指令，直接无视就是了。

char 	1字节		 汇编后缀 		b
short 	2字节 		 后缀		w
int 		4字节 		 后缀		l
long   char*		8字节		后缀  	q
ps;指针都是8字节

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502084629606.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
对于 movq 指令来说，需要源操作数和目标操作数，源操作数可以是立即数、寄存器值或内存值的任意一种，但目标操作数只能是寄存器值或内存值。指令的具体格式可以这样写 movq [Imm|Reg|Mem], [Reg|Mem]，第一个是源操作数，第二个是目标操作数。
但我们没有办法用一条指令完成内存间的数据交换。

而mov指令的后缀与目的操作数有关
ps：当目的操作数为内存引用时，就要看源操作数。
有时还会进行扩展，movz表示0扩展	movs表示符号扩展

在汇编代码中用小括号表示取地址
前六个寄存器(%rax, %rbx, %rcx, %rdx, %rsi, %rdi)称为通用寄存器，有其『特定』的用途：
o		%rax(%eax) 用于做累加   		%rax 也用于函数的返回
o		%rcx(%ecx) 用于计数
o		%rdx(%edx) 用于保存数据
o		%rbx(%ebx) 用于做内存查找的基础地址
o		%rsi(%esi) 用于保存源索引值
o		%rdi(%edi) 用于保存目标索引值
而 %rsp(%esp) 和 %rbp(%ebp) 则是作为栈指针和基指针来使用的。


首先要理解的是，寄存器中存储着当前正在执行的程序的相关信息：
o临时数据存放在 (%rax, …)
o运行时栈的地址存储在 (%rsp) 中
o目前的代码控制点存储在 (%rip, …) 中
o目前测试的状态放在 CF, ZF, SF, OF 中

CF 进位标志    ZF 零标志    SF 符号标志    OF 溢出标志
其实这四个条件代码，是用来标记上一条命令的结果的各种可能的，是自动会进行设置的。
注意，使用 leaq 指令的话不会进行设置。


。对于寻址来说，比较通用的格式是 D(Rb, Ri, S) -> Mem[Reg[Rb]+S*Reg[Ri]+D]，其中：
oD - 常数偏移量
oRb - 基寄存器
oRi - 索引寄存器，不能是 %rsp
oS - 系数
除此之外，还有如下三种特殊情况
o(Rb, Ri) -> Mem[Reg[Rb]+Reg[Ri]]
oD(Rb, Ri) -> Mem[Reg[Rb]+Reg[Ri]+D]
o(Rb, Ri, S) -> Mem[Reg[Rb]+S*Reg[Ri]]
运算指令：这里以 leaq 指令为例子。具体格式为 leaq Src, Dst，其中 Src 是地址的表达式，然后把计算的值存入 Dst 指定的寄存器，也就是说，无须内存引用就可以计算，类似于 p = &x[i]

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019050208501234.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
移位操作：左移为乘法，右移为除法。都为2的倍数

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502085651931.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)

注意：cmp s1 s2；实际是用s2-s1来比较的。
		   test %rax %rax 用来检测%rax为正或负或零。
		   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502090927154.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
		   跳转指令：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502090945278.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
jmp 直接跳转；
还有一个间接跳转 加上*

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502091604548.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
do while 		/while 		/	for 三个循环都是用条件判断和跳转指令在汇编层面实现。
switch是编写一个跳转表来实现。适用于条件过多，且范围跨度较小时使用。

**过程调用：**
1.传递控制：包括如何开始执行过程代码，以及如何返回到开始的地方
2.传递数据：包括过程需要的参数以及过程的返回值
3.内存管理：如何在过程执行的时候分配内存，以及在返回之后释放内存
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190502091710547.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)

新入栈的数据放在低地址

调用新的函数时，总是push指令保存栈，然后mov指令
函数调用中会利用 %rax 来保存过程调用的返回值，以便程序继续运行的。
如果参数没有超过六个，那么会放在：%rdi, %rsi, %rdx, %rcx, %r8, %r9 中
且函数内的参数后面的先入栈，前面的后入栈

递归调用的所需空间和时间都很大，增加很多额外的东西，一般避免使用递归

