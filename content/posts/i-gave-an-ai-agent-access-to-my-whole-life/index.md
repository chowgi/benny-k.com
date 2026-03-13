---
title: "I Gave an AI Agent Access to My Whole Life. Here's What Five Weeks Looks Like."
date: 2026-03-11T16:00:00+11:00
draft: false
tags: ["openclaw", "ai", "agents"]
description: "Five weeks ago I set up a personal AI agent. It now runs my email, calendar, tasks, CRM, health tracking, and helped me build a side project. This is what actually happened."
cover:
  image: "cover.png"
  relative: true
  alt: "I gave an AI agent access to my whole life"
---

Five weeks ago I set up an AI agent called Clawdia using OpenClaw. I expected to play with it for a weekend and move on. She's still running. She's become the most useful tool I've ever used, and I don't think I could go back.

## What OpenClaw actually is

The TLDR is, OpenClaw is an open-source, self-hosted AI agent. You run it on your own machine, connect it to Telegram or WhatsApp, and talk to it like you'd message anyone. No dashboard. No app. Just a chat window.

It crossed 250,000 GitHub stars in about 3 months. Mac Minis sold out globally because people were buying them specifically to run this thing (which actually makes no sense, but I'll leave that for another post). That should tell you something about the demand.

You give the agent a name, a personality file, and a set of rules. This sounds like a gimmick. It isn't. I named mine Clawdia, wrote a personality file called `SOUL.md`, and immediately started caring about the setup more than I would have for some anonymous bot. I still don't fully understand why naming something changes how you treat it, but it does.

## What she actually does (mostly... I can't share all my secrets)

Every morning at 8am I get a Telegram message. Calendar for the day. Melbourne weather. My tasks sorted by priority. A high-protein breakfast suggestion, because I asked her to track that. A Stoic quote, because apparently that's who I am now. I also have Clawdia in a group chat with my wife because she has access to both my work (only free/busy) and my personal calendar. My wife just asks, "Hey, when has Ben got some availability to take Max for a haircut?" or "When can I book Ben his dentist appointment?" 

Clawdia will then give Cathy some free times. Cathy will say, "Yep, please book it in there," and Clawdia just does it.

But it's not just scheduling. I sent her a link to a YouTube video about kitchen lighting for our renovation. She watched it, extracted every tip, saved it to a file, and created a formatted Google Doc in my "Renovation 2026" Drive folder. Took about 90 seconds.

Last week I was building a presentation for presentation to the devs. I sent her the PDF. She reviewed every slide, told me my title was too salesy ("The Modern Transactional Standard", yeah, fair call), pointed out that one of my anti-pattern examples would contradict a project I'm doing with the same customer, and wrote me speaker notes. All of that feedback was right. The talk went well. I used some of her notes that were genuinely good and that I had not thought about.

Individually, none of these tasks are hard. A calendar check, a summarisation, a web search. But because Clawdia remembers every interaction, the usefulness compounds. She knows my accounts, my preferences, my projects, my wife's name. Five weeks of context makes every new request faster and more accurate than the last.

## She built a side project with me

I had an idea for an AI voice receptionist for tradies. The kind of bloke who misses calls because he's under a sink. Every missed call is a lost job.

I described what I wanted to Clawdia over Telegram. Over a weekend she helped me build Sam, an AI receptionist who answers the phone in a natural Australian accent, has a conversation with the caller, takes their details, and sends me a summary. She set up the Twilio integration, configured the voice agent, wrote the webhook server, and got a phone number live.

I'm not an engineer. I'm a Solutions Architect. I'm comfortable in a terminal but I'm no programmer. Clawdia handled the code. I handled the decisions. That division of labour is the thing nobody talks about enough.

## What went wrong

She told me to drink a glass of water "before bed" at 1pm on a Monday. More than once. The time awareness thing nearly drove me insane. Every message I send has a timestamp right there in the metadata. She just wouldn't read it. We've fixed it now. Probably.

She gave Cathy dates for availability two weeks in the past because she didn't bother to look up what the date was. But the beauty is, I asked her to fix it, so she did. I hope.

She hallucinated Todoist tasks that didn't exist. I asked for my task list and she gave me one from memory instead of actually querying Todoist. The tasks sounded plausible. They were completely made up. That's the dangerous kind of hallucination. Not obviously wrong, just subtly wrong enough that you might act on it.

I'm sure there's other things I can't remember right now, but every time she screws up we work on a fix, and she iteratively just gets better.

## What I actually learned

**It's super addictive.** What I didn't expect was how addictive and fun it is to work with a personal agent. There's something about the process of brainstorming and building something, seeing it work, seeing it break, making it better, that provides a constant dopamine hit. It's better than any of those other dopamine-inducing bad habits we have.

**Memory is everything.** Clawdia wakes up fresh every session. No continuity except what's in the files. `SOUL.md` is personality. `AGENTS.md` is how she operates. `USER.md` is context about me. `MEMORY.md` is accumulated knowledge. The quality of those files is the quality of the agent. It took me a couple of weeks to take that seriously.

**Research in, quality out.** She has a MongoDB knowledge base I call "The Vault." I throw everything into it. Articles, tweets, research. When I ask her to do something complex, the output quality correlates almost exactly with what I've put in.

**Trust is a cycle.** I'd delegate something. It'd go wrong. I'd pull back, add constraints, try again a bit further out. Five weeks in, I trust her with more than I did in week one. But I'm not at "runs everything while I sleep." Not quite.

**You can't outsource the thinking.** Aditya Agarwal, the ex-CTO of Dropbox, wrote about this last week. AI handles the routine stuff. What's left for you is the hard decisions. That's genuinely exhausting. I feel more mentally tired working with AI than I did before it existed. The routine tasks used to give my brain recovery time during the day. Now every hour is high-intensity decision making. The health tracking and protein goals aren't optional wellness stuff. They're infrastructure for the way I work now.

## The reality check

This costs real money. Claude Max is about $170 a month in Australia. The Pro tier won't cut it. You'll hit the rate limit in minutes.

You need to be comfortable with a terminal. It's not hard, but it's not nothing. If you've never SSH'd into a server, budget some learning time.

The security question is genuine. She runs shell commands. She reads files. She has API keys to my email, calendar, and task manager. That's a real attack surface. The guardrails in `AGENTS.md` aren't just for functionality. They're security rules. I tested them hard before I trusted them.

I'm not saying any of that to put people off. I'm saying it because if you go in expecting magic with no tradeoffs, you'll set it up badly and blame the tool.

## Five weeks in

Five weeks in, I'm not the same person who set this up. Not in some profound way. I just don't carry stuff in my head anymore. Tasks, follow-ups, meeting prep, the renovation, the side project. It's all somewhere that isn't my brain. That's it. That's the whole thing.

I thought AI agents were for engineers building startups. Turns out they're for anyone who's tired of keeping 50 tabs open in their head.

---

*OpenClaw on GitHub: [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)*
