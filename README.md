## Requirements

### IDE

[Android Studio][android-studio-intro] should be the default IDE for Android Development. At this time of writing, [Android Studio 3.0 is the latest stable version][android-studio].

To get started, please follow this instruction on [installing Android Studio][android-studio-install].

### JDK/JRE

> **Note**: In this guide, JDK and JRE are used interchangeably to avoid confusion.

Android Studio is bundled with JDK and it's recommend to use it.

<img src="https://i.imgur.com/yEuQ10N.png"/>

> **Note**: Though it's probably configured already. The Android SDK just [recently supports Java 8][java-8-support]. So, use it like below.

<img src="https://i.imgur.com/WEcBHCT.png"/>

### Bundled JDK not working?

If by for whatever reason, the bundled JDK doesn't work, follow the [JRE installation guide](#installing-jdk-8) below.

After you've successfully installed the JRE, open the project on Android Studio, then `File -> Project Structure -> JDK location` then **uncheck** `Use embedded JDK (recommended)` and locate your JDK.

<img src="https://i.imgur.com/wbdcqon.png"/>

### Installing JDK 8

**SKIP THIS** if the JDK that's bundled with Android Studio is working! Go immediately to [Android Studio Setup](#android-studio-setup)

Make sure you have [Java 8][java-8] installed on your machine. To check if you have Java configured correctly. Executing `java -version` on the terminal should typically output something like below.

```no-highlight
$ java -version
java version "1.8.0_152"
Java(TM) SE Runtime Environment (build 1.8.0_152-b16)
Java HotSpot(TM) 64-Bit Server VM (build 25.152-b16, mixed mode)
```

If you can't see the output above. Follow these [instructions on how to install Java][install-java].

## Android Studio Setup

> **Note**: This section assumes that you have working Android Studio installed already. If you haven't done so, please refer to the [IDE](#ide) section above.

For starters, Android Studio Welcome screen looks something like this.

<img src="https://imgur.com/js8OPun.png"/>

### Install _required_ Android SDK Platforms

Click `Configure -> SDK Manager`...

<img src="https://imgur.com/K7hfd9t.png"/>

Below is the window where you can install Android SDKs.

<img src="https://imgur.com/csBEDIf.png"/>

> Make sure you checked the `Show Package Details` before you proceed!

You **don't need** to **install** all Android SDKs to run this project. Below are the recommend Android SDK that's typically needed for this project.

Installing this platforms may take awhile depending on your internet connection. Most likely, the total disk space that will be consumed when installing this platforms will be `~18 GB`.

Now, mark every item specified below as checked then click `Apply`.

#### Android SDK Platforms

* Android API 27
	* Android SDK Platform 27
	* Google APIs Intel x86 Atom System Image
* Android 8.0 (Oreo)
	* Android SDK Platform 26
	* Sources for Android 26
	* Google APIs Intel x86 Atom System Image
	* Google Play Intel x86 Atom System Image
* Android 7.1.1 (Nougat)
	* Android SDK Platform 25
	* Sources for Android 25
* Android 7.0 (Nougat)
	* Android SDK Platform 24
	* Sources for Android 24
	* Google Play Intel x86 Atom System Image
* Android 4.4 (KitKat)
	* Android SDK Platform 19
	* Sources for Android 19
	* Google APIs Intel x86 Atom System Image

### SDK Tools

Please refer to the image below.

<img src="https://imgur.com/5pIWfRc.png"/>

### Android Emulators

At this stage, you should now be able to code, add feature, create tests, etc. But you won't be able to actually test what the app looks like or how the app behaves on a _real_ device unless you have a physical device with API version supported by the app.

To remedy this, the Android Framework team provide a bunch of Android Emulators to test your app. Android Emulator behaves like a _real device_.

> The Android Emulator provides almost all the capabilities of a real Android device. You can simulate incoming phone calls and text messages, specify the location of the device, simulate different network speeds, simulate rotation and other hardware sensors, access the Google Play Store, and much more.

You can learn more about the Android Emulators [here][android-emulator].

To install and create an Android Emulator, go to `Tools -> Android -> AVD Manager` or click the `AVD Manager` icon.

<img src="https://imgur.com/yZq44Jr.png"/>

> Rule of thumb: Choosing what Android Emulator version to install depends on the app's minimum supported SDK version (`minSdkVersion` and `targetSdkVersion` in the `build.gradle` file ) and maximum supported SDK version.

#### Recommended Android Emulators

In our case, we need to install at least two emulators, one with `API 19` and the other with `API 26 or 27` version to test our app with.

> As much as possible, it's still much better to test the app on a real device.

It does not matter what device definition you choose. What matters is the API version and Google Play Support.

Currently, only Nexus 5x device is packaged with Google Play Store. We need the Google Play Store emulator if we want to test our app's feature that needs Google Play Services.

For comprehensive list of API that [Google Play Services offers, check here][google-play-services].

## Build System

Our default option should be [Gradle][gradle] using the [Android Gradle Plugin][android-gradle-plugin].

It is important that our application's build process is defined by Gradle files, rather than being reliant on IDE specific configurations. This allows consistency between tools and better support for continuous integration systems.

## Opening the project for the first time

<img src="https://imgur.com/oSiD4vr.png"/>

Expect that it will take some time to comfortably work on a feature since Gradle is downloading dependencies that our project needs. So, please be patient. :)

Most likely, Gradle is downloading the ff.

* Gradle wrapper (`~1-2GB` file size)
* Libraries (inside the [project-wide `build.gradle`][build-gradle-1] and [app-specific `build.gradle`][build-gradle-2])

## Project structure

Although Gradle offers a large degree of flexibility in your project structure, unless you have a compelling reason to do otherwise, you should accept its [default structure][gradle-sourcesets] as this simplify your build scripts.

## Package by features

We recommend using a *feature based* package structure for your code. This has the following benefits:

* Clearer feature dependency and interface boundaries.
* Promotes encapsulation.
* Easier to understand the components that define the feature.
* Reduces risk of unknowingly modifying unrelated or shared code.
* Simpler navigation: most related classes will be in the one package.
* Easier to remove a feature.
* Simplifies the transition to module based build structure (better build times and Instant Apps support)

The alternative approach of defining your packages by how a feature is built (by placing related Activities, Fragments, Adapters etc in separate packages) can lead to a fragmented code base with less implementation flexibility. Most importantly, it hinders your ability to comprehend your code base in terms of its primary role: to provide features for your app.

## Avoid Fragments

We recommend that you avoid Fragments at all cost due to its unpredictable nature, its complicated lifecycle,
and weird interactions with other Android components. Which leads to bugs that are hard to debug and resolve.

**BUT**, if you're successful using Fragments and knows its ins and outs, then by all means, use it. Just make sure that your Fragments is very _simple_ and _easy_ to manage.

### Use Conductor

[Conductor][conductor] is a small, yet full-featured framework that allows building View-based Android applications. Its [lifecycle][conductor-lifecycle] is very simple and works well with other Android components. It's actively maintained and it's open-source!

There is another library that behaves the _same_ called [Flow][flow] but haven't explored its capabilities yet but I think it's something that we should be aware of.

You can read more [here][eww-fragment-1], [here][eww-fragment-2], and [here][eww-fragment-3] why.

#### Below is the lifecycle for Fragments and Conductor.

##### Fragment
<img src="https://i.imgur.com/5ZMVs1e.png"/>

##### Conductor
<img src="https://i.imgur.com/mcN0Qlu.jpg"/>


### Using Fragments

Because of Android API's history, you can loosely consider Fragments as UI pieces of a screen. In other words, Fragments are normally related to UI. Activities can be loosely considered to be controllers, they are especially important for their lifecycle and for managing state. However, you are likely to see variation in these roles: activities might take UI roles (delivering transitions between screens), and [fragments might be used solely as controllers][fragment-as-controllers]. We suggest you sail carefully, making informed decisions since there are drawbacks for choosing a fragments-only architecture, or activities-only, or views-only. Here is some advice on what to be careful with, but take them with a grain of salt:

* Avoid using [nested fragments][nested-fragments] extensively, because [matryoshka bugs][matryoshka-bugs] can occur. Use nested fragments only when it makes sense (for instance, fragments in a horizontally-sliding ViewPager inside a screen-like fragment) or if it's a well-informed decision.

* Avoid putting too much code in Activities. Whenever possible, keep them as lightweight containers, existing in your application primarily for the lifecycle and other important Android-interfacing APIs. Prefer single-fragment activities instead of plain activities - put UI code into the activity's fragment. This makes it reusable in case you need to change it to reside in a tabbed layout, or in a multi-fragment tablet screen. Avoid having an activity without a corresponding fragment, unless you are making an informed decision.

## Contributing

As a contributor to this repository, we expect that you already read the guidelines that
the Android team is following. We recommend that you read the [guidelines][contributing] if you haven't done so.

## Resources

Below the resources that this guide is inspired from.

* https://github.com/futurice/android-best-practices
* https://github.com/ribot/android-guidelines

[contributing]: https://gitlab.com/channelfix/android-app/blob/develop/CONTRIBUTING.md
[android-studio-intro]: https://developer.android.com/studio/intro/index.html
[android-studio]: https://developer.android.com/studio/index.html
[android-studio-install]: https://developer.android.com/studio/install.html
[java-8-support]: https://developer.android.com/studio/write/java8-support.html
[gradle]: https://gradle.org/
[java-8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[install-java]: https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html
[android-emulator]: https://developer.android.com/studio/run/emulator.html
[google-play-services]: https://developers.google.com/android/
[android-gradle-plugin]: https://developer.android.com/studio/build/index.html
[gradle-sourcesets]: https://developer.android.com/studio/build/index.html#sourcesets
[build-gradle-1]: https://gitlab.com/channelfix/android-app/blob/develop/build.gradle
[build-gradle-2]: https://gitlab.com/channelfix/android-app/blob/develop/app/build.gradle
[conductor]: https://github.com/bluelinelabs/Conductor/
[flow]: https://github.com/square/flow
[flow-verdict]: https://www.reddit.com/r/androiddev/comments/4em04y/whats_the_verdict_on_conductor_why_use_it_and_why/d21e2ry/?st=ja0ldzkr&sh=2fe3003e
[fragment-lifecycle]: https://i.imgur.com/5ZMVs1e.png
[conductor-lifecycle]: https://i.imgur.com/mcN0Qlu.jpg
[fragment-as-controllers]: https://developer.android.com/guide/components/fragments.html#AddingWithoutUI
[nested-fragments]: https://developer.android.com/about/versions/android-4.2.html#NestedFragments
[matryoshka-bugs]: http://delyan.me/android-s-matryoshka-problem/
[eww-fragment-1]: https://medium.com/square-corner-blog/advocating-against-android-fragments-81fd0b462c97
[eww-fragment-2]: https://redd.it/2iodnx
[eww-fragment-3]: https://academy.realm.io/posts/michael-yotive-state-of-fragments-2017/
