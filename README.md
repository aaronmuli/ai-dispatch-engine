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

- **Worker**  
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

---

````markdown
## API Endpoints

### 1. Create Job
**POST /jobs**

Creates a new delivery job and sends it to the queue for AI processing.

Request:
```json
{
  "pickupLocation": "Lusaka CBD",
  "dropoffLocation": "Levy Junction Mall",
  "distance": 10
}
````

Response:

```json
{
  "id": "job_123",
  "status": "pending"
}
```

---

### 2. Get All Jobs

**GET /jobs**

Returns a list of all jobs.

---

### 3. Get Job by ID

**GET /jobs/:id**

Returns details of a specific job, including:

* price
* priority
* assigned driver
* status

---

### 4. Update Job Status

**PATCH /jobs/:id/status**

Manually update job status (for simulation/testing).

Request:

```json
{
  "status": "completed"
}
```

---

### 5. Get Drivers

**GET /drivers**

Returns available and assigned drivers.

---

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/your-username/dispatchflow-ai.git
cd dispatchflow-ai
```

---

### 2. Install dependencies

```bash
npm install
```

---

### 3. Set up environment variables

Create a `.env` file in the root directory:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/dispatchflow
REDIS_URL=redis://localhost:6379
ANTHROPIC_API_KEY=your_anthropic_api_key
```

---

### 4. Set up the database

```bash
npx prisma migrate dev
```

(Optional: seed drivers)

```bash
npx prisma db seed
```

---

### 5. Start Redis

Make sure Redis is running locally:

```bash
redis-server
```

---

### 6. Start the backend server

```bash
npm run start:dev
```

---

### 7. Start the worker (queue processor)

```bash
npm run worker
```

This is required for:

* AI processing
* Driver assignment
* Job updates

---

## Quick Test Flow

1. Send a POST request to `/jobs`
2. Job is created with status `pending`
3. Worker processes the job:

   * Calls AI agent
   * Updates price and priority
   * Assigns a driver
4. Check `/jobs` to see updated results

---

## Notes

* If the AI agent fails, the system retries automatically
* All job updates are logged internally
* Queue must be running for full functionality

```

