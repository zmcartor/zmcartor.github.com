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

Ever forgotten your wallet? Had dinner plans not work out? In the real world, there are no guarantees. Sometimes things just don't work out as planned. 

### _The world is largely optional_

- The Network can fail.
- Cars run out of gas.
- They forgot your name.

Now let's think about computer programs. Even through we as programmers control the whole of reality, a boundry of uncertainty still exists. Within a program, conditions we thought were constant can also "not work out." Just like in the real world. 

- Again, the network can fail.
- The write failed, filesystem is full.
- Session token is not present.

Dealing with failures in the real world are things we handle all the time. If dinner plans fall
through, new plans are made (or pizza is ordered.) One doesn't end up starving
because a present condition failed to be true. In the digital world, things are
much different. Programs crash or [delete your entire company] if conditions
deviate from the razor-thin assumptions held during execution. To deal with
these uncertanties, programming languages enable the programmer to specify
steps to take in the event of a failure. However in practice, these mechanisms
are often under utilized, cumbersome, and typically reserved for the most likely
failure conditions.


## Swift Optionals are special
Swift embraces uncertainty by having the 'optional' type as part of the
language. A first class type that represents uncertainty allows it to be
[better than exception handling ]. Seemlessly fits within the language to
provide a richer uncertainty handling than exceptions/assertions. [Options are
uncertainty handling ]

In Swift, optionals keep 'nil' out of bad places that burn the house down.

#Optionals Expose Assumptions
Look through your old ObjC codebase. Is every call to : [examples] 

NSURL(string:) , NSDict checks

within the context of a larger system. `UIImage(named:)` returns an Optional.
Parsing JSON can 'not work out.' Fields may be missing. Crashes occur when
passing 'nil' into an NSUrl urlWithString crashes if fed a 'nil' value.
Dictionaries and Arrays burn the house down if they catch a whiff of nil.

# Optionals have your back
In ObjC many methods return nil. Back in the old days, this was a common way of
indicating failure. We didn't have fancy optionals like today's kids ;) It is the responsibility of the
developer to be aware of these cases and take precautions should things not go
as planned. Experience has shown that 'taking precautions' can go backseat to
inexperience or deadlines. 

# Optionals 
The power of optionals means the developer doesn't have to aggressively consult
documentation for possible nil return values in order to write "safe" code. Should
a potentially unsafe case arise, the case must be handled by convention of the language. 
Freshly written Swift code has a feeling of confidence and safety that I have never experienced 
with ObjC.
