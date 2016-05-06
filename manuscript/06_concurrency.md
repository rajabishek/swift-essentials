# Concurrency in Swift

This is an important concept that no developer can skip. It is a powerful feature that lets developers write efficient, fast, and responsive applications. But with great power comes great responsibility. But once you understand the inner workings and dive a little deeper it becomes very easy to implement.

In iOS we have multiple APIs that can be used to achieve concurrency. One is NSOperation and its associated classes and the other one is Grand Central Dispatch (GCD).

## Where is concurrency used ?
Have you noticed that whenever we do some kind of a heavy tasks ( like database calls, fetching data from network, drawing graphics on screen) out UI is no longer responsive. The reason is that we are executing these heavy tasks on the main queue. And iOS internally uses this main queue to respond to UI events. Therefore till out heavy task in completed iOS is not able to respond to user's actions.

This is where concurrency comes handy. We can perform all the heavy tasks concurrently. Now concurrency doesn't mean parallelism. This is a very important concept to understand.
If you want to delve more deeply into this subject, check out this excellent [talk by Rob Pike](http://vimeo.com/49718712).

## GCD (Grand Central Dispatch)
GCD is the most commonly used API to manage concurrent code and execute operations asynchronously. GCD is the marketing name for libdispatch, Apple’s library that provides support for concurrent code execution on multi core hardware on iOS and OS X.

GCD provides dispatch queues to handle submitted tasks.

## Dispatch Queues
These queues manage the tasks you provide to GCD and execute those tasks in FIFO order. There are two varieties of dispatch queues one is serial and the other is concurrent. Any task that is added to any one of the queues are being executed in separate threads than the thread in which they were created on. 

Therefore whenever you want to perform some kind of computationally heavy task you create warp a block of code in a closure ( a task) and submit it to dispatch queues in the main thread. But all these tasks (closures) will run in separate threads instead of the main thread.

## Serial Queues
This type of a queue can execute only one task at a time. The tasks will be executed in the order in which they were added to the queue. If 3 tasks were added to the serial queue then task 2 will wait for task 2 to complete and then it will start its execution. Similarly task 3 will wait for task 2 to complete before it starts. 

A task in a serial queue will wait only for the preceding task in the same queue. It will not wait or depend on any other task that is present in a different queue. Therefore we can still execute tasks concurrently if we create multiple serial queues.
If lets say we create 3 serial queues and load them with tasks, then each queue executes only one task at a time, but we can have 3 tasks that are executing simultaneously one from each serial queue.

If we are having multiple tasks that share the same piece of data, executing them simultaneously will result in wired errors.This is called as race condition. Serial queues can help us solve this problem. By adding such tasks to a serial queue it is ensured that the tasks are executed sequentially only one at a time.

Some benefits of using a serial queue are:
- Sequential execution one at a time to avoid race condition of tasks accessing a shared resource
- Tasks are executed in the same order as they are added in the queue
- Multiple serial queues can be created