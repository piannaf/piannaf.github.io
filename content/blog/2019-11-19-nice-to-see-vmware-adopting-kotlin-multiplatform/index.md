+++
title = "Nice to see VMWare adopting Kotlin Multiplatform"
date = 2019-11-19
description = """
There are more and more (respected and influential) companies experimenting with and talking about Kotlin Multiplatform. Some have put it into production already.
"""
[extra]
canonical_url = "https://touchlab.co/nice-to-see-vmware-adopting-kotlin-multiplatform/"
+++

I saw Hadi's tweet and I couldn't agree more.

![Hadi's tweet saying "Nice to see VMWare adopting Kotlin Multiplatform"](image.png)

There are more and more (respected and influential) companies, like [VMWare](https://medium.com/vmware-end-user-computing/adopting-a-cross-platform-strategy-for-mobile-apps-59495ffa23b0) experimenting with and talking about Kotlin Multiplatform. Some have put it into production already.

## Personal Thoughts

Here are a few quotes from VMWare's post and my thoughts:

### Experimental Technology

> Kotlin Multiplatform is still an experimental technology. It’s very early on in its days, and is subject to breaking changes going forward.

There are a lot of people who are afraid of this, but we should remember that Swift was also in that state until version 4, and version 5 was the first to introduce binary stability so binary frameworks would work with future releases of Swift. That didn't stop people from learning it, experimenting with it, and putting it into production. Because they trusted Apple.

JetBrains has a long history of building developer trust. The Kotlin Multiplatform early adopters trust JetBrains. And, it is again, well deserved. Android Studio is built on IntelliJ and Google made Kotlin the official language of Android starting in 2017. You and VMWare aren't risking much to experiment with Kotlin Multiplatform now.

### Library Support

> One issue you will most likely encounter with Kotlin Multiplatform, is that the set of libraries that support it are very small at this point

This is true. And that's why [Touchlab is working with Square](https://touchlab.co/touchlab-square-collaborating-on-kotlin-multiplatform/) and developing the [xcode-kotlin plugin](https://github.com/touchlab/xcode-kotlin) and making SQLDelight the recommended multiplatform persistence solution (see @joreilly's [writeup](https://johnoreilly.dev/posts/sqldelight-multiplatform/)) and we want to support and elevate community projects when we can, like [Multiplatform Settings](https://github.com/russhwolf/multiplatform-settings) and more.

And, of course, JetBrains isn't letting us down on this front. The most recent example is ongoing work to support multithreaded coroutines in Kotlin Native. It's one of the biggest missing pieces and [it's getting very close to ready](https://dev.to/touchlab/kotlin-native-coroutines-preview-3edc).

### Leverage Native Android and iOS Libraries

> It wasn’t the most elegant solution, but it was better than writing our own version of libphonenumber by a mile (or several).

The interop story is really where Kotlin Multiplatform shines. Other solutions require bridges, channels, JNI on Android, and/or excessive boilerplate. When you run into the "set of libraries" issue above, you can still use existing, battle-tested libraries from the native ecosystems.

This interop story also allows for incremental refactoring that significantly lowers the risk of adopting an "experimental" technology. I wrote about this in some detail in [Kotlin Multiplatform Can't Do It All. Which is Exactly Why You Should Try It!](@/blog/2019-08-20-kotlin-multiplatform-can-t-do-it-all-which-is-exactly-why-you-should-try-it/index.md)

### More to Come

> Stay on the lookout for follow up posts where I will talk more about additional features, issues, and how to get the most out of Kotlin Multiplatform

I am very much looking forward to more. And I hope you all are too!

----

This post originally written for [Touchlab](https://touchlab.co/nice-to-see-vmware-adopting-kotlin-multiplatform/)

----