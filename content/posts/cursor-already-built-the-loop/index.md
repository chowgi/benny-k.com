---
title: "Cursor already built the loop"
date: 2026-06-19
draft: true
tags: ["cursor", "agents", "engineering", "ai-coding"]
description: "Everyone's talking about loop engineering. Cursor shipped it already."
---

"I don't prompt Claude anymore. I have loops that are running. They're the ones prompting Claude and figuring out what to do. My job is to write loops."

That's Boris Cherny. Creator of Claude Code. He said it a couple of weeks ago and it's been bouncing around AI circles ever since. Peter Steinberger put the same point to 2.2 million people: stop prompting your agents, design loops that prompt them for you.

Good take. Here's the thing though — if you're using Cursor, you already have the infrastructure. You just might not have connected the dots yet.

## What a loop actually is

Strip away the hype and a loop is four steps: set a goal, act, check whether you're done, and if not, feed the error back and go again. You stop typing prompts. You write the system that types them. The model becomes a subroutine.

The shape is always the same. What changes is what you plug in at each step.

## The six parts, mapped to Cursor

Every production loop is built from the same six components. Here's what each one is — and what Cursor calls it.

**A trigger.** Something that starts the run without you pressing go. A schedule, a webhook, a PR label. Without this, you're not running a loop, you're just repeating yourself by hand. In Cursor, this is Automations — cloud agents that fire on a schedule or a lifecycle event.

**Isolation.** A private environment per agent so two concurrent runs can't step on each other. Cursor's cloud agents spin up in isolated VMs. Each run gets its own sandbox.

**Written-down context.** The conventions, standards, and project-specific rules the agent reads on every run. Skip this and the loop re-derives your project from scratch each pass. In Cursor, this is Rules and Skills. They exist precisely so the agent starts every session with what it needs to know.

**Reach into your tools.** Connectors to your issue tracker, CI, database, and chat. So the agent can open the PR, link the ticket, and post the result — instead of printing a fix and waiting for you to carry it the rest of the way. That's MCP.

**A second agent checks.** A separate worker that grades the output. A model reviewing its own work passes almost everything. You need an independent reviewer. That's BugBot — held apart from the agent that wrote the code, reviewing every PR.

**State on disk.** Anything outside the conversation that records what's done and what's next. The model forgets between runs. The repo, the Rules file, the Skills — these don't.

Six parts. Cursor ships all of them.

## Iterations are the budget line, not tokens

Here's where it gets interesting from a cost perspective.

Most conversations about AI spend are stuck on "which model, how many tokens." Inside a loop, that's the wrong question. The spend is how many times the loop goes around. A cheaper model that loops twice as often isn't cheaper — it's the same or worse.

Cursor published their own data on this. Composer 2.5 clocks 63.2% on their benchmark at $0.55 per task. Opus 4.7 maxed out scores 64.8% — for $11.02 per task. Twenty times the cost for less than two percentage points of improvement.

That gap is almost never worth it at the task level. But the more important insight is that cost per task is what matters, not cost per call. A loop that lands on the first pass is cheaper than a cheap model that takes six attempts, regardless of how you price the tokens.

Track cost per finished task. Everything else is noise.

## A weak verifier is the most expensive bug you can ship

There's a line from the DAIR.AI piece on loop engineering that stuck with me: "A weak verifier is the most expensive bug you can ship."

If the check that decides "done" is loose, the loop either stops early on broken work or grinds on work that was already fine. Both waste whole iterations. In a loop running unattended, that multiplies fast.

This is the BugBot story reframed. You're not adding a code reviewer. You're tightening the verifier that decides whether the loop's output is trustworthy. That's the component that makes everything else safe to run while you're not watching.

Fix the verifier before you optimise anything else.

## The engineer who designs the loop matters more, not less

Every time someone describes what's possible with autonomous agent loops, someone else asks whether developers are going to be needed at all.

Wrong question. The loop writes faster than you can review. A weak check on a tight loop digs a hole fast and quietly. The agents that run while you sleep also make mistakes while you sleep.

What's actually happening is that the expensive, failure-prone part of the system is moving. It used to be writing the code. Now it's designing the loop — the triggers, the context, the verifiers, the stop conditions. That's an engineering problem. It requires taste, judgement, and someone who understands the system well enough to know when it's wrong.

The engineers who figure this out first are going to pull a long way ahead of the ones still typing prompts.
