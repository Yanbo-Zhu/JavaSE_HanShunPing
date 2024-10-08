
https://www.baeldung.com/java-heap-thread-core-dumps

**A dump is data queried from a storage medium and stored somewhere for further analysis**. 
The [Java Virtual Machine (JVM)](https://www.baeldung.com/jvm-vs-jre-vs-jdk#jvm) helps to manage memory in Java, and in the case of errors, we can get a dump file from the JVM to diagnose errors

Dump从JVM导出， 用来诊断问题的， 

# 1 Heap Dump

During runtime, the JVM creates the heap, which contains references to objects in use in a running Java application. **The [heap dump contains a saved copy](https://www.baeldung.com/java-heap-dump-capture) of the current state of all objects in use at runtime**.

Additionally, **it’s used to analyze the [_OutOfMemoryError_](https://www.baeldung.com/java-memory-leaks) errors in Java**.

Furthermore, **the heap dump can be in two formats – the classic format and the Portable Heap Format (PHD)**.

The classic format is human-readable, while the PHD is in binary and needs tools for further analysis. Also, PHD is the default for a heap dump.

Moreover, modern heap dumps also contain some thread information. Starting from Java 6 update 14, a heap dump also contains stack traces for threads. **The stack traces in the heap dump connect objects to the threads using them**.

Analysis tools like Eclipse Memory Analyzer include support to retrieve this information.

## 1.1 Use Case[](https://www.baeldung.com/java-heap-thread-core-dumps#1-use-case)

如何产生heap dump 

**Heap dumps can help when analyzing _OutOfMemoryError_ in a Java application**.

Let’s see some example code that throws _OutOfMemoryError_:

```java
public class HeapDump {
    public static void main(String[] args) {
        List numbers = new ArrayList<>();
        try {
            while (true) {
                numbers.add(10);
            }
        } catch (OutOfMemoryError e) {
            System.out.println("Out of memory error occurred!");
        }
    }
}
```

In the example code above, we create a scenario of an infinite loop until the heap memory is full. As we know, the _new_ keyword helps to allocate memory on the heap in Java.

To capture the heap dump of the code above, we’ll need a tool. **One of the most used tools is [_jmap_](https://www.baeldung.com/java-heap-dump-capture#1-jmap)**.

First, we need to get the process ID of all running Java processes on our machine by running the _jps_ command:

```bash
$ jps
```

The command above outputs to the console all running Java processes:

```java
12789 Launcher
13302 Jps
7517 HeapDump
```

Here, our process of interest is _HeapDump._ Therefore, let’s run the _jmap_ command with the _HeapDump_ process ID to capture the heap dump:

```bash
 $ jmap -dump:live,file=hdump.hprof 7517
```

The command above generates the _hdump.hprof_ file in the project root directory.

Finally, **we can use tools like Eclipse Memory Analyzer to analyze the dump file**.


# 2 Thread Dump[](https://www.baeldung.com/java-heap-thread-core-dumps#thread-dump)

**The [thread dump](https://www.baeldung.com/java-thread-dump#:~:text=A%20thread%20dump%20is%20a,it%20displays%20the%20thread's%20activity.) contains the snapshot of all threads in a running Java program at a specific instant**.

A thread is the smallest part of a process that helps a program to operate efficiently by running multiple tasks concurrently.

Furthermore, a **thread dump can help diagnose efficiency issues in a Java application**. Thus, it’s a vital tool for analyzing performance issues, especially when an application is slow.

Additionally, **it can help detect threads stuck in an infinite loop**. **It can also help identify [deadlocks](https://www.baeldung.com/java-deadlock-livelock#deadlock), where multiple threads are waiting for one other to release resources**.

Additionally, it can identify a situation where certain threads aren’t getting enough CPU time. This can help identify performance bottlenecks.


## 2.1 Use Case[](https://www.baeldung.com/java-heap-thread-core-dumps#1-use-case-1)

Here’s an example program that can potentially have a slow performance due to a long-running task:

```java
public class ThreadDump {
    public static void main(String[] args) {
        longRunningTask();
    }
    
    private static void longRunningTask() {
        for (int i = 0; i < Integer.MAX_VALUE; i++) {
            if (Thread.currentThread().isInterrupted()) {
                System.out.println("Interrupted!");
                break;
            }
            System.out.println(i);
        }
    }
}
```

In the sample code above, we create a method that loops through to _Integer.MAX_VALUE_ and outputs the value to the console. **This is a long-running operation and will potentially be a performance issue**.

**To analyze the performance, we can capture the thread dump**. First, let’s find the process ID of all running Java programs:

```java
$ jps
```

The _jps_ command outputs all Java processes to the console:

```java
3042 ThreadDump
964 Main
3032 Launcher
3119 Jps
```

We have an interest in the _ThreadDump_ process ID. Next, **let’s use the [_jstack_](https://www.baeldung.com/java-thread-dump#1-jstack) command with the process ID to take the thread dump**:

```bash
$ jstack -l 3042 > slow-running-task-thread-dump.txt
```

The command above captures the thread dump and saves it in a _txt_ file for further [analysis](https://www.baeldung.com/java-analyze-thread-dumps).


# 3 Core Dump[](https://www.baeldung.com/java-heap-thread-core-dumps#core-dump)

**The core dump, also known as the crash dump, contains the snapshot of a program when the program crashed or abruptly terminated**.

The JVM runs bytecode and not native code. Hence, Java code cannot cause core dumps.

However, some Java programs use [Java Native Interface (JNI)](https://www.baeldung.com/jni) to run native code directly. It’s possible for the JNI to crash the JVM because external libraries can crash. We can take the core dump at that instant and analyze it.

Furthermore, **a core dump is an OS-level dump and can be used to find the details of native calls when a JVM crashes**.

## 3.1 Use Case[](https://www.baeldung.com/java-heap-thread-core-dumps#1-use-case-2)

Let’s see an example that generates a core dump using JNI.

First, let’s create a class named _CoreDump_ to load a native library:

```java
public class CoreDump {
    private native void core();
    public static void main(String[] args) {
        new CoreDump().core();
    }
    static {
        System.loadLibrary("nativelib");
    }
}
```

Next, let’s compile the Java code using the _javac_ command:

```ruby
$ CoreDump.java
```

Then, let’s generate a header for native method implementation by running the _javac -h_ command:

```bash
$ javac -h . CoreDump.java
```

Finally, let’s implement a native method in C that will crash the JVM:

```c
#include <jni.h>
#include "CoreDump.h"
    
void core() {
    int *p = NULL;
    *p = 0;
}
JNIEXPORT void JNICALL Java_CoreDump_core (JNIEnv *env, jobject obj) {
    core();
};
void main() {
}
```

Let’s compile the native code by running the _gcc_ command:

```bash
$ gcc -fPIC -I"/usr/lib/jvm/java-17-graalvm/include" -I"/usr/lib/jvm/java-17-graalvm/include/linux" -shared -o libnativelib.so CoreDump.c
```

This generates shared libraries named _libnativelib.so_. Next, let’s compile the Java code with the shared libraries:

```bash
$ java -Djava.library.path=. CoreDump
```

**The native method crashed the JVM and generated a core dump in the project directory:**

```bash
// ...
# A fatal error has been detected by the Java Runtime Environment:
# SIGSEGV (0xb) at pc=0x00007f9c48878119, pid=65743, tid=65744
# C  [libnativelib.so+0x1119]  core+0x10
# Core dump will be written. Default location: Core dumps may be processed with 
# "/usr/lib/systemd/systemd-coredump %P %u %g %s %t %c %h" (or dumping to /core-java-perf/core.65743)
# An error report file with more information is saved as:
# ~/core-java-perf/hs_err_pid65743.log
// ...
```

The above output shows the crash information and the location of the dump file.


## 3.2 Key Differences[](https://www.baeldung.com/java-heap-thread-core-dumps#key-differences)

Here’s a summary table showing the key differences between three types of Java dump files:

| Dump Type   | Use Case                                                              | Contains                                 |
| ----------- | --------------------------------------------------------------------- | ---------------------------------------- |
| Heap Dump   | Diagnose memory issues like _OutOfMemoryError_                        | Snapshot of objects in the Java heap     |
| Thread Dump | Troubleshoot performance issues, thread deadlocks, and infinite loops | Snapshot of all thread states in the JVM |
| Core Dump   | Debug crashes caused by native libraries                              | Process state when JVM crashes           |
