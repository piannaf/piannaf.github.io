+++
title = "Kotlin Multiplatform Can’t Do It All. Which is Exactly Why You Should Try It!"
date = 2019-08-20
description = """
Other multiplatform solutions aspire to support all application layers, they can’t adequately cover them all. Kotlin Multiplatform does not live within its own ecosystem. Instead, it is very much like a “choose your own adventure” book, which is what makes it so powerful.
"""
[extra]
canonical_url = "https://touchlab.co/kotlin-multiplatform-cant-do-it-all/"
+++

Before I begin, I want to be clear that all tools are created to solve problems, but none of them solve all problems. If your development team is in a hurry, or high-quality UI is not a major priority, go ahead and choose an ‘end-to-end’ development platform like Xamarin or Flutter or React Native to code your app for Android and iOS. You’ll get the job done 🎉!

Now, if you already have UIs for your apps, or if you want optimized UI for each platform, you’ll want to use Kotlin Multiplatform. The reason? It doesn’t do UI at all (not yet, anyway). But what it does do — and really well — is [business logic for Android and iOS apps](https://touchlab.co/future-shared-code-kotlin-multiplatform/).

 

## Though the other multiplatform solutions aspire to support all application layers, they can’t adequately cover them all.

And sharing UI code across platforms is not necessarily desirable anyway. Very often when this is done, there will need to be multiple iterations to make the UI look and behave more natively. This will burn through development cycles, putting more pressure on your dev teams to deliver on time. And normally, the business is pushing for more features over UI quality. Sharing UI is risky, and it’s rarely good for morale, or for business.

 

## Unlike Xamarin, Flutter or React Native, Kotlin Multiplatform does not live within its own ecosystem. Instead, it is very much like a “choose your own adventure” book, which is what makes it so powerful.

While KMP currently has limited libraries (though the number is growing), it does enable you to use all existing libraries and tools on iOS and Android, so there’s no need to wait for libraries or implement hacks and workarounds. You can’t do that with Flutter or React Native without running into sizable obstacles.

The output from Kotlin Multiplatform is just another package on Android and framework on iOS. That can save a significant amount of time and headaches because there is far less time spent writing bridge code or fully re-writing the things that are missing from other solutions. 

To code business logic in Flutter, your team first has to code shared logic in a language that is not widely used – Dart – in a new ecosystem, with a smaller community, and difficulty bridging it with existing code. In React Native, your mobile team would need to immerse themselves in the web ecosystem of JavaScript’s new IDEs and other tools . And in Xamarin, they’d have to code in C# , use Visual Studio, and a smaller, less active community. To make matters worse, whichever of these platforms your team uses, they’ll need a communication bridge between native and non-native code. 

 

## In Kotlin Multiplatform, however, your team can code the platform-specific business logic, with direct communication with the native platform, with no need to wait for libraries or implement hacks or workarounds.

(You can if you want, that’s part of the adventure you choose.) And even if there are any issues, optional sharing with Kotlin Multiplatform means you only need to revert the code directly related to the issue — no ripping out the whole engine because of a bad spark plug. So, you always have choices. 

This is important of course, because it’s the business logic that determines how all features within an app will work. Because you’re writing the native code once for this layer, you accelerate development time and help ensure a solid code base. Plus, writing one set of native code is a highly effective way of future-proofing the code for later releases.

 

## Kotlin Multiplatform, in other words, provides your dev teams with greater flexibility.

The other multiplatform solutions are essentially proprietary, resulting in vendor lock-in. It also results in the need to manage, in effect, a third platform because the ecosystems are too different from the native platforms and, because they try to solve everything but they can’t solve everything, you will need to write more platform specific code than advertised.

Unlike Xamarin and React Native, Kotlin Multiplatform doesn’t require a VM. Flutter doesn’t require a VM in production, but it does put you in a non-native ecosystem writing in a non-native language unlike Kotlin Multiplatform which respects the native languages and ecosystems of each platform. Kotlin Multiplatform is the most native multiplatform solution your team can use today. 


## KMP doesn’t hide the fact that you’re dealing with multiple platforms because it already compiles to a native library for iOS or Android.

There are no intermediate layers to deal with, virtually eliminating any interop bottlenecks. And since Kotlin Multiplatform works with the native platform ecosystems rather than becoming its own, devs can use the tools and libraries they’ve always used including new platform innovations like SwiftUI and Jetpack Compose. Limitations you do encounter are not dead ends because you can always code around them with Kotlin, Swift, or whatever language lets you solve the issue with the least risk.

## Ranking multiple cross-platform development solutions

In sum, here’s how we at [Touchlab](https://touchlab.co) measure the world of multiplatform solutions. Next week, we’ll share more details about the specifics of our rankings. In the meantime, if you’re interested in speaking to us about Kotlin Multiplatform, please [contact us](https://touchlab.co/contact-us/).

![Ranking multiple cross-platform development solutions](https://res.cloudinary.com/practicaldev/image/fetch/s--y-p04eWp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://4d4f6p22cgml1ale382crgth-wpengine.netdna-ssl.com/wp-content/uploads/2019/08/Harvey-Scorecard-%25E2%2580%2593-4%402x-1080x608.png)

----

This post originally written for [Touchlab](https://touchlab.co/kotlin-multiplatform-cant-do-it-all/)

----