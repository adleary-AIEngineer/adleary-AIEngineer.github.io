---
layout: post
title: "Containerized Development and Thinking in Systems"
date: 2026-03-16
categories: ai emergency-management architecture network VLAN containers
excerpt: "Development within a container while exploring container ochestration."
---

**March 16, 2026**

## Introduction
One of the themes emerging from this journey into AI engineering is that the model itself is rarely the hardest part of the system. More often, the challenge lies in everything surrounding it: development environments, dependency management, networking, orchestration, and infrastructure.

This week I continued exploring those layers by containerizing one of my earlier AI projects and using it as a test case to better understand how modern development environments actually work.

Along the way, I also began exploring Kubernetes and network segmentation concepts that mimic how cloud systems are structured.

## Refactoring a Project into a Containerized Environment
The project I chose was a simple chatbot originally built during an online course. The chatbot uses personal information from my resume and LinkedIn profile to answer questions as a kind of automated personal representative.

The application itself is relatively small, which made it a good candidate for experimentation.

My main goal was to solve a persistent problem I had been running into: dependency conflicts across projects. Even though I had set up Python virtual environments in VS Code, they weren’t always behaving the way I expected. Over time, those environments can become difficult to manage, especially when working across multiple experiments and tools.

So I decided to rebuild the project from scratch inside a containerized environment.

## Understanding Docker: What Finally Clicked
While working through the container setup, one conceptual piece finally clicked for me.
Docker relies on two key components that work together:
- Dockerfile defines how the container image is built. It specifies things like:
  - the base operating system
  - Python installation
  - required packages
  - application files
In other words, it defines the environment.

- docker-compose.yaml defines how the container should run. It specifies:
  - which services start
  - environment variables
  - port mappings
  - connections to other services
In other words, it defines how the environment operates.

Once I understood that relationship, the rest of the system started to make sense.

## Development Inside a Container
One subtle but important concept took a little time to internalize.
Even though I am still writing code inside VS Code, the application itself is running inside the container.

That means:
- the Python environment lives inside the container
- the dependencies live inside the container
- the execution environment lives inside the container

If I introduce a new import into the code, I need to add it to the requirements.txt file. The next time the container image is rebuilt, the new dependency will automatically be included. This creates something powerful: reproducible environments. Instead of trying to maintain multiple Python environments across projects, each project carries its own isolated environment with it.

## Connecting to a Local AI Runtime
The containerized chatbot connects to Ollama, which runs on a separate AI inference node in my home lab. That separation helped reinforce another concept that keeps appearing in AI system architecture: the AI model is often just one component in a larger distributed system.

In this case the pieces include:
- the development container
- the application logic
- the local AI inference service
- the data used to generate responses

Even a small project quickly becomes a system made of several interacting components.

![Containerized Development Stackl](/assets/images/ai-systems-field-notes-week5-container-stack.png "Container Stack")

## Looking Ahead: Kubernetes
Once I became more comfortable with Docker, the natural next question emerged: What happens when you need to coordinate many containers across many machines? That question leads directly into Kubernetes, which is designed to orchestrate containerized systems at scale. At this point I’m still in the early exploration phase, but understanding Kubernetes seems like an important step toward understanding how real-world cloud systems operate.

## Exploring Network Segmentation with VLANs
On the networking side of my home lab experiments, I also began exploring VLANs.

The goal here is not necessarily to build a complex enterprise network, but to simulate some of the patterns used in cloud infrastructure.
For example:
- separating development and production environments
- isolating infrastructure services
- controlling how systems communicate across networks

These kinds of segmentation strategies are common in cloud environments, and experimenting with them locally helps make those architectural patterns more tangible.

## A Lesson in Scale
One observation stood out to me this week: Systems scale faster than our intuition.
- A simple command becomes a function.
- Functions combine into a program.
- Programs interact to form systems.
- Systems connect to form systems of systems.

Eventually those systems run at global scale.

In emergency management, thinking about cascading systems and second-order consequences is a critical skill. As I learn more about AI system architecture, I’m realizing that the same mindset applies here.

Understanding how things connect is essential if you want to understand how they fail.

## Final Thoughts
This week’s work wasn’t about building a flashy new AI capability.

Instead, it was about strengthening the foundation beneath those systems.

By containerizing development environments, exploring orchestration concepts, and experimenting with network segmentation, I’m slowly building a better mental model of how modern AI systems actually operate.

Each experiment adds another layer to that understanding.
