---
layout: "posts"
title: "Agentic Engineering vs. Three Tiny Humans"
date: "2026-08-24 16:00:00"
image: /assets/images/FamilyGDD.png
header:
  teaser: /assets/images/FamilyGDD.png
  og_image: /assets/images/FamilyGDD.png
toc: true
toc_sticky: true
classes: wide
---

On July 21st, [Guardian Driven Development hit 1.0](https://github.com/SiliconSaga/yggdrasil/releases/tag/v1.0.0). First tag the repo has ever worn, four months almost to the day after [I first wrote about it here](/guardian-driven-development/).

Then I spent the next couple of days doing... approximately nothing with it. No announcement, no victory lap, just a strange decompression. It turns out that after months of "one more session to clear the path," the path being clear is disorienting. So another whole month passed, just being productive and enjoying life. But as the next point release is firming up I am giving finishing this another try :-)

## The Wildest Realization

I kept coming back to a crazy new reality:

**The productivity (and morale!) boost from GDD has probably outweighed the productivity cost of raising three tiny humans.**

Anyone with small children knows what they do to focused time. It doesn't shrink — it *shatters*. You get fragments: fifteen minutes here, a nap-length window there, an evening that ends the moment someone has a bad dream. The pre-kids me with whole uninterrupted weekends would look at my current life and declare serious project work impossible, even hobby tinkering unlikely.

And yet, I'm fairly confident that no-kids-no-GDD me would have shipped *less* than three-kids-with-GDD me actually has. Not less per hour — less, period.

That's not because the AI writes code fast (it does, but that's the boring part). It's because GDD is built around exactly the failure mode fragmented time creates: **losing the thread**. The [Thalamus](https://siliconsaga.github.io/yggdrasil/gdd/thalamus/) remembers where every effort stood when the bad dream happened. Sessions re-orient themselves in seconds instead of me spending my precious fragment reconstructing context (they can even leave messages for each other). "Arc" style larger projects survive across machines, days, and interruptions, and a dashboard adds visibility. The fifteen-minute fragment becomes *usable*, because neither of us starts from zero.

Another angle hides in that "(and morale!)" bit. I'm doing more and better _off_ the nerdy computer topics too. Measuring the basement for better HVAC Manual J load numbers and ductwork options? Check! Feeling like I can volunteer more with local community groups? I got the energy. Networking with other professionals and locals on topics that _seem_ completely unrelated? Exciting to tie it all together! Not to sound like a motivational speaker advertisement but it is wild how just a trickle of more meaningful activity can motivate you to keep that going on a path to some sort of greater level of self-actualization.

I'm managing to also touch grass. But I might still have used my agent to find the optimal grass!

## What Five Months Actually Produced

(Well, at publish time it has been another month, but I don't want to redo all the numbers ..)

Counting GDD itself plus everything built with it — the apps, the platform stack, the civic sites, even the shared-memory hoards, but *excluding* the big pre-existing projects, forks, and vendored code — the last five months (the earliest script pieces predate the March announcement) come to roughly **220,000 lines across ~1,500 files**. About 70k of that is GDD and its community-config layer; about 130k is things built *with* it; the rest is the living memory the methodology runs on — some 40k lines of curated notes, arcs, and audit trails that my old workflow simply never produced as an artifact at all.

For rough comparison, that's approaching the size of [Terasology](https://terasology.org/) - which was built by a community over a decade-plus. Different composition, absolutely — there's a lot of markdown and YAML in my pile — but GDD treats the design docs and the shared memory *as* product, not just overhead, and one todo is post-processing that further into Architectural Decision Docs and similar for the future.

- **117 processed pull requests** (and 7 open as they pile up waiting for v1.1 to finish so the v1.2 floodgates can open) to the workspace itself, every one reviewed — by AI review bots, by me, and toward the end by *other agent sessions cross-reviewing each other's work* from parallel workspaces not all run by me! The final pre-release PR was built by two agents in two workspaces, each catching real bugs in the other's commits.
- One stretch I wrote down in the moment because I didn't quite believe it: **three workspaces in parallel over two days** produced a school-district civic site, a release-operations skill for my day job, and a security tier for GDD's own permission system — about 11.5k lines of reviewed, tested code plus 5k lines of design docs.
- Among other case studies a [local campaign website rebuilt for a non-technical friend](https://siliconsaga.github.io/yggdrasil/gdd/case-studies/) who could request site updates via chat even dropping in images that a sandboxed agent then turns into reviewed pull requests with a live preview site and visual diffs. He doesn't know what git is. His edits can ship to production anyway.
- And in the last week before the tag: the release process caught a real bug in *git version compatibility* using its own acceptance tests, on my own decade-old machine. The tooling audited itself on the way out the door.

## The Balancing Act

If you're an AI-tooling nerd, here's the frame I'd offer: GDD sits deliberately between two poles that get all the attention.

On one side, the cautious camp: AI on a short leash, autocomplete-plus or agentic-curious, serious control with limited reach. On the other, the YOLO swarm — a thousand agents, minimal review, maximum throughput, and whatever happens to code quality happens ([I wrote about my conflicted feelings about Gas Town last time - good and I love that it exists, but it is for specific audiences](/guardian-driven-development/)).

GDD is my attempt at the interesting middle: agents with real autonomy inside **deterministic guardrails**. A permission hook that denies dangerous shell patterns before they run and *teaches* the agent the house style with corrective messages. Trust gates where activating community config requires a human to approve trust — fingerprinted, so if it changes later, trust goes stale and you get asked again. Commit, issue, and PR attribution that hard-errors rather than guess who did what. Every commit through a template that demands the *why*.

My favorite proof came at the very last step. When the tiny release commit was ready, the agent tried to push it to main — and got refused, because branch protection requires pull requests and doesn't care that you're the release ceremony. I chose to push it direct, deliberately, with my own credentials separate from agent tokens. The framework's whole thesis — automate everything except the click that must be human (as per human preferences) — held against its own author's agent, at the finish line, on the commit that shipped it. I could not have scripted a better closing argument.

## Receipts for the Skeptics

I recently shared two PRs with a (healthily) skeptical open source community member, and I think they make the case better than any pitch. You don't need to read the text in them — just scroll [PR #134](https://github.com/SiliconSaga/yggdrasil/pull/134) (two agents in two workspaces cross-reviewing each other, then two review bots, then me reviewing *their* reviews) and [this thread on PR #111](https://github.com/SiliconSaga/yggdrasil/pull/111#discussion_r3484064368) (my agent disagreeing with a bot, with the reasoning posted for the record) and absorb the *shape*. That volume of scrutiny is something human teams never find time for — the skill is triaging it honestly: fix what's real, push back with reasons on what isn't, skip what's too edge-case to matter. Confident-but-wrong slop is real, and it is *not what this is*.

What happened *after* I sent those receipts turned into the best day GDD has ever had — a real Terasology contributor, four merged PRs, and a years-old engine bug found and killed, mostly from my phone. That story is getting its own post soon.

Oh, and about words: I've settled on **agentic engineering** as the term I actually want to use. "Vibecoding" describes a real and fun thing, but it isn't this — there's nothing vibes-based about fingerprinted trust boundaries and massive regression test suites. And sprinkling "AI" into every second sentence has stopped meaning anything at all. Engineering, with agents. That's the discipline I'm trying to practice, and name things for.

Someone recently asked me how GDD compares to Claude or Codex, and I finally have the answer down to a sentence: **an agentic engineering methodology that goes on top of either.** The agents supply the horsepower and they keep getting better for free; GDD supplies the memory, the guardrails, and the discipline — the parts that decide whether that horsepower compounds into a project or dissipates into a chat log.

## The Honest State of Things, 1.0 Edition

My March post had an honest-state section so here's another.

GDD 1.0 is **not** yet the thing I can hand to a non-technical friend cold. Fresh-machine testing made that very clear — the intro dives into machinery before explaining itself, the first tutorial assumes more comfort than a newcomer has, and the ideal "here's a webpage, edit it, see it change, no accounts needed" on-ramp doesn't exist yet (oh wait, a month passed, now it does!). All of that is [on the roadmap](https://siliconsaga.github.io/yggdrasil/gdd/roadmap/) as a multi-phase learning arc, along with chat-driven workflows and PR previews with visual diffs so a non-developer can *see* a change before it ships. It'll get there. Version one of anything is a promise, not a finish line.

What it *is*, today: a genuinely usable workspace for a developer (or a patient adventurous human) who wants to work with agents without surrendering judgment — Claude-first, with the cross-agent story actively underway. Codex isn't at feature parity yet and sandboxes differently, as does Claude if you enable that new `auto` mode that is being pushed - both those cases can get a little ahead of my _personal_ desired gating boundaries, but they still work (and go faster as one may expect - again it comes down to individual author preferences)

## The Lull That Wasn't

A confession about how long this post took to actually publish: the quiet after 1.0 turned out to be so pleasantly productive that a whole point release fell out of it while I wasn't looking. Remember that roadmap paragraph two sections up — chat-driven workflows, previews with visual diffs so a non-developer can *see* a change? Between drafting this post and hitting publish, those stopped being roadmap lines. v1.1 is out the door with this post, headlined by [gdd-sandbox](https://github.com/SiliconSaga/gdd-sandbox): a scoped GDD agent in a Docker container (soon: [full Nvidia OpenShell](https://docs.nvidia.com/openshell/about/overview)?) that someone can drive entirely over Discord, with every change still arriving as a reviewed pull request — plus the web tooling that grew around it, where each PR gets a live preview and before/after screenshots so "ship it" can be an informed thing to say from a phone. More on all that in its own post, once I stop building long enough to write it. 😅

## Try It

Same invitation as March, now with fewer sharp edges:

1. [Clone the workspace](https://siliconsaga.github.io/yggdrasil/getting-started/) — `ws preflight` will tell you what's missing (including, thanks to that last-week bug, whether your git is too old)
2. Start Claude Code from the yggdrasil directory and say hello
3. Ask for the mentoring overlay if you're new — the agent should now lead with what GDD *is* before asking you to configure anything, which took an odd amount of testing to get right
4. Consider asking for the GitHub Page tutorial (it works pretty well - and there's now [a pending advanced version that supercharges it and can involve Backstage](https://github.com/SiliconSaga/leidangr/pull/21) because of course, I'm a nerd.)
5. See if the flow clicks

The [release notes](https://github.com/SiliconSaga/yggdrasil/releases/tag/v1.0.0) carry the full inventory, and [the issues remain open](https://github.com/SiliconSaga/yggdrasil/issues).

## Ship It Like Nobody's Watching

Somewhere in those five months, between the school site and the campaign site and the security waves and the bedtime interruptions, this stopped being a tooling experiment and became just... how I work. The wildest part isn't any single feature — it's that the dusty-todo-list era of my creative life appears to be over, and it ended during the least free time I have ever had.

If you're a parent, a maintainer, or anyone whose focus comes in fragments: this class of tooling is for you most of all. The swarm people will still set the throughput records, but that's OK - we still get to finish our project, in each our own way. 

... I'll skip the not-news about the fate of _not_ agentically augmenting yourself in _some_ way since I'm trying to keep this blog uplifting and empowering. More fixes and balancing to come, I'm sure - even us little-people will have the ability to affect that soon.

Welcome back to Beehalla. Growing extra arms may have started as an AI hallucination, now I ask for it on purpose to better represent the feeling 🐝

[![The Guardian's family workshop — this tool helped me grow more hands](../assets/images/FamilyGDD.png)](../assets/images/FamilyGDD.png)
