---
title: "Why OpenClaw Broke the Internet"
date: 2026-03-05T16:00:00+11:00
draft: false
tags: ["openclaw", "ai", "agents", "presentation"]
description: "I gave a talk at MongoDB about OpenClaw and the AI agent I've been running for months. Here's what I covered — and what I actually learned."
cover:
  image: "cover.png"
  relative: true
  alt: "Why OpenClaw Broke the Internet"
---

I gave a Lunch & Learn at MongoDB last week on OpenClaw. It was well attended, but what was great was people coming to terms with the absurd detail that the slide deck I was presenting was one-shotted from a prompt. I sent a single Telegram message to Clawdia (my OpenClaw assistant), went and made a coffee, and came back to a complete Reveal.js presentation sitting in my Google Drive. That's not a demo I staged for the talk. That's just what happened.

You can [view the actual slide deck here](/presentations/openclaw-lunch-and-learn.html).

---

## So what is OpenClaw?

It's an open-source framework that lets you run an AI agent on your own computer. The agent connects to chat apps you already use — WhatsApp, Telegram, Slack — so you interact with it the same way you'd message anyone. No new app, no dashboard to check.

The project was built by one person — Peter Steinberger, the iOS developer behind Swipe keyboard. It's cleared 235,000 GitHub stars. Hit 100,000 in under 30 days, which makes it the fastest-growing repository in GitHub history. At some point during that run, Apple Mac Minis — the preferred hardware to host it on — sold out globally for six weeks. People are genuinely losing their minds.

---

## Why did it go viral?

I spent some time on this in the talk because I think the obvious answer ("AI is hot right now") misses what actually happened.

A few things came together at once.

The models finally crossed a threshold where they're good enough to run autonomously without constant babysitting. Opus 4.6, Codex 5.3 — these aren't "impressive demo" models anymore. They're "actually does useful work" models.

The interface decision was smart. Talking to your agent via WhatsApp or Telegram sounds trivial, but it matters enormously. Most AI tools fail because you have to go somewhere special to use them. OpenClaw goes where you already are.

Then there's the self-modifying software angle. An AI agent that can edit its own source code, write its own tools, and extend its own capabilities. That breaks something in your brain a bit, honestly.

And finally — the `SOUL.md` file. You give the agent a name, a personality, a set of values. It sounds gimmicky until you realise that naming the thing makes you take it seriously. You write better guardrails. You invest in making it good at its job.

---

## What does Clawdia actually do for me?

The honest answer is: more than I expected, and in ways I didn't anticipate.

Life admin that would otherwise rot in my brain: Todoist tasks, warranty claims, email drafts. I describe something in a message, it creates a draft or a task or a reminder. I've found myself just talking at it the way you'd vent to a colleague, and it handles things.

It built this presentation. It writes Google Docs. It maintains a vector search knowledge base (MongoDB Atlas, obviously) where I can throw articles, notes, and transcripts and search them later. I now have a second brain that I can actually find things in.

It runs on a schedule too. Morning briefing, meal suggestions for the kids, bin day reminder, weekly research summaries. None of that needed a single line of code from me. I just described what I wanted.

The compounding effect surprised me. By week four, it was catching things before I'd thought about them.

---

## What I've actually learned

This is the part I spent the most time on in the talk, because the lessons from actually running this for months are different from what you'd expect.

**Do the research first.** My agent has a MongoDB second brain that I throw everything into before building something. The quality of what comes out correlates almost exactly with the quality of what went in. Garbage in, garbage out — still true, still underappreciated.

**It will break things.** Clawdia has hallucinated tasks that didn't exist, spammed my phone at 3am, and nearly deleted its own memory file. Each failure became a rule in `AGENTS.md`. I think of it as scar tissue. The document now has guardrails that came from real mistakes, and the agent is meaningfully better for it.

**Trust is a cycle, not a switch.** I'd delegate something, it'd go wrong, I'd pull back and add constraints, then delegate again further. I'm further along that cycle now than I was in week one, but I'm not at "run everything autonomously while I sleep." Not quite.

**Memory is everything.** The agent wakes up fresh every session. It has no continuity except what's in the files. I've spent real time on those files. `SOUL.md` is personality. `AGENTS.md` is how it operates. `USER.md` is context about me. `MEMORY.md` is accumulated knowledge. The quality of those files is the quality of the agent. It took me a while to take that seriously.

**Naming it mattered.** I don't fully understand why, but having a named agent with a personality made me invest differently in the setup. I wrote better constraints. I thought harder about what I actually wanted it to do. Maybe it just forces you to be specific.

---

## The reality check

This costs real money. You need a Claude Max subscription (about $170 aussie). Pro won't cut it. You'll burn through the 5-hour rate limit in about 5 minutes.

You need to be comfortable with a terminal and Docker. It's not hard, but it's not nothing. If you've never run a container before, budget some time.

And the security question is genuine. The agent runs shell commands. It reads files. It has API keys. That's a real attack surface. The guardrails in `AGENTS.md` aren't just for functionality — they're actual security rules. You have to think about this.

I'm not saying any of that to put people off. I'm saying it because if you go in expecting magic with no tradeoffs, you'll set it up wrong and it'll disappoint you.

---

## If you're thinking about trying it

Start with the `SOUL.md` and `AGENTS.md` files. That's where the agent actually lives. Most people install the software and then write three lines in those files and wonder why the agent feels generic. It's generic because you gave it nothing to work with.

Pick one job for it to own completely before adding more. Mine started with Todoist and email drafts. Once that was solid, I added the calendar, then the second brain, then the proactive stuff.

And write down every mistake it makes. That document becomes your operating manual, and it's worth more than any tutorial.

---

*OpenClaw on GitHub: [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)*
