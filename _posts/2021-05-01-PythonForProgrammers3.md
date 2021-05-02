---
layout: post
title: Python For Programmers Part 3
---

This is a series on Python and how to correctly use Python when coming from a background in another computer language. Because of this, this will not be a slow intro into programming and it will be assumed you have a preferred text editor and are smart enough to get Python running. You can download installers and packages from their official website found [here](https://www.python.org/downloads/). Basic knowledge of how to use [Git](https://git-scm.com/) and how to operate a computer is also preferred.

This series will assume you are familiar with some form of a computer language and basic programming paradigms. C/C++, JavaScript, and Go will be referred to most as they are my most familiar languages other than Python, but anyone with experience in really anything else should be able to follow along.

- [Part 1](https://chand1012.dev/PythonForProgrammers/)
- [Part 2](https://chand1012.dev/PythonForProgrammers2/)

Also I know Python calls them methods but I use "methods" and "function" interchangeably.

# Classes

There is something I omitted from the last article that you experienced object-oriented programmers and hackers would probably be in anguish without: classes. Classes are very engrained in Python's nature. Creating a class can be done like so. 

```Python
# This file is named "dogs.py"

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

    def set_parts(self, tails=1, ears=2, paws=4):
        self.parts = { # this sets the instance variable
            "tails": tails,
            "ears": ears,
            "paws": paws
        }
        return self.parts # You can also return the value like a standard function

    def sheds(self):
        if self.furry and not self.hypoallergenic: # using the instance variables within logic
            return True
        return False

    @static_method # this is a method that can be 
    def bark():  # called without instantiating a class
        print('Bark Bark!')

if __name__=="__main__": # Only run if the file is being explicitly ran as a script.
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

    # you can also call static methods like this
    Dog.bark()

```

Instance variables can be defined in functions with `self.<var name>`, and I prefer to define all the ones that I will be using in the `__init__` method for my own organization. Class variables are defined outside of methods and are shared between all functions, even when changed. For example, If I were to change Murphy's genus to 'cat' or something absurd, Champions' genus would change as well. Also, classes can inherit other classes. Here is an example, assuming that the previous class-related code was imported previously.

```Python

from dogs import Dog 

class Labrador(Dog):
    def is_purebred(self):
        return True

if __name__=='__main__'
# the class labrador would have the same 
# methods, class vars and instance vars as
# the previous dog class.
marley = Labrador("Marley", 14)
marley.bark()
marley.set_parts()
if marley.sheds():
    print("Labs shed a lot.")

if marley.is_purebred():
    print("This dog is pure!")
```

In this example, I imported some of the previously written code. When you call the `import` statement, Python searches in a few places for your code. The places that it searches are called the `PYTHONPATH`, and your current directory is included. When you import another file in Python, the filename is omitted. Here is another import example:

```Python
import os

from dogs import Dog

name = os.getenv('DOG_NAME')
age = os.getenv('DOG_AGE')

dog = Dog(name, age)
```

In this import example, the standard library module `os` was used. This module gives access to operating system functions while remaining platform agnostic, so functions will work on any OS. 

The order for imports, while not enforced, usually goes like this:

```Python
import standard_library

import pypi_module

import local_module
```

To install a PyPI module, run the command `pip install <name>`. Common modules include [`requests`](https://pypi.org/project/requests/), a really simple HTTP REST request library, [Flask](https://flask.palletsprojects.com/en/1.1.x/), a popular and lightweight HTTP server framework, and  [Pillow](https://python-pillow.org/), an image manipulation library. Many, many more exist, and can be found on [PyPI](https://pypi.org/).

Usually, when working with many Python packages, virtual environments are used. These make a project-level installation of packages, similar to how `npm` installs packages locally. See [here](https://virtualenv.pypa.io/en/latest/installation.html) for install instructions. 

To use on Windows:
```
python -m venv env
.\env\Scripts\activate
```

To use on MacOS & Linux:
```
python3 -m venv env
source env/bin/activate
```

Then you can install packages as previously stated, but they will install to the virtual environment. To save all of the packages for later use on another system, run `pip freeze > requirements.txt`. To install packages from a fresh virtual environment:

```
python3 -m venv env
source env/bin/activate
pip install -r requirements.txt
```

All packages in `requirements.txt` will be installed to the virtual environment. This is equivalent Node's `package-lock.json` file and should be committed to your source control.

This concludes Python For Programmers part 3.