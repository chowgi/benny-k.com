---
title: "Prompting is dead. It's all about loops."
date: 2026-06-19
draft: true
tags: ["cursor", "agents", "engineering", "ai-coding"]
description: "Everyone's talking about loop engineering. Cursor shipped it already."
---

Prompting is the new COBOL. If you're still hand-crafting perfect prompts and feeding pristine context like it's 2024, you're already ancient — at least according to the latest hype cycle started by Boris Cherny and Peter Steinberger.

"I don't prompt Claude anymore. I have loops that are running. They're the ones prompting Claude and figuring out what to do. My job is to write loops."

That's Boris Cherny. Creator of Claude Code. Peter Steinberger said the same thing to 2.2 million people: stop prompting your agents, design loops that prompt them for you.

It's a good take. And if you're using Cursor, you already have everything you need to actually do it. You've just been using it wrong.

## What a loop is

Four steps. Set a goal, act, check whether you're done, and if not, feed the error back and go again. You stop typing prompts. You write the system that types them. The model becomes a subroutine.

That's it. The shape is always the same — what changes is what you plug in at each step.

## The six parts, and what Cursor calls them

Every production loop needs six things. Here's where they live in Cursor.

**A trigger.** Something that starts the run without you pressing go — a schedule, a webhook, a PR label. Without this, you're not running a loop, you're just doing the same thing over and over by hand. Cursor calls this Automations: cloud agents that fire on a schedule or a lifecycle event.

**Isolation.** A private environment per agent so concurrent runs can't step on each other. Cursor's cloud agents spin up in isolated VMs. Each run gets its own sandbox.

**Written-down context.** The conventions, standards, and project rules the agent reads on every run. Skip this and the loop re-derives your entire project from scratch each pass and guesses at the gaps. That's Rules and Skills. They exist so the agent starts every session already knowing what it needs to know.

**Reach into your tools.** Connectors to your issue tracker, CI, database, chat. So the loop can open the PR, link the ticket, and post the result — instead of printing a suggested fix and waiting for you to carry it the rest of the way. That's MCP.

**A second agent checks.** A model reviewing its own work passes almost everything. You need an independent reviewer held apart from the one that wrote the code. That's BugBot.

**State on disk.** Something outside the conversation that remembers what's done and what's next. The model forgets between runs. The repo doesn't. Your Rules file doesn't. Your Skills don't.

Six parts. Cursor ships all of them.

## The cost insight everyone's missing

Most conversations about AI spend are still stuck on "which model, how many tokens." Inside a loop, that's the wrong question.

The real number is how many times the loop goes around. A cheap model that takes six attempts isn't cheaper than an expensive one that lands on the first pass — do the maths.

Cursor published the data. Composer 2.5 scores 63.2% on their benchmark at $0.55 per task. Opus 4.7 maxed out scores 64.8% — for $11.02. Twenty times the cost for less than two percentage points.

That gap is almost never worth it. Track cost per finished task. Everything else is noise.

## The most expensive bug you can ship

There's a line from a DAIR.AI piece on loop engineering that's stuck with me: "A weak verifier is the most expensive bug you can ship."

If the check that decides "done" is loose, the loop stops early on broken work, or grinds on work that was already fine. Both waste whole iterations. In a loop running unattended overnight, that compounds fast.

This is what BugBot actually is. Not a code reviewer bolted on for compliance — a verifier that decides whether the loop's output is trustworthy enough to ship. That's the component that makes everything else safe to run while you're not watching.

Tighten the verifier before you optimise anything else.

## The engineers who figure this out first

Every time someone talks about autonomous loops, someone else asks whether developers are still needed.

Wrong question. The loop writes faster than you can review. A loose check on a fast loop digs a hole quickly and quietly. The agents running while you sleep also make mistakes while you sleep.

What's actually shifting is where the hard, failure-prone work lives. It used to be writing the code. Now it's designing the loop — the triggers, the context, the verifiers, the stop conditions. That's still an engineering problem. It still requires judgement and taste and someone who knows the system well enough to catch it when it's wrong.

The engineers who get this first are going to pull a long way ahead of the ones still typing prompts.
