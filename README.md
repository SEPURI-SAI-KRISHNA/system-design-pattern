# System Design Patterns 🏗️

A practical, interview-oriented collection of **system design patterns** used to build **scalable, reliable, and high-performance distributed systems**.

This repository focuses on **why a pattern exists**, **when to use it**, **trade-offs**, and **real-world examples** — not just definitions.

---

## 📌 Why this Repository?

Most system design resources:
- Explain patterns in isolation  
- Miss real-world trade-offs  
- Don’t connect patterns to **interview decision-making**

This repo is built to:
- Help you **think like a senior engineer**
- Improve **system design interviews**
- Serve as a **quick reference** during architecture discussions

---

## 🧠 What is a System Design Pattern?

A system design pattern is a **reusable solution** to a **recurring architectural problem** in distributed systems, such as:
- Scaling reads/writes
- Handling failures
- Reducing latency
- Managing consistency
- Controlling traffic

Patterns are **not frameworks** — they are **thinking tools**.

---

## 📚 Patterns Covered

### 🔹 Scalability Patterns
- Horizontal Scaling
- Vertical Scaling
- Sharding (Data Partitioning)
- Consistent Hashing
- Read Replicas
- Caching (Client, CDN, Server-side)

### 🔹 Reliability & Fault Tolerance
- Replication
- Leader–Follower
- Heartbeat
- Failover
- Circuit Breaker
- Bulkhead Pattern

### 🔹 Performance & Latency
- Caching Strategies
- Load Balancing Algorithms
- Asynchronous Processing
- Batching
- Backpressure

### 🔹 Data Management
- CQRS (Command Query Responsibility Segregation)
- Event Sourcing
- Two-Phase Commit (2PC)
- Saga Pattern
- Idempotency

### 🔹 Communication Patterns
- Request–Response
- Pub/Sub
- Message Queues
- Streaming (Kafka-style)
- WebSockets

### 🔹 Consistency Patterns
- Strong vs Eventual Consistency
- Quorum Reads/Writes
- Read-Your-Writes
- Write-Through vs Write-Back Cache

---
