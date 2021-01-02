---
layout: post
title: 
---

To master the basics of how to write a computer program, you first must master the basic terminology that is the same in all programming languages and how they work. The following concepts are all important and are in no particular order.

## Variables

Variables are how you can store information within a program, whether that information be text data (called "strings" in programming), numbers, objects, or list of other data. Variables can be defined a number of ways, and how they are defined is heavily dependent on what language you are using. One of the larger differences between many languages is the difference between statically typed and dynamically typed languages.

### Static Types

If a languages has a static typing system, that means that when you define a variable you cannot change its type while within the same scope. The "scope" of the variable is the block of code you are using the variable in. If a variable has a global scope, that means that the variable can be read from and written to anywhere in the program. Scope will be explained further and will be easier to understand after the concept of code blocks are introduced. For now, the main thing to take away from this section is that you cannot change the type of a static variable once its set. Most of the time, when you set a statically typed variable, you also define what type the variable is. Here is an example in C++:

```C++
int x = 7;
float cash = 12.77;
std::string name = "John Doe";
```

The first word is the data type. Numbers generally come in three types: integers, floating point numbers, and complex numbers. Integers can only hold whole values, floating point numbers are effectively decimal numbers, but with varying levels of accuracy. The exact amount of accuracy can be affected by the language you are using, the compiler you are using, the architecture of your computer, and a few others. For the most part, numbers with type of `float` are not as accurate but use less memory. Numbers with the type `double` use twice as much memory as the `float` type, but have significantly more accuracy. Variations of these two types exist, but these are the two you will use the most. Complex numbers aren't really used by the average programmer.

### Dynamic Types

Dynamically typed languages are those where the type of the variable is independent of scope or definition, and you can assign any type to any variable at any given time. This sound like a no-brainer, and it sounds like this would make 