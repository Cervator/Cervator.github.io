---
layout: "posts"
title: "Civic Reclamation: Coordinating a Town with Multi-Agent Systems"
date: "2026-06-19 12:00:00"
image: /assets/images/CoordinationTaxCollapses.png
header:
  teaser: /assets/images/CoordinationTaxCollapses.png
  og_image: /assets/images/CoordinationTaxCollapses.png
toc: true
toc_sticky: true
classes: wide
---

_This is the nerd cut — written for the engineer who looks at what multi-agent orchestration can do right now and feels a little sick watching it get pointed at ad click-through rates. If you're more neighbor than nerd, I wrote [a plain-language version of the same idea](/it-takes-a-village/) that skips the jargon and leads with people instead of architecture._

[![Left: one frazzled volunteer coordinator drowning in sticky notes and group chats. Right: the same field humming, the coordination now invisible — warm threads quietly routing help where it's needed.](/assets/images/CoordinationTaxCollapses.png)](/assets/images/CoordinationTaxCollapses.png)

If you've sat through a Board of Education budget meeting lately — I have, more than is healthy — you've watched a slow-motion structural failure that everyone in the room is too close to name. My town, West Orange, is staring down a deficit in the neighborhood of $15 million. On May 4 the budget passed, and with it the outsourcing of more than 200 paraprofessionals to staffing agencies. That harm doesn't land in a spreadsheet — it lands in September, on specific kids, including, for the record, [mine](https://schools.frontstate.org).

It is easy to blame local politics or incompetence. The root cause is structural. Over the last few decades, a lot of public functions got quietly enclosed by private intermediaries. School lunch used to be cooked in the building by people who lived in the neighborhood; now it is often a catering contract. Paraprofessionals were district employees with a union and benefits; outsource them and an agency takes a 30–40% markup while the actual human doing the work sees less money, fewer benefits, and a name tag that changes every year. I wrote a whole [community whitepaper](https://schools.frontstate.org) about this pattern — I call it the Shrinking Slice: the community's share of its own economic pie eroded by layers of rent that simply did not exist a generation ago.

We are told the only options are to raise property taxes or cut public jobs. That's a false choice, and mostly a failure of operational imagination.

## We are not going to be granted permission

I have gone to those meetings as an idealist with smaller versions of these ideas, and every time I think: why on earth would they listen to me? They would be a little insane to. A superintendent who greenlights some unvetted algorithmic volunteer scheme and watches one kid get hurt, or one union grievance get filed, is done. A superintendent who rejects it and lets the budget bleed out over ten years gets to call that "the economy," and nobody gets fired. Add ego — the volunteer coordinator or PTA president whose relevance suddenly feels threatened — and the walls go up instantly.

The thing is, the people on that side of the table mostly aren't villains. The problem is the format. Elected and appointed officials genuinely do not have the time, and elections don't change that. When the structure itself can't solve the problem, you stop asking the structure for permission. You build the thing on the outside until it's good enough that the institution chooses to absorb it.

Which means we don't wait. Not for Trenton, not for a state funding-formula fix, and — the part the tech crowd needs to hear — not for some future universal basic income that may never arrive at the scale that would actually matter. The usual utopian script is passive: automation displaces us, so eventually somebody hands us money. The move I'm interested in is active, and it works in fractions, today.

## The one thing that's actually new

Mutual aid, time-banking, solidarity economies — none of this is new. People have tried to coordinate spare time and spare stuff for as long as there have been neighbors. It always collapses under the same load: coordination overhead. One human coordinator burns out matching spreadsheets, chasing flakes, and backfilling no-shows. [Coase](https://en.wikipedia.org/wiki/The_Nature_of_the_Firm) named the underlying thing — transaction costs — and they have always been the ceiling on this kind of organizing.

The genuinely new variable, over roughly the last year, is that agentic tooling collapses that specific cost. Not in a vague "AI will be smart" way — concretely: a part-time organizer, backed by agents that parse a chaotic group chat, match availability to need, send the reminders, and notice when a slot is uncovered, can now run what used to require a stressed full-timer. The coordination tax drops. That is the whole thesis, and it doesn't need anything wilder to stand up.

[![A warm overhead view of a neighborhood and a sports field, gentle golden threads weaving spare time and idle capacity toward where it's needed — no servers, no screens.](/assets/images/InvisibleVillageMesh.png)](/assets/images/InvisibleVillageMesh.png)

I want to be precise about what's new and what isn't, because LLMs reward you for over-claiming and this whole space is drowning in hype. Web3 already took a swing at this with DAOs and tokens — and missed, because it mistook financial incentives for operational capability. A smart contract can move a token. It cannot reroute a parent to grab the practice cones when the first parent's kid spikes a fever. What changed is semantic: agents that can read messy human text, reason about a schedule, call an API, and negotiate a backfill over plain SMS. That's the unlock the DAO crowd never had.

One thing I missed at first, and it's the part an institution actually cares about: the value isn't the free labor — it's **reliability**. The board's recurring, completely fair question is "will people actually show up?" Volunteer efforts fail less from lack of goodwill than from nobody tracking who committed to what. So the real product is accountability infrastructure — commitment tracking, coverage dashboards, a visible volunteer SLA. The answer to the board stops being "trust our enthusiasm" and becomes "read the dashboard."

## The Time Dividend (you know the one)

Those of us whose day jobs these tools are already transforming are in a strange and lucky spot. We can see the deflation coming. The question is which kind. Bad deflation: companies cut headcount and pocket the productivity gains, displacing people into destitution. Good deflation: costs fall fast enough that people simply need less income to be okay. I sketched this fork out at length in [the AI-horizon piece](https://schools.frontstate.org) on the school site.

The interesting thing is that you can start banking the *good* version yourself, right now, in fractions. Reclaim a sliver of the productivity AI is handing you and point it at the place you actually live. I'm not going to spell out the wink any harder than that — the arithmetic on what "I don't need a full forty hours to ship my work anymore" implies is easy enough to do. Call it the Time Dividend: hours you can give a community not because you have to, but because, for the first time, you can. No federal program required. Just a town quietly removing the tolls from its own roads, one at a time.

## The cheat codes

A multinational staffing or catering giant has economies of scale. A local school district or an allied 501(c)(3) has structural advantages those corporations would kill for, and a platform should systematically exploit them:

- **Tax-exempt procurement.** Public entities don't pay sales tax on wholesale buys. Build direct-to-farm or direct-to-manufacturer pipelines and you skip the distributor markup entirely.
- **Stranded assets.** Towns are littered with dark infrastructure. School auditoriums and gyms sit empty most evenings and all summer; commercial kitchens go cold for sixteen hours a day; fields can be leased for a dollar. (A neighbor near me runs an honest-to-goodness heavy-equipment-sharing thing — the excavator one household bought and uses twice a year, available to a block.) Even outgrown soccer cleats are a stranded asset: a dead-simple cleat exchange over a group chat keeps that value in the community instead of letting it rot in a closet or vanish into a donation truck bound for a sorting facility a thousand miles away.
- **The Virtuous Spiral.** Unlike natural costs, extraction costs can be *removed*, and removals compound. The first season is grinding. The second is momentum. By the third, the community wonders why it ever accepted the old cost structure. Snowballs, not silver bullets.

## The cold water (this is the part that makes it real)

If you only take one section seriously, make it this one, because it's where the credibility lives and where most "AI fixes civics" pitches faceplant.

- **Not every rising cost is extraction.** Some of it is [Baumol](https://en.wikipedia.org/wiki/Baumol%27s_cost_disease). A string quartet still takes four players the same hours it did in 1800; teacher pay rising is largely this, and it *should* rise. Don't confuse the Baumol layer with the rent layer. We target the rent layer.
- **Some scarcity is real.** Stripping out billing overhead doesn't strip out actual scarcity. A town can coordinate its own lawn care and run its own gear swap. It cannot manufacture a GLP-1 drug or a CT scanner, and pretending otherwise is exactly the kind of hopium that gets this stuff laughed out of the room.
- **It has to be additive and temporary.** The instant community capacity is framed as a *substitute* for paying people properly — "we have volunteers now, so we don't need to fund the position" — it turns toxic, and it becomes a new extraction, this time of volunteer labor. This is why my whole framing is an **exoskeleton**: a support structure *around* the institution, scaffolding that comes off once the building stands, never a permanent offload.
- **The law is genuinely unsettled.** Can a district even legally accept community funding earmarked for specific positions? Once a volunteer drives produce or chops a carrot, sovereign immunity, FLSA volunteer rules, and commercial insurance all wake up. The serious answer is an independent cooperative entity that contracts *with* the district rather than volunteers working *for* it. State this plainly or you're selling snake oil.
- **Respect the union, hard.** Volunteer optimization exists to kill *corporate vendor contracts* — the outsourced catering, the staffing agency — and claw that margin back for public employees. It does **not** exist to replace union jobs. Change the funding source, not the employment. Bring labor in early as a coalition partner, not as a thing to route around.

## Where you actually start

Not at the BoE. You start in the lowest-stakes, highest-pain sandbox you can find, which in my town is the youth sports league. No sovereign-liability shield, no taxpayer budget, but absolute parental burnout and red tape thin enough to actually move. If the equipment logistics break, the game just doesn't happen — stakes low enough to experiment, high enough that people feel real relief when it works.

The first micro-module is boring on purpose: the cleat exchange plus a carpool-and-equipment mesh. Background agents ingest the league's chaotic group chat, extract the intent ("need three parents for field prep Saturday"), and coordinate over SMS so nobody has to download anything. You don't show the league president an economic theory — you show them a season-end dashboard: *this saved our families this many driving hours and a few thousand dollars in gas, and recirculated this many pairs of cleats.* The one ethical line I hold: invisible-and-accountable is the goal; invisible-and-unaccountable is how you end up coordinating people through a black box they can't see or question. Keep it legible and opt-in.

(Yes, I'm the guy who once half-joked at a board meeting about a volunteer leaf-blower brigade — extra funny in a town that just [banned gas leaf-blowers](https://schools.frontstate.org) and watched groundskeeping costs jump. The joke keeps being half-serious.)

## If any of this lands

I don't want to build a standalone app with onboarding and passwords and user buy-in. I want a coordination protocol that wraps the messy reality people already live in. The base layer is taking shape under the [Yggdrasil / GDD work](/guardian-driven-development/) I've written about, branded [Demicracy](https://frontstate.org), with the full civic argument hosted at [schools.frontstate.org](https://schools.frontstate.org). The methodology side of how I build any of this with a toddler on my lap is its own [whole story](/guardian-driven-development/).

If you're an engineer who looks at the current state of multi-agent orchestration and feels that it's being wasted, the immediate task isn't rewriting the municipal code — it's the first fault-tolerant, text-based agent that can turn a chaotic community request into a deterministic schedule — and prove, one soccer field at a time, that this is one of the most worthwhile things you could possibly point this technology at.

If you want to help build the pipes to take a town back, [get in touch](https://github.com/SiliconSaga/schools/issues). And if the architecture isn't your thing but the heart of it is, go read [the neighbor's version](/it-takes-a-village/).
