---
layout: post
title: Python For Programmers Part 2; Controls
---

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

- [Part 1](https://chand1012.dev/PythonForProgrammers/)

# Indentation

Before we start talking about controlling your scripts and programs, we should talk about how Python can tell what's in a statement/loop or anything else. Rather than using brackets like most other languages, Python opts to scan for indentations. This can be one of two ways, but not both at the same time. The first and most common/most "Pythonic" method is to use four spaces as one indentation level. Most text editors that support Python do this already, and if not, consult your text editors settings to change this. The other way is with tab characters, but with modern text editors, this is being phased out. If you mix these two together, the Python script will not execute at all. The start of an indented code block is preceded on the line that requires it with a colon, as you will soon see.

# Loops

Loops, as we all know, are fundamental parts of programming. Now C/C++ has a function called `do...while`. I stray away from this function, but only because I have been conditioned to do so by Python. Python has two main loop types: `while` and `for` loops. Because of my experience with Python, `do...while` loops always felt unnatural to me. Here are the two loops broken down.

## For Loops

These are for iterating. Examples of iterables in Python are lists, strings, ranges, and dictionaries. There are more, but these are the examples I will be using. The example I see most is looping through a list of items, which I always call and array because of JavaScript and C. This is as simple as this:

```Python

dogs = ['Murphy', 'Champion', 'Marley']

for dog in dogs:
    print(dog)

```

That's it. You have just iterated over all the dogs. This will print each dog line-by-line. Yes, it is that simple. If you want to do this the old-fashioned way, you are more than welcome to, its just not considered the "Pythonic" way:

```Python

dogs = ['Murphy', 'Champion', 'Marley']

# range takes an integer and makes a list from 0 to n-1
# an index of zero to makes indexing lists easier. 
# 'len' simply gets the length of a list or string.

for i in range(len(dogs)):
    print(dogs[i])

```

You can do the same with strings and dictionaries. Dictionaries do require an extra step though:

```Python

breeds = {
    "Murphy": "German Shepard",
    "Champion": "Mixed",
    "Marley": "Labrador Retriever"
}

for key in breeds:
    # this is the "safe" way of 
    # getting dictionary values.
    # if a key doesn't exists, 
    # returns "None"
    value = breeds.get(value) 
    print(value)

# dictionaries can also be called upon this way:
for key, value in breeds.items():
    print(key)
    print(value)

```

## While Loops

While loops are arguably simpler. They are the same as C: `while <conditional>`. The most common use of a `while` loop in Python you will see is the `while True` loop which is just the same as it is in other languages: an endless loop. Like C/C++, you just use the `break` keyword to escape it, which also works in `for` loops if needed. Example:

```Python

counter = 0

while True:
    # Python doesn't have an
    # increment operator
    counter += 1
    print(counter)
    if counter >= 10:
        break

```

Quick side note: Python has a few shortcut operators, like from C: `+=` adds and sets, `-=` subtracts and sets, `*=` multiplies and sets, and `/=` divides and sets.

Now this same loop can be achieved with less code, like so:

```Python

counter = 0

while counter < 10:
    counter += 1
    print(counter)

```

But `break` has its uses. `Break` can also be used to escape `for` loops before they are finished iterating.

## Continue

The `continue` statement also has its roots in C. It does the same thing as it does in most other languages: skips the rest of the loop and starts the next iteration. Here is an example of that:

```Python

breeds = {
    "Murphy": "German Shepard",
    "Champion": "Mixed",
    "Marley": "Labrador Retriever"
}

print("Here is a list of purebred dogs: ")

for key in breeds:
    breed = breeds.get(key)
    if breed is 'Mixed':
        continue
    print(breed)

```

# Logic

## Conditionals

Python uses a mix of conditionals from C and a few of its own. For example, the statements `==`, `!=`, `<=`, and `>=` are all perfectly valid in Python. These all compare the values of the items in question. Python also adds `is`, as previously shown, and `in`. The `in` keyword can either be used with iterators or as a conditional. You have already seen how it can be used with loops, but here is an example of it used as a conditional:

```Python

# the colors of the dogs!
colors = ['black', 'brown', 'white']

if 'brown' in colors:
    print("I found brown!")

color = colors[1]

if 'own' in color:
    print('There is "own" in brown!')

```

The `in` statement works with strings, lists, dictionaries, and a few others. The conditional statement `is` compares identity rather than value. When I say `identity`, I mean type and value, so for example:

```Python

# these are mathematically the same
x = 7
y = 7.0
z = complex(7, 0)

print(x == y)
print(x is y)
print(y == z)
print(y is z)
print(x == z)
print(x is z)

```

The output would be `True`, `False, `True`, `False, `True`, `False`. While from a value standpoint, `7.0` is the same as `7`, they are not the same thing. Same with the complex form of `7+0j`. What I have used the `is` statement for the most is checking whether or not a value is `None`, as it is both the "Pythonic" way to do it, and because it looks nicer. Python is supposed to be both a functional and readable language, so you should use it as such.

The next thing we should talk about is the logical operators: `not`, `and`, and `or`. Rather than using the symbols, Python shot for the most human readable way possible and just literally uses the English words for the code. The `not` operator is the same as `!` in every other language, and should be used as such. The `and` and `or` statements should be self explanatory. Here is an example of all three.

```Python

team = "team"

print(not "i" in team)
print("e" in team and "a" in team)
print("t" in team or "j" in team)

```

These will all print out `True`.

## If Statements

In the last example of the Loops section, you see a simple use of an `if` statement in Python. In Python, the three `if` statement keywords are `if`, `elif`, and `else`, with `elif` being the equivalent to most other languages' `else if`. Here is an example of an `if..elif..else` statement.

```Python
# I will discuss imports in part 3
# for now just know that I needed this
# for my example. This is math functions.
import math 

x = 128

if math.log2(x).is_integer(): # is_integer method checks if a float is also an integer (in a mathematical sense)
    print("x is power of two.")
elif x % 2 is 0:
    print("x is even.")
else:
    print("x is odd.")
```

The `elif` statement can be used as many times as you want in an `if..elif..else` block. If you were to say have three `if` statement in a row with an `if..elif..else` block at the end, it would have significantly different behavior than if I were to use an `if..elif..elif..elif..elif..else` block. The three `if` statements would be assumed to all be apart of separate logic, just as if you were to do the same in C. There is no `switch` statements in Python, so if you have a situation where you would use them, you may just be using an `if` statement with a lot of `elif`s mixed in.

# Methods (Functions)

Methods, or as they are known in other programming languages, Functions, are a quintessential part of the programming paradigm. Functions in Python are not similar to C, but more similar to JavaScript, in the sense that type of the input and output variables is not set. Python functions can be created before or after the code that calls them, and they can be used recursively. Here are a few examples of them.

```Python

# simple square function
def square(x):
    return x*x

# simple recursive function
def recursive_power(x, y):
    if y>1:
        return x*recursive_power(x, y-1)
    else:
        return x

# function with no return
def print_10_times(a_string):
    for _ in range(10):
        print(a_string)

```

There is one thing to be discussed that I did in the `for` loop, which is the use of the underscore as the variable to be used when iterated. It is common practice in Python, and most other languages, to use this variable as a sort of "I don't care" variable for values that can be ignored. In some languages it is a reserved keyword and can only be used for that purpose, such as in Golang, but in Python, its actually just a variable, and could be called upon if used as a standard variable. 

The other thing about underscores in Python is that prefixing a function with them implies that the function should only be used internally, wether that is for classes (we will get to those in the next part of this series), or for files that just contain functions. This does not stop them from being used, but most programmers should stray away from using functions that other programmers may not want you to use. 

That concludes part 2 of "Python for Programmers". In the next article, I will be going over classes and imports.