---
title: "Chemical Incident Response Platform Simulation (ChIRPS)"
---

## 📌 Project Overview

**ChIRPS** is a web-based simulation and training platform designed to model complex chemical, biological, radiological, and nuclear (CBRN) emergency response scenarios. The system enables facilitators to run realistic, multi-agency incident exercises while capturing detailed, time-ordered decision-making and operational actions.

The platform bridges the gap between traditional training exercises and real-world incident complexity by introducing dynamic, AI-driven scenario evolution that accounts for human behavior and crowd dynamics.

---

## 🎯 Problem Statement

Training and exercising for low-frequency, high-consequence threats like CBRN incidents is increasingly difficult at all levels of response due to resource limitations, personnel attrition, and the growing number of high-frequency threats responders must prepare for.

**Current Exercise Limitations:**
- **Functional Exercises** excel at assessing specific tactics and multi-agency coordination, but require pulling responders "off the line" or bringing them in during off-shifts
- **Table Top Exercises** identify communication and coordination challenges but lack realism
- **Neither approach** adequately simulates one of the most challenging response aspects: unpredictable human behavior and crowd dynamics

ChIRPS addresses these gaps by providing near real-time problem-solving scenarios with AI-generated injects and crowd behavior elements that stress-test response capabilities.

---

## ✨ Key Capabilities

- **Multi-Exercise Support**: Run concurrent simulations across different scenarios and agencies
- **Role-Based Interaction**: Distinct interfaces and permissions for facilitators, responders, and observers
- **Event-Driven Simulation Engine**: Dynamic scenario evolution based on responder actions
- **Immutable Event Logging**: Complete audit trail for compliance and replay analysis
- **Materialized State Views**: Real-time simulation status and decision tracking
- **After Action Reports (AAR)**: Automated documentation of decisions and outcomes
- **API-Driven Architecture**: Extensible design for analytics, UI, and third-party integration

---

## 🏗 High-Level Architecture

The system employs a layered, event-driven architecture with clear separation of concerns:

![ChIRPS Architecture](/assets/images/ChIRPS-architecture.png)

**Architectural Principles:**
- **Simulation Logic**: Domain-driven design with deterministic simulation rules
- **API Orchestration**: FastAPI-based service layer for all interactions
- **Current State**: Materialized views for real-time query performance
- **Immutable History**: Event sourcing pattern for complete audit trail and replay capability

This design supports traceability, debugging, and compliance use cases common in emergency management and government systems.

---

## 🛠 Tools & Technologies

| Layer | Technology | Purpose |
|-------|------------|---------|
| Language | Python | Core application development and business logic |
| API Framework | FastAPI | High-performance async API with automatic documentation |
| Data Validation | Pydantic | Type-safe models and data validation |
| Architecture | Event Sourcing | Immutable event log with materialized views |
| Design Pattern | Repository Pattern | Persistence abstraction for future database flexibility |
| API Design | API-First | Clean separation enabling multiple client types |

---

## 🚀 Future Roadmap: Cloud Scale

The platform is architected for deployment to **AWS GovCloud** to support sensitive-but-unclassified (SBU) use cases:

- **Cloud Infrastructure**: AWS GovCloud compliance and security controls
- **Database**: DynamoDB for scalable event storage
- **Security**: IAM-based access control and encryption at rest/in transit
- **Compliance**: Audit logging and data retention patterns for government requirements
