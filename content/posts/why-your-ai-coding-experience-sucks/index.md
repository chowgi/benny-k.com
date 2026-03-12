---
title: "Why Your AI Coding Experience Sucks (Probably)"
date: 2026-03-12T09:00:00+11:00
draft: true
tags: ["ai", "developer-tools", "claude-code", "productivity"]
description: "The AI coding divide isn't believers vs sceptics. It's people who get to choose their tools vs people who don't."
cover:
  image: "cover.png"
  relative: true
  alt: "Why your AI coding experience sucks"
---

A mate of mine is a real developer (and a good one). I've always dabbled with code, but I've never done it professionally and wouldn't consider myself a developer in the traditional sense. I've always wondered what actual developers think about coding agents. I was talking to him about all the incredible things I was doing, and he gave me a wry smile, saying, "Yeah, but that's toy code. You're not working on production code." He went on to explain that he's been using GitHub Copilot, and it's allowed him to do things probably two or three times faster in terms of code refactoring. Based on what I see and read, this seems really low, but I didn't push back. I was thinking about it the whole drive home though.

Then last week I was working with a customer. Let's call him Michael. Smart guy, genuinely trying to get value from AI tooling. He's got a Gemini plugin running in VS Code, which his organisation rolled out. He's struggling with it. Nothing dramatic. It just doesn't quite do what he wants. Half the time he ends up fighting the suggestions, having to copy in context manually from various sources. His conclusion is more or less the same as my mate's: AI coding is useful, yes, but a bit oversold.

I've started noticing this pattern everywhere.

I'm not a professional coder. But I am building a personal AI agent, several side projects, and half my presentations with Claude Code. I'm amazed by how much I can achieve. It's the most productive I've felt in years. We're not having the same experience at all.

After enough of these conversations, I think I've figured out why.

It comes down to the freedom to choose your AI tools. In both examples, they're having their tools dictated to them. I can choose the ones that I know produce vastly better outcomes.

So why are companies choosing these subpar tools?

Here's how it usually goes. Some executive reads a Gartner report or sees a competitor announce an AI initiative. The mandate comes down: we're doing AI. Procurement looks at what they already pay for (Microsoft Copilot, Google Gemini, whoever) and finds a box they can tick. GitHub Copilot's bundled with the enterprise GitHub agreement. Gemini comes with Google Workspace. Done.

Other times it comes down to "this is what the security team has told us we can use." Which, in enterprise settings like banking, I totally get.

There's a technical reason too. Tools like GitHub Copilot route every prompt through Microsoft's Azure proxy for pre-inference screening: safety checks, compliance filters, content exclusions, responsible AI guardrails. By the time your code reaches the model and comes back, it's been through several layers of corporate bubble wrap. The model underneath might be perfectly capable. You're just not getting the full thing.

Developers install it. Some like it. Most find it fine at best. The quality gap between "best AI coding tool available right now" and "the thing we licensed" can be enormous. Claude Code can navigate a full codebase, run tests, and iterate autonomously. Some enterprise tools are still doing glorified autocomplete.

Meanwhile, some random bloke is running fleets of agents with Claude Code and state-of-the-art models, shipping circles around them.

The problem isn't AI coding agents. We're there. They work. Anthropic and OpenAI have said publicly that a significant portion of their own code is now written by AI.

The issue is companies, arguably for good reasons, mandating which tools to use. And they're falling behind.

Amazon found this out the hard way.

Earlier this year, Amazon internally mandated a tool called Kiro for its engineering teams. Around 1,500 engineers pushed back. Their position was that Claude Code was better and they wanted to keep using it. Leadership overruled them.

What followed was rough. A 13-hour AWS outage. A 6-hour Amazon.com shopping outage. The tool those engineers were forced to adopt is now linked to a string of production incidents. I want to be careful here: outages are complicated, and I'm not saying Kiro caused all of it. But the timing is what it is, and the engineers who protested were apparently right about something.

That's the extreme version. Most companies won't have public outages. They'll just have developers who are quietly less productive than they could be, and a leadership team that thinks AI coding is working fine because the adoption dashboard is green.

I'm not objective here. I use Claude with Opus, I run it with full autonomy on my personal projects, and I think it's genuinely transformative. I've built a personal AI agent, shipped a bunch of conference presentations, and made real progress on a side project. All in the margins of a busy job with two young kids. Without the right tool, that doesn't happen.

So I'm probably biased. But I also talk to a lot of developers, and the pattern I keep seeing is real. The people who've had good AI coding experiences almost always chose their own tools. The people who are sceptical almost always had a tool chosen for them.

That's not a coincidence.
