---
layout: post
title: "Back to Basics: The Weeks Where You Sharpen the Map"
date: 2026-03-09
categories: ai emergency-management architecture network
excerpt: "Developing a home network to better understand deployment infrastructure."
---

**March 9, 2026**

## Not every week produces a demo worth showing.

Some weeks are quieter than that—slower, more deliberate. You’re not building the thing people will eventually see. You’re building the foundation underneath it, reinforcing the mental model, filling in gaps you didn’t know you had. This past week was that kind of week.
I didn’t ship anything flashy. I went back to fundamentals. And I came out the other side with a clearer picture of how these systems actually fit together—which, it turns out, is exactly what I needed.
Infrastructure, Again (Still)

I spent more time this week on the home lab I built during my “sidequest” last week—tightening up network segmentation, improving routing decisions, and thinking harder about service isolation. This wasn’t scope creep. It was the logical continuation of a lesson I keep relearning: AI systems don’t live in notebooks. They run on real infrastructure with real constraints, and if you don’t understand that infrastructure, you’ll keep being surprised by it.
Every hour I spend understanding how services communicate, where they fail, and why routing decisions matter is an hour that pays forward into ERES and every other tool I build. The fundamentals aren’t a detour. They’re the work.

## Progress on ChIRPS
In parallel, I made meaningful progress on [ChIRPS](/projects/Chemical-Incident-Simulation)—a chemical incident exercise and simulation system I’ve been developing alongside [ERES](/projects/Emergency-Response-Evaluation-System). Three things moved forward this week.

First, I built a basic UI to drive the workflow. Nothing polished—just functional enough to stop testing everything through the terminal and start interacting with the system the way a real user eventually will. That shift in perspective matters more than it sounds. You notice different things when you’re clicking through a workflow versus reading log output.

Second, I implemented LLM narrator actions across the system steps. The model now provides running commentary as the workflow moves through stages—not just outputs at the end, but context and explanation at each decision point. In a training environment, that narration is part of the value.

Third, I introduced system prompt versioning. This one felt small when I did it, but it’s already proving its worth. Managing behavior changes in an LLM-powered system without versioning is like deploying software without version control—you lose track of what changed and why, and debugging becomes guesswork. Versioning the prompts gives me a record I can reason about.

## A Small Step: The Containerized Dev Environment
I also spun up a containerized development environment for my next project. I’m not ready to say much about that project yet, but the setup step itself is worth noting. Containerizing from the start—before a project has any real code—is a discipline I’m trying to build. It forces clean dependency management, prevents the “works on my machine” problem early, and means I’m not retrofitting isolation onto a mess later. A small step, but the right one.

## Building a RAG Chatbot for My Own Infrastructure
This was the most interesting part of the week, and it started with a frustration.

Troubleshooting my home lab had become a constant cycle of jumping between documentation tabs, half-remembered config syntax, and forum posts that may or may not apply to my specific setup. I was losing time to context-switching. So I decided to build a tool to fix that: a RAG chatbot trained on the documentation of the services I’m actually running.

RAG—Retrieval-Augmented Generation—is an approach where you give a language model access to a specific set of documents at query time, rather than relying solely on what it learned during training. The model retrieves relevant chunks from your documents and uses them to ground its response. It’s particularly good for technical documentation, where accuracy and specificity matter more than general knowledge.

### Step One: Getting the Docs (Harder Than It Should Be)
I started with what seemed like the easy part: scraping documentation from the services I’m running. It was not easy.

My first approach was a simple Python script using the requests library to pull HTML content from each documentation site. That worked for some sites and failed immediately for others—two of them blocked the requests outright, identifying it as bot traffic. A third rendered its content dynamically with React, meaning the HTML returned by requests was essentially an empty shell with no actual documentation in it.

So I rebuilt the scraper using Playwright, a browser automation library that drives a real browser rather than making raw HTTP requests. Playwright renders JavaScript, handles dynamic content, and—critically—looks a lot more like a human browsing the web than a script does. The scraper became more complex, but it worked. Every documentation site yielded clean, structured text.

This is a pattern I keep encountering: the first tool doesn’t work, so you build a better one. That’s not a failure. It’s the actual process.

### Step Two: Building the RAG Pipeline
With clean documentation in hand, I built the pipeline using Python, LangChain, ChromaDB, and Ollama.

LangChain handles the orchestration—chunking the documentation, managing the retrieval logic, and chaining the retrieval step to the generation step. ChromaDB is the vector database that stores the embedded document chunks and enables fast similarity search when I ask a question. Ollama runs the language model locally, which was an intentional choice: the documentation for my infrastructure shouldn’t leave my network, and a locally-hosted model keeps everything self-contained.

The workflow is straightforward. I ask a question—“Why is this Nginx routing rule not working?” or “How do I configure this service to use a custom DNS resolver?”—and the system retrieves the most relevant chunks from my documentation corpus and passes them to the model alongside the question. The model answers using that specific context rather than general internet knowledge.

It’s not fancy. It’s not production-grade. It’s a personal tool I built for myself in a few days, and it works surprisingly well for what it is.

## The Thing I Keep Coming Back To
Building this chatbot reinforced something I’ve been circling for months now.

AI engineering isn’t just about models. It’s about systems—systems made up of infrastructure, orchestration, data pipelines, prompts, guardrails, observability, and failure modes, all interacting in ways that aren’t always obvious until something breaks. The model is one component in a larger architecture. Understanding only the model is like understanding only the incident commander in an emergency response—without knowing how logistics, communications, and operations interact, you don’t actually understand the system.

This week, I touched nearly every layer of that stack: the network layer, the application layer, the data layer, the model layer. Not deeply in each case, but enough to feel how they connect. That’s what a fundamentals week buys you. Not a finished product—a sharper map.

Progress isn’t always linear. Sometimes the most important work is the work that makes the next work possible.
