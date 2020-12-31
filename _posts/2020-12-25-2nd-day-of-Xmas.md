---
layout: "posts"
title: "Second Day of Xmas"
date: "2020-12-25 23:59"
---

_Post series - [part one](2020-12-24-1st-day-of-Xmas.md), **part two**, [part three](2020-12-26-3rd-day-of-Xmas.md), [part four](2020-12-27-4th-day-of-Xmas.md)_

On the second day of Xmas my nerdy love led to me ... writing [https://github.com/Cervator/KubicTerasology](https://github.com/Cervator/KubicTerasology) - unsurprisingly a Kubernetes hosting setup for Terasology now!

After the first blog entry on [KubicArk](https://github.com/Cervator/KubicArk) this 2nd one is probably fairly obvious, and there isn't _that_ much to share which isn't already covered. But still, not every blog entry has to be a *huge* wall of text, some can be small'ish walls of text!


## Objectives here

Much like with KubicARK the fundamental purpose here simply is being able to easily spin up a Terasology game server in Kubernetes, even managing several with different configuration just as easily.

For the Terasology community setting up servers more easily and quickly would help in a variety of ways, from maintaining official long-lived servers, spinning up a play test server real quick, or even running an ad-hoc test using a game zip from a PR build (although TODO: Actually make that work with modules ...)

Before getting ChatOps working a simple job in Jenkins able to use a Kubernetes credential to allow it access into the parent cluster would then be able to execute a few commands (even partially provided by job parameters) and a server should be available in a few minutes.


## Differences from KubicARK

So ARK is a vastly more complicated game, clearly, at least so far! But even looking aside its complexity and relative configuration nightmare the sheer fact that it is closed source and not officially documented contribute plenty to making it hard. No such problem with Terasology!

KubicARK for instance has a sizable init container that needs to run before the main server container starts, as exactly which files get read from where and can even exist at which points in time is complicated. Additionally with a game cluster setup it makes sense to have a global set of configs of a given type plus a server-specific set - we don't need that for Terasology, yet. So no init container. Yay simplicity!

ARK also uses multiple game ports _and_ has a Steam integration which can make it hard to redirect ports, especially when you then try to transfer between servers in a cluster from in-game. So in that case ARK had to use pass-through ports - same numbers on the outside and the inside. For Terasology we could always leave the server on the same internal port (and have to, for now, as we don't even support changing it, I think?) then simply use a different port externally and redirect it via Kubernetes.

We also don't need the file share, only have one config file, and we can easily redirect where said file lives.


## Future extensions

For Terasology specifically we can modify the engine and everything else easily, yay! So the sky's the limit, don't have to worry about what you may be able to mod or not. So that gives us some immediate options:

* For regular servers (play tests, official release server, official dev build server) we can let them hibernate then literally add a button to the "Join Game" menu that'll send a wake-up request to such a target server. Probably that would just be some tiny ping to an API URL, with no permissions or anything needed, since we'd be running the servers 24/7 otherwise so a bunch of "Start the server please!" requests will change nothing. That _could_ of course eventually become a target for DDOS type attacks but ... don't prematurely ~~optimize~~ secure?
* The initial effort here only sets up the regular headless game server. We could instead aim to use the [FacadeServer](https://github.com/MovingBlocks/FacadeServer) to readily expose more API options, that could be both hooked up on the web and via chat bot
  * One minor obstacle here: the `override.cfg` file comes from a ConfigMap and is thus read-only. It'll apply every time the server starts, so if the FacadeServer is used to change things it cannot edit that file. An similar workaround is used in KubicArk where config files are copied forward from the config volume to other resting places where they can be edited
* A hook has been left in place via separate ConfigMap to populate player-related config files, such as a whitelist
* Extra environment variable spots have been prepared for other enhancements, such as setting a server access password or a way to escalate your permissions to admin via password rather than secret key in the generated `config.cfg` - but again, these aren't implemented on the game server side yet