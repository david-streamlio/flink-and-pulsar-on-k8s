# Getting Started with Pulsar and Flink on Kubernetes

This repository contains a collection of instructions the guide you through the process of installing Flink inside
your K8s environment, developing applications that use both Apache Flink and Apache Pulsar, and deploying the
applications inside a K8s environment.

------------
Requirements
------------
- kubectl (v1.16 or higher), compatible with your cluster (+/- 1 minor release from your cluster).
- Helm (v3.0.2 or higher).
- Kubernetes cluster (v1.24 or higher).

Ensure you have allocated enough resources to Kubernetes: at least 16Gb.

---
Development Guides
---
- Flink on K8s installation [guide](./docs/flink-k8s-operator-guide.md)