
🌐
Project Observability Setup Guide
Amazon Web Services (AWS) & Google Cloud Platform (GCP)
Logs  •  Metrics  •  Traces
A centralized reference for enabling full-stack observability across Kubernetes, virtual machines, serverless, and microservice architectures.
 Table of Contents
Purpose	3
Observability Architecture	3
AWS Architecture	3
GCP Architecture	3
AWS Observability	4
Logs	4
Metrics	5
Traces	6
GCP Observability	7
Logs	7
Metrics	8
Traces	8
Advantages & Disadvantages	10
AWS vs GCP Comparison	11
Project Monitoring Checklist	12
Recommended Monitoring Strategy	13
Repository Structure	14
 Purpose
Observability helps teams understand the health and behavior of applications and infrastructure in production. Rather than reacting blindly to incidents, teams with strong observability can quickly detect, localize, and resolve issues before they impact users.
This guide explains how to configure observability using the three foundational pillars:
Pillar	Description
📜  Logs	What happened? — discrete, timestamped event records
📈  Metrics	How is the system performing? — numeric time-series data
🔍  Traces	Where did the request go, and where is the delay? — end-to-end request paths

Observability Architecture
AWS Architecture
Every AWS-native workload — containers, VMs, or serverless functions — feeds each pillar into its dedicated managed service:
 
Figure 1 — AWS Observability Architecture Overview
GCP Architecture
On GCP, the same three signal types route to Google Cloud's operations suite:
 
Figure 2 — GCP Observability Architecture Overview
 AWS Observability
3.1  Logs
Service: Amazon CloudWatch Logs
Supported resources:
•	Amazon EKS
•	Amazon ECS
•	EC2
•	Lambda
Configuration — Amazon EKS
Install AWS Fluent Bit as a DaemonSet to ship container logs from every node.
 
Figure 3 — AWS EKS Log Pipeline
Configuration — EC2
Install the CloudWatch Agent and configure log collection from:
/var/log/messages
/var/log/syslog
/var/log/nginx/access.log
/var/log/nginx/error.log
Configuration — Lambda
No configuration required — Lambda automatically writes logs to CloudWatch.
Verification
Navigate to: AWS Console → CloudWatch → Log Groups
Confirm the following are present:
•	Application logs
•	Error logs
•	Startup logs
•	Request logs
3.2  Metrics
Service: Amazon CloudWatch Metrics
AWS automatically collects metrics for:
•	EC2
•	Lambda
•	RDS
•	ELB
•	S3
•	DynamoDB
Amazon EKS
Enable Container Insights, the CloudWatch Agent, and (optionally) Amazon Managed Service for Prometheus.
 
Figure 4 — AWS EKS Metrics Pipeline
Infrastructure Metrics	Application Metrics
CPU Utilization	Request Count
Memory Usage	Response Time
Disk Usage	Error Rate
Network Traffic	Pod CPU / Memory / Restarts

Verification
Navigate to: CloudWatch → Metrics
Confirm CPU, Memory, Latency, and Request Count are reporting.
3.3  Traces
Service: AWS X-Ray
Purpose: track a single request as it flows through multiple services.
 
Figure 5 — Example Request Trace (AWS)
Configuration
Instrument applications using:
•	OpenTelemetry SDK
•	AWS X-Ray SDK
•	OpenTelemetry Java Agent
 
Figure 6 — AWS Distributed Tracing Pipeline
Verification
Navigate to: CloudWatch → Application Signals, or the AWS X-Ray Console.
•	Trace ID
•	Request Duration
•	Service Map
•	Error Details
•	Slow Requests
 GCP Observability
4.1  Logs
Service: Cloud Logging
Supported resources:
•	GKE
•	Compute Engine
•	Cloud Run
•	App Engine
Configuration
Use the built-in logging integration, or the Google Cloud Ops Agent where applicable.
 
Figure 7 — GCP Logging Pipeline
Verification
Navigate to: Google Cloud Console → Observability → Logs Explorer
•	Application logs
•	HTTP request logs
•	Error logs
•	Startup logs
4.2  Metrics
Service: Cloud Monitoring
Metrics are automatically collected for Google Cloud resources. For Kubernetes, enable Cloud Monitoring and (optionally) Managed Service for Prometheus.
Infrastructure	Application
CPU	Request Count
Memory	Error Rate
Disk	Latency
Network	Pod CPU / Memory / Restart Count

Verification
Navigate to: Observability → Monitoring
4.3  Traces
Service: Cloud Trace
Important: Cloud Trace does NOT automatically collect application traces — instrumentation is required.
Applications must be instrumented using:
•	OpenTelemetry SDK
•	OpenTelemetry Java Agent
•	OpenTelemetry Collector (optional)
Kubernetes (GKE)
 
Figure 8 — GCP Tracing Pipeline (GKE)
Verification
Navigate to: Observability → Trace
•	Trace IDs
•	Span IDs
•	Service Names
•	Latency
•	Errors
Example request path:
 
Figure 9 — Example Request Trace (GCP)
 Advantages & Disadvantages
Logs
✅ Advantages	⚠️ Disadvantages
Detailed event history	High storage costs
Error investigation	Difficult to analyze at scale
Root cause analysis	Requires retention management
Security auditing	

Metrics
✅ Advantages	⚠️ Disadvantages
Real-time monitoring	Cannot explain why a problem occurred
Alerts	Custom metrics may incur additional cost
Dashboards	
Capacity planning	

Traces
✅ Advantages	⚠️ Disadvantages
End-to-end request visibility	Requires application instrumentation
Root cause analysis	Additional configuration effort
Latency troubleshooting	Sampling may not capture every request
Service dependency mapping	

AWS vs GCP Comparison
Feature	AWS	GCP
Logs	CloudWatch Logs	Cloud Logging
Metrics	CloudWatch Metrics	Cloud Monitoring
Traces	AWS X-Ray	Cloud Trace
Dashboards	CloudWatch Dashboard	Cloud Monitoring Dashboard
Alerts	CloudWatch Alarms	Alerting Policies
Service Map	AWS X-Ray	Cloud Trace
Auto Infrastructure Metrics	✅	✅
Auto Infrastructure Logs	✅	✅
Automatic Application Tracing	❌	❌
OpenTelemetry Support	✅	✅

Project Monitoring Checklist
AWS
☐  CloudWatch Logs Enabled
☐  CloudWatch Metrics Enabled
☐  Container Insights Enabled
☐  CloudWatch Agent Installed
☐  AWS X-Ray Enabled
☐  OpenTelemetry Configured
☐  CloudWatch Alarms Created
☐  Dashboards Created
GCP
☐  Cloud Logging Enabled
☐  Cloud Monitoring Enabled
☐  Cloud Trace Enabled
☐  OpenTelemetry Configured
☐  Alert Policies Created
☐  Dashboards Created
 Recommended Monitoring Strategy
 
Figure 10 — Recommended Monitoring Workflow
1.	Monitor Metrics to detect performance degradation.
2.	Use Traces to identify the affected service.
3.	Review Logs to determine the exact error.
4.	Configure dashboards and alerts for proactive monitoring.
5.	Continuously optimize observability to improve reliability.
 Repository Structure
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

Contributing
Contributions are welcome! Feel free to open issues, submit pull requests, or suggest improvements to enhance this documentation.
License
This project is intended for educational and operational documentation purposes. Update the license section according to your organization's policy.
