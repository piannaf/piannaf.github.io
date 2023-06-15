+++
title = "You Need a Rubric (when evaluating multiplatform solutions)"
date = 2020-04-15
description = """
I got a comment on my Lead Dev NYC 2019 talk, bringing up 5 criteria that leads him to choose React Native. My response became more of an article, so here we are: his comment, and my response
"""
[extra]
canonical_url = "https://dev.to/touchlab/you-need-a-rubric-when-evaluating-multiplatform-solutions-1ii0"
+++

Last week, I spoke to an audience about [evaluating multiplatform solutions](https://youtu.be/6RquJJM1jaE). Spoiler: you need a rubric, and the criteria you pick will be heavily context-dependent.

Coincidentally, I got [a comment on my Lead Dev NYC 2019 talk](https://www.youtube.com/watch?v=sA_JIqqj9js&lc=UgwOcrMX_6ClLHIQIRF4AaABAg) a few days later, bringing up 5 criteria that leads him to choose React Native. 

My response became more of an article, so here we are: his comment, and my response

----

> Kotlin multiplatform is promising, but when comparing stacks, these key factors rarely get mentioned:
> 1) does it allow for a shared end to end testing and continuous integration setup between platforms?
> 2) does it have feature parity by design?
> 3) can code be shared between web and mobile?
> 4) availability of experienced developers?
> 5) shared pull requests between ios, android, and web (and thus converging  knowledge and coding standards)?
> 
> Kotlin MP will likely never check these boxes, which people should be aware of before deciding. E.g. React Native is quite powerful when looking at the overall benefits and teams like shopify seem to agree.

Thanks for raising these questions.

Before anyone asks questions, they need to know if it is relevant to their business. And, when they ask questions, they need to know how important each one is to their business. There are world-class engineers at Nubank, Shopify, and Square and each of them made different code sharing decisions ([Flutter](https://medium.com/building-nubank/https-medium-com-freire-why-nubank-chose-flutter-61b80b568772), [React Native](https://engineering.shopify.com/blogs/engineering/react-native-future-mobile-shopify), and [Kotlin Multiplatform](https://developer.squareup.com/blog/developing-on-ios-and-android/) respectively). And let’s not forget the recent examples of [Airbnb](https://medium.com/airbnb-engineering/react-native-at-airbnb-f95aa460be1c) and [Udacity](https://engineering.udacity.com/react-native-a-retrospective-from-the-mobile-engineering-team-at-udacity-89975d6a8102) abandoning React Native after years of investment in using it. Since every business is different, there is no one-size-fits-all evaluation criteria.

To answer your questions:

1. Depends, because the focus is on sharing business logic, not UI, the shared testing is focused on the business logic. If you are building an SDK without UI concerns, then yes, you can E2E test the product under test. If you are building apps that share business logic with KMP, then no, but you can share common tests. 
2. No, it has optional sharing by design. This allows for greater feature alignment while reducing re-write risk in existing code, and easier, purposeful platform specific integration if the product calls for it. (See [my thoughts related to feature-alignment as a goal](@/blog/2020-01-13-don-t-sacrifice-user-experience-for-a-better-developer-experience/index.md))
3. Yes, [Quizlet is doing exactly that](https://medium.com/tech-quizlet/shared-code-at-quizlet-kotlin-multiplatform-2ee1b57646c). KotlinJS came before Kotlin/Native and they are now both under the Kotlin Multiplatform umbrella.
4. Depends what experience you are looking for. Since React Native has been out longer, there is a larger pool of people with React Native experience. However, React Native is different enough from both React Web and Native Mobile, that both experienced react web devs and native mobile devs have issues with it. And people who have a few years experience with React Native often build within the confines of React Native and libraries available which gives them trouble when the product requires more native platform integration or platform specific look and feel. As for KMP, Kotlin experience is easy to come by and the experience needed to integrate the generated framework into an iOS project is easy to come by as well. The difficulties are in the details which is more about good software development, rather than experience with each technology.
5. Yes, Touchlab and other companies adopting KMP recommend shared PRs and ways to enable that like monorepos, bringing together Android and iOS teams into Mobile teams, building tools like our [Kotlin Xcode plugin](https://github.com/touchlab/xcode-kotlin), and shared coding standards, especially around architecture and API style, to ease the transition to all platforms working together.

I wouldn’t say “likely never check these boxes”. For 1, the open source community is growing and there are already several concepts around sharing UI with KMP as well. For 2, similar answer as 1, you can already [build full react web apps in Kotlin](https://play.kotlinlang.org/hands-on/Building%20Web%20Applications%20with%20React%20and%20Kotlin%20JS/01_Introduction) and the community is looking at ways to share everything for feature parity across all supported platforms.

Again, not everyone’s questions/criteria will be the same and the weight given to each will be different too. The people choosing KMP put more weight on its optionality, native interop, use of Kotlin, and deference to the platform UI and ecosystems. (See [my recent webinar on this subject](https://youtu.be/6RquJJM1jaE))

Lastly, since my talk, React Native has continued down the path of becoming “more native” with the introduction of JSI and Turbo Modules. Though, so far, those are more native in the C++ sense, rather than the Kotlin/Swift sense and that will bring issues related to [what DropBox encountered](https://dropbox.tech/mobile/the-not-so-hidden-cost-of-sharing-code-between-ios-and-android). But again, there’s no one-size-fits-all criteria. 

----

For more of my thoughts on this topic, please watch my 20 minute webinar

{{ youtube(id="6RquJJM1jaE") }}

----

This post originally written for [Touchlab](https://dev.to/touchlab/you-need-a-rubric-when-evaluating-multiplatform-solutions-1ii0)

----