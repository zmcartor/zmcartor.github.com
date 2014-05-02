---
layout: post
title: "Testing Core Data with OCMock"
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

NSEntityDescription *pDesc = [NSEntityDescription entityForName:@"Person" 
                                         inManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];
Person *realPersonModel = [[Person alloc] initWithEntity:pDesc 
            insertIntoManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];

id personMock = [OCMockObject partialMockForObject: realPersonModel];

// argh! Person.name selector not defined, what's going on? 
// OCMockObject wreaked Core Data @dynamic selectors 

XCTestAssertNil(realPersonModel.name, @"person not nil");

{% endhighlight %}

The code above creates a new Person object and inserts it into the default
NSManagedObjectContext using helpers provided by Magical Record. A partial mock of 'Person' object is created with OCMock.
However, whenever a property of the NSManagedObject is accessed, we receive an odd 'unknown selector' error.

### @Dynamic properties
Core Data implements property accesors through @dynamic properties. Any property marked @dynamic means the class will figure out howto respond to the selector at runtime. Unlike @property, an automatic _ivar is not created when using @dynamic. When an NSManagedObject is passed to OCMock ```mock object``` or ```partialMockForObject``` the @dynamic selectors are wrecked. 

### Never, ever mock Core Data
Don't mock out Core Data. There is too much proprietary weird stuff going on to trust the test completely. If this feels dirty to you, another option is to abstract any business operations out of your NSManagedObjects into a service layer which can be properly mocked. 

#### In memory store

When testing with XCTest, the ```+ setUp ``` and ```+ tearDown``` methods create a new in-memory Core Data stack and tear it down once the tests have completed. I find this provides the speed and test isolation necessary to 'trust' the tests.

{% highlight objective-c %}
+ (void)setUp {
    [MagicalRecord setupCoreDataStackWithInMemoryStore];
}

+ (void)tearDown {
    [MagicalRecord cleanUp];
}

{% endhighlight %}

### Verify NSManagedObject methods without wrecking @dynamic properties

Besides providing mock objects, OCMock still has use by providing stubs, block callbacks, and verifying method calls.

{% highlight objective-c %}
NSEntityDescription *pDesc = [NSEntityDescription entityForName:@"Person" 
                                         inManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];

// dont pass this model to OCMock, but use this model within assertions or pass to other components.
Person *realPersonModel = [[Person alloc] initWithEntity:pDesc 
            insertIntoManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];


// OCMock will wreak out @dynamic properties, so make a new model for mock partial
Person *modelForMocking = [[Person alloc] initWithEntity:pDesc 
            insertIntoManagedObjectContext:[NSManagedObjectContext MR_defaultContext]];

// create our stubs on this mock
id groupModelMock = [OCMockObject partialMockForObject:modelForMocking];
[[[groupMock stub] andReturn:@"ZachDude"] name];

// oddly enough, now calling realPersonModel.name will return @"ZachDude".

{% endhighlight %}

### Programmer discovers howto use OCMock with Core Data using this one weird trick!
Creating two NSManagedObjects, one for use within assertions and another to feed into OCMock for stubbing, etc works because of this reason. (taken from OCMock docs)
>Partial Mocks
>
>Creates a mock object that can be used in the same way as anObject. When a method that is not stubbed is invoked it will be forwarded to anObject. When a stubbed method is invoked using a reference to anObject, rather than the mock, it will still be handled by the mock.

In laymens terms, this means by passing ```modelForMocking``` to partialForMock:, the other Person model, ```realPersonModel``` is also affected. Except now that ```realPersonModel``` can finally resolve it's @dynamic selectors, we can contiue to test in comfort.

