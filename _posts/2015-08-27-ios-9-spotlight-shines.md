---
layout: post
title: "iOS 9 Spotlight Shines"
description: "Introduction to the power and possibility of new iOS 9 Search APIs"
category: 
tags: ["iOS"]
image : search.jpg
---
{% include JB/setup %}

![They call me the seeker](/images/search.jpg)

## TL;DR
iOS 9 search APIs allow apps to add their own search results to Spotlight search. Previously, only the name of the app appeared as a Spotlight search result. Spotlight is also able to show relevant results from apps which are not installed on the phone, resulting in richer app discovery.

Before iOS 9, apps were opaque bundles of information. Data, photos, contacts, and documents were locked away from enhanced discoverability by the operating system. So much potential for bookmarking and richer media integration with iOS. However, with iOS 9, the dystopian past of poor discoverability has finally ended. 

This article provides an introduction to two new iOS 9 search APIs. The third type of new search technology, Web Markup, will be covered in a separate article.


## Activity Indexing
Activity Indexing allows user activity to drive the relevancy of search results. This is accomplished by creating `NSUserActivity` entries when a user creates, views or performs an action worth recalling at a later time. These user activity records represent a 'state' of the app; such as a specific screen, document, or point within a media file. Kind of like website bookmarks, but for an app. 

To become searchable, entries must be added to a search index. An `NSUserActivity` can be marked as either public or private. Public activities will be pushed up to Apple and facilitate app discovery. When the amount of public `NSUserActivity` records submitted from your app reaches a certain threshold, these search results will show up during search __even if the user doesn't have the app installed!__  


It's a new world of App Search Optimization (ASO)


## Core Spotlight
Core Spotlight allows any stored data within the app to be exposed as search resuls. This means Core Data, SQlite or data pulled from the web can be added to the on-device Core Spotlight index. Apple recommends indexing only pertinent things such as user created content, or items involved in user activity. It's also important that the app keep track of old entries, and remove them from the index to keep results fresh. 

See full guidelines [https://developer.apple.com/library/prerelease/ios/technotes/tn2416/_index.html](https://developer.apple.com/library/prerelease/ios/technotes/tn2416/_index.html)

It's certainly an exciting time to begin thinking about what data or user actions would be enhanced through integration with the new search APIs. As the public search index grows over time, Spotlight has a chance to do for app discovery what Google did for the web.
