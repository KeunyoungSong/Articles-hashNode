---
title: "[Docs Review]Tasks and the back stack"
datePublished: Fri Dec 22 2023 13:13:40 GMT+0000 (Coordinated Universal Time)
cuid: clqgnk8ew000008le1iyzbt2n
slug: docs-reviewtasks-and-the-back-stack

---

Task
- A task is a collection of activities that users interact with when trying to do something in your app. 

BackStack
- `Task` is a stack where the activities are arranged.

# Lifecycle of a task and its back stack
Activities in the stack are **never rearranged**, only pushed onto and popped from the stack as they are started by the current activity and dismissed by the user through the Back button or gesture.

## Root launcher activities and Back tap behavior for it
Root launcher activities are activities that declare an intent filter with both ACTION_MAIN and CATEGORY_LAUNCHER.  
> You can find it in the manifest.xml file  

When a user taps or gestures Back from a root launcher activity, the system handles the event differently depending on the version of Android that the device is running.

### System behavior on Android 11 and lower
- The system finishes the activity.
### System behavior on Android 12 and higher
- The system moves the activity and its task to the background instead of finishing the activity. This behavior matches the default system behavior when navigating out of an app using the Home button or gesture.
> It you want to have custom back navigation, use `Android X Activity APIs` rather then overriding `onBackPressed()`. However, if you want to override `onBackPressed()` then call `super.onBackPressed()` to provides a more consistent navigation experience for users.

## Background and foreground tasks
`A task` is a cohesive unit that can move to the background when a user begins a new task or goes to the Home screen. While in the background, all the activities in the task are stopped, but the back stack for the task remains intact—the task loses focus while another task takes place
> `A task` is a set of activities and is managed separately for each app.

> If the user runs many background tasks at the same time, the system might begin destroying background activities to recover memory. If this happens, the activity states are lost.

## Multiple activity instances
> An activity can be instantiated multiple times, and you can choose whether you want to use an activity as a single instance or not.

## Multi-window environments
When apps run simultaneously in a multi-windowed environment, supported in Android 7.0 (API level 24) and higher, the system manages tasks separately for each window. Each window can have multiple tasks.

## Lifecycle recap
> Basically, an activity goes into the background whenever you initiate a new activity, press the home button, or utilize back taps or gestures, unless the system is experiencing a shortage of memory  


Note: For more about how to design your app's navigation structure for Android, see `Design navigation graphs`.

# Manage tasks
> Theses are the settings you can modified to manage activities within a task
Android manages tasks and the back stack by placing all activities started in succession in the same task, in a last in, first out stack.  

You might decide that you want to interrupt the normal behavior. For example:  
- you might want an activity in your app to begin a new task when it is started, instead of being placed within the current task.
- when you start an activity, you might want to bring forward an existing instance of it, instead of creating a new instance on top of the back stack.
- you might want your back stack to be cleared of all activities except for the root activity when the user leaves the task.

These are the principal <activity> attributes that you can use to manage tasks:  
- taskAffinity: Specifies a unique identifier for the task of the activity.
- launchMode: Defines how the activity should behave when it is started.
- allowTaskReparenting: Indicates whether the activity can move to a different task.
- clearTaskOnLaunch: Determines whether the activity should clear all existing activities in the task when it is launched.
- alwaysRetainTaskState: Specifies whether to retain the state of the task even when the user leaves it.
- finishOnTaskLaunch: Indicates whether the previous instance should be finished when the activity is relaunched in the same task.  

And these are the principal intent flags:
- FLAG_ACTIVITY_NEW_TASK: Starts the activity in a new task.
- FLAG_ACTIVITY_CLEAR_TOP: Removes all activities above the target activity in the current task and brings the target activity to the top.
- FLAG_ACTIVITY_SINGLE_TOP: Reuses the current instance of the activity if it is already at the top of the stack.

## Define launch modes
- Using the manifest file  

When you declare an activity in your manifest file, you can specify how the activity associates with tasks when it starts.  

- Using intent flags  

When you call startActivity(), you can include a flag in the Intent that declares how (or whether) the new activity associates with the current task.

> If both activities define how Activity B associates with a task, then Activity A's request, as defined in the intent, is honored over Activity B's request, as defined in its manifest.

<div/>
> Note: Some launch modes available for the manifest file aren't available as flags for an intent. Likewise, some launch modes available as flags for an intent can't be defined in the manifest.

## Define launch modes using the manifest file
When declaring an activity in your manifest file, you can specify how the activity associates with a task using the <activity> element's launchMode attribute.  

There are five launch modes you can assign to the launchMode attribute:
1. "standard"  
The default mode. The system creates a new instance of the activity in the task it was started from and routes the intent to it. The activity can be instantiated multiple times, each instance can belong to different tasks, and **one task can have multiple instances.**
> In the case where the launch mode of a designated activity is set to "standard," it follows that with each receiving intent, the existing instance of the activity is removed from the back stack. Instead, a new instance is created in its place to handle the incoming intent.

2. "singleTop"  
If an instance of the activity already exists at the top of the current task, the system routes the intent to that instance through a call to its onNewIntent() method, rather than creating a new instance of the activity. The activity is instantiated multiple times, each instance can belong to different tasks, and one task can have multiple instances **(but only if the activity at the top of the back stack is not an existing instance of the activity).**
3. "singleTask"  
The system creates the activity at the root of a new task or locates the activity on an existing task with the same affinity. If an instance of the activity already exists, the system routes the intent to the existing instance through a call to its onNewIntent() method, rather than creating a new instance. **Meanwhile all of the other activities on top of it are destroyed.**
> An app can have multiple tasks, if it's necessary
4. "singleInstance"  
The behavior is the same as for "singleTask", except that the system doesn't launch any other activities into the task holding the instance. The activity is always the single and only member of its task. Any activities started by this one open in a separate task.
5. "singleInstancePerTask"  
The activity can only run as the root activity of the task, the first activity that created the task, and therefore there can only be one instance of this activity in a task. In contrast to the singleTask launch mode, this activity can be started in multiple instances in different tasks if the FLAG_ACTIVITY_MULTIPLE_TASK or FLAG_ACTIVITY_NEW_DOCUMENT flag is set.

> Note: "singleTask" and "singleInstancePerTask" remove all activities that are above the starting activity from the task. For example, suppose a task consists of root activity A with activities B and C. The task is A-B-C, with C on top. An intent arrives for an activity of type A. If A's launch mode is "singleTask" or "singleInstancePerTask", the existing instance of A receives the intent through onNewIntent(). B and C are finished, and the task is now A.

<div/>
> **If the activity is already part of a background task with its own back stack, then that entire back stack also comes forward, on top of the current task.**

## Define launch modes using Intent flags
When starting an activity, you can modify the default association of an activity to its task by including flags in the intent that you deliver to startActivity(). The flags you can use to modify the default behavior are the following:

- FLAG_ACTIVITY_NEW_TASK  

The system starts the activity in a new task. If a task is already running for the activity being started, that task is brought to the foreground with its last state restored, and the activity receives the new intent in onNewIntent().  
This produces the same behavior as the "singleTask" launchMode value discussed in the preceding section.

- FLAG_ACTIVITY_SINGLE_TOP

If the activity being started is the current activity, at the top of the back stack, then the existing instance receives a call to onNewIntent() instead of creating a new instance of the activity.

This produces the same behavior as the "singleTop" launchMode value discussed in the preceding section.

- FLAG_ACTIVITY_CLEAR_TOP

If the activity being started is already running in the current task, then—instead of launching a new instance of that activity—the system destroys all the other activities on top of it. The intent is delivered to the resumed instance of the activity, now on top, through onNewIntent().

There is no value for the launchMode attribute that produces this behavior.

FLAG_ACTIVITY_CLEAR_TOP is most often used in conjunction with FLAG_ACTIVITY_NEW_TASK. When used together, these flags locate an existing activity in another task and put it in a position where it can respond to the intent.

## Handle affinities
An affinity indicates which task an activity "prefers" to belong to. By default, all the activities from the same app have an affinity for each other: they "prefer" to be in the same task.  
...

## Clear the back stack
If the user leaves a task for a long time, the system clears the task of all activities **except the root activity.** When the user returns to the task, only the root activity is restored. The system behaves this way based on the assumption that after an extended amount of time users have abandoned what they were doing before and are returning to the task to begin something new.

- alwaysRetainTaskState

When this attribute is set to "true" in the root activity of a task, the default behavior just described does not happen. **The task retains all activities in its stack even after a long period.**

- clearTaskOnLaunch

When this attribute is set to "true" in the root activity of a task, **the task is cleared down to the root activity whenever the user leaves the task and returns to it.** In other words, it's the opposite of alwaysRetainTaskState. The user always returns to the task in its initial state, even after leaving the task for only a moment.

- finishOnTaskLaunch

This attribute is like clearTaskOnLaunch, **but it operates on a single activity, not an entire task.** It can also cause any activity to finish except for the root activity. When it's set to "true", the activity remains part of the task only for the current session. **If the user leaves and then returns to the task, it is no longer present.**

## Start a task
You can set up an activity as the entry point for a task by giving it an intent filter with "android.intent.action.MAIN" as the specified action and"android.intent.category.LAUNCHER"

# References
%[https://developer.android.com/guide/components/activities/tasks-and-back-stack]
