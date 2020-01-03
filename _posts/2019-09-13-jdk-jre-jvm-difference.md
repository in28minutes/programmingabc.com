---
layout:     post
title:      Introduction to Java Platform - JDK vs JRE vs JVM 
date:       2018-09-12 07:47:00
summary:    Explains how The terms JDK, JRE and the JVM are related, but different
permalink:  /introduction-to-java-platform-jdk-jre-jvm-differences
---

## You will learn

* What is the main difference between JDK and JRE?
* What is the main difference between JVM and JRE?
* What is the difference between JVM and the JDK?

## We will discuss:

* How the terms JVM, JRE and JDK are related
* How the trms JVM, JRE and JDK are different

## We assume you are aware of:

* The meaning of the terms compiler, operating system, programming language.

## Programming Courses

- [Java Programming in 250 Steps](https://rebrand.ly/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://rebrand.ly/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://rebrand.ly/MISC-JAVA-PYTHON){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}


## Code Translation

A computer only understands binary code, which are sequences of 0’s and 1’s (bits). Hence, all instructions and data that we want a computer to understand, must be fed to it as such. These binary coded instruction and data formats form the instruction set architecture (**ISA**) for that processor.

We write our programs in a high-level language such as Java, as it is easier to learn, remember and maintain. For it to actually run, code needs to be translated into the binary format. This is made possible by two kinds of system programs:

The **Compiler**: This program understands the source-level instruction and data constructs, as well as the processor-level instruction and data formats. Its job is to translate from source to binary, or flag error messages to help the programmer make corrections. If translation is successful, the compiler creates a binary executable that can be run on the computer.

The **Operating System** (OS): The OS generally exposes a command-line interface, used to input commands for invoking and running the compiler. When a binary executable is created, it provides the environment and resources for execution.

## The Challenge Java Designers Faced

Not all processors have the same ISA. Although the above system tools allow us to code in Java, they cannot guarantee that the executable will run on other computers. The OS being a resource manager, it is tightly tied to the architecture.
Every processor type needs its own specific OS, and a specific Java compiler to translate Java source. In other words, a preinstalled (**OS + Java Compiler**) bundle is a must for developing Java software in that machine.

The designers of the language took this one step further. Their goal was platform-independent source development, and they addressed an important issue. The issue was this:

***Since every computer needed a unique (OS + Java Compiler) bundle, the source code needs to be re-translated on every new processor type on which it is deployed. This takes a lot of time and effort.***

## The Solution

The solution they came up with was simple:

***Have all Java compilers translate source code to an intermediate representation (IR). Install a system program called a Virtual Machine (VM) which would translate this IR to the native instruction set.***

What we have now is a (**OS + Compiler + VM**) bundle as the core execution environment, with resources still managed by the OS. The IR standard specified by Java is called **bytecode**, and the VM is labelled the Java VM (**JVM**). The Java Compiler translates Java source code to bytecode, which is stored as a .class file on the computer. This bytecode is then translated by the installed JVM to binary format, for execution on the native processor. Normally, bytecode-to-binary translation an its execution are combined into a single step.

The bytecode format depends only on the Java Standard, and hence is *portable*. Bytecode generated on one computer can be put on another computer that has a JVM of the same standard, and executed there!

## The JVM, JRE and JDK Explained

* The **JVM** is a system software bundle, with components that run your program bytecode.
* **JRE** = **JVM** + **Libraries** + **Other Components**
	* We just saw what the *JVM* is there for.
	* *Libraries* are built-in  Java utilities that can be used within any program you create. These are organized as **packages**, which makes it easier to maintain and distribute them. ```System.out.println()``` was a method in ```java.lang```, one such standard Java package.
	* *Other Components* include tools for debugging and code profiling (for memory management and performance).
* **JDK** = **JRE** + **Compilers** + **Debuggers**
	* *JDK* refers to the **Java Development Kit**. It's an acronym for the bundle needed  to compile (with the compiler) and run (with the *JRE* bundle) your Java program.

An interesting way to remember this organization is:

**JDK** is needed to **Compile and Run** Java programs

**JRE** is needed to **Run** Java Programs

**JVM** is needed to **Run Bytecode** generated from Java programs

## Summary

In summary, what we have explored and learned here are:

* The reason why Java uses and intermediate language such as bytecode
* How the software components of the Java build system is built round bytecode representation
* What are the differences between JDK, JRE and the JVM, in composition and functionality


## Next Steps
- [Java Programming in 250 Steps](https://rebrand.ly/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://rebrand.ly/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://rebrand.ly/MISC-JAVA-PYTHON){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA