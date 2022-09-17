# Your workshop environment

## Introduction

This lab introduces you to the workshop environement.

Estimated Time: ~10 minutes

### **Objectives**

In this lab, you will:

* Discover OCI Cloud Shell and Cloud Editor.
* Do some minimal setup to prepare your environment.


## Task 1: Cloud Shell

To perform this workshop, you will use Java 19 on Oracle Cloud Infrastructure.
In theory, you can also do the workshop on your own machine with Java 19 installed but in the interest of time, it is easier to use OCI.

Cloud Shell is a browser-based terminal that provides an ephemeral Linux machine.
It simplifies application development and access to cloud resources on OCI.
Under the hood, Cloud Shell uses an OCI pre-configured Virtual Machine with preinstalled tools, including Java and as you will see in this workshop, Cloud Shell can also be used to develop simple applications.

To launch Cloud Shell, simply log on the OCI console via [cloud.oracle.com](https://cloud.oracle.com), and click the Cloud Shell icon on the top right.

![Starting Cloud Shell](../images/cs-start.png)

After ~30 seconds, your Cloud Shell VM will be up and running, and your Cloud Shell terminal will be displayed in your browser.

![Cloud Shell started](../images/cs-started.png)

You now can use Cloud Shell as a regular shell.

## Task 2: Prepare your environment

This workshop uses Java 19 which was released last month.
And although Cloud Shell supports Java, it doesn't come yet with Java 19 support.
So you first need to install and configure Java 19.

The first step is to clone the workshop repository to get all workshop material.

```
<copy>git clone TODO</copy>
```

You can now install and setup Java 19 with the following commands.

```
<copy>cd TODO ⚠️
source scripts/TODO.sh</copy>
```

This script downloads and untars Java 19, and configures the JAVA_HOME environment variable properly.
You can confirm that Java 19 is installed with the command below.

```
<copy>java -version</copy>
```


## Task 3: Cloud Editor

During the workshop, you will also also use Cloud Editor, a Cloud Shell feature that offers a browser-based modern text editor.
To launch Cloud Editor, simply click, in the [OCI console](https://cloud.oracle.com), on the Cloud Editor icon on the top right, next to the Cloud Shell icon.

![Starting Cloud Editor](../images/ce-start.png)

💡 Cloud Editor runs in the Cloud Shell VM but you don't have to start Cloud Shell to use Cloud Editor.
If you don't, the Cloud Shell VM will automatically be started when Cloud Editor is launched.

![Cloud Editor default layout](../images/cs-ce-horizontal.png)

By default, Cloud Shell and Cloud Editor use a stacked layout.
You can adjust this layout to fit your preferences by clicking on the top-left "View" option.

![Cloud Editor default layout](../images/cs-ce-view.png)

You can also re-size, maximize, minimize, swap, close the Cloud Shell and/or the Cloud Editor window, change fonts (check the "Gear" icon), etc.
We suggest spending 1 or 2 minutes getting familiar with both Cloud Shell and Cloud Editor.

💡 For the workshop, we suggest keeping both Cloud Shell and Cloud Editor open in the same window, and the workshop guide in a different browser window/tab.

💡 As mentioned earlier, Cloud Shell doesn't come yet with Java 19 support. For this workshop, Cloud Editor has been set up to support Java 19.

Congratulations, you are now all set for the next step!


## Learn More

* [Cloud Shell documentation](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)
* [Code Editor documentation](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/code_editor_intro.htm)


## Acknowledgements

* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** - [Nicolai Parlog, DevRel, Java Platform Group - Oracle](https://nipafx.dev/)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - Nicolai Parlog, Sep. 19th 2022
