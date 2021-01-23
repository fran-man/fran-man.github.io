---
layout: post
title:  "Entropy As A Mental Model Of Complexity"
date:   2021-01-19 21:00:25 +0000
categories: update welcome
---
## Introduction
In my 9 years as a professional software engineer, I've been in many discussions, some technical, some not so technical,
that are related to eliminating or reducing complexity. I'm sure many others will have been too.

These discussions are interesting and often seem practically very useful, however, it can often seem that no matter
how much you discuss and plan to reduce complexity, and how much refactoring, simplification etc. you do, that you still
end up with an over-complicated system.

Because of this and other reasons I observed over the years, something did not sit quite right with me. It wasn't that
the never-ending crusade to eliminate complexity was useless - quite the opposite. But there was something in the way it was discussed
and conceived of that did not seem quite right.

Over the recent months, a way to articulate this has begun to form in my head to the point where I feel I can put it down in writing.
We start with a little background around my inspiration, and then move onto how this might be applied in real life.

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

Another, more common analogy is that of *disorder*. This considers how disordered the system is, and then considers how many
combinations of the microstates (states of items such as atoms), would result in the overall macrostate. For example, 
there are many more ways to have a deck of cards shuffled in a random order than there are to have them in some logical order.
This then to some degree helps explain why shuffling a deck of cards will almost never result in putting the cards back in the right order.
The shuffled deck of cards has more entropy than one in order.

It should be noted that the model of entropy as being representative of disorder is now considered to be not entirely accurate.
However to me, it remains a useful analogy for explain the basic idea.

#### Entropy Is Non-decreasing
The main point here is that in a closed system, entropy must always stay the same or increase. The immediate retort to this,
using the pack of cards analogy, might be *"But I can put the cards back in order again, decreasing the entropy"*. And this would be correct.
But, the deck of cards is not a closed system; the decrease in entropy **must** be offset by an increase elsewhere, for example,
the mixing of the air in the room due to the shuffling, and the dispersal of heat that you cause by physically moving your hands.
An immediate response to *this* might well be *"But those things are irrelevant to me!"* - if this is you, hold that thought for later.

### Conservation of Energy
As explained by [NASA](https://www.grc.nasa.gov/WWW/K-12/airplane/thermo1f.html), the conservation of energy (CoE), is one
of the fundamental concepts within thermodynamics. In short, energy can never be created or destroyed. Strictly, it is moved around
and/or converted from one type to another (e.g. a radiator converts electric energy in the grid into heat energy in your house).
This is an absolute rule and cannot be violated, hence why perpetual motion machines are an absolute impossibility - if
they worked, they would be generating energy from nothing.