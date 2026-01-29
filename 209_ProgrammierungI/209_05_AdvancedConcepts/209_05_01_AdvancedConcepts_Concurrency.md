
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

# 4 Runnable, Callable, and executors


In practice, we will rarely start and manage our own threads:

- We implement the interfaces Runnable and `Callable<T>`
    
- We submit instances of Runnable and `Callable<T>` to thread pools
    
    Take a look at the APIs of the involved classes/interfaces:
    
    https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Callable.html https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html
    
    https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html

## 4.1 Runnable

Instead of sub-classing Thread and overriding the run() method, we can also implement the interface Runnable which forces us to implement the run() method.
We then simply pass an instance of that Runnable to a new Thread instance.

Runnable r = new MyRunnableImplementingClass(); Thread t = new Thread(r);
t.start();

This has the same effect as the following code (assuming identical code in the run() methods): Thread t = new MyCustomThreadSubclass();
t.start();

Runnable 接口：

是一个函数式接口，只包含一个抽象方法 run()。

实现该接口的类必须提供 run() 方法的具体实现。

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 线程要执行的代码
    }
}

// 创建线程
Runnable r = new MyRunnable();
Thread t = new Thread(r); // 将 Runnable 实例传递给 Thread 构造器
t.start(); // 启动线程
```


![](image/Pasted%20image%2020260122145523.png)


为什么推荐实现 Runnable？
避免单继承限制：
Java 不支持多继承，如果类已经继承了其他类，就无法再继承 Thread。
实现 Runnable 更灵活，因为类可以实现多个接口。

更好的设计分离：
Runnable 只定义任务逻辑（run() 方法）。
Thread 类负责线程管理（启动、调度等）。
符合"组合优于继承"的原则。


资源共享：
多个线程可以共享同一个 Runnable 实例。
适合多个线程执行相同任务的情况。

---


## 4.2 Thread pools

Thread pools reuse Thread objects to execute instances of Runnable for us. Thread pools are implementations of the interface ExecutorService.

While we can write our own ExecutorService implementations, the usual way is to use any of the factory methods in class Executors which provide access to standard thread pool implementations.

• Executors.newCachedThreadPool()returnsanExecutorServiceinstancewhichwill start threads as needed but reuse them if possible.

• Executors.newFixedThreadPool(intn)returnsanExecutorServiceinstance which will start n threads working on an input queue.

---

Thread pools reuse Thread objects to execute instances of Runnable for us. Thread pools are implementations of the interface ExecutorService.

While we can write our own ExecutorService implementations, the usual way is to use any of the factory methods in class Executors which provide access to standard thread pool implementations.

• Executors.newCachedThreadPool()returnsanExecutorServiceinstancewhichwill start threads as needed but reuse them if possible.

• Executors.newFixedThreadPool(intn)returnsanExecutorServiceinstance which will start n threads working on an input queue.

---

Once we have access to an instance of ExecutorService, we can call its instance method execute(Runnable r) to drop Runnable instances into the thread pool‘s job queue:

Runnable r = ...;  
ExecutorService pool = Executors.newCachedThreadPool(); pool.execute(r);

Depending on the thread pool implementation chosen and the current resource utilization, our Runnable instance may either be queued for a while or experience either a cold or warm start directly.

Thread pool implementations provide methods for stopping the acceptance new jobs, for additionally emptying the job queue, and for awaiting the successful pool shutdown.


----



线程池（Thread Pools）是 Java 并发编程中用于管理和复用线程的重要机制。以下是详细解释：

**1. 线程池的基本概念**
- **目的**：避免频繁创建和销毁线程带来的开销，通过复用线程对象提高性能。
- **核心接口**：`ExecutorService`（位于 `java.util.concurrent` 包中），定义了线程池的基本行为。
- **工作方式**：线程池维护一组线程，接收 `Runnable` 或 `Callable` 任务，并分配给空闲线程执行。

 **为什么使用线程池？**
- **降低资源消耗**：复用已有线程，减少创建/销毁开销。
- **提高响应速度**：任务到达时可直接使用空闲线程。
- **提供管理能力**：支持任务队列、线程数量控制、饱和策略等。 
- **避免无限制创建线程**：通过队列和固定大小防止系统过载。

```
// 创建固定大小的线程池（4个线程）
ExecutorService executor = Executors.newFixedThreadPool(4);

// 提交 Runnable 任务
executor.submit(() -> {
    System.out.println("Task running in thread: " + Thread.currentThread().getName());
});

// 优雅关闭线程池
executor.shutdown(); // 停止接收新任务，等待已提交任务完成
// executor.shutdownNow(); // 尝试立即停止所有任务
```

**2. 标准线程池的实现**

Java 通过 `Executors` 工具类提供多种预定义的线程池（工厂方法）：
 **(1) `Executors.newCachedThreadPool()`**
- **特点**：弹性线程池
    - 按需创建新线程（初始时线程数为 0）。
    - 空闲线程可存活 60 秒，超时后被回收。
    - 适合大量短期异步任务的场景。        
- **风险**：如果任务提交速度远高于处理速度，可能无限创建线程导致资源耗尽。
    

**(2) `Executors.newFixedThreadPool(int n)`**
- **特点**：固定大小线程池
    - 创建指定数量（n）的核心线程，线程数始终不变。
    - 多余的任务放入无界队列等待。
    - 适合负载较稳定的并发场景。
        

**(3) 其他常见线程池**（简要提及）
- `newSingleThreadExecutor()`：单线程池，保证任务顺序执行。
- `newScheduledThreadPool(int n)`：支持定时或周期性任务调度。
- `newWorkStealingPool()`（Java 8+）：基于工作窃取算法的并行线程池。


## 4.3 `Callable<T>`

![](image/Pasted%20image%2020260122150339.png)

 If you want a run() method with a non-void return type, just replace run() with T call()

and in the class header implement `Callable<T> `instead of Runnable.  
Main issue: We can only run a Callable via a thread pool instance and we still don‘t know how

to get the Callable‘s result.


## 4.4 Thread pools and `Future<T>`

![](image/Pasted%20image%2020260122150502.png)


## 4.5 Concurrency Outlook

Java 并发不仅仅是 `synchronized`，还包括 **内存可见性（volatile）**、  
**高级锁（Lock / Condition）**、**无锁原子类（CAS）** 以及 **高性能并发集合**。

There‘s more to concurrency:

- Doing single 64bit operations on a 32bit OS will be two instructions and may need locking.
    
- Threads are allowed to cache variables – if this becomes an issue, check-out volatile.
    
- The package java.util.concurrent.locks contains more fine-grained alternatives to
    
    synchronized blocks.
    
- Based on these advanced locks, we can create multiple Condition objects and explicitly
    
    signal waiting threads instead of the coarse-grained wait()/notify().
    
- In package java.util.concurrent.atomic, there are atomic, thread-safe data types
    
    such as AtomicInteger. These do not require locking, instead exploiting low-level single
    
    machine instructions such as compare-and-swap.
    
- The collection classes contain thread-safe, concurrent implementation for most abstract data
    
    types which are fast since they avoid locking. •...


**并发还有更多需要注意的地方：**

- 在 **32 位操作系统** 上执行 **单个 64 位操作** 时，实际上会被拆分成 **两条指令**，因此**可能需要加锁**来保证原子性。
    
- **线程被允许缓存变量的值**——如果这会引发可见性问题，可以使用 **`volatile`** 关键字。
    
- **`java.util.concurrent.locks` 包** 提供了比  
    **`synchronized` 代码块** 更加 **细粒度** 的锁机制。
    
- 基于这些高级锁，可以创建 **多个 `Condition` 对象**，  
    并且可以 **显式地唤醒（signal）等待的线程**，  
    从而替代较为粗粒度的 **`wait()` / `notify()`** 机制。
    
- 在 **`java.util.concurrent.atomic` 包** 中，提供了 **原子、线程安全的数据类型**，例如 **`AtomicInteger`**。  
    这些类型 **不需要显式加锁**，而是利用底层的**单条机器指令**（如 **compare-and-swap，CAS**）来实现线程安全。
    
- Java 的 **集合类** 中为大多数抽象数据类型提供了 **线程安全的并发实现**，  
    这些实现通常 **性能更高**，因为它们 **避免了传统的锁机制**。
    
- **……**

