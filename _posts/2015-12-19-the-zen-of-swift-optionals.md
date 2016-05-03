---
layout: post
title: "The Zen of Swift Optionals"
description: "Swift Optionals allow anyone to write bulletproof programs by
embracing uncertainty"
category: 
hero_img : "cool.jpg"
tags: ["iOS", "Swift"]
image: optionalzen.png
---
{% include JB/setup %}

Ever forgotten your wallet? Had dinner plans not work out? Phone numbers are lost, pockets can have holes. In the real world, there are no guarantees. Sometimes things just don't work out as planned. 

### _The world is largely optional_

- The Network can fail.
- Cars run out of gas.
- They forgot your name.
- Bikes don't yield and hit people.

Now let's think about computer programs. Even through we, as programmers, control the whole of reality, a boundary of uncertainty still exists. Within a program, conditions we thought were constant can also "not work out." It's the digital equivalent of dinner plans falling through.

### _How do programs work in a world of uncertainty?_
- Again, the network can fail.
- The write failed, filesystem is full.
- Session token is not present.
- A fellow developer redefined the meaning of 'true'

Dealing with failures in the real world are things we handle all the time. If dinner plans fall
through, new plans are made (or pizza is ordered.) One doesn't end up starving
because a present condition failed to be true. In the digital world, things are
much different. Programs crash or [delete your entire company](http://www.snopes.com/man-deletes-company-code/) if conditions
deviate from the razor-thin assumptions held during execution. To deal with
these uncertainties, programming languages enable the programmer to specify
steps to take in the event of a failure. However in practice, these mechanisms
are often under utilized, cumbersome, and typically reserved for the most likely
failure conditions. 

## Swift Optionals are special
Swift embraces uncertainty through the concept of 'optional' types. 
Optionals are a first class type that represents uncertainty. There may or may
not be a value inside. The guard construct allows optionals to be handled
gracefully and easily without obtuse try/catch/fail constructs littering the
codebase. By being easier to use than exception handling, I believe this makes
writing bulletproof programs easier and more accessible in Swift than Objective-C.

Optionals keep 'nil' out of bad places that burn the house down.

## Optionals Expose Assumptions
Look through your old ObjC codebase. Is every call to `[NSURL initWithString:]`
checked to ensure `nil` is not passed by mistake ? Or how about every call to
`[UIImage imageNamed:]` ? Sometimes image names and URls just don't work out.
(and don't even think about putting nil into an NSDictionary..)

Parsing JSON can 'not work out' in all kinds of ways. Fields may be missing, or
could be the wrong type. A certain API which will remain nameless had the bad
habit of randomly returning "string-ly typed" numbers. In ObjC, this was
tedious to debug, but not something that is uncommon. Fortunately in Swift,
casting a type with the `as?` operator has our back and returns an optional.

## Optionals have your back
In ObjC many methods return nil. Back in the old days, this was a common way of
indicating failure. We didn't have fancy optionals like today's kids ;) It is the responsibility of the
developer to be aware of these cases and take precautions should things not go
as planned. Experience has shown that 'taking precautions' can go backseat to
inexperience or deadlines.

## Easier. Safer. 
The power of optionals means the developer doesn't have to aggressively consult
documentation for possible nil return values in order to write "safe" code. Should
a potentially unsafe case arise, the case must be handled by convention of the language. 
Freshly written Swift code has a feeling of confidence and safety that I have never experienced 
with ObjC. I welcome and look forward to a Swift-y future, with all it's `guarded` uncertainties.

