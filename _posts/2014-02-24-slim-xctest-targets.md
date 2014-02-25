---
layout: post
title: "Slim XCTest Targets"
description: "Slim XCTest targets build faster and are easier to manage.
Properly configuring the test and app target allows for clean code separation."
category: code
tags: ["iOS" , "testing"]
---
{% include JB/setup %}

#### What is XCTest ?
XCTest has made testing on iOS dead simple easy; click a few buttons and a
shiny new XCTest target is added to the project. There are still developers out
there who don't test, and the difficultly of adding test machinary to a project
is no longer an excuse. 

If you're new to XCTest and want to give it a try, I recommend the 2013 WWDC
video "Testing in Xcode 5." Thirty minutes and you'll be up to speed.

## Adding XCTest to an existing project

The procedure to add XCTest to a new project is exactly the same as adding to
an existing except for one important detail. To keep the XCTest target slim,
the setting __Symbols hidden by default__ must be switched to __NO__

![symbols hidden changed to NO](/images/symbols-hidden.png)

## Don't add app sources to the test target!

The XCTest framework is injected into the app target at runtime and now has
access to the symbols contained within. It's not necessary to re-add these
source files to the XCTest Target.

Remember, the XCTest target compile sources only requires test files. That's it!

![xctest target compile sources](/images/slim-test-target.png)
