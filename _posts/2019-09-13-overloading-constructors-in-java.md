---
layout:     post
title:      Overloading Java Constructors - Be careful about it
date:       2018-09-13 11:58:00
summary:    Explains what choices are available to maximize reuse of constructor code
categories: Java, Constructors
permalink:  /overloading-java-constructors-smartly
---

## You will learn

* Can a class have multiple constructors in Java?
* What is method overloading, and constructor overloading in Java?
* What are the advantages of constructor overloading in Java?

## We will discuss:

* The way we can overload constructors, to give us object creation options
* How we can cause constructor code to be reused, by calling one constructor within another

## We assume you are aware of:

* The meaning of terms such as constructor, new operator, method overloading

## Tools you will need

* The JDK Environment installed
* A code editor or an IDE, such as Eclipse

## Programming Courses

- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- [Spring Boot for Beginners in 10 Steps](https://courses.in28minutes.com/p/spring-boot-for-beginners-in-10-steps){:target="_blank"}
- [Complete in28Minutes Course Guide](https://courses.in28minutes.com/p/in28minutes-course-guide){:target="_blank"}

## What is a Constructor?

A constructor’s role is to hold code for object initialization. An object is created using the Java new operator, which allocates memory for the object, then invokes the class constructor to initialize it. Note that a constructor can be invoked on an object exactly once during its lifetime, never after that.

Just like any regular method, a constructor may also have overloaded definitions. How does that change object creation and initialization, when all you can do is call exactly one of them, ever? Constructor overloading provides flexibility in object creation, which is worth its weight in gold in many situations.

Let's assume the boss at a company invests his time and money, into getting a coffee vending machine operational. For this, he hires a professional programmer to write code for it. The programmer quickly realizes there is no one cure for all ailments. There are different kinds of coffee people drink, and factors such as:

* Coffee Bean Type (Robusta? Arabica?)
* Milk Type (Skimmed? Toned? Rich? No Milk?)
* Sugar / No Sugar
* Hot / Cold (how hot? how cold?)
* Cream / No Cream

matter a lot. The programmer decides to feed in different recipes to the kiosk, as the coffee making part remains the same. In addition, constructors are preferred to set() methods for object instantiation:

#### Example-01: Initial set of Constructors

```java
public class CoffeeRecipe {
    // Hot black coffee, no sugar or cream
    public CoffeeRecipe(String bean, short temperature) {
        /*  */ }

    // Hot coffee with milk, no sugar, no cream
    public CoffeeRecipe(String bean, String milkType, short temperature) {
        /*  */ }

    // Hot coffee with milk, sugar, no cream
    public CoffeeRecipe(String bean, String milkType, boolean addSugar) {
        /* */ }

/*
    // Hot Coffee with milk, sugar, cream
    CoffeeRecipe(String bean, String milkType, boolean addSugar, boolean withCream) {
         }

    // Cold Coffee with milk, sugar, no cream
    CoffeeRecipe(String bean, String milkType, boolean addSugar) {
         }

    // Cold Coffee with milk, sugar, and cream
    CoffeeRecipe(String bean, String milkType, boolean addSugar, boolean withCream) {
         }*/
}
```
There may be many more. The CoffeMaker kiosk will consume different CoffeeRecipes:

```java
    public class CoffeeMaker {
    	public void makeCoffee(CoffeeRecipe recipe){ /*    */ }
    	public static void main(String[] args) {
    		CoffeeRecipe hotBlackArabica = new CoffeeRecipe("arabica");
    		makeCoffee(hotBlackArabica);
    		CoffeeRecipe hotLeanRobusta = new CoffeeRecipe("robusta", "skimmed");
    		makeCoffee(hotLeanRobusta);
    		CoffeeRecipe coldRichRobusta = new CoffeeRecipe("robusta", "rich", true, true);
    		makeCoffee(coldRichRobusta);
    	}
    }
```

#### Example-01 Explained

Note a few points in this (hurriedly assembled) CoffeeRecipe class:

* Too many constructors, enough to make even the programmer grumble
* An inconsistent set of constructor parameters:
	*Coffee temperature is specified for making hot coffee, not the cold one
	* The boolean addSugar parameter is specified only for some definitions. Then why is it a boolean? Ditto for the boolean withCream parameter.

## Trimming down the ```CoffeeRecipe``` and Runner Classes

I agree with you, an API needs to be consistent. In that case, we would need just one constructor:

#### Example-02: The ```CoffeeRecipe``` and Runner classes
```java
    public class CoffeeRecipe {
    CoffeeRecipe(String bean, String milkType, boolean addSugar, boolean withCream, shortTemperature) { /* */
     }
    }
```

And the coffee maker’s code would be along these lines:

```java
    public class CoffeeMaker {
    	public void makeCoffee(CoffeeRecipe recipe){ /*    */ }
    	public static void main(String[] args) {
    		CoffeeRecipe hotBlackArabica = new CoffeeRecipe("arabica", "no-milk", false, false, 150);
    		makeCoffee(hotBlackArabica);
    		CoffeeRecipe hotLeanRobusta = new CoffeeRecipe("robusta", "skimmed", true, false, 120);
    		makeCoffee(hotLeanRobusta);
    		CoffeeRecipe coldRichRobusta = new CoffeeRecipe("robusta", "rich", true, true, 20);
    		makeCoffee(coldRichRobusta);
    	}
    }
```

#### Example-02 Explained

Now, a coffee maker kiosk needs a simple interface, and one that seems to have some common sense:

* Black coffee is without milk, sugar or cream.
* Cold coffee generally has milk and sugar
* People are sometimes not fussy about temperature, only things like “hot”, “warm”, “steaming” and “ice-cold” would do.
* People are sometimes too lazy to give coffee options, such as its temperature above. The coffee maker needs to make sensible assumptions as well!

## A User Friendly ```CoffeeMaker```

A user-friendly ```CoffeeMaker``` allows itself to be used this way:

#### Example-03: ```CoffeeMaker``` Revisited

```java
    public class CoffeeMaker {
    	public void makeCoffee(CoffeeRecipe recipe){ /*    */ }
    	public static void main(String[] args) {
    		// user knows black coffee
    		CoffeeRecipe hotBlackArabica = new CoffeeRecipe("arabica");
    		makeCoffee(hotBlackArabica);
    		// adding milk, so give temperature
    		CoffeeRecipe hotLeanRobusta = new CoffeeRecipe("robusta", "skimmed", true, false, "hot");
    		makeCoffee(hotLeanRobusta);
    		// cold coffee, buddy
    		CoffeeRecipe coldRichRobusta = new CoffeeRecipe("robusta", "rich", 20);
    		makeCoffee(coldRichRobusta);
    	}
    }
```

#### Example-03 Explained

We need to support several constructors, but essentially recipe creation would not change. What would be the way, then, to reuse this common code? Encapsulate it within a single constructor, and call it where needed. 

## ```CoffeeRecipe``` Revisited

The code we need is similar to what we saw, a little earlier:

#### Example-04: ```CoffeeRecipe``` Revisited

```java
    public class CoffeeRecipe {
    CoffeeRecipe(String bean, String milkType, boolean addSugar, boolean withCream, String howHot) { /* */ }
    }
```

was the most general constructor version. We can call it from within other common-sense constructors, with some fixed parameter values:

```java
    public class CoffeeRecipe {
    //Hot black coffee, no sugar or cream
    	public CoffeeRecipe(String bean) { 
    		this(bean, "no-milk", false, false, "steaming");
    	}
    //hot or cold, lean coffee, with milk, sugar and no cream
    	public CoffeeRecipe(String bean, String milkType, String howHot) {
    		this(bean, milkType, true, false, howHot);
    	}
    	//Hot or cold,  coffee with milk and cream, so add sugar too!
    	public CoffeeRecipe(String bean, String milkType, boolean withCream, String howHot) {
    		this(bean, milkType, true, withCream, howHot);
    	}
    	// Hot or cold, Coffee with milk, sugar, cream choices
    	CoffeeRecipe(String bean, String milkType, boolean addSugar, boolean withCream, String howHot) { 
    		//The most general functionality constructor
    		/*
    		...
    		*/
    	}
    }
```

#### Example-04 Explained

The programmer has pulled off a fine balancing act, drawing a cheer from the crowd lined up at the kiosk! The boss was never happier, and drinks the coffee himself!

Note that if programmer does not define a constructor, the Java compiler generates one. That constructor performs default-initialization of the object’s member variables. However, if the programmer provides even one overloaded definition, the compiler will refuse to generate other default versions. Do them all yourself, if you don’t like what I do for you!

## Summary

In summary: 

* Constructor overloading gives the programmer the advantage of code flexibility 
* Constructor code reuse is one other advantage that can be gained if we keep the programming interface simple
* We also understand that if we define at least one constructor in any class we write, the compiler stops generating constructors from its side.


## Next Steps
- [Java Programming in 250 Steps](https://courses.in28minutes.com/p/java-tutorial-for-beginner-in-250-steps){:target="_blank"}
- [Python Programming in 250 Steps](https://www.udemy.com/course/python-tutorial-for-beginners/?couponCode=OCTOBER-2019){:target="_blank"}
- [Python For Java Programmings](https://www.udemy.com/course/learn-python-programming-for-java-programmers?couponCode=OCTOBER-2019){:target="_blank"}
- Learn Basics of Spring Boot - [Spring Boot vs Spring vs Spring MVC](http://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring){:target="_blank"}, [Auto Configuration](http://www.springboottutorial.com/spring-boot-auto-configuration){:target="_blank"}, [Spring Boot Starter Projects](http://www.springboottutorial.com/spring-boot-starter-projects){:target="_blank"}, [Spring Boot Starter Parent](http://www.springboottutorial.com/spring-boot-starter-parent){:target="_blank"}, [Spring Boot Initializr](http://www.springboottutorial.com/spring-initialzr-bootstrap-spring-boot-applications){:target="_blank"}


## Complete Code Example
- NA