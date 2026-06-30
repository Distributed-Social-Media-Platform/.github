# Social Media Feed Engine (Microservices Architecture)

An event-driven, horizontally scalable microservices ecosystem designed to handle high-throughput content creation and low-latency, real-time feed materialization. This project demonstrates advanced distributed systems principles, caching topologies, and automated cloud orchestration.

---

## 🔗 Project Artifacts
* **Engineering Design Document:** [View Architecture Specifications & Deep Dives](https://docs.google.com/document/d/1nsxwAS25OM2eRazXJoB9zwbnMFIg-dViZ0xl2HduEL0/edit?tab=t.0)
* **End-to-End System Demo:** [Watch the Deployment & Load Testing Video](https://www.youtube.com/watch?v=o-gpOsAnS_w)

---

## 🏗️ System Architecture
The platform transitions traditional relational data models into an **Event-Driven Fan-Out on Write (Push)** architecture to achieve $O(1)$ feed retrieval times.

### **Core Microservices**
* **Auth Service:** Stateless identity management issuing cryptographically signed JWTs. Features a reactive Redis-backed denylist for secure token revocation during logout.
* **Post Service:** Ingests user content, persists records to Amazon RDS (PostgreSQL), and publishes asynchronous content creation events.
* **Feed Service:** Serves materialized user timelines directly from distributed **Amazon ElastiCache (Redis)** sorted sets, bypassing heavy relational table joins.

### **Infrastructure & Integration Tier**
* **Asynchronous Messaging:** Utilizes message middleware (Amazon SQS / Event Streams) to guarantee at-least-once delivery semantics and decouple write-heavy ingestion from read-heavy distribution.
* **Orchestration Engine:** Fully containerized architecture deployed on an **Amazon EKS (Elastic Kubernetes Service)** cluster with fine-grained access control managed via **OIDC Identity Federation (IRSA)**.

---

## 📊 Performance & Load Testing
The system was subjected to rigorous stress testing up to 1,000 concurrent virtual users to identify compute-bound boundaries, database I/O saturation thresholds, and scale limitations.

* **100 Users:** Optimal performance achieving **11.31 RPS** with a **99.16% Success Rate**.
* **500+ Users:** System experiences non-linear degradation, exposing upstream resource constraints and network latency gates (stabilizing at $\approx$ 60-second timeouts).

---

## 🚀 Technical Highlights & Best Practices
* **Stateful Idempotency:** Implemented deterministic key-sorting and normalized JSON payloads within Redis Sorted Sets to ensure event retries result in stateful *No-Ops* rather than duplicate timeline data.
* **Layer-4 & Layer-7 Routing:** Structured utilizing Kubernetes services, ClusterIP abstractions, NodePort boundaries, and AWS Application Load Balancers (ALBs).
* **Cloud Economics (FinOps):** Fully documented Total Cost of Ownership (TCO) analysis accounting for EKS control planes, compute capacity nodes, and load balancer capacity units (LCUs).

---




