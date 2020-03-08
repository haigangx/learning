# 进程间关系

## 进程间关系——进程组

Linux下每个进程都隶属于一个进程组，每个进程除了PID外，还有进程组ID(PGID)

```
#include <unistd.h>
pid_t getpgid(pid_t pid);
```

getpgid成功时返回进程pid所属进程组的PGID，失败返回-1并设置errno

首领进程：每个进程组中其PGID和PID相同的进程。进程组将一直存在，直到其中所有进程都退出或者加入到其他进程组

```
#include <unistd.h>
int setpgid(pid_t pid, pid_t pgid);
```

setpgid将PID为pid的进程的PGID设置为pgid
- 如果pid和pgid相同，则由pid指定的进程讲被设置为进程组首领；
- 如果pid为0，则表示设置当前进程的PGID为pgid；
- 如果pgid为0，则使用pid作为目标PGID

setpgid函数成功时返回0，失败则返回-1并设置errno

一个进程只能设置自己或者其子进程的PGID，并且当子进程调用exec系列函数后，就不能再在父进程中对它设置PGID

## 进程间关系——会话

一些由关联的进程组将形成一个会话(session)，setsid函数用于创建一个会话：

```
#include <unistd.h>
pid_t setsid(void);
```

该函数不能由进程组的首领进程调用，否则将产生一个错误，对于非组首领的进程，调用该函数不仅创建新会话，而且有以下额外效果：
- 调用进程成为会话的首领，此时该进程是新会话的唯一成员
- 新建一个进程组，其PGID就是调用进程的PID，调用进程成为该组的首领
- 调用进程讲甩开终端(如果有的话)

setsid函数成功返回新的进程组的PGID，失败返回-1并设置errno

Linux进程并未提供会话ID(SID)的概念，但Linux系统认为它等于会话首领所在的进程组的PGID，并提供了getsid函数获取SID：
```
#include <unistd.h>
pid_t getsid(pid_t pid);
```

## 进程间关系——用ps查看进程间关系

在bash下执行ps命令查看进程、进程组和会话之间的关系：

```
$ ps -o pid, ppid, pgid, sid, comm | less
PID     PPID    PGID    SID     COMMAND
1943    1942    1943    1943    bash
2298    1943    2298    1943    ps
2299    1943    2298    1943    less
```

这条命令由3个子命令组成(bash, ps, less)，这3个子命令创建了1个会话(SID=1943)，2个进程组(PGID分别为1943和2298).三条命令的关系如下图：
