# Introduction to the Foreign Function & Memory API

## Introduction


This lab introduce yout to the Foreign Function & Memory API which is a Preview Featture in Java 19.

Estimated Time: ~8 minutes



### **About Panama's Foreign Function & Memory API**

The Foreign Function & Memory (FFM) API introduces an API by which Java programs can interoperate with code and data outside of the Java runtime. The FFM API enables (1) to efficiently invoke **foreign functions** (i.e., code outside the JVM), and (2) to safely access **foreign memory** (i.e., memory not managed by the JVM). In practice, the FFM API defines classes and interfaces so that client code can

   * Allocate foreign memory (`MemorySegment`, `MemoryAddress`, and `SegmentAllocator`),
   * manipulate and access structured foreign memory (`MemoryLayout`, `VarHandle`),
   * control the allocation and deallocation of foreign memory (`MemorySession`), and
   * call foreign functions (`Linker`, `FunctionDescriptor`, and `SymbolLookup`).


### **Objectives**


In this workshop, you will:
* Get an overview of the FFM API and some of its key concepts.
* Get a brief introduction to Method Handle and Var Handle, and Preview Feature.

💡 	**Note:** This part of the workshop is theoretical as it introduces and explains some key concepts required to better understand the hands-on exercises.

## Task 1: Foreign Function & Memory API - Overview and Key Concepts

   At a high level, the steps required to invoke, from Java code, a foreign function are
   
   1. Find the target *foreign function* (in a C library)
   
   To find the target foreign function, the FFM API uses a *Linker*. A Linker is a bridge between two binary interfaces: the Java Virtual Machine and C ABIs (Application Binary Interface).
   
   2. Allocate *foreign memory*
   
   The Java client code needs to allocate, using the FFM API, the foreign memory, i.e. off-heap memory, required for proper operation of the native function.
   
   3. Obtain a method handle of the target foreign function, and invoke it

   To perform a downcall, the FFM API needs to know which native function to invoke. This is achieved with a *FunctionDescriptor* which describes the signature of the C function to invoke, i.e. its parameter types and its return type. To model those C types from Java, the FFM API uses different *MemoryLayout* objects.

   The native function will be invoked via a corresponding Method Handle. This Method Handle is created using the corresponding FunctionDescriptor.
   
   

💡 	**Note:** This section above describes, at a high-level, how a *downcall* (Java code calling native code) is performed but the Linker interoperates in both-direction and also enables *upcall* (native code calling Java code). Upcalls will be discussed in a later section.


## Task 2: Method Handle & Var Handle Overview

Up to this point, we have introduced new concepts introduced by Foreign Function & Memory API. We now should briefly discuss about Method Handle and Var Handle as they are leveraged by the FFM API. **TODO**

## Task 3: Preview Feature

To conclude this section, we need to discuss Preview Feature as you will be using Java 19. **TODO - Preview Features**



## Learn More


* [java.lang.foreign](https://download.java.net/java/early_access/jdk19/docs/api/java.base/java/lang/foreign/package-summary.html) Javadoc **TODO update to GA links**

## Acknowledgements
* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - David Delabassée, Sept. 4 2022
