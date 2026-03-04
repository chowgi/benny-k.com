---
title: "Your AI Agent is Only as Good as What You Feed It"
date: 2026-03-05
tags: [ai, agents, mongodb, architecture]
draft: true
slug: "your-ai-agent-is-only-as-good-as-what-you-feed-it"
---

I've been running a personal AI agent for about three months now. Not a chatbot, not a Copilot autocomplete — an actual autonomous agent that reads my email, manages my CRM, builds presentations, and occasionally tells me to go to bed at 3pm because it got the timezone wrong.

Along the way I've learned something that none of the "build your first agent" tutorials mention: the model barely matters.

## The loop is boring. The context is everything.

Every production agent — Claude Code, Cursor, Manus, Devin — runs the same basic loop:

```
while (model returns tool call):
    execute tool → capture result → append to context → call model again
```

That's it. The model decides what to do. The context decides how well it does it.

There's a stat I keep coming back to: Claude Opus 4.5 scores 42% on a benchmark with one scaffold and 78% with another. Same model. Same test. The only variable was what information the model had access to and when.

## What happens when your agent needs customer data

Here's where it gets real. Say your agent needs to handle a customer query. It needs their account details, recent support tickets, billing status, KYC check, and product holdings.

Most agent architectures handle this with tool calls. The agent calls the CRM API, then the billing API, then the support system, then the compliance check. That's four or five round trips through the agent loop. Each one burns tokens, adds latency, and forces the model to process another chunk of raw API response.

I measured it on my own setup. Five tool calls to assemble context: roughly 12,000 tokens consumed and 7-16 seconds of latency before the agent even starts thinking about the actual question. And the kicker — the model decides what to fetch and in what order. Every time. Non-deterministically. Sometimes it skips a system. Sometimes it calls them in the wrong sequence. Sometimes it just gets confused by the response format.

## There's a concept called the 40% rule

Research from the "12 Factor Agents" methodology puts a number on it: push past 40% of a model's context window and you enter what they call the "dumb zone." Signal-to-noise degrades, attention fragments, and the agent starts making mistakes that look like reasoning failures but are really just information overload.

12,000 tokens on context assembly before the agent even starts working? You're eating into that budget fast.

## What if the agent got everything in one read?

This is the bit that seems obvious in hindsight. Instead of the agent deciding what to fetch from five different systems, you pre-assemble the context it needs. Deterministically. In real time. Into a single document.

One database read. Complete customer context. About 2,000 tokens. Under 50 milliseconds.

The data assembly is your code — CDC pipelines, event streams, aggregation. The model does what it's actually good at: reasoning and responding. Not data plumbing.

## This isn't a new pattern

If you've worked in enterprise data for any length of time, you'll recognise this immediately. It's an Operational Data Layer. Banks have been building these for decades to stop their digital channels from hammering the mainframe directly.

Same architecture. Same value. The only difference is the consumer. Instead of a web app or mobile app reading from the ODL, it's an AI agent.

I find that framing useful because it means organisations that have already built an operational data layer for their digital channels are halfway there for agents. The pipes exist. The aggregation logic exists. You just need to shape the output for a different consumer.

## The numbers

Here's the before and after from my setup:

Without the ODL: 5 tool calls, roughly 12,000 tokens for context assembly, 7-16 seconds latency, non-deterministic results.

With it: 1 read, about 2,000 tokens, under 50 milliseconds, deterministic every time.

At any kind of scale — say 100,000 customer interactions a month — that's roughly a billion tokens saved. That's not an abstract efficiency gain. That's actual dollars.

## What I actually think

Look, I'm not saying every agent needs a dedicated data layer from day one. If you're building a prototype or a demo, just let the agent call your APIs directly. It'll work fine.

But if you're building something that needs to handle real traffic, with real customers, where reliability and cost actually matter — the agent-as-data-plumber pattern falls apart fast. The model is the engine. The context is the fuel. Put in cheap fuel and don't be surprised when performance is rubbish.

The organisations that figure this out early will build agents that actually work. The rest will spend six months wondering why theirs keeps hallucinating customer details and burning through the token budget.

Give the agent a curated view and it's brilliant. Give it raw access to six APIs and watch it struggle. I've run both. Not even close.
