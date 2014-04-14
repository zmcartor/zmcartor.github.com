---
layout: post
title: "iOS Testing with Core Data and OCMock"
description: ""
category: code
tags: ["iOS" , "testing"]
---
{% include JB/setup %}

Models backed by a data persistence layer are too important to simply punt on proper testing. The rest of the app expects the data models to work as designed; data persistence and all. This article presents a solution to unit testing Core Data models using [OCMock](http://ocmock.org/) and [Magical Record](https://github.com/magicalpanda/MagicalRecord). 

### The straight mocking problem
If you're familiar with OCMock, this code may look familiar. If you've never
used OCmock before, [take a look at this tutorial](/code/2014/03/03/effective-testing-with-ocmock/), and the OCMock site for
additional resources. Anyway, let's pretend we have a simple NSManaged object
we're innocently trying to test: 

{% highlight objective-c %}
// data model for Person
// name -- NSString
// lunchOrder -- NSString
// cost -- int16

FUCKING FIX THIS
NSEntityDescription *pDesc = [NSEntityDescription entityForName:@"Person" 
                                         inManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];
Person *p = [[Person alloc] initWithEntity:pDesc 
            insertIntoManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];

id personMock = [OCMockObject partialMockForObject: p];

// argh! Person.name selector not defined, what's going on?
XCTestAssertNil(p.name, @"person not nil");

{% endhighlight %}

The code above creates a new Person object and inserts it into the default
NSManagedObjectContext courtesy of Magical Record. A partial mock is created
and a method stubbed out. However whenever a property of the NSManagedObject is
accessed, we receive an odd 'unknown selector' error. What's going on here ?

### @Dynamic properties
Core Data implements property accesors through @dynamic properties. Write about
the Dynamic stuff and how that works.

### Never, ever mock Core Data
Don't mock out Core Data (Even Saul says so)
Use MR to create in-memory stores for the objects during each test.

{% highlight objective-c %}

This is some code here showing everything works

{% endhighlight %}


