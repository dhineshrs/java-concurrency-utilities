# java-concurrency-utilities

### CountDownLatch in Java
**CountDownLatch** is used to **make sure that a task waits for other threads before it starts**. To understand its application, let us consider a server where the main task can only start when all the required services have started.

#### Working of CountDownLatch
When we create an object of CountDownLatch, we specify the number of threads it should wait for, all such thread are required to do count down by calling CountDownLatch.countDown() once they are completed or ready to the job. As soon as count reaches zero, the waiting task starts running.

**Facts about CountDownLatch:**
- CountDownLatch works in **latch principle**
- Creating an object of CountDownLatch by passing an int to its constructor (the count), is actually number of invited parties (threads) for an event.
- The thread, which is dependent on other threads to start processing, waits on until every other thread has called count down. All threads, which are waiting on await() proceed together once count down reaches to zero.
- countDown() method decrements the count and await() method blocks until count == 0

- the main thread will wait until the gate is open. One thread waits for n threads, specified while creating the CountDownLatch.
- **Use CountDownLatch** when one thread (like the main thread) requires to wait for one or more threads to complete, before it can continue processing.
- A **classical example** of using CountDownLatch in Java is a server side core Java application which uses services architecture, where *multiple services are provided by multiple threads and the application cannot start processing until all services have started successfully*.
- One **disadvantage of CountDownLatch** is that **its not reusable once count reaches to zero** you can not use CountDownLatch any more, but don't worry Java concurrency API has another concurrent utility called **CyclicBarrier** for such requirements.

### CyclicBarrier in Java
The java.util.concurrent.CyclicBarrier class is a synchronization mechanism that can synchronize threads progressing through some algorithm. In other words, it is a barrier that all threads must wait at, until all threads reach it, before any of the threads can continue. 


Two threads waiting for each other at CyclicBarriers.

The threads wait for each other by calling the await() method on the CyclicBarrier. Once N threads are waiting at the CyclicBarrier, all threads are released and can continue running.

**Creating a CyclicBarrier** When you create a CyclicBarrier you specify how many threads are to wait at it, before releasing them. Here is how you create a CyclicBarrier:
**CyclicBarrier barrier = new CyclicBarrier(2);**

**Waiting at a CyclicBarrier**
Here is how a thread waits at a CyclicBarrier:
**barrier.await();**

We can also specify a timeout for the waiting thread. When the timeout has passed the thread is also released, even if not all N threads are waiting at the CyclicBarrier. Here is how you specify a timeout:
**barrier.await(10, TimeUnit.SECONDS);**

**The waiting threads waits at the CyclicBarrier until either:**
- The last thread arrives (calls await() )
- The thread is interrupted by another thread (another thread calls its interrupt() method)
- Another waiting thread is interrupted
- Another waiting thread times out while waiting at the CyclicBarrier
- The CyclicBarrier.reset() method is called by some external thread.

**CyclicBarrier is used to make threads wait for each other.** It is **used when** *different threads process a part of computation and when all threads have completed the execution,* **the result needs to be combined in the parent thread**. In other words, a CyclicBarrier is used when **multiple thread carry out different sub tasks and the output of these sub tasks need to be combined to form the final output**. After completing its execution, threads call await() method and wait for other threads to reach the barrier. Once all the threads have reached, the barriers then give the way for threads to proceed.
![cyclicbarrier](https://user-images.githubusercontent.com/36996525/46131166-9bb45f00-c258-11e8-89de-0e98d4e07c16.png)

Once the number of threads that called await() equals numberOfThreads, the barrier then gives a way for the waiting threads. The CyclicBarrier can also be initialized with some action that is performed once all the threads have reached the barrier. This action can combine/utilize the result of computation of individual thread waiting in the barrier.
 <br /> Runnable action = ... 
 <br /> //action to be performed when all threads reach the barrier;
 <br /> CyclicBarrier newBarrier = new CyclicBarrier(numberOfThreads, action);

**Important Methods of CyclicBarrier:**
 <br /> **2. getParties**: Returns the number of parties required to trip this barrier.
  <br />            Syntax:  public int **getParties()**
  <br />            Returns: the number of parties required to trip this barrier
  
 <br /> **2. reset**: Resets the barrier to its initial state.
  <br />            Syntax:  public void **reset()**
  <br />            Returns: void but resets the barrier to its initial state. If any parties are currently waiting at the barrier, they   <br />                     will return with a BrokenBarrierException.

 <br /> **3. isBroken**: Queries if this barrier is in a broken state.
  <br />            Syntax:  public boolean **isBroken()**

 <br /> **4. getNumberWaiting**:  Returns the number of parties currently waiting at the barrier.
  <br />            Syntax:  public int **getNumberWaiting()**
  <br />            Returns: the number of parties currently blocked in await().
  
   <br /> **5. await**:  Waits until all parties have invoked await on this barrier.
  <br />            Syntax:  public int **await() throws InterruptedException, BrokenBarrierException**
  <br />            Returns: the arrival index of the current thread, where index getParties() – 1 indicates the first to arrive and   <br />                       zero indicates the last to arrive.

**Difference between a CyclicBarrier and a CountDownLatch**
- A CountDownLatch can be used only once in a program(until it’s count reaches 0).
- A CyclicBarrier can be used again and again once all the threads in a barriers is released.

### Semaphore in Java
A **semaphore controls access to a shared resource through the use of a counter**. If the counter is greater than zero, then access is allowed. If it is zero, then access is denied. What the counter is counting are permits that allow access to the shared resource. Thus, to access the resource, a thread must be granted a permit from the semaphore.

#### Working of semaphore
In general, to use a semaphore, the thread that wants access to the shared resource tries to acquire a permit.
- If the semaphore’s count is greater than zero, then the thread acquires a permit, which causes the semaphore’s count to be decremented.
- Otherwise, the thread will be blocked until a permit can be acquired.
- When the thread no longer needs an access to the shared resource, it releases the permit, which causes the semaphore’s count to be incremented.
- If there is another thread waiting for a permit, then that thread will acquire a permit at that time.

