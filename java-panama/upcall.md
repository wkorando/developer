#  Upcalls

## Introduction


This lab introduces you to upcalls, i.e. a call from native code back to Java code. More specifically, it enables you to pass Java code as a function pointer to a foreign function.


Estimated Time: 15 minutes

## Task 1: Short Pointer Primer

There are 2 ways to pass parameters to a function in C: (1) by value, and (2) by reference, i.e. using a pointer. For instance, the C `puts` function accepts as parameter a pointer to a _char_ array. Given that Java does not expose pointers to developers, the concept of pointer is obscure if not unknown to most Java developers. In short, a pointer is "simply" a variable whose value is the address of another variable.

```c
      int puts(const char* s);
```

💡 The asterisk * is used to declare a pointer. 

Native C types such as _int_, _char_, _struct_, etc., can be accessed through pointers. In addition to pointing to some data, a pointer can also point to C function. A function pointer is a variable that stores the address of a function, function that can later be invoked through this function pointer.


```c
      <copy>
      #include <stdio.h>

      int main() {
          int (*putsPtr) (const char*);
          putsPtr = &puts;
          putsPtr("Hello-hello! Long time no see!");

          return 0;
      }
      </copy>
```

Let's look  more closely at the snippet above. It starts with a pointer declaration. This pointer is named `putsPtr` and points to a function that accepts `const char*` as parameter, and returns an `int` value. 
```c
int (*putsPtr) (const char*);
```
This a just a pointer declaration. `putsPtr` is only bound to a specific funtion, i.e. to the 'puts' function of the C stdio library in this case, during the assignment. 

```c
putsPtr = &puts;
```

Following this pattern, a pointer to the C `printf` function would have the following form.

```c
      <copy>
      #include <stdio.h>

      int main() {
          int (*printfPtr) (const char*, ...);
          printfPtr = &printf;
          printfPtr("The answer is %d", 42);

          return 0;
      }
      </copy>
```

Note that `printfPtr` has a variadic arguments definition as denoted by the `...` agrument.

## Task 2: Invoking a Java method from a C function

1. Create a callback C function

This simple function (`callback_function`) will accept a pointer to another function that doesn't return something, i.e. `void()`: `*ptrToFunction()`

Create a C source file named `upcall.c` with the following content.

```c
      <copy>
      #include <stdio.h>

      void callback_function(void (*ptrToFunction)()) {

         puts("Foreign\t start of callback_function()");

         // invoke the function via the pointer passed as parameter
         (*ptrToFunction)();

         puts("Foreign\t end of callback_function()");

      }
      </copy>
```  

💡 The C [`puts`](https://cplusplus.com/reference/cstdio/puts/) function accepts a pointer to a string and displays this string on the standard output. It is more or less the equivalent of the Java `println` method. 


2. Create a shared library

Compile your C code using `gcc`.

```text
> <copy>gcc -fPIC -shared -o ~/mylib.so upcall.c</copy>
```

This will generate a shared library that hosts the C `callback_function` function that accepts, as parameter, a pointer pointing to a function with the `void()` signature.

```text 
> <copy>file mylib.so</copy>

mylib.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=c45a…eb5, not stripped
```

3. Create the Java application

Create an Upcall.java application with a simple `callback()` method.

```java
      <copy>
      public class Upcall {

          static void callback() {
              System.out.println("Java\t callback() method");
          }

          public static void main(String[] args) throws Throwable {

          System.out.println("Java\t main() method");

          System.out.println("Java\t main() method exit");

          }

      }</copy>
```


3. Setup the Java callback

Define a method handle for the `void callback()` method.

```java
      …
      public class Upcall {

      <copy>
      private static final MethodHandle callbackHandle;

      static {

         System.load(System.getProperty("lib.path"));

         try {
             // creat a Method Handle for the Upcall#callback method.
             callbackHandle = MethodHandles
                 .lookup()
                 .findStatic(
                    Upcall.class,
                    "callback",
                    MethodType.methodType(void.class)
                 );
         } catch (IllegalAccessException | NoSuchMethodException e) {
             throw new RuntimeException(e);
         }

      }</copy>
…
```

** Function Descriptor **

```java
      …
      <copy>
      static final FunctionDescriptor callbackDescriptor = FunctionDescriptor.ofVoid();
  </copy>
```

4. Setup the Linker


```java
      …
      public class Task {
      …
      <copy>
      static final Linker linker = Linker.nativeLinker();

      private static final SymbolLookup
          linkerLookup = linker.defaultLookup();

      private static final SymbolLookup
          systemLookup = SymbolLookup.loaderLookup();

      private static final SymbolLookup
          symbolLookup = name ->
             systemLookup.lookup(name)
                         .or(() -> linkerLookup.lookup(name));
      </copy>
      …
```


5. Create...


```java
      …
      <copy>
      static final MethodHandle nativeFunctionWithCallback
         = symbolLookup.lookup("callback_function")
              .map(add ->
                  linker.downcallHandle(add, FunctionDescriptor.ofVoid(ADDRESS))
               ).orElseThrow();
      </copy>
      …
```

6. Invoke the 


💡 A **stub** is a consumer proxy object which is communicating with a `foreign` provider.


To invoke the callback via its method handle, update the main() method as follow.

```java
      System.out.println("Java\t main() method");

      <copy>
      var ms = MemorySession.openConfined();
      var callbackNativeSymbolSegment = linker.upcallStub(
                  callbackHandle, callbackDescriptor, ms);

       nativeFunctionWithCallback.invokeExact((Addressable) callbackNativeSymbolSegment);
      </copy>

      System.out.println("Java\t main() method exit");
```

java -Dlib.path=$PWD/lib.platform --enable-native-access=ALL-UNNAMED --enable-preview --source 19 Upcall.java


7. Test the upcall

To compile the code, you have to fix the `import`s as follow, and update the signature of the `main` method to throw a `Throwable`.

```java
      <copy>
      import java.lang.foreign.*;
      import java.lang.invoke.*;
      import static java.lang.foreign.ValueLayout.ADDRESS;
      </copy>

      …

      public static void main(String[] args) throws Throwable {
      …
  
```

You can now compile and run the code.

```java
      <copy>
      java -Dlib.path=$PWD/lib.platform --enable-native-access=ALL-UNNAMED --enable-preview --source 19 Upcall.java
      </copy>
````

You will then see the invocation chain, i.e. from Java to C (downcall), C to Java (upcall), and then back to Java.

```text
Java     main() method
Foreign  start of callback_function()
Java     callback() method
Foreign  end of callback_function()
Java     main() method exit
```


## Conclusion

💡 In the spirit of showing how things work, those examples sometimes take a few shortcuts. For example, potential errores aren't always handled properly, arguments are not validated, etc. Those and other aspects such as security, synchronization, etc. are matters that should always be properly handled in any code that will go into production but that is not the focus of this lab.


## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - David Delabassée, Sept. 27 2022