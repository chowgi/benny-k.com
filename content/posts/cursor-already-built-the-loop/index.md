---
title: "Prompting is dead. It's all about loops."
date: 2026-06-19
draft: true
tags: ["cursor", "agents", "engineering", "ai-coding"]
description: "Everyone's talking about loop engineering. Cursor shipped it already."
---

Prompting is the new COBOL. Still technically works. Still widely practiced. Ancient.

Boris Cherny — the guy who built Claude Code — said it straight: "I don't prompt Claude anymore. I have loops that are running. They're the ones prompting Claude and figuring out what to do. My job is to write loops."

Peter Steinberger said the same thing. 2.2 million people saw it. The point isn't subtle: the work has moved up a level. You don't write the code anymore. You write the system that writes the code.

Here's what nobody in that conversation is saying clearly enough: if you're already using Cursor, you have the infrastructure. All of it. You've just been thinking about it wrong.

## A loop is four steps

Set a goal. Act. Check whether you're done. If not, feed the error back and go again.

That's it. You stop typing prompts. The model becomes a subroutine your loop calls. What changes between a toy loop and a production one is what you plug in at each step.

## What those steps look like in Cursor

Every production loop needs six things. Here's what Cursor calls them.

**A trigger** — something that starts the run without you. A schedule, a webhook, a PR label. Without this you're not running a loop, you're just doing the same thing repeatedly by hand. Cursor calls this Automations.

**Isolation** — a private environment per agent so two concurrent runs can't overwrite each other. Cloud agents spin up in isolated VMs. Each run gets its own sandbox.

**Written-down context** — the conventions, standards, and project rules the agent reads on every run. Skip this and the loop re-derives your entire codebase from scratch each pass. That's Rules and Skills. They exist so the agent isn't guessing at your standards every single time.

**Tool access** — connectors to your issue tracker, CI, database, chat. So the loop opens the PR, links the ticket, and posts the result. Not prints a suggestion. Actually does the thing. That's MCP.

**An independent reviewer** — a model reviewing its own work will pass almost everything. You need a second agent held apart from the one that wrote the code. That's BugBot.

**State on disk** — something outside the conversation that remembers where things are up to. The model forgets between runs. The repo doesn't. Your Rules file doesn't. Your Skills don't.

Six parts. Cursor ships all of them. Most people are using maybe two.

## Everyone's optimising the wrong thing

The AI spend conversation is still stuck on "which model, how many tokens." Inside a loop, that's irrelevant.

The real variable is how many times the loop goes around. A cheap model that needs six attempts isn't cheaper than an expensive one that lands first pass — do the maths. Cost per call means nothing. Cost per finished task is everything.

Cursor published their benchmark data. Composer 2.5 scores 63.2% at $0.55 per task. Opus 4.7 maxed out scores 64.8% at $11.02 per task. Twenty times the cost for less than two percentage points.

That gap is almost never worth it. Most people are burning money on model selection when the real savings are in loop design.

## The most expensive bug you can ship

A DAIR.AI piece on loop engineering put it better than I could: "A weak verifier is the most expensive bug you can ship."

If the check that decides "done" is loose, the loop either stops early on broken work or grinds on work that was already fine. Both waste whole iterations. Running unattended overnight, that compounds fast and quietly.

This reframes what BugBot actually is. Not a code reviewer bolted on for compliance theatre. A verifier that decides whether the loop's output is trustworthy enough to keep going. The thing that makes it safe to close your laptop.

Get the verifier right before you touch anything else.

## Who wins from here

The "are developers still needed" question comes up every time anyone describes what autonomous loops can do.

It's the wrong question. The loop writes faster than you can review. A loose check on a fast loop digs a hole quietly. The agents running while you sleep also break things while you sleep.

The work isn't disappearing. It's moving. It used to be writing the code. Now it's designing the loop — the triggers, the context, the verifiers, the stop conditions, the budgets. That's still an engineering problem. It still requires someone who understands the system well enough to know when it's gone wrong.

The engineers who get this first will pull a long way ahead. The ones still hand-crafting prompts won't know what hit them.
