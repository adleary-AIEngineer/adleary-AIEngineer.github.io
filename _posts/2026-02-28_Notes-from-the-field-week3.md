---
layout: post
title: "The Sidequest That Wasn’t a Sidequest"
date: 2026-02-28
categories: ai emergency-management architecture network
excerpt: "Developing a home network to better understand deployment infrastructure."
---

**February 21, 2026**

I told myself it was a detour.

This week, I was supposed to be adding features to [ERES](/projects/Emergency-Response-Evaluation-System)—a RAG-based tool to gain insights from operational plans and after-action reviews. Instead, I found myself staring at a stack of infrastructure problems I didn’t fully understand. Services failing in ways I couldn’t explain. Memory ceilings I kept bumping into. Permission errors that seemed to appear at random. I could patch each one individually and move on, but something told me that was the wrong approach.

In emergency management, project management, and many other fields we have a name for what I was doing: treating symptoms instead of the root cause. So I stopped. And I built a lab.

## What I Actually Built
The goal was to stand up a production-style home lab that would force me to confront infrastructure at every layer—not just the AI layer where I spend most of my time. I started with Ubuntu Server as the foundation and Cockpit for system management, which gave me a clean browser-based view into what the machine was actually doing: resource usage, service health, storage, logs.

From there, I containerized everything with Docker and used Portainer to manage it—a surprisingly good tool for someone who still thinks in incident command structures rather than DevOps pipelines. Portainer makes container management visual and navigable in a way that raw Docker CLI doesn’t.

I put Nginx in front of everything as a reverse proxy, handling routing and SSL termination so services don’t have to expose themselves directly. Network segmentation came next—isolating services from each other the same way you’d isolate operational zones in a CBRN response to prevent cross-contamination. And for secure remote access, I set up Tailscale, which handles encrypted tunneling without punching holes in the firewall. Once you understand what it’s doing under the hood, it’s elegant.

I added AdGuard for DNS-level filtering and ad blocking across the network, and Nextcloud as a self-hosted file and collaboration layer. Finally, a monitoring stack for full observability—because a system you can’t see is a system you can’t trust.

## What Broke (Everything, At Least Once)
Almost everything failed at some point during this build—which was entirely the point.

Nginx routing rules that looked correct and weren’t. Containers that started fine in isolation but conflicted when networked together. DNS resolution that worked until I added AdGuard and suddenly didn’t. Tailscale connectivity that dropped when a service I didn’t realize was a dependency restarted.

Each failure taught me something a tutorial couldn’t: how these pieces actually interact under real conditions, not ideal ones. That’s the difference between reading about incident command and running a drill. You learn different things when something breaks on you.
The Mental Model That Clicked

## Somewhere around day three, something shifted.
I’ve spent twenty years analyzing systems under stress—looking for single points of failure, cascading dependencies, resource bottlenecks, fragile assumptions. That’s the intellectual core of emergency management. And I realized I was doing exactly the same thing here, just with a different vocabulary.

Network segmentation is operational zoning—you contain the problem so it doesn’t spread. Container isolation is task force structure—each unit has defined responsibilities and limited exposure to other units. A monitoring dashboard is a situational awareness board. When a service goes down and takes three others with it, that’s a cascading failure, the same kind I’d model in a mass casualty exercise.

The concepts weren’t new. The implementation language was.

## Why This Matters for ERES
ERES is designed to be a tool emergency managers can trust. That means it can’t just work when conditions are perfect. It has to work when it is needed, regardless of how many people are trying to access it at the same time.

AI doesn’t fail in isolation. It fails inside infrastructure. Before this week, I understood that intellectually. Now I understand it operationally—I’ve watched it happen, diagnosed it, and fixed it. That’s a different kind of knowledge. A tool built by someone who’s only ever seen the AI layer is a tool that will surprise you in production. I don’t want to build that tool.

## On Sidequests
I called this a sidequest when I started. I’m not sure that’s right anymore.

In emergency management, we talk about the difference between training to standard and training to reality. Training to standard means you can pass the test. Training to reality means you’ve been stress-tested enough to know how you’ll actually behave when something goes wrong.

This week wasn’t a detour from building ERES. It was training to reality. Every hour I spent debugging a reverse proxy misconfiguration or tracing a DNS failure through three layers of services is an hour I’m less likely to be surprised by those same failures when someone is depending on the tool.

The path from “I want to build AI tools for emergency response” to “I can build AI tools emergency managers can trust” runs straight through understanding the infrastructure those tools run on. There are no shortcuts. There are only systems, and the time you spend learning how they fail.

Sidequest complete. Back to ERES—now with a better mental model of the battlefield.
