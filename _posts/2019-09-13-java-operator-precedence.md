---
layout:     post
title:      Operator Precedence - Understand How Java Evaluates Expressions
date:       2018-09-09 10:30:00
summary:    Explains Java Operator Precedence Rules. Helps you easily understand expression evaluation.
categories: Java
permalink:  /operator-precedence-how-java-evaluates-expressions
---

## You will learn

* What do you mean by operator precedence?
* What is operator precedence in Java?
* Which operator has high precedence in Java?
* How parentheses override Java operator precedence?

## We will discuss:
* How combining different operators and different data - makes expressions tricky
* How operator precedence sorts out expression evaluation
* Why Java allows you to group sub-expressions, using parentheses

## We assume you are aware of:

* The meaning of terms type, expression, operator, operand and sub-expression

## Tools you will need

* JDK Environment Installed
* A REPL tool like JShell, or an IDE such as Eclipse

## Programming Courses

- [Java Programming in 250 Steps](https://rebrand.ly/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://rebrand.ly/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://rebrand.ly/MISC-JAVA-PYTHON){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}


## Operators And Expressions

The Java language has the concept of an **operator**. The ones that come to mind immediately are:

* ```+```: Addition
* ```-```: Subtraction
* ```*```: Multiplication
* ```/```: Division
* ```%```: Modulo

All these operators expect 2 **operands** each. Combining an operator with operands gives us an **expression**, which can be evaluated. The following examples will give you a glimpse of how you can form expressions.

#### Example-1: Operators And Expressions

**jshell>** ```5 * 10```

_$2 ==> 50_

**jshell>** ```5 + 10```

_$3 ==> 15_

**jshell>** ```5 - 10```

_$4 ==> -5_

**jshell>** ```10 / 2```

_$5 ==> 5_

**jshell>** ```9 % 2```

_$6 ==> 1_

**jshell>** ```8 % 2```

_$7 ==> 0_

**jshell>**

#### Example-1 Explained

The above expressions are quite simple, in that they involve just a single operator. Java makes them very easy to use, and evaluates them without a problem. Let us now explore what more we can pack into an expression.


## Expressions With Multiple Operators

You can also create expressions with more that one operator within them. For example, you can create expressions with:

* Two additions
* Two subtractions
* A combination of addition and subtraction

And so on. We are just warming up here...have a look at the following examples.

#### Example-2: Complex Expressions
 
**jshell>** ```5 + 5 + 5```

_$8 ==> 15_

**jshell>** ```5 + 10 - 15```

_$9 ==> 0_

**jshell>** ```5 * 5 + 5```

_$10 ==> 30_

**jshell>**  ```5 * 15 / 3```

_$11 ==> 25_

**jshell>** 

#### Example-2 Explained

* We gave ```JShell``` four expressions, each of which had two operators and three operands.
	* ```5 + 5 + 5``` has two identical ```+``` operators
	* The expressions ```5 + 10 - 15```, ```5 * 5 + 5``` and ```5 * 15 / 3``` have two  distinct operators each.

## Why Operator Precedence Rules Are Needed

We write English left-to-right, and carry this habit to calculations as well. While giving code such as ```5 + 5 * 6``` to ```JShell```, we probably want to do the addition first, followed by the multiplication. Is that what we get? Find out in the example below.

#### Example-3: Operator Precedence  

**jshell>** ``` 5 + 5 * 6 ```

_$1 ==> 35_

**jshell>** ``` 5 - 2 * 2 ```

_$2 ==> 1_

**jshell>**  ``` 5 - 2 / 2 ```

_$3 ==> 4_

**jshell>**

#### Example-3 Explained

*  Instead of ```60```, the expression ```5 + 5 * 6``` hits us with a result of ```35```! Clearly, ```JShell``` has evaluated the **sub-expression** ```5 * 6``` first, and added its value to the first literal, ```5```. Why would it do that? Use your magnifying lens here again, please!
 
	_In expressions with multiple operators, the order of sub-expression evaluation depends on **operator precedence**._

* The rules for operator precedence are actually quite simple. The operators in the set  {```*```, ```/```, ```%```} have higher precedence than the operators in the set {```+```, ```-```}.
* In the expression ```5 + 5 * 6```:
	* Since ```*``` has higher precedence than ```+```, the sub-expression ```5 * 6``` is evaluated first, resulting in ```30```.
	* The new sub-expression ```5 + 30``` is evaluated next, giving a value of ```35```, which is what ```JShell``` prints out.
* We can easily infer that ```JShell``` evaluates ```5 - 2 * 2``` and ```5 - 2 / 2```  by following the same rules.

## Using Parentheses Within An Expression

Parentheses play an important role in expression evaluation. The code example that follows,  will lay things bare for you.

#### Example-4: Parentheses In Expressions

**jshell>** ``` (5 - 2) * 2 ```

_$4 ==> 6_

**jshell>**  ``` 5 - (2 * 2) ```

_$5 ==> 1_

**jshell>**

#### Example-4 Explained

* Java allows the use of parentheses, to group parts of an expression. Parentheses are generally used to **overrule** operator precedence.
	* In  the expression ```(5 - 2) * 2``` , the parentheses force ```JShell``` to evaluate ```5 - 2``` first, which gives a result ```3```.
	* It then evaluates the resulting sub-expression ```3 * 2```, to yield ```6``` as the final result.
	* Evaluating ```5 - (2 * 2)``` yields the same result (```1```) as ```5 - 2 * 2```, since here the parentheses don't don't do anything different.

## The Advantages Of Using Parentheses 

The use of parentheses to group sub-expressions, is considered a **good programming practice**. In large software projects, programmers who initially write software are not always the ones who maintain it later. Parentheses lead to better readability, as they reduce confusion and help avoid errors. The old adage **_"A stitch in time saves nine"_** rings very true here.

## Summary

In summary, what we have explored and learned are:

* Expressions can be formed with more than one operator, and their operands
* When we combine different operators in a single expression, the result is not always what we expect
* Knowledge of Java's Operator Precedence rules helps us understand that behavior
* Parentheses are a useful tool to structure the expressions for the result we need

We hope this article leaves you better placed to understand operators and expression evaluation, even when they are complex. We'll see you again soon. Until then, bye-bye!


## Next Steps
- [Java Programming in 250 Steps](https://rebrand.ly/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://rebrand.ly/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://rebrand.ly/MISC-JAVA-PYTHON){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA