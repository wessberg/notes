# Android Basics

> [Android Fundamentals](https://developer.android.com/guide/components/fundamentals.html) (Only Introduction, App Components, Activities and Activating Components)
>
> [Intro to Activities](https://developer.android.com/guide/components/activities/intro-activities.html)

## Application Fundamentals

Android apps are written in Java.

### Android SDK

The Android SDK compiles your code along with any data and resource files into an APK (An *Android package*), which is an archive file with an `.apk` suffix.

### An `.apk`

An `.apk` is a file that contains all the contents of an Adroid app and is the file that Android-powered devices use to install an app.

## Security

Here's how Android handles Security:

### Sandbox

Each Android app lives in its own security sandbox, protected by these Android security features:

- The Android OS is a multi-user Linux system in which **each app is a different user**.
- Each app has a unique Linux user ID. Only the user ID assigned to the app can access the files within the app.
- Each process has its own VM, so an app's code runs in isolation from other apps.
- By default, every app runs in its own Linux process. The Android system starts the process when any of the app's components needs to be executed, and then shuts down the process when it's no longer needed or when the system must recover memory for other apps.

#### The Principle of least privilege

By default, each app has access only to the components that it requires to do its work and no more. This is the *least privilege* principle. There *are* ways for an app to share data with other apps and for an app to access system services:

#### Sharing data with other apps

Two apps can share the same Linux user ID, in which case they are able to access each other's files. To conserve system resources, apps with the same user ID can also arrange to run in the same Linux process and share the same VM. **The apps must be signed with the same certificate**.

An app can request permission to access device data such as the user's contacts, SMS messages, the mountable storage, camera and Bluetooth. The user has to **explicitly grant these permissions**.

## App Components

App components are the essential building blocks of an Android app. Each component is an entry point through which the system or a user can enter your app. Some components depend on others.

There are four different types of app components:

- Activities
- Services
- Broadcast receivers
- Content providers

Each has a distinct purpose and has a distinct lifecycle that defines how the component is created and destroyed.

### Activities

**Activies are like Views**!

**An Activity is the entry point for interacting with the user**.
It represents a single screen with a user interface.

An email app might have one activity that shows a list of new emails, another activity to compose an email and another for reading emails.

Each activity is independent of the others. **A different app can start *any* of these activities if the email app allows it**.

To create an activity you make it as a subclass of the activity class.

## Services

A service is a component that runs in the background. It does not provide a user interface.
An activity can spawn a service. A service is implemented as a subclass of `service`.

## Activating components

To activate an activity or service you use asynchronous messages called `intents`.

### Intents

Intents bind individual components to each other at run time. They request actions from other components.

#### Explicit vs. implicit intents

Explicit intents are intents which define messages to activate specific components whereas implicit intents activate specific types of components.

#### Payloads on intents

An `intent` can carry a payload such as a URI or other simple data that might help the receiver know the intention of the user. Hence the name.

An intent can also return a result to the invoker.

### Ways to activate components

1. Passing an `intent` to `startActivity()` or `startActivityForResult()`.
2. Starting an activity directly.
3. Using the `JobScheduler` to schedule actions.

## Deepdive into Activities

### Concept

All interaction between an app and the user takes place through activities.
An `Activity` provides the window in which the app draws its UI.

To implement an Activity, you declare a subclass of the `Activity` class.

#### Main Activity

The *Main activity* of an app is the first screen to appear when the user launches the app.
Each activity can then start another activity in order to perform different actions.

#### Registering activities

To use activities, you must register information about them **in the app's manifest**.
Also, you must manage activity lifecycles appropriately.

## Declaring activities in the manifest

Here's how to add an activity:

```xml
<manifest ...>
	<application ...>
		<activity android:name=".ExampleActivity" />
		...
	</application>
</manifest>
```

As you can see, you simply add an `<activity>` element as a child of the `<application>` element.

You give the Activity a name with the `android:name` attribute, which specifies the class name of the activity.

## Intent filters

Intent filters allows the system to launch an activity based not only on an explicit request, but also an implicit one. An implicit request could be *"Start a Send Email screen in any activity that can do the job"*. When the system UI asks a user which app to use in performing a task that's an intent filter at work.

### Declaring intent filters

To declare an Intent filter, use the `<intent-filter>` element inside an `<activity>` element:

```xml
<activity android:name=".ExampleActivity">
	<intent-filter>
		<action android:name="android.intent.action.SEND" />
		<category android:name="android.intent.category.DEFAULT" />
		<data android:mimeType="text/plain" />
	</intent-filter>
</activity>
```

The `<category>` and/or `<data>` elements are optional. These combine to specify the *type* of intent to which your activity can respond.

**Activities that you don't want to make available to other applications should have no intent filters!** You can then start them yourself with explicit intents.

### Declaring permissions

You can constrain which apps can start a particular activity to only apps with a specific activity by adding a `<uses-permission>` element inside the manifest and a `android:permission` attribute on the activity:

```xml
<manifest>
	<uses-permission android:name="com.google.socialapp.permission.SHARE_POST" />
	<activity
		android:name="..."
		android:permission="com.google.socialapp.permission.SHARE_POST"
	/>
</manifest>
```

### The Activity lifecycle

An Activity goes through a number of states throughout its lifetime:

#### `onCreate()`

This **must** be implemented. It fires when the system creates your activity. It should initialize the essential components of your activity. It should create views and bind data to lists here.

**Most importantly, this is where you must call `setContentView()` to define the layout for the activity's user interface**.

#### `onStart()`

This is the callback after `onCreate()`. This activity enters the `Started` state and the activity becomes visible to the user. This callback contains what amounts to the activity's final preparations for coming to the foreground and becoming interactive.

#### `onResume()`

This callback is invoked **just before** the activity starts with interacting with the user. At this point, the activity is at the top of the activity stack, and captures all user input.

#### `onPause()`

This callback is called when the activity loses focus and enters `Paused` state. It occurs when, for example, the user taps the Back or Overlay button.

#### `onStop()`

When this is called, the activity is no longer visible to the user. This may happen because the activity is being destroyed, a new activity is starting or an existing activity is entering a Resumed state and is covering the stopped activity.

#### `onRestart()`

This callback is invoked when an Activity in the `Stopped` state is about to restart. It restores the state of the activity from the time that it was stopped.

#### `onDestroy()`

This is invoked before an activity is destroyed.

It is the final callback that the activity receives. It is usually implemented to ensure that all of an activity's resources are release when the activity, or the process containing it, is destroyed.