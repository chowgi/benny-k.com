---
title: "The Learning Is the Collateral"
date: 2026-07-02
draft: false
tags: ["ai", "engineering", "cursor", "shopify"]
cover:
  image: cover.png
  relative: true
  alt: "A human figure casting a shadow shaped like a lightbulb and code — learning as the output"
---

Farhan Thawar, head of engineering at Shopify, gave an excellent talk at the recent Cursor Compile event. A few things in particular stuck with me.

The first takeaway was "the learning is the collateral, not the code." He told a story about 50 engineers who spent 18 months building something and were ready to launch. The day before launch, Tobi — Shopify's founder — asks one question: "If you could start over, how would you build it?"

Someone goes to the whiteboard. Describes a completely different, simpler architecture. Tobi says: build that instead. They deleted what they'd built and started over. Three months later, they shipped something better.

The key for me: we're all learning the new way of doing things in an agentic world. That means focusing less on token usage, PRs, lines of code (still matter, obviously) and more on what we actually learn from building and iterating.

---

The second takeaway was this — the whole narrative of "so can we just have fewer engineers?" is completely backwards. Where's the ambition? The question Farhan posed was: "We have 3,000 engineers. Why can't they act like 100,000?"

The ambition of what teams can and should attempt is increasing. People are trying bigger things because the cost of trying has dropped.

---

A few practical things from the talk worth keeping:

**Only use big models.** Shopify doesn't let engineers use smaller, cheaper models. The reasoning is clean: human time costs more than token time. A small model introduces a subtle bug; a senior engineer spends hours finding it. Just use the expensive model.

**Responsibility can't be given away.** AI writes the code, but your name goes on the PR. You can tag the model, but you own it. This isn't a moral point — it's a governance point. Someone has to be accountable, and it can't be the tool.

**The throughline:** in an AI-native engineering org, the scarce thing isn't code. It's judgment. Taste. Knowing which of the 10,000 right solutions to actually build.
