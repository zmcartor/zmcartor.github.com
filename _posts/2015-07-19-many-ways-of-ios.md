---
layout: post
title: "The many ways of iOS"
description: "How many ways are there to do something on iOS?"
category: 
tags: ["iOS", "development"]
image: gitfrog.png
---
{% include JB/setup %}

After spending time refactoring a large legacy app, I've come to recognize a
fact about iOS: _it's TMTOWTDI as heck._ Think about how many ways there are to
put a `UIButton` on screen. While I'm generally against TMTOWTDI in
languages, it's manfestation on iOS is one of careful progress
rather than glib overcomplication. iOS is a flexible platform and It's interesting 
how many different ways there are to accomplish something; each with their own purpose 
and tradeoffs.

## Layout
Consider placing a control on the screen at programmer defined coordinates.

- Frame hell, the most basic way. Set a frame and stick on the screen.
  Varying screen sizes and orientations have made this less favorable. Complex
  UI often involves an unintelligible gob of magic constants and opaque
  calculations.

- Autoresizing masks, springs struts. Define how a view shrinks or expands
  makes sense with this simple metaphor.

- Autolayout; the way of the future (so far). Autolayout makes dealing with
  differnt device sizes and orientations better than Frames; however this comes at a steeper
  learning curve and harder debugging.

## Creatiing a UI

- Go "Code only" commando and forgo Interface builder. Perform all
  AutoLayout/Springs/Frame configuration within code.  Benefits are source
  control merges are not life threatening and opening a large source file does
  not lock up Xcode for days (as when working with Storyboards...)
  Disadvantage is the inability to actually "see" what you're
  doing. Complex layout become an exercise in mental gymnastics.
  A picture is worth 1000 words (of code.)

- With Xibs, a single UI element is visually designed in a single file. The
  flow between screens is less obvious, but multiple contributors can work on
  UI without an inevitable merge collision.
  The 'File's Owner' outlet can be a little confusing. ViewControllers
  which make use of Xibs must be instantiated with `init(nibName nibNameOrNil: String?,
      bundle nibBundleOrNil: NSBundle?)`

- In storyboards, the flow of an application can be understood by
  non-developers. Different types of segues connect mocked out screens and
  backing view controllers. Prototyping and coordinating with designers
  is easier, but more than one developer working with a storyboard will cause
  merge conflicts.

## Object Coupling

- UINotifications are quick and easy solution when several object care about when
  something happens, and the data associated with that action. However strictly
  relying on NSNotifications produces code with lower cohesion. 

- Delegate Pattern and Protocols appear extensively within Cocoa. The
  interchangability combined with strict type checking and a clearly defined
  interface make this preferred when extending object functionality. A happy
  object is one that is closed to modification but open to extensibility.

- Sometimes a delegate and protocol can be a bit heavy. My rule is when a
  delegate protocol has less than 3 methods, block based callbacks make more
  sense. Block still provide strict type checking, but don't littler the
  codebase with sparse protocol declarations.

(I'm sure I forgot others, coupling and cohesion within OO languages is a vast
  topic.)

## Animations
- Most times, a simple `UIView` animation is enough. The block based interface
  even allows configurable spring based movement for a custom feel.
  UIView provides convenience methods in the family of `animateWithDuration:animations` 
  which are great when animating transforms, opacity or other layer properties.

- For more control, there's `CABasicAnimation` and `CAKeyFrameAniamtions` which
  are added directly to the UIView layer. These offer more powerul
  customization and timing.

- If you need realistic physics animations, there's UIKit Dynamics. Bouncing,
  rolling, falling, colliding bodies are tricky to implement completely by
  hand. (or are not a physicist.)

 - Write your own! With [CADisplayLink](http://www.bigspaceship.com/ios-animation-intervals/) it's possible to write your very own
   animation framework. [Facebook POP](https://github.com/facebook/pop) uses this approach.

## Data Persistence

- NSUserDefaults provides key:value based persistence between launches. Of
  course, don't abuse it as a full-fledged persistence solution. It's main
  purpose is to provide sensible app defaults to the user.

- Core Data is THE mobile database persistence framework for iOS. Entites and relations are modeled via an object graph. This is a higher abstration then thinking in tables and SQL relations.
Core Data has a steep learning curve. Multithreaded data access and persistence while maintaining user responsiveness 
can be a tricky balancing act.

- NSKeyedArchiver encodes objects into blobs of `NSData.` The object must
  implement `encodeWithCoder` and provide a way to encode it's properties into
  appropriate representations. If your app doesn't require querying associated
  data, this is a good choice to skip to overhead of Core Data.

## Go it alone
In every area presented, there is enough low-level functionality exposed
within the framworks for a developer to write a completely custom solution if
desired. Maybe you have a better idea for a persistence framework or a new way
to model UIView layout? Before running off to re-implement UIKit (poorly), I
recommened getting to know the whys and hows of solutions presented above. In
many cases, a solution can be found by tweaking and combining several
technologies in a novel way.

What other areas does iOS offer varying ways to do things? Would love to hear
from the community on areas I've passed over.
