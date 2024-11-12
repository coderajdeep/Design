## Basics of Threads in Java

### Creating Threads
In Java, threads can be created in two primary ways: by extending the `Thread` class or by implementing the `Runnable` interface.

1. **Extending the Thread Class**:
   - You can create a new thread by extending the `Thread` class and overriding its `run()` method.
   ```java
   class MyThread extends Thread {
       public void run() {
           System.out.println("Thread is running");
       }
   }

   MyThread thread = new MyThread();
   thread.start(); // Starts the thread
   ```

2. **Implementing the Runnable Interface**:
   - Alternatively, you can implement the `Runnable` interface and pass an instance of it to a `Thread` object.
   ```java
   class MyRunnable implements Runnable {
       public void run() {
           System.out.println("Runnable thread is running");
       }
   }

   Thread thread = new Thread(new MyRunnable());
   thread.start(); // Starts the thread
   ```

### Thread Lifecycle
The lifecycle of a thread in Java consists of several states, each representing a different phase in a thread's execution:

1. **New**: The thread is created but not yet started. It remains in this state until the `start()` method is invoked.
2. **Runnable**: The thread is ready to run and waiting for CPU time. It may be actively running or waiting to be scheduled.
3. **Blocked**: The thread is blocked and waiting to acquire a lock on an object to enter a synchronized block or method.
4. **Waiting**: The thread is waiting indefinitely for another thread to perform a particular action (e.g., using `wait()`, `join()`).
5. **Timed Waiting**: The thread is waiting for another thread to perform an action for a specified waiting time (e.g., using `sleep(millis)`, `wait(millis)`).
6. **Terminated**: The thread has completed its execution and cannot be restarted.

### Thread Priority
Java allows you to set the priority of threads, which can influence their scheduling. Thread priorities range from `Thread.MIN_PRIORITY` (1) to `Thread.MAX_PRIORITY` (10), with `Thread.NORM_PRIORITY` (5) as the default. However, priority does not guarantee that higher-priority threads will always execute before lower-priority ones.

```java
Thread highPriorityThread = new Thread(new MyRunnable());
highPriorityThread.setPriority(Thread.MAX_PRIORITY);
```

### Synchronization & Thread Safety
Synchronization is crucial in multithreading to prevent data inconsistency when multiple threads access shared resources.

#### Synchronized Methods
You can declare a method as synchronized to ensure that only one thread can execute it at a time for a given object.

```java
public synchronized void synchronizedMethod() {
    // Critical section code
}
```

#### Synchronized Blocks
For finer control over synchronization, you can use synchronized blocks within methods. This allows you to lock only specific parts of your code.

```java
public void methodWithBlock() {
    synchronized(this) {
        // Critical section code
    }
}
```

### Volatile Keyword
The `volatile` keyword in Java is used to indicate that a variable's value will be modified by different threads. Declaring a variable as volatile ensures that its value is always read from the main memory and not from a local cache, thus providing visibility guarantees across threads.

```java
private volatile boolean flag = false;
```

### Conclusion
Understanding the basics of threads in Java, including how to create them, their lifecycle states, synchronization mechanisms, and the use of keywords like `volatile`, is essential for developing robust multithreaded applications. Properly managing threads enhances performance and responsiveness while ensuring data integrity in concurrent environments.

Citations:
[1] https://www.javatpoint.com/life-cycle-of-a-thread
[2] https://en.wikipedia.org/wiki/Hardware_thread
[3] https://www.upwork.com/resources/what-is-multithreading
[4] https://www.javatpoint.com/multithreading-in-java
[5] https://www.pcmag.com/encyclopedia/term/multithreading
[6] https://totalview.io/blog/multithreading-multithreaded-applications
[7] https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html
[8] https://www.scaler.com/topics/difference-between-hashmap-and-hashtable/