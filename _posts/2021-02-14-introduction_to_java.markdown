---
layout: post
title:      "Introduction to Java"
date:       2021-02-15 02:06:55 +0000
permalink:  introduction_to_java
---


As I continue my learning of Data Structures, I found Java being a little similar to JavaScript based on the documentations I have read so far. So I started learning Java. Here is a  summary of what I have learned so far.

## What is Java

Java is a platform independent programming language, meaning that it allows application programs to be built and run on any platform without having to be rewritten or compiled by the programmer. When first run, Java files are first compiled into Java byte code (stored in binary .class files) using the Java compiler, and the byte code is then executed using the **Java Virtual Machine (JVM)**. 

**JVM** is like an abstract computer implemented for several operating systems to translate byte code to a set of instructions that a computer's CPU can execute directly. 

![](https://cdn.programiz.com/sites/tutorial2program/files/how-java-program-runs.jpg)

Another important tool to run Java program is the **Java Runtime Environment (JRE)**. JRE is a superset of JVM. It is a software package that contains both JVM and Java class libraries necessary to run Java applications. However, JRE cannot compile Java applications.  You can download JRE [here](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html).

Last, the tool necessary to develop Java applications is the **Java Development Kit (JDK)**.  This is the software development kit that contains the JRE , Java complier and other necessary developement tools like the Java Debugger and JavaDoc. JDK can be downloaded [here](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html).
The following image summarizes the relationship between JVM, JRE, and JDK:

![](https://cdn.programiz.com/sites/tutorial2program/files/jdk-jre-jvm.jpg)

## How Java Program works

As a newbie in Java, my first written program is obviously the famous **Hello World**  program. It is always good to start with the basics and build on it. The code for the program is as follow:

```java

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); 
    }
}

```

When run in your local machine, this code will output `Hello, World!` in your console. 

Now let's dissect what's going on in the above code. Every program in Java starts with a class definition like in the above example `class HelloWorld { ... }`; and by convention, the name of the class must be the same as the file's name. Thus, the file name in question here will be `HelloWorld.java`.  This is important for the program to compile properly. 

Next, the public class must contain a `main` method, which represents the entry point for any java application as the complier starts compiling codes from the main method.  The signature for the main method is as follow: 
```java

public static void main(String[] args) { ... }

```
The meaning of the keywords `public`, `static`, and `void` will be discussed in future posts as I learn more about them. But you can find out more about them [here](https://www.programiz.com/java-programming/methods).

Finally, the last part of the program snippet is `System.out.println("Hello, World!");`. This is a print statement that will output the text or string `Hello, World!` on the screen or console. Strings are written in quotation marks ("") in Java.

> ### Final Note
> * If you want to develop and run Java programs, you will need a [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html), which contains the necessary tools to compile and execute a Java applications. 
> * A valid Java application must contain a class definition that has the same name as the filename.
> * The class must contain a `main` method which is the entry point where the complier starts executing Java codes.

That's it for the Intro to Java. Hope you enjoyed the reading

Until next time,

Happy coding!!!
