---
layout: "posts"
title: "Third Day of Xmas"
date: "2020-12-26 23:59"
---

_Post series - [part one](2020-12-24-1st-day-of-Xmas.md), [part two](2020-12-25-2nd-day-of-Xmas.md), **part three**, [part four](2020-12-27-4th-day-of-Xmas.md)_

On the third day of Xmas I wrote a bunch of stuff!

Unlike the first two blog entries in this series, on [KubicArk](https://github.com/Cervator/KubicArk) and [KubicTerasology](https://github.com/Cervator/KubicTerasoloy) there's now no code to look at - this is all design. This part is mid-term'ish (in a long term sense, of sorts)

With those two prior blog posts we covered Kubernetes being used to host ARK and Terasology, and that we could achieve some additional benefits with Terasology that you likely can't mod into a game like ARK, and that there often may not be appetite for in a regular game developer to really take on.

Lets look at some of those options as it relates to current and potential soon'ish :tm: architectural refactoring for Terasology!  

(As a minor disclaimer: Some bits below may be impacted by potentially faulty memory and incomplete understanding of various systems - but best effort is made, feedback appreciated!)


## Some from the past, some in the present, some for the future

Traditionally we've probably done more reshuffling of the architectural deckchairs of our amateur cruise liner of a project than wise versus the amount of focus on playable content. This is mostly a consequence of our organizational structure - we're made up of geeky volunteers mostly scratching a variety of itches of personal interest, typically more short term in nature than aiming for overall project health in the long term. 

We're probably doing better on this right now than we have in years with our recent focus on stability and content. But there's still room for architecture - we just need to prioritize and focus on finishing things rather than sprinkle unfinished sprawl all over the place. As such these points are not likely priority points particularly soon, but when it seems sensible and we have interested contributors to work on them.

* Past: Single player vs multiplayer (old design desire)
* Past: Multi-world and sectors (incomplete GSOC projects)
* Current: Breaking apart the engine into subsystems
* Current: Better handling of config, saved games, serialization, and more
* Future: Multi-process to run worlds in parallel
* Future: Kubernetes facade for subsystems-as-pods


### Single player vs multiplayer

Long ago there was `CoreRegistry`, and all was good. Well, kinda. It was better than what came before, what little there was. Later came `Context` in one of our many attempts to architect something better that improved on things but did not actually finish the original objective fully.

One problem with `CoreRegistry` is its static and singleton nature. It can be hard to unit test, and easy for leaks to sneak in. An original objective of `Context` was actually being able to run multiple within the running engine serving different, well, contexts. Which works now to a point.

At present running the game in single player is markedly different from running in multiplayer, and not just from the potential player count. Behind the scenes single player uses special cases rather than just being a networked session with a single player. This is faster and easier to develop (multiplayer was added to Terasology long after its birth), but it means things work differently in those two cases, content can be developed and work in single player but be completely broken in multiplayer.

`Context` came about in part to make it easier to arrive at architecture where single player was simply a case of multiplayer with just one player - a full game server and separate client connected to it. Less static stuff and easier testing, more consistency in development (no two separate cases for a running game session)., fewer concerns about context leaks, etc.

The goal was also to _completely_ replace `CoreRegistry`, but despite the best intentions as it often happens the project didn't quite complete and today `CoreRegistry` simply wraps a static `Context` for backwards compatibility and usages of both are all over the codebase. And the split for the special case single player never started, nor was it necessarily universally acclaimed.

At the least we need to finish the conversion that was started from `CoreRegistry` to `Context` for better context handling, automated testing, and so on. This will likely be a JOML migration level effort. I believe we'd also be well off approaching the second half and looking at making single player server-based. 


### Multi-world and Sectors

Yours truly comes up with a lot of crazy ideas and pushes a wide array of hopefully interesting but also likely complex concepts. I apologize!

One of these mega-topics was [posted in 2015](https://forum.terasology.org/threads/new-conceptual-layer-sector-plus-musings-on-multi-world-node.1420/) to cover primarily two new concepts:

* Multi-world - nothing revolutionary, seems like a fairly basic sort of feature we should support. This became the basis of a GSOC project that got _quite close_ to where we have all the UI elements and support, but it didn't quite arrive at a solution for spinning up world gen in parallel nor full travel between worlds before the GSOC work period ended.
* Sectors - a more niche desire to have a way to cut a given world into _sub-worlds_ for eventual optimizing of processing-heavy built up worlds. Most simulation (machines running, animals grazing, etc) is local and need not worry about similar activities half a world over. Also led to a GSOC project, that again did some bits (like Surfaces/Zones) but not all.

Both lean into my desire to _eventually_ be able to scale dramatically, both within worlds (supporting large populations as you can process distinct parts of a world in parallel) and with several entire worlds in a given game. But they're complicated topics, and realistically were too much for GSOC, even broken down to simplified approaches.

I still believe in their future, but moreso multi-world because that's content-centric in the end, sectors is purely about optimization, and a premature one even today. I'm honestly kinda surprised we did a GSOC on it, and probably am to blame personally for being overly enthusiastic to the point where it was picked by an applicant ;-)

So - how could we finish multi-world? Possibly in a radically different way, building on the ARK map cluster approach covered in part 1 of this blog series along with the later topics in this post...


### Separated engine sub-systems

Sometimes large batches of really cool and awesome code just appear as if out of nowhere. That was the case [when DarkWeird recently started churning out engine-subsystems](https://github.com/MovingBlocks/Terasology/pulls?q=is%3Apr+subsystem+author%3ADarkWeird) as a new "item" type in the engine repo. This follows [a similar suggestion from 2017](https://forum.terasology.org/threads/architecture-discussion-for-v3-engine-split-engine-libs.1897/#post-15190) (great minds think alike?) which I think didn't make it past the design stage originally due to lower priority (again, no content relation - although a potentially vast improvement to overall code quality).

In short moving the sub-systems from being _sub-packages_ to their own sub-project item akin to modules, but all still embedded in the engine repo, allows for benefits mentioned in that thread like improved API surfaces, smaller code sets to write tests for, better encapsulation, and so on. It also helps address a move away from `CoreRegistry`

Exactly where this will go and how fast is still to be seen, but I'm thrilled it is happening, and it helps feed into some other long term desired improvements and some further ideas :-)


### Better handling of ... stuff

Akin to the sub-system encapsulation there is present work inproving some of the features being encapsulated. Other long standing desires here include being able to better support config in modules, altering save games externally and internally.

Not much to actually say here as I'm not on top of the details and when what is happening, but again it is great to get this sorted out and it'll again help on some future angles.


### Multi-process for multi-world

This would be a future suggestion that would depend on the split between singleplayer and multi-player, likely brought about in part by the two prior sections, and inspired by how ARK does multiple worlds (maps) as completely separate and loosely linked together server instances.

At present the engine contains the PC facade that has a toggle for whether to run headlessly or as a regular client, which in turn can also run a hosted, but headed, multiplayer server that the host also joins as a client. Which may actually be a third scenario that can also hide subtle bugs (both client and authority systems execute in that case, but wouldn't if you were a standalone connected client)

Everything runs in a single Java process, where some things can be threaded. On a server host you could imagine running several headless servers and simply implement a way to jump between them, achieving multi-world that way. There are also ways to run sub-processes in Java, although I'll not claim any real exposure to it beyond just looking up that it is possible - just to avoid the setup of multiple configured servers because you want to play multiple worlds with yourself. If doable that gives us an interesting option:

* Split current PC facade (which is essentially just headless vs not, with "not" able to both host+play or just play) into two
  * PcServerFacade - runs a _single_ headless server  - this could be run in parallel to host multiple worlds, effectively a set of world subsystems in a standalone process or subprocess per world/server/game (important extension to this in the next section)
  * PcClientFacade - runs the regular headed game client - in here when the user starts/loads a game then a PcServerFacade is started as a sub-facade and in turn handles world-level subsystems
    * Even if the player starts a single player game they still get a PcServerFacade to connect to. There is no longer a standalone play, always a client and a separate server 
* Existing "FacadeServer" gets renamed to RestServerFacade (or better name? ApiServerFacade?) which again runs the PcServerFacade as a sub-facade, while exposing some game capabilities via API (which in turn we have a web front-end that can interact with

This does not quite align with the current concept of a facade being the one single thing you put in front of the engine to give it some sort of execution objective. There may be some more design lurking here on what's a facade, when can one be used as a sub-facade by _another_ facade, and when do things start becoming sub-systems. Another potential example would be touchscreen support - is that a facade or a sub-system?

To achieve multi-world support here you'd use the ARK-style approach of treating worlds as independent servers, linking them together into a game server cluster the player can traverse in some fashion.

This avoids part of the complexity that doomed the initial GSOC project on multi-world (and sectors too, really) where all the logic was tied up in the engine, and had to deal with all the existing quirks, leaks, and limitations. Do you keep track of which world a component applies to? Or are entity pools independent and isolated? 

While you may still need to _start_ with everything organized by a single process I imagine you could step the separation up to the next level by using sub-processes. But really what I'm after here is the next section.


### Kubernetes facade for subsystems-as-pods

It may well sound a tad awkward trying to support multiple processes in the regular game client, and honestly it probably would be - it really is just a shortcut to the dedicated server hosting scenarios below.

At some point there's probably a cutoff to the amount of stuff you'd want to cram into a game client, creating a truly dedicated server instead, that may differ vs the client bit. Facades and subsystems allow us to do that in a nicely modular way.

1. A single player who just wants to play the game, with any server/client mumbo-jumbo figured out completely seamlessly. Here we've still arrived at a higher code quality level by splitting apart single player to always use a client+server(s if multi-world enabled)
1. Somebody wants to host one or more worlds headlessly and have clients connect externally. This is the typical approach, and would work nice and cleanly à la ARK - the special part just being each server hosts one world 
1. The new school way of containerizing everything: run each server/world in a Kubernetes pod, gain all the benefits noted in the first two blogs in this series, _and_ open the future to what other systems might host in pods vs a monolithic application

For the 3rd scenario we make a new KubernetesServerFacade that would be a container-native approach to run the game server and other affiliated services, an alternative to the PcServerFacade. It would wrap the multiple world subsystems in their own pods

You _could_ just run separate processes all as headless components without Kubernetes, really k8s is just a convenient and powerful wrapper. Plus the hibernation option and such from prior blog entries. 

You might want a login server (maybe integrated with our central identity server) that handles arriving at the world servers giving the player a choice to pick one, rather than needing to keep a list of each individual server/world.

You might want centralized chat that would work both in a _lobby_ during a character creation phase as well as between worlds

You may even want an external celestial system handling the movement of bodies in a solar system, assuming each world is a planet, where each server process can query updates via API rather than calculating (and synchronizing!) positions on their own. Maybe that could even be a movement space of sorts itself ...

Several of these would be non-critical, allowing Kubernetes to move them around to where compute resources are available.

Ultimately these could all likely be expressed as subsystems, or even _regular_ entity system classes, starting out together in the engine repo (or modules) with no apparent change to the game. Then over time reorganize available facades and the modes in which the game executes, to where you could run a series of processes, be they within the single game client, hosted standalone in any sort of hosting solution, or fully managed within Kubernetes and able to scale to infinity and beyond!

This is already beginning to sound somewhat crazy befitting my reputation, but just wait for part 4 ...