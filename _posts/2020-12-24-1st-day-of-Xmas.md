---
layout: "posts"
title: "First Day of Xmas"
date: "2020-12-24 23:59"
---

On the first day of Xmas my nerdy love led to me ... writing [https://github.com/Cervator/KubicArk](https://github.com/Cervator/KubicArk) - a Kubernetes hosting setup for ARK!

This isn't very [Terasology](https://terasology.org)-related, but it will be eventually. This helps set the stage, with some fun discoveries and experimentation!

## Why ARK

[ARK: Survival Evolved](https://store.steampowered.com/app/346110/ARK_Survival_Evolved/) is a fairly decent game with a somewhat troubled past at times. Can't argue against it not being popular though, but opinions vary.

Something with it clicked with me and I've played it a fair bit over the years. A lot of the creature related systems really hit the spot for gameplay I'd like to see in Terasology as well.

* Taming system - quite a rich series of differing methods here, from the basic "knock out and shove food in" to quite elaborate challenges, where position matters in a hostile world.
* Combat - of course. It was really quite something to ride a Rex for the first time and roar at the world around you. Plus travel fast! Even fly or swim.
* Utility roles - some creatures excel at harvesting given resources, others have special abilities in combat, yet more have unique extras like crafting/refinement options or moving objects around.
* Breeding system - find and tame specimens excelling in a given stat, put them together with a mate in a suitable environment, get baby creatures with a chance to inherit the better stats.
  * Mutations - small chance to get a spontaneous mutation increasing stats a little further, possibly passing on existing beneficial mutations, as well as possibly gaining new body colors. 
* Variant colors - all creatures have a set of color zones for parts of their bodies, inherited randomly from parents or rarely from mutations, leading to desirable lines of unusual looks.

That's not to mention the amazingly beautiful and detailed worlds, the (at times awkward) building system, challenging caves, boss fights, lore, nice crafting / tech tree and so on.

The one problem: dealing with annoying discount-y game hosting sites that were awkward at best and liable to lose your saved worlds at worst. Plus one of the unusual features in ARK: cross-server transfers to other maps.


## Game server clusters

Without diving too deep into the extensive backstory and progression in ARK one concept is that you're actually in a network of linked worlds, and players can traverse between different unique maps through certain map/building features.

This can be a little tricky to set up, with game server hosters having automated (or not) the process to various degrees. Plus ARK doesn't really help itself a lot with _no official documentation at all_, relying entirely on a [player-managed wiki](https://ark.gamepedia.com/) (probably cost-savings to a point), as well as an extensive set of configuration options that thanks to the lack of official and up-to-date documentation can be a frustrating adventure to get right.

* Each server node is entirely individually configured with every possible configuration option you'd want, the map choice, PVP settings, and so on. One tiny element is a shared "cluster name" meant to be in sync with all members of a cluster.
* The servers must be able to reach each others on *some* game ports - exactly which thanks to the lack of docs is unclear.
* All servers must be able to access a shared file directory
* Servers must be patched individually, can drift out of version sync, and clients can only play on servers matching their major version (with the usual Steam hassle to find ways to downgrade if needed - for a 200GB+ game)

The various configuration options can be applied in a mish-mash of different config files and/or as command line arguments to the game executable, with or without option values, sometimes differing in separation characters, and so on.

Some game server hosters expose all the files, others provide UI configurators, or a mix thereof. Access is often limited to a web control panel à la basic web hosting with maybe limited file browsing or FTP access.

If you host yourself you're additionally on the hook for making sure all the right ports are exposed through routers and firewalls, let alone any sort of LAN quirks.

Finally with everything configured seemingly right each server is still an entirely independent monolith with all its own data. Each server must be managed independently, logged into independently, and so on. Typical Steam quirks apply (the server listing through Steam may or may not work, friend invites likewise, in-game server listings may differ..)

Ultimately if you make it to a transfer-point in-game you'll be able to see a list of other target maps you can transfer to, assuming the server config is correct. You then may be able to transfer assuming all connectivity is working.


### Cluster file share

Transferring itself is fairly straight forward and remarkably simple, focused around the configured file share.

* You can "upload" creatures without anything in their inventory (a saddle is allowed)
* You can upload *most* items, or maybe that's just a thing in [PixARK](https://store.steampowered.com/app/593600/PixARK/) - the pixellated Minecraft-style cousin of ARK
* You can upload your character itself and immediately auto-login to a target server, assuming you're not carrying any forbidden items (usually unique, event, or end-game)

From any map transfer station you can re-download creatures, items, or even yourself if something goes wrong mid-transfer leaving you in a limbo state "in the network" - this is to keep your character safe if you do not finish the respawning process on the target server.

The way this works is via basic text files being written to the file share, describing whatever object was saved to the network. When such an object is downloaded it is recreated fresh on the target server, with some limited loss of things like inventory folders (for optional organizing of items), active buffs, and so on. I forget if active gestation transfers in a living being ..

In other words it is almost like a serialization & deserialization process, using the fileshare as a transfer point, instead of anything more complicated like a direct transfer byte by byte between two live servers talking over a network. The network share is a safe checkpoint - first atomic removal from one server, storage in the midpoint, then asynchronous recreation in the target when needed.

With admin access this can naturally be abused, as you can mess with the files directly, duplicate things, and so on. Plus the share could be lost or have files go corrupted. So far I haven't seen this happen, and duplication has been accidental between server backups - having transferred objects off one map then later ended up restoring a backup where said objects were still present.

Both objects _and players_ can end up in several places at once which doesn't bother the game in the slightest. On the flipside tribe establishment doesn't transfer or sync at all, meaning you have to recreate on each map, and each such tribe is technically unique.

The only limitation is that a player can only have one character on a map at a given time. If you try to transfer into / download a character to a map where you already *have* a character you'll be asked which to keep, with an option to exit, leaving your transferred character safe in the limbo between servers.

When you have a character on a server a player file is stored in the world directory, based (normally) on your Steam id, which is also how whitelisting, auto-admins, etc is handled. I assume essentially that file just gets moved to the file share and later onto a different server. Tribe files work similarly but cannot be transferred automatically.


## Enter Kubernetes!

So - the game setup is both fairly complex yet fundamentally also pretty basic. Mostly we're just talking about a bunch of configuration files and folders then one process per running server. "Professional" (or "amateur" in some cases) hosting companies provide typically awkward and limited options for setting up and managing your server(s)

To improve this process and also learn a bunch about Kubernetes (k8s) ahead of taking a couple certification exams (passed, huzzah!), I set out to try this in a better way using the dark magic hosting tech everybody is supposed to use these days, fun! Containers everywhere! 

The initial result lives at [https://github.com/Cervator/KubicArk](https://github.com/Cervator/KubicArk), works, and is fairly well documented I'd like to think. As of writing this it is set up to just run two separate maps (there are nearly 10 available), and can run out of the box in about 15-20 minutes on a fresh Kubernetes cluster, to where you can start the game, join either server, and travel to the other successfully. Not bad!

It heavily leverages k8s config maps to take the place of _physical_ files - you never touch a single config file directly. And these config maps work at two layers, a global set that applies to _all_ maps in the cluster (no more copy pasting for every server!), then a local set for unique settings, if any.

The few things that always vary (server name, target map, ports, etc) are handled in the k8s deployment and service objects, meaning nothing truly needs to vary by server in the config maps.

At present this is very little change and easy to just clone to new server sets. Eventually using something like Kustomize, a custom k8s resource, or Helm with values files may make sense, but at present that'd be overkill.

When a set of resource files are applied to spin up a server the various config maps are spliced together if needed as files locally on the volume associated with the pod holding the game server process.

If at any point a config change is needed it can be edited in-place or updated in Git, with the resources re-applied. This will simply destroy and recreate the pod holding the server, reattaching to its persistent volume for game data.

This means server processes can be treated as [cattle, not pets](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/) with the separation between logic and game data. Backups and restores become trivial, as do game server version updates (aided further by using an ARK manager tool on a given server), or even replacing the entire OS.

Oh, and you don't actually need to keep the server part online all the time, as long as the volume sticks around and you can trigger a server recreation easily ...


## Future extensions

This brings us through the present to the precipice of the future. Another drawback of current game server hosting providers can be simply the setup and billing for individual services, lag caused by manual customer support, and just the sheer cost of having services sit around you might use a few hours a week.

What if the servers could just turn themselves off and back on again when needed?

Kubernetes again makes this trivial! One of the next steps will be to run a simple [cron](https://en.wikipedia.org/wiki/Cron) (either of the k8s variety or on the individual server pods) job that checks on server activity, and if a server isn't active any more (no player activity for x minutes) then blow it up! You don't need to keep it online forever, and any gameplay content that's enhanced heavily by copious amount of unattended server time just seems like grind that should be refactored anyway, and an irresponsible waste of computing resources (and therefore electricity) 

For ARK, which is a commercial closed-source game with _some_ amount of modding possibly there's no inherent ability for a server to bring itself back online automatically, although an old coworker of mine had a brilliant home router setup where if a statically defined server list in a game was queried the packet trying to reach a downed game server would trigger a process that would start the server and it would be available if you refreshed a few minutes later. For ARK the server just likely would never show up nor get queried - although maybe, via Steam server list.

But there's a simple alternative: make another robot do it! Specifically a chat bot, able to connect to a Discord server and listen to users on a whitelist. Simply ask the bot "Hey please bring up the Crystal Isles map" and the bot can scale the deployment for that map from 0 to 1 pods. Wait 10 minutes and game on. The same cron process as earlier would then turn the server back off again later.

At a likely high time efficiency (number of minutes online with and without players) odds are this would be cheaper than even the most discounty server hosters - assuming you don't run an intensely active server community anyway, which is sadly unlikely in my case ;-)

Other potential options include maintenance operations (trigger upgrades, add new players to the whitelist, etc) and admin commands (trigger events, kick users, chat from outside the game). Sometimes you have to navigate awkward web consoles or be in-game and escalated as an admin to do those things.

And that's just ARK - a closed source game. There are many others that could get the same treatment here for easier and cheaper gaming communities. But what if a game was literally built to take advantage of such things? For that we'll have to wait for the next day of Xmas, although one quick teaser would be a button inside the Terasology join game UI that would trigger the startup of a given server, or perhaps better options to trigger server testing via pull requests on GitHub ...
