---
layout: post
title: Python For Programmers Part 1
---

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

# Intro: Printing things.

The introduction into any computer language starts with printing out `Hello World!`, no matter how uncomplicated or complicated it is. In Python, we need only one line:

```Python
print("Hello World!")
```

To execute this Python script, let's call it `hello.py`, just type `python hello.py` while in the same directory in your terminal as the script.

# Variables.

That's it. There are no libraries to import, no extra function calls, no semicolon, none of that. Let's do the same thing with a variable so you know how those work in Python:

```Python
name = "Chandler"

print(f"Hello, {name}!")
```

For variables in Python, you just specify the name and then an equal sign, along with the data being stored. Unlike statically typed languages, this can be any value that Python knows how to handle, and the type is not required as that is determined at runtime. The basic types in Python (i.e. The ones that you don't need objects for) are strings, integers, floats, lists (arrays), dictionaries (literally just JSON), complex numbers, and booleans, and null which is called `None` in Python. Here is an example of each in use:

```Python
# string
# edit: single and double quotes are
# both acceptable
name = "Murphy"

# all numbers are signed.

# integer
age = 7 # dog years of course

# float
weight = 100.25 # lbs

# dictionary
body_parts = {
    "tail": 1,
    "paws": 4,
    "eyes": 2,
    "ears": 2
}

# list can hold any data type, this one is just strings though
# a list with mixed types is acceptable.
friends = ['Daisy', 'Olivia', 'Dan']

# calling a variable from the list
best_friend = friends[0]

# booleans are always capitalized
furry = True

# complex number
complex_state = 2-5j
# can also be annotated like so
complex_state = complex(2, -5)

```

# Converting Between Types

Converting between strings, integers, and floats is easy. Basic arithmetic is also the same as most other languages. Here are a few options:

```Python

x = 12
y = 5.5
z = '14.4'

# the old way
str_of_x = 'x is ' + str(x)
str_of_y = 'y is ' + str(y)

# the new way
format_str_x = f'x is {x}'
format_str_y = f'y is {y}'

# this would not work
test = 'x is ' + x

# arithmetic between integers and floats can be done
# without conversion, but the output is always a float

total = x + y + float(z)
average = total / 3
product = x * y * float(z)
difference = x - y

# you can also convert z into an int
# this will round to the nearest whole
# number

y_int = round(y)
z_int = round(float(z))

# this will cause a runtime error
divide_by_zero = x / 0

```

You can also get all of the keys or all of the values of a dictionary as a list, like so:

```Python

# dictionary
body_parts = {
    "tail": 1,
    "paws": 4,
    "eyes": 2,
    "ears": 2
}

keys = list(body_parts.keys())
values = list(body_parts.values())


```

There is two other data types that is included in Python's standard library that we should mention: tuples and ranges. Tuples are essentially just immutable lists, as in they are a list that cannot be changed after being defined. Everything else about them is the same, they index at one, individual variables are access using the same method as lists, and they can accept any type. They can be converted to lists by passing them into the `list(x)` function. Ranges are a way to generate lists of numbers. By default, a range is its own data type, but they can be converted to lists same as tuples. They have three initialization variables, but this also depends on how you generate the range. Here are a few examples from Python's [official documentation](https://docs.python.org/3/library/stdtypes.html?highlight=range#range):

```Python
>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> list(range(0, 30, 5))
[0, 5, 10, 15, 20, 25]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> list(range(0))
[]
>>> list(range(1, 0))
[]
```

The ending number is always omitted. This can be used to form loops, which we will go into more detail in the next article. Here is a small example:

```Python
for i in range(10):
    print(f'This is the {i+1} loop')

```

# Closing Statements

That was a fast rundown of the basic data types of Python. For the next article, we will go over how to use loops, logic, and error handing. Part 3 will start covering some included libraries to try out and use in your own projects. Hope you found this helpful!
