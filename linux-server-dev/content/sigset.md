# 信号集

## 信号集函数

Linux使用数据结构sigset_t表示一组信号
#include <bits/sigset.h>
#define _SIGSET_NWORDS (1024/(8*sizeof(unsigned long int)))
typedef struct
{
    unsigned long int __val[_SIGSET_NWORDS];
}__sigset_t;
sigset_t实际上是一个长整型数组，数组的每个元素的每个位表示一个信号，这种定义方式和文件描述符fd_set类似
信号集操作函数集：
#include <signal.h>
int sigemptyset(sigset_t* _set);    //清空信号集
int sigfillset(sigset_t* _set);     //设置信号集所有信号
int sigaddset(sigset_t* _set, int _signo);  //将信号_signo添加至信号集中
int sigdelset(sigset_t* _set, int _signo);  //将信号_signo从信号集中删除
int sigismember(sigset_t* _set, int _signo);    //测试_signo是否在信号集中

## 进程信号掩码

设置进程信号掩码的两种方式：
1、利用sigaction结构体的sa_mask成员
2、sigprocmask函数来设置和查看进程掩码
#include <signal.h>
int sigprocmask(int _how, _const sigset_t* _set, sigset_t* _oset);
_set：指定新的信号掩码
_oset：输出原来的信号掩码(如果不为NULL)
_how：指定设置进程信号掩码的方式，_how的可能取值如下表：
_how参数	含义
SIG_BLOCK	新信号掩码是当前值和_set的并集
SIG_UNBLOCK	_set指定的信号集将不被屏蔽
SIG_SETMASK	设置进程信号掩码为_set
如果_set为NULL，则进程信号掩码不变，此时我们仍然可以利用_oset参数获得进程当前的信号掩码
sigprocmask：成功返回0，失败返回-1并设置errno

## 被挂起的信号

设置进程信号掩码后，被屏蔽的信号将不能被进程接收。如果给进程发送一个被屏蔽信号，则操作系统将该信号设置为进程的一个被挂起信号。如果我们取消对被挂起信号的屏蔽，则它能立即被进程接收到
sigpending函数可以获得进程当前被挂起的信号集：
#include <signal.h>
int sigpending(sigset_t* set);
set：保存返回的被挂起的信号集，进程即使多次接收到同一个被挂起的信号，sigpending函数也只能反映一次，所以当我们再次使用sigprocmask函数使能接收该挂起的信号时，该信号的处理函数也只能触发一次
sigpending成功返回0，失败返回-1并设置errno

编程过程中，要始终清楚的直到进程在每个运行时刻的信号掩码
多进程、多线程编程环境中，要以进程、线程为单位来处理信号和信号掩码。不能设想新创建的进程、线程具有和父进程、主线程完全相同的信号特征。比如，fork调用产生的子进程将继承父进程的信号掩码，但具有一个空的挂起信号集