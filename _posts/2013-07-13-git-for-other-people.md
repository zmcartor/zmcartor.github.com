---
layout: post
title: "Git for other people"
description: "Explanation of Git for non developers using soup as an analogy for version control"
category: git
tags: ['git']
---
{% include JB/setup %}

Development, like any specialized industry is exceptionally chock full of
ridiculous sounding, but enabling technical jargon. Explaining these abstract
concepts to other less technical folks can be quite a challenge; especially
when explaining these concepts involves building on other equally esoteric
ideas with little analog to the physical world. One of these concepts
is Git - with it's hashs, pull requests, forks, refs, branches, merges,
re-bases, etc. What on earth are they talking about?

I recently needed a way to explain the Git workflow to non technical project managers, so they could better understand our workflow and be informed about the state of various features across multiple teams. Instead of sending them links to cryptic man pages, as any UNIX beard would do, I explained it using an example I hope will be useful to other developers seeking to educate their less-technical brethern on the process of Git. This short story is completely non-jargonified, and can serve as an introductory piece for those new to Git. Anyway, I hope you enjoy it!

Now, on with the story!

 ![](/images/ChefPenguinTitleCard.png)

The restaurant where Chef Penguin resides is widely reknowned for it's ability to quickly  produce exceptionally delicious soups. Now, Chef Penguin is a skilled gourmand, able to recognize the subtle differences and savory pairings of spices, broth, and meats. But his skill is only half the story: the kitchen's ability to produce such high quality soups also relies on a unique collaborative process. Chef Penguin employs three Sous Chefs which every morning, take a crock of soup from the Master serving pot:

<!--- picture of each chef and their own crock of soup taken from master serving pot. -->

Customers are dished up from the Master serving pot as well. When taking their own crocks of soup, it's important each Sous Chef ladle from the same pot as customers.
Throughout the day, each Sous Chef experiments within their own crock of soup. 

<!---
one picture:
 picture of adding new spices
 picture adding grilled chicken
 picture of chili power exploding on soup
 -->

Add new spices, Grilled Chicken, Whoops that's too much chili powder!

Accidents may happen, but that's ok! When a Sous Chef doesn't like the outcome, it's no problem to dump out their crock, and ladle up another from the Master pot. (The Master pot is very very big, so there's never any chance of running out of soup.)

Yum! Those new spices are delicious. The sage and rosemary really bring out the savory side of the eggplant and rutabaga! What does Chief Chef Penguin think?

When a Sous Chef has something delicious they would like to add to the Master pot, they submit a 'pour' request. Chief Chef Pengin first tastes the change and if accepted, added to the Master pot.

<!--- picture of Chef penguin thumbs up, and adding to Master pot -->

Great! Customers will love those new spices. Now that the Master pot has new spices, the other Sous Chefs must make sure to ladle in the new spices to their crocks.

 <!--- picture new spices ladled into other pots, arrows? -->

Hmm, the new spices don't taste quite right with the grilled chicken, so this Sous Chef switches out grilled chicken for sauteed chicken.

Sometimes pour requests are rejected, but that's ok, it's all part of the process.

and on and on throughout the afternoon Chefs continue to experiment, making the Master pot of soup better and better with every 'pour' request.
* add various tweaks in private crock of soup
* submit pour requests to Chief Chef Penguin, and 
* fetch new updates from the Master pot 

Continue throughout the day until dinner time. Now the final product is served to hungry customers. 

<!---
 picture of happy customers
 -->

Yum! Great job everyone, see you all tomorrow!
