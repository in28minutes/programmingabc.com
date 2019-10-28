---
layout:     post
title:      Introduction to Lambda Expressions in Java
date:       2018-09-11 17:36:00
summary:    Introduction to lambda Expressions. The main building block of functional programming.
categories: Java
permalink:  /lambda-expressions-in-java

---

## You will learn

* Why do I need lambda expression in Java?
* How do lambda expressions reduce the amount of code I write?
* What is the purpose of Java lambda expressions?

## We will discuss:

* How using lambda expressions can reduce the amount of code we write
* We can focus on the application logic (the "what")
* We don't have to bother with the implementation logic (the "how") 

## We assume you are aware of:

* The meaning of terms object-oriented programming

## Tools you will need

* The JDK Environment installed
* The JShell REPL tool
* A code editor or an IDE, such as Eclipse

## Programming Courses

- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}


## The Object Oriented Way

Before we explain how *FP* is different from *OOP*, maybe we could see where exactly it fits in.  Consider a typical program written to process a list. You need to create a ```List``` of ```String``` objects and iterate through it, printing out its contents. 

A simple Java program that uses OOP concepts to do so, looks like this.

#### Example-01 : OOP List Traversal 

**_FunctionalProgrammingRunner.java_**

```java
import java.util.List;

public class FunctionalProgrammingRunner {

   public static void main(String[] args) {

   <String> list = List.of("Apple", "Banana", "Cat", "Dog");

      for(String str:list) {

         System.out.println(str);

      }

   }

}
```

**_Console Output_**

_Apple_

_Banana_

_Cat_

_Dog_

#### Snippet-01 Explained

* The above example typifies the fact that the code revolves round the data. The logic of printing out the contents of ```list``` involves looping, where the programmer needs to both set up the mechanism and track the task progress. As you will see in the next step, *FP* changes this.


## The Functional Programming Way

The concept of **Functional Programming** (**FP**) uses a different approach to manage  state and behavior in your program. *FP* treats **functions** as *first-class* citizens, giving them the emphasis due to them. Object state within a program, does not get to steal the limelight any more. With *FP*, a function is a piece of code that does a unit of computation. It could be printing a list of strings on the console, computing the sum of the first 10 integers, and the like.

* Imagine a program where instead of objects, *functions* are passed around to compute different stuff. Functions would then be treated on par with data. The curious few among you might ask: 
	* Can you assign a function to a variable?
	* Can you pass a function as an argument to a method?
	* Can you obtain a function as a return value, from a method invocation?

We will see how Java FP answers these questions, and more. The next step introduces you to the basics of FP as implemented  Java.

## Defining Methods, The FP Way

The conventional approach (*structural*, and/or *OOP*) forces you to take care of both the **what**, and the **how** of a computation. We saw that not only did we need to access individual elements of a ```List``` during iteration, we also had to setup and track the progress of its iteration. The main activity, calling  ```System.out.println()``` to print each element, was actually quite simple. A classic case of _"Much ado about nothing!!"_! 

This is where *FP* comes across as a breath of fresh air. This approach allows you to focus mainly on the **what**, while it takes care of mechanisms to care for the **how**. To highlight the difference, it's useful to compare and contrast. Let's write both the *OOP*/*Structural* and the *FP* versions of such a logic, as separate methods. The following example should help you identify the shift in approach.

#### Example-02 : ```printBasic()``` And ```printFunctional()```

```java

//**_FunctionalProgrammingRunner.java_**

import java.util.List;

public class FunctionalProgrammingRunner {

	public static void main(String[] args) {

		List<String> list = List.of("Apple", "Banana", "Cat", "Dog");

		// printBasic(list);

		printFunctional(list);

	}

	public static void printBasic(List<String> list) {

		for (String str : list) {

			System.out.println(str);

		}

	}

	public static void printFunctional(List<String> list) {

		list.stream().forEach(

				element -> System.out.println(element)

		);

	}

}
```

**_Console Output_**

_Apple_

_Banana_Cat_

_Dog_

#### Example-02 Explained

* ```printFunctional()``` showcases *FP* for you, the alert programmer! In plain english, one could describe this method as: 
	* ```printFunctional()``` accepts ```list``` as an argument
	* It converts ```list``` into a ```Stream``` of ```String``` objects, and 
	* ```forEach``` ```element``` in that ```Stream```, do a console display using ```System.out.println()```. 
* The code ```element -> System.out.println(element)``` that appears as the argument to ```forEach()```, is called a **lambda expression**. This is a *directive*, or a *statement of action*, that does actual computation in an *FP* setup. Roughly, it is called a **function** in *FP* terms. 
* Did you notice that in the code above, we passed a *lambda expression* as a parameter to a method (instead of an object or other data, as in the case of *OOP*)? 


## Looping Through A ```List```

*FP* has opened a window to a whole new world for us. **"Passing a function as a method argument"**. Isn't that out of this world? Now you're curious to see what else this new approach can do. Maybe you want to rewrite every program you wrote so far, if you could. 

Let's do this one step at a time. Let's also see if ```JShell``` can help us come to grips with *FP*. The following example tries to solve this problem: *"Given a list of numbers, iterate through this list and print each number out."*

#### Example-03 : Loop Using FP

**jshell>** List<Integer> list = List.of(1, 4, 7, 9);

_list ==> [1, 4, 7, 9]_

**jshell>** list.stream().forEach(elem -> System.out.println(elem));

_1_

_4_

_7_

_9_

**jshell>**

#### Example-03 Explained

* Using *FP*, we hardly broke a sweat here! We could write the actual code in just one statement! The one thing that stands out here is the elegance of the code written. I would travel miles to write, and run that piece of code!

## Filtering Results

If we asked you to write software to control a coffee machine, a lot of choices and options that users may have, would go into code as well. These choics would include coffee type, temperature, quantity, and the like. The code would need to filter out options based on a user's choice, and not just filter the coffee! 

Yes, data filtering is a major part of programming, and *FP* makes it simple for you with lambda expressions. The following example filters out certain strings from others, for instance.   

#### Example-04 : Using ```filter()```

```java
package com.in28minutes.functionalprogramming;

import java.util.List;

public class FunctionalProgrammingRunner {

	public static void main(String[] args) {

		List<String> list = List.of("Apple", "Bat", "Cat", "Dog");

		// printBasicWithFiltering(list);

		printFPWithFiltering(list);

	}

	public static void printBasicWithFiltering(List<String> list) {

		for (String str : list) {

			if (str.endsWith("at")) {

				System.out.println(str);

			}

		}

	}

	public static void printFPWithFiltering(List<String> list) {

		list.stream()

				.filter(elem -> elem.endsWith("at"))

				.forEach(element -> System.out.println(element));

	}
}
```

**_Console Output_**

_Bat_

_Cat_

#### Example-04 Explained

* A ```Stream``` object is nothing but a sequence of values from a collection. The ```filter()``` method is called on a ```Stream``` object, and it filters the ```Stream``` elements based on some logic. The logic is executed by the lambda expression passed to  ```filter()``` as an argument.

## Another example using ```filter```

Why just strings, can't ```filter()``` work with numbers as well? Can it be used to separate even numbers from the odd ones, from within a list? It sure can.

#### Example-05 : Printing even/odd numbers

**jshell>** ```List<Integer> list = List.of(1, 4, 7, 9);```

_list ==> [1, 4, 7, 9]_

**jshell>** ```list.stream().forEach(elem -> System.out.println(elem));```

_1_

_4_

_7_

_9_

**jshell>** ```list.stream().filter(num -> num%2 == 1).forEach(elem -> System.out.println(elem));```

_1_

_7_

_9_

**jshell>**  ```list.stream().filter(num -> num%2 == 0).forEach(elem -> System.out.println(elem));```

_4_

**jshell>**

#### Example-05 Explained

* The logic that runs the filtering part, actually encodes a condition in conventional programming. These conditions would be:
	* ```num``` is odd : ```if(num % 2 == 1) { /*  */ }```
	* ```num``` is even : ```if(num % 2 == 0){ /*  */ }```
* In *FP*, we do this with lambda expressions: 
	* ```num``` is odd: ```num -> num%2 == 1```
	* ```num``` is even: ```num -> num%2 == 0```

Notice the amount of code clutter that we have done away with. This is a very big reason why functional programming holds its own, when challenged by well-established programming practices.

## Summary

In summary, we covered the following concepts in this article:

* Functional programming focuses more on the "what", rather than on the "how"
* We reinforced this fact by writing a simple program to traverse a list, and compared it with the object-oriented approach
* We also explored a built-in Java method called ```filter()```, to extract desirable results from a list.

## Next Steps
- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA