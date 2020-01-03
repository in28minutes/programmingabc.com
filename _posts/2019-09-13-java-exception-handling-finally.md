---
layout:     post
title:      Java Exception Handling with Finally
date:       2018-09-07 17:36:00
summary:    Understand how adding a finally clause to a try catch block neatly releases resources if there is an exception
categories: Java
permalink:  /java-exception-handling-with-finally

---

## You will learn

* What is ```finally``` keyword in Java?
* Why is ```finally``` useful in Java?
* Does ```finally``` always execute in a Java program?
* How to close a ```Scanner``` in Java using ```finally```?

## We will discuss:
*	The importance of releasing acquired resources
*	Using a simple ```try...catch``` for this, is not elegant
*	Adding a ```finally``` clause solves this issue neatly

## We assume you are aware of:

* Exception handling using ```try…catch```
* Reading console input

## Tools you will need

* JDK Installed
* A Code Editor or IDE, like Eclipse

## Programming Courses

- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=NOVEMBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=NOVEMBER-2019){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}

## Exception Handling Basics

No matter how well you code, there is always a chance that your program has errors hidden inside. Such errors can appear when you run your code, and can take you by surprise. Examples of such errors are:

* Your program runs out of call stack during execution
* Your code attempts a division by zero
*  You try to access memory beyond an array's bounds

Such run-time errors are also known as **exceptions**. When the run-time system encounters an exception, it *abruptly* stops running your code. There is nothing you can do to resume execution from that point. The good thing is that the run-time prints the method stack trace, which you can use to locate and debug the error. Java gives you a mechanism to handle exceptions, the ```try...catch``` block. With **exception handling**, you can manage the execution in a graceful manner, and the program can resume running after that.

## Accessing System Resources

Very frequently, your code needs to access more than just the heap, or the call stack. Examples of such resources are the console, file handles and database connections. Since these are accessed at run-time, the chance of errors does exist. As expected, Java denotes  a large number of such errors, with exception classes. While managing such resources, it is your responsibility to return them when you're done. This holds true whether execution goes through normally, or with exceptions showing up. 

The rest of this article discusses how you can free up resources you use, in an elegant manner.

## An Example Of Resource Access

Let's take a simple example of how you can access resources within your code. Java's ```Scanner``` ```class``` provides an easy way to read user input from the console. The snippet below will show you how to access a ```Scanner``` instance.

### Example-01: Using The Scanner Utility

**_FinallyRunner.java_**

```java

	package com.in28minutes.exceptionhandling;
	
	import java.util.Scanner;
	
	public class FinallyRunner {
	
		public static void main(String[] args) {
	
			Scanner scanner = new Scanner(System.in);
	
			// Program logic, probably using scanner input
	
			int[] numbers = { 1, 2, 3, 4 };
	
			int num = numbers[5];
	
			System.out.println("Before scanner close");
	
			scanner.close();
	
		}
	
	}

```

**_Console Output_**

_Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException_

_at com.in28minutes.exceptionhandling.FinallyRunner.main (FinallyRunner.java:8)_

### Example-01 Explained

The ```FinallyRunner``` ```main()``` method acquires a ```Scanner``` object, and then proceeds to index an array. The length of ```numbers``` is fixed to ```4``` at compile-time, so ```numbers[5]``` is illegal! Naturally, Java will throw an exception, which here is ```ArrayIndexOutOfBoundsException```. Without a ```try...catch``` block, this program will end abrutply.

## Handling The Exception

As we all know, handling this exception will make recovery possible, and graceful. Adding a ```try...catch``` block should solve our problem, right? Let's have a look below.

### Example-02: Adding Exception Handling

```java

	package com.in28minutes.exceptionhandling;
	
	import java.util.Scanner;
	
	public class FinallyRunner {
	
		public static void main(String[] args) {
	
			Scanner scanner = null;
	
			try {
	
				scanner = new Scanner(System.in);
	
				// Program logic, probably using scanner input
	
				int[] numbers = { 1, 2, 3, 4 };
	
				int num = numbers[5];
	
			} catch (Exception e) {
	
				if (scanner != null) {
	
					System.out.println("Before scanner close");
	
					scanner.close();
	
				}
	
			}
	
			System.out.println("Before exiting main");
	
			scanner.close();
	
		}
	
	}

```

### Example-02 Explained

Note that when an exception is handled, a particular ```catch``` clause should have code for it. Execution from this catch could follow a different path from now on, by throwing a fresh exception, for instance. Therefore, resource release is a must at such places. Also, we need to release it even when no exceptions occur! This is a classic case of repeated code.

## ```finally```: Handling Code Duplication

Code duplication is a maintenance nightmare. In large software projects, developers writing original code, are often different from those who maintain it. Now, suppose in the beta-version of this code, there were two ```catch``` clauses following the ```try```. Later on during maintenance, more code is added in the same ```try```, and a couple more ```catch``` clauses are needed to go with it. To err is human, and the maintenance team could easily forget to release the ```Scanner``` object in one such ```catch```. We would surely have a problem here! 

Java steps in once again, and gives you the ```finally``` clause. ```finally``` holds code that runs not only when no exceptions occur, but also when exceptions are handled in the ```try...catch```. A ```finally``` clause is added to the end of an existing ```try...catch``` block. here is how it can reduce clutter: 

### Example-03: Adding ```finally``` to the ```try...catch``` 

```java

	package com.in28minutes.exceptionhandling;
	
	import java.util.Scanner;
	
	public class FinallyRunner {
	
		public static void main(String[] args) {
	
			Scanner scanner = null;
	
			try {
	
				scanner = new Scanner(System.in);
	
				// Program logic, probably using scanner input
	
				int[] numbers = { 1, 2, 3, 4 };
	
				int num = numbers[5];
	
			} catch (Exception e) {
	
				e.printStackTrace();
	
			} finally {
	
				if (scanner != null) {
	
					System.out.println("Before scanner close");
	
					scanner.close();
	
				}
	
			}
	
			System.out.println("Before exiting main");
	
		}
	
	}

```

**_Console Output_**

**_ArrayIndexOutOfBoundsException_**

_Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException_

_at com.in28minutes.exceptionhandling.FinallyRunner.main (FinallyRunner.java:10)_

_Before scanner close_

_Before exiting main_

### Example-03 Explained

As you can see, ```scanner.close()``` when placed within the ```finally```, handled both the following scenarios:

* The program runs without any exception
* An exception is thrown by the run-time

Note that the ```scanner``` reference was declared before the try clause, but was assigned an instance within the ```try```. This means code inside the  ```finally``` needs to check for ```null```, before the releasing ```scanner```. This issue is handled even more neatly in a construct called ```try```-with-resources, but that could be the subject of another write-up! 

Let's now summarize what we've learned in this article.

## Summary

In this article, we learned that:

* Resources used by a program, need to be released
* Exceptions disrupt program flow, and you must release resources there as well
* Using just the ```try...catch``` block is not elegant, and makes maintenance difficult
* The ```finally``` clause is a safe place to do this neatly

We hope this article leaves you better placed to manage your program's resources, even when exceptions occur. We'll see you again soon. Until then, bye-bye!

## Next Steps
- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=NOVEMBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=NOVEMBER-2019){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA