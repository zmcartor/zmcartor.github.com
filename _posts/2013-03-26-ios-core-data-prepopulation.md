---
layout: post
title: "iOS Core Data Prepopulation"
description: "Step by step instructions describing howto prepopulate CoreData and sqlite. "
category: code
tags: [iOS, core data]
---
{% include JB/setup %}

When developing a mobile app, it's necessary to ship the app with some default
data. I recently came across the task of pre-populating an application with a
fair amount of data and found documentation on exactly how to do this with Core
Data lacking. Maybe it's because I'm not a seasoned Core Data expert, but
come on! Searching The Google also turned up many others having a similar hard
time finding clarity on this issue. 

This article presents a method for pre-populating a Core Data application on
IOS.

#### Procedure
We'll walk through the specific steps in detail, but this is the general
approach to Core Data pre-population:

1. Generate a Core Data database with pre-populated data
2. Add this database to the project
3. When app starts for the first time, copy the pre-populated DB into the
   managed store and start application.
4. Profit!

#### Step 1: Generate a Core Data database

A common misconception about Core Data is that 'its just a sqlite database,
gosh! ' Which is true; however sqlite is just one of the ways Core Data can
persist itself. The problem with this conception is the Core Data sqlite
schemea is proprietary - don't even try opening it up with any tools because
the data and column names won't make any sense. Changing anything may render
the DB unusable. What we need to do is somehow populate the DB via the Core
Data interface. This may require writing our own parsing code, or using an app
like [Core Data Editor](http://christian-kienle.de/CoreDataEditor)

In apps I've done, data was first extracted as JSON, manually parsed, and then
saved within Core Data.

#### Step 2: Add the pre-populated database to the project
Add the function we've created to parse and prepopulate Core Data somewhere within the Appdelegate, or
another place to be executed on launch. Next, within the AppDelegate, we need to find the
location of our newly populated sqlite file. Add this code to the
'persistentStoreCoordinator' in AppDelegate :

{% highlight objective-c %}
- (NSPersistentStoreCoordinator *)persistentStoreCoordinator {
    /* ... */    
    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"Foobar.sqlite"];
    NSLog(@"CoreData Location: %@", storeURL);
{% endhighlight %}

Awesome, now we gotcha!
Rename this file to something like 'Prepopulated.sqlite' and add it to project
as a resource.

#### Step 3: Core Data switcheroo

This code taken from Apple example project [Core Data Books](https://developer.apple.com/library/ios/#samplecode/CoreDataBooks/Listings/Classes_CoreDataBooksAppDelegate_m.html#//apple_ref/doc/uid/DTS40008405-Classes_CoreDataBooksAppDelegate_m-DontLinkElementID_8)

Replace the method 'persistentStoreCoordinator' within AppDelegate with this
one. Notice places in the code to fill in for your respective DB names.

{% highlight objective-c %}
- (NSPersistentStoreCoordinator *)persistentStoreCoordinator
{
    if (_persistentStoreCoordinator != nil) {
        return _persistentStoreCoordinator;
    }
 
    NSURL *storeURL = [[self applicationDocumentsDirectory] URLByAppendingPathComponent:@"PREPOPULTED_NAME.sqlite"];
 
    /*
     Set up the store.
     For the sake of illustration, provide a pre-populated default store.
     */
    NSFileManager *fileManager = [NSFileManager defaultManager];
    // If the expected store doesn't exist, copy the default store.
    if (![fileManager fileExistsAtPath:[storeURL path]]) {

        // typically the main store name is 'appName.sqlite'
        NSURL *defaultStoreURL = [[NSBundle mainBundle] URLForResource:@"YOURAPPNAME" withExtension:@"sqlite"];
        if (defaultStoreURL) {
            [fileManager copyItemAtURL:defaultStoreURL toURL:storeURL error:NULL];
        }
    }
 
    NSDictionary *options = [NSDictionary dictionaryWithObjectsAndKeys:[NSNumber numberWithBool:YES], NSMigratePersistentStoresAutomaticallyOption, [NSNumber numberWithBool:YES], NSInferMappingModelAutomaticallyOption, nil];
    _persistentStoreCoordinator = [[NSPersistentStoreCoordinator alloc] initWithManagedObjectModel: [self managedObjectModel]];
 
    NSError *error;
    if (![_persistentStoreCoordinator addPersistentStoreWithType:NSSQLiteStoreType configuration:nil URL:storeURL options:options error:&error]) {
        /*
         Replace this implementation with code to handle the error appropriately.
         
         abort() causes the application to generate a crash log and terminate. You should not use this function in a shipping application, although it may be useful during development. 
         
         Typical reasons for an error here include:
         * The persistent store is not accessible;
         * The schema for the persistent store is incompatible with current managed object model.
         Check the error message to determine what the actual problem was.
         
         
         If the persistent store is not accessible, there is typically something wrong with the file path. Often, a file URL is pointing into the application's resources directory instead of a writeable directory.
         
         If you encounter schema incompatibility errors during development, you can reduce their frequency by:
         * Simply deleting the existing store:
         [[NSFileManager defaultManager] removeItemAtURL:storeURL error:nil]
         
         * Performing automatic lightweight migration by passing the following dictionary as the options parameter:
         @{NSMigratePersistentStoresAutomaticallyOption:@YES, NSInferMappingModelAutomaticallyOption:@YES}
         
         Lightweight migration will only work for a limited set of schema changes; consult "Core Data Model Versioning and Data Migration Programming Guide" for details.
         
         */
        NSLog(@"Unresolved error %@, %@", error, [error userInfo]);
        abort();
    }
 
    return _persistentStoreCoordinator;
}
{% endhighlight %}

#### Step 4: Profit

That's it! Hopefully this approch worked out and your app can now ship with
pre-populated data.
