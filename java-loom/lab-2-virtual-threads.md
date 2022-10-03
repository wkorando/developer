# Exploring Virtual Threads

## Introduction


This lab guides you through Virtual Threads and related APIs.

Estimated Time: 25 minutes


### Objectives

In this lab, you will:
* Create virtual threads
* Join virtual threads to platform threads
* Yield virtual threads

### Prerequisites (Optional)

If you haven't already, clone the repository and change your directory to the base of the repository: 

```
git clone TODO
cd TODO
```

Move to the `org.paumard.loom.virtualthread` package, which contains the code for completing this lab. 


Virtual Threads are a preview feature in JDK 19. You will need to enable preview features at compile and runtime explicitly. The below command demonstrates how to compile with preview features: 

```
javac --enable-preview --release 19 Main.java
```

Running code with preview features would look like this:
```
java --enable-preview Main
```

You can learn more about preview features here: [https://dev.java/learn/using-the-preview-features-available-in-the-jdk/](https://dev.java/learn/using-the-preview-features-available-in-the-jdk/).


## Task 1: Version Check

Open `A_VersionCheck`.

This first class is just there to ensure that you are running the correct version of Java. Also, make sure that you have enabled preview features. You need to run this class.


## Task 2: Starting Threads

Open `B_StartingThreads`.

You can just run this class and see what it prints. The main thread is a platform thread. What is the number of this thread?


## Task 3: Starting Virtual Threads

Open `C_StartingThreads`.

Let us now see how to create and launch virtual threads.

First, Loom brings a new pattern to start platform threads. Platform threads and virtual threads are running tasks, instances of `Runnable`.

So let us create a first task that prints the current thread. The current thread is the thread that is running this task.

Loom also brings new patterns to create and launch platform threads. You need to call `join()` to ensure that this thread has enough time to run. Check the code of this class to see this pattern.

Then try to find a similar pattern to start a virtual thread. Create a new virtual thread with the name "virtual", identical to the previous platform thread, and start it.

Do not forget to call `join()` on this virtual thread. You need to configure your IDE so that the preview features of your JDK 19 are enabled.

How can you tell the thread you created is a virtual thread?

What is platform thread used to run this virtual thread?

You can also explore these two patterns and see how you can customize the threads you are launching.

## Task 4: Yielding Virtual Threads

Open `D_YieldingVirtualThreads`.

Let us now create a bunch of virtual threads and see how they can jump from one platform thread to another. This is a feature that is unique to Loom virtual threads.

You need to create an unstarted virtual thread in the code that does the following:

* If the index is 0, then print the current thread,
* then go to sleep for a few milliseconds; 20 is enough.
* If the index is 0, then print the current thread again,
* then go to sleep again,
* If the index is 0, print the current thread one last time.

What platform thread is running your virtual thread?

Can you see your virtual thread jumping from one platform thread to the other?

Does blocking a virtual thread block a platform thread?

Try to decrease the number of virtual threads you are creating. Do you still see this behavior?

Because blocking a Loom virtual thread is so cheap, trying to pool them becomes useless. 

## Task 5: How Many Platform Threads

Open `E_HowManyPlatformThreads`.

Let us discover how many platform threads you need to run your virtual threads.

There are two concurrent sets created in this class, one to store the name of the pooled platform threads used by default and the other to store the name of the platform threads.

You can paste the code from the previous exercise here. Then, you can call the two methods `readThreadPoolName()` and `readPlatformThreadName()` and add the pool name and the platform thread name in the corresponding sets.

How many pools is Loom using?

How many platform threads have been used for your virtual threads?

You can try to increase the number of virtual threads to see if this number varies. You can also try to run this code on a machine with a different number of cores to see how this number changes.

## Learn More

* [Virtual Threads - JEP 425](https://openjdk.org/jeps/425)

## Acknowledgements
* **Author** - José Paumard, Java Developer Advocate, Java Platform Group
* **Contributors** -  Billy Korando, Java Developer Advocate, Java Platform Group
* **Last Updated By/Date** - Billy Korando, September 2022
