+++
title = "Zola in Codespaces"
date = 2023-06-14
description = """
My personal computer is actually a MacBook Air from mid 2012. Not the best for programming these days. I installed and ran Zola locally first to try it out and it worked just fine. So I could just edit my posts in Vim, preview locally, and push to prod, but: one, I don’t want this to be a dev machine; two, where’s the fun in that?
"""
[extra]
+++

## Choosing Zola and Codespaces

My personal computer is actually a MacBook Air from mid 2012. Not the best for programming these days. It can’t even process 1080p H264 videos fast enough to stream over Plex. There’s also the issue of disk space. At 64G, it’s pretty easy to run out of space after downloading dependencies, tools, all the personal apps and files, oh and MacOS itself (just over 10G at the time of writing). I’d rather just focus this old hardware on browsing the web. Luckily, [GitHub Codespaces](https://github.com/features/codespaces) turns software development into “browsing the web”, at least as far as my computer is concerned.

As I was setting up this site, I knew I wanted it to be static and host it on [GitHub Pages](https://pages.github.com/). There are approximately 200 billion trillion stars in the universe, which coincidentally, is just about how many static site generators there are. I’ve played around with Hugo, Jekyll, 11ty, Next.js, and Svelte but never stuck with it. For this go around, I wanted to try one written in Rust. I’m less than 100 pages into [The Rust Programming Language book](https://doc.rust-lang.org/book/), but I’ve heard so many great things from the community that I just wanted to dive in. There are [approximately 3 static site generators written in Rust](https://jamstack.org/generators/). I picked [Zola](https://www.getzola.org/) because it has a large community, is actively maintained, and more feature-full (not that I need a lot of features).

I installed and ran Zola locally first to try it out and it worked just fine. So I could just edit my posts in Vim, preview locally, and push to prod, but: one, I don’t want this to be a dev machine; two, where’s the fun in that?

## This issue with Codespaces

The first issue I ran into is I couldn’t find a Codespaces template with Zola already installed. That’s fine. There’s a [Codespaces Rust Starter](https://github.com/codespaces-examples/rust) written by a few amazing people. I opened it in a Codespace, installed Zola, the port forwarding worked flawlessly, and I could see the initial page running. Now, when I say “port forwarding worked flawlessly”, I mean that in the way developers say something works but users beg to differ. Technically, the port for serving the page and the port for serving [LiveReload](https://github.com/livereload/livereload-js) over websockets were working. But, LiveReload was erroring because it couldn’t actually talk back to the server. The issue being that Codespaces forwards ports over HTTP and gives each its own address.

The simplest way to use livereload is to [host it from the livereload server](https://github.com/livereload/livereload-js/blob/c85ac0db2d46584eb488cad7f46a541a37f43602/README.md?plain=1#L25), that way the livereload library will just connect to the host and port which is serving it. When Zola injects the livereload script tag into the page, [it specifies the port it used to start the websocket server](https://github.com/getzola/zola/blob/93d61cdf7233c5839220ee5948528337107a0653/components/site/src/lib.rs#L545) but the host will be inferred by the livereload library as the current page’s address. That’s a problem for Codespaces because each port gets its own address. Luckily, [livereload lets you specify a host in the query string](https://github.com/livereload/livereload-js/blob/master/README.md?plain=1#L117), too. I found [an issue comment in the Zola repo](https://github.com/getzola/zola/issues/1560#issuecomment-885867717) to corroborate that specifying the right host over port 80 or 443 should do the trick. But what’s the right host? [GitHub Codespaces environments export environment variables](https://docs.github.com/en/codespaces/developing-in-codespaces/default-environment-variables-for-your-codespace) to figure that out.

So with all this info, if Zola is running in a Codespace, it should change the injected script tag from

```rust
format!(r#"<script src="/livereload.js?port={}&amp;mindelay=10"></script>"#, port,);
```

to

```rust
let codespace_name = std::env::var("CODESPACE_NAME").unwrap();
let codespace_port_domain = std::env::var("GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN").unwrap();
let host = format!("{}-{}.{}", codespace_name, port, codespace_port_domain);
format!(r#"<script src="/livereload.js?host={}&amp;port={}&amp;mindelay=10"></script>"#, host, 443,);
```

Note the port Zola uses to run the livereload server becomes part of the host variable, and the port passed to the script is 443. Without specifying a port for the script, [it will default to 35729](https://github.com/livereload/livereload-js/blob/0300e8e34ec573e160aeb5b352510be03d81f797/src/options.js#L5).

## So now it works? Nope. But there’s a workaround

Sometimes it seemed to work, but sometimes not. I tried refreshing in different ways, using different browsers, different combinations of incognito and not. A fun side quest was to [read all about websockets](https://web.archive.org/web/20220429053637/http://enterprisewebbook.com/ch8_websockets.html), and [how to debug them](https://mtyiu.site/tutorials/9/ws-debug/), which led me to realize that there was nothing wrong with how Zola was serving things from Codespaces or how the script was getting called/addressed from the client.

The problem was GitHub authentication. Not that authentication is a problem. But when you want live reload to “just work” and it doesn’t, it sure feels like a problem.

Of course, [GitHub’s got your back](https://docs.github.com/en/codespaces/developing-in-codespaces/forwarding-ports-in-your-codespace#using-command-line-tools-and-rest-clients-to-access-ports-1)

> to connect to a private port at the remote domain, you must authenticate by using the GITHUB_TOKEN access token in your request.
> 

Except, if you clicked on that link, you’d see it’s in the section “Using command-line tools and REST clients to access ports”.

This can’t be used by livereload because it [creates a web socket URI](https://github.com/livereload/livereload-js/blob/af658470433d1d0cdc1a3ad272fc911efced967f/src/connector.js#L13) from the host parameter then [passes that along to the browser’s WebSocket constructor](https://github.com/livereload/livereload-js/blob/af658470433d1d0cdc1a3ad272fc911efced967f/src/connector.js#L78). After learning about WebSockets, I now know that you can’t alter the headers of a websocket — it isn’t an option in [the constructor or instance methods](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket). The host address trick isn’t going to work so simply. Maybe a modified version of livereload.js could open a request to the host over http first with the GITHUB_TOKEN in the headers before trying to open the websocket connection? That would also require modifying Zola to pass along that information... no, too much work.

So, two workarounds:

1. Make the port public
2. Open manually first

Since the Codespace URL isn’t so easy to guess, maybe [making the port public](https://docs.github.com/en/codespaces/developing-in-codespaces/forwarding-ports-in-your-codespace#sharing-a-port-1) isn’t so bad. Unfortunately, there’s no way to use the [devcontainer.json](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers#devcontainerjson) to configure that the port be made public every time the Codespace is run. Codespaces are able to [load dotfiles from a dotfiles repository](https://docs.github.com/en/codespaces/customizing-your-codespace/personalizing-github-codespaces-for-your-account#dotfiles), so some series of incantations eventually leading to [a GitHub CLI command altering the port visibility](https://cli.github.com/manual/gh_codespace_ports_visibility) might work. But that’s a global thing for every Codespace and, also, too much work.

So that means, opening the livereload address in the browser manually is the workaround for me. I didn’t actually know if this would work. Opening the livereload address in a new tab automatically authenticates via github, since I am already authenticated, but it gives an error because there’s no webserver running on that “port”. However, once authenticated, it allows the livereload client on the actual web “port” to open the websocket over the livereload “port”. And that’s all I ever really wanted.