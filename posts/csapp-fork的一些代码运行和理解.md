---
title: csapp fork的一些代码运行和理解
date: 2020-07-07 16:41:14
tags: csdn fork

---

### fork

**1.** 先说一点简单的fork定义
1.创建一个新进程，新进程被称为子进程。两者最大的区别在于pid不同，一般通过返回值分辨是哪个进程。
2.fork函数被调用一次但返回两次。两次返回的唯一区别是子进程中返回0值而父进程中返回子进程ID。
3.子进程是父进程的副本，它将获得父进程数据空间、堆、栈等资源的副本。子进程持有的是上述存储空间的“副本”，这意味着父子进程间不共享这些存储空间。各自都有自己的独立地址。
4.两者的执行顺序是随机并发执行的，也就是不能判断执行顺序。

**2** 为什么fork会返回两次？
由于在复制时复制了父进程的堆栈段，所以两个进程都停留在fork函数中，等待返回。因此fork函数会返回两次，一次是在父进程中返回，另一次是在子进程中返回，这两次的返回值是不一样的，**父进程返回子进程的pid，子进程返回0**
**3**现在来看下一些代码吧：
1.
```cpp
void fork1() {
	int x = 1;
	pid_t pid = fork();
	if (pid == 0) {
		printf("Child has x = %d\n", ++x);
	} else {
		printf("Parent has x = %d\n", --x);
	}
	printf("Bye from process %d with x = %d\n", getpid(), x);
}
```
结果：![在这里插入图片描述](https://img-blog.csdnimg.cn/2019110910200816.png)
先fork一个子进程，这次子进程先执行，之后再试父进程，最后得到这个结果。


下面有一些关键字，[waitpid等](https://blog.csdn.net/GEAUSE/article/details/102933742)，可以去看一下。

2.下一个是关于僵尸进程的问题

```c
void fork7() {
	if (fork() == 0) {
		/* Child */
		printf("Terminating Child, PID = %d\n", getpid());
		exit(0);
	} else {
		printf("Running Parent, PID = %d\n", getpid());
		while (1);
		/* Infinite loop */
	}
}
```
结果：![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109103227316.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
**僵死进程**：是指子进程退出时，父进程并未对其发出的SIGCHLD信号进行适当处理，导致子进程停留在僵死状态等待其父进程为其回收，这个状态下的子进程就是僵死进程。

主进程运行然后fork一个子进程，父进程因为while（1），一直在后台运行，但是子进程正常执行完程序。最后在后台卡住，使用Ctrl+z挂起这个程序，之后ps命令去看可看见这个程序的父进程（pid=4300）还是在后台挂起，只能用kill命令强制结束。

**3.** 这个是关于孤儿进程

```c
void fork8() {
	if (fork() == 0) {
		/* Child */
		printf("Running Child, PID = %d\n",getpid());
		while (1)
			    ;
		/* Infinite loop */
	} else {
		printf("Terminating Parent, PID = %d\n",
			       getpid());
		exit(0);
	}
}
```
结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109104910232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
**孤儿进程**：父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤儿进程。孤儿进程将被init进程(pid=1)所收养，并由init进程对它们完成状态收集工作。

由ps命令可看见子进程（pid=4304）还在后台运行，这就是一个孤儿进程。

**4.**关于wait

```c
#define N 5
void fork10() {
	pid_t pid[N];
	int i, child_status;
	for (i = 0; i < N; i++)
		if ((pid[i] = fork()) == 0) {
		exit(100+i);
		/* Child */
	}
	for (i = 0; i < N; i++) {
		/* Parent */
		pid_t wpid = wait(&child_status);
		if (WIFEXITED(child_status))
			    printf("Child %d terminated with exit status %d\n",
				   wpid, WEXITSTATUS(child_status)); else
			    printf("Child %d terminate abnormally\n", wpid);
	}
}

void fork11() {
	pid_t pid[N];
	int i;
	int child_status;
	for (i = 0; i < N; i++)
		if ((pid[i] = fork()) == 0)
		    exit(100+i);
	/* Child */
	for (i = N-1; i >= 0; i--) {
		pid_t wpid = waitpid(pid[i], &child_status, 0);
		if (WIFEXITED(child_status))
			    printf("Child %d terminated with exit status %d\n",
				   wpid, WEXITSTATUS(child_status)); else
			    printf("Child %d terminate abnormally\n", wpid);
	}
}
```
结果：![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109113504188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzOTc3ODE4,size_16,color_FFFFFF,t_70)
对于**fork10**，通过for循环先fork一个子进程，父进程进行到wait暂停，之后子进程运行exit正常退出，给出一个信号给父进程，然后根据wait的特点，父进程继续执行输出语句。

对于**fork11**，通过for循环先fork一个子进程，父进程进行到waitpid(pid[i],&child_status, 0)暂停，之后waitpid等待着pid[i]的子进程结束，之后for（0-4）正常结束子进程，for（4-0）waitpid返回子进程结束值，之后执行输出语句。

**5**关于信号

```c
 */
void fork12() {
	pid_t pid[N];
	int i;
	int child_status;
	for (i = 0; i < N; i++)
		if ((pid[i] = fork()) == 0) {
		/* Child: Infinite Loop */
		while(1);
		}
	for (i = 0; i < N; i++) {
		printf("Killing process %d\n", pid[i]);
		kill(pid[i], SIGINT);//杀死此进程
		}
	for (i = 0; i < N; i++) {
		pid_t wpid = wait(&child_status);
		if (WIFEXITED(child_status))
			    printf("Child %d terminated with exit status %d\n",wpid, WEXITSTATUS(child_status)); 
		else
			    printf("Child %d terminated abnormally\n", wpid);
		}
}
* fork12 - Sending signals with the kill() function,发送信号给前一个参数的进程
 cheng@晟松:/mnt/d/计算机基础代码测试/chap8_code$ ./forks 12
Killing process 34
Killing process 35
Killing process 36
Killing process 37
Killing process 38
Child 34 terminated abnormally
Child 35 terminated abnormally
Child 36 terminated abnormally
Child 37 terminated abnormally
Child 38 terminated abnormally
 */
```
kill（）函数就是给一个信号给进程。[参考kill()函数](https://blog.csdn.net/qq_42152681/article/details/90261295)
此函数里面父进程被强制结束，子进程一直循环，结果子进程被系统自动回收，WIFEXITED()不能得到信号，最后显示出子进程被异常终止。