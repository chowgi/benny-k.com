---
title: "Why Your AI Coding Experience Might Suck"
date: 2026-03-12T09:00:00+11:00
draft: false
tags: ["ai", "developer-tools", "claude-code", "productivity"]
description: "The AI coding divide isn't believers vs sceptics. It's people who get to choose their tools vs people who don't."
cover:
  image: "cover.png"
  relative: true
  alt: "Why your AI coding experience sucks"
---

Quick disclaimer: I've never been a professional developer, so take everything here with a grain of salt.

I've been noticing a pattern. I see people doing incredible things with the current generation of agentic coding tools on X/Twitter, and I'm doing things I have no business being able to achieve with my level of skill. But I started wondering — is this the norm? Turns out it isn't.

I recently caught up with a mate who's a real software engineer. I started talking about my demos, my side hustles, what I've been doing with Clawdia. He gave me a wry smile that said, "Yeah, but that's toy code. You're not working on production code." The discussion went on for a bit, then I asked him about his experience. He's been using GitHub Copilot, and it's let him do things probably two or three times faster for code refactoring. Based on what I experience and read, that seems really low. I didn't push back. I was thinking about it the whole drive home though.

Then last week I was working with a customer. Smart guy, genuinely trying to get value from AI tooling. He's got a Gemini plugin running in VS Code, which his organisation rolled out. He's struggling with it. Nothing dramatic. It just doesn't quite do what he wants. Half the time he ends up fighting the suggestions, copying in context manually from various sources. His take was similar: AI coding is useful, but oversold.

After enough of these conversations, I think I've worked out why.

It comes down to freedom of choice. In both cases, they're having tools dictated to them. I get to choose the ones that produce better outcomes.

Tools like GitHub Copilot route every prompt through Microsoft's Azure proxy for pre-inference screening: safety checks, compliance filters, content exclusions, responsible AI guardrails. By the time your code reaches the model and comes back, it's been through several layers of corporate bubble wrap. The model underneath might be perfectly capable. You're just not getting the full thing.

Gemini is a lot better than it used to be, but the latest Opus and Codex models still run rings around it. I'm sure that'll change, but for now, that's just reality.

So why are companies choosing these tools?

Here's how it normally goes. Some executive mandates "we're doing AI." Procurement looks at what they already pay for — Microsoft Copilot, Google Gemini, whoever — and finds a box they can tick. GitHub Copilot is bundled with the enterprise GitHub agreement. Gemini comes with Google Workspace. Done.

Other times it comes down to "this is what the security team has told us we can use." And look, I get it. These aren't stupid decisions. Enterprise security teams have real concerns: code leaking to third-party APIs, IP exposure, compliance with data residency requirements, the audit trail question. When you've got 5,000 developers and a regulated industry, "just let everyone use whatever they want" isn't really an option. The problem is that the tools which tick the security and procurement boxes aren't the ones producing the best results. There's a gap between what's approved and what's good, and right now that gap is massive.

The problem isn't AI coding agents. We're there. They work. Anthropic and OpenAI have said publicly that the vast majority of their code is now written by AI. OpenClaw, one of the fastest-growing open-source projects in recent memory, is 100% live-coded.

The issue is companies mandating which tools to use, and in doing so, kneecapping the experience.

Amazon found this out the hard way.

Recently, Amazon internally mandated a tool called Kiro for its engineering teams. Around 1,500 engineers pushed back. Their position was that Claude Code was better and they wanted to keep using it. Leadership overruled them.

What followed was rough. A 13-hour AWS outage. A 6-hour Amazon.com shopping outage. The tool those engineers were forced to adopt is now linked to a string of production incidents. I want to be careful here: outages are complicated, and I'm not saying Kiro caused all of it. But the timing is hard to ignore, and the engineers who protested were apparently right about something.

That's the extreme version. Most companies won't have public outages. They'll just have developers who are quietly less productive than they could be, and a leadership team that thinks AI coding is working fine because the adoption dashboard is green.

I'm not objective here. I use Claude with Opus, I run it with full autonomy on my personal projects, and I think it's genuinely transformative. I've built a personal AI agent, shipped a bunch of conference presentations, and made real progress on a side project. All in the margins of a busy job with two young kids. Without the right tool, none of that happens.

So I'm probably biased. But I also talk to a lot of developers, and the pattern keeps showing up. The people who've had good AI coding experiences almost always chose their own tools. The people who are sceptical almost always had a tool chosen for them.

That's not a coincidence.