+++
title = "The Future of Cross-Platform is Native"
date = 2019-08-22
description = """
The quest for the perfect cross-platform solution has been like the quest for the Holy Grail. It’s been going on a long time, there are a myriad of perceived benefits, and every time someone claims to have found it, it’s never the right one.
"""
[extra]
canonical_url = "https://touchlab.co/future-cross-platform-native/"
+++

# Cross-Platform Baggage

“Cross-platform” as a term and a development tool is not held in high regard these days, largely because the apps that purport to provide cross-platform support have never really done the job effectively.

Touchlab go deeper into why “cross-platform” as a term is problematic in the post, “[Cross-Platform? We Don’t Say That Around Here Anymore](https://touchlab.co/we-dont-say-cross-platform-anymore/).”

With that said, I believe the time has come for us to reconsider. The arguments in favor of cross-platform development are the same as they’ve always been (D.R.Y., Risk Mitigation and Feature Parity).

# Don’t. Repeat. Yourself (D.R.Y.)

There’s the argument for gaining efficiencies and cost savings through streamlining the development process. Program once to create cohesive code that can be deployed simultaneously to iOS and Android.

# Risk Mitigation

There’s also the argument for minimizing risk regarding how UI will be developed. The biggest risk is that the UI won’t meet user expectations on either platform. This is a major reason why development teams opt for programming apps independently from one another. At the same time, business logic and backend development are at higher exposure because both determine how all features within an app will work. Ideally, in a cross-platform development scenario, an organization could take the time to focus on the nuances of backend and logic development, while putting less strain on UI development.

# Feature Parity

The other argument is for feature parity and inclusivity, that is, fewer differences in functionality, whether iOS or Android. The benefit — you treat all users equally because they are using essentially the same program, whichever platform they choose.

# Native Multiplatform Development

However, what was missing from “cross-platform” is native multiplatform development: Native CPU, Native UX, and Native developer experience and tools for iOS and Android. **Cross-platform programming has the potential to thrive if the focus shifts to native coding** , which is a more direct approach to produce the same functionality across platforms and devices. To better understand this, here’s a quick look at the most popular cross-platforms solutions and their native limitations.

## Xamarin

Xamarin was one of the first to focus on a native approach to programming across iOS, Android, and Windows, starting with shared business logic and working its way toward shared UI with Xamarin Forms. However, its native elements are limited because it lives within its own ecosystem and uses C# (a language not native to iOS or Android development) and Microsoft Visual Studio instead of Android Studio or Xcode.

![](https://cdn-images-1.medium.com/max/782/0*yil38-6AVKVzgacs)

## React Native

React Native (RN) represented a leap forward in terms of how developers thought about cross-platform because it empowered them to apply their web development knowledge to build native iOS and Android apps. But, like Xamarin, it too, lives within its own ecosystem, using Javascript and non-standard editors . And like Xamarin, it needs to wrap native controls and view hierarchy from its own interop, making it necessary to construct the UI with its own language.

![](https://cdn-images-1.medium.com/max/957/1*DR542WwsH27ESJ5TztZvnQ.png)

## Flutter

Flutter, a newer addition to cross-platform programming, uses its language Dart to create iOS and Android apps. Flutter also makes use of rich widgets to provide remarkable native experiences on Android and iOS platform — but the widgets are not native. Flutter also employs a shared UI platform that only works on mobile with a language (Dart) that isn’t widely used.

![](https://cdn-images-1.medium.com/max/1024/1*yNIuTaM8FHitgI48bQFrSg.png)

These are just a few examples of why cross-platform programming has traditionally been a serious challenge to manage. It’s one of the reasons I believe we should move away from the term “cross-platform.” A more apt term is “multiplatform”, because the goal is that any code you share maximizes what each platform offers.

# Kotlin Multiplatform

That brings us to Kotlin Multiplatform. It is the rising star in the multiplatform space, and is, in fact, more native than Xamarin, RN or Flutter. Currently the dominant Android language, Kotlin has a strong, enthusiastic base of developers worldwide, and is praised by the community for providing a superior developer experience. Kotlin Multiplatform enables developers to write once, and test once, then use the same code across iOS, Android, and Web apps.

While not the first multiplatform tool to split business logic and UI (for instance, Xamarin for logic/libraries and Xamarin Forms for UI), it’s more native than Xamarin, RN, and Flutter because it uses shared logic and libraries below the UI layer, which developers can interact with in the native developer environments — Xcode, Swift, and Objective-C for iOS; Android Studio and Kotlin for Android; JavaScript for the web — and it outputs native code for each platforms.

As a language, Kotlin enables developers to produce applications more cohesively. It’s a modern language that dovetails with native platforms on Android, iOS, Java, and the web, allowing development teams to build on what’s already been coded. And because it’s essentially an extension of Java, it’s relatively easy for Java developers to get started. Kotlin isn’t too much of a departure from Swift either and at Touchlab we currently have iOS developers coding in Kotlin.

![](https://cdn-images-1.medium.com/max/920/1*hPon7c6yMC-1qpFvKYBtHQ.png)

Equally important, Kotlin is a way to future-proof applications developed today. It’s a sound technology investment because the code works on all platforms without vendor lock-in like Xamarin or React Native. So, whether the dominant platform in the future is web or mobile doesn’t really matter, because the code ports to either environment.

At the same time, development teams no longer have to be siloed, and instead can work together as a cohesive whole. There is no longer a need to have dedicated iOS and Android teams. This unified mobile approach simply makes more sense operationally, technically, culturally, and financially.

[Is Kotlin the safest bet](http://touchlab.co/future-shared-code-kotlin-multiplatform/)? Everything indicates that it, in fact, is. In a world where we’re always looking for greater efficiencies in development time and costs, [Kotlin has proven itself to be a very viable player](http://touchlab.co/the-case-for-kotlin/) — one that has already shown itself to be a truly multiplatform programming option.

----

This post originally written while working at [Touchlab](https://touchlab.co)

----