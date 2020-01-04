---
layout:     post
title:      Executor framework in Java - Take control over your tasks 
date:       2018-09-11 07:47:00
summary:    Understand how to use Executor from in Java with Code Examples
permalink:  /executor-framework-in-java-with-examples
---

## You will learn

* What are Executors in Java?
* What are the advantages of using Executors in Java?

## We will discuss:

* How Executors address certain weaknesses of the Thread class API
* How we can use Executors to fine-tune execution of thread pools
* How Executors allow threads to synchronize with each other

## We assume you are aware of:

* The meaning of terms such as thread, synchronization, scheduling, time-slice

## Tools you will need

* The JDK Environment installed
* A code editor or an IDE, such as Eclipse


## Programming Courses

- [Java Programming in 250 Steps](https://links.in28minutes.com/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://links.in28minutes.com/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://links.in28minutes.com/MISC-JAVA-PYTHON){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}


## Threads And Concurrency

Any program that runs on a single-processor machine, goes through with its code in a *single sequence*. Concurrency changes the *single sequence* part, by providing the concept of a *thread*. A thread consists of a single sequence of instructions, so it's similar in meaning to a program. What stands out however, is that independent threads can be executed **concurrently**. In programming terms, this means that even though our main program runs on a single processor, individual threads within it could achieve progress. 

The Java Run-time has a **thread scheduler** running to help with this. Whatever **time-slice** the OS scheduler gives to a program, is split among its threads by the thread scheduler. What follows is processor **time-sharing**, and these threads overlap with each other as they run. If certain parts of a program need to wait for an external event, other parts that do not, could be allowed to proceed.

## Introducing The Executors framework

In order to address some serious limitations of the ```Thread``` API, a new ```Executor Service``` was added in **Java SE 5**. The ```ExecutorService``` is a framework you can use to manage threads in your code, better.  It has built-in ways to:

* Create and launch threads more intuitively
* Manage thread state and its life-cycle more easily
* Synchronize between threads with more control, and
* Handle groups of threads neatly

The ```ExecutorService``` provides utilities to achieve each one of these. Let's start with thread creation, which the next example takes care of.

#### Example-01 : Creating a Thread with Executor Service

```java
//**_ExecutorServiceRunner.java_**
import java.util.concurrent.ExecutorService;

import java.util.concurrent.Executors;

class Task1 extends Thread {

	public void run() {

		System.out.println("Task1 Started ");

		for (int i = 101; i <= 199; i++) {

			System.out.print(i + " ");

		}

		System.out.println("\nTask1 Done");

	}

}

class Task2 implements Runnable {

	@Override

	public void run() {

		System.out.println("Task2 Started ");

		for (int i = 201; i <= 299; i++) {

			System.out.print(i + " ");

		}

		System.out.println("\nTask2 Done");

	}

}

public class ExecutorServiceRunner {

	public static void main(String[] args) {

		ExecutorService executorService = Executors.getNewSingleThreadExecutor();

		executorService.execute(new Task1());

		executorService.execute(new Thread(new Task2()));

	}

}

```

**_Console Output_**

_Task1 Started_

_101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199_

_Task1 Done_

_Task2 Started_

_201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 267 268 269 270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 285 286 287 288 289 290 291 292 293 294 295 296 297 298 299_

_Task2 Done_

#### Example-01 Explained

* An ```ExecutorService``` instance is returned by the the ```Executors``` framework  ```class```. On this reference, invoking ```getNewSingleThreadExecutor()``` returns you a reference to a single, manageable thread. Sure, this code will create just a  single sub-task, so is equivalent to a single-threaded program. But doesn't the code look neater and more compact, already?

## More of Executors

Let's now have a taste of the real thing, creating  more than one thread using ```ExecutorService```!

#### Example-02 : Running threads Concurrently using Executor Service

```java
public class ExecutorServiceRunner {

	public static void main(String[] args) {

		ExecutorService executorService = Executors.getNewSingleThreadExecutor();

		executorService.execute(new Task1());

		executorService.execute(new Thread(new Task2()));

		System.out.print("\nTask3 Kicked Off\n");

		for (int i = 301; i <= 399; i++) {

			System.out.print(i + " ");

		}

		System.out.println("\nTask3 Done");

		System.out.println("\nMain Done");

		executorService.shutdown();

	}

}

```

#### Example-02 Explained

The only order in the resulting chaos is: *Task2* starts execution only after *Task1* is done. This tells us that a ```SingleThreadExecutor``` service can be used to control thread execution, but only of threads availing its services! The thread running ```main()``` are out of bounds for the ```ExecutorService```.

## Customizing Number Of Threads

With the ```ExecutorService```, it is possible to create a *pool* of threads. Such a group of threads share similar properties, regarding they ways in which you can control, and manage them. Normally, you can pass in a parameter to a factory method that creates this thread pool, which indicates its size.

The following examples will show you how you can create thread pools of varying kinds, and of course, of different sizes. 

#### Example-03 : Executors for Concurrent threads

```java
//**_ExecutorServiceRunner.java_**
public class ExecutorServiceRunner {

	public static void main(String[] args) {

		ExecutorService executorService = Executors.getNewFixedThreadPool(2);

		executorService.execute(new Task1());

		executorService.execute(new Thread(new Task2()));

		System.out.print("\nTask3 Kicked Off\n");

		for (int i = 301; i <= 399; i++) {

			System.out.print(i + " ");

		}

		System.out.println("\nTask3 Done");

		System.out.println("\nMain Done");

		executorService.shutdown();

	}

}
```

#### Example-03 Explained

By using an ```ExecutorService``` of type ```FixedThreadPool```:

* *Task1* and *Task2* execute concurrently as part of the ```ExecutorService```, and
* The thread running ```main()``` executes concurrently with this thread pool, created by ```ExecutorService```.


## More on Thread Pools

Let's explore one more scenario involving thread pool creation.

#### Example-04 : All-Executor Task Execution

```java
//**_ExecutorServiceRunner.java_**
import java.util.concurrent.ExecutorService;

import java.util.concurrent.Executors;

class Task extends Thread {

	private int number;

	public Task(int number) {

		this.number = number;

	}

	public void run() {

		System.out.println("Task " + number + " Started");

		for (int i = number * 100; i <= number * 100 + 99; i++) {

			System.out.print(i + " ");

		}

		System.out.println("\nTask " + number + " Done");

	}

}

public class ExecutorServiceRunner {

	public static void main(String[] args) {

		ExecutorService executorService = Executors.getNewFixedThreadPool(2);

		executorService.execute(new Task(1));

		executorService.execute(new Task(2));

		executorService.execute(new Task(3));

		executorService.shutdown();

	}
}
```

#### Example-04 Explained

* Note that we have now created all sub-tasks as threads, which belong to the same thread pool.
* The thread ```new Task(3)``` is executed only after any one of ```new Task(1)``` and ```new Task(2)``` have completed their execution.  

#### Example-05 : Larger Thread Pool Size

```java
public class ExecutorServiceRunner {

	public static void main(String[] args) {

		ExecutorService executorService = Executors.getNewFixedThreadPool(3);

		executorService.execute(new Task(1));

		executorService.execute(new Task(2));

		executorService.execute(new Task(3));

		executorService.execute(new Task(4));

		executorService.execute(new Task(5));

		executorService.execute(new Task(6));

		executorService.execute(new Task(7));

		executorService.shutdown();

	}
}

```

#### Example-05 Explained

When a larger pool size is selected, say ```3```, an interesting scenario plays out:

* Initially Tasks ```1```, ```2```, ```3``` are added to the ```ExecutorService``` and are started.
* As soon as any one of them is terminated, another task from the thread pool is marked for execution, and so on. A classic case of musical chairs, with regard to the slots available in the ```ExecutorService``` thread pool, is played out here!

#### Summary

In summary, we have learned about the following concepts when using the Executors framework:

* You can create pools of threads of the same kind, using the ```ExecutorService```
* One could specify the size of a thread pool, as different from the executor threads


## Next Steps
- [Java Programming in 250 Steps](https://links.in28minutes.com/MISC-JAVA){:target="_blank"}
- [Python Programming in 250 Steps](https://links.in28minutes.com/MISC-PYTHON){:target="_blank"}
- [Python For Java Programmings](https://links.in28minutes.com/MISC-JAVA-PYTHON){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA