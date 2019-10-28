---
layout:     post
title:      Java Printf - How to format console output
date:       2018-09-12 11:58:00
summary:    Format your Java Console output with printf
categories: Java
permalink:  /introduction-to-printf-format-java-console-output
---

## You will learn

* How to format text output for the console in Java?
* How to print formatted a string in Java?
* How to format text using ```System.out.printf()```?
* Why use ```System.out.printf()``` along with ```System.out.println()```?

## We will discuss:
* Why converting any data into a string format is necessary
* How to use the built-in utility, ```System.out.printf()```, to format such data
* Why ```System.out.println()``` also plays a part in neat console output

## We assume you are aware of:

* What the term console means, in a programming context
* The Java built-in ```System.out.println()``` method

## Tools you will need

* JDK Environment Installed
* A REPL tool like JShell, or an IDE such as Eclipse

## Programming Courses

- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}

## Console Output With ```System.out.println()```

You would be aware that Java has a built-in utility called ```System.out.println()```, that displays data on the console. When we typed in ```System.out.println(3*4)``` in our program, we formed an expression ```3*4```, and *passed* it to ```System.out.println()```.
	
Now suppose you want to print the exact information ```5 * 2 = 10```, on the console. Clearly, ```System.out.println()``` does not accept ```5 * 2 = 10``` as a valid parameter. Earlier though, it had accepted ```5 * 2```, so something is missing here.

Note that ```5 * 2 = 10``` is no longer an expression, so was rejected. What you want to do is print a **string literal**, which is a sequence of characters, enclosed within **double quotes**: '```"```' and '```"```'. Such a literal can be directly passed to ```System.out.println()```, and will be printed **as-is**. A literal such as ```"5 * 2 = 10"``` will work here.

By now, you have figured out that ```System.out.println()``` only accepts a character string as argument. As a programmer, you would need a way to print both numbers and strings, side-by-side on the console. It turns out that Java has a way to do this, and is called **formatted output**. The  built-in ```System.out.printf()``` utility comes into play here.

## Introducing ```System.out.printf()```

The ```System.out.printf()``` method accepts a variable number of arguments:
	* The first argument is the format in which to print data. This is a string literal, having zero or more **format specifiers**. A format specifier is predefined (such as ```%d```), that formats data of a particular type. For example, ```%d``` formats data of type ```int```.
	* The trailing arguments are the expressions, whose content you want to display. 
	* To get the display you want, make sure that:
		* The number of format specifiers matches the number of trailing arguments
		* The format specifiers are in the same order as the trailing arguments.

**jshell>** ```System.out.println("5 * 2 = 10")```

_5 * 2 = 10_

**jshell>**  ```System.out.printf("5 * 2 = 10")```

_5 * 2 = 10$1 ==> java.io.PrintStream@4e1d422d_

**jshell>**
 
* When ```System.out.printf()``` is called with a *single* string argument, it behaves just like  ```System.out.println()```. Well, almost! In addition, it also prints some illegible information (the ```$1 ==> java.io.PrintStream@4e1d422d_``` you see above). For now, just take this as information about ```java.io.PrintStream``` (a the built-in type).

## Beautifying The Output of ```System.out.printf()```

**jshell>** ```System.out.printf("5 * 2 = %d", 5*2).println()```

_5 * 2 = 10_

**jshell>**

* The good news is, that if we call ```System.out,printf()```, and the call   ```System.out.println()``` on the ```java.io.Printstream``` object, the illegible stuff disappears. In addition, a newline is inserted for neater printing!

## Passing Arguments to ```System.out.printf()```

As we just mentioned, ```System.out.printf()``` accepts a variable number of arguments:

* The first one is a format specifier string
* The rest of the arguments are expressions that need to be displayed, and are formatted by these specifiers.

Let's try and play around with them a little, shall we? Look at the code examples below.

**jshell>** ```System.out.printf("%d + %d + %d", 5, 6, 7).println()```

_5 + 6 + 7_

**jshell>**

When the number and format of arguments match exactly, there is no problem at all. We get what we want. 

**jshell>** ```System.out.printf("%d + %d + %d = %d", 5, 6, 7).println()```

_5 + 6 + 7 = | java.util.MissingFormatArgumentException thrown: Format specifier '%d'_

_| at Formatter.format (Formatter.java:2580)_

_| at PrintStream.format (PrintStream.java:974)_

_| at PrintStream.printf (PrintStream.java:870)_

_| at (#52:1)_

**jshell>**

* The code ```System.out.printf("%d + %d + %d = %d", 5, 6, 7).println()``` is food for  thought. There are four format specifiers (all '```%d```' for ```int```). However, just three arguments follow it. This is not acceptable in Java, and ```JShell``` immediately *throws* an **exception**.

**jshell>**

 ```System.out.printf("%d + %d + %d", 5, 6, 7, 8).println()```

_5 + 6 + 7_

**jshell>**

* The statement ```System.out.printf("%d + %d + %d", 5, 6, 7, 8).println()``` seems to work just fine! That's because if ```System.out.printf()``` is called with excess parameters, they are simply ignored. The program continues running as if nothing has happened.

## Other Format Specifiers

So far, we've just stuck to formatting integers, and their expressions. Let's try to format other basic Java types, shall we?


**jshell>** ```System.out.printf("Print %s", "Testing").println()```

_Print Testing_

**jshell>** ```System.out.printf("%d + %d + %d", 5.5, 6.5, 7.5).println()```

_| java.util.IllegalFormatConversionException thrown: d != java.lang.Double_

_| at Formatter$FormatSpecifier.failedConversion(Formatter.java:4331)_

_| at Formatter$FormatSpecifier.printInteger(Formatter.java:2846)_

_| at Formatter$FormatSpecifier.print(Formatter.java:2800)_

_| at Formatter.format(Formatter.java:2581)_

_| at PrintStream.format(PrintStream.java:974)_

_| at PrintStream.print(PrintStream.java:870)_

_| at #(57:1)_

**jshell>** ```System.out.printf("%f + %f + %f", 5.5, 6.5, 7.5).println()```

_5.500000 + 6.500000 + 7.500000_

**jshell>**

As you can see with string data, ```%s``` is the right format specifier to use. However, even though both integers and floating point data are numbers, their format specifiers are different. Only use ```%f``` for floating-point data.

## Formatting for a combination of data

**jshell>** ```System.out.printf("I saw %f exactly %d time", 7.5, 1).println()```

_I saw 7.500000 exactly 1 time_

**jshell>**

Did you notice that in the code above, we have combined data of three types (string, floating-point and integer), in a single format string? That is the beauty of ```System.out.printf()```, short and sweet!

Let's now summarize what we've learned in this article.

## Summary

We have just explored the following areas:

* Even though ```System.out.println()``` is useful, it cannot format all kinds of data
* ```System.out.printf()``` can format a combination of data, by using format specifiers.
* ```System.ut.println()``` can be used on top of ```System.out.printf()```, for neat output.

We hope this article leaves you better placed to format your data, even when they are a combination of different types. We'll see you again soon. Until then, bye-bye!


## Next Steps
- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA