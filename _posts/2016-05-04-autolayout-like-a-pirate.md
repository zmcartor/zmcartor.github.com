---
layout: post
title: "Autolayout like a pirate"
description: "crush out constraints pirate style with NSLayoutAnchor"
category: autolayout 
tags: ["iOS"]
image : "nslayoutanchor.png" 
---
{% include JB/setup %}

Yeearghh! That's either the sound of a pirate taking over the bridge, or most
likely , someone is writing autolayout code programmatically. 

iOS 6 gave us `constraintsWithVisualFormat`. For most autolayout done via code,
this was ok, however the amount of ceremony required to add a subview, build
the constraints, and create a view dictionary made it a drag. 

## Drop the anchor!

`NSLayoutAnchor` introduced in iOS 9 makes dealing with autolayout an easy, sea breeze.
`UIView` subclasses now have `NSLayoutAnchors` for each right, left, top,
center, bottom, middle points. The mental model of constraints is still valid here. Layout 
anchors are mathematically related to each other using the familiar vocabulary of `leading` , `trailing` and `constant`.
However the syntax is incredibly succinct and I feel flows better with Swift.

Here's adding a subview over top of another view. The `leadingAnchor` of
`littleView` is set to be equal to the `leadingAnchor` of the container. The
remaining edges are set as equal to container and set to active. All only four
lines of code.

{% highlight swift %}
  littleView.leadingAnchor.constraintEqualToAnchor(container.leadingAnchor).active = true
  littleView.trailingAnchor.constraintEqualToAnchor(contianer.trailingAnchor).active = true
  littleView.topAnchor.constraintEqualToAnchor(container.topAnchor).active = true
  littleView.bottomAnchor.constraintEqualToAnchor(container.bottomAnchor).active = true
{% endhighlight %}


## Multiply, Offset, Inequalities

Just like constraints, `NSLayoutAnchors` can also be adjusted via constants, multipliers, and inequalities. 

{% highlight swift %}
  
  // All NSLayoutAnchor methods also take a 'constant' param
  someView.bottomAnchor.constraintEqualToAnchor(view.bottomAnchor, constant: 42)

  someView.topAnchor.constraintGreaterThanOrEqualToAnchor(view.topAnchor, constant: 33)

{% endhighlight %}


## Centering? No problem.
{% highlight swift %}
  
  someView.centerXAnchor.constraintEqualToAnchor(view.centerXAnchor).active = true
  someView.centerYAnchor.constraintEqualToAnchor(view.centerYAnchor).active = true

{% endhighlight %}

Porting existing autolayout code into `NSLayoutAnchor` code could improve
readability, but isn't required. Anything that can be done with constraints can
be achieved with `NSLayoutAnchor`; albeit much less code and "Yeaaargghhhs!"
