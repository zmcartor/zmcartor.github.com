---
layout: post
title: "UIVisualEffects in 10mins"
description: "iOS8 UIVisualEffects description, code examples"
category: presentaion
tags: ["iOS", "Design"]
image: uivisualeffects.png
---
{% include JB/setup %}

Slides from a talk I gave at Boulder iOS October Meetup

<iframe src="http://www.slideshare.net/slideshow/embed_code/41064230" width="476" height="400" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"> </iframe>

# Summary
With iOS 8, Apple has provided a new technology which makes it very easy to
create frosted, translucent overlays. These overlays are interactive and will
reflect the state of activity beneath. This is an upgrade from previous
technigues which involved creating static images and applying blur effects.

## Usage
Instantiate an instance of ```UIVisualEffectView``` and hand it an effect.
Presently, the only effects available are instances of either
```UIBlurEffect``` or ```UIVibrancyEffect```.

UIBlurEffects can be configured for dark, light, or extra light. Apply a
```tintColor``` to ```UIVisualEffectView``` to make it fit properly in a
colorscheme.

{% highlight objective-c %}
    UIBlurEffect *blur = [UIBlurEffect effectWithStyle:UIBlurEffectStyleDark];
    UIVisualEffectView *blurView = [[UIVisualEffectView alloc] initWithEffect:blur];
    blurView.frame = self.hudView.bounds;
    [self.view addSubview:blurView];
{% endhighlight %}

UIVibrancyEffect is best used with text, or other controls that need to remain
readable, as frosted overlays can obscure important details in the
foreground.

### Apply Vibrancy
To apply vibrancy, create another ```UIVisualEffectView``` with an instance of
```UIVibrancyEffect.``` *note* Vibrancy needs to know about the current
```UIBlurEffect``` settings. Add the second ```UIVisualEffectView``` configured for
vibrancy as a subview of the first. Just like ```UITableViewCell```,
the ```UIVisualEffectView``` has a ```contentView``` property for subviews.

{% highlight objective-c %}
UIVisualEffectView *vibrantView = [[UIVisualEffectView alloc] initWithEffect:[UIVibrancyEffect effectForBlurEffect:blur]];
vibrantView.frame = blurView.bounds;
UILabel *label = [[UILabel alloc] init];
[label setFont:[UIFont fontWithName:@"HelveticaNeue" size:33]];
label.frame = CGRectMake(8, 30, 400, 500);
label.numberOfLines = 0;
label.text = @"THIS IS VIBRANT TEXT!";
[vibrantView.contentView addSubview:label];

// add vibrantView to first blurView
[blurView.contentView addSubview:vibrantView];
{% endhighlight %}

### Working Example
A working example complete with animations, blur and vibrancy effects is
available here : [UISpookyEffects on Github](https://github.com/zmcartor/UIVisualEffectsDemo) 
