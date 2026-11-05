# Phase 3 end-of-phase exam

← [Phase 3](../README.md)

| Item | Details |
| --- | --- |
| When | Week 37, after all 31 Phase 3 topic CBTs are attempted |
| Where | Moodle (itlearning.tech), "Phase 3 end-of-phase exam" quiz |
| Format | **60 scenario-based multiple-choice questions**, each a short workplace situation with one best answer, drawn at random for every attempt from a larger bank (the module weights below are fixed) |
| Time | **2 hours** (120 minutes), one sitting; the timer keeps running if you close the browser |
| Pass mark | **70%** |
| Attempts | 2, at least 48 hours apart; highest grade counts |
| Order | Questions and answer options are shuffled for every attempt |
| Resources | Closed book. No IDE, no AI assistants, no notes. **From 1 December 2026, taken in [Safe Exam Browser](../../../docs/assessments/safe-exam-browser.md)** (Windows, macOS or iPad) |
| Review | Correct answers and explanations are shown after the exam closes for the cohort |

## Blueprint

| Module | Questions | Weight | Topics |
| --- | --- | --- | --- |
| 10. Microservices & Distributed Systems | 9 | 15% | Domain-driven design and boundaries (10.1), gateway, config and discovery (10.2), resilience between services (10.3), data consistency across services (10.4) |
| 11. Event-Driven Architecture with Kafka, and Batch Processing | 11 | 18% | Kafka fundamentals (11.1), Spring for Apache Kafka and RabbitMQ (11.2), outbox, sagas and idempotent consumers (11.3), dead letters, retries and schemas (11.4), Spring Batch (11.5) |
| 12. Kubernetes, Helm & Cloud on AWS | 10 | 17% | Kubernetes objects (12.1), probes, resources and autoscaling (12.2), Helm and local clusters (12.3), AWS essentials and EKS (12.4), Terraform and GitOps (12.5) |
| 13. Observability, Performance & Production Support | 10 | 17% | OpenTelemetry (13.1), Prometheus, Grafana and logging (13.2), JVM profiling and heap analysis (13.3), caching and databases under load (13.4), load testing, SLOs and incidents (13.5) |
| 14. Reactive & High-Throughput Java | 6 | 10% | WebFlux and Reactor (14.1), R2DBC (14.2), reactive versus virtual threads (14.3) |
| 15. AI Integration with Spring AI | 7 | 11% | ChatClient (15.1), RAG with pgvector (15.2), tool calling and MCP (15.3), guardrails, evaluation and cost (15.4) |
| 16. Capstone, System Design & Interview Prep | 7 | 12% | Finishing a system (16.1), deployment, ADRs and runbooks (16.2), the final load report (16.3), system-design interviews (16.4), mock interviews (16.5) |
| **Total** | **60** | **100%** | |

## What the questions look like

Two examples in the exam's style (these two are not in the exam):

**Example 1.** The transfers service saves a transfer and then publishes an event to Kafka. Occasionally a transfer is saved but the ledger never hears about it. What is the most robust fix?

- A. Publish the event first, then save the transfer
- B. Retry the publish three times
- C. Write the event to an outbox table in the same transaction, and have a relay publish it
- D. Use a longer Kafka timeout

<details><summary>Answer</summary>

**C.** The outbox makes the state change and the event one atomic write; the relay may publish twice, so consumers must be idempotent.

</details>

**Example 2.** After a release, pods restart every few minutes under load, although the service answers requests normally between restarts. The liveness probe calls an endpoint that checks the database. What is the most likely cause?

- A. The liveness probe depends on the database, so a slow database gets healthy pods killed
- B. The readiness probe is missing
- C. The image is too large
- D. The HPA is scaling down

<details><summary>Answer</summary>

**A.** Liveness should only say whether the process itself is stuck; dependencies belong in readiness, which takes a pod out of traffic without restarting it.

</details>

## How to prepare

1. Retake all 31 Phase 3 topic CBTs without notes until you score 90%+.
2. Re-read each lesson's "Common mistakes" section.
3. Explain ITL Pay aloud: the saga and its compensations, the outbox, consumer idempotency, the deployment path, your SLOs and your load-test findings.
4. Practise under time pressure: about 2 minutes per question.

