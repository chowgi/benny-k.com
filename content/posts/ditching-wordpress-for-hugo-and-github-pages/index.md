---
title: "I Ditched WordPress for Hugo and GitHub Pages. Here's How."
date: 2026-03-05
tags: ["hugo", "github", "wordpress", "blogging"]
draft: true
cover:
  image: "cover.png"
  alt: "Ditching WordPress for Hugo"
summary: "My blog was costing me money to serve 17 visitors a month. That felt stupid. So I moved to Hugo and GitHub Pages in about an hour."
---

My Premium WordPress blog had 17 visitors in December. WordPress is fine. It does what it does. But I've had a personal blog sitting there since early 2024, posting maybe once every few months, and the whole setup felt heavy for what it was. So I moved the whole thing to Hugo and GitHub Pages. With Clawdia, my AI assistant, it took about 15 minutes of my actual time. This is how it went.

## Why I moved

Three reasons. None of them are earth-shattering.

**It costs nothing.** GitHub Pages is free. WordPress was charging me for hosting on top of that. For a site that gets double-digit monthly traffic, paying for hosting felt like buying a gym membership I never use.

**I already live in markdown and git.** I now write everything in markdown, primarily because of the AI assistant I've been building with OpenClaw. My notes, my docs, my presentations. Having a blog that wants me to log into a web admin panel and use a block editor is friction I don't need.

**I wanted my AI assistant to be able to publish for me.** This is the real one. I've been running an AI agent that handles a bunch of my daily workflow — email, task management, research, document drafting. If my blog is just a git repo with markdown files, I can draft posts in my own words, get Clawdia to refine them, and she commits and pushes. No WordPress API, no authentication tokens, no plugins. Just git push.

## How I did it

I'm not going to write a step-by-step tutorial — there are plenty of those. But here's roughly what happened:

**Hugo setup:** This was Clawdia's suggestion. I had no idea what Hugo was, but she installed it, created a new site, and picked the PaperMod theme because it's clean and fast. Configured `hugo.toml` with my details. Maybe 10 minutes.

**Post migration:** I had 10 posts on WordPress. Clawdia fetched each one, converted them to markdown with proper frontmatter (title, date, tags), downloaded the images, and dropped them into Hugo's content folder. Each post lives in its own directory with its images alongside it — Hugo calls these "page bundles" and it keeps things tidy.

**GitHub repo:** She created a repo, added a GitHub Actions workflow that builds the Hugo site and deploys to GitHub Pages on every push to main. The workflow file is about 40 lines of YAML that I'll never touch again.

**DNS:** Pointed my domain's A records to GitHub's IPs, added a CNAME for www, typed the domain into GitHub Pages settings, and waited 20 minutes for the TLS certificate to provision.

The whole thing — from "I should probably move off WordPress" to "the site is live on the new stack" — was about an hour in between doing my day job. Most of that was wrangling the GitHub and WordPress interfaces.

## What's different now

The site loads faster. There's no database. There's no admin panel to secure. There's no plugin update notifications. There's no PHP.

When I want to write a post, I write markdown. When I want to publish, I push to git. When I want Clawdia to draft something, she writes a markdown file and pushes. The whole thing is transparent — you can literally see the source code for this site.

## What's next

I've got a few posts in the pipeline about building and running AI agents in practice — not the "here's a LangChain tutorial" kind, more the "here's what actually happens when you let an AI agent manage your CRM and it sends you a message at 3am because it got the timezone wrong" kind.

If you're interested in that sort of thing, stick around. And if you've been thinking about ditching WordPress for something simpler, honestly just do it. It's less work than you think.
