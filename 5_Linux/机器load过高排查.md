# load的概念

系统负载（Load）是指在一段时间内CPU正在处理(R状态)以及等待CPU处理(D状态)的进程数之和的统计信息，也就是 CPU使用队列的长度的统计信息。

# load是怎么来的

首先要澄清一个概念，Linux中load的定义和传统Unix是不同的。在Linux系统中，除了R状态（Runnable：运行在CPU上或就绪等待运行），D状态（Uninterruptible sleep）也需要计算进来，导致D状态的可能原因很多，比如IO等待、锁竞争等等，这往往使Linux系统的load问题分析非常复杂。

那么为什么Linux和传统Unix在计算load时会有这样的区别呢？

当初设计如何计算load时, 如果只用R状态的进程计算load，是无法衡量系统的总体负载的。举个例子，如果只用R状态的进程计算load，当从一块更快的磁盘更换为更慢的磁盘时，由于花在磁盘上的时间变长了，进程运行在CPU的时间就会降低。磁盘变慢，系统平均load反而降低，这是不符合常理的，或者更准确的说，是不符合”系统平均负载“这个定义。所以将TASK_UNINTERRUPTIBLE和TASK_SWAPPING（后被移除），增加到了系统平均load的计算中。

或许设计者没有想到，20多年后的今天，有400多种可能将进程设置为D状态，也就是说如果出现由于D状态导致的load高，排查起来会比较困难。

## 计算load的大概过程

1.每隔一个load更新周期LOAD_FREQ（约5秒），内核会计算并更新一次当前的load值。伪代码如下: 
```C
//把R状态和D状态的线程加起来
nr_active = 0;
for_each_possible_cpu(cpu)
        nr_active += cpu_of(cpu)->nr_running + cpu_of(cpu)->nr_uninterruptible;
```

2.这里的nr_active表示活跃的任务数量。根据这个值，采用指数衰减的平均方式计算最近1分钟，5分钟，15分钟的平均load，递推公式如下：
```C
//把R状态和D状态的进程加起来
avenrun[n] = avenrun[0] * exp_n + nr_active * (1 - exp_n)
```
3.数衰减平均的好处是离当前时间越近，权重越大，所以更能反映当下的系统压力及负载变化趋势。计算得出1分钟、5分钟、15分钟的load值，会写入文件/proc/loadavg中，我们可以通过这个文件，查询到load的值，top等命令也是从该文件读取的。

```
$cat /proc/loadavg
0.00 0.01 0.05 7/2074 17373
```


# top
通过top命令看到这是一个load过高的机器.

![avatar](/resource/load%E8%BF%87%E9%AB%98%E9%97%AE%E9%A2%98%E6%8E%92%E6%9F%A5.png)

top命令的结果告诉我们系统无论是CPU还是IO，压力都没有那么大。但是有一个疑点：running进程的数量比预想中的要少，根据running的数量去换算load值完全对不上。

# ps

## 查看R状态和D状态的进程
```
ps -elf r
```
通过 ps -elf r 命令可以列举当前处于running的`进程`.
![avatar](/resource/load过高问题排查2.png)

上面说load高可能是R状态和D状态的进程导致的, 通过的"ps -elf r"命令可以确定本例几乎没有D状态的进程。

## 查看R状态的线程
`但是为什么top输出的load值和running进程数对不上呢？`

因为计算load使用的是线程数，还要查看R状态的线程.
```
ps -eLo tid,comm,stat | awk '$3 ~ /R.*/'
```
![avatar](/resource/load%E8%BF%87%E9%AB%98%E9%97%AE%E9%A2%98%E6%8E%92%E6%9F%A53.png)
这里能和第一张图的load对应上.

从这里能查到系统正在运行的线程来排查问题.




