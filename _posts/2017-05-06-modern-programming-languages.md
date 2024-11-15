---
layout: post
title: The Rise of Modern Programming Languages
comments: true
tags: Rust Golang Scala Swift Kotlin
excerpt_separator: <!--more-->
---

Nineteen ninety-five was a particularly interesting year in the world of computer programming. During this year, four new programming languages were released that would go on to influence the global programming community in ways not anticipated at the time of their official announcements. Interestingly, this period also coincided with the early days of the web, as the Internet was on the verge of exploding into mainstream digital culture.

<!--more-->

> Disclaimer: This article was originally published as an essay on Medium in 2017. You can check it out [here](https://medium.com/@chiedo.chrispine/the-rise-of-modern-programming-languages-c923a2b914fc).

The four languages that made their debut in 1995 were **Java**, **JavaScript**, **PHP** and **Ruby**. Although there wasn’t much fanfare accompanying their respective releases, these languages would eventually grow to become ubiquitous programming tools for most software developers. Up until that time, **C** and **C++** were the dominant languages. Despite the fact that these two were very powerful, they were not inherently suited for the world-wide web. Additionally, they were often considered to be a little complicated and intimidating to novice programmers (especially C++).

Among the four, Java proved to be a runaway success. With its oft-quoted slogan of "**Write Once, Run Anywhere**", Java became an instant hit as it was much easier to learn and master (compared to C++). Java came with a Virtual Machine (the **JVM**), that made it possible to write programs that would be run on different platforms without the need for recompilations.

In recent years, **JavaScript** has displaced Java as the [top](https://insights.stackoverflow.com/survey/2017) programming language in the world. The rise of JavaScript is largely attributed to the introduction of [Node.js](https://nodejs.org/en/), a technology that made it possible for JavaScript to be run on the _server-side_.

**PHP** became a dominant force in web programming, especially in combination with other popular open-source technologies like **Linux**, **Apache** and **MySQL**. Together, they formed what was commonly referred to as the **LAMP** stack.

**Ruby** became famous among web developers after the release of the [Ruby on Rails](http://rubyonrails.org/) web framework in 2005 by the Danish programmer **David Heinemeier Hansson** (DHH).

### The Need for New Programming Languages

Beginning in the early 2000s, the programming landscape started to change. More computing machines were shipping with multiple processors, and even individual processors were being designed with multiple cores. This shift in hardware architecture created a need for programming languages that could fully leverage these new processors. Languages needed to execute processes concurrently or in parallel to maximize the potential of multi-core processors. _Concurrency_ was no longer an afterthought; it had to be built into the language itself.

In recent years, there has been a resurgence of interest in [functional programming](https://en.wikipedia.org/wiki/Functional_programming), a paradigm that aims to minimize 
[side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science)). In fact, side effects have proven to be a major challenge in the life of modern programmers, often making the already-dreaded task of debugging even more difficult. In concurrent programming environments, data _safety_ and _immutability_ become essential considerations for developers. Preventing data _corruption_ and _race conditions_ is critical is such environments.

Last but not least, modern machines have become increasingly powerful. While performance speed was a primary concern in the past, recent focus has shifted toward _programmer productivity_. As a result, programming languages benefit from having clean, elegant syntax that is easy to write and read. A new developer should be able to pick up the language quickly and start working with it in the shortest time possible. Ideally, one should be able to experiment with the language immediately using an interactive loop, commonly known as a **REPL** (Read-Eval-Print Loop), without dealing with tedious setup or compilation processes.

### Teaching an Old Dog New Tricks

To stay relevant in the evolving programming landscape, significant efforts have been made to update older languages like Java and C++ to meet modern demands. For example, Java introduced _Lambda Expressions_ and the _Stream API_ in its Java 8 release, and Java 9 will include a REPL (JShell). However, the main drawback of these ‘old’ languages is that they weren’t originally designed to address the challenges of modern programming. Retrofitting new features has not always been an elegant solution. As a result, we are increasingly turning to new programming languages, built from the ground up to tackle the complex issues in modern software engineering.

### Necessity is the Mother of Invention

Due to the factors discussed above (and many others not covered in this article), several programming languages have been developed to address some, if not all, of these challenges. I refer to these languages as '_modern_' because all of the them have been released within this century (i.e. from 2001 onwards).

As you will soon realize, some (if not most) of these new languages have a lot in common.

Some of the features shared by these modern languages are:

* Variables are typically _immutable_ by default.
* Most languages favor _type inference_ over explicit type annotations.
* There is a strong emphasis on _type safety_.
* Many feature _trailing_ function return types.
* Some provide simpler ways to spawn multiple threads (or processes) for _concurrent_ execution.
* Some offer easier mechanisms for communicating between processes through _channels_ or similar primitives.
* Many emphasize the _functional programming_ style, such as the use of pattern matching and lazy evaluation.
* Semicolons are generally _not required_ as statement terminators.
* Some languages include a _REPL_ for interactive programming.
* Many are typically _statically-typed_.
* Most often have clean/concise syntax with _minimal clutter_ and less verbosity.

### Popular Modern Programming Languages

The following may arguably be considered as the most 'popular' modern programming languages:

1. Scala
1. Golang (Go)
1. Rust
1. Kotlin
1. Swift

What follows is a brief look at some of the features of each of these languages. I will also highlight how each language implements _concurrency_, which is an important aspect that cuts across all the languages.

### 1. Scala

This is a language that made its debut in the first decade of this century and is arguably the 'oldest' modern programming language. It is a product of the academia, having been designed by Martin Odersky while working at the EPFL in Switzerland. First released in 2004, [Scala](https://www.scala-lang.org/) combines object-oriented and functional programming paradigms.

Applications written in Scala run on top of the JVM, hence they are easily portable to multiple platforms.

A simple function declaration in Scala is written as follows:

```scala
def factorial(x: Int): Int = {

      // The body of the function goes here...

}
```

A function in Scala is declared using the `def` keyword (similar to Python and Ruby). This is followed by the name of the function, in this case **factorial**, then a list of parameters in a pair of parentheses. Note that the parameter, in this case ***x***, is followed by a colon (**:**) and then its data type (`Int`, for an integer). If there are more than one parameter in the list, then they should be separated by a comma.
After the closing parenthesis of the function parameter list, we have another colon and an `Int` after it. This is the trailing function return type (more properly called the ***result type*** in Scala).
Following the function's result type is an equals sign (**=**) and a pair of curly braces that contain the body of the function. The equals sign might appear a little strange to those who are still new to Scala but it makes a lot of sense from a mathematical point of view. The contents within the curly braces may be considered as a mathematical expression that returns (or yields) a result depending on a particular set of input values. The result would then be assigned to whatever is to the left of the equals sign. In other words, when the ***factorial*** function is called, it will '***return***' whatever the result of the **expression** within the curly braces is.

Variables in Scala are declared using either the `val` or `var` (similar to JavaScript) keywords. Variables declared using the ***val*** keyword are **immutable** and therefore are not reassignable (this is the preferred option in Scala). On the other hand variables declared using the ***var*** keyword can be reassigned.
If a variable is given an initial value, then its type doesn't have to be declared explicitly, the interpreter can easily 'infer' its type from the initial value. For example:

```scala
val x = 20
```

In this case the type of '***x***' will be inferred to be an integer (`Int`).
If a variable has no initial value then its type must be declared explicitly, like so:

```scala
var x: Int
```

An important point to note is that Scala _requires_ you to initialize a variable, whether you intent for it to be immutable or not.
So for the case using *var* above, both the declaration and initialization of the variable can be done simultaneously, like so:

```scala
var x: Int = 20
```

***Concurrency in Scala***:

Scala uses ***Actors*** (based on the [Actor Model](https://en.wikipedia.org/wiki/Actor_model) proposed by **Carl Hewitt** in the early 1970s) to implement concurrency. The implementation method is heavily influenced by [Erlang](https://www.erlang.org/).
A simple example is shown below:

```scala
import scala.actors.Actor
import scala.actors.Actor._

val simpleActor = actor {
      loop {
            receive {
                 case s: String => println("This is a string: " + s)
                 case i: Int => println("This is an integer: " + i.toString)
                 case _ => println("I don't know what this is.")
            }
      }
}

simpleActor ! "scala is awesome"
simpleActor ! 32
simpleActor ! 32.134
```

When the code above is run, the output will be as shown below:

	This is a string: scala is awesome
	This is an integer: 32
	I don't know what this is.

The first two lines just import the **scala.actors** package together with its accompanying methods.
The line that begins with the ***val*** definition creates an **actor** called ***simpleActor*** that is able to receive messages depending on the cases expressed within the **receive** block.
An actor is a thread-like object that is able to receive and 'store' messages. Actors are usually mapped to native threads which can then be executed concurrently.
Actors communicate through passing messages to each other. To send a message to an actor, an exclamation mark or bang (**!**) is used, as shown below:

```scala
simpleActor ! "scala is awesome"
```

In this case, a string message, '*scala is awesome*' is send to the **actor** '***simpleActor***'. Note that the actor is always to the left of the bang and the message is always to the right.

When an actor receives a message it tries to find a corresponding match within the cases defined in the **receive** block using **pattern matching**. If no matching case is found, the default case (if any), will be executed. For example, in our sample program above, when a message containing a `float` (32.134) was sent to the actor, the default case was executed ("I don't know what this is"). Note the underscore (**_**) for the default case.

Every actor has a **mailbox** in which  messages sent to it are queued for further action. When an actor sends a message, it does not block, and when it receives a message, it is not interrupted.

An alternative to ***receive*** is ***react***, which unlike receive, does not return after it finds and processes a message. That is, its result type is '**Nothing**'. The advantage of using '*react*' instead of '*receive*' is that with react only a **single** thread is necessary (ideally) to host all of a program's actors.

One of the early adopters of Scala was **Twitter** way back in 2009.

The main drawback for Scala seems to be its steep learning curve.

One of the authoritative books on Scala is [Programming in Scala](https://www.amazon.com/Programming-Scala-Comprehensive-Step-Step/dp/0981531644) by Martin Odersky, Lex Spoon and Bill Venners. It is worth checking out.

### 2. Golang (Go)

This is a language that was originally created by Google for its own internal use. [Go](https://golang.org/) is particularly interesting because one of its designers (**Ken Thompson**) is a folklore hero in computer science. He is the creator of the venerable **UNIX** operating system, which he designed in the early 1970s while working at Bell Labs. Go was announced in 2009 and has since been released as an open-source project. The designers of the language have indicated that one of their motivations was their shared frustrations with the complexity inherent in C++.

Go is often touted as being '**C for the 21st Century**'. It is a compiled language (with much faster compile times compared to C/C++), and has automatic memory management through the use of a **garbage collector**.

One of the strengths of Go is how it handles concurrency using **goroutines** and **channels**.

A simple function declaration in Go looks as follows:

```go
func factorial(x int) int {

     // The body of the function goes here....

}
```

A function in Go is declared using the `func` keyword. This is followed by the name of the function and a list of parameters in a pair of parentheses. In this case, the factorial function only takes one argument, an integer **x**. The function also '***returns***' an integer, as indicated by the `int` keyword after the closing parenthesis of the argument list. The body of the function is enclosed within a pair of curly braces. Similar to Scala, the function return type comes _after_ the name of the function (_trailing_ return type), not before, as is the case with C or Java.

Variables in Go are declared using the keyword `var` like shown below:

```go
var s string
s = "Welcome to Go"
```

Of course, the declaration and initialization can be done at same time like so:

```go
var s string = "Welcome to Go"
```

Go also offers a shorter way of declaring variables as shown below:

```go
s := "Welcome to Go"
```

In this case the Go compiler will infer '***s***' as a `string`.

***Concurrency in Go***:

As already mentioned, concurrency is one of the areas in which Go shines the most.
Go uses *goroutines* and *channels* to implement concurrency.

A simple example is shown below:

```go
func main() {
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        for {
            c1 <- "hello from channel 1"
        }
    }()

    go func() {
        for {
            c2 <- "hello from channel 2"
        }
    }()

    go func() {
        for {
            select {
                case msg1 := <- c1:
                    fmt.Println(mg1)
                case msg2 := <- c2:
                    fmt.Println(mg2)
                default:
                    fmt.Println("Nothing is ready yet.")
            }
        }
    }()

    var input string
    fmt.Scanln(&input)
}
```

A **goroutine** is declared using the `go` keyword. Goroutines are thread-like entities that can be executed concurrently. Goroutines communicate through **channels**.

The channel type is indicated by the keyword `chan` followed by the type of the 'traffic' that the channel will handle, as shown below:

```go
c1 := make(chan string)
```

In this case we use the built-in `make` function to create a channel called **c1**, which can handle traffic of `string` type.

The left arrow operator (**<-**) is used to send (and receive) messages through the channel as shown below:

```go
c1 <- "hello from channel 1"
```

In this case, the string message '*hello from channel 1*' is '_send_' to the channel **c1**.

The receive construct is shown below:

```go
msg1 := <- c1
```

The code snippet above can be interpreted as:

   "receive a message from channel **c1** and store it in **msg1**."

The **select** block picks the first channel that is ready and receives a message from it. If more than one of the channels are ready, then it randomly selects which one to receive from.
If none of the channels is ready, the statement waits until a channel becomes available (a process commonly known as ***blocking***). But if a *default* case is provided, then it will be executed instead.

The most famous project implemented using Go is perhaps [Docker](https://www.docker.com/), the software technology for creating *containers*.

The following are two links that I have found to be great for a beginner in Go:

1. An interactive tour of Go can be found [here](https://tour.golang.org/).
2. If you want to learn Go by example, [here](https://gobyexample) is the place to go.

### 3. Rust

[Rust](https://www.rust-lang.org) is a systems programming language that was created by Mozilla Research as a safer and more efficient alternative to C/C++. It was officially released as an open-source project back in 2010. The lead designer of the language was **Graydon Hoare**, who has since moved to **Apple** and is now working as part of the **Swift** (discussed later) language team.

The stated goals of the Rust programming language are:

* Safety
* Speed
* Concurrency

Out of the three, Rust takes **safety** more seriously. It enforces safety through one of its most unique (and tricky) concepts: **ownership**. [Here](https://chrismorgan.info/blog/rust-ownership-the-hard-way.html) is an excellent article explaining the concept.

A simple function declaration in Rust looks like this:

```rust
fn factorial(x: i32) -> i32 {

    // The body of the function goes here....

}
```

By now, I am sure that you can tell what is going on in the code snippet above.

A function in Rust is declared using the keyword `fn`. This is followed by the name of the function and a list of arguments within a pair of parentheses. For the case above, we only have one argument, **x**, which is a 32-bit signed `integer` number. Note that the function argument must be annotated with a type, and if there are more than one argument, each argument should be separated by a comma.

The function returns another 32-bit signed integer number, as indicated by the right arrow operator (**->**) and `i32` after it.

Variables in Rust are _immutable_ by default, and they are declared using the keyword `let`, as shown below:

```rust
let x = 7;
```

The code above declares a variable and assigns it a value of *7*. The code snippet is officially called a ***variable binding*** in Rust. In this case, **7** is '*bound*' to the variable ***x***.
The Rust compiler will infer the type of ***x*** as `i32`.

The idea of variable bindings allows for pattern matching as shown below:

```rust
let (x, y) = (7, 13);
```

In this case, 7 will be bound to ***x*** and 13 to ***y***.

The type of the variable may be explicitly declared as shown below:

```rust
let x: i32 = 7;
```

If a variable will need to change after its initialization, then we should add the keyword `mut` (mutable) like so:

```rust
let mut x = 7;
```

Note: Unlike most of the other languages surveyed in this article, Rust requires a semicolon as a _statement terminator_ (although there are a few exceptions to this rule).

***Concurrency in Rust***:

Being a systems language, Rust offers native thread-level concurrency primitives. It promises '*concurrency without data races*'.

Threads in Rust are created by using the following syntax:

```rust
let thread = std::thread::spawn(|| {
       println!("Graydon");
});
```

In order to create a new thread, we use the `spawn()` function from the standard thread library: **std::thread**.

The `spawn()` function takes as its argument a ***closure***, which in this case prints out a `string`.

Note the bang (**!**) at the end of **println**. This indicates that it is a `macro` invocation, and not a normal function call.

Just like in Go, threads in Rust communicate through **channels**. This is illustrated by the following example:

```rust
use std::thread;
use std::sync::mpsc;

fn main() {
   let (tx, rx) = mpsc::channel();

   for _ in 0..10 {
        let tx = tx.clone();

        thread::spawn(move || {
             let result = 77u32;

             tx.send(result);
        });
   }

   rx.recv().ok().expect("could not receive the result.");
}
```

First we begin by importing the standard thread and synchronization (`sync`) libraries.

A channel is created by calling the `channel()` method from the **std::sync::mpsc** package, as shown below:

```rust
let (tx, rx) = mpsc::channel();
```

In this case the channel is bound to a tuple of **arity** 2 (Arity refers to the number of arguments an operation takes).

The initials in **mpsc** stand for "*multiple producer, single consumer*." This means that the '*sending*' end of a channel (in this case, ***tx***), can be ***cloned*** (copies of it can be made), and used by as many threads as you would like to enqueue values; but the '*receiving*' end of the channel (in this case, ***rx***) cannot be cloned, so only a single thread is allowed to extract values from the queue.

After the thread is created, it calculates the *result* and **sends** it over the channel through ***tx***, as seen below:

```rust
tx.send(result);
```

Notice that in the example, we are using a `move` closure when creating the thread. This enforces **ownership** by giving the closure its own **stack frame**.

If ***rx*** receives a *result*, the `expect` method will return the value of the result, otherwise it returns an error, `Err(e)`, with the string message: '*could not receive the result*'.

Rust's standard synchronization package (**std::sync**) offers two more types that are useful for ensuring safety in a multithreaded environment: `Mutex<T>` and `Arc<T>`. In order to use them, they need to be imported as shown below:

```rust
use std::sync::Mutex;
use std::sync::Arc;
```

The above import can also be done using just a single line, like so:

```rust
use std::sync::{Mutex, Arc};
```

The ***Mutex*** type offers **locks** through its `lock` method. This ensures that two threads cannot claim ownership of the same piece of data at the same time.

The initials in **Arc** stand for '*Atomic reference count*'. The ***Arc*** type ensures that sharing immutable data is **threadsafe**. It tries to avoid **data races** between multiple threads.

Finally, Rust offers a robust package manager called **Cargo**. This helps in managing Rust's external code libraries (those that are not part of its standard library), known as '***crates***' by the Rust community.

The following code snippet shows how you can use a library crate called `rand`, that offers random number functionality:

```rust
extern crate rand;
```

The `extern` keyword indicates that this is an *external* library (***crate***).

Another area that Rust excels in is its support for **FFI** (Foreign Function Interface). This means that it can easily be embedded inside of other languages.

In terms of its learning curve, Rust would lie just below **Scala**.

I should mention that according to the [Stack Overflow Developer Survey](https://insights.stackoverflow.com/survey/2017), Rust has been the most loved programming language for two years in a row (2016 and 2017).

Rust also happens to be my favourite among the five languages discussed in this post.

The following are two great projects that have been done in Rust:

* [Redox](https://www.redox-os.org/): This is a micro-kernel based OS written entirely in Rust.
* [Servo](https://servo.org/): A browser engine developed by Mozilla Research in conjunction with Samsung.

If you are interested in learning Rust through interactive examples, you can find such a resource [here](https://rustbyexample.com/).

### 4. Kotlin

Until very recently, [Kotlin](https://kotlinlang.org/) was barely known within the larger programming community. The language, named after **Kotlin Island** near St. Petersburg in Russia, was created by [JetBrains](https://www.jetbrains.com/), the company behind popular products like **IntelliJ IDEA**, **PhpStorm** and **PyCharm**. It was officially released in 2011, and it runs on top of the **Java Virtual Machine** (similar to **Scala**).

Kotlin got a really big boost when at the 2017 edition of **Google I/O**, it was [announced](https://techcrunch.com/2017/05/17/google-makes-kotlin-a-first-class-language-for-writing-android-apps/) that the language had been given first-class support on the Android mobile platform (becoming only the third language to be accorded such status, after **Java** and **C++**). The news was received with a lot of excitement by the Android developer community. As a result, interest in the language has skyrocketed over the last couple of months following the announcement. Additionally, this might be considered a strategic move by Google as it has been embroiled in a [legal tussle](http://www.reuters.com/article/us-oracle-google-lawsuit-idUSKCN0Q42MG20150730) with Oracle over the use of certain Java Classes for Android development.

One of the stated goals of Kotlin is to be fully interoperable with Java and run just as fast as Java native code.

A simple function declaration in Kotlin looks like this:

```kotlin
fun factorial(x: Int): Int {

       // The body of the function goes here...

}
```

This code block looks very similar to the Scala code shown previously, and therefore it doesn't require much explanation. The only thing to note here is that in Kotlin, a function is declared using the keyword `fun`.
In this case, the function takes one argument of type `Int` and returns an `Int` as well.

Just like in Scala, variables in Kotlin are declared using either `val` or `var` keywords, as shown below:

```kotlin
val x: Int = 3
var y = 7
```

In this case, ***x*** is an immutable variable of type `Int`, while ***y*** is mutable. The type of ***y*** is inferred as `Int`.

***Concurrency in Kotlin***:

Compared to the three languages discussed above, I consider the implementation of concurrency in Kotlin to be the least robust.

Kotlin uses ***coroutines*** to implement concurrency (although the last time I checked, it was still in the experimental phase). It applies the concept of ***suspension***, which means that computations can be suspended without blocking a running thread.

The list below shows some good Kotlin resources for further reading:

* [Kotlin in Action](https://www.amazon.com/Kotlin-Action-Dmitry-Jemerov/dp/1617293296), a book by two core Kotlin developers at JetBrains.
* [Kotlin for Android Developers](https://leanpub.com/kotlin-for-android-developers) by Antonio Leiva.
* Kotlin's own documentation which can be downloaded as a [PDF](https://kotlinlang.org/docs/kotlin-docs.pdf) file.

[Here](https://medium.com/@magnus.chatt/why-you-should-totally-switch-to-kotlin-c7bbde9e10d5) is an excellent article on why you should switch to Kotlin, according to one developer who is thrilled by the language.

For strategies on how to apply Kotlin to existing Java code, [here](https://medium.com/@enriquelopezmanas/on-strategies-to-apply-kotlin-to-existing-java-code-6317974717ec?imm_mid=0f4065&cmp=em-prog-na-na-newsltr_20170701) is a post that might be of great help.

### 5. Swift

Created at Apple, [Swift](https://swift.org/) is meant to be a safer and more concise alternative to ***Objective-C*** for App development within the Apple ecosystem. The language was introduced at Apple's ***Worldwide Developers Conference*** in 2014, and released as an open-source project a year later. Its lead designer was Chris Lattner (who also happens to be one of the original authors of the [LLVM](http://llvm.org/) project).

Most of Swift code looks a bit similar to code written in Rust.

A simple function declaration in Swift is written as shown below:

```swift
func factorial(x: Int) -> Int {

        // The body of the function goes here...

}
```

Again, this should look very familiar by now. A function in Swift is declared using the keyword `func` (similar to **Go**). In this particular instance, the function has one argument of type `Int` and has an `Int` as its return type.

Swift makes a clear distinction between immutable and mutable variables. Immutable variables are called ***constants*** while their mutable counterparts are just called ***variables*** (as expected).

Constants are declared using the `let` keyword while variables are declared using the `var` keyword. A simple illustration is shown below:

```swift
let str: String = "Welcome to Swift Programming"
var x = 10
```

The first line declares a constant ***str*** of type `String`, while the second line declares a variable ***x*** whose type is inferred as `Int`.

***Concurrency in Swift***:

Unfortunately, Swift has been very slow in adopting language-level concurrency features (although there seems to be some progress beginning from Swift 3).

Perhaps the most iconic feature of Swift is the availability of ***Swift Playgrounds***, which offers some kind of a sandbox within which Swift code can be executed '*on-the-fly*'. Even more interesting is the fact that the Playgrounds work on iPads as well!

The only great resource that I have found for learning Swift is the free manual offered by Apple, which can be downloaded as an [ePub](https://swift.org/documentation/TheSwiftProgrammingLanguage(Swift4).epub) file. This document is often known as '**The Swift Programming Language**'.

### Conclusion

This relatively long post has attempted to present a broad overview of some of the most common modern programming languages. My main aim has been to give a comprehensive survey of the modern programming landscape, and the languages that are shaping it, for better or worse.

It should be noted clearly that I am not a language design expert. What I have discussed here should be taken as my own assessment of the respective languages and how they try to solve the problems that were articulated at the beginning of this essay.

My hope is that after reading this article, you will be motivated to explore the languages more and find out how they might help with your own projects.
