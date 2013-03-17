---
layout: post
title: "We can do better.. no more debugging with console.log"
description: ""
category: 
tags: []
---
{% include JB/setup %}
#We can do better, no more console.log(..)

This is an appeal to JS developers everywhere. 

We can do better than console.log when debugging our code. I know how easy it is to debug via console.log and how fun it is to hit ‘refresh’ and then eagerly check the Firebug or Chrome console. I know it’s almost like being a child at Christmas : *“What did console-claus output for me!!”*

Errant debug statements are always cleaned from the code before it ships, right?

(Yeah, probably…)

For fun, keep the JS console open while browsing the Internet today. Be amazed at the hidden world of errant console.log(..) calls unfolding before your eyes..

Oh, and console.log(..) crashes your shit on IE huh? That’s a real bummer. 

### What do to

*TL;DR - You’ll be able to debug faster and may learn a thing or two about your code.*

A symbolic debugger offers some big advantages over console.log(..)

1.  The ability to see the entire runtime environment at once, including local variable values. A complete view of the execution environment, not just a slice.
2.  The grind of ‘place debug code/refresh’ gets tedious and can turn into a time waster. A symbolic debugger speeds up the process through live tracing and dynamic breakpoints.
3.  No extra code to clean up after debugging is complete.

Get familiar with Firebug Script Console,  or the Chrome/Safari developer tools. 

**Chrome/Safari**



**Firebug**





###

 []: http://www.we-are-gurus.com/blog/1578-avoid-console-log-error-with-ie "console.log and IE"
 []: http://www.nsbasic.com/app/tutorials/TT10.htm "http://www.nsbasic.com/app/tutorials/TT10.htm"
 []: http://getfirebug.com/javascript "http://getfirebug.com/javascript"
 []: http://getfirebug.com/wiki/index.php/Script_Panel "http://getfirebug.com/wiki/index.php/Script_Panel"  
