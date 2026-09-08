# 🕵️SHERLOCK

## AI-Powered API Debugging & Distributed Systems Observability Platform

> **Sherlock helps developers investigate API failures by correlating requests, logs, distributed traces, metrics, dependencies, and deployments to identify evidence-backed root causes, with AI providing an understandable investigation and explanation.**
> 

---

# 1. 📌 Project Overview

## 1.1 What is Sherlock?

Modern applications are rarely a single backend.

A typical production application may look like:

```
User
 ↓
Frontend
 ↓
API Gateway
 ↓
Auth Service
 ↓
Order Service
 ↓
Payment Service
 ↓
Inventory Service
 ↓
Database
 ↓
External APIs
```

When something fails, developers often need to inspect:

- API responses
- Application logs
- Distributed traces
- Database performance
- Service dependencies
- Metrics
- Deployment history
- Infrastructure events
- External API failures

The biggest problem is **correlation**.

A developer may know that:

```
POST /api/orders
```

returned:

```
HTTP 504 Gateway Timeout
```

But they still need to determine:

```
Why did it timeout?
Which service caused it?
Which dependency was slow?
Was there a deployment?
Was the database slow?
Did an external API fail?
```

Sherlock solves this problem.

---

# 2. 🎯 Sherlock's Core Mission

Sherlock follows this pipeline:

```
API Failure
     ↓
Collect Evidence
     ↓
Correlate Signals
     ↓
Build Investigation
     ↓
Analyze Dependencies
     ↓
Determine Probable Root Cause
     ↓
AI Explains WHY
     ↓
Developer Fixes Problem
```

The important principle is:

> **AI should explain evidence, not invent the evidence.**
> 

Sherlock should remain useful even when the AI layer is disabled.

---

# 3. 💡 Problem Statement

## Current debugging process

A developer receives:

```
500 Internal Server Error
```

They may need to open:

- Postman
- application logs
- Grafana
- Prometheus
- Jaeger
- OpenSearch/Kibana
- cloud logs
- database monitoring
- deployment system

Then manually connect the information.

This is slow and error-prone.

## Sherlock's approach

Sherlock automatically connects:

```
API Request
    │
    ├── Trace
    │
    ├── Logs
    │
    ├── Metrics
    │
    ├── Dependencies
    │
    ├── Errors
    │
    └── Deployment
          ↓
      Investigation
          ↓
     Root Cause
          ↓
     AI Explanation
```

---

# 4. 🧠 Example Investigation

Suppose the user calls:

```
POST /api/orders
```

The request returns:

```
504 Gateway Timeout
```

Sherlock discovers:

```
API Gateway
    ↓ 180ms
Order Service
    ↓ 250ms
Payment Service
    ↓ 2.8 seconds
External Payment API
    ↓
Timeout
```

Sherlock also discovers:

```
Payment latency increased 5 minutes ago
Payment timeout count increased
Deployment occurred 7 minutes ago
Order Service started returning 504
```

Sherlock creates an investigation:

```
Probable Root Cause:
Payment Service dependency timeout

Confidence:
91%

Evidence:
1. Payment latency increased from 300ms → 2.8s
2. Payment timeout count increased
3. Order failures correlate with payment failures
4. Deployment occurred shortly before degradation
```

AI then explains the investigation in human-readable language.

---

# 5. 🏷️ Project Identity

| Category | Sherlock |
| --- | --- |
| Product Type | Developer Observability SaaS |
| Core Problem | API & distributed-system debugging |
| Core Feature | API debugging + distributed observability |
| Backend | Java + Spring Boot |
| Frontend | Next.js + React |
| Messaging | Apache Kafka |
| Database | PostgreSQL |
| Search | OpenSearch |
| Cache | Redis |
| Telemetry | OpenTelemetry |
| Metrics | Prometheus |
| Visualization | Grafana / Sherlock UI |
| AI | LLM-based investigation & explanation |
| Architecture | Event-driven microservices |
| Deployment | Docker + Kubernetes |
| Cloud | AWS |
| Target Users | Developers, SREs, DevOps teams |

---

# 6. 👥 Target Users

Sherlock is designed primarily for:

### Developers

Investigate:

- API failures
- slow APIs
- exceptions
- dependency failures
- database issues

### SRE Teams

Investigate:

- incidents
- service degradation
- latency
- error spikes
- dependency failures

### DevOps Teams

Investigate:

- deployments
- infrastructure changes
- service failures
- production incidents

### Engineering Teams

Understand:

- service dependencies
- system behavior
- production reliability

---

# 7. 🏗️ High-Level Architecture

```
                    ┌──────────────────────┐
                    │      Developer       │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Sherlock Frontend  │
                    │   Next.js / React    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     API Gateway      │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
        Identity Service   Project Service   Debug API
              │                │                │
              └────────────────┼────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Ingestion Service  │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │        Kafka         │
                    └──────────┬───────────┘
                               │
          ┌────────────────────┼─────────────────────┐
          ▼                    ▼                     ▼
   Log Processing       Trace Processing      Metrics Processing
          │                    │                     │
          └────────────────────┼─────────────────────┘
                               ▼
                    ┌──────────────────────┐
                    │ Correlation Service  │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Analysis Engine    │
                    └──────────┬───────────┘
                               │
                    ┌──────────┴───────────┐
                    ▼                      ▼
              Rule Engine             AI Engine
                    │                      │
                    └──────────┬───────────┘
                               ▼
                    ┌──────────────────────┐
                    │ Incident / Result    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Sherlock Dashboard   │
                    └──────────────────────┘
```

---

# 8. 🧩 Core Microservices

Sherlock is divided into several logical services.

Important:

> These do not all need to become separate deployable applications on day one.
> 

Start with the core pipeline and split services when scaling or ownership requires it.

---

## 8.1 API Gateway

### Responsibilities

- Request routing
- Authentication
- Authorization
- JWT validation
- Rate limiting
- API versioning
- Tenant identification
- Request correlation

### Technology

```
Spring Cloud Gateway
Spring Security
Redis
JWT
```

Example:

```
/api/auth/**       → Identity Service
/api/projects/**   → Project Service
/api/debug/**      → Debug Service
/api/incidents/**  → Incident Service
```

---

# 9. 🔐 Identity Service

Responsible for:

- User registration
- Login
- JWT
- Refresh tokens
- Organizations
- Workspaces
- Roles
- Permissions
- API keys

## Roles

```
OWNER
ADMIN
DEVELOPER
VIEWER
```

Example:

```
Organization
    │
    ├── Workspace A
    │       ├── Developer
    │       └── Viewer
    │
    └── Workspace B
            ├── Admin
            └── Developer
```

---

# 10. 📁 Project Service

Projects represent applications being monitored.

Example:

```
Organization
   ↓
Workspace
   ↓
Project
   ↓
Environment
   ↓
Services
```

Example:

```
Project: E-Commerce

Environment:
- Development
- Staging
- Production

Services:
- Gateway
- User Service
- Order Service
- Payment Service
- Inventory Service
```

---

# 11. 📥 Ingestion Service

The ingestion service receives telemetry.

Possible sources:

```
Java
.NET
Node.js
Python
Go
Mobile
Kubernetes
External APIs
```

Telemetry:

```
Logs
Traces
Metrics
API Requests
Errors
Deployments
Events
```

Example endpoint:

```
POST /v1/ingest
```

Headers:

```
Authorization: Bearer <API_KEY>
Content-Type: application/json
```

Example event:

```json
{
  "projectId": "project-123",
  "environment": "production",
  "serviceName": "payment-service",
  "traceId": "abc123",
  "spanId": "span456",
  "timestamp": "2026-09-04T10:30:00Z",
  "eventType": "ERROR",
  "message": "Payment provider timeout"
}
```

---

# 12. 📡 OpenTelemetry

Sherlock should use **OpenTelemetry** as the primary telemetry standard.

Architecture:

```
Application
     ↓
OpenTelemetry SDK
     ↓
OpenTelemetry Collector
     ↓
Sherlock Ingestion
     ↓
Kafka
```

Supported languages can eventually include:

```
Java
.NET
Node.js
Python
Go
```

---

# 13. 📨 Apache Kafka

Kafka is the event backbone of Sherlock.

Instead of:

```
Application → Processing Service
```

Sherlock uses:

```
Application
    ↓
Ingestion
    ↓
Kafka
    ↓
Consumers
```

Benefits:

- High throughput
- Decoupling
- Buffering
- Replay
- Consumer groups
- Horizontal scaling
- Fault tolerance

---

# 14. Kafka Topics

Recommended topics:

```
sherlock.api-events
sherlock.logs
sherlock.traces
sherlock.metrics
sherlock.errors
sherlock.deployments
sherlock.incidents
sherlock.analysis
```

Example:

```
Kafka
 │
 ├── logs
 │
 ├── traces
 │
 ├── metrics
 │
 ├── API events
 │
 ├── deployments
 │
 └── incidents
```

---

# 15. 🔗 Correlation Service

This is one of the most important components of Sherlock.

Its job is to answer:

> Which logs, traces, metrics, dependencies and deployments belong to this API failure?
> 

It correlates using:

```
traceId
spanId
requestId
serviceName
timestamp
environment
projectId
user/session context
```

Example:

```
Request ID:
req-123
      │
      ├── Gateway Log
      ├── Order Trace
      ├── Payment Trace
      ├── Payment Error
      ├── DB Query
      └── Deployment Event
```

---

# 16. 🔍 Trace Service

Responsible for distributed traces.

Example:

```
POST /api/orders

Gateway              100ms
   │
   └── Order Service  300ms
           │
           ├── Inventory 120ms
           │
           └── Payment 2.8s
                    │
                    └── External API 2.5s
```

Sherlock should display:

- Trace ID
- Span ID
- Parent/child spans
- Duration
- Service
- Endpoint
- Status
- Errors
- Dependencies

---

# 17. 📜 Log Service

Centralized log storage and search.

Example:

```
2026-09-04 10:30:12
ERROR
payment-service

Payment provider timeout

traceId=abc123
requestId=req456
```

Users should be able to search:

```
traceId=abc123
```

or:

```
service=payment-service
level=ERROR
```

or:

```
"timeout"
```

---

# 18. 📊 Metrics Service

Sherlock should monitor:

### API Metrics

```
Request Count
Error Rate
Latency
Throughput
```

### Latency

```
P50
P90
P95
P99
```

### Infrastructure

```
CPU
Memory
Disk
Network
```

### Database

```
Query latency
Connection pool
Timeouts
Errors
```

---

# 19. 🚨 Incident Service

Creates incidents from abnormal behavior.

Examples:

```
High Error Rate
High Latency
Service Down
Database Failure
Dependency Failure
Deployment Regression
External API Failure
```

Example:

```
INC-1023

Payment Service Degradation

Severity:
HIGH

Started:
10:24 AM

Affected:
Order API

Status:
Investigating
```

---

# 20. 🧠 Analysis Engine

The Analysis Engine is the brain of Sherlock.

However, it should not depend entirely on AI.

It first performs deterministic analysis.

---

# 21. 🧮 Rule-Based Root Cause Analysis

Example rule:

```
IF
Service A error rate increases
AND
Service B latency increases
AND
A depends on B
AND
A failures occur during B degradation

THEN

B is a probable root cause.
```

Another rule:

```
IF
Database latency increases
AND
SQL timeout count increases
AND
API latency increases

THEN

Database performance is a probable cause.
```

Another:

```
IF
Deployment occurred immediately before
error spike

THEN

Deployment is a correlated event.
```

---

# 22. 🎯 Root Cause Confidence

Sherlock can assign confidence scores.

Example:

```
Payment Service
Confidence: 91%

Database
Confidence: 58%

Order Service
Confidence: 32%
```

Possible scoring factors:

```
Dependency relationship
Error correlation
Latency correlation
Temporal correlation
Deployment correlation
Trace evidence
Metric anomaly
Log evidence
```

---

# 23. 🤖 AI Engine

AI is an additional investigation layer.

The AI should receive structured evidence.

Bad approach:

```
Send thousands of raw logs to AI
        ↓
Ask:
"What is wrong?"
```

Better:

```
Logs
Traces
Metrics
Deployments
Dependencies
        ↓
Correlation
        ↓
Structured Evidence
        ↓
AI
```

Example:

```json
{
  "incident": "INC-1023",
  "service": "order-service",
  "errorRate": 18.4,
  "normalErrorRate": 1.2,
  "dependency": {
    "service": "payment-service",
    "latency": 2800,
    "normalLatency": 300
  },
  "timeouts": 142,
  "deployment": {
    "version": "v2.8.1",
    "minutesBeforeIncident": 7
  }
}
```

AI response:

```
The strongest evidence points to the Payment Service.

Evidence:
1. Payment latency increased from approximately 300ms to 2.8s.
2. Payment timeout events increased significantly.
3. Order Service failures correlate with Payment Service degradation.
4. Version v2.8.1 was deployed shortly before the incident.

Recommended first investigation:
Check the changes introduced in Payment Service v2.8.1.
```

---

# 24. 🗣️ AI Debugging Assistant

Users can ask:

```
Why is my checkout API failing?
```

```
What changed before the incident?
```

```
Which service caused this timeout?
```

```
Why is payment slow?
```

```
Show me the most likely root cause.
```

```
What should I check first?
```

Sherlock converts the question into an investigation over available evidence.

---

# 25. 🔬 API Debugger

One of Sherlock's primary features.

Developer enters:

```
Method:
POST

URL:
/api/orders

Headers:
Authorization: Bearer ...

Body:
{
  ...
}
```

Sherlock sends the request and displays:

```
Status: 504
Latency: 3.4s
```

Then:

```
Request
 ↓
Trace
 ↓
Service Timeline
 ↓
Dependencies
 ↓
Logs
 ↓
Metrics
 ↓
Incident
 ↓
Root Cause
 ↓
AI Explanation
```

---

# 26. 🖥️ Frontend Architecture

Frontend:

```
Next.js
React
TypeScript
Tailwind CSS
React Query
Recharts
```

Suggested pages:

```
/login

/dashboard

/projects

/projects/:id

/projects/:id/overview
/projects/:id/api
/projects/:id/traces
/projects/:id/logs
/projects/:id/metrics
/projects/:id/incidents
/projects/:id/deployments
/projects/:id/debug
/projects/:id/settings
```

---

# 27. 📊 Dashboard

Main dashboard:

```
┌─────────────────────────────────────────┐
│ Sherlock                                │
│ Project: E-Commerce                     │
├───────────┬───────────┬─────────────────┤
│ Requests  │ Error Rate│ Avg Latency     │
│ 1.2M      │ 2.4%      │ 340ms           │
├───────────┴───────────┴─────────────────┤
│                                         │
│ Request / Error Graph                   │
│                                         │
├─────────────────────────────────────────┤
│ Active Incidents                        │
│                                         │
│ 🔴 Payment Service Degradation          │
│ 🟡 High API Latency                     │
│                                         │
├─────────────────────────────────────────┤
│ Service Health                          │
│                                         │
│ Gateway      Healthy                    │
│ Orders       Warning                    │
│ Payment      Critical                   │
│ Inventory    Healthy                    │
└─────────────────────────────────────────┘
```

---

# 28. 🔎 Investigation UI

When an incident is selected:

```
Incident #1023

Payment Service Degradation
HIGH

Timeline
────────────────────────────

10:20  Deployment v2.8.1
10:24  Latency increases
10:25  Payment timeouts
10:26  Order API errors
10:27  Incident created
```

Then:

```
Probable Root Cause

Payment Service
Confidence: 91%
```

Then:

```
Evidence

✓ Payment latency increased
✓ Timeout count increased
✓ Order depends on Payment
✓ Error correlation detected
✓ Deployment preceded incident
```

Then:

```
AI Investigation

"Based on the available evidence..."
```

---

# 29. 🗄️ Data Storage Architecture

Sherlock uses different storage technologies for different workloads.

```
                    Sherlock
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   PostgreSQL       OpenSearch       Redis
        │              │              │
 Transactional      Telemetry        Cache
 Data               Search           State
```

---

# 30. PostgreSQL

Use PostgreSQL for structured application data.

Tables:

```
users
organizations
workspaces
memberships
roles
permissions
projects
environments
api_keys
services
incidents
deployments
alerts
rules
audit_logs
```

Do not store millions of raw logs in PostgreSQL.

---

# 31. OpenSearch

OpenSearch is suitable for high-volume searchable telemetry.

Store:

```
logs
traces
spans
API requests
errors
events
```

Example index:

```
sherlock-logs-2026.09.04
```

```
sherlock-traces-2026.09.04
```

```
sherlock-api-events-2026.09.04
```

Time-based indexes help with:

- retention
- performance
- deletion
- scaling

---

# 32. Redis

Redis can handle:

```
Caching
Rate limiting
Temporary correlation state
Sessions
Short-lived investigation state
API key lookup
```

---

# 33. 🏢 Multi-Tenancy

Sherlock is a SaaS platform.

Architecture:

```
Organization
      │
      ├── Workspace
      │       │
      │       ├── Project
      │       │      ├── Environment
      │       │      └── Services
      │       │
      │       └── Project
      │
      └── Workspace
```

Every resource must contain appropriate tenant ownership.

Example:

```
organization_id
workspace_id
project_id
environment_id
```

---

# 34. 🔒 Tenant Isolation

A user from:

```
Organization A
```

must never be able to access:

```
Organization B
```

Even if they know an ID.

Bad:

```sql
SELECT *
FROM incidents
WHERE id = :id;
```

Better:

```sql
SELECT *
FROM incidents
WHERE id = :id
AND organization_id = :organizationId;
```

Tenant validation must happen throughout the backend.

---

# 35. 🔐 Security

Sherlock should implement:

### Authentication

```
JWT
Refresh Tokens
API Keys
```

### Authorization

```
RBAC
```

Roles:

```
OWNER
ADMIN
DEVELOPER
VIEWER
```

### Security controls

```
HTTPS
Input validation
Rate limiting
API key hashing
Password hashing
Secret management
Audit logging
Tenant isolation
CORS
CSRF protection where applicable
Security headers
```

---

# 36. 🔑 API Key Design

Customer applications use a Sherlock API key to send telemetry.

Example:

```
sk_live_xxxxxxxxx
```

Never store raw API keys.

Store:

```
hash(api_key)
```

The displayed key should only be shown once.

---

# 37. 🔄 Complete Data Flow

Example production request:

```
Customer Application
        │
        ▼
OpenTelemetry SDK
        │
        ▼
OpenTelemetry Collector
        │
        ▼
Sherlock Ingestion
        │
        ▼
Kafka
        │
        ├── Logs
        ├── Traces
        ├── Metrics
        ├── API Events
        └── Deployments
                 │
                 ▼
        Processing Services
                 │
                 ▼
        Correlation Service
                 │
                 ▼
        Evidence Graph
                 │
                 ▼
        Analysis Engine
                 │
          ┌──────┴───────┐
          ▼              ▼
      Rule Engine     AI Engine
          │              │
          └──────┬───────┘
                 ▼
            Investigation
                 │
                 ▼
          Sherlock UI
```

---

# 38. 🧬 Evidence Graph

One of Sherlock's strongest concepts.

Instead of treating signals independently:

```
Log
Trace
Metric
Deployment
Dependency
```

Sherlock connects them.

Example:

```
                 Deployment
                     │
                     ▼
              Payment Service
                 /        \
                /          \
          High Latency    Errors
               │             │
               └──────┬──────┘
                      ▼
                 Order Service
                      │
                      ▼
                  API Failure
```

This creates an investigation graph.

---

# 39. 📈 Scalability

Suppose:

```
1,000 API requests/sec
```

and each request touches:

```
10 services
```

Potential telemetry volume:

```
≈ 10,000 service-level events/sec
```

Sherlock must scale horizontally.

Kafka allows:

```
Consumer 1
Consumer 2
Consumer 3
Consumer 4
```

Each consumer processes part of the workload.

---

# 40. ⚡ Backpressure

If ingestion receives:

```
20,000 events/sec
```

while processing can handle:

```
10,000 events/sec
```

Kafka acts as a buffer.

```
Producers
    ↓
Kafka
    ↓
Consumers
```

The backlog can be processed as consumers scale.

---

# 41. 🔁 Retry Strategy

Processing failure:

```
Event
 ↓
Consumer
 ↓
Processing Failed
 ↓
Retry
 ↓
Retry
 ↓
Retry
 ↓
Dead Letter Queue
```

Recommended:

```
Retry topic
DLQ topic
```

Never endlessly retry poison messages.

---

# 42. 🧱 Idempotency

Telemetry may be delivered more than once.

Sherlock should avoid duplicate processing.

Use identifiers such as:

```
eventId
traceId
spanId
requestId
```

Example:

```
eventId = abc123
```

If already processed:

```
ignore duplicate
```

---

# 43. ⏱️ Time Correlation

Many incidents require temporal correlation.

Example:

```
10:20 Deployment
10:23 Latency begins increasing
10:24 Errors increase
10:25 Incident
```

Sherlock should understand:

```
Event A occurred shortly before Event B.
```

This becomes evidence rather than proof.

Important:

> Temporal correlation should increase confidence, not automatically declare causation.
> 

---

# 44. 🚀 Deployment Correlation

Sherlock tracks:

```
Service
Version
Deployment timestamp
Environment
Commit SHA
```

Example:

```
payment-service

v2.7.4
10:00

v2.8.1
10:20
```

If failures begin at:

```
10:23
```

Sherlock marks the deployment as a correlated event.

---

# 45. 🧪 Testing Strategy

## Backend

```
JUnit 5
Mockito
Spring Boot Test
Testcontainers
REST Assured
```

## Infrastructure integration

Use Testcontainers for:

```
PostgreSQL
Kafka
Redis
OpenSearch
```

## Frontend

```
Jest
React Testing Library
Playwright
```

---

# 46. 🧪 Testing Pyramid

```
              E2E Tests
                 ▲
                 │
        Integration Tests
                 ▲
                 │
           Unit Tests
                 ▲
```

Most tests should be unit tests.

Critical flows should have integration and E2E coverage.

---

# 47. 📦 Repository Structure

Recommended monorepo:

```
sherlock/
│
├── README.md
├── pom.xml
├── docker-compose.yml
│
├── api-gateway/
│
├── identity-service/
│
├── project-service/
│
├── ingestion-service/
│
├── trace-service/
│
├── log-service/
│
├── metrics-service/
│
├── correlation-service/
│
├── incident-service/
│
├── analysis-service/
│
├── ai-service/
│
├── frontend/
│
├── infrastructure/
│   ├── kafka/
│   ├── postgres/
│   ├── redis/
│   ├── opensearch/
│   └── otel-collector/
│
├── deployment/
│   ├── docker/
│   └── kubernetes/
│
└── docs/
```

---

# 48. ☕ Backend Stack

Use:

```
Java 21
Spring Boot
Spring Security
Spring Cloud Gateway
Spring Data JPA
Hibernate
Maven
```

Recommended supporting libraries:

```
Lombok
MapStruct
Flyway
Bean Validation
OpenAPI
Micrometer
OpenTelemetry
Resilience4j
```

---

# 49. 🌐 Frontend Stack

```
Next.js
React
TypeScript
Tailwind CSS
React Query
Recharts
```

Optional future additions:

```
React Flow
Monaco Editor
Zustand
```

React Flow can be useful for the service dependency graph.

---

# 50. 🐳 Local Development

Docker Compose should provide:

```
PostgreSQL
Redis
Kafka
Kafka UI
OpenSearch
OpenTelemetry Collector
```

Application services can initially run directly from the IDE.

Example:

```
Docker
 ├── PostgreSQL
 ├── Redis
 ├── Kafka
 ├── OpenSearch
 └── OTel Collector

Local JVM
 ├── API Gateway
 ├── Identity
 ├── Project
 ├── Ingestion
 └── Analysis
```

This keeps development faster.

---

# 51. ☁️ Production AWS Architecture

Recommended production architecture:

```
                    Internet
                       │
                       ▼
                Load Balancer
                       │
                       ▼
                    EKS
                       │
       ┌───────────────┼────────────────┐
       │               │                │
       ▼               ▼                ▼
   API Gateway      Services         Workers
       │               │                │
       └───────────────┼────────────────┘
                       │
              ┌────────┼────────┐
              ▼        ▼        ▼
             RDS   ElastiCache  MSK
           Postgres    Redis    Kafka
                       │
                       ▼
                  OpenSearch
```

AWS services:

```
EKS
RDS PostgreSQL
ElastiCache Redis
MSK Kafka
OpenSearch Service
S3
CloudWatch
ECR
Route 53
ALB
```

---

# 52. ☸️ Kubernetes

Production services can run as:

```
Deployment
Service
ConfigMap
Secret
HorizontalPodAutoscaler
Ingress
```

Example:

```
sherlock-ingestion
replicas: 3
```

During high load:

```
3 pods
 ↓
6 pods
 ↓
12 pods
```

---

# 53. 📈 Observability of Sherlock Itself

Sherlock must monitor Sherlock.

Use:

```
OpenTelemetry
Prometheus
Grafana
OpenSearch
```

Track:

```
Kafka lag
API latency
Error rate
CPU
Memory
Database latency
OpenSearch health
Consumer throughput
Queue depth
```

---

# 54. 🔄 CI/CD Pipeline

```
Developer
    ↓
Git Push
    ↓
GitHub
    ↓
GitHub Actions
    ↓
Build
    ↓
Unit Tests
    ↓
Integration Tests
    ↓
Security Scan
    ↓
Docker Build
    ↓
Push to ECR
    ↓
Deploy to Kubernetes
    ↓
Health Check
    ↓
Production
```

---

# 55. 🛡️ Reliability Features

Sherlock should eventually support:

```
Timeouts
Retries
Circuit Breakers
Bulkheads
Rate Limiting
Backpressure
Dead Letter Queues
Idempotency
Health Checks
Graceful Shutdown
```

For service-to-service calls:

```
Resilience4j
```

can provide:

```
Circuit Breaker
Retry
Time Limiter
Bulkhead
```

---

# 56. 📋 API Design

Example APIs:

## Authentication

```
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/refresh
POST /api/v1/auth/logout
```

## Projects

```
POST /api/v1/projects
GET /api/v1/projects
GET /api/v1/projects/{id}
PUT /api/v1/projects/{id}
DELETE /api/v1/projects/{id}
```

## API Keys

```
POST /api/v1/projects/{id}/api-keys
GET /api/v1/projects/{id}/api-keys
DELETE /api/v1/api-keys/{id}
```

## Debugging

```
POST /api/v1/debug/request
GET /api/v1/debug/{requestId}
GET /api/v1/debug/{requestId}/trace
GET /api/v1/debug/{requestId}/logs
GET /api/v1/debug/{requestId}/analysis
```

## Incidents

```
GET /api/v1/incidents
GET /api/v1/incidents/{id}
POST /api/v1/incidents/{id}/acknowledge
POST /api/v1/incidents/{id}/resolve
```

## Telemetry

```
POST /v1/ingest
```

---

# 57. 🗃️ Core Database Model

High-level:

```
users
 │
 ├── memberships
 │
 ▼
organizations
 │
 ▼
workspaces
 │
 ▼
projects
 │
 ├── environments
 │
 ├── services
 │
 ├── api_keys
 │
 ├── incidents
 │
 └── deployments
```

Example:

```
users
organizations
organization_members
workspaces
workspace_members
projects
environments
services
api_keys
incidents
incident_events
deployments
alerts
rules
audit_logs
```

Telemetry itself should primarily live in OpenSearch.

---

# 58. 📊 Service Dependency Graph

Sherlock should automatically construct:

```
Gateway
   │
   ├──────────────┐
   ▼              ▼
Order          User
   │
   ├──────────────┐
   ▼              ▼
Payment       Inventory
   │
   ▼
External Payment API
```

This graph can help identify:

```
Critical dependencies
High-error dependencies
High-latency dependencies
Failure propagation
```

---

# 59. 🚨 Incident Severity

Recommended:

```
P0 - Critical
P1 - High
P2 - Medium
P3 - Low
```

Example:

### P0

Complete production outage.

### P1

Major functionality unavailable.

### P2

Significant degradation.

### P3

Minor issue.

---

# 60. 🧠 Sherlock Investigation Lifecycle

```
Detection
   ↓
Incident Creation
   ↓
Evidence Collection
   ↓
Correlation
   ↓
Analysis
   ↓
Root Cause Candidates
   ↓
Confidence Scoring
   ↓
AI Explanation
   ↓
Developer Investigation
   ↓
Resolution
   ↓
Incident Closed
```

---

# 61. 📝 Investigation Record

Example:

```json
{
  "incidentId": "INC-1023",
  "projectId": "project-123",
  "environment": "production",
  "severity": "HIGH",
  "status": "INVESTIGATING",
  "affectedService": "order-service",
  "probableRootCause": "payment-service",
  "confidence": 0.91
}
```

Evidence:

```json
{
  "type": "DEPENDENCY_LATENCY",
  "service": "payment-service",
  "observed": 2800,
  "baseline": 300
}
```

---

# 62. 🤖 AI Provider Architecture

Do not tightly couple Sherlock to one AI provider.

Use an abstraction:

```
AI Service
    │
    ├── LLM Provider Interface
    │
    ├── Provider A
    │
    ├── Provider B
    │
    └── Provider C
```

This allows changing providers later.

Possible future providers:

```
Gemini
OpenAI
Claude
Groq
OpenRouter
```

The important architecture is:

```java
interface AiProvider {
    InvestigationExplanation explain(
        InvestigationContext context
    );
}
```

---

# 63. 🧠 AI Safety / Reliability Principle

AI output should never directly become:

```
ROOT CAUSE = X
```

without evidence.

Instead:

```
Deterministic Analysis
        ↓
Evidence
        ↓
Candidate Root Causes
        ↓
AI Explanation
```

AI should produce:

```
Most likely cause
Supporting evidence
Confidence
Recommended investigation
Possible alternatives
```

---

# 64. 🔮 Future AI Features

Later Sherlock can support:

### Natural-language investigation

```
"Why is checkout slow?"
```

### Incident summarization

```
"Summarize this incident."
```

### Historical comparison

```
"Has this happened before?"
```

### Suggested investigation

```
"What should I check next?"
```

### Fix suggestions

```
"How can this issue be fixed?"
```

### RAG

Sherlock could retrieve:

```
Previous incidents
Runbooks
Architecture documentation
Known errors
Past fixes
```

Then provide contextual recommendations.

---

# 65. 🚀 MVP

Do NOT build everything initially.

The first version should answer one question:

> **Why did this API request fail?**
> 

MVP:

```
Application
    ↓
OpenTelemetry
    ↓
Collector
    ↓
Ingestion
    ↓
Kafka
    ↓
Processing
    ↓
OpenSearch
    ↓
Correlation
    ↓
Rule-Based RCA
    ↓
Dashboard
```

MVP features:

```
✓ User authentication
✓ Project creation
✓ API keys
✓ Telemetry ingestion
✓ Logs
✓ Traces
✓ API requests
✓ Log search
✓ Trace visualization
✓ Request/trace correlation
✓ Basic RCA
✓ Debug dashboard
```

AI can be added after this pipeline works.

---

# 66. 🗺️ Development Roadmap

## Phase 1 — Foundation

```
Git repository
Project structure
Docker Compose
PostgreSQL
Redis
Kafka
OpenSearch
API Gateway
```

---

## Phase 2 — Authentication

```
Registration
Login
JWT
Refresh tokens
RBAC
Organizations
Workspaces
```

---

## Phase 3 — Projects

```
Project creation
Environments
API keys
Service registration
Project settings
```

---

## Phase 4 — Telemetry

```
OpenTelemetry Collector
Log ingestion
Trace ingestion
Metric ingestion
Kafka pipelines
```

---

## Phase 5 — Observability

```
Log search
Trace viewer
API request viewer
Metrics dashboard
Service health
Dependency graph
```

---

## Phase 6 — Correlation

```
Request → Trace
Trace → Logs
Service → Dependency
Error → Request
Incident → Deployment
```

---

## Phase 7 — Incident Management

```
Error spike detection
Latency anomaly detection
Incident creation
Severity
Incident timeline
Incident history
```

---

## Phase 8 — Root Cause Analysis

```
Rule engine
Dependency analysis
Latency analysis
Error propagation
Deployment correlation
Confidence scoring
```

---

## Phase 9 — AI

```
AI context generation
Incident summaries
Root cause explanation
Natural-language debugging
Suggested investigation
Historical comparison
RAG
```

---

## Phase 10 — Production

```
Kubernetes
AWS
Autoscaling
CI/CD
Security scanning
Monitoring
Alerting
Backups
Disaster recovery
Load testing
Rate limiting
Production documentation
```

---

# 67. 🎯 First Production Milestone

The first meaningful milestone is:

```
A developer can connect an application
to Sherlock and investigate a failed API request.
```

Complete flow:

```
1. Create Sherlock account
2. Create project
3. Create API key
4. Connect application
5. Generate API request
6. Request fails
7. Sherlock receives telemetry
8. Sherlock correlates trace + logs
9. Sherlock identifies probable cause
10. Developer opens investigation
11. Sherlock explains evidence
```

---

# 68. 📊 Example User Journey

### Step 1

Developer creates:

```
Project:
E-Commerce
```

### Step 2

Sherlock generates:

```
API Key
```

### Step 3

Developer integrates:

```
OpenTelemetry
```

### Step 4

Application sends:

```
POST /api/orders
```

### Step 5

Request fails:

```
504
```

### Step 6

Sherlock detects:

```
Payment Service timeout
```

### Step 7

Dashboard:

```
Order API
ERROR RATE: 18%
LATENCY: 3.4s
```

### Step 8

Sherlock investigation:

```
Probable Root Cause:
Payment Service

Confidence:
91%
```

### Step 9

AI:

```
Payment Service appears to be the strongest
candidate because its latency and timeout rate
increased immediately before Order Service failures.
```

---

# 69. 🏆 Competitive Positioning

Do not position Sherlock as:

> "An AI that analyzes logs."
> 

Better:

> **"Sherlock turns a failing API request into an evidence-backed investigation across your distributed system."**
> 

The differentiation is:

```
API Request
     ↓
Trace
     ↓
Logs
     ↓
Metrics
     ↓
Dependencies
     ↓
Deployments
     ↓
Evidence
     ↓
Root Cause
     ↓
AI Explanation
```

---

# 70. 🧠 Core Architecture Principle

This is the most important architectural rule for Sherlock:

```
                 ❌ BAD

Logs → AI → Root Cause
```

Instead:

```
                 ✅ GOOD

Logs
Traces
Metrics
Deployments
Dependencies
     ↓
Correlation
     ↓
Evidence Graph
     ↓
Deterministic Analysis
     ↓
Root Cause Candidates
     ↓
AI Explanation
```

This makes Sherlock:

```
Reliable
Explainable
Debuggable
Testable
AI-independent
```

---

# 71. 📈 Long-Term Vision

Sherlock can eventually evolve into:

```
OBSERVE
   ↓
UNDERSTAND
   ↓
ACT
```

### Observe

```
Logs
Traces
Metrics
APIs
Infrastructure
Deployments
```

### Understand

```
Correlation
Incidents
Root Cause
AI Investigation
```

### Act

Future capabilities:

```
Fix suggestions
Runbook execution
Rollback recommendations
Automated remediation
Developer approval
```

The final vision:

```
Production Problem
       ↓
Sherlock Detects
       ↓
Sherlock Investigates
       ↓
Sherlock Explains
       ↓
Sherlock Suggests Fix
       ↓
Developer Approves
       ↓
Sherlock Helps Remediate
```

---

# 72. 🏁 Final Architecture

```
                         ┌──────────────────┐
                         │    Developers    │
                         └────────┬─────────┘
                                  │
                                  ▼
                    ┌─────────────────────────┐
                    │   Sherlock Web App      │
                    │ Next.js + React + TS    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │      API Gateway        │
                    │ Spring Cloud Gateway     │
                    └────────────┬────────────┘
                                 │
             ┌───────────────────┼───────────────────┐
             │                   │                   │
             ▼                   ▼                   ▼
       Identity Service    Project Service      Debug API
             │                   │                   │
             └───────────────────┼───────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Ingestion Service    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │          Kafka           │
                    └────────────┬────────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          │                      │                      │
          ▼                      ▼                      ▼
       Logs                  Traces                 Metrics
     Processing            Processing              Processing
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Correlation Engine    │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     Evidence Graph      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     Analysis Engine     │
                    └────────────┬────────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
                    ▼                         ▼
             Rule-Based RCA              AI Engine
                    │                         │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     Investigation       │
                    │ Root Cause + Evidence   │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Sherlock Dashboard   │
                    └─────────────────────────┘

      ┌──────────────────────────────────────────────────┐
      │                    STORAGE                       │
      │                                                  │
      │ PostgreSQL │ OpenSearch │ Redis │ Object Storage │
      └──────────────────────────────────────────────────┘
```

---

# 73. 📌 Technology Summary

```
Frontend
────────
Next.js
React
TypeScript
Tailwind CSS
React Query
Recharts

Backend
───────
Java 21
Spring Boot
Spring Security
Spring Cloud Gateway
Spring Data JPA
Hibernate
Maven

Messaging
─────────
Apache Kafka

Telemetry
─────────
OpenTelemetry
OpenTelemetry Collector

Storage
───────
PostgreSQL
OpenSearch
Redis

Observability
─────────────
Prometheus
Grafana
OpenTelemetry

AI
──
LLM Provider Abstraction
RAG
Embeddings
Vector Database (future)

Infrastructure
──────────────
Docker
Kubernetes
AWS
GitHub Actions

Testing
───────
JUnit 5
Mockito
Testcontainers
REST Assured
Jest
React Testing Library
Playwright
```

---

# 74. 💼 Resume Description

> **Sherlock — AI-Powered API Debugging & Distributed Observability Platform:** Built a multi-tenant observability and API debugging platform using Java, Spring Boot, Kafka, PostgreSQL, OpenSearch, Redis, and OpenTelemetry to correlate API requests, distributed traces, logs, metrics, service dependencies, and deployments; implemented deterministic root-cause analysis with confidence scoring and an AI investigation layer for evidence-backed incident diagnosis.
> 

---

# 75. 🎤 One-Minute Interview Explanation

If an interviewer asks:

**"Tell me about Sherlock."**

Answer:

> Sherlock is a distributed observability and API debugging platform I designed to solve the problem of debugging failures in microservice-based applications. When an API request fails, Sherlock collects telemetry through OpenTelemetry, sends events through Kafka, stores structured data in PostgreSQL and high-volume telemetry in OpenSearch, and correlates requests, traces, logs, metrics, dependencies, and deployments. It then uses deterministic rules to identify probable root causes and confidence scores. An optional AI layer takes this structured evidence and explains the investigation in natural language. The key design principle is that AI doesn't guess the root cause; Sherlock first builds evidence and performs deterministic analysis, while AI explains the findings to the developer.
> 

---

# 76. ⭐ Sherlock's Core Value Proposition

### Without Sherlock

```
API Failure
   ↓
Open Logs
   ↓
Open Traces
   ↓
Open Metrics
   ↓
Check Deployment
   ↓
Check Database
   ↓
Check Dependencies
   ↓
Manually Correlate
   ↓
Find Root Cause
```

### With Sherlock

```
API Failure
   ↓
Sherlock
   ↓
Collect Evidence
   ↓
Correlate Signals
   ↓
Analyze
   ↓
Root Cause
   ↓
AI Explanation
```

---

# 77. 🧭 Final Product Philosophy

Sherlock should not attempt to replace developers.

It should remove the repetitive investigation work.

The developer should spend less time asking:

```
"Where should I look?"
```

and more time asking:

```
"How should I fix this?"
```

Therefore:

> **Sherlock is not just an observability dashboard. It is an investigation system for distributed applications.**
> 

---

# 78. 🚀 Final Goal

The ultimate goal of Sherlock is:

> **Give a developer a failing API request and let Sherlock explain what happened, why it happened, what evidence supports the conclusion, and what the developer should investigate or fix next.**
> 

```
              🕵️ SHERLOCK

       "Every failure leaves clues."

API Failure
     ↓
Evidence
     ↓
Correlation
     ↓
Investigation
     ↓
Root Cause
     ↓
Explanation
     ↓
Resolution
```

**Project:** Sherlock

**Category:** Developer Observability SaaS

**Architecture:** Event-Driven Microservices

**Primary Backend:** Java + Spring Boot

**Core Messaging:** Kafka

**Telemetry:** OpenTelemetry

**Primary Principle:** Evidence First, AI Second