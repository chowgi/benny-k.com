---
title: "What I learned migrating my personal AI agent from OpenClaw to Hermes"
date: 2026-05-14
draft: true
tags: ["ai", "hermes", "openclaw", "personal-agent", "productivity"]
---

I've been running a personal AI agent since the start of the year. It started on OpenClaw — a self-hosted setup with a customised assistant called Clawdia (yes, with a claw, because the mascot is a crab, and yes that made more sense at the time). It handled my Todoist tasks, summarised YouTube videos, managed contacts, talked to MongoDB Atlas, and generally made itself useful.

This week I've been migrating the whole thing to a [Hermes](https://github.com/NousResearch/hermes-agent)-based framework — an open-source agent project from Nous Research. This is what I found.

---

## Why migrate at all

OpenClaw worked. That's the honest answer. I wasn't fleeing a burning building.

But I'd noticed a few things that were starting to bother me. The memory system felt unreliable — I'd ask the assistant to remember something, it would say "noted!", and then the next session it was gone. The vault was treated as something you explicitly queried, not something the agent was aware of by default. I've written about these frustrations in previous posts, so I won't rehash them here.

I'd been keeping an eye on Hermes for a while but hadn't made the jump. What actually pushed me over the edge was a single comment on a LinkedIn post — someone pointed me at Hermes and said it had solved a lot of the memory problems I was describing. That was enough. I decided to take a proper look.

---

## What the migration actually looked like

Messier than expected, but not in a bad way.

There is a migration tool — `hermes claw migrate` — but from what I'd read it was flaky, and honestly I didn't want to use it anyway. Having built OpenClaw from scratch I knew exactly what I'd set up and why. Starting fresh felt like the right call — rebuild it with that knowledge, make better decisions this time, and ditch the stuff that had accumulated for no good reason.

That turned out to be the right instinct. It forced a genuine audit of what was worth keeping. The answer: not as much as I thought. A lot of the OpenClaw setup was plumbing specific to that environment — shell scripts wrapping Google Workspace, a Node.js CLI for YouTube transcripts, various one-off hacks that'd accumulated over months. Most of it got rebuilt from scratch in an hour or two, cleaner this time.

What did migrate directly:
- MongoDB Atlas credentials and the entire vault (documents, entities, contacts, conversation history)
- Google Workspace OAuth tokens (copied across, repointed to Hermes token paths)
- Todoist API token
- X API credentials
- Writing voice and style notes

What got rebuilt:
- All tool implementations (native Python this time, no CLI wrappers)
- Skills (Hermes has a first-class skill system; none of my OpenClaw skills translated directly)
- Cron jobs (different scheduling system entirely)

What got binned:
- A bunch of lifestyle cron jobs that I'd stopped actually reading anyway

---

## The memory problem

This is the one that mattered most.

In OpenClaw, memory worked roughly like this: the agent had a set of tools — `remember_this`, `recall`, `search_vault`. If something was worth saving, you'd either ask it to save, or it would offer to. If you wanted it to know something, you'd ask it to look it up. The vault was a database you queried on demand.

The fundamental problem with this model is that it relies on the agent correctly identifying what's worth remembering, and then actually executing the save. Those are two separate failure modes, and both happened regularly. I'd watch the agent say "I've saved that to memory" in a session, come back the next day, and it clearly hadn't. Or it had technically saved it, but in a format that was never surfaced again unless I specifically asked.

Hermes inverts this. Memory is injected directly into the system prompt on every turn. There's a user profile (who you are, your preferences, how you communicate) and a memory notes store (environment facts, tool quirks, project conventions). Both get loaded before the agent sees your first message. There's no "recall" step. The agent just... knows.

The Vault still exists — it's a MongoDB Atlas collection with semantic + lexical search — but its role is different. It's for large reference material: documents, contacts, research, conversation history. Stuff that's too big or too specific to live in the system prompt permanently. The agent searches it proactively when something seems relevant, or when you drop in a URL or photo.

In practice, this changes how the agent behaves quite a bit. I don't need to brief it at the start of every session. It already knows my timezone, my family, my work context, my communication preferences. It picks up where we left off without a preamble.

The other thing I noticed — and this one surprised me — is that the agent is considerably more disciplined about actually committing things to memory when it should. With OpenClaw I had to be explicit: "remember that", "save this". With Hermes, it saves proactively, without being asked, and you can verify it's actually happened because the memory store is directly inspectable. There's no magic, no "trust me I saved it" — you can read exactly what's in there.

---

## Hermes is infrastructure, not a product

OpenClaw felt more like a product. It had opinions about how your agent should work. The tools were curated, the memory model was built-in, the personality system was part of the framework.

Hermes feels more like infrastructure. The agent is a conversation loop. Skills are markdown files with tool call instructions. Memory is a pluggable provider. Tools are Python modules that register themselves. The default setup is intentionally minimal.

That's a genuine trade-off. If you want something that works out of the box without much fiddling, Hermes will frustrate you initially. There's real work involved in getting it set up the way you want. I spent a solid session rebuilding my Todoist integration, another getting the MongoDB Atlas memory plugin right, and a good chunk of time migrating Google Workspace credentials and testing each scope.

But the flip side is that when something goes wrong, you know where to look. The code is there. The config is readable YAML. The skills are markdown you can edit in any text editor. It doesn't abstract you away from the mechanics.

---

## What still needs work

No point pretending it's all been perfect.

**Cron timezone handling** is annoying. Hermes schedules everything in UTC, which is correct, but it means every time I set up a cron job for Melbourne time I need to mentally convert — and then remember that AEST and AEDT are different offsets and the schedule will drift an hour seasonally. A per-job timezone setting would fix this, and I'd happily submit a PR if I can find the time.

**Browser automation on ARM64** was a headache. The agent-browser tool that Hermes uses for interactive web sessions couldn't get Chromium running on this EC2 instance — ARM64, no display, snap packages fighting with the headless setup. I ended up installing Playwright directly and writing a wrapper. It works, but it's not integrated into the normal browser toolset. I've since removed the Chromium install entirely and am working around it.

**The skill system takes some getting used to.** Skills are powerful — essentially persistent instruction sets that load into specific sessions — but writing a good skill requires thinking carefully about when it should load, what it should contain, and how it should interact with the agent's other context. I've written probably a dozen skills now, and the first few were too vague to be useful.

---

## Would I do it again

Yes. Quickly.

The memory system alone is worth it. Not having to re-brief the agent every session, not watching things silently fail to save, not having to explicitly query the vault for things the agent should already know — that's a meaningful quality-of-life improvement for daily use.

The infrastructure-over-product philosophy is a good fit for how I want to work. I'd rather understand what's happening under the hood and fix it when it breaks than be dependent on a black box that mostly works.

The migration itself took maybe two full sessions of solid work. The messy bits were mostly specific to my setup — two machines, manually transferred credentials, a bunch of accumulated hacks that needed untangling anyway. If you're starting fresh, or migrating from a simpler OpenClaw setup, it'd be faster.

One last thing. I renamed the assistant. She's Claudia now. The claw pun doesn't quite land on a framework called Hermes.

---

*Hermes Agent is open source: [github.com/NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)*
