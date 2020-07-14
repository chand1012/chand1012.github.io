---
layout: post
title: Python For Programmers Part 2
---

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

- [Part 1](https://chand1012.dev/PythonForProgrammers/)

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

# range takes the length of an array and starts
# at an index of zero to make these things
# easier. 'len' simply gets the length of a list or string.

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

Now this same loop can be achieved with less code, like so:

```Python

counter = 0

while counter < 10:
    counter += 1
    print(counter)

```

But `break` has its uses.

## Continue

