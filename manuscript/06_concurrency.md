# Concurrency in Swift

This is an important concept that no developer can skip. It is a powerful feature that lets developers write efficient, fast, and responsive applications. With great power comes great responsibility. But once you understand the inner workings and dive a little deeper it becomes very easy to implement.

In iOS we have multiple APIs that can be used to achieve concurrency. One is NSOperation & its associated classes and the other one is Grand Central Dispatch (GCD).

## Where is concurrency used ?
Have you noticed that whenever we do some kind of a heavy tasks ( like database calls, fetching data from network, drawing graphics on screen) the UI is no longer responsive. The reason is that we are executing these heavy tasks on the main queue. iOS internally uses this main queue to respond to UI events. Therefore till out heavy task in completed iOS is not able to respond to user's actions.

This is where concurrency comes in handy. We can perform all the heavy tasks concurrently. Now concurrency doesn't mean parallelism. Both may look similar but they are different concepts to understand.
If you want to delve more deeply into this subject, check out this excellent [talk by Rob Pike](http://vimeo.com/49718712).

## GCD (Grand Central Dispatch)
GCD is the most commonly used API to manage concurrent code and execute operations asynchronously. GCD is the marketing name for libdispatch, Apple’s library that provides support for concurrent code execution on multi core hardware on iOS and OS X.

GCD provides dispatch queues to handle submitted tasks.

## Dispatch Queues
These queues manage the tasks you provide to GCD and execute those tasks in FIFO order. There are two varieties of dispatch queues one is serial and the other is concurrent. Any task that is added to any one of the queues is executed in separate threads than the thread in which they were created on. 

Therefore whenever you want to perform some kind of computationally heavy task you create warp a block of code in a closure ( a task) and submit it to dispatch queues in the main thread. But all these tasks (closures) will run in separate threads instead of the main thread.

## Serial Queues
This type of a queue can execute only one task at a time. The tasks will be executed in the order in which they were added to the queue. If 3 tasks were added to the serial queue then task 2 will wait for task 1 to complete and before its starts its execution. Similarly task 3 will wait for task 2 to complete before it starts. 

A task in a serial queue will wait only for the preceding task in the same queue. It will not wait or depend on any other task that is present in a different queue. Therefore we can still execute tasks concurrently if we create multiple serial queues.
If lets say we create 3 serial queues and load them with tasks, then each queue executes only one task at a time, but we can have 3 tasks that are executing simultaneously one from each serial queue.

If we are having multiple tasks that share the same piece of data, executing them simultaneously will result in weird errors.This is called as race condition. Serial queues can help us solve this problem. By adding such tasks to a serial queue it is ensured that the tasks are executed sequentially only one at a time.

Some benefits of using a serial queue are:
- Sequential execution one at a time to avoid race condition of tasks accessing a shared resource
- Tasks are executed in the same order as they are added in the queue
- Multiple serial queues can be created

## Concurrent Queues
Current queues allows you to execute multiple tasks concurrently. The tasks that are added to the queue become ready for execution in the same order in which they are added. Unlike serial queues a task need to wait for the previous task to complete its execution before it starts. 

Concurrent queues guarantee that tasks start in same order but you will not know the order of completion, execution time or the number of tasks being executed at a given point.

Lets say 3 tasks are added to the concurrent queue. Now task 1 becomes ready and its starts executing. Lets say that task 1 is a very heavy task and takes a very long time to complete. While task 1 is still running task 2 can start, after task 2 starts task 3 will start. Now before task 1 completes task 2 and task 3 can complete executing. Therefore only the order in which the tasks start executing is guaranteed and not anything else.

Some benefits of using a concurrent queue are:
- Tasks start executing in the same order as they are added in the queue
- Multiple concurrent queues can be created
- Apple provides four concurrent queues called as global dispatch queues
- Tasks need not wait for the previous task to complete its execution before it starts executing

## Dispatch queues in action
Now that we have seen both the type of queues. Its time to see on how we can put then to use. By default Apple provides 1 serial queue and 4 concurrent queues. The single serial queue is the main queue that executes the tasks on the applications main thread. It should be used to update the application's UI and all tasks that make a change in the user interface. Since its a serial queue only one task can be execute at a time.

Besides the main queue, we have 4 concurrent queues, which are called the global dispatch queues. Each one of this queue has a different priority assigned to it. Before we use any one of them we should get a reference using the `dispatch_get_global_queue` function. The first parameter is priority and the second parameter is 0. The priority can be any one of the following.
- `DISPATCH_QUEUE_PRIORITY_HIGH`
- `DISPATCH_QUEUE_PRIORITY_DEFAULT`
- `DISPATCH_QUEUE_PRIORITY_LOW`
- `DISPATCH_QUEUE_PRIORITY_BACKGROUND`

The list is in the descending order of priority. Once we add tasks to any one of these 4 queues, lets say 2 tasks are ready for execution one from queue with `DISPATCH_QUEUE_PRIORITY_HIGH` priority and other from queue with `DISPATCH_QUEUE_PRIORITY_BACKGROUND` priority. The task from the queue with higher priority will be given preference first.

These constants were a part of Objective-C. In Swift we have the Quality of Service (QoS) class. The QoS classes are meant to express the intent of the submitted task so that GCD can determine how to best prioritize it.
- `QOS_CLASS_USER_INTERACTIVE`: The user interactive class represents tasks that need to be done immediately in order to provide a nice user experience. Use it for UI updates, event handling and small workloads that require low latency. The total amount of work done in this class during the execution of your app should be small.
- `QOS_CLASS_USER_INITIATED`: The user initiated class represents tasks that are initiated from the UI and can be performed asynchronously. It should be used when the user is waiting for immediate results, and for tasks required to continue user interaction.
- `QOS_CLASS_UTILITY`: The utility class represents long-running tasks, typically with a user-visible progress indicator. Use it for computations, I/O, networking, continuous data feeds and similar tasks. This class is designed to be energy efficient.

- `QOS_CLASS_BACKGROUND`: The background class represents tasks that the user is not directly aware of. Use it for prefetching, maintenance, and other tasks that don’t require user interaction and aren’t time-sensitive.

Instead of using the constants we can use the quality of service class. We get the priority value from this class using the static value attribute of the class. 

So you can decide the queue you use based on the priority of the task. Please also note that these queues are being used by Apple’s APIs so your tasks are not the only tasks in these queues.

We can create any number of serial queues and concurrent queues as we require. It is highly recommended that in case of concurrent queues we use any one of the default 4 queues available, instead of creating a new one.

## Lets get our hands dirty
```swift
//Get a reference to the main queue
let mainQueue = dispatch_get_main_queue()

//Get a reference to a global dispatch queue
//First parameter is the priority and second is always 0
let queue = dispatch_get_global_queue(Int(QOS_CLASS_UTILITY.value), 0)

//Creating our own serial queue
let serialQueue = dispatch_queue_create("com.app.serialQueue", DISPATCH_QUEUE_SERIAL)

//Creating our own concurrent queue
let serialQueue = dispatch_queue_create("com.app.concurrentQueue", DISPATCH_QUEUE_CONCURRENT)


//Adding a task to the queue
dispatch_async(queue, { () -> Void in
	//You're task is written here
})
```

