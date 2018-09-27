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
