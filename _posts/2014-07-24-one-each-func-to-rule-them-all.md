---
layout: post
title: "One .each func to rule them all"
description: "Swift implementation of an each function"
category: code
tags: ["iOS", "Swift"]
---
{% include JB/setup %}

While translating some project code into Swift for fun, I started getting sick of using
a 'for' loop to iterate over collection objects like Array, Dictionaries or tuples.
There must be a better way!

[Collection Each](https://github.com/oarrabi/Collection-Each) adds several methods via extensions
to Dictionary and Array. This is a reasonable approach, but requires separate extensions for
each class.

Luckily, there is one function to rule all Sequences :

{% highlight objective-c %}

 func each<S:Sequence, T where T==S.GeneratorType.Element>(sequence: S, callback:(T) -> Void) {
        for item in sequence {
            callback(item)
        }
    }

{% endhighlight %}

The beginning type specifiers are a little weird, so let's walk through it.
First, S is expected to be a type of Sequence. This represents a Tuple, Array, Dictionary, or any
other class which implements the Sequence protocol. Sequences implement a generator
which returns the next item in the sequence, or nil. Sequence.GeneratorType.Element
is a type which corresponds to the 'type' of object within the sequence.

I first tried a simple (T) type with no specifiers, but this did not work, unfortunately.
Our one .each method is callable with a trailing closure as:

{% highlight objective-c %}

[1,2,3,"bench!"].each {println($0)}

{% endhighlight %}

References:
[Mostly Software, Playing With Swift](http://schani.wordpress.com/2014/06/03/playing-with-swift/)
