---
layout: post
title: Transisioning from Python to Golang and why Python programmers should consider it.
---

## Intro

Ever since I started programming, high-level languages, like Python, Ruby, and Lua, have always been my languages of choice for my personal projects. I also dabbled with a VERY slight amount of C and C++, most of which was due to my small collection of Arduino boards. If you have ever used either Python, Ruby, or Lua, you probably heard that these are called *interpreted* languages. This means that when you install them, you never have to run any sort of compiler. On Windows, all you have to do to run them is either double click or run `python myscript.py` in the directory of the script and it runs. Go is a *compiled* langage, meaning that it needs to be compiled to machine code before it can be run. This has the advantage of being **much** faster than an interpreted langage. Most compiled langages, especially ones like C and Java, are known to have a somewhat steep learning curve for beginners. Go is special in that reguard, as it tries to bridge the gap between the two types of langages, being compiled like C but with a more user-friendly route to coding, like Python. This post is not really a tutorial, but mostly a guide for people like me who are used to interpreted and dynamically typed languages  like Python in transition to a statically typed and compiled langage like Golang.

## Types

I attempted to avoid most statically-typed and/or compiled languages as my coding style was, while not sloppy, was also not anything close to standard. Take this code for example:

```Python
thing = None
a = 1

if a==1:
    thing = 3
else:
    thing = "Hello world!"

```

In most low-level or compiled languages, Golang included, this is horribly taboo. This is because Python is a dynamically typed language, as most interpreted languages are. This means that the varaibles in the program do not have to hold their type, whether that be a string, a number (float or integer), or a array/list. When you declare a varable in Go, while you can delcare a variable without delaring its type, the type must remain the same for the duration of the scope, like so:

```Go
package main
import "fmt"

func main(){
    things := []string{"one", "two", "three"}

    for i := 0; i<10; i++ {
        fmt.Println(i)
    }

    for _, i := range things {
        fmt.Println(i)
    }
}
```

This will execute, but that is because the variable `i` is used in two different scopes. The variable used in the loop was defined at the start of the scope, you would have to keep it as whatever that type was. For example:

```Go
package main
import "fmt"

var (
    i int
    things []string
)

func main {
    // This could have been definied like in the previous example
    // but it is better practice to predefine your variables.
    things = []string{"one", "two", "three"}
    for i = 0; i<4; i++{
        fmt.Println(i)
        fmt.Println(things[i])
    }
    // up to here will compile
    // this will not work
    for _, i := range things{ // This doesnt work because i is an integer,
        fmt.Println(i)        // not a string.
    }

}
```

## Unused code

Another thing that tripped me up a lot is that you cannot have unused variables in your Go code. If you have code like so:

```Go
package main
import "fmt"

func main() {
    var i int
    fmt.Println("Hello world!")
}
```

The Go compiler will not allow you to compile the code at all. It will just spit out an error claiming that there is an unused variable. This is to make the code more efficient, so that the computer's memory and CPU resources are not wasted defining a variable that has no use.

## Pointers

Python tends to hide memory management from its users. This is in an effort to make it more user friendly for begginers and to make it easier to develop with. Go, while much more high level than C/C++, does not try to hide the computer's memory from you, the developer. This has its ups and downs, but the biggest is that you can share variable values across variables without using more memory, making the program more efficient.

Pointers in Go operate exactly like they do in C. If you want to read more about pointers, [here](https://beginnersbook.com/2014/01/c-pointers/) is a good example in C that transfers well to Go. The easiest way to think of pointers is what they are, an address. They "point" to a value in your computer's memory. They are also a really good way to escape scope. They allow you to pass a global variable into the function as a parameter and be changed in the function. If a pointer is not used, the function outside the variable is unchanged. Here is an example from the linked article but translated into Golang:

```Go
package main
import "fmt"
import "strconv"
// thing is used instead of var because var is a reserved name
func salaryhike(thing *int, b int) {
    *thing = *thing + b
}

func main() {
    var salary int = 95000
    var bonus int = 1500
    fmt.Println("Employee salary: " + strconv.Itoa(salary))
    fmt.Println("Bonus: " + strconv.Itoa(bonus))
    salaryhike(&salary, bonus)
    fmt.Println("Final salary: " + strconv.Itoa(salary))
}
```

This allows the variable in the `main` scope to be changed by the `salaryhike` function.

## *Go*routines and Channels

One thing that Python does not include in its standard library is a system for multithreading or any sort of coroutines. Go, which does not include much in its standard library, includes a way to multithread your programs and to correctly funnel data between them. Those two things are called goroutines and channels respectively. Goroutines, called coroutines in literally every other language, are an easy way to asynchronously execute a function or a set of functions. All you have to do is put the keyword `go` before calling the function to start execution of the function while starting execution of the next command. For example:

```Go
package main
import "fmt"
import "time"

func helloWorld() {
    fmt.Println("Started secondary routine.")
    time.Sleep(time.Second * 3)
    fmt.Println("Slept for 3 seconds")
}

func main() {
    fmt.Println("Started main routine.")
    go helloWorld()
    time.Sleep(time.Second(time.Second * 4))
    fmt.Println("Finished both threads.")
}
```

This will quickly and effectively allow for coroutines in your programs. If you have to execute the same function a few times, all you do is call `go` a few times with the function in a loop like so:

```Go
package main
import "fmt"
import "time"
import "strconv"

func helloWorld(i int) {
    fmt.Println("Started routine #" + strconv.Itoa(i))
    time.Sleep(time.Second * (i+1))
    fmt.Println("Slept for 3 seconds")
}

func main() {
    fmt.Println("Started main routine.")
    for i:=0; i < 5; i++ {
        go helloWorld(i)
    }
    time.Sleep(time.Second(time.Second * 7))
    fmt.Println("Finished all threads.")
}
```

Now, it is **really** bad practice as a developer to have multiple threads write to the same variable at the same time. At best, the data gets a little corrupted but the program keeps chugging. At worst, one of the threads has a `segmentation fault`, or an error in memory reads and writes, which will crash the entire program. Go remedies this by providing channels to funnel data between the threads. When you request a variable from a channel, it gets the oldest value added and continues your program. The channel isn't ready to read or write, execution will pause until the channel is complete being written to or until the channel is ready to send your data to a variable. A nice little website called [Go By Example](https://gobyexample.com) has a better way of explaining basically everything on this page, and I highly reccomend you go and check out that site for more Go stuff, but for now I am going to borrow the part the site about using channels as a tool to better organize your goroutines. First I will show you his example from [here](https://gobyexample.com/channel-synchronization) and explain a little better after you look over it:

```Go
package main

import "fmt"
import "time"

func worker(done chan bool) {
    fmt.Println("working...")
    time.Sleep(time.Second)
    fmt.Println("done")

    done <- true
}

func main() {

    done := make(chan bool, 1)
    go worker(done)

    <-done
}
```

By default, channels will pause your program until they have data to read from, in this case that data is a boolean value. This is considered the "old" way. The "new" and "recommended" way (which I myself follow) uses the `sync` module. Instead of having a seperate channel for the status of the thread, you pass a pointer to a variable of the type `sync.WaitGroup` into the worker and defer the command `wg.Done()` like so:

```Go
package main

import (
    "fmt"
    "strconv"
    "sync"
    "time"
)

func worker(wg *sync.WaitGroup, t int) {
    defer wg.Done()
    fmt.Println(strconv.Itoa(t) + " :Working...")
    time.Sleep(time.Second * time.Duration(int64(t+1)))
    fmt.Println(strconv.Itoa(t) + " :Done.")
}

func main() {
    var wg sync.WaitGroup

    for i := 0; i < 5; i++ {
        wg.Add(1)
        go worker(&wg, i)
    }

    wg.Wait()
}
```

You can spawn as many goroutines as you would like regardless of what method you use. Depending on the computational intensity and requirements of the task at hand, it may be more efficient to use a few goroutines, or you could be like [my Discord bot](https://github.com/chand1012/Discord-Quick-Meme) and spawn a routine for every server on the bot. It does this for two different tasks. The way the library [DiscordGo](https://github.com/bwmarrin/discordgo) was designed was to spawn a coroutine every time a request was made to the server. The other uses I have for the routines are related to post and data collection. The bot gets random posts off of Reddit and spits them into the Discord channel that made the request. The other function that spawns a thread for each server runs during the initialization sequence of the bot. For each server (or "Guild" as they are referred to in the manual), the goroutine will loop through every single text channel, get its name and ID, and then store that name and ID in the RAM of the system for fast access. This cut the latency to find the name of a channel on a respective server down from about 300ms down to around 1ms. Even with a routine on every server, it still takes around 6.5 seconds to get all of the names and IDs and store them to RAM, but that is loads better than taking 30 seconds to build the cache.

## Conclusion

Go is very powerful and leagues faster than really any Python script. Now is Go going to replace all of my Python scripts and still allow me to prototype in minutes? No. I will still use both where I see fit, but I will use Go where speed is of the essence and if the system is strapped for resources. Both languages have their place, and it my projects they are both two of my most powerful tools.
