---
layout: post
title: "Run Run Run NSRunLoop"
description: "Use NSRunLoop and async operations within a cmdline ObjC program."
category: code
tags: ["iOS"]
image : runloop.png
---
{% include JB/setup %}

Writing commandline applications in Objective-C isn't the most exciting thing
in the world, but I found myself in need of a small utility for refreshing the Core
Data sqlite database which comes pre-bundled within an app. The requirements for this
little application where rather straighforward:

* ##### Grab remote webservice JSON data

* ##### Process and save within Core Data

* ##### Move the newly generated sqlite database to a specific location for inclusion within the app.

To start things off, a new Xcode project was created and the template
"commandline application" was chosen. Ok, looks good I guess.

{%highlight objective-c %}

int main(int argc, const char * argv[]) {

  // program goes here ...
  
  }
{% endhighlight %}

To populate this data, I re-used some AFNetworking code to grab the JSON from
the remote server which looked a little like this :

{% highlight objective-c%}
void (^processSuccess)(NSURLRequest *request, NSHTTPURLResponse *response, id JSON) =
    ^(NSURLRequest *request, NSHTTPURLResponse *response, id JSON){
        printf("[+] Successful hit for URL, creating the things...\n");
        [thingImporter importThings:JSON inContext:context];
        exit(1);
    };

    void (^failBlock)(NSURLRequest *request, NSHTTPURLResponse *response, NSError *error, id JSON) =
    ^(NSURLRequest *req, NSHTTPURLResponse *resp, NSError *error, id JSON) {
        printf("[!!] Fetch request failed to URL\n");
        exit(0);
    };

    AFJSONRequestOperation *networkOp = 
    [AFJSONRequestOperation JSONRequestOperationWithRequest:thingRequest
                                                    success:processSuccess
                                                    failure:failBlock];
    [networkOp start];
    printf("[+] Beginning Import from URL %s\n",[self.urlString UTF8String]);
{% endhighlight %}

When run, the program would kick off the asychronous request, and then exit!
Hey wait a second, if the network call succeeded (which I verified it was) why wasn't
the success block called? 

It turns out the main thread was calling it 'mission complete' before the
background thread making the network call had a chance to complete.

I'll admit, I did naively attempt to delay execution by using while(1),
sleep(), and all manners of terribly time intensive programming, but to no
avail. (and that's probably a good thing..)

### GO NSRunLoop!

NSRunLoop is one of those magical Cocoa frameworks that you don't really know
if there until you need it. Part of the magical background fabric of Cocoa
reality, NSRunLoop continuously manages all things 'event.' ie - keyboard,
touches, and other input. 

From the Apple documentation: 

*An NSRunLoop object processes input for sources such as mouse and keyboard events from the window system, NSPort objects, and NSConnection objects. An NSRunLoop object also processes NSTimer events.*

When creating a strictly commandline application, my program won't have any
keyboard or touch events, so it looks like an NSTimer is required to execute
the job on the NSRunLoop.

So what's an NSTimer?

From the Apple docs:

*You use the NSTimer class to create timer objects or, more simply, timers. A timer waits until a certain time interval has elapsed and then fires, sending a specified message to a target object. For example, you could create an NSTimer object that sends a message to a window, telling it to update itself after a certain time interval.*

The network code was instead abstracted into a 'job' class which was fed to an
NSRunLoop within an NSTimer object. The basic setup looks like this:

{% highlight objective-c %}

  ImportJob *job = [[ImportJob alloc] init];
  job.urlString = [[NSString alloc] initWithUTF8String:argv[1]];

  // NSRunLoops process NSTimer events. Within commandline app, 
  // package up our task within an object and run it via an NSTimer.
  // The 'run' event makes the runloop process data from all available input
  // sources. For our case, this time it is an NSTimer object.

  NSDate *now = [[NSDate alloc] init];
  NSTimer *timer = [[NSTimer alloc] initWithFireDate:now
                                            interval:.01
                                              target:job
                                            selector:@selector(beginImport)
                                            userInfo:nil
                                             repeats:NO];

  NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
  [runLoop addTimer:timer forMode:NSDefaultRunLoopMode];
  [runLoop run];

{% endhighlight %}

beginImport: kicks off the whole process of hitting the network, running the
response through Core Data, and then happily copying out the finished sqlite DB. 

This approach may not be suitable for larger applications, but for a small
commandline app to pre-process a database, it worked out rather well. In the
future, adding more complexity can easily be accomplished by creating more job
classes, and feeding them into the run loop. Quick and dirty programs are
great, but the ability to make sense of them several months down the road is
priceless :)

More on NSRunLoop from this fine article:
[Cocoa Factory, what is an NSRunLoop](http://cocoafactory.com/blog/2012/09/06/whats-a-run-loop-anyway/)
