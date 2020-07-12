---
layout: post
title: Python For Programmers Part 1
---

# What?

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

# Intro: Printing things.

The introduction into any computer language starts with printing out `Hello World!`, no matter how uncomplicated or complicated it is. In Python, we need only one line:

```Python
print("Hello World!")
```

# Variables.

That's it. There are no libraries to import, no extra function calls, no semicolon, none of that. Let's do the same thing with a variable so you know how those work in Python:

```Python
name = "Chandler"

print(f"Hello, {name}!")
```

For variables in Python, you just specify the name and then an equal sign, along with the data being stored. Unlike statically typed languages, this can be any value that Python knows how to handle, and the type is not required as that is determined at runtime. The basic types in Python (i.e. The ones that you don't need objects for) are strings, integers, floats, lists (arrays), dictionaries (literally just JSON), complex numbers, and booleans, and null which is called `None` in Python. Here is an example of each in use:

```Python
# string
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

# list
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

Converting between strings, integers, and floats is easy. Here are a few options:

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

# you can also convert z into an int
# this will round to the nearest whole
# number

z_int = round(float(z))

```

