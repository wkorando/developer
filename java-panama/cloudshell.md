# Discover your environment.

## Introduction

This lab introduces you the workshop environement.

Estimated Time: ~5 minutes

### **Objectives**

In this lab, you will:
* Discover OCI Cloud Shell and Cloud Editor.
* Do some minimal setup to prepare your environment.

## Task 1: Cloud Shell

To perform this workshop, you will use Java 19 on Oracle Cloud Infrastructure. Note that you can obviously do it on our own machine with Java 19 but in the interrest of time, it is easier to use OCI. 

Cloud Shell is a browser-based terminal that provides an ephemeral Linux machine, it simplifies application development and access to cloud resources on OCI. Under thehood, Cloud Shell uses an OCI pre-configured Virtual Machine with preinstalled tools. As you will see, Cloud Shell can also be used to develop simple applications.


To launch Cloud Shell, simply log on the OCI console via [cloud.oracle.com](https://cloud.oracle.com), and click the Cloud Shell icon on the top right.

  ![Starting Cloud Shell](../images/cs-start.png)

After 20~30 seconds, your Cloud Shell terminal will be displayed in the OCI console.

  ![Cloud Shell started](../images/cs-started.png)

You now can use Cloud Shell as a regular shell.

## Task 2: Cloud Editor


During the workshop, you will also use Cloud Editor, a Cloud Shell feature that offers a browser-based modern text editor.

To launch Cloud Editor, simply click, in the [OCI console](https://cloud.oracle.com), on the Cloud Editor icon on the top right, next to the Cloud Shell icon.

  ![Starting Cloud Editor](../images/ce-start.png)
  
Note that Cloud Editor runs with the Cloud Shell VM. If needed, this VM will be started when Cloud Editor is launched.

  ![Cloud Editor default layout](../images/cs-ce-horizontal.png)

By default, Cloud Shell and Cloud Editor uses an horizontal layout. You can adjust this layout to fit your preferences by clicking on the top-left "View" option.

  ![Cloud Editor default layout](../images/cs-ce-view.png)

You can also re-size, maximize, minimze, swap, close the Cloud Shell and the Cloud Editor window, change fonts (check the "Gear" icon), etc. We suggest to spend 1 or 2 minutes to get familar with both Cloud Shell and Cloud Editor.

For the workshop, we suggest to keep both Cloud Shell and Cloud Editor open in the same winow.

## Task 3: Prepare your environment.

**TODO**


You are now ready to move to the next step.


## Task 2: A Simple Downcall

   The previous section explained, in a simplified way, how the FFM API works with downcalls, .i.e., Java code invoking C native code. In this exercise, you will write your first downcall, i.e. a simple Java application that will invoke the C `puts` function to print a string.
   
   
   The C `puts` function has the following signature: `int puts ( const char * str );` This function writes the *C string* pointed by *str* to the *standard output* (stdout) and appends a newline character ('\n'). On success, the function returns a non-negative value and -1 on error.
    
   
1. Get a Linker   
   
   
   

💡 	> **Note:** TODO Note -enable-native-access





## Task 1: Concise Step Description

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




## Learn More


* More information on [Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)
* [java.lang.foreign](https://download.java.net/java/early_access/jdk19/docs/api/java.base/java/lang/foreign/package-summary.html) Javadoc **TODO update to GA links**


## Acknowledgements
* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - David Delabassée, Sept. 2 2022
