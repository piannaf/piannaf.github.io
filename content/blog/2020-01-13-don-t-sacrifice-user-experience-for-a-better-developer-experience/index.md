+++
title = "Don't sacrifice user experience for a better developer experience"
date = 2020-01-13
description = """
Android, Desktop, iOS, Web. Users want the best experience. Developers want to be efficient. Users should come first.
"""
[extra]
canonical_url = "https://dev.to/touchlab/don-t-sacrifice-user-experience-for-a-better-developer-experience-1lci"
+++

As a mobile developer, it is understandable to focus on platform fragmentation and writing UI twice as pain points.

As a web developer, it is understandable to try to leverage your web expertise so that developing for a new platform is less painful.

![Cross-platform is not an excuse to ignore the platform](https://thepracticaldev.s3.amazonaws.com/i/tuujmxx8pv6ehyephse6.png)
<figcaption>Love this slide from Ellen Shapiro's Talk<sup>1</sup></figcaption>

From a user perspective, fragmentation is what makes different platforms unique. Games can get away with having the same UI across all platforms because users expect games to be their own world, not confined to platform conventions. And, although platform UI conventions have seen convergence (e.g. material design includes guidance on Bottom navigation now), they are still different. When non-game apps don’t conform, they are perceived as a Linux app running on Windows, or an iOS app running on Android, or somewhere in the uncanny valley.

Apps shouldn’t conform 100% because they have different branding, use cases, flows, audiences, etc. They require deliberate design both for what elements are shared and what diverge. That is where native UIs become an asset. Yes, there is more for developers to think about and to deal with. Yes, it’s a pain for developers to keep things updated and meet user expectations of different OSs and versions of OSs. But, you can also make the best UI possible for each platform.

[Apple](https://twitter.com/jaredsinclair/status/1136057545804734465?s=09), [Google](https://twitter.com/ricknout/status/1207943716801073152), and [Microsoft](https://github.com/Microsoft/microsoft-ui-xaml) invest a lot in the standard UI tooling, libraries, and widgets so that when you use an app on iOS, Android, MacOS, or Windows, there is a certain consistency of quality and experience, and, when you develop for them, you are using the best tool for the job. 

It’s possible to make a terrible app no matter what tools you use. Flutter and React Native may make it easier to make a *decent* app across platforms, but you need to go native to build an *excellent* app across platforms.

The history of write-once-run-anywhere and shared UI is a volatile one. The history of shared code, below the UI, is a successful one. [Kotlin Multiplatform focuses on shared code below the UI](@/blog/2019-08-20-kotlin-multiplatform-can-t-do-it-all-which-is-exactly-why-you-should-try-it/index.md) and lets you use the platform provided tooling to their fullest extent. That makes it doubly less risky: less risk of making a poor UI and less risk of being locked into a 3rd party (for now, Flutter is at best 2nd party).

KMP is still in the relatively early phases, [@touchlab](https://dev.to/touchlab) is an innovator on the adoption curve and anyone putting KMP into production right now is an early adopter. 2020 may be the start of the early majority phase with [iOS support in Android Studio](https://www.infoworld.com/article/3509457/jetbrains-bringing-ios-device-support-to-android-studio.html) and [multithreaded coroutines in Kotlin Native](https://dev.to/touchlab/kotlin-native-coroutines-preview-3edc).

![just because you can, doesn't mean you should](https://media.giphy.com/media/mCClSS6xbi8us/giphy.gif)
<figcaption>Just because you can share UI, doesn't mean you should</figcaption>

Coming back to the UI topic. There are already attempts at shared UI libraries built on KMP, and there will be more and there will be ones that gain popularity and maturity. The fact that you can already write [UIKit code with Kotlin](https://github.com/JetBrains/kotlin-native/tree/master/samples/uikit) makes that easier. 

But again, be careful, be deliberate, know your users. Just because you can share UI, doesn't mean you should. Android, iOS and Web are very different. But we learned from [Catalyst](https://developer.apple.com/mac-catalyst/) that users even notice the difference between iPad and Mac. The outcry [stopped one prominent developer from releasing a quick port to Mac](https://twitter.com/_chuckyc/status/1190626773181976576) and it became clear that [Apple should have provided better guidance on the UI differences expected](https://twitter.com/stroughtonsmith/status/1183219285256298496).

Lastly, remember that Apple and Google put a lot of money, time, and effort into their platforms. They want people to buy their devices which means good UX (user experrience) and they need developers to build more apps for each platform which means good DX (developer experience). Jetpack Compose is maturing, Swift UI is maturing. Kotlin Multiplatform doesn't stop you from investing in either. Jetpack Compose is already Kotlin First, and SwiftUI+KMP [might just be an ideal developer experience](https://twitter.com/RunChristinaRun/status/1138987880674521088) for making ideal user experiences on iOS, too.

----
<sub>1. [I walk the line](https://speakerdeck.com/designatednerd/native-and-what-maybe-shouldnt-kotlinconf-copenhagen-denmark-december-2019)</sub>

----

This post originally written for [Touchlab](https://dev.to/touchlab/don-t-sacrifice-user-experience-for-a-better-developer-experience-1lci)

----