---
layout: post
title: The Beast Roars Back The Resurgence of C++
---
### Introduction
Since its inception in 1979 as "**C with Classes**", ***C++*** has grown to become a powerful tool for millions of software developers all over the world. According to a 2015 [survey](https://blog.jetbrains.com/clion/2015/07/infographics-cpp-facts-before-clion/) done by **JetBrains**, there were around **4.4 million** C++ developers all across the globe. Over its long and often tumultuous history, the language has seen its fair share of ups and downs. It has constantly tried to fend off numerous accusations leveled against it, such as the fact that it's an extremely **complex** and **intimidating** language to learn. In recent history, other languages that are perceived to be less complicated and easier to learn have overtaken C++ in popularity within the programming circles. One such language has been **Java**. Right from its announcement in 1995, Java took the programming community by storm, owing to the fact that it was much friendlier to novices. In addition, programs written in Java were meant to run atop the **JVM**, hence making Java programs highly portable, unlike C++ programs.

Despite its gloomy past, all has not been lost for C++. The language is still the most preferred for writing applications where **performance** is the most crucial consideration. Examples of such applications include:

>* Internet search engines (e.g. Google)
* Web browser engines (e.g. Mozilla Firefox)
* Systems software (e.g. Operating Systems and Device Drivers)
* Game development
* Telecommunications systems software.

For a comprehensive list of applications written using C++, you can check this [link](http://www.stroustrup.com/applications.html) maintained by **Bjarne Stroustrup** (The creator of the language).

### The Beast Roars Back
As explained in the tiny book [C++ Today](http://www.oreilly.com/programming/free/c++-today.csp) published by O'Reilly, the proliferation of **mobile devices** and the rise of **cloud-computing** has brought back the issue of performance to the center stage. According to the authors of the book, ***performance per watt*** is now one of the most important considerations for mobile devices. The same concern goes for cloud-based computers that run in  cloud data centers. These factors have led to a resurgent interest in C++ by the wider developer community.

In order to better serve the new legion of software engineers interested in the language , C++ has had to undergo tremendous changes. The changes have been aimed at making the language more approachable and less complicated. Also, new features (for example, **multithreading** capabilities and **lambda expressions**) have been incorporated into the language to make it more modern.

Such efforts, geared towards making C++ popular again, have directly led to what is now commonly known as '**Modern C++**'. Beginning with ***C++11***, a bunch of new nifty features have found their way into the language thus resulting in a fresh look that most developers have been yearning for. ***C++14*** was considered a small maintenance release while ***C++17*** has added a few more features to the language.

What follows is a brief discussion on some of the new features of C++ as from ***C++11***.

#### Type Deduction Using the `auto` Keyword

Type inference (or deduction) is a default feature in most modern programming languages. For example, a variable declaration in **Go** looks like this:

	x := 10 // x inferred as an integer
	
 As seen from the code snippet, you don't need to declare the type of the variable '***x***' explicitly. The Go compiler will do that for you instead, based on the initial value given.
 
 In 'old' C++ (***pre-C++11***), the type of a variable had to be declared explicitly, like so:
 
	int x;      // x declared as an integer
	int y = 10; // y declared and initialized at the same time
 
 Beginning from **C++11**, it is now possible to take advantage of type inference using the `auto` keyword, as shown below:
 
	auto x = 1; // x deduced as an integer
 
 Note that for type deduction to work, the variable should be **initialized**.
 
#### Range-based `for` Loop
 
 This feature makes writing `for` loops more concise, and fun too. 
 
 In Python, a range-based `for` loop looks like this:
 
	for i in range(10):
	   print(i) 
 
 The output for the code above is:
 
	0
	1
	2
	3
	4
	5
	6
	7
	8
	9
 
 In order to achieve a similar result using old C++ code, we would have to write something like this:
 
	// declare and initialize an array of integers
	int a[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
 	
	for ( int i = 0; i < 10; ++i ) {
	     cout << a[i] << endl;
	}
 	
 Using the range-based `for` loop, the above code can be re-written as shown below:
 
	int a[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
 	
	for ( int i : a ) {
	     cout << i << endl;
	}
 
 Much better and cleaner, don't you think?
 
 For a better illustration of the advantages of the `auto` keyword and the range-based `for` loop, here is another example:
 
 This first version is written in old C++ style.
 
	#include <iostream>
	#include <vector>
 	
	using namespace std;
 	
	int main( int argc, char ** argv ) {
		// Declare and initialize a vector of integers
		vector<int> vi = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
		 	       
		// Declare an iterator and loop through all the vector elements
		for ( vector<int>::iterator it = vi.begin(); it < vi.end(); ++it) {
		   cout << *it << endl;
		}
		return 0;
	}
 	
 Below is the second version using the `auto` keyword:
 
	#include <iostream>
	#include <vector>
 	
	using namespace std;
 	
	int main( int argc, char ** argv ) {
		// Declare and initialize a vector of integers
		vector<int> vi = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
		 	       
		// Type deduction using auto
		for ( auto it = vi.begin(); it < vi.end(); ++it) {
		   cout << *it << endl;
		}
		return 0;
	}
 	
 Note that now we no longer have to declare the type of `it` as a vector iterator.
 
 The third version is even much shorter and concise. Here, we employ the range-based `for` loop:
 
	#include <iostream>
	#include <vector>
 	
	using namespace std;
 	
	int main( int argc, char ** argv ) {
		// Declare and initialize a vector of integers
		vector<int> vi = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
		 	       
		// Range-based for loop
		for ( auto & i : vi ) {
		   cout << i << endl;
		}
		return 0;
	}
 	
 The output for the code above will be:
 
	1
	2
	3
	4
	5
	6
	7
	8
	9
	10
 
#### `Auto` in Templates

This is a feature in C++17 that now allows the declaration of non-type template arguments with the keyword `auto`. This makes it possible for template parameters to be deduced at the point of instantiation rather than explicitly specified, as shown below:

	template <auto V> void somefunc() { } // V is some constant (const) value
	
	somefunc<20>();    // 'int' is deduced
	
Previously, you would have to write something like this:

	template <typename T, V> void somefunc() { }
	
	somefunc<int, 20>();
	
### if constexpr (expression)

This is another new feature in C++17 that may be considered as the C++ version of **compile-time** `if`.

It allows blocks of code to be compiled only if the `constexpr` evaluates to `true` during compilation time:

	if constexpr(is_integer ...) { // execute integerstuff }
	else if constexpr(is_floating_point ...) { // execute floatingpointstuff }
	else { // NaN; }
	
For more information about the many other new features of C++, check out this [link](https://isocpp.org/files/papers/p0636r0.html) that is maintained by the Standard C++ Foundation (isocpp.org).

Two of the most popular projects done using modern C++ are:

* [Seastar](www.seastar-project.org): An open-source C++ framework for high-performance server applications that forms the basis of [ScyllaDB](http://www.scylladb.com/), a NoSQL database that is touted as the next generation [Cassandra](http://cassandra.apache.org/).
* [OSv](osv.io): A cloud-based operating system written entirely in C++.

### Conclusion

Just as it is often said that the Lion never sleeps, C++ has never been asleep. And just when we thought that the beast was nodding off, he has roared back, much to everyone's delight (and astonishment!)

#### References:
* [C++ Today](http://www.oreilly.com/programming/free/c++-today.csp): A tiny free ebook (74 pages) by Jon Kalb and Gasper Azman.
* [Effective Modern C++](http://shop.oreilly.com/product/0636920033707.do): A book by Scott Meyers.

N/B: This article was published on Medium and can be read [here](https://medium.com/@chiedo.chrispine/the-beast-roars-back-the-resurgence-of-c-928edcd64318), and on LinkedIn and here is the [link](https://www.linkedin.com/pulse/beast-roars-back-resurgence-c-chrispine-chiedo).