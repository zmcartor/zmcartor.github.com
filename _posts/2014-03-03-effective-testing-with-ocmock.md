---
layout: post
title: "Effective iOS testing with OCMock"
description: "Walkthrough of OCMock and how mocking features can be applied to common test scenarios"
category: code
tags: ["iOS", "testing"]
---
{% include JB/setup %}

### Audience
This article assumes the reader is familiar with testing in Xcode 5 using [XCTest](https://developer.apple.com/library/ios/documentation/ToolsLanguages/Conceptual/Xcode_Overview/UnitTestYourApp/UnitTestYourApp.html), BDD style [Kiwi](https://github.com/allending/Kiwi) or another iOS testing framework.

### What is mocking ?
![paper tiger](/images/paper_tiger.jpg)
When unit testing, it's imperative to instantiate as little concrete components as possible to keep tests short, fast, and preserve unit isolation. In a modern Object Oriented system, the component under test will likely have several object dependencies. Instead of instantiating dependencies as concrete classes, we use mocks. Mocks are 'fake' objects with pre-defined behavior to stand-in for concrete objects during testing. The component under test does not know the difference! With mocks, a component can be tested with confidence that it behaves as designed within a larger system.
<br/>
### Common mock use cases

#### Stubbed methods
To start, here's a simple example which explains the general stubbing grammar in OCMock : 
{% highlight objective-c %}
 id jalopy = [OCMock mockForClass[Car class]];
 [[[jalopy stub] andReturn:@"75kph"] goFaster:OCMOCK_ANY units:@"kph"];
 
 // if returning a scalar value, andReturnValue: can be used
 {% endhighlight %}
 
 This contrived example first creates a mock jalopy from the class Car. Next, it stubs out the method  ``` goFaster:``` to return the NSString ```@"75kph"```. The stubbing syntax may seem a litle weird at first, but this is the general idea :
 
 ```ourMockObject``` stub] ```whatItShouldReturn``` ] ```method:```
 
**One very important note:** , notice the usage of ```OCMOCK_ANY```. When specifying a method which takes parameters, the mock will return the value specified by ```andReturn:``` *only* when the method is invoked with the provided parameters. The macro ```OCMOCK_ANY`` tells the stub to fire for any parameter value. In the example, an invocation of 
 
 {% highlight objective-c %}
[car goFaster:84 units:@"mph"];
{% endhighlight %}

Would _not_ trigger the stub because the last parameter ```units:``` does not match ```@"kph"```

#### Class methods
OCMock will find class methods on the mock instance as long as there is not an instance method with the same name. In the event of same named methods, employ the ```classMethod``` method:

{% highlight objective-c %}
[[[[jalopy stub] classMethod] andReturn:@"expired"] checkWarrany];
{% endhighlight %}
</br>

#### Types of mocks - niceMock, partialMock
OCmock provides several diffent types of mocks, each with their specific use case.

Any mock created in this fashion : 
{% highlight objective-c %}
 id mockThing = [OCMock mockForClass[Thing class]];
{% endhighlight %}

Is what I call a 'vanilla' mock. Vanilla mocks will raise an exception if an un-stubbed method is invoked. This can get a tedious as every single method called during the mock's lifecycle must be stubbed. (more on stubbing in the next section)

If stubbing out many methods isn't your thing, use a 'nice' mock. Nice mocks are polite and do not raise an exception if an unstubbed method is invoked.

{% highlight objective-c %}
 id niceMockThing = [OCMock niceMockForClass[Thing class]];
{% endhighlight %}

The last type of mock is a 'partial' mock. When an unstubbed method is invoked, that method call is forwarded onto the actual object. That's technically cheating with mocks, but can be useful when there are aspects of the class which don't lend themselves well to stubbing.

{% highlight objective-c %}
Thing *someThing = [Thing alloc] init];
id aMock = [OCMockObject partialMockForObject:someThing]
{% endhighlight %}

<br/>

#### Verify method was or wasn't called
Verifying a method was or was not called is easy. This can be accomplished by ```expect```, ```reject``` and ```verify``` methods :

{% highlight objective-c %}
 id niceMockThing = [OCMock niceMockForClass[Thing class]];
 [[niceMockThing expect] greeting:@"hello"];
 
 // verify the method was called as expected
 [niceMocking verify];
 
{% endhighlight %}

The ```verify``` method will throw an exception if the method was not called. If you're using XCTest, wrap the ```verify``` call within an ```XCTAssertNotThrow```. Reject works the same way, but will throw when the method _is_ called on the mock. Just like when stubbing, the selector and arguments passed to ```verify``` must match those passed by the caller. Use ```OCMOCK_ANY``` to make things easier.
<br/>

### In closing
![paper tiger](/images/origami-swan-cute.jpg)
Hopefully this article and examples have clarified some of the most commons uses of OCMock. The OCMock site: [http://ocmock.org/features/](http://ocmock.org/features/) is a the best reference for everything in the OCMock world.

Mocking can be tedious but is necessary to fully test a modern, OO system. If a dependency graph is difficult to test with mocks, this is an indication the design may need to be re-considered.

### Another great OCMock article
[Making fun of things with OCMock](http://alexvollmer.com/posts/2010/06/28/making-fun-of-things-with-ocmock/)