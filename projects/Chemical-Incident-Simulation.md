# Chemical Incident Response Platform Simulation (ChIRPS)

## Overview

This project is a web-based simulation and training platform designed to model complex chemical, biological, radiological, and nuclear (CBRN) emergency response scenarios. The system enables facilitators to run realistic, multi-agency incident exercises while capturing detailed, time-ordered decision-making and operational actions.

The platform is intended to support:

- Emergency management training
- Multi-agency coordination exercises
- After Action Reports (AAR)
- Decision analysis and replay
- Auditability and compliance-oriented recordkeeping

The architecture is designed from the outset to support sensitive-but-unclassified (SBU) use cases and future deployment to AWS GovCloud.

---
## Problem Statement
Training and exercising for low-frequency, high consequence threats like CBRN incidents is increasingly difficult at all levels of response, due to a variety of factors, including: resource limitations, personnel attrition, and the increasing number of high-frequency threats responders must prepare for. Functional Exercises are great for assessing specific tactics, techniques, and coordination druing a multi-agency response. However, they are challenging since responders have to be pulled "off the line" or brought in during their off-shift to participate. Discussion-based or Table Top Exercises are good for identifying communication and coordination challenges and planning factors, but they lack realism. Neither of these types of exercises take into account one of the most challenging aspects of a response: how people will behave. I wanted to design a system that helps responders work through a problem in near real-time that includes injects that no one developed ahead of time and could include an element of crowd behavior to stress the response. 

## Key Capabilities

- Multi-exercise support (concurrent simulations)
- Role-based interaction (facilitator, responder, observer)
- Event-driven simulation engine
- Immutable event logging for audit and replay
- Materialized state views for real-time simulation
- API-driven architecture for UI, analytics, and integration

---

## High-Level Architecture

The system uses a layered, event-driven architecture separating:

- Simulation logic
- API orchestration
- Current state (materialized views)
- Immutable event history (audit trail)

This design supports traceability, replay, debugging, and compliance use cases common in emergency management and government systems.

---

## Architectural Flow Diagram

```mermaid
flowchart TD
    UI[Web UI / Client]
    API[FastAPI API Layer]
    ENGINE[Simulation Engine]
    STATE[Exercise State Store]
    EVENTS[Immutable Event Log]
    FUTURE[(Future: DynamoDB / GovCloud)]

    UI --> API
    API --> ENGINE
    ENGINE --> STATE
    API --> STATE

    API --> EVENTS
    EVENTS --> API

    STATE -.-> FUTURE
    EVENTS -.-> FUTURE
