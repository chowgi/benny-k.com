---
title: "I Gave an AI Agent Access to My Whole Life. Here's What Five Weeks Looks Like."
date: 2026-03-11T16:00:00+11:00
draft: true
tags: ["openclaw", "ai", "agents"]
description: "Five weeks ago I set up a personal AI agent. It now runs my email, calendar, tasks, CRM, health tracking, and helped me build a side project. This is what actually happened."
cover:
  image: "cover.png"
  relative: true
  alt: "I gave an AI agent access to my whole life"
---

Last week I stood in front of 50 developers at a bank and asked who'd heard of vector embeddings. I got blank stares. A few polite nods from people who were clearly bluffing.

I spend too much time on X. In my feed, everyone's building autonomous agents, shipping production AI systems, and debating whether software engineers are already obsolete. I genuinely felt like I was falling behind. Then I looked at that room and realised I'd lapped most of them without noticing.

Five weeks ago I set up an AI agent called Clawdia using OpenClaw. I expected to play with it for a weekend and move on. She's still running. She's become the most useful tool I've ever used, and I don't think I could go back.

## What OpenClaw actually is

OpenClaw is an open-source, self-hosted AI agent. You run it on your own machine, connect it to Telegram or WhatsApp, and talk to it like you'd message anyone. No dashboard. No app. Just a chat window.

It crossed 250,000 GitHub stars in about four months. Mac Minis sold out globally because people were buying them specifically to run this thing. That should tell you something about the demand.

You give the agent a name, a personality file, and a set of rules. This sounds like a gimmick. It isn't. I named mine Clawdia, wrote a personality file called `SOUL.md`, and immediately started caring about the setup more than I would have for some anonymous bot. I still don't fully understand why naming something changes how you treat it, but it does.

## What she actually does

Every morning at 8am I get a Telegram message. Calendar for the day. Melbourne weather. My tasks sorted by priority. A high-protein breakfast suggestion, because I asked her to track that. A Stoic quote, because apparently that's who I am now.

None of that is groundbreaking. It's a calendar check, a weather API, and a query to my task manager. But I actually read it every morning. I never checked my calendar consistently before. The friction of opening four apps was apparently enough to stop me.

That pattern keeps repeating. Small friction removed, behaviour changed.

When someone sends me a customer email, Clawdia reads it, extracts the contacts, logs the interaction to a CRM she built in MongoDB, and archives the email. I BCC her on client emails and it just happens. No data entry. No forgetting.

I sent her a link to a YouTube video about kitchen lighting for our renovation. She watched it, extracted every tip, saved it to a file, and created a formatted Google Doc in my "Renovation 2026" Drive folder. Took about 90 seconds.

Last week I was building a presentation for a developer day at a bank. I sent her the PDF. She reviewed every slide, told me my title was too salesy ("The Modern Transactional Standard" — yeah, fair call), pointed out that one of my anti-pattern examples would contradict a project I'm doing with the same customer, and wrote me speaker notes. All of that feedback was right. The talk went well. I used most of her notes.

## She built a side project with me

I had an idea for an AI voice receptionist for tradies. The kind of bloke who misses calls because he's under a sink. Every missed call is a lost job.

I described what I wanted to Clawdia over Telegram. Over a weekend she helped me build Sam — an AI receptionist who answers the phone in a natural Australian accent, has a conversation with the caller, takes their details, and sends me a summary. She set up the Twilio integration, configured the voice agent, wrote the webhook server, and got a phone number live.

You can call +61 3 4062 9727 right now and talk to Sam. That number was set up five weeks ago. It's still running.

I'm not an engineer. I'm a Solutions Architect. I'm comfortable in a terminal but I'm not writing production Node.js from scratch. Clawdia handled the code. I handled the decisions. That division of labour is the thing nobody talks about enough.

## What went wrong

She told me to drink a glass of water "before bed" at 1pm on a Monday. More than once. The time awareness thing nearly drove me insane. Every message I send has a timestamp right there in the metadata. She just wouldn't read it. We've fixed it now. Probably.

She hallucinated Todoist tasks that didn't exist. I asked for my task list and she gave me one from memory instead of actually querying Todoist. The tasks sounded plausible. They were completely made up. That's the dangerous kind of hallucination — not obviously wrong, just subtly wrong enough that you might act on it.

She spammed my phone at 3am. She nearly deleted her own memory file. She told another user's group chat something she shouldn't have had access to.

Every one of those failures became a rule in her operating manual. I think of it as scar tissue. That document now has guardrails from real mistakes, and she's genuinely better for each one.

## What I actually learned

**Memory is everything.** Clawdia wakes up fresh every session. No continuity except what's in the files. `SOUL.md` is personality. `AGENTS.md` is how she operates. `USER.md` is context about me. `MEMORY.md` is accumulated knowledge. The quality of those files is the quality of the agent. It took me a couple of weeks to take that seriously.

**Research in, quality out.** She has a MongoDB knowledge base I call "The Vault." I throw everything into it — articles, tweets, meeting notes, research. When I ask her to do something complex, the output quality correlates almost exactly with what I've put in. Garbage in, garbage out. Still true. Still underappreciated.

**Trust is a cycle.** I'd delegate something. It'd go wrong. I'd pull back, add constraints, try again a bit further out. Five weeks in, I trust her with more than I did in week one. But I'm not at "runs everything while I sleep." Not quite.

**You can't outsource the thinking.** Aditya Agarwal, the ex-CTO of Dropbox, wrote about this last week. AI handles the routine stuff. What's left for you is the hard decisions. That's genuinely exhausting. I feel more mentally tired working with AI than I did before it existed. The routine tasks used to give my brain recovery time during the day. Now every hour is high-intensity decision making. The health tracking and protein goals aren't optional wellness stuff — they're infrastructure for the way I work now.

## The reality check

This costs real money. Claude Max is about $170 a month in Australia. The Pro tier won't cut it — you'll hit the rate limit in minutes.

You need to be comfortable with a terminal. It's not hard, but it's not nothing. If you've never SSH'd into a server, budget some learning time.

The security question is genuine. She runs shell commands. She reads files. She has API keys to my email, calendar, and task manager. That's a real attack surface. The guardrails in `AGENTS.md` aren't just for functionality — they're security rules. I tested them hard before I trusted them.

I'm not saying any of that to put people off. I'm saying it because if you go in expecting magic with no tradeoffs, you'll set it up badly and blame the tool.

## The gap is bigger than you think

Here's the thing that surprised me most. I assumed everyone technical was doing this stuff. Then I stood in front of 50 professional developers at a major Australian bank and most of them hadn't heard of embeddings. Not hobbyists. Professional developers. Building production systems.

The gap between people who are actually using AI tools and people who are reading about them isn't 10%. It's closer to 10x. And it's not a generational thing. I'm 45. The dividing line is whether you're the kind of person who tries things or the kind who waits for permission.

If you're reading this and thinking about setting it up: start with the `SOUL.md` and `AGENTS.md` files. That's where the agent actually lives. Most people install the software, write three lines in those files, and wonder why the agent feels generic. It's generic because you gave it nothing to work with.

Pick one job for it to own completely before adding more. Mine started with Todoist and email drafts. Once that was solid, I added the calendar, then the knowledge base, then the proactive stuff.

Write down every mistake it makes. That document becomes your operating manual, and it's worth more than any tutorial.

---

*OpenClaw on GitHub: [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)*
