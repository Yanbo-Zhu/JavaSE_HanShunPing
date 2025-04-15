
# 1 Concurrency 



# 2 Processes and threads


Each process has its own memory space and has at least one thread.
• All threads within a process share the same memory space (i.e., they have access to the same
variables).

For Java:
• It is possible to create new processes (see class ProcessBuilder), but the typical scenario will
be the Java VM as one process and multiple threads within that process.
• When you start a Java program, the Java VM creates the default thread “main“ and uses it to
execute the specified main method.
• During execution, often new threads are created – either explicitly by your program or implicitly
by using certain classes*.



runable mit return value in type long 


# 3 Sleeping and interruptions


用 interrupt 去唤醒一个 sleep 的 thread 

isInterrupted ist eine boolean flag in jede Thread 

A thread can go to sleep for a specified period of time by invoking Thread.sleep(long).
• As a thread may be interrupted (i.e., politely be asked to terminate), the compiler forces us to
catch an InterruptedException. This exception is thrown whenever someone invokes the
interrupt() method on our thread while our thread is sleeping.
• Important: Catching this exception resets the interrupted flag which we can also query using
isInterrupted(). To avoid this, we should call interrupt() in the catch block agai





