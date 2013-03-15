---
layout: post
title: "iOS Network Testing"
description: ""
category: 
tags: []
---
{% include JB/setup %}
[Permalink](http://hackazach.net/post/44988952817/ios-network-testing "Permalink to HackaZach; • iOS Network Testing")

# HackaZach; • iOS Network Testing

During development of an API client, do you require VPN connectivity to hit web service? Is a network service your app relies on a little less than completely reliable? oh and each network error handler has been tested  to ensure things keep working, right? Yeah - Maybe not. Network testing can be a real pain in the ass. The aim of this article is to lower the aching pain in your backside while testing iOS apps; and to hopefully make it a part of normal workflow. 

This article contains a sample walk-through of testing an imaginary web service with ILTesting, AFNetworking and Kiwi. It’s assumed the reader is familiar with the basics of Cocoa Touch, XCode and project creation.

If you’re a solid iOS dev but not familiar with Kiwi or AFnetworking, please check them out:

[Kiwi][1] 

[AFNetworking][2] 

It should be noted the approach shown using ILTesting will work with most any testing framework or networking library on iOS.

**Example DogPark Application**

The example application repo is located here on Github: [DogPark][3]

In our example application, we’ll be testing an implementation of an imaginary Dog Park API client. The client sends requests over HTTP and presently has two methods:

We’ll get to the fun of testing a Dog Park API soon, but first a little about the network testing library we’ll be using. 

**Intro To ILTesting ([GitHub][4])**  


The magic behind ILTesting is simple: it ingeniously registers itself as an NSURLPrototcol handler thereby getting “first dibs” to handle HTTP requests from the application. (A more technically in-depth article on NSURLProtocol is available here : [NSHipster][5] .)

The registration of a pre-defined HTTP handler effectively keeps our app from hitting the network. (This speeds up our tests!) But the main point is predictively crafting an HTTP response for a given URL. 

(If you’re wondering ‘why not just mock AFNetworking instead’, I shy away from mocking code that I did not write myself.)

Let’s take a look at a couple ILTesting class methods:

**The ILTesting Delegate**

ILTesting allows for delegation of client response data to a class which implements the  protocol.

 The protocol-delegate protocol looks like this and includes several optional methods:

Within the sample application, the delegate is implemented in HKZFakeWebsServer.

A tip for dealing with large APIs is to have several different classes each implement CannedURLProtocol and break up response testing between various functional areas. Ex - billing, user authentication, etc.

Enough stuffy code talk, let’s get going and have some fun! 

**Back to the DogPark**

**![image][6]**

So anyway, we need to test our example APIClient methods. When beginning network testing, this process has proved successful:

1. Capture the desired JSON response from the API and save it within a file. (In the sample application, these are kept within the DogParkTests/JSON folder.

2. Create a trigger within the ILTesting delegate to serve the response.

3. Write a test which utilizes the response.

Take a look at HKZFakeWebserver which implements ‘*responseDataForClient’* which is self explanitory ;) - it loads various JSON files as response data when specific URLs are requested. The request is matched on HTTP verb and path.

**Taking things further - Completing the implementation**

Within the sample application, both tests presently fail. I leave the implementation of the client to the reader as an exercise in using ILTesting. The example code also serves as a reference to employ this testing method in your own project.

Happy testing!

References:  
[1] Credit for the original idea of injecting test data via NSURLProtocol goes to Claus Broch : [www.infinite-loop.dk][8]

 [1]: https://github.com/allending/Kiwi/wiki/Guide:-Up-and-Running-with-Kiwi "Kiwi"
 [2]: https://github.com/AFNetworking/AFNetworking
 [3]: https://github.com/zmcartor/DogPark-ILTesting
 [4]: https://github.com/zmcartor/ILTesting
 [5]: http://nshipster.com/nsurlprotocol/
 [6]: http://media.tumblr.com/547a729f43c4bddfcc8603571dc8ecb9/tumblr_inline_mjfb450f2O1qz4rgp.jpg
 []: http://www.infinite-loop.dk/blog/2011/09/using-nsurlprotocol-for-injecting-test-data/
 [8]: http://www.infinite-loop.dk 
