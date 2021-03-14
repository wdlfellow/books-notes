## 多线程基础

- setDeamon(true) 守护线程,jvm规定：当所有的非守护线程都退出后，jvm进程就会被关闭
- 轻量级阻塞：可以被中断的阻塞，线程状态是 waiting, timed_waiting
- 重量级阻塞: synchronized，状态是blocked
