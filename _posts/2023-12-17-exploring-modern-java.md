---
layout: post
title: "Getting re-Java-nated: Exploring Modern Java"
comments: true
tags:
  - Java
  - Lambda
  - Quarkus
  - GraalVM
excerpt_separator: <!--more-->
---

If the last time you worked with Java was close to a decade ago, then you would be forgiven for feeling a bit out of place when you encounter modern versions of Java.

Since the release of Java 8 in 2014, the language has gone through substantial transformations.

In this article, we are going to look at how Java (the language, as well as its ecosystem), has changed over the years to become what it is today.
<!--more-->

## A (very) brief history of Java

Java was developed at Sun Microsystems and first released in 1995. The lead designer of the languagage was [James Gosling](https://en.wikipedia.org/wiki/James_Gosling). Sun was acquired by Oracle in 2010. The first release of Java under Oracle was Java 7 that came out in July 2011. 

## Java 8 and the beginning of modern Java

The release of Java 8 in March 2014 marked perhaps the most significant transformation of the language since Java 5 (or Java 1.5, as it was then called), which came out in September 2004, introducing features like generics, annotations, and enums.

With Java 8, the language introduced several functional programming features: 

- Lambda expressions and functional interfaces
- Method references
- Stream API


### Functional interfaces and Lambda expressions

Here's a simple example of a functional interface in Java:

```java
@FunctionalInterface
public interface TaxFunction {
    double calculateTax(double grossIncome);
}
```

> Note the use of the `@FunctionalInterface` annotation to define a functional interface.

We can then use the functional interface as shown below:

```java
public class Customer {

    private String name;

    private double grossIncome;

    public Customer(String name, double grossIncome) {
        this.name = name;
        this.grossIncome = grossIncome;
    }

    // getters and setters omitted...

    public double calculateApplicableTax(TaxFunction taxFunc) {
        return taxFunc.calculateTax(this.getGrossIncome());
    }
}
```

And here's an example of a lambda expression used together with the functional interface:

```java
public class TestClass {

    public static void main(String[] args) {

          // define a function as a lambda expression and assign the result to a variable
          TaxFunction taxFunction = (grossIncome) -> {
              double payableTax;

              if (grossIncome < 30000) {
                  payableTax = grossIncome * 0.05;
              } else {
                  payableTax = grossIncome * 0.06;
              }

              return payableTax;
          };

          Customer customer = new Customer("Jane Doe", 50000);

          double calculatedTax = customer.calculateApplicableTax(taxFunction); // we pass in a lambda expression

          String name = customer.name();

          System.out.println("The calculated tax for " + name + " is "+ calculatedTax); // prints: The calculated tax for Jane Doe is 3000.0
    }
}
```

### Stream API for processing collections

Here's an example of using the Stream API to process a Java collection:

```java
import java.util.HashMap;
import java.util.Map;

public class TestClass {

    public static void main(String[] args) {

          Map<String, String> houses = new HashMap<>(Map.of(
                "Stark", "Winter Is Coming",
                "Targaryen", "Fire and Blood",
                "Lannister", "Hear Me Roar",
                "Arryn", "As High as Honor",
                "Tully", "Family, Duty, Honor",
                "Greyjoy", "We Do Not Sow",
                "Baratheon", "Ours is the Fury",
                "Tyrell", "Growing Strong",
                "Martell", "Unbowed, Unbent, Unbroken",
                "Hightower", "We Light the Way"
                ));

          houses.entrySet()
                .stream()  // turning the collection into a stream
                .forEach(house -> System.out.println(
                                  "House: " + house.getKey() 
                                            + "," + " Motto: "
                                            + house.getValue()));
    }
}
```

The output of the code snippet above looks as follows:

```text
House: Lannister, Motto: Hear Me Roar
House: Targaryen, Motto: Fire and Blood
House: Baratheon, Motto: Ours is the Fury
House: Hightower, Motto: We Light the Way
House: Martell, Motto: Unbowed, Unbent, Unbroken
House: Tyrell, Motto: Growing Strong
House: Tully, Motto: Family, Duty, Honor
House: Stark, Motto: Winter Is Coming
House: Greyjoy, Motto: We Do Not Sow
House: Arryn, Motto: As High as Honor
```

Before Java 8, we would probably write the previous code as shown below:

```java
import java.util.HashMap;
import java.util.Map;

public class TestClass {

    public static void main(String[] args) {

          Map<String, String> houses = new HashMap<>();

          houses.put("Stark", "Winter Is Coming");
          houses.put("Targaryen", "Fire and Blood");
          houses.put("Lannister", "Hear Me Roar");
          houses.put("Arryn", "As High as Honor");
          houses.put("Tully", "Family, Duty, Honor");
          houses.put("Greyjoy", "We Do Not Sow");
          houses.put("Baratheon", "Ours is the Fury");
          houses.put("Tyrell", "Growing Strong");
          houses.put("Martell", "Unbowed, Unbent, Unbroken");
          houses.put("Hightower", "We Light the Way");

          // using the enhanced for statement to process the collection
          for (Map.Entry<String, String> house : houses.entrySet()) {
                System.out.println("House: " + house.getKey()
                                             + "," + " Motto: "
                                             + house.getValue());
          }
    }
}
```

Running the code above produces the same result as before.

Apart from the functional programming utilities describe above, Java 8 also introduced `default` and `static` methods in interfaces.

## Java 9 and the introduction of modules

Java 9, which came out in September 2017 introduced, among other things, two important features:

- Java Platform Module System (JPMS)
- JShell, an interactive Java REPL

The Java Platform Module System introduced the concept of modules in Java. A `module` is basically a grouping of closely related packages. A module contains a `module descriptor file` that defines several aspects of the module, for example, the module's name, a list of its dependencies, a list of its (public) packages, etc. A module allows for another layer of encapsulation above the package level.

Here's a simple example of a module descriptor file (`module-info.java`):

```java
module org.example {  
  requires java.base;

  exports org.example.hello;  
}
```

JShell, on the other hand, is an interactive tool for learning Java and allows for easy prototyping of Java code. JShell is a Read-Evaluate-Print Loop tool (REPL), which evaluates declarations, statements, and expressions as they are entered and immediately shows the results. The tool is run from the command line. JShell was inspired by similar tools in languages like Python and Ruby.

## Project Amber and paving the on-ramp

Over the years, Java has gained a reputation for being overly verbose and less approachable to beginners. [Project Amber](https://openjdk.org/projects/amber/) was introduced to address some of these concerns by exploring productivity-oriented Java language features.

Some of the interesting features that have come out of Project Amber so far are: 

- Local Variable Type Inference (var)
- Text Blocks
- Unnamed Classes and Instance Main Methods

### Local variable type inference using `var`

This feature was introduced in Java 10. Instead of writing explicit types for variables, you can use `var`, and the variable type will then be inferred by the compiler.

Here's the previous code example rewritten using `var`:

```java
import java.util.HashMap;
import java.util.Map;

public class TestClass {

    public static void main(String[] args) {

          // using var instead of explicit type annotation
          var houses = new HashMap<String, String>();

          houses.put("Stark", "Winter Is Coming");
          houses.put("Targaryen", "Fire and Blood");
          houses.put("Lannister", "Hear Me Roar");
          houses.put("Arryn", "As High as Honor");
          houses.put("Tully", "Family, Duty, Honor");
          houses.put("Greyjoy", "We Do Not Sow");
          houses.put("Baratheon", "Ours is the Fury");
          houses.put("Tyrell", "Growing Strong");
          houses.put("Martell", "Unbowed, Unbent, Unbroken");
          houses.put("Hightower", "We Light the Way");

          // using var in for-each loop
          for (var house : houses.entrySet()) {
                System.out.println("House: " + house.getKey()
                                             + "," + " Motto: "
                                             + house.getValue());
          }
    }
}
```

### Text blocks

Text blocks became a standard feature in Java 15. Text blocks were added to provide clarity by way of minimizing the Java syntax required to render a string that spans multiple lines. This comes in handy when you want to embed HTML or SQL queries in your code.

Here's a simple example of using a text block:

```java
String someHtml = """
                  <html>
                    <body>
                      <p>Here goes some text</p>
                    </body>
                  </html>
                  """;
```

This certainly looks much cleaner than using a bunch of string concatinations.

### A smooth on-ramp for Java beginners

Virtually everyone who starts to learn Java is "forced" to memorize this piece of code:

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

Of course there's a lot to unpack here:

- what is a `class`?
- what does `void` mean?
- what does `static` mean?
- what is `System.out`?
- why `public`?

For someone who is brand new to programming and who's using Java as their entry point to the field, requiring an understanding of all of these concepts at the very beginning might be a bit too much to ask.

Compare this to what a beginner using Python would need to do:

```python
print("Hello, World!")
``` 

This is where [JEP 445](https://openjdk.org/jeps/445) (Unnamed Classes and Instance Main Methods) comes in.

The main goal of this JEP is to eventually allow a beginner in Java to write _Hello, World_ as shown below:

```java
void main() {
    println("Hello, World");
}
```

## Java 17 and data-oriented programming

Java 17 (which came out in September 2021) introduced `sealed` classes and interfaces. This feature, combined with `records` (which were introduced in Java 16), and `pattern matching`, enable data-oriented programming in Java.

### Modeling data as data using records

Records (or record classes) are special types of classes that represent immutable data carriers.

Java records are similar to Kotlin's [data classes](https://kotlinlang.org/docs/data-classes.html).

Here's an example of a record in Java:

```java
record Rectangle(int length, int width) { }
```

### Better type hierarchies using sealed classes

A sealed class (or interface) can only be extended or implemented by those classes and interfaces permitted to do so.

Here's an example that shows the usage of sealed classes in Java:

```java
package dev.example.geometry;

// only three classes are permitted to extend the `Shape` class
public abstract sealed class Shape
    permits Circle, Rectangle, Triangle { ... }

public final class Circle extends Shape { ... }

public sealed class Rectangle extends Shape 
    permits Square { ... }

public final class Square extends Rectangle { ... }

public final class Triangle extends Shape { ... }
```

### Pattern matching in Java

Let's consider the following Java interface:

```java
public interface Shape {

    public double area();
}
```

And the following types that implement the `Shape` interface:

```java
record Circle(double radius) implements Shape {

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

record Rectangle(double length, double width) implements Shape {

    @Override
    public double area() {
        return length * width;
    }
}

record Triangle(double base, double height) implements Shape {

    @Override
    public double area() {
        return 0.5 * base * height;
    }
}
```

Now, here's how we can use pattern matching with `switch expression` to calculate the area of each type of shape:

```java
public static double calculateAreaOfShape(Shape shape) throws IllegalArgumentException {
    return switch (shape) {
        case Circle c    -> c.area();
        case Rectangle r -> r.area();
        case Triangle t  -> t.area();
        default          -> throw new IllegalArgumentException("Unrecognized shape");
    };
}
```

Before the addition of pattern matching functionality in Java, this is how we would implement the `calculateAreaOfShape` method:

```java
public static double calculateAreaOfShape(Shape shape) throws IllegalArgumentException {
    if (shape instanceof Rectangle r) {
        return r.area();
    } else if (shape instanceof Circle c) {
        return c.area();
    } else if (shape instanceof Triangle t) {
        return t.area();
    } else {
        throw new IllegalArgumentException("Unrecognized shape");
    }
}
```

## Java 21 and virtual threads

Java 21 (which came out in September 2023) introduced the [Thread.Builder](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/lang/Thread.Builder.html) API for creating and starting both `platform` and `virtual` threads. Platform threads are the traditional threads in Java that are thin wrappers around OS threads. [Virtual threads](https://openjdk.org/jeps/444) on the other hand are lightweight threads that are not directly tied to OS threads.

Here's how we can use the thread builder API to create a platform thread in Java:

```java
Thread platformThread = Thread.ofPlatform().unstarted(() -> {
                        System.out.println("Hello from a platform thread!"); });

platformThread.start(); // start platform thread
platformThread.join();  // wait for thread to finish work
```

And here's how we can create a virtual thread:

```java
Thread virtualThread = Thread.ofVirtual().unstarted(() -> {
                       System.out.println("Hello from a virtual thread!"); });

virtualThread.start(); // start virtual thread
virtualThread.join();  // wait for thread to finish
```


## GraalVM and Project Leyden

Java has had a reputation for having relatively long "warmup" times. This is especially _undesirable_ for modern cloud-based applications that run in containerized/serverless environments. GraalVM and Project Leyden are attempts to improve Java startup times (and memory footprint), and make it suitable for building modern cloud-native  applications.

[GraalVM](https://www.graalvm.org/) is a high-performance JDK distribution that compiles Java applications ahead of time into native binaries. These binaries start instantly, provide peak performance with no warmup, and use fewer resources. GraalVM was created and is maintained by Oracle Labs.

[Project Leyden](https://openjdk.org/projects/leyden/), on the other hand, aims to port GraalVM features back to the reference OpenJDK implementation of the Java language.

## Other improvements in the Java ecosystem

Here are some other improvements that have been seen in the wider Java ecosystem over the recent years:

### New modern frameworks

If you are a Java veteran, then you would most likely be familiar with [Spring](https://spring.io/), which has been the dominant framework in the Java ecosystem (and in many ways, still is). But in recent years, some promising new frameworks have emerged.

Here are some of the popular new Java frameworks:

- [Quarkus](https://quarkus.io/): Open-source framework developed by Red Hat.
- [Micronaut](https://micronaut.io/): Developed by Graeme Rocher, who is also the creator of the [Grails](https://grails.org/) framework.
- [Helidon](https://helidon.io/): Open-source framework developed by Oracle.


### Java playground

Following in the footsteps of other languages like Go and Rust, Java now has a 
[playground](https://dev.java/playground/) as well. This is where you can play around with little Java code snippets in the browser without installing any tools locally. 

### A new consistent and predictable JDK release cadence

Beginning with Java 7, Oracle has moved to a 6-month JDK release cadence, with LTS (Long Term Support) versions released [every two years](https://blogs.oracle.com/java/post/moving-the-jdk-to-a-two-year-lts-cadence).

## Summary

While Java has been around for a relatively long time, it keeps evolving to meet the needs of modern software development.

In this article, we've looked at some of the important changes that the language has gone through over the years.

