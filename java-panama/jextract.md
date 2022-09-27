# Using jextract

## Introduction


This lab walks you through the the jextract tool and show you how to use it.

Estimated Time: 10 minutes


### Objectives

In this lab, you will:
* TODO
* TODO



## Task 1: Jextract overview

The previous labs explained the different steps required to perform downcalls, part of it was to write some code required for native code invocation such as
* Instantiate and configure a Linker.
* Perform a SymbolLookup to locate, using their string-based names, the native symbols of the foreign functions.
* Construct a FunctionDescriptor to describe native functions signatures.
* Create MemoryLayout objects to represent the arguments and the return value of the foreign functions.
* Etc. 

In the end, that is a lot of 'infrastructure' code, i.e. code that is required to perform what we want to do, i.e. invoking a foreign function. A clear benefit of the FFM API is that all this 'infrastructure' code is written in Java, which is not the case with the JNI API for which you have to write some of that 'infrastructure' code using C. 


The situation is getting even more complicated with native [variadic foreign functions](https://en.wikipedia.org/wiki/Variadic_function), i.e. functions accepting a variable number of arguments AKA *varargs* in Java. In the following C example...

```
<copy>
#include <stdio.h>

int main() {
   printf("Hello %s\n", "World!");
   printf("%s %\n", "Hello", " World!");
   return 0;
}
</copy>
```

... the compiler will create a special variant for the 2 `printf` functions:

```
int(const char*);
int(const char*, const char*);
```

This means that you would have to also handle the infrastructure code to deal with those 2 variants.


Writing infrastructure code can sometimes be seen as a necessary distraction. This is one of the problems that Project Panama aims to solve by providing a new code-generating tool - jextract - whose goal is to help developers to generate the infrastructure code around the native code of any C library. Jextract helps developers focus on writing business logic that uses foreign functions, and not on writing the infrastructure code required to use those foreign functions. 

Jextract is a tool, developed under the Project Panama umbrella, whose goal is to mechanically generates, from the corresponding C-header files, Java bindings for native libraries.

Technically, jextract relies on 2 dependencies: (1) the Foreign Function & Memory API, available in JDK19+ and (2) [libclang](https://clang.llvm.org/doxygen/group__CINDEX.html), a C interface to the Clang.

Jextract uses libclang to parse C header files and extract entities from native symbols (`struct`, `typedef`, `macro`, `global constant`, and `function`).

💡 libclang is a C interface to Clang and comes with Clang, a *compiler front-end for C-based languages* (ex. C, C++,  etc.). The Clang compiler front-end is responsible for translating C source code into an intermediate state. Clang will then use LLVM, a compiler back-end, to translate that intermediate state into machine code. 


## Task 2: Installing jextract

Although jextract is developed in the [OpenJDK Code Tools project](https://openjdk.org/projects/code-tools/), it is not per se part of the JDK itself. This is due to several reasons. First, jextract isn't yet finished and is only available, at this stage, via Early-Access builds. Second, not all Java developers write code that needs to interoperate with native code. Third, and probably the main reason, jextract would significantly increase the size of the JDK. For all those reasons, jextract is a separate bundle that needs to be downloaded independently.



Installing jextract is straight forward.

1. Get the jextract Early Access build.

Head to [https://jdk.java.net/jextract/](https://jdk.java.net/jextract/) and download the **Linux / x64** Early-Access builds using `wget`.

```text
> <copy>
wget https://download.java.net/java/early_access/jextract/2/openjdk-19-jextract+2-3_linux-x64_bin.tar.gz -P ~/
</copy>

Resolving download.java.net (download.java.net)... 23.55.248.91
Connecting to download.java.net (download.java.net)|23.55.248.91|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 153788298 (147M) [application/x-gzip]
Saving to: ‘/home/david_dela/openjdk-19-jextract+2-3_linux-x64_bin.tar.gz’

100%[==========================================>] 153,788,298  112MB/s in 1.3s

‘/home/david_dela/openjdk-19-jextract+2-3_linux-x64_bin.tar.gz’ saved [153788298/153788298]
```

2. Untar the archive

```text
> <copy>tar xvf ~/openjdk-19-jextract+2-3_linux-x64_bin.tar.gz
</copy>
   ```

Make sure to update the path to include jextract.

```text
> <copy>export PATH=$PATH:~/jextract-19/bin</copy>
````

💡 If you want the updated path to be persisted, update your `.bashrc` as follow.
```text
> <copy>echo 'export PATH=$PATH:~/jextract-19/bin' >> ~/.bashrc</copy>
```

3. Check the insallation

Confirm that jextract is installed properly.

```text
> <copy>jextract --version</copy>

jextract 19JDK version 19.0.1
clang version 13.0.0
```

## Task 3: Using jextract

To get some help, use the `-?` flag with `jextract`.

```text
<copy>
Usage: jextract <options> <header file>                                

Option                         Description                             
------                         -----------                             
-?, -h, --help                 print help                              
-D <macro>                     define a C preprocessor macro           
-I <path>                      specify include files path              
--dump-includes <file>         dump included symbols into specified file
--header-class-name <name>     name of the header class                
--include-function <name>      name of function to include             
--include-macro <name>         name of constant macro to include       
--include-struct <name>        name of struct definition to include    
--include-typedef <name>       name of type definition to include      
--include-union <name>         name of union definition to include     
--include-var <name>           name of global variable to include      
-l <library>                   specify a lib name or absolute lib path
--output <path>                specify the dir to place generated files
--source                       generate java sources                   
-t, --target-package <package> target package for specified header files
--version                      print version information and exit
</copy>   ```



1. Generate Java source classes from a C library.

The goal of jextract is to mechanically generate Java bindings from C native library headers. This can be done with the following command.

```text
> <copy>jextract --source -t clang.stdlib.stdio -I /usr/include --output ~/lab/src/main/java /usr/include/stdio.h</copy>
```

Let's break down the different arguments.
* `--source` instruct jextract to generate java sources
* `-t` is used to specify what target package should be used for the generated sources
* `-I` tells jextract where to find the C header files
* `--output` tells jextract where the java sources should be generated to
the final argument specifies which header(s) should be converted


Running the command above instructs jextract to create a new Java package containing classes covering the C stdio library API defined in the standard `stdio.h` header file. 

2. Exploring the generated classes.

You can confirm that jextract has generated multiple classes in the target directory with the target namespace.

```text
> <copy>find ~/lab/src/main/java/clang/stdlib/stdio/</copy>

…/lab/src/main/java/clang/stdlib/stdio/stdio_h.java
…/lab/src/main/java/clang/stdlib/stdio/__fsid_t.java
…/src/main/java/clang/stdlib/stdio/__mbstate_t.java
…/src/main/java/clang/stdlib/stdio/_G_fpos_t.java
…src/main/java/clang/stdlib/stdio/_G_fpos64_t.java
…/src/main/java/clang/stdlib/stdio/_IO_marker.java
…/src/main/java/clang/stdlib/stdio/_IO_FILE.java
…/src/main/java/clang/stdlib/stdio/__io_read_fn.java
…/src/main/java/clang/stdlib/stdio/__io_write_fn.java
…/src/main/java/clang/stdlib/stdio/__io_seek_fn.java
…/src/main/java/clang/stdlib/stdio/__io_close_fn.java
…/src/main/java/clang/stdlib/stdio/fpos_t.java
…/src/main/java/clang/stdlib/stdio/__FILE.java
…/src/main/java/clang/stdlib/stdio/FILE.java
…/src/main/java/clang/stdlib/stdio/constants$0.java
…/src/main/java/clang/stdlib/stdio/constants$1.java
…
…/src/main/java/clang/stdlib/stdio/Constants$root.java
…/src/main/java/clang/stdlib/stdio/RuntimeHelper.java
```


jextract creates different classes, including:
* **`stdio_h.java`** - a public API class that contains all exportable C stdio functions.
* **`FILE.java`** - a class that represents the C FILE struct, it contains [GroupLayout](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/foreign/GroupLayout.html) that corresponds to the C FILE struct as well as a set of getters and setters for each fields of the C struct.
* a series of **`constants$XX.java`** classes holding function descriptors and method handles for every native function extracted from the C stdio library
* **`Constants$root.java`** - a placeholder for [ValueLayouts](https://docs.oracle.com/en/java/javase/19/docs/api/java.base/java/lang/foreign/ValueLayout.html) corresponding to C primitive types
* **`RuntimeHelper.java`** - a private service class that contains helpers designed to construct method handles for each native function.

💡 A [struct](https://en.wikipedia.org/wiki/Struct_(C_programming_language)) is way of grouping, in C, mutliple variables into a single composite data type.


There are a few things to keep in mind:

* jextract runs recursively against the specified target header file(s). In this case, the C `stdio.h` contains some additional imports from the C standard library. So jextract is also generating the corresponding Java sources for those.
* There is only one public API class no matter how large the library is.
* jextract will create a public class for every `struct` and `typedef` referenced in a header file.



## Task 4: Using the generated classes

1. TODO


## Task 5: Jextract advanced usage


As you saw, jextract generates a lot of Java sources for the C stdio.h header file. Not all of those header files are required to execute a specific C function. To check what exactly jextract will generate for a header file it has the following parameter:

--dump-includes <file> dump included symbols into specified file
For instance, the following command will create stdio_dump.txt file containing all native symbols jextract is capable to extract:

jextract --source -t com.clang.stdlib.stdio -I /usr/include --output src/main/java --dump-includes stdio_dump.txt /usr/include/stdio.h
This parameter will instruct the jextract tool to create a file containing all native symbols it could potentially extract from a header file. A newly created file will contain a combination of the following option values:

--header-class-name <name>
--include-function <name>
--include-enum <name>
--include-macro <name>
--include-struct <name>
--include-typedef <name>
--include-union <name>
--include-var <name>
Using the combination of these parameters it’s possible to limit the amount of code jextract could generate. For instance, the following command will create the infrastructure code only for C puts native function:

jextract --source -t com.clang.stdlib.stdio -I /usr/include --output src/main/java --include-function puts /usr/include/stdio.h
Filtering helps to avoid creating unnecessary Java source files by omitting certain functions, macros, structs, vars, and typedefs not mentioned in that dump file. From the technical standpoint, the dump file is a placeholder for jextract parameters, it can be supplied using a simple shell trick - @<file>. Now let's manually create a new dump file that contains jextract include parameters necessary for exporting the C puts:

echo "--include-function puts" >> puts.txt
jextract --source -t com.clang.stdlib.stdio -I /usr/include --output src/main/java  @puts.txt /usr/include/stdio.h
Note: It’s essential to know what you are exporting, there may be a dependency between components of a header file, like stdio.h FILE and _sFILE types where the FILE native symbol is an alias to the _sFILE typedef. Excluding one may lead to problems with others.


1. TODO


2. TODO


## Conclusions

In this lab, you have used jextract, a tool that generates the infrastructure code required for using foreign functions. Jextract helps developers as they can focus solely focus on writing the logic that will rely on this generated code to invoke foreign functions. An additional benefit of jextract is that the generated code will always be optimized to take benefits of various optimizations (ex. JIT compiler and HotSpot optimizations), another important aspect that developers won't have to worry about.!


## Learn More


* [Jextract Early-Access Builds](https://jdk.java.net/jextract/)

## Acknowledgements
* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - David Delabassée, Sept. 20 2022
