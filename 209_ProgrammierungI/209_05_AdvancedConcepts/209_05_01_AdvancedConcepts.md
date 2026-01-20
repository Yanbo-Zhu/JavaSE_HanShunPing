
# 1 Locking in Java 

Every object has a so-called monitor which we can use as a lock, i.e., threads can ask the JVM to get exclusive access to an object‘s monitor. There are two ways for this: - The older, more coarse-grained, but easier-to-use keyword synchronized - The modern, more fine-granular locking via classes in package java.util.concurrent.locks => we‘ll go with the easier option in this course


## 1.1 synchronized block 

![](image/Pasted%20image%2020260120125622.png)



If there are several synchronized blocks locking on the same object, threads waiting at either of these blocks form a joint queue. 
Remember: 
![](image/Pasted%20image%2020260120130110.png)


 You have to make sure that all code accessing critical resources applies for a lock on it. 
 If there is even one thread that doesn‘t, your program will not have the desired behavior. 
 Hint: it helps to make variables private and to lock on such private fields as the only way to access the variable will be within the same class (unless we leaked the reference of that object…).

![](image/Pasted%20image%2020260120130516.png)

![](image/Pasted%20image%2020260120130954.png)

The following three conditions have to be met: 1. There are multiple threads. 2. At least one of these threads will occasionally write to a variable. 3. It is possible that another thread tries to access the same variable at the same time. => We have to avoid all write-write and read-write conflicts.


---

![](image/Pasted%20image%2020260120131133.png)


![](image/Pasted%20image%2020260120131249.png)


- `synchronized void foo() { ... }`  
    这是方法级的同步，锁对象是 **调用该方法的实例对象**（即 `this`）。
- 方法级同步 (`synchronized void foo()`)  
    锁住整个方法体，无法灵活缩小同步范围。


- `void foo() { synchronized(this) { ... } }`  
    这是块级同步，显式指定锁对象为 `this`，同样锁定整个实例。
- 块级同步 (`synchronized(this) { ... }`)  
    可以只锁必要的代码段，让方法中非线程不安全的部分能并发执行，提高性能。



（1）锁粒度控制
方法级同步 (synchronized void foo())
锁住整个方法体，无法灵活缩小同步范围。

块级同步 (synchronized(this) { ... })
可以只锁必要的代码段，让方法中非线程不安全的部分能并发执行，提高性能。
```
void foo() {
    // 不需要同步的代码
    synchronized(this) {
        // 需要同步的代码
    }
    // 不需要同步的代码
}
```


（2）锁对象选择的灵活性
方法级同步只能锁 this（实例方法）或 ClassName.class（静态方法）。

块级同步可以锁任意对象，例如私有锁对象：
```
private final Object lock = new Object();
void foo() {
    synchronized(lock) { ... }
}
```


3）重入性
两者都支持重入（同一个线程可重复获取同一把锁），无区别。




Why are synchronized methods too coarse-grained?
Reason #1: Methods can contain critical and safe instructions. E.g., imagine a method that
reads details about a shopping order from the console, builds an order object from it, and adds
the object to a list of shopping orders.
 Sync method: while the slow end user is typing in their order, all other threads can't access
the shopping order list
 Sync block: other threads are only blocked during the list.add() call.

Reason #2: A class could have ten instance variables of a collection type to which write access
needs to be thread-safe.
 Sync method: at any point in time, only one of the ten collections can be accessed.
 Sync block: at any point in time, only one access per collection is possible.

---

## 1.2 Summary

Locking is the mechanism to get exclusive access to a resource.
All Java objects can be used as a lock.
Threads that are waiting for a lock are in state blocked.
Locking is done via synchronized blocks.
We need to be careful to synchronize on the same object and in all places where we access a
critical resource.



# 2 deadlock

This is called a deadlock – two or more entities (here: philosophers, in Java: threads) are waiting for the respective other entities to release some resource.


Generally, deadlocks happen when threads need multiple resources to proceed.
Example:
```
void buildSandcastle() {
    synchronized (bucket) {
        synchronized (spade) {
            //build castle
        }
    }
}
```

There are different solutions to this problem. A simple one (where possible) is to
always acquire locks in the same order (e.g., always bucket before spade)
everywhere in the code. An alternative could be to have one dedicated lock
object and use that instead.


==Deadlocks happen when threads need locks on 2+ resources and are waiting for each other to release the respective other one. A simple approach (if possible) is to use the same lock acquisition order everywhere in the code.==

# 3 Thread: wait() and notify()



![](image/Pasted%20image%2020260120133133.png)

Producer-Consumer Problem
This scenario is called the producer-consumer problem. We can solve it via busy waiting +
polling (see example program NoWaitNotify.java), i.e., both threads continuously check whether
they can do their job (=produce or consume). This is really inefficient, though.


A better alternative is to have the threads notify each other when they are done:
![](image/Pasted%20image%2020260120134014.png)


Wait() and notify()
In Java, we can use a mechanism provided by an object's monitor: Once we have access to the monitor, we can call notify() or notifyAll() on that object to wake up one or all waiting threads. And we can call wait() to start waiting for notifications.


![](image/Pasted%20image%2020260120134153.png)


This mostly works. Unfortunately, there is a chance of so-called spurious wakeups, i.e., we
can't be sure that someone notified our thread or whether it was just a random wakeup.
=> Turn condition check into loop.
![](image/Pasted%20image%2020260120134309.png)



![](image/Pasted%20image%2020260120134630.png)

Instead of busy waiting, we let threads notify each other and wait for notifications – as we would intuitively do in the real world.
For this, threads call an object's (typically the buffer's) wait() and notify()/notifyAll() methods.
Let's briefly check performance via the example programs WaitNotify.java and NoWaitNotify.java.


