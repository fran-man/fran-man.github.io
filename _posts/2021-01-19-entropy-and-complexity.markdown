---
layout: post
title:  "Entropy As A Mental Model Of Complexity"
date:   2021-01-19 21:00:25 +0000
categories: update
---
## Introduction
In my 9 years as a professional software engineer, I've been in many discussions, some technical, some not so technical,
that are related to eliminating or reducing complexity. I'm sure many others will have been too.

These discussions are interesting and seem practically very useful. However, it can often seem that no matter
how much you discuss and plan to reduce complexity, and how much refactoring, simplification etc. you do, that you still
end up with an over-complicated system.

Because of this and other observations I made over the years, something did not sit right with me. It wasn't that
the never-ending crusade to eliminate complexity was useless - quite the opposite. But there was something in the way it was discussed
and conceived of that seemed wrong and sometimes counter-productive.

Over the recent months, a way to articulate this has begun to form in my head. I'm now at a point where I think I can
put it down in writing. We start with a little background around my inspiration, and then move onto how this might be
applied in real life.

### Disclaimer
Some of the ideas I refer to below talk about certain concepts from Physics. I feel it necessary to make clear that I am
not a Physicist; I am a Software Engineer. I have no more than a casual interest in Science and Theoretical Physics. Therefore,
I do not claim that any of the below explanations are 100% scientifically accurate, nor do I need or want them to be. I wish to use them
as framing and motivation for a philosophical discussion, which shall follow.

## Entropy and The Conservation of Energy

### Entropy
There are a number of different definitions of Entropy available; some may be very academically correct but hard to understand,
while others may be the opposite.

One of the explanations often given is the [statistical one](https://en.wikipedia.org/wiki/Entropy_(energy_dispersal\)).
This relates to the concept of considering how "spread out" the energy is (e.g. temperature, pressure), and then consider

*[...]the energy dispersal for a system by the number of accessible microstates, the number of different arrangements
of all its energy at the next instant.* (From Wikipedia).

This then means that a system is statistically inclined to a higher-entropy state where the energy is more spread out.
This explains why, for example, when left undisturbed, heat will spread evenly throughout a room over time.

Another, more common analogy is that of *disorder*. This considers how disordered the system is, and then considers how many
combinations of the microstates (states of items such as atoms), would result in the overall macrostate. For example, 
there are many more ways to have a deck of cards shuffled in a random order than there are to have them in some logical order.
This then to some degree helps explain why shuffling a deck of cards will almost never result in putting the cards back in the right order.
The shuffled deck of cards has more entropy than one in order.

It should be noted that the model of entropy as being representative of disorder is now considered to be not entirely accurate.
However to me, it remains a useful analogy for explain the basic idea.

#### Entropy Is Non-decreasing
The important point that I want to make, is that in a closed system, entropy must always stay the same or increase.
The immediate retort to this, using the pack of cards analogy, might be *"But I can put the cards back in order again,
decreasing the entropy"*. And this would be correct. But, the deck of cards is not a closed system; the decrease in
entropy **must** be offset by an increase elsewhere, for example, the mixing of the air in the room due to the shuffling,
and the dispersal of heat that you cause by physically moving your hands. An immediate response to *this* might be
*"But those things are irrelevant to me!"* - if this is you, hold that thought for later.

### Conservation of Energy
As explained by [NASA](https://www.grc.nasa.gov/WWW/K-12/airplane/thermo1f.html), the conservation of energy (CoE), is one
of the fundamental concepts within thermodynamics. In short, energy can never be created or destroyed. Strictly, it is moved around
and/or converted from one type to another (e.g. a radiator converts electric energy in the grid into heat energy in your house).
This is an absolute rule and cannot be violated, hence why perpetual motion machines are an absolute impossibility - if
they worked, they would be generating energy from nothing.

## Application to Software Engineering
OK, great. We've talked about entropy and CoE which are try physics concepts, which surely is nothing to do with working as a
software engineer, right? These are totally unrelated fields after all.

Maybe not, though. At the lowest level, computers are just moving subatomic particles (electrons) around. They also convert
lots of energy from one type to another, as anyone who has run a computer in a small room will confirm! So, maybe it is not
such a non-sequitur to jump between these two topics. I'm not saying there's actually a direct causation here, but it's
an interesting consideration.

Anyway - I've been in many situations where you look back and despair that you were not able to get rid of all the complexity
from your system. What happened? I started to think that maybe "reducing the complexity" was the wrong goal to aim for.
My interest and awareness in the topics mentioned above led me to consider another viewpoint:

*Instead of eliminating or reducing complexity, we instead focus on rearraging, moving around, or "converting" the complexity*

The key question then becomes how can you rearrange the complexity in such a way that it is beneficial to you?

This might sound odd but I have considered some examples to illustrate

### Examples

#### Refactoring Functionality into a Class
We've all experienced code that was written without much planning. We can end up with functions strewn all over the place
and interlinked, known affectionately as "spaghetti code". This might often be described by the maintainers as "complicated";
it's hard to understand, hard to debug, and hard to extend.

Now suppose you roll your sleeves up and write a service class for all the code that's related. This makes it easier to use,
easier to extend, easier to see the whole picture, and harder to make mistakes. Hooray - complexity reduced! Well, sort of.
In the real world you may well come across some issues when doing this refactor, that mean you have to add some little bits of complexity
along the way to keep everything working. You have also created a new class that you now need to consider the usage of
- Is it a singleton?
- Does it carry any state?
- How is it instantiated?

And there is also the *work* that you have done to transform the code from one form into another. But, in general circles, these will all be seen as positives,
or necessary evils. So, going back to the point above, we have *converted* and *rearranged* the complexity into a form
that is more agreeable and that we can more easily deal with.

#### Replacing Code With a Framework
You've written your app from scratch and it has properties that it uses to determine how it runs. You are reading these all from disk,
having to parse the text, check for errors, and then use the values. Beyond one or two properties this is going to get annoying.

So let's say you decide to use a framework like spring boot for this. Spring handles the wiring of all your properties, checks the types,
and puts the values where they need to be. It scales up nicely to hundreds of values easily and all you have to do is write a bit of 
yml - complexity gone!

Well - again not really. The code you can *see* is a lot simpler. But really you've offloaded your complexity to spring boot.
Spring boot is a *massive* project that I often hear people refer to as being too complex or "magic". So really, this
activity was more a case of moving your complexity from somewhere you didn't want it (your own code) to somewhere you
can tolerate it (the framework). Whether this is tolerable and worthwhile for you is a personal decision.

#### Simplifying Some Deliberately Complex Code
A thought occurred to me, "what about code that you deliberately inserted artificial complexity into?" such as:

    Random r = new Random();
    
    int ten = 0;
    
    while (ten != 10) {
        ten = r.nextInt(20);
    }

Now this is artificially complex - there's a loop and random number generation when really all we need is

    int ten = 10;
    
So what gives? Surely we can do this simplification for "free"? Well, almost, but we can observe that the complexity is
"converted" into cognitive load as you examine the complex code, figure out what it should be doing, and re-write it. In this case,
the complexity is offset by the need to "do work", much like the CoE example. Obviously, this is an absurd example and
no one in their right mind would argue that the small amount of effort to refactor this
is not worth it. But still it is worth consideration as an illustrative example.

## Conclusion
We began with an observation about the struggles of eliminating complexity from code and software systems. We then took
a layman's view of the Physical concepts of Entropy and Conservation of Energy, to set the scene. We then discussed how
we might lift some aspects of these concepts and lay them on top of software engineering, focusing on complexity reduction.
Finally, we took a look at some examples to illustrate the point in a more understandable way.

I hope this has been somewhat interesting and hopefully may promote further thoughts and discussions in your work.
If you've found this useful or interesting, please share this idea with others. Further, if you have any comments or input,
feel free to reach out to me at my twitter handle.