---
layout: post
title: "The Zen of Swift Optionals"
description: "Swift Optionals reveal all the ways in which a program can fail"
category: 
hero_img : "cool.jpg"
tags: ["iOS", "Swift"]
image: zen.png
---
{% include JB/setup %}

# The world is largely optional
Stop thinking about programming for a second and imagine the world outside computers. Things there can 'not work out all the time. Dinner plans fall through, the machine runs out of coke, the meeting room is booked, someone forgot their wallet. 

The world is full of optionals.

Would you like fries ?

Clothing Optional.

Coffee ?

Network Down.

Tipping Optional

In Swift, optionals keep 'nil' out of bad places that burn the house down. Apps not crashing is an easy benefit; but things go
deeper than that. Optionals pave a safe and pargamatic path through a world of
dangerous code assumptions.

#Optionals Expose Assumptions
In the physical world outside iOS devices, we're attuned to
planning for 'contingencies'. I carry an umbrella in my pack because sometimes
it rains. Weather conditionals are an optional. There's a spare tire in car 
because safe travel is 'optional' and subject to failure. 
In Swift, optionals highlight at the language level those things which can go awry.
The implications are significant when considered
within the context of a larger system. `UIImage(named:)` returns an Optional.
This makes sense, because what if the string was mistyped ? 

##OTHER EXAMPLES##
Parsing JSON can 'not work out.' Fields may be missing. Crashes occur when
passing 'nil' into an NSUrl urlWithString crashes if fed a 'nil' value.
Dictionaries and Arrays burn the house down if they catch a whiff of nil.

# Optionals have your back
In ObjC , methods can sometimes return nil. It is the responsibility of the
developer to be aware of these cases and take precautions should things not go
as planned. Experience has shown that 'taking precautions' can go backseat to
inexperience or deadlines. 

# Optionals 
The power of optionals means the developer doesn't have to aggressively consult
documentation for possible nil return values in order to write "safe" code. Should
a potentially unsafe case arise, the case must be handled by convention of the language. 
Freshly written Swift code has a feeling of confidence and safety that I have never experienced 
with ObjC.
