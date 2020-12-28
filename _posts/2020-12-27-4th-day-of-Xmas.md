---
layout: "posts"
title: "Fourth Day of Xmas"
date: "2020-12-27 23:59"
---

On the fourth day of Xmas I went a bit insane ...

If it wasn't clear from the third part of this blog series a lot of this stuff is forecast quite a ways in the future, if ever - this part in particular really tries to **suggest** a possible vision for the _second decade_ of Terasology and our organization overall. Which does start in less than a year, but certainly we should keep the focus on stability and content going to at least, and beyond too, really :-)


### Portals and the void

One classic example and challenge with multi-world is how you might travel between them and _visualize_ things in other worlds. Usually some sort of portal option is available, although whether it is an opaque surface or something trying to indicate what is on the other side varies.

A more complicated approach with our entity system might try to literally render entities from another world in the vision of a player on one side. Make sure chunks are loaded on both sides, let animals and things spawn, and just adjust positions somehow to approximate "normal" world on the other side of the portal, rather than show what's there on the host world.

A _potentially_ easier and more performant approach might be able to take advantage of a Kubernetes cluster where you can have free floating game services not tied to a given world. Chat is a simple example - odds are it could be its own service, and just interact with target worlds using an API system. But then chat isn't very intensive, the gains would likely be low. 

What if a floating mini-world service could be created as needed to contain a mini-world of just a few chunks and whichever entities exist there, started out in sync with the origin world, then perhaps synchronized only every few seconds, akin to how server prediction / lag prevention attempts to show the world as it might be, yet the server corrects if needed? Register a special cross-world component on the relevant visible entities, then capture and duplicate events for them to the floating mini world. Between updates let it simulate expected movement. Output a single image that represents the appearance of the stuff in that world from the angle a player is viewing a portal at, and just send that image to the other world to paint the front of the portal with. 

Make it work both ways, add some graphic effects to the actual portal images, allow for gameplay reasons to show delayed or distorted images, and you have a believable system that exerts next to no pressure to the real worlds, even if you've got a dozen players on either side observing at different angles. You just have a single small system on each side responsible for tagging visible objects around the portal with a single component so you can capture and forward a few events, duplicated per user if needed.

In a perhaps forced example this may remind a bit of the Record & Replay system - only rather than replaying events that have happened in the same world at a different time you can transfer the data to an ephemeral floating virtual world where it plays back just to generate images for portals.

Imagine what other opportunities this sort of approach could open, while minimizing performance impacts, for instance the celestial calculation system mentioned in the prior blog entry. Break apart that monolith, and make it shine.


## Inter-server travel, round two

In the prior blog entry I linked the [multi-world / sectors thread](https://forum.terasology.org/threads/new-conceptual-layer-sector-plus-musings-on-multi-world-node.1420/) from 2015. There's an interesting little timeline or map near its top:

> Block - Chunk - Sector (simulation) - Cluster (multi-node) - World - Universe (multi-world) - Metaverse (inter-server)

In past GSOCs we attempted Sector and Universe - and still have _some_ functional code from those efforts. Previous parts of this blog series has hinted at the cluster and meta-verse steps, although with some adjustments

* Cluster - originally the goal here was to group sectors and run them in parallel with their own threads or even server nodes. Realistically that remains premature optimization, yet the ARK cluster setup is a tantalizing way to rethink how you might achieve multi-machine hosting of a single game. It becomes more about splitting apart different worlds and game service not needing to be in the same process than sub-dividing a single world full of different systems.
* Meta-verse - again originally a bit different, in thinking it would let you travel between individual game servers, _each_ of which could host multiple worlds. With ARK-style multi-world the different worlds _are_ different servers. And you're not really limited in keeping them to the same config, you can even host the same map multiple times or give people personal worlds.

If a single _Kubernetes_ cluster can host a bunch of game services, and an arbitrary number of worlds, all in parallel and scaling to additional server nodes, then that's effectively the universe layer. Everything in that Kubernetes _and_ game cluster is really one grouping of game things.

You could go beyond that in additional namespaces or even other Kubernetes clusters, or alternative ways to host the game services. With server-transfer being the same as world-transfer there's really no difference between swapping worlds in your local cluster vs some friend's remote cluster. Just more definitions of where to find things (think cloud-backed file share storage for inter-cluster data transfer), presenting metadata on destinations you can travel to like rulesets, moderation, etc. 

That may present a challenge if different modules are enabled within different server groupings. Much like how an ARK server may have mods enabled while another in its cluster does not. But since the transfer happens via simple text files in a file share, you don't run into any sort of syntax errors. You _may_ end up with data (like unknown components) a target server has no idea what to do with. I'm sure there are solutions for that, like checking your JoshariasSurvival powertools at the door if you're entering a server running MetalRenegades, potentially reattaching those extra components on your way back.

Lets call that the _regular_ meta-verse - jumping between grouped Terasology servers, that may vary in config, even modules enabled.


## Inter-GAME travel, wat

Hold up. If you can transfer between servers running different _modules_ by virtue of simply stashing your incompatible items on the way there to return later - what about entirely different **games** assuming both are built on our frameworks like Gestalt powering assets, entities, and so on? We already share libraries between Terasology and Destination Sol, and they will likely grow closer. Conceptually you would _already_ be able to use very basic modules unmodified in both games (think basic algorithms and such, like MarkovChains).

Is it really a stretch to use the same approach to literally jump from a Terasology world to a 2D spaceship taking off from a ground base on a planet in Destination Sol? Which could well represent a 2D lower detail version _of_ that Terasology world, but from space? You'd just have to leave more incompatible things behind. You won't need your pickaxe while flying a spaceship - although maybe you could pull it back out if you land on an asteroid, so long as you also acquire a spacesuit. If the minerals you mine are the same in both games does it really matter where they came from? Could flying a spaceship in DS between planets be another way to travel between Terasology worlds?

At some point to make such a transfer meaningful you'd probably need to build in _some_ sensible translation layer for more than just whichever components happen to exist in both games. Maybe your 3D avatar in Terasology should have a 2D profile picture generated which could be shown on an info panel for a spaceship you're flying in DS. If not then at some point you'd probably be just duplicating the lobby systems like battle.net or your Steam profile or some system avatar on a Wii. It needs to be more than that for sure.

Additionally, it is one thing to change world in Terasology, you're still talking the same engine, with a set of modules, facades, etc. But wait a minute - haven't we been talking about a `gestalt-engine` for some time now? And we can already hotswap things like assets in modules? Other than immense effort for a fairly crazy concept this seems like it should be technically feasible - although I for sure wouldn't want to defend it as an investment to a corporate board. Oh hey, there's one of those potential advantages a large open source project may have over a commercial entity ... even if it might take us a century to do it ;-)


## The OASIS

Does that cross-game talk sound familiar to anybody, perhaps for readers of [Ready Player One](https://en.wikipedia.org/wiki/Ready_Player_One) or likely a slew of fictional literature like it?

Now, realizing all this in virtual reality with full-body haptic suits and other crazy tech might be a tad out of our reach, but transferring between entire game settings within a single universe (or meta-verse, or multi-verse ... we might need to do some bikeshedding here!), that might actually be doable. Perhaps even within our second decade, particularly if we can actually pull off the stabilization and content-focus to actually have a substantial player base _for_ our second decade.

This brings up other challenges which can be somewhat glazed over for a book or movie that the viewer is only indirectly connected with as a passive obsever. How do you balance one player who specializes in medieval weaponry with a space adventurer?

Additionally, on a more fictional note in regard to the setup of [the OASIS itself](https://readyplayerone.fandom.com/wiki/OASIS), and really as a flourish to end this blog series on, how would one actually host and build such a thing from scratch? In the book and movie it is a single and immensely powerful corporation only held in check by its benevolent creator's will and terms enforced by an army of lawyers, which teeters on the brink of disaster anyway. This is not the open source way, of course: decentralization is key. What better way than organic growth starting at the individual server/world level, building into small universes, that connect through friends sharing different communities causing those small universes to connect in larger groups. Maybe the groups could later federate together further, forming a map of connections to ever-more distant and strange worlds.

Wouldn't that be neat. We'll probably never get there, and surely it would take years and years. But why not idly try to build it together? I know I'm game.
