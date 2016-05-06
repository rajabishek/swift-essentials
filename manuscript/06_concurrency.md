# Concurrency in Swift

This is an important concept that no developer can skip. It is a powerful feature that lets developers write efficient, fast, and responsive applications. But with great power comes great responsibility. But once you understand the inner workings and dive a little deeper it becomes very easy to implement.

In iOS we have multiple APIs that can be used to achieve concurrency. One is NSOperation and its associated classes and the other one is Grand Central Dispatch (GCD).

## Where is concurrency used ?
Have you noticed that whenever we do some kind of a heavy tasks ( like database calls, fetching data from network, drawing graphics on screen) out UI is no longer responsive. The reason is that we are executing these heavy tasks on the main queue. And iOS internally uses this main queue to respond to UI events. Therefore till out heavy task in completed iOS is not able to respond to user's actions.

This is where concurrency comes handy. We can perform all the heavy tasks concurrently. Now concurrency doesn't mean parallelism. This is a very important concept to understand.
If you want to delve more deeply into this subject, check out this excellent [talk by Rob Pike](http://vimeo.com/49718712).
