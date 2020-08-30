---
layout: post
title:  "Writing a Quine in Kotlin"
date:   2020-08-30 13:34:25 +0100
categories: update welcome
---
## Introduction
For a long time now, writing a [Quine](https://en.wikipedia.org/wiki/Quine_(computing\)) has been something on my programming "bucket list".
I was always intrigued by them, and thought I wanted to try writing one, but I figured it would be difficult and frustrating, so I never got round to it.

Then, the other night, I thought about it again and decided to have a go. With plenty of free time coming up over the bank holiday weekend, it was a perfect opportunity.

In this post I will quickly run through what a quine is, some of the rules, and then I will walk through in detail the steps that I took, and the thought processes that I followed (this went through a few iterations before success!)
## What is a Quine?
According to wikipedia, a quine is 

"*a computer program which takes no input and produces a copy of its own source code as its only output*"

Which is a deceptively simple description. Being more explicit, there are certain things that a quine must or must not do:
* **MUST** Produce its own source code when run
* **MUST NOT** Take any kind of input (this includes reading from disk or a REST API, for example)
* **MUST NOT** Produce any kind of output *other than* the output representing its source code

When you break it down like this, the difficulty of the task becomes more tangible, especially if you begin to consider possible ways to implement such a program.
## Building the Quine - Some Dead Ends
It's pretty obvious that the quine will need some sort of print function, to print a list of strings (the lines). Let's keep it simple and use Kotlin's built in functions. It's easy to throw together a quick example like this:
{% highlight kotlin %}
fun main() {
    val output = listOf("STRING1","STRING2")
    output.forEach { println(it) }
}
{% endhighlight %}

Seems simple enough, right. So now we just replace the example strings with the source code of the file itself....
{% highlight kotlin %}
fun main() {
    val output = listOf("fun main() {",/*What goes here??*/"output.forEach { println(it) }","}")
    output.forEach { println(it) }
}
{% endhighlight %}

Now it should be obvious why this is hard. What should we have as the string representing the line that is storing all the strings? If we copy paste this line into the string, 
then that changes the contents of the line and so the string needs to be updated... we're stuck! We could do this forever and never be finished!
## A Fool's Errand?
At first glance, it might seem now like this is impossible - I mean, every time you change the list of lines to reflect the source code, this changes the source code, and you get stuck in an infinite loop of updating the source code!
It feels like the only way would be to have the quine read it's source file and spit this to stdout...but as per above this is cheating!

However, I know that this is possible, so it's was clear that we simply need a more novel approach.
### An Additional Roadblock
Something else I became aware of when tinkering with this, was the problem or representing strings within strings. Strings are delimited somehow
e.g. `"` in kotlin. So the "inner" quotes would need to be escaped `\"`. But then, what about when this is in the source code? Well,
the slash will need to be escaped too! You end up having to escape escape characters, which gets pretty horrible quickly.
I soon decided that the best way around this was to represent all the strings as byte arrays to get around this, e.g.
{% highlight kotlin %}
print("Hello world")
// Will be represented in the kotlin source as
listOf(112,114,105,110,116,40,34,72,101,108,108,111,32,119,111,114,108,100,34,41)
{% endhighlight %}

*(note: I did not work all of these out myself! I wrote a [small utility](https://github.com/fran-man/Kotlin-Quine/blob/master/src/main/kotlin/com/franm/quine/FileToByteArrays.kt) to help me)*

## A New Perspective
I came to realise that the main thing that caused the recursion issue above, was that I was not being as "efficient" as I could be with the data in my file (i.e. the strings).
What do I mean by this? I mean that, in some sense, we are using each string once. But in a way, we need some of the strings to be used *twice* - 
and I don't mean re-used in two different places. I mean actually used in *two different ways*, assigning two fundamentally different meanings to it.
### Double Meaning
This all seems pretty abstract - what am I going on about and what do I actually mean? Well, when we have a string in some source code, like this
{% highlight kotlin %}
val string = "print(\"Hello world\")"
{% endhighlight %}
In a way this whole line has two meanings.
- A simple string of characters
- (When interpreted by a compiler) a assign some value to a variable called "string"

So what if we actually used some of our lines of code (specifically, those lines representing a line of source as a string) for TWO things?
- Print the string *represented* by the line - the actual source
- Print *the line itself*

If you're still confused, take this away and mull it over for a while.

## Putting it all together
OK, first step is to write a shell of an application, and use a utility (see above), to dump all these lines out as lists of bytes.
Then, we insert this list of bytes into the application. Now, we've altered our application, so it's immediately wrong! However, we can fix this now, with the above thinking.

Think about how we can print the strings in the list in two ways (this took some experimentation to get it character-perfect):
{% highlight kotlin %}
// Print the string represented by the bytes
    fileLines.forEach {
        it.map {char ->
            //map each number to a char e.g. 125 -> }
            char.toByte().toChar()
        }.forEach {char ->
            //Then print the char
            print(char)
        }
        //Carriage return
        println()
    }
    
// Then, print the list itself
    fileLines.forEach { innerIt ->
         // Remember to add prefix/suffix representing the listOf syntax
         print("        listOf(")
         print(innerIt.joinToString(",") + ")")
         println(',')
    }
{% endhighlight %}
We are so close! Now, we just need to put some final sugar at the right place in the code to overcome some subtle issues:
- Do not print a newline when on the final line of the file
- Do not print a comma on the final item in the list
- Keep track of where we are, so we can print out the list of lists at the right point (remember we ARE NOT representing these lists as strings - they are representing themselves...)

I won't explain all these in detail, it's basically just incrementing counters so you know where you are in the file as you iterate.

Now, for one final time, we run the utility to spit out the byte arrays, remove the lines *representing the byte arrays themselves*, and paste the result in.

Give it one final run, and then use your file diff utility of choice to check the result!

## Final Code
The final code can be found [here](https://github.com/fran-man/Kotlin-Quine/blob/master/src/main/kotlin/com/franm/quine/Quine.kt)