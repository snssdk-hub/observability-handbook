# 🌐 Project Observability Setup Guide (AWS & GCP)

This repository contains documentation, implementation guides, and best practices for configuring **Observability** across **Amazon Web Services (AWS)** and **Google Cloud Platform (GCP)**.

The goal is to provide a centralized guide for enabling **Logs**, **Metrics**, and **Traces** for cloud-native applications running on Kubernetes, virtual machines, serverless services, and microservices.

---

# 📖 Table of Contents

- [Purpose](#purpose)
- [Observability Architecture](#observability-architecture)
  - [AWS Architecture](#aws-architecture)
  - [GCP Architecture](#gcp-architecture)
- [AWS Observability](#aws-observability)
  - [Logs](#31-logs)
  - [Metrics](#32-metrics)
  - [Traces](#33-traces)
- [GCP Observability](#gcp-observability)
  - [Logs](#41-logs)
  - [Metrics](#42-metrics)
  - [Traces](#43-traces)
- [Advantages & Disadvantages](#advantages--disadvantages)
- [AWS vs GCP Comparison](#aws-vs-gcp-comparison)
- [Project Monitoring Checklist](#project-monitoring-checklist)
- [Recommended Monitoring Strategy](#recommended-monitoring-strategy)

---

# Purpose

Observability helps teams understand the health and behavior of applications and infrastructure.

This guide explains how to configure observability using the **three pillars**:

| Pillar | Description |
|----------|-------------|
| 📜 Logs | What happened? |
| 📈 Metrics | How is the system performing? |
| 🔍 Traces | Where did the request go and where is the delay? |

---

# Observability Architecture

## AWS Architecture

```text
Application / Microservices
        │
        ├──────── Logs ─────────► CloudWatch Logs
        │
        ├──────── Metrics ──────► CloudWatch Metrics
        │
        └──────── Traces ───────► AWS X-Ray
```

---

## GCP Architecture

```text
Application / Microservices
        │
        ├──────── Logs ─────────► Cloud Logging
        │
        ├──────── Metrics ──────► Cloud Monitoring
        │
        └──────── Traces ───────► Cloud Trace
```

---

# AWS Observability

## 3.1 Logs

### Service

- Amazon CloudWatch Logs

### Supported Resources

- Amazon EKS
- Amazon ECS
- EC2
- Lambda

### Configuration

### Amazon EKS

Install **AWS Fluent Bit**.

```text
Application Pods
        │
        ▼
AWS Fluent Bit
        │
        ▼
CloudWatch Logs
```

### EC2

Install the **CloudWatch Agent**.

Configure log collection from:

```
/var/log/messages
/var/log/syslog
/var/log/nginx/access.log
/var/log/nginx/error.log
```

### Lambda

No configuration required.

Lambda automatically writes logs to CloudWatch.

### Verification

Navigate to:

```
AWS Console
    └── CloudWatch
          └── Log Groups
```

Verify:

- Application logs
- Error logs
- Startup logs
- Request logs

---

# 3.2 Metrics

### Service

Amazon CloudWatch Metrics

### Automatic Metrics

AWS automatically collects metrics for:

- EC2
- Lambda
- RDS
- ELB
- S3
- DynamoDB

### Amazon EKS

Enable:

- Container Insights
- CloudWatch Agent
- Amazon Managed Service for Prometheus (Optional)

```text
Pods
   │
Container Insights
   │
CloudWatch Metrics
```

### Important Metrics

Infrastructure

- CPU Utilization
- Memory Usage
- Disk Usage
- Network Traffic

Application

- Request Count
- Response Time
- Error Rate
- Pod CPU
- Pod Memory
- Pod Restarts

### Verification

Navigate to:

```
CloudWatch
    └── Metrics
```

Verify:

- CPU
- Memory
- Latency
- Request Count

---

# 3.3 Traces

### Service

AWS X-Ray

### Purpose

Track a request through multiple services.

```text
Client
   │
API Gateway
   │
Order Service
   │
Payment Service
   │
Database
```

### Configuration

Instrument applications using:

- OpenTelemetry SDK
- AWS X-Ray SDK
- OpenTelemetry Java Agent

```text
Application
      │
OpenTelemetry
      │
AWS X-Ray
```

### Verification

Navigate to:

```
CloudWatch
    └── Application Signals
```

or

```
AWS X-Ray Console
```

Verify:

- Trace ID
- Request Duration
- Service Map
- Error Details
- Slow Requests

---

# GCP Observability

## 4.1 Logs

### Service

Cloud Logging

### Supported Resources

- GKE
- Compute Engine
- Cloud Run
- App Engine

### Configuration

Use the built-in logging integration or Google Cloud Ops Agent where applicable.

```text
Pods
   │
Cloud Logging
```

### Verification

Navigate to:

```
Google Cloud Console

Observability
     └── Logs Explorer
```

Verify:

- Application logs
- HTTP request logs
- Error logs
- Startup logs

---

# 4.2 Metrics

### Service

Cloud Monitoring

### Configuration

Metrics are automatically collected for Google Cloud resources.

For Kubernetes:

Enable:

- Cloud Monitoring
- Managed Service for Prometheus (Optional)

### Monitor

Infrastructure

- CPU
- Memory
- Disk
- Network

Application

- Request Count
- Error Rate
- Latency
- Pod CPU
- Pod Memory
- Restart Count

### Verification

Navigate to:

```
Observability
     └── Monitoring
```

---

# 4.3 Traces

### Service

Cloud Trace

### Important

Cloud Trace **does not automatically collect application traces.**

Applications must be instrumented using:

- OpenTelemetry SDK
- OpenTelemetry Java Agent
- OpenTelemetry Collector (Optional)

```text
Application
      │
OpenTelemetry
      │
Cloud Trace
```

### Kubernetes (GKE)

```text
Application Pod
       │
OpenTelemetry SDK
       │
OpenTelemetry Collector
       │
Cloud Trace
```

### Verification

Navigate to:

```
Observability
      └── Trace
```

Verify:

- Trace IDs
- Span IDs
- Service Names
- Latency
- Errors

Example

```text
Browser
    │
Ingress
    │
User Service
    │
Payment Service
    │
Cloud SQL
```

---

# Advantages & Disadvantages

## Logs

### Advantages

- Detailed event history
- Error investigation
- Root cause analysis
- Security auditing

### Disadvantages

- High storage costs
- Difficult to analyze at scale
- Requires retention management

---

## Metrics

### Advantages

- Real-time monitoring
- Alerts
- Dashboards
- Capacity planning

### Disadvantages

- Cannot explain why a problem occurred
- Custom metrics may incur additional cost

---

## Traces

### Advantages

- End-to-end request visibility
- Root cause analysis
- Latency troubleshooting
- Service dependency mapping

### Disadvantages

- Requires application instrumentation
- Additional configuration effort
- Sampling may not capture every request

---

# AWS vs GCP Comparison

| Feature | AWS | GCP |
|----------|-----|-----|
| Logs | CloudWatch Logs | Cloud Logging |
| Metrics | CloudWatch Metrics | Cloud Monitoring |
| Traces | AWS X-Ray | Cloud Trace |
| Dashboards | CloudWatch Dashboard | Cloud Monitoring Dashboard |
| Alerts | CloudWatch Alarms | Alerting Policies |
| Service Map | AWS X-Ray | Cloud Trace |
| Auto Infrastructure Metrics | ✅ | ✅ |
| Auto Infrastructure Logs | ✅ | ✅ |
| Automatic Application Tracing | ❌ | ❌ |
| OpenTelemetry Support | ✅ | ✅ |

---

# Project Monitoring Checklist

## AWS

- [ ] CloudWatch Logs Enabled
- [ ] CloudWatch Metrics Enabled
- [ ] Container Insights Enabled
- [ ] CloudWatch Agent Installed
- [ ] AWS X-Ray Enabled
- [ ] OpenTelemetry Configured
- [ ] CloudWatch Alarms Created
- [ ] Dashboards Created

---

## GCP

- [ ] Cloud Logging Enabled
- [ ] Cloud Monitoring Enabled
- [ ] Cloud Trace Enabled
- [ ] OpenTelemetry Configured
- [ ] Alert Policies Created
- [ ] Dashboards Created

---

# Recommended Monitoring Strategy

1. Monitor **Metrics** to detect performance degradation.
2. Use **Traces** to identify the affected service.
3. Review **Logs** to determine the exact error.
4. Configure dashboards and alerts for proactive monitoring.
5. Continuously optimize observability to improve reliability.

---

# Repository Structure

```
cloud-observability-docs/
│
├── README.md
├── aws/
│   ├── logs.md
│   ├── metrics.md
│   ├── traces.md
│   ├── dashboards.md
│   └── alerts.md
│
├── gcp/
│   ├── logging.md
│   ├── monitoring.md
│   ├── tracing.md
│   ├── dashboards.md
│   └── alerts.md
│
├── kubernetes/
│   ├── eks.md
│   ├── gke.md
│   └── opentelemetry.md
│
├── architecture/
├── diagrams/
└── troubleshooting/
```

---

## Contributing

Contributions are welcome! Feel free to open issues, submit pull requests, or suggest improvements to enhance this documentation.

---

## License

This project is intended for educational and operational documentation purposes. Update the license section according to your organization's policy.
