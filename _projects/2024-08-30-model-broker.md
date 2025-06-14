---
title: Model Broker
subtitle: A High-Performance Model Orchestration Service
description: A Kubernetes-native service for orchestrating machine learning model inference at scale, built during my time as Senior MLOps Engineer at Best Egg.
featured_image: /images/demo/model-broker.jpg
---

# Model Broker: ML Model Orchestration at Scale

## Overview

Model Broker is a sophisticated model orchestration service I developed while serving as Senior MLOps Engineer at Best Egg. It serves as a central hub for managing and executing machine learning model inference across the organization, providing a unified interface for consuming services to access our ML capabilities.

## Architecture Evolution

### Initial Serverless Implementation
The first iteration of Model Broker was built as a serverless application using AWS Step Functions. This architecture provided:
- Automatic scaling
- Pay-per-use cost model
- Integration with AWS services

However, this approach faced challenges with:
- Cold start latencies
- Complex state management
- Limited control over execution environment

### Kubernetes-Native Redesign
The current version represents a significant architectural shift to a Kubernetes-native service, addressing the limitations of the serverless approach. Key improvements include:
- Reduced latency through persistent service deployment
- Better resource utilization
- Enhanced monitoring and observability
- Native integration with Kubernetes ecosystem

## Key Features

### Intelligent Model Routing
- Dynamic model selection based on request path
- Support for multiple model versions
- Flexible model deployment strategies

### Feature Management
- Integration with Feature Platform
- Dynamic feature computation
- Efficient feature caching and retrieval
- **Consumer-Independent Feature Management**: One of the most powerful aspects of the Model Broker is its tight integration with our Feature Platform. This integration creates a crucial abstraction layer where:
  - Consumers only need to provide basic input data
  - Model Broker automatically determines and fetches all required features for each model
  - Models can be retrained and redeployed with new feature requirements without consumer coordination
  - This decoupling enables rapid model iteration and deployment without impacting downstream services

### Parallel Model Execution
- Concurrent model inference
- Aggregated response payload
- Optimized resource utilization

### Audit and Compliance
- Comprehensive logging of all model executions
- Artifact storage in S3
- Metadata tracking in DynamoDB
- Full audit trail for compliance and debugging

## Technical Implementation

### Kubernetes Integration
The service leverages Kubernetes-native features through:
- Custom Resource Definitions (CRDs) for model definitions
- Automated model deployment via Kubernetes operators
- Support for both SageMaker and native Kubernetes deployments

### Model Deployment
Models can be deployed in two ways:
1. **SageMaker Endpoints**: For models requiring specialized hardware or SageMaker features
2. **Kubernetes Services**: For models that can run efficiently in the cluster

### Performance Optimizations
- Efficient resource allocation
- Connection pooling
- Caching strategies
- Load balancing

## Impact

The transition to a Kubernetes-native architecture has resulted in:
- Significantly reduced latency
- Improved reliability
- Better resource utilization
- Enhanced monitoring capabilities
- Simplified operational management
- **Accelerated Model Development**: The consumer-independent feature management system has dramatically improved our model development velocity:
  - Models can be updated and redeployed without coordinating with consuming services
  - Data scientists can iterate on models more quickly
  - New features can be added to models without requiring consumer changes
  - Reduced deployment complexity and risk

## Technologies Used

- Kubernetes
- Python
- AWS SageMaker
- DynamoDB
- S3
- Custom Kubernetes Operators
- Feature Platform Integration

This project demonstrates the evolution of ML infrastructure from serverless to containerized architectures, highlighting the importance of choosing the right deployment strategy based on specific requirements and constraints. The integration with the Feature Platform represents a significant architectural achievement, enabling true independence between model development and consumer services. 