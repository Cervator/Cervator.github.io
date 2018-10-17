---
layout: "posts"
title: "GSOC Mentor Summit 2018"
date: "2018-10-16 23:00"
---

# A Trellorific Time

I'm nearing the end of a 3-part trip to the Bay Area, with the Google Summer of Code Mentor Summit finishing a couple days ago. Quite an experience, like always. Yesterday three of us mentors spent some time at the Aloft hotel with laptops working on Terasology, mostly preparing GCI tasks. Today was day one of GitHub Universe.

At the GSOC Mentor Summit I hosted a talk about some of the effort I've put into using Trello for Terasology over the years and how we might go about automating the process better and involving more people.

## Organizing and Motivating Volunteers

My talk focused on our current Trello boards being used to better stay on top of the coming and goings of volunteers on our projects, as well as how to better motivate and direct people to useful tasks needing work.

### Organizing

Analytics like those available on GitHub work decently for showing project activity from a code perspective, and to a degree how many have contributed over time and very recently. But outside of looking at a single contributor's commit graph you don't usually know much about them specifically, at least not without starting some custom searches or queries. And commits don't show the whole story to begin with - plenty of project activity doesn't result in merged commits.

As a project maintainer I missed more of a contributor-centric view, and ended up developing that in Trello. I had a few goals:

* I don't want volunteers "slipping through the cracks" - whether that's a new would-be contributor being too shy to ask a follow-up question that would have helped them solve their first issue, or a longer term contributor that drifts off simply from not knowing what to work on
* I would like to be able to better direct people with questions to those who might have answers (such as trying to set up the game in Eclipse instead of IntelliJ)
* I want to pair volunteers with tasks we badly need help with, beyond simply pointing somebody at an issue tracker (which again doesn't necessarily cover all project activity), and have an easier overview of what everybody is working on. This should help with change notes as well

### Motivating

On the motivating side of things I'd like to maintain more insight in where individuals are at on a project journey, along with better having the ability to reward them with swag or positive reinforcement (badges, vanity titles, project roles) from time to time.

* For a new contributor we'd encourage them along a themed path matching their interests, toward a clearly marked goal where they can acquire some project swag for their efforts. Hardly an effective bribe for most contributors, but a cool little symbolic token :-)
* By better exposing and making named contributor roles visible I hope to make it more meaningful to commit to an on-going project role. If you take on this tiny responsibility you get to refer to yourself as an Earl of PR review! Imagine the CV possibilities!

### Trello Overview

For those unaware of what [Trello](https://trello.com) is: it is simply a fancy kanban board - a series of list columns filled with note cards. Sometimes that's used in agile development / Scrum, but you can use it for all kinds of things. I'm working with three primary boards these days.

* [Roadmap and Volunteers](https://trello.com/b/E6PfiSX8/roadmap-and-volunteers) - a public board used for three primary purposes, from left to right
  * On-going tasks: stuff we need to do regularly, and could use volunteers to take on as a committed role. Ex.: reviewing PRs or our social networks
  * Roadmap items: larger chunks of usually developer-focused work that's planned or being worked on by often more than one person. GSOC projects are included here when in season, including a series of simple checklists to track the process of each project
  * Released items: those larger chunks of stuff that have been completed, sometimes along with smaller stuff that have been grouped into a particular release.
* A "Contributors" board which is private and ideally would hold a card for every contributor or would-be contributor we've noticed. Goals for this board:
  * Linking a user with a different name in various systems (forum, github, chat, Trello itself) to a central point of reference
  * Indicate how far along a sort of "contributor journey" somebody is. This connects to a set of badges a user earns in the forum, makes sure we remember to add the person to the appropriate credits, and helps indicate whether somebody is ready for a larger role (such as GSOC mentoring)
  * Allow for note-taking both good and bad: this person is using Eclipse, is familiar with machine learning, usually available at a specific time / time zone ... or tends to get into trouble on chat
  * Let cards for users we haven't engaged with lately visibly change to indicate that we might want to check on those users
* Purpose specific boards like the one we have for GCI 2018, also private. This is where we stage tasks as part of the initial signup. We're trying to introduce automation to this approach this season
  * Cards in lists further on the left are draft tasks, not yet published to the GCI site. They're grouped into the 5 GCI task categories by list, indicating the _primary_ category (for balancing out available work)
  * Cards further on the right are published tasks, meant for or already on the GCI site. There are custom fields on the cards containing the info to be used on the GCI site, like each applicable category, whether something is a beginner task, number of days to complete, etc

## Automation Potential

Currently all of the above is done manually, and mostly by me, which does not exactly scale well. Goal is to automate as much of this as possible. Here are some initial ideas I hope we can work on:

* Automating the link from the GCI task cards to the GCI site. This would allow us to use the feature richness of Trello to manage our GCI tasks
* Integrating GitHub with Trello to automatically refresh contributor cards if they're actively doing project work (which means they're probably doing fine)
* Use the state of the contributor cards to automatically apply badges, add people to the credits, appropriate teams on GitHub, queue up swag requests, and so on
* Try to pull out some meaningful metrics, such as how many new contributors stay active for various amounts of time. Especially for GSOC students and well-performing GCI students, to have a clearer view of the retention rate for contributors entering through those programs

There is a lot more that could be done and more details could be written, but first we should get some basic work completed! :-)
