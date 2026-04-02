# DispatchFlow — AI-Powered Logistics Dispatch System

DispatchFlow is a backend system that simulates a modern logistics and transport dispatch platform powered by AI.

It handles job creation, automated pricing, prioritization, and driver assignment using a structured AI agent, while maintaining reliability through queues, retries, and clear state management.

This project focuses on how AI can be integrated into real backend systems — not just for responses, but for decision-making and workflow execution.

---

## Overview

The system allows users to create delivery jobs, which are then processed asynchronously.

An AI agent evaluates each job and determines:
- Pricing
- Priority level
- Estimated duration

Once processed, the system assigns a driver, updates job status, and logs each step of the workflow.

---

## Key Features

- AI-driven pricing and prioritization (structured outputs, not free text)
- Background job processing using queues
- Driver assignment logic
- Job lifecycle tracking (pending → completed)
- Retry handling for failed AI calls
- Structured logging for observability
- Clean, modular backend architecture

---

## System Architecture

The system is split into clear components:

- **API Layer**  
  Handles incoming requests and validation

- **Queue System (BullMQ)**  
  Processes jobs asynchronously to avoid blocking requests

- **AI Agent Service**  
  Uses Claude API to make decisions (price, priority, duration)

- **Database (PostgreSQL + Prisma)**  
  Stores jobs, drivers, and logs

- **Worker პროცეს**  
  Executes background tasks:
  - Calls AI agent
  - Updates job
  - Assigns driver
  - Logs events

---

## AI Agent Design

Instead of generating free-form text, the AI agent is constrained to return structured JSON:

```json
{
  "price": 120,
  "priority": "urgent",
  "estimatedDuration": 45,
  "reasoning": "Long distance and time-sensitive delivery"
}
