---
layout: post
title: "Objective-C method metaprogramming"
description: "A simple introduction to the world of meta-programming in Objective-C. Code provided which illustrates howto metaprogram dynamic instance and class methods"
category: code 
tags: ['iOS', 'metaprogramming']
image: metaprogramming.png
---
{% include JB/setup %}

The Objective-C language is an odd beast; stodgy old C, with it's terseness, speed and heritage combined with a dynamic, message-dispatch runtime.
This amazing symbiosis of is what allows us to take a seemingly static language and dynamically add class and instance methods _while the program is executing!_ The effect of this ability is profound: The ability of a class to dynamicaly change it's behavior makes it enormously re-usable.

This article is a brief introduction to Objective-C metaprogramming. Code which demonstrates dynamic addition of Class and instance methods included. Familiarity with Objective-C is assumed.For completeness I highly recommened taking a glance at the Apple documentation.

[Apple Objective-C runtime reference](https://developer.apple.com/library/mac/#documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html)

#### The Flow
The example is completely contrived, but it's important to understand the basics before tampering with the fabric of the Objective-C universe. Let's break down what's going on here step by step:

* ##### Step 1: resolveInstanceMethod/resolveClassMethod
Right before our program derails into exception canyon, we have one last chance to resolve a missing class/instance implementation. Depending on the type of method (class or instance) the appropriate method is invoked. In this example, the name of the selector is dynamically created by combining the name of the class with a string value. (insaneInstance or crazyClass). For example, a class of 'Coffee' would have a method called 'crazyClassCoffee.'

* ##### Step 2: provide a dynamic implementation with *class_addMethod*
This is where the 'magic' happens! A selector is created within a class along with an (IMP) pointer which ties the new selector to a dynamic implementation. 

* ##### Step 3: return YES
After providing an implementation, returning YES informs the runtime to re-dispatch the method call. Now that the new method has been dynamically added, any following invocations will flow directly to the implementation.

#### Dynamic Instance Method Example
{% highlight objective-c%}

+ (BOOL)resolveInstanceMethod:(SEL)aSEL {
    NSString *classname = NSStringFromClass([self class]);
    NSString *selectorString = [NSString stringWithFormat:@"insaneInstance%@",classname];
    SEL ourSelector = NSSelectorFromString(selectorString);
    if (aSEL == ourSelector) {
        class_addMethod([self class], aSEL, (IMP)insaneInstanceMethod, "v@:");
        return YES;
    }
    return [super resolveInstanceMethod:aSEL];
}

void insaneInstanceMethod(id self, SEL _cmd) {
    NSLog(@"insane instance method has been added!");
}

{% endhighlight %}

#### Dynamic Class Method Example

Adding a class method involves a slight wrinkle when compared to resolveInstanceMethod.
Notice the difference in the first parameters passed to 'class_addMethod'. In resolveInstanceName, this is merely the result of [self class]. However when adding a class method, we're actually adding a new method to the classes META class.

As in the previous example, the name of the method is constructed from the calling class.

{% highlight objective-c %}

+ (BOOL)resolveClassMethod:(SEL)name {
    NSString *classname = NSStringFromClass([self class]);
    NSString *selectorString = [NSString stringWithFormat:@"crazyClass%@",classname];
    NSLog(@"string is %@", selectorString);
    SEL ourSelector = NSSelectorFromString(selectorString);
    
    if (name == ourSelector) {
        // adding class method to meta-class
        Class ourClass = object_getClass(NSClassFromString(classname));
        class_addMethod(ourClass, ourSelector, (IMP)crazyClassMethod, "@v:@");
        return YES;
    }
    return [super resolveClassMethod:name];
}

void crazyClassMethod(id self, SEL _cmd) {
    NSLog(@"crazyClassMethod has been added!");
}

{%endhighlight%}


#### Dealing with ARC
Unfortunately, all this magical metaprogramming still requires a proper method definition within the associated class header file. The reason for this is ARC must know the size of our method's return value to ensure seamless memory management. This is in contrast to Ruby, where methods can be defined without any explicit definition. (Of course, Ruby is not a compiled language).


#### Where to go from here?
The examples above are certainly contrived, but have hopefully served as a solid starting for your own forays into re-usable, dynamic components.

To get the ball rolling, here are a couple examples of applied metaprogramming:

* Add findBy methods for each attribute within an NSManaged Object: [CoreKitty Github](http://hackazach.net/CoreKitty)

* Create more re-usable tableViews: [Less boilerplace UITableview code](http://www.element84.com/metaprogramming-in-objective-c.html)

* Metaprogram DSLs, validation, mocks, ohmy! [Many uses of Metaprograming](http://www.tuicool.com/articles/NRfEri)
