
Thread, Runnable
Locking and synchronized
Thread lifecycle: wait, sleep, yield, notify, interrupt, join

# 1 Concept 

Concurrency is the parallel or pseudo-parallel execution of instructions.

Parallelization: Code without cross-dependencies can be executed in parallel – once it has been parallelized, we can also distribute the code to multiple machines.

Scheduling: 
Today’s CPUs have multiple independent cores which can execute tasks in parallel. Older CPUs and individual cores of CPUs typically use time slice-based scheduling approaches
![[Pasted image 20250416135542.png]]



# 2 Thread


Each process has its own memory space and has at least one thread.
• All threads within a process share the same memory space (i.e., they have access to the same
variables).

For Java:
• It is possible to create new processes (see class ProcessBuilder), but the typical scenario will be the Java VM as one process and multiple threads within that process.
• When you start a Java program, the Java VM creates the default thread “main“ and uses it to execute the specified main method.
• During execution, often new threads are created – either explicitly by your program or implicitly by using certain classes*.

## 2.1 Creating threads in Java

There are two ways to explicitly create a new thread in Java:
new Thread();
new Thread(Runnable r);

To create our “own“ thread, we can either extend the class Thread or implement the interface Runnable. In both cases, we override/implement the run() method where we specify the code that shall be run in a parallel thread.

If we just invoke this run() method, the code within it is (obviously) executed sequentially. If we want to run it in parallel, we have to invoke the start() method (defined in class Thread), which executes the run()method in a new thread in parallel. 


Example: Thread
```java
public class CounterThread extends Thread {
    public void run() {
        for (int i = 1; i < 11; i++) {
            System.out.println("I am " + Thread.currentThread().getName() + " and my number is " + i);
            try {
                Thread.sleep((long)(Math.random() * 100));
            } catch (InterruptedExceptione) {
                this.interrupt();
            }
        }
    }
    publicstaticvoidmain(String[] args) {
        new CounterThread().start();
        new CounterThread().start();
    }
}
```

output
```
I am Thread-0 and my number is 1
I am Thread-1 and my number is 1
I am Thread-1 and my number is 2
I am Thread-1 and my number is 3
I am Thread-0 and my number is 2
I am Thread-1 and my number is 4
I am Thread-0 and my number is 3
I am Thread-1 and my number is 5
I am Thread-1 and my number is 6
I am Thread-0 and my number is 4
I am Thread-0 and my number is 5
I am Thread-1 and my number is 7
I am Thread-0 and my number is 6
I am Thread-0 and my number is 7
I am Thread-0 and my number is 8
I am Thread-1 and my number is 8
I am Thread-1 and my number is 9
I am Thread-0 and my number is 9
I am Thread-1 and my number is 10
I am Thread-0 and my number is 10
```


Example: Runnable
```java
public class CounterRunnable implements Runnable {
    public void run() {
        for (int i = 1; i < 11; i++) {
            System.out.println("I am " + Thread.currentThread().getName() + " and my number is " + i);
            try {
                Thread.sleep((long)(Math.random() * 100));
            } catch (InterruptedExceptione) {
                Thread.currentThread().interrupt();
            }
        }
    }
    publicstaticvoidmain(String[] args) {
        new Thread(new CounterRunnable()).start();
        new Thread(new CounterRunnable()).start();
    }
}
```


```
I am Thread-0 and my number is 1
I am Thread-1 and my number is 1
I am Thread-0 and my number is 2
I am Thread-1 and my number is 2
I am Thread-0 and my number is 3
I am Thread-0 and my number is 4
I am Thread-0 and my number is 5
I am Thread-0 and my number is 6
I am Thread-1 and my number is 3
I am Thread-1 and my number is 4
I am Thread-1 and my number is 5
I am Thread-0 and my number is 7
I am Thread-1 and my number is 6
I am Thread-1 and my number is 7
I am Thread-0 and my number is 8
I am Thread-0 and my number is 9
I am Thread-0 and my number is 10
I am Thread-1 and my number is 8
I am Thread-1 and my number is 9
I am Thread-1 and my number is 10
```


---

Thread vs. Runnable

Normally, the approach via Runnable is taken unless the respective class is onlya thread.

extends Thread
- Has direct access to instance methods of superclass
- Can have no other superclass
- Create and start instance directly

- **Inherits** from `Thread` class.
- **Cannot extend** any other class (Java has single inheritance).
- Each instance **is a thread**, so can be directly started with `.start()`.
- Can override the `run()` method to define behavior.
- **Tight coupling** between task and thread execution.

```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in MyThread");
    }
}
MyThread t = new MyThread();
t.start();
```

implements Runnable
- Needs to access those methods via Thread.currentThread()
- Can have arbitrary superclasses
- Build new thread from runnable object and start that thread
- Can be used with thread pools

- **Implements** `Runnable` interface.
- Can **extend any other class** (more flexible).
- Must define the `run()` method.
- Create a `Thread` and pass the `Runnable` instance to it.
- Better separation of task and thread, especially useful in **thread pools / executors**.

```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Running in MyRunnable");
    }
}
Thread t = new Thread(new MyRunnable());  // Better separation of task and thread, especially useful in **thread pools / executors**.
t.start();
```

---

Use **`Runnable`** (or even better, `Callable` with `Executors`) when possible, unless you have a specific reason to subclass `Thread`.

|Feature|`extends Thread`|`implements Runnable`|
|---|---|---|
|Inheritance|Must extend `Thread`|Can extend any class|
|Reusability|Less reusable|More reusable|
|Separation of concern|Not separated (thread = task)|Clear separation (task ≠ thread)|
|Thread pools|❌ Not ideal|✅ Recommended|
|Flexibility|Limited|High|

## 2.2 Properties of threads

- Name: can be set via the constructor or the getter/setter. Default name is “Thread-n”.
- State: thread state can be New, Runnable (Ready, Running), Blocked, Waiting, Timed Waiting or Terminated. State changes are mainly managed by the Java VM (see lifecycle of threads).
- Priority: threads have a priority which determines the share or CPU time that they should be assigned. Since this is OS-specific behavior, there are no guarantees for this.


Thread lifecycle
![[Pasted image 20250416142136.png]]


### 2.2.1 Ready 和 Running
The scheduler assigns time slices to the individual threads and sets their state either to Ready (could be executed but is not assigned to a CPU core) or Running (is being executed).
Any thread may invoke Thread.yield() to signal to the scheduler its willingness to change its own state to Ready.

### 2.2.2 Sleeping and interruptions



`isInterrupted` ist eine boolean flag in jede Thread 

- A thread can go to sleep for a specified period of time by invoking Thread.sleep(long).
- As a thread may be interrupted (i.e., politely be asked to terminate), the compiler forces us to catch an InterruptedException. This exception is thrown whenever someone invokes the `interrupt()` method on our thread while our thread is sleeping.
	- 用 interrupt 去唤醒一个 sleep 的 thread 
- Important: Catching this exception resets the interrupted flag which we can also query using `isInterrupted()`.(就是说 isInterrupted() )  To avoid this, we should call interrupt() in the catch block a gai

```
public void run(){
while(!isInterrupted()){
	//…
	}
}

run()：线程启动后执行的方法
isInterrupted()：这是 Thread 类的一个方法，用于检查当前线程是否被中断。
while (!isInterrupted())：只要线程没有被中断，就会一直循环执行。
```


---
例子

sleep → interrupt → catch → flag

1 Thread.sleep(long millis)
- 让当前线程“休眠”指定毫秒数。
- 在睡眠期间，线程进入 **TIMED_WAITING** 状态。


2 如果有线程在 sleep()，可以通过 interrupt() 来打断它
```java
Thread t = new Thread(() -> {
    try {
        Thread.sleep(5000);
    } catch (InterruptedException e) {
        System.out.println("I was interrupted during sleep!");
    }
});
t.start();
t.interrupt(); // 会让上面的线程从 sleep 中醒来，并抛出 InterruptedException
```

3 重要行为：InterruptedException 会清除中断标志！
```java
try {
    Thread.sleep(10000);
} catch (InterruptedException e) {
    System.out.println(Thread.currentThread().isInterrupted()); // false!
}
```
- 即使是被中断唤醒的，但 **isInterrupted() 是 false**
- 因为 JVM 自动清除了标志位（为了防止你不小心再次中断）


4 如果你想保留中断状态怎么办？
你可以手动“重置”中断标志：
```java
try {
    Thread.sleep(10000);
} catch (InterruptedException e) {
    Thread.currentThread().interrupt(); // 手动设置回来
}
```

这样，isInterrupted() 就会返回 true，后续的代码就能识别到这个中断信号。

如果你想让程序在 `catch` 块里“记住”自己被中断了，就要 **重新调用一次 `interrupt()`**，否则这个信号就“丢了”。



## 2.3 Interdependencies of threads join()

Most program sections can be run in parallel as these sections do not have interdependencies.
There are some exceptions, though:
- Waiting for another thread to terminate. Example: we trigger a job that in parallel retrieves some data. When our execution reaches a certain point in the code, we need that data to continue.
- Synchronizing critical sections*. Example: we want to increment a variable; this involves reading the value and writing the new value. If another thread does the same in parallel, arbitrary outcomes are possible. (i++is not atomic!)
- Waiting for another thread to produce results. Example: one thread produces values; another thread consumes these values.

Waiting for another thread to terminate
- The current thread can wait for another thread to terminate by calling the join() method on that other thread. This causes the current thread to go into state Waiting until the other thread has reached the state Terminated.
- There is also a join(long)method, which tries to wait for another thread until either the timeout expires, or the other thread terminates. In that case, the current thread goes into Timed Waiting.
- When we call join(), we are also forced to consider the checked InterruptedException.


- `join()` 是线程间同步的一种简单方式。
- 它会让主线程等待子线程执行完成后再继续。
- 非常适合用于**串行执行多个线程任务**的场景。

```java
public class JoinedThread extends Thread {
    public void run() {
        System.out.println(getName() + ": Sleeping now…");
        try {
            Thread.sleep(1000); // 模拟任务
        } catch (InterruptedException e) {
            this.interrupt(); // 再次设置中断状态（可选）
        }
        System.out.println(getName() + ": I'm done.");
    }

    public static void main(String[] args) throws InterruptedException {
        JoinedThread jt = new JoinedThread();
        System.out.println(Thread.currentThread().getName() + ": Ready to start.");
        jt.start();
        System.out.println(Thread.currentThread().getName() + ": Ready to join.");
        jt.join(); // 主线程等待 jt 完成
        System.out.println(Thread.currentThread().getName() + ": He's done!");
    }
}
```


ouput
```
main: Ready to start.
main: Ready to join.
Thread-0: Sleeping now...
Thread-0: I'm done.
main: He's done!
```


|关键代码|说明|
|---|---|
|`jt.start();`|启动子线程，执行 `run()`|
|`jt.join();`|主线程会阻塞，直到子线程 `jt` 执行完毕|
|`Thread.sleep(1000);`|子线程休眠 1 秒，模拟任务耗时|
|`this.interrupt();`|可选操作，用于保留中断状态（通常在中断捕获后不会自动保留）|

步骤解释 
1. JoinedThread jt = new JoinedThread();
	1. 主线程（名字通常是 "main"）创建了一个子线程对象，线程还没有启动。
2. `System.out.println(Thread.currentThread().getName() + ": Ready to start.");`
	1. 这句由主线程执行。
3. `jt.start();`
	1. 启动子线程，子线程开始执行 `run()` 方法中的代码。
	2. 注意：调用 `jt.start()` 之后，**主线程并不会等待子线程**，而是继续执行到 `jt.join()`。
4. `System.out.println(Thread.currentThread().getName() + ": Ready to join.");`
5. jt.join();
	1. 这行代码的作用是：主线程进入“等待”状态，**直到子线程运行完毕**。
6. 执行 run()  中内容 


关于 `this.interrupt();` 的作用

在 `catch (InterruptedException e)` 中调用 `this.interrupt()` 是为了 **保留中断状态**。因为 `Thread.sleep()` 后被中断时：
-  中断就是代表 执行 t.interrupt(); // 会让上面的线程从 sleep 中醒来，并抛出 InterruptedException
- 执行完了 t.interrupt() 后 Thread.currentThread().isInterrupted() = false 
- JVM 会清除线程的中断标志（`isInterrupted()` 会变为 `false`）, 而不是 true 。`isInterrupted()`  = true 代表  你曾经被中断过 。  因为 JVM 自动清除了标志位（为了防止你不小心再次中断
- 如果你还希望后续代码知道“线程曾经被中断过”，你可以**手动再设置一次中断状态**

这在某些复杂业务逻辑中是有意义的。


# 3 Locking and synchronized

- Whenever we need to execute several instructions atomically, i.e., no other thread may access the respective variables in between, we have to coordinate which thread may work on which resources.
- For this purpose, Java offers several mechanisms based on locking.

Um Konfilct zu vermeiden,  die durch  mehere Thread geleichzeitig an andenn selber object oder daten bearbeiten 

## 3.1 Locking in Java

Java offers several mechanisms to handle locks:
- synchronized(as language keyword)
- A number of classes in the package java.util.concurrent.locks (in this course, we will only introduce synchronized which is less efficient but also easier to use)


### 3.1.1 synchronized block 

A synchronized block always requires an object to “lock” on; typically, one uses the object which shall be modified as lock.
Instructions within a synchronized block may still be “interrupted” (remember the time slices!), but only one thread may execute synchronized blocks, which lock on the same object, in parallel.
Threads that are waiting for a lock are in state Blocked

---

什么是 `synchronized block`？
Java 中的 `synchronized` 关键字用于**同步线程对共享资源的访问**，防止数据竞争（race conditions）。
```java
synchronized (someObject) {
    // 只能一个线程进入这个代码块（如果锁的是同一个对象）
}
```


A synchronized block always requires an object to “lock” on; typically, one uses the object which shall be modified as lock.
synchronized 块总是需要一个对象来加锁；通常选择被修改的对象作为锁。
- Java 中每个对象都可以作为“锁对象”（monitor）。
- 当一个线程进入一个 `synchronized(obj)` 代码块时，它必须先**获得这个对象的锁**。
- 其他线程如果也想进入对同一个 `obj` 加锁的代码块，就必须**等待**前一个线程释放这个锁。



Instructions within a synchronized block may still be “interrupted” (remember the time slices!), but only one thread may execute synchronized blocks, which lock on the same object, in parallel.
**翻译：** `synchronized` 代码块中的指令仍然可能被“中断”（例如，线程时间片耗尽），但**同一时间**只能有一个线程执行对同一个锁对象加锁的代码块。

**说明：**
- **线程时间片（time slice）**指的是 CPU 给线程运行的时间窗口。即使线程获得了锁，它运行中也可能因为时间片耗尽而暂时停止，换另一个线程执行。
- 但！如果另一个线程也想进入 `synchronized(obj)` 的块，它**必须等待锁释放**，即使前一个线程被 CPU 调度暂停了，也不会放弃锁。
- 因此：**互斥性 mutual exclusivity 是基于锁，而不是 CPU 是否在运行线程。**



- `synchronized` 是**互斥锁机制（mutex）**的一种实现。
- 你锁的是对象，不是代码块。
- 所以多个 `synchronized(sameObject)` 的代码块即使出现在不同的方法或类中，也只允许一个线程进入！


----

例子 


1

```java
public class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) {  // 锁住的是当前 Counter 实例
            count++;
        }
    }
}
```


如果两个线程都调用了 `increment()`：
- 第一个线程进入 `synchronized(this)` 并加1。
- 第二个线程**必须等待**第一个线程退出 `synchronized` 才能进入。

---


2 

![[Pasted image 20250416154723.png]]

```java
public class SyncBlock {
    List < String > stringList = newArrayList < > ();
    void add (String s) {
        synchronized (stringList) {
            stringList.add(s);
        }
    }
    void print() {
        synchronized (stringList) {
            for (String s: stringList) System.out.println(s);
        }
    }
}
```


### 3.1.2 synchronized method 


We can also synchronize methods, which is equivalent to syncing on “this“ or the respective class. Since this is very coarse-grained, synchronized methods are typically not a good idea:


![[Pasted image 20250416155307.png]]


# 4 notify and wait 

Waiting for other threads to produce results

- `wait()`、`notify()` 和 `notifyAll()` 必须在 `synchronized` 代码块中调用！
- 这些方法属于 **Object** 类，而不是 Thread。
- 推荐使用 `while` 而不是 `if`，防止虚假唤醒（spurious wakeup）。

## 4.1 wait(), notify(), and notifyAll()


Class Objectalready implements these notification mechanisms so that we only have to use them:

==When we invoke wait()on an object, the current thread goes into state Waiting (or Timed Waiting) until another thread invokes notify()or notifyAll()on the same object==.

![[Pasted image 20250416155639.png]]

Note that a thread needs to have the lock of the respective object, e.g., ==execution must be in a synchronized block on the same object==, to be able to invoke either wait(), notify(), or notifyAll().
Typically, notifyAll() is the preferred alternative.

==notify() 被执行后 会自动 找 同一个 object 下 那个一个 thread in state waiting, 然后 interupt 这个 thread 唤醒他 ==


## 4.2 notify()

- This call **selects ONE of the waiting threads** to be awakened.
- **Which one?** → It’s **chosen arbitrarily** (depends on JVM/thread scheduler — not under your control).


## 4.3 例子  producer-consumer problem


Sometimes, synchronizing critical sections alone does not suffice. We also may need to distinguish different types of threads.


![[Pasted image 20250416155359.png]]

This scenario is called the producer-consumer problem. We can solve it via polling (see example program NoWaitNotify), i.e., both threads continuously check whether they can do their job (=produce or consume). This is really inefficient, though.

A better alternative is to have the threads notify each other when they are done:

![[Pasted image 20250416155432.png]]


- **生产者线程**添加数据
- **消费者线程**等待数据

我们用 `wait()` 让消费者“等一会儿”，用 `notify()` 或 `notifyAll()` 唤醒它们。

==When we invoke wait() on an object, the current thread goes into state Waiting (or Timed Waiting) until another thread invokes notify() or notifyAll() on the same object==.

```java
class Buffer {
    private int data;
    private boolean hasData = false;

    public synchronized void produce(int value) throws InterruptedException {
        while (hasData) {
            wait();  // wait until the buffer is empty
        }
        data = value;
        hasData = true;
        System.out.println("Produced: " + data);
        notify();  // notify consumer   notify() 被执行后 会自动 找 同一个 object 下 那个一个 thread in state waiting, 然后 interupt 这个 thread 唤醒他 
    }

    public synchronized void consume() throws InterruptedException {
        while (!hasData) {
            wait();  // wait until the buffer has data
        }
        System.out.println("Consumed: " + data);
        hasData = false;
        notify();  // notify producer
    }
}

class Producer extends Thread {
    private final Buffer buffer;

    public Producer(Buffer buffer) {
        this.buffer = buffer;
    }

    public void run() {
        for (int i = 1; i <= 5; i++) {
            try {
                buffer.produce(i);
                Thread.sleep(500);  // simulate time to produce
            } catch (InterruptedException ignored) {}
        }
    }
}

class Consumer extends Thread {
    private final Buffer buffer;

    public Consumer(Buffer buffer) {
        this.buffer = buffer;
    }

    public void run() {
        for (int i = 1; i <= 5; i++) {
            try {
                buffer.consume();
                Thread.sleep(800);  // simulate time to consume
            } catch (InterruptedException ignored) {}
        }
    }
}

public class ProducerConsumerTest {
    public static void main(String[] args) {
        Buffer buffer = new Buffer();
        new Producer(buffer).start();
        new Consumer(buffer).start();
    }
}
```



![[a1300749-0088-4d12-b91a-d2df660f57a8.png]]


# 5 Further topics

ReentrantLockas alternative to synchronizedblocks.
Conditionas alternative to wait()and notify()
Callableand executors as alternative to direct thread management.
