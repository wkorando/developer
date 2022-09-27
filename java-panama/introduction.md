# Introduction

## About this Workshop


Project Panama is an OpenJDK project whose goal is to improve and enrich the connections between the Java Virtual Machine and well-defined but “foreign” (non-Java) APIs, including many interfaces commonly used by C programmers. In a nutshell, Panama eases the interaction between Java code and native code, in both ways.

Project Panama also tackles Single Intrsuction Mulitple Data programming in Java. SIMD is a programming technique that leverages Vector units embedded in CPUs to produce more performant code. 

In practive, there are, in Java 19, 2 APIs that were developed under the Panama umbrella:
- the Foreign Function & Memory API (Preview)
- the Vector API (Fourth Incubator)
Moreover, jextract is a tool also developed under the same umbrella.

The goal of this workshop is to introduce you to Project Panama, its 2 APIs, and the jextract tool. 

Estimated Workshop Time: ~75 minutes

### Objectives


In this workshop, you will learn:
* the challenges of writing Java code that interoperate with native code, and why it matters
* how to use Foreign Function & Memory API
* how to use jextract, and the benefits it offers
* the concept of SIMD programming
* how to use the Vector API


### Prerequisites

This lab assumes you have:
* some Java skills
* some Linux skills


## Learn More

* Panama section on [Inside Java](https://inside.java/tag/panama)
* [JEP 424: Foreign Function & Memory API (Preview)](https://openjdk.org/jeps/424)
* [Dev.Java](https://dev.java)

## Acknowledgements
* **Author** - [Denis Makogon, DevRel, Java Platform Group - Oracle](https://twitter.com/denis_makogon)
* **Contributor** -  [David Delabassée, DevRel, Java Platform Group - Oracle](https://twitter.com/delabassee)
* **Last Updated By/Date** - David Delabassée, Sept. 20 2022
