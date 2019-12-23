# 处理僵尸进程

多进程程序中，当子进程结束运行时，内核不会立即释放该进程的进程表表项，以满足父进程后续对该子进程退出信息的查询
子进程进入僵尸态的两种情况：
1、在子进程结束运行之后，父进程读取其退出状态之前，称该子进程处于僵尸态
2、父进程结束或者异常终止，而子进程继续运行。此时子进程的PPID将被操作系统设置为1，即init进程。init进程接管了该子进程并等待它结束。在父进程退出之后，子进程退出之前，该子进程处于僵尸态
僵尸态的子进程将一直占据着内核资源导致资源浪费，所以应该父进程结束前调用wait或者waitpid函数来等待子进程结束并获取子进程的返回信息来避免僵尸进程的产生或者使处于僵尸态的子进程立即结束
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait( int* stat_loc );
wait函数将阻塞进程直到该进程的某个子进程结束运行为止
stat_loc：子进程退出状态信息保存到stat_loc所指的内存空间
sys/wait.h中定义了几个宏来帮助解释子进程的退出状态信息：
宏	含义
WIFEXITED(stat_val)	如果子进程正常结束，返回非0值
WEXITSTATUS(stat_val)	如果WIFEXITED非0，返回子进程退出码
WIFSIGNALED(stat_val)	如果子进程因为一个未捕获的信号而终止，返回非0值
WTERMSIG(stat_val)	如果WIFSIGNALED非0，返回一个信号值
WIFSTOPPED(stat_val)	如果子进程意外终止，返回非0值
WSTOPSIG(stat_val)	如果WIFSTOPPED非0，返回一个信号值
返回值：结束运行的子进程的PID，
pid_t waitpid( pid_t pid, int* stat_loc, int options );
pid参数：
如果pid > 0，waitpid函数等待pid参数指定的子进程结束
如果pid == -1，waitpid等价于wait函数，等待任意一个子进程结束
stat_loc：和wait相同
options参数：
如果options取值WNOHANG，waitpid将是非阻塞的：如果pid指定的目标子进程没有结束，则waitpid立即返回0；如果目标子进程正常退出，则返回该子进程PID
返回值：waitpid失败返回-1并设置errno

当一个子进程结束时，会给父进程发送SIGCHLD信号，可以在父进程中捕获SIGCHLD信号并在其信号处理函数中调用waitpid以彻底结束一个子进程来提高代码效率
一个SIGCHLD信号的处理函数典型例子：
static void handle_child( int sig )
{
    pid_t pid;
    int stat;
    while ( (pid = waitpid(-1, &stat, WNOHANG)) > 0 )
    {
        //....  //对结束的子进程的善后处理
    }
}