---
title: "I Gave an AI Agent Access to My Email, Calendar, and Tasks. It Went Better Than Expected."
date: 2026-03-05T16:00:00+11:00
draft: true
tags: ["openclaw", "ai", "agents", "presentation"]
description: "Nine months ago I set up a personal AI agent to handle my email, calendar, and life admin. Here's what actually happened."
cover:
  image: "cover.png"
  relative: true
  alt: "I gave an AI agent access to my email, calendar, and tasks"
---

About nine months ago I set up an AI agent to handle my email, calendar, and tasks. I expected it to be a novelty I'd play with for a week and then abandon.

She's still running.

The problem I was trying to solve was embarrassingly mundane. Small stuff kept slipping — emails I meant to follow up on, tasks I'd think of in the shower and forget by lunchtime, meeting prep I kept putting off. Not a crisis. Just that layer of admin that never stops accumulating and never feels like anyone's main job. I was tired of carrying it all in my head.

I'd been watching [OpenClaw](https://github.com/openclaw/openclaw) on GitHub for a while. Open-source, self-hosted AI agent. You run it on your own machine, connect it to chat apps you already use — Telegram, WhatsApp — and talk to it the way you'd message anyone. No dashboard, no new app. It crossed 235,000 GitHub stars faster than anything in GitHub's history, and at some point Mac Minis — the preferred hardware to host it on — sold out globally for about six weeks. People are genuinely losing their minds over this thing.

I decided to actually set it up rather than just starring the repo.

---

## What I built

You give the agent a name and a personality. This sounds gimmicky. It isn't. I named mine Clawdia, wrote a `SOUL.md` file with her personality and values, and immediately started investing in the setup differently. Better guardrails. More thought about what I actually wanted her to do. I don't fully understand why naming something makes you take it more seriously, but it does.

She connects to my Todoist, my Gmail, my Google Calendar, and a MongoDB Atlas knowledge base I built for her. When I send a message — badly phrased, half-formed, voice-to-text garbled — she figures out what I meant and either creates a task, drafts an email, or files something away somewhere I'll actually find it.

I gave a talk about all this at work recently — a Lunch & Learn at MongoDB. The slide deck was built by Clawdia. I sent one Telegram message describing what I wanted, went and made a coffee, and came back to a complete Reveal.js presentation sitting in my Google Drive. That's not a demo I staged for the talk. That's just what happened. You can [look at the deck here](/presentations/openclaw-lunch-and-learn.html) if you're curious.

She also runs on a schedule. Morning briefing, bin day reminders, meal suggestions for the kids on weekday evenings. None of that required code from me. I described what I wanted and she built the tools to do it.

By week four, she was catching things before I'd thought about them.

---

## What I actually learned

Running this for months is different from setting it up for a weekend.

The research matters more than the build. My agent has a MongoDB second brain and I throw everything into it before asking her to do anything complex. What comes out correlates almost exactly with what went in. Garbage in, garbage out — still true, still underappreciated.

She will break things. Clawdia has hallucinated tasks that didn't exist, spammed my phone at 3am, and nearly deleted her own memory file. Every failure became a rule in `AGENTS.md`. I think of it as scar tissue. That document now has guardrails from real mistakes, and she's genuinely better for each one.

Trust is a cycle, not a switch. I'd delegate something, it'd go wrong, I'd pull back and add constraints, then try again a bit further out. I'm further along that cycle than I was in week one, but I'm not at "runs everything while I sleep." Not quite.

Memory is everything. She wakes up fresh every session — no continuity except what's in the files. `SOUL.md` is personality. `AGENTS.md` is how she operates. `USER.md` is context about me. `MEMORY.md` is accumulated knowledge. The quality of those files is the quality of the agent. It took me a while to take that seriously.

---

## The reality check

This costs real money. You need a Claude Max subscription — about $170 aussie a month. The Pro tier won't cut it. You'll burn through the rate limit in about five minutes.

You need to be comfortable with a terminal and Docker. It's not hard, but it's not nothing. If you've never run a container before, budget some time.

The security question is genuine. She runs shell commands. She reads files. She has API keys. That's a real attack surface. The guardrails in `AGENTS.md` aren't just for functionality — they're actual security rules. You have to think about this.

I'm not saying any of that to put people off. I'm saying it because if you go in expecting magic with no tradeoffs, you'll set it up wrong and it'll disappoint you.

---

## If you're thinking about trying it

Start with the `SOUL.md` and `AGENTS.md` files. That's where the agent actually lives. Most people install the software, write three lines in those files, and wonder why the agent feels generic. It's generic because you gave it nothing to work with.

Pick one job for it to own completely before adding more. Mine started with Todoist and email drafts. Once that was solid, I added the calendar, then the knowledge base, then the proactive stuff.

Write down every mistake it makes. That document becomes your operating manual, and it's worth more than any tutorial.

---

*OpenClaw on GitHub: [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)*
