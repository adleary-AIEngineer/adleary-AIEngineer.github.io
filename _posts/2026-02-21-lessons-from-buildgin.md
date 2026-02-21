---
layout: post
title: "When Your Laptop Tells You "No": Lessons from Building an AI Emergency Management Tool"
date: 2026-02-21
categories: ai emergency-management architecture
excerpt: "Challenges faced during development and transition from local-only to hybrid-local/cloud."
---

**February 21, 2026**

Last week I shared why I started building instead of just watching. This week, I want to talk about what building actually looks like — because it's rarely the clean, linear process the tutorials make it out to be.

I've been working on [ERES](projects/Emergency-Response-Evaluation-System) — an AI-powered Retrieval Augmented Generation (RAG) system designed to help emergency managers query complex Emergency Operations Plans and SITREPs conversationally, instead of digging through hundreds of pages under pressure. The concept is straightforward. The execution? A masterclass in things breaking in ways I didn't know were possible.

**Challenge 1: The model was too big for the room**

My first real wall hit before I wrote a single line of application code. I'd chosen an AI model (Llama 3.1, 8 billion parameters) that required about 4.8 gigabytes of memory to run. My development environment — a virtualized Linux instance running inside Windows — could only allocate 4.3. Result: a cryptic "500 Internal Server Error" and a lot of confused Googling.

The fix wasn't glamorous, it was a resource allocation problem. I needed to right-size the resources to the problem, with room to scale up if needed. 

I ran a tradeoff analysis between model intelligence and performance, then switched to a smaller model (Llama 3.2, 3 billion parameters) that cut memory usage by 50% and ran three times faster — without meaningfully sacrificing the accuracy I needed for pulling facts out of emergency plans.

**Challenge 2: A cascade failure in the build environment**

Later, during a particularly resource-intensive build process, everything crashed with a `SIGBUS: bus error` — which I learned means the virtual disk my Linux environment runs on lost sync with the Windows host under high memory and processing load. Essentially, a span-of-control problem. When one system gets overloaded and starts failing, everything downstream fails with it. The fix isn't to push harder — it's to reduce the load and establish clear lanes.

The fix had two parts: I changed how I was compiling code (switching to a method called CGO-free static builds, which reduces the system's dependency on shared memory during compilation) and I reconfigured how much dedicated RAM and swap space the virtual environment could access. Once the environment had stable footing, the crashes stopped.

**Challenge 3: Getting data into an AI system is harder than it sounds**

*Raise your hand if you've ever attended an after-action review and communications was **NOT** an area for improvement.*

Emergency documents are messy. PDFs with tables, maps, and multi-column layouts don't parse the way clean text does. Standard scrapers turn all of that into what I can only describe as "word soup" — the spatial context that makes the information meaningful gets completely lost. A table showing resource assignments becomes a jumbled list of words with no relationship to each other.

I switched to a layout-aware parsing library (unstructured, using a high-resolution strategy) that analyzes the document's visual structure before extracting text — identifying headers, tables, and sections the way a human reader would. It's slower and more resource-intensive, but it preserves the meaning.

**Challenge 4: The local-to-cloud pivot**

The biggest architectural decision came when I accepted what my hardware had been quietly telling me all along: a production-grade tool for emergency management cannot run off a personal laptop.

So I started rebuilding ERES as a hybrid system. Heavy file processing and document storage now run on AWS using serverless, event-driven architecture — meaning the system responds to data as it arrives (a new SITREP drops into storage, the pipeline automatically triggers) rather than waiting to be told to check. The AI reasoning engine stays local, which keeps operating costs near zero and keeps sensitive emergency planning data off third-party servers.

I learned the hard way that cross-platform builds are their own discipline — compiling code on Windows for a Linux-based cloud environment requires explicit instructions about the target architecture. I thought about NIMS and interoperability - making sure everyone speaks the same language helps coordinate responses and makes a lot of decisions automated.

**What I'm actually learning**

Every one of these problems had a shape I recognized from the field: resource constraints forcing triage decisions, cascade failures propagating through interconnected systems, the tension between processing speed and accuracy when the stakes are high, and the hard lesson that what works in a controlled exercise doesn't always scale to an actual incident.

I'm not just learning to code. I'm evaluating systems under stress in a new language — which, it turns out, is something two decades in emergency management prepared me for more than I realized.

#EmergencyManagement #AI #MachineLearning #CBRN #BuildingInPublic #RAG #AWSLambda
