#  Upcalls

## Introduction


This lab introduces you to upcalls, i.e. native C code invoking some Java code via the FFM API.

Estimated Time: 15 minutes


### Objectives

*List objectives for this lab using the format below*

In this lab, you will:
* Objective 1
* Objective 2
* Objective 3


## Task 1: Short Pointer Primer

There are 2 ways to pass parameters to a function in C: (1) by value, and (2) by reference, i.e. using a pointer. For instance, the C `puts` function accepts as parameter a pointer to a _char_ array. Given that Java does not expose pointers to developers, the concept of pointer is obscure if not unknown to most Java developers. In short, a pointer is "simply" a variable whose value is the address of another variable.

```
int puts(const char* s);```

💡 The asterisk * is used to declare a pointer. 

Native C types such as _int_, _char_, _struct_, etc., can be accessed through pointers. In addition to pointing to some data, a pointer can also point to C function. A function pointer is a variable that stores the address of a function, function that can later be invoked through this function pointer.


```c
#include <stdio.h>

int main() {
    int (*putsPtr) (const char*);
    putsPtr = &puts;
    putsPtr("Hello-hello! Long time no see!");

    return 0;
}
```

Let's look  more closely at the snippet above. It starts with a pointer declaration. This pointer is named `putsPtr` and points to a function that accepts `const char*` as parameter, and returns an `int` value. 
```c
int (*putsPtr) (const char*);
```
This a just a pointer declaration. `putsPtr` is only bound to a specific funtion, i.e. to the 'puts' function of the C stdio library in this case, at the assignment. 

```c
putsPtr = &puts;
```

Following this pattern a pointer to C printf would have the following form:

(optional) Step 1 opening paragraph.

1. Sub step 1

	![Image alt text](images/sample1.png)

	> **Note:** Use this format for notes, hints, tips. Only use one "Note" at a time in a step.

2. Sub step 2

  ![Image alt text](images/sample1.png)

4. Example with inline navigation icon ![Image alt text](images/sample2.png) click **Navigation**.

5. Example with bold **text**.

   If you add another paragraph, add 3 spaces before the line.

## Task 2: Concise Step Description

1. Sub step 1 - tables sample

  Use tables sparingly:

  | Column 1 | Column 2 | Column 3 |
  | --- | --- | --- |
  | 1 | Some text or a link | More text  |
  | 2 |Some text or a link | More text |
  | 3 | Some text or a link | More text |

2. You can also include bulleted lists - make sure to indent 4 spaces:

    - List item 1
    - List item 2

3. Code examples

    ```
    Adding code examples
  	Indentation is important for the code example to appear inside the step
    Multiple lines of code
  	<copy>Enclose the text you want to copy in <copy></copy>.</copy>
    ```

4. Code examples that include variables

	```
  <copy>ssh -i <ssh-key-file></copy>
  ```

## Learn More

*(optional - include links to docs, white papers, blogs, etc)*

* [URL text 1](http://docs.oracle.com)
* [URL text 2](http://docs.oracle.com)

## Acknowledgements
* **Author** - <Name, Title, Group>
* **Contributors** -  <Name, Group> -- optional
* **Last Updated By/Date** - <Name, Month Year>
