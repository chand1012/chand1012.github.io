---
layout: post
title: Python For Programmers Part 3
---

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

- [Part 1](https://chand1012.dev/PythonForProgrammers/)
- [Part 2](https://chand1012.dev/PythonForProgrammers2/)

Also I know Python calls them methods but I use "methods" and "function" interchangeably.

# Functions/Methods

# Classes

There is something I omitted from the last article that you experienced object-oriented programmers and hackers would probably be in anguish without: classes. Classes are very engrained in Python's nature. Creating a class can be done like so. 

```Python

class Dog:
    # this is a class variable and will be shared
    # with all instances of class 'Dog'
    genus = 'canine'

    # this function gets called
    # When you initialize a new instance
    # of the class 'Dog'
    # these variables are on a per instance
    # basis and will not be shared.
    def __init__(self, name, age):
        self.age = age
        self.name = name
        self.parts = {}
        self.furry = True
        self.hypoallergenic = False
    
    # this will be called when the 
    # class needs to be represented
    # such as when you try to print
    # an instance of it
    def __repr__(self):
        return f"({self.name}, {self.age})"

    # This is a simple example
    # of a class method
    # this can also use input variables
    # same as the init function
    def bark(self):
        print("Bark Bark")

    def set_parts(self, tails=1, ears=2, paws=4):
        self.parts = {
            "tails": tails,
            "ears": ears,
            "paws": paws
        }
        return self.parts # and you can use functions to return class variables as well

    def sheds(self):
        if self.furry and not self.hypoallergenic:
            return True
        return False

# Here is how to use the class
murphy = Dog("Murphy", 7)
murphy.bark()
murphy.set_parts() # ticks all of the boxes for the default
if murphy.sheds():
    print("You should buy a roomba.")

champion = Dog("Champion", 2)
champion.set_parts(1, 2, 3) # The dog from Parks and Rec only has 3 legs

print(murphy)
print(champion)

```

Instance variables can be defined in functions with `self.<var name>`, and I prefer to define all the ones that I will be using in the `__init__` method for my own organization. Class variables are defined outside of methods and are shared between all functions, even when changed. For example, If I were to change Murphy's genus to 'cat' or something absurd, Champions' genus would change as well. Also, classes can inherit other classes. Here is an example, assuming that the previous class-related code was imported previously.

```Python
from dogs import Dog

class Labrador(Dog):
    def is_purebred(self):
        return True

# the class labrador would have the same 
# methods, class vars and instance vars as
# the previous dog class.
marley = Labrador("Marley", 14)
marley.bark()
marley.set_parts()
if marley.sheds():
    print("Labs shed a lot.")
```