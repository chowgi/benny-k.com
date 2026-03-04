---
title: "I Ditched WordPress for Hugo and GitHub Pages. Here's How."
date: 2026-03-05
tags: ["hugo", "github", "wordpress", "blogging"]
draft: true
summary: "My blog was costing me money to serve 17 visitors a month. That felt stupid. So I moved to Hugo and GitHub Pages in about an hour."
---

My WordPress blog had 17 visitors in December. I was paying for hosting. Something about that equation bothered me.

Look, WordPress is fine. It does what it does. But I've had a personal blog sitting there since early 2024, posting maybe once every few months, and the whole setup felt heavy for what it was. A CMS, a database, plugins I never update, a theme I picked in 5 minutes and never touched again. All to serve a handful of markdown posts to almost nobody.

So I moved the whole thing to Hugo and GitHub Pages. It took about an hour. This is how it went.

## Why I moved

Three reasons. None of them are earth-shattering.

**It costs nothing.** GitHub Pages is free. My domain registration is about $14 a year and that's the total cost. WordPress was charging me for hosting on top of that. For a site that gets double-digit monthly traffic, paying for hosting felt like buying a gym membership I never use.

**I already live in markdown and git.** I write everything in markdown. My notes, my docs, my presentations. Having a blog that wants me to log into a web admin panel and use a block editor is friction I don't need. With Hugo, a blog post is a markdown file. I push to git. The site rebuilds. Done.

**I wanted my AI assistant to be able to publish for me.** This is the real one. I've been running an AI agent that handles a bunch of my daily workflow — email, task management, research, document drafting. If my blog is just a git repo with markdown files, the agent can draft posts, commit them, and push. No WordPress API, no authentication tokens, no plugins. Just git push.

## How I did it

I'm not going to write a step-by-step tutorial — there are plenty of those. But here's roughly what happened:

**Hugo setup:** Installed Hugo, created a new site, picked the PaperMod theme because it's clean and fast and I didn't want to spend three hours comparing themes. Configured `hugo.toml` with my details. Took maybe 10 minutes.

**Post migration:** I had 10 posts on WordPress. Fetched each one, converted to markdown with proper frontmatter (title, date, tags), downloaded the images, and dropped them into Hugo's content folder. Each post lives in its own directory with its images alongside it — Hugo calls these "page bundles" and it keeps things tidy.

**GitHub repo:** Created a repo, added a GitHub Actions workflow that builds the Hugo site and deploys to GitHub Pages on every push to main. The workflow file is about 40 lines of YAML that I'll never touch again.

**DNS:** Pointed my domain's A records to GitHub's IPs, added a CNAME for www, typed the domain into GitHub Pages settings, and waited 20 minutes for the TLS certificate to provision. That was the only part that required any patience.

The whole thing — from "I should probably move off WordPress" to "the site is live on the new stack" — was about an hour. And honestly, most of that was downloading images from WordPress.

## What I actually did vs. what I just described

I should be honest here. When I said "I did this," what I mean is I told my AI assistant to do it and then approved the output. Clawdia set up Hugo, migrated the posts, created the GitHub repo, configured the deployment pipeline, and pushed. I pointed DNS and clicked a few buttons in GitHub settings.

That's kind of the point. If your blog infrastructure is simple enough that an AI agent can set it up, migrate your content, and publish posts — it's probably the right level of complexity for a personal blog. WordPress was overkill. A static site generator with git-based deployment is about right.

## What's different now

The site loads faster. There's no database. There's no admin panel to secure. There's no plugin update notifications. There's no PHP.

When I want to write a post, I write markdown. When I want to publish, I push to git. When I want my AI assistant to draft something, it writes a markdown file and pushes. The whole thing is transparent — you can literally see the source code for this site.

## What's next

I've got a few posts in the pipeline about building and running AI agents in practice — not the "here's a LangChain tutorial" kind, more the "here's what actually happens when you let an AI agent manage your CRM and it sends you a message at 3am because it got the timezone wrong" kind.

If you're interested in that sort of thing, stick around. And if you've been thinking about ditching WordPress for something simpler, honestly just do it. It's less work than you think.
