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
place a `UIButton` on something. Programmers can get set in a certain way, and
completely miss an alternate way of doing something. Awareness of all the options
and their tradeoffs enables optimal decision making.

While I'm generally against TMTOWTDI in
languages, but it's manfestation on iOS is one of careful progress
rather than glib overcomplication. iOS is a flexible platform; It's interesting 
how many different ways there are to accomplish something; each with their own purpose 
and tradeoffs. I wrote this quick list first, for fun to see how many ways I
could come up with for a given topic. This list is for newbies just learning iOS and looking for a
high level view of different ways to solve a problem.

### Layout
Consider placing a control on the screen at programmer defined coordinates.

- Frame hell, the most basic way. Set a frame and stick on the screen.
  Varying screen sizes and orientations have made this less favorable. Complex
  UI often involves an unintelligible gob of magic constants and opaque
  calculations.

- Autoresizing masks, springs struts. Defining how a view shrinks or expands
  made sense with this simple metaphor.

- Autolayout; the way of the future (so far). Autolayout makes dealing with
  varying device sizes and orientations better than Frames; however this comes at a steeper
  learning curve. There are of couse several ways of configuring Autolayout.

### Layout Organization
Any of the Layout methods can be used with these methods of encapsulating
layout information. 

- The "Code only" means to not use Interface builder and to do the
  AutoLayout/Springs/Frame configuration all within code.  Benefits are source
  control merges are not life threatening and opening a large source file does
  not lock up Xcode for days as when working with Storyboards. A disadvantage
  to working with only code is the inability to visually "see" when you're
  doing which is a boon when creating complex layouts. A picture is worth 1000
  words (of code.)

- With Xibs, UI can be drug out and positioned with whatever method makes
  sense. The 'File's Owner' outlet can be a little confusing. ViewControllers
  which make use of Xibs must be instantiated with `init(nibName nibNameOrNil: String?,
      bundle nibBundleOrNil: NSBundle?)` Because each Xib is it's own file, the
      change for collison in source control is lessened.

- In storyboards, the flow of an application can be understood by
  non-developers. Different types of segues connect mocked out screens and
  backing view controllers. Prototyping flows and coordinating with designers
  is nicer with UX designers who need to see several screens and their
  relation.

### Object Coupling
Objects love to pass around information. For example, a selected
`UITableViewCell` which must pass back information to the previous screen.

- UINotifications are quick and easy when several object care about when
  something happens, and the data associated with that action. However strictly
  relying on NSNotifications produces code with lower cohesion as the cause and
  affect of an action takes some digging.

- Delegate Pattern and Protocols are used extensively within Cocoa. The
  interchangability combined with strict type checking and a clearly defined
  interface make this preferred when extending object functionality.

- Sometimes a delegate and protocol can be a bit heavy. My rule is when a
  delegate protocol has less than 3 methods, block based callbacks make more
  sense. Block still provide strict type checking, but don't littler the
  codebase with sparse delegate protocol declarations.

(I'm sure I forgot others, coupling and cohesion within OO languages is a vast
  topic.)

### Animations
- Depending on what needs animating, a simple `UIView` animation may be enough.
  UIView provides convenience methods in the family of `animateWithDuration:animations` 
  which are great when animating transforms, opacity or other layer properties.

- For more control, there's `CABasicAnimation` and `CAKeyFrameAniamtions` which
  are added directly to the UIView layer. These offer more powerul
  customization and timing.

- If you need realistic physics animations, there's UIKit Dynamics. Bouncing,
  rolling, falling, colliding bodies are tricky to implement completely by hand
  and can feel very unatural.

 - Write your own! With [CADisplayLink](http://www.bigspaceship.com/ios-animation-intervals/) it's possible to write your very own
   animation framework. [Facebook POP](https://github.com/facebook/pop) uses this approach.

### Data Persistence

- NSUserDefaults provides key:value based persistence between launches. Of
  course, don't abuse it as a full-fledged persistence solution. It's main
  purpose is to provide sensible app defaults to the user.

- Core Data is THE mobile database persistence framework for iOS. Entites and relations can be modeled via an object graph.
Core Data has a steep learning curve. Multithreaded data access and persistence while maintaining user responsiveness 
can be a hard balancing act.

- NSKeyedArchiver encodes objects into blobs of `NSData.` The object must
  implement `encodeWithCoder` and provide a way to encode it's properties into
  appropriate representations. If your app doesn't require querying associated
  data, this is a good choice to skip to overhead of Core Data.

### Go it alone
In every area presented, there is enough low-level functionality exposed
within the framworks for a developer to write a completely custom solution if
desired. Maybe you have a better idea for a persistence framework or a new way
to model UIView layout? Before running off to re-implement UIKit (poorly), I
recommened getting to know the whys and hows of solutions presented above. In
many cases, a solution can be found by tweaking and combining several
technologies in a novel way.

What other areas does iOS offer varying ways to do things? Would love to hear
from the community on areas I've passed over.
