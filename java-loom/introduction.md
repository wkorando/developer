# Introduction

This lab covers two aspects of the Loom project: virtual threads and structured concurrency.

You need a JDK 19 to run it. Virtual threads are a preview feature of the JDK 19, so you need to enable preview features both at compile time and at run time.

## About this Workshop

This lab is about exploring Loom and what this project is bringing to the JDK. It focuses on two features of Loom: Virtual threads ([https://openjdk.org/jeps/425](https://openjdk.org/jeps/425)) and Structure Concurrency ([https://openjdk.org/jeps/428](https://openjdk.org/jeps/428)). These features are still preview features or incubator features, meaning that there are available for testing and evaluation, but are subject to change before making as final features. 

What can you expect from this lab?
1. a good understanding of what a virtual threads are, how you can launch and create them, and what do they bring to the concurrent programming model.
2. Organize our asynchronous tasks using the Structured Concurrency API, and more precisely the `StructuredTaskScope` class. 
   1. you will launch tasks using instances of the `StructuredTaskScope` class, and get the results from them,
   2. you will handle time-outs,
   3. you will override this class to implement your own business logic.

### Objectives

In this workshop, you will learn how to:
* Become familiar with Virtual Threads
* Become familiar with Structured Concurrency


### Prerequisites (Optional)

This lab assumes you have:
* An Oracle account
* All previous labs successfully completed

## Learn More

* [Java 19 Virtual Threads - JEP Café #11](https://www.youtube.com/watch?v=lKSSBvRDmTg)
* [Launching 10 millions virtual threads with Loom - JEP Café #12](https://www.youtube.com/watch?v=UVoGE0GZZPI)
* [Java Asynchronous Programming Full Tutorial with Loom and Structured Concurrency - JEP Café #13](https://www.youtube.com/watch?v=2nOj8MKHvmw)

## Acknowledgements
* **Author** - José Paumard, Java Developer Advocate, Java Platform Group
* **Contributors** -  Billy Korando, Java Developer Advocate, Java Platform Group
* **Last Updated By/Date** - Billy Korando, September 2022
