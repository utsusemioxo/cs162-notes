# Condition Variable

## wait()
wait调用时总是假设你调用它时已经持有锁、调用者睡眠之前会释放锁，返回前重新持有锁。
## signal()

*Always hold the lock when calling signal or wait.*
