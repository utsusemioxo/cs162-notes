# How To Reimplement Locks
## Disable interrupts as lock
关闭中断的方式来实现锁：坏处，不支持多处理器，并且如果关闭中断太久会导致系统整个卡住。

## Implement locks with disabling interrupts

加锁之后，加锁的线程sleep，别的线程唤醒。

>比喻：把Lock的🔑，从线程A递给线程B，所以线程B可以wake up，不再sleep

>代码执行到临界区的时候，锁要么锁了要么没锁，锁是原子的。


锁有一个waiters queue，一个队列，用来track所有想要acquire lock的正在sleep的线程。
