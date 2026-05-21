---
title: "What I learned migrating my personal AI agent from OpenClaw to Hermes"
date: 2026-05-14
draft: true
tags: ["ai", "hermes", "openclaw", "personal-agent", "productivity", "mongodb"]
---

Clawdia is now Claudia. The "Claw" pun stopped making sense when I moved her off OpenClaw onto Hermes. That's the least interesting thing that happened.

---

## Why migrate at all

OpenClaw worked. That's the honest answer. I wasn't fleeing a burning building.

But I'd noticed a few things that were starting to bother me. The memory system felt unreliable — I'd ask the assistant to remember something, it would say "noted!", and then the next session it was gone. The vault was treated as something you explicitly queried, not something the agent was aware of by default. I've written about these frustrations in previous posts, so I won't rehash them here.

I'd been keeping an eye on Hermes for a while but hadn't made the jump. What actually pushed me over the edge was a single comment on a LinkedIn post — someone I don't know pointed me at Hermes and said it had solved a lot of the memory problems I was describing. Not sure why that was enough, but it was.

---

## The setup thing

I saw a few comments while researching Hermes saying it was hard to configure. Fiddly, lots of moving parts, not for the faint-hearted. No setup wizard like OpenClaw.

I genuinely didn't find that.

My theory: those people were approaching it like it was 2023. Open the README, follow instructions line by line, manually edit YAML, copy-paste credentials, wonder why it doesn't work. That's the old way of doing this stuff.

In 2026 you open a Claude Code session, point it at the repo, and say "get me up and running — deploy an EC2 instance, connect Telegram, set up MongoDB Atlas as the memory backend, sort out the config." Then go and make a coffee.

It sorted it out. Not magically — I had to clarify a few things, check some outputs, make some decisions. But the scaffolding, the config structure, figuring out which settings went where — all handled.

There are things an agent genuinely can't do for you. You need to create the Telegram bot yourself in BotFather and grab the token. You need to whitelist your server IP in Atlas. Anything that involves clicking around in a third-party console — that's yours. But that's 10 minutes, not a hard problem.

The rest? Let the Claude, Codex, or Cursor-based agent figure it out.

Most importantly, the final step was renaming the agent to Claudia because the "Claw" pun no longer worked. I know that's lame. I'm not going to argue it's not.

---

## What the migration actually looked like

Messier than expected, but not in a bad way.

There is a migration tool — `hermes claw migrate` — but from what I'd read it was flaky, and honestly I didn't want to use it anyway. Having built OpenClaw from scratch I knew exactly what I'd set up and why. Starting fresh and rebuilding with that knowledge turned out to be the right instinct. It forced a genuine audit of what was worth keeping.

There was a lot of plumbing specific to OpenClaw — shell scripts wrapping Google Workspace, various one-off hacks that'd accumulated over months, tools and cron jobs I'd created and never really used. I rebuilt the stuff I actually used in an hour or two, cleaner this time.

For everything else I ran OpenClaw in parallel for a week or two. When I asked Claudia for something I realised hadn't been migrated, I'd get her to explain the setup, then hand that to Claude Code to implement in the Hermes config. Eventually I did a final commit and gave Claude Code access to the old repo. I then shut down OpenClaw for good. I'm still occasionally discovering something that didn't make the trip — but having access to the repo is enough for Claudia to figure it out.

---

## The memory problem

This is the one that mattered most.

OpenClaw's memory architecture is built entirely around plain Markdown files. `MEMORY.md` loads into every session. Daily logs auto-load for today and yesterday. Your agent identity files — `SOUL.md`, `AGENTS.md`, `USER.md` — all inject at session start. The philosophy is explicit: *the model only remembers what gets saved to disk.*

In theory, that's clean. In practice, the weak link is that memory writes depend entirely on the LLM deciding to actually call the write tool. The official docs even acknowledge it: *"It helps to remind the model to store memories."* That's a polite way of saying passive memory is unreliable. I'd watch it say "noted!" and come back next session to find nothing had been written.

Also — and I'll be honest about this — I work at MongoDB as a Solutions Architect. Storing long-term knowledge in flat Markdown files felt deeply wrong. Like using a spreadsheet to track inventory and just... accepting it.

Hermes ships with a pluggable memory provider system, and the default is a lightweight in-process store backed by SQLite. Better than Markdown. Still felt wrong.

So I built a plugin.

The Vault is MongoDB Atlas. Five collections: `documents` for general knowledge and research, `entities` for people and companies, `contacts`, `conversation_history`, and `interactions`. Embeddings are generated via Voyage AI (voyage-3-large, 512 dimensions). Every document that goes in gets a vector embedding alongside it.

Search is hybrid. When Claudia queries the Vault, she runs two pipelines in parallel — Atlas Full Text Search (`$search`) for lexical matching and `$vectorSearch` for semantic similarity — then merges the results using Reciprocal Rank Fusion. RRF gives each document a combined score based on its rank in both lists, so something that scores well in both tends to surface at the top. There's also time-decay weighting, so recent documents score slightly higher than old ones for the same relevance.

The honest footnote: `$rankFusion` is natively supported in Atlas but requires a higher cluster tier than I'm currently on. Right now the RRF merge happens in Python after two separate round trips to Atlas. It works, but it's not as clean as it should be. I've got a Todoist task to upgrade the cluster.

The result is a long-term memory system that actually behaves like one. Claudia queries it proactively before answering anything substantive. She doesn't wait to be asked — if your question looks related to something in the Vault, it surfaces automatically. I didn't build that behaviour in explicitly. The system prompt just says the Vault exists and when to use it. She figured out the rest.

The plugin is something I'd like to clean up and submit as a PR to Hermes. Right now it's hardcoded to my setup in a few places. Making it properly generic — configurable database names, swappable embedding providers, a real schema definition — is probably two or three hours of work. When the cluster's upgraded and `$rankFusion` is in play, it'll be worth doing properly.

---

## Would I do it again

Yes. Quickly.

The memory system alone is worth it. Not having to re-brief the agent every session, not watching things silently fail to save, not having to explicitly query the vault for things the agent should already know — that's a meaningful quality-of-life improvement for daily use.

One other thing that sounds minor but isn't: you can interrupt Hermes mid-task. With OpenClaw, if you thought of something while it was working, you were stuck. You couldn't insert it at the right moment without derailing the whole conversation — you'd either lose the thought or shove it in at the wrong point and make a mess. Hermes handles interruptions cleanly. The flow stays coherent.

The infrastructure-over-product philosophy is a good fit for how I want to work. I'd rather understand what's happening under the hood and fix it when it breaks than be dependent on a black box that mostly works.

The migration itself took maybe two full sessions of solid work. The messy bits were mostly specific to my setup — two machines, manually transferred credentials, a bunch of accumulated hacks that needed untangling anyway. If you're starting fresh, it'd be faster.

One last thing. I renamed the assistant. She's Claudia now. The claw pun doesn't quite land on a framework called Hermes.

---

*Hermes Agent is open source: [github.com/NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)*
