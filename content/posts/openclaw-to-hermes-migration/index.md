---
title: "What I learned migrating my personal AI agent from OpenClaw to Hermes"
date: 2026-05-14
draft: true
tags: ["ai", "hermes", "openclaw", "personal-agent", "productivity"]
---

I have a personal AI agent. Her name is Clawdia, she runs on a spare MacBook, and she's genuinely useful — Todoist, YouTube summaries, contacts, MongoDB Atlas. I had grand plans to run local models but that's a different story. This week I migrated her from OpenClaw to Hermes. Here's what I found.

---

## Why migrate at all

OpenClaw worked. That's the honest answer. I wasn't fleeing a burning building.

But I'd noticed a few things that were starting to bother me. The memory system felt unreliable — I'd ask the assistant to remember something, it would say "noted!", and then the next session it was gone. The vault was treated as something you explicitly queried, not something the agent was aware of by default. I've written about these frustrations in previous posts, so I won't rehash them here.

I'd been keeping an eye on Hermes for a while but hadn't made the jump. What actually pushed me over the edge was a single comment on a LinkedIn post — someone pointed me at Hermes and said it had solved a lot of the memory problems I was describing. Not sure why that made me take the plung to be honest but it was enough.

---

## The setup thing

I saw a few comments while researching Hermes saying it was hard to configure. Fiddly, lots of moving parts, not for the faint-hearted. No "setup wizard like Openclaw".

I genuinely didn't find that.

My theory: those people were approaching it like it was 2023. Open the README, follow instructions line by line, manually edit YAML, copy-paste credentials, wonder why it doesn't work. That's the old way of doing this stuff.

in 2026 you open a Claude Code session, point it at the Hermes repo, and say "get me up and running — Telegram connected, MongoDB Atlas as the memory backend, sort out the config." Then went and made a coffee.

It sorted it out. Not magically — I had to clarify a few things, check some outputs, make some decisions. But the scaffolding, the config structure, figuring out which settings went where — all handled.

There are things an agent genuinely can't do for you. You need to create the Telegram bot yourself in BotFather and grab the token. You need to whitelist your server IP in Atlas. Anything that involves clicking around in a third-party console — that's yours. But that's twenty minutes, not a hard problem.

The rest? Let the agent figure it out.

The barrier to entry on tools like Hermes hasn't really dropped because the tools got simpler. It's dropped because the *way* you deal with complexity has changed. Pointing a coding agent at a repo and saying "help me set this up" works. It works well.

---

## What the migration actually looked like

Messier than expected, but not in a bad way.

There is a migration tool — `hermes claw migrate` — but from what I'd read it was flaky, and honestly I didn't want to use it anyway. Having built OpenClaw from scratch I knew exactly what I'd set up and why. Starting fresh and rebuilding with the knowledge gained from working with Openclaw turned out to be the right instinct. It forced a genuine audit of what was worth keeping.

There was a lot of plumbing specific to Openclaw — shell scripts wrapping Google Workspace, various one-off hacks that'd accumulated over months, tools and crons jobs I created and never really used. I rebuilt the stuff I used all the time in an hour or two, cleaner this time. 

For everything else I ran Openclaw in parallel  for a week or two, when I would ask something from Claudia that I relised  that I hadn't migrated I got Clawdia to explain the setup then gave it Claudia (I know confusing) to implment. Then I just did a final commit of Clawdia to her repo and gave Claudia access. I am still discovering the odd little useful thing that didn't come accross but access to the repo is enough for Claudia to figure it out. 

---

## The memory problem

This is the one that mattered most.

OpenClaw's memory architecture is built entirely around plain Markdown files in your workspace. `MEMORY.md` loads into every session. Daily logs (`memory/YYYY-MM-DD.md`) auto-load for today and yesterday. Your agent identity files — `SOUL.md`, `AGENTS.md`, `USER.md` — all inject at session start. The philosophy is explicit: *the model only remembers what gets saved to disk.* No hidden state.

In theory, that's clean. In practice, the weak link is that memory *writes* depend entirely on the LLM deciding to actually call the write tool. The official docs even acknowledge it: *"It helps to remind the model to store memories."* That's a polite way of saying passive memory is unreliable. I'd watch the agent say "noted!" and come back next session to find nothing had been written. You had to explicitly say "write this to MEMORY.md" to have any confidence it would actually stick.

Context compaction had its own issues. When the context window filled up, OpenClaw would summarise older messages — with a pre-compaction flush that was supposed to write important facts to the daily log first. It worked most of the time. But summaries are lossy: you lose the *reasoning* behind decisions, the dead ends you'd already explored, the conversational thread.

The vault — a separately queried knowledge store — had a different problem. It was on-demand only. You had to know what you'd stored to ask for it back. The agent didn't check it automatically; it was something you invoked, not something it drew on.

Hermes rethinks all of this. Memory is split into two stores — a user profile and a notes store — both frozen into the system prompt before the agent sees your first message every session. There's no recall step. The agent just knows. The character budgets are tight (around 2,200 characters for notes), which forces curation, but that's a feature: everything in there is something that genuinely needs to be available every turn.

The Vault still exists — MongoDB Atlas with hybrid semantic and lexical search — but its role is explicitly different. Large reference material: documents, contacts, research, conversation history. Too big for the system prompt, but searchable on demand. The agent queries it proactively when something seems relevant, rather than waiting to be asked.

What I noticed most in practice: the agent is considerably more disciplined about actually committing things to memory. With OpenClaw I had to be explicit every time. With Hermes it saves proactively — and critically, you can verify it. The memory store is directly inspectable. No "trust me I saved it." You can read exactly what's in there, edit it if it's wrong, and know with certainty what the agent will know next session.

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
