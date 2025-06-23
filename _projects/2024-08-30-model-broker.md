---
layout: post
title: "Building a Smarter Model Broker: Evolving ML Inference at Scale"
date: 2025-06-20
author: Josh Rosen
tags: [mlops, kubernetes, sagemaker, devops, infrastructure, best-egg]
description: How we evolved Best Egg's model broker from serverless roots to a high-performance Kubernetes-native orchestration platform for machine learning inference.
---

In my role as Senior MLOps Engineer at Best Egg, I led the development of a system we call **Model Broker**—a centralized orchestration layer for machine learning model inference across the organization. What began as a simple way to unify how downstream services consumed ML predictions evolved into a foundational component of our ML platform. Today, Model Broker enables scalable, low-latency, and feature-aware model execution across a variety of teams and use cases.

This post walks through the evolution of the system, key architectural decisions, and the core features that make it powerful.

---

## From Serverless to Kubernetes-Native: An Architectural Evolution

### Initial Serverless Implementation

The first version of Model Broker was built using AWS Step Functions. This serverless design offered several early benefits:

- **Automatic scaling** based on request volume  
- **Pay-per-use cost structure**, making it financially efficient for lower-volume models  
- **Tight integration with AWS services**, streamlining our early workflows  

But as we scaled up, the limitations became too significant to ignore:

- **Cold start latency** was unacceptable for real-time applications  
- **State management** within Step Functions became complex and brittle  
- **Execution environment control** was minimal, limiting optimization and monitoring  

We needed something more performant and flexible—without giving up the abstraction benefits Model Broker provided.

### Kubernetes-Native Redesign

The second iteration of Model Broker was a complete architectural overhaul. We transitioned to a **Kubernetes-native implementation**, which addressed our key limitations and unlocked new capabilities:

- **Persistent services** eliminated cold starts and dramatically reduced latency  
- **Better resource control** allowed for smarter utilization and tuning  
- **Native observability** improved debugging, metrics, and alerting  
- **Seamless integration** with Kubernetes-native tools and operators  

This shift brought us closer to a platform model, where deploying, updating, and monitoring models became far more reliable and repeatable.

---

## Core Features of Model Broker

### Intelligent Model Routing

Model Broker intelligently routes inference requests to the correct model version based on the request path and metadata:

- **Dynamic version selection** ensures the right model is used per request context  
- **Multiple versions** of the same model can coexist  
- **Custom routing logic** supports rollout strategies, A/B testing, and shadow deployment  

This routing layer abstracts away the complexity of model versioning from the consumer entirely.

---

### Feature Management and Abstraction

One of Model Broker’s most transformative capabilities is **decoupling feature engineering from consuming services**.

- Consumers send **basic input data** (e.g., customer ID, loan ID)  
- Model Broker fetches and computes all **required features** for the specific model being invoked  
- Models can **evolve independently** of downstream services  

Thanks to integration with our internal **Feature Platform**, features are dynamically computed, cached, and retrieved as needed. This design dramatically reduces coordination costs between data scientists and application teams.

**The result?**

- Consumers don’t need to know what features the model requires  
- Models can be retrained, redeployed, or updated with new features—**no consumer code changes necessary**  

---

### Parallel Model Execution

In many use cases, a single request may trigger predictions from **multiple models**. Model Broker supports:

- **Concurrent execution** across models  
- **Aggregated responses** returned in a unified payload  
- **Optimized resource scheduling** to avoid contention and maximize throughput  

This is especially useful in decisioning systems that rely on ensemble or multi-model outputs.

---

### Auditability and Compliance

In a regulated environment, transparency is non-negotiable. Model Broker offers robust observability and traceability features:

- **All executions are logged** with full context  
- **Artifacts** (inputs, outputs, metadata) are stored in **Amazon S3**  
- **Execution metadata** is persisted in **DynamoDB**  
- **Full audit trails** support compliance, debugging, and reproducibility  

This has been essential for supporting both internal audits and external regulatory requirements.

---

## Under the Hood: Technical Implementation

### Kubernetes Integration

We lean heavily on Kubernetes-native patterns to manage our infrastructure:

- **Custom Resource Definitions (CRDs)** define model metadata and routing logic  
- **Kubernetes Operators** automate model lifecycle events (deployments, upgrades, monitoring)  
- Support for both **in-cluster** and **external** inference backends  

### Flexible Model Hosting

We support two deployment backends:

1. **AWS SageMaker**  
   Ideal for models that require GPU acceleration, elastic scaling, or integration with SageMaker pipelines.

2. **Kubernetes Services**  
   Used for lower-latency, in-cluster models where we want more direct control and observability.

This flexibility ensures the right balance of **cost**, **performance**, and **capability** for each use case.

---

## Performance and Optimization

As usage grew, we invested in critical optimizations:

- **Connection pooling** to backend inference services  
- **Aggressive caching** of features and routing rules  
- **Load balancing** across replicas for throughput and fault tolerance  
- **Efficient memory allocation** per model based on profiling  

These improvements helped us scale horizontally without increasing latency or cost disproportionately.

---

## Impact and Business Value

The move to a Kubernetes-native Model Broker was a turning point:

- **Latency dropped** significantly across all endpoints  
- **System reliability** improved, with fewer timeouts and edge case failures  
- **Monitoring and alerts** became proactive instead of reactive  
- **Operational complexity decreased**, thanks to automation via CRDs and GitOps  

But most importantly, the **model development lifecycle** was transformed:

- Data scientists can **deploy updates without breaking consumers**  
- New models can be rolled out faster  
- Features can evolve independently from the services that consume them  

This autonomy unlocked a virtuous cycle of faster experimentation, more accurate models, and better outcomes for the business.

---

## Tech Stack

Model Broker is built on a robust stack of tools:

- **Kubernetes** – orchestration and service management  
- **Python** – core service logic and model interfaces  
- **AWS SageMaker** – optional hosting for specialized models  
- **Amazon S3** – artifact and log storage  
- **Amazon DynamoDB** – metadata and execution tracking  
- **Custom Kubernetes Operators** – automation of deployment workflows  
- **Feature Platform Integration** – real-time feature resolution and caching  

---

## Final Thoughts

Model Broker is more than a routing layer—it’s a **platform for scalable, maintainable, and consumer-agnostic model inference**. By abstracting feature computation, model versioning, and execution environments, we’ve dramatically simplified the experience of deploying machine learning at scale.

This project taught us a lot about the tradeoffs between serverless and containerized architectures, the importance of observability, and the power of true decoupling between data science and engineering.

And we’re just getting started.
