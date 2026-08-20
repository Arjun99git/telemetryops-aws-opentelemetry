# TelemetryOps — AWS OpenTelemetry Observability Platform

A hands-on AWS observability portfolio project demonstrating how a distributed microservices application can be deployed on Amazon EC2 and observed with the OpenTelemetry ecosystem.

> **Portfolio scope:** This repository documents my AWS deployment, Linux/Docker setup, troubleshooting, application validation, and observability learning. The demo application itself is based on the upstream OpenTelemetry Astronomy Shop project and is not claimed as original application code.

## What I Built

I deployed the OpenTelemetry Astronomy Shop demo on an Ubuntu EC2 instance using Docker Compose and validated the application through its storefront and shopping-cart workflow.

The environment included:

- AWS EC2
- Ubuntu Linux
- Docker and Docker Compose
- OpenTelemetry Collector
- Prometheus
- Grafana
- Jaeger
- Terraform CLI
- kubectl
- Git and GitHub

## Architecture

```text
User
  |
  v
OpenTelemetry Astronomy Shop
  |
  v
Distributed Microservices
  |
  v
OpenTelemetry SDKs / Instrumentation
  |
  v
OpenTelemetry Collector
  |
  +-------------------+-------------------+
  |                   |                   |
  v                   v                   v
Metrics              Traces              Telemetry
  |                   |
  v                   v
Prometheus           Jaeger
  |
  v
Grafana
```

## AWS Implementation

The lab was deployed on an Ubuntu EC2 instance.

Key implementation steps:

- Connected to EC2 with SSH key-based authentication.
- Installed Docker and Docker Compose.
- Installed and verified `kubectl`.
- Installed Terraform from the HashiCorp repository.
- Started the OpenTelemetry demo stack with Docker Compose.
- Validated the storefront and shopping-cart workflow from the EC2-hosted application.
- Diagnosed a Docker image pull failure caused by insufficient disk capacity.
- Increased the EC2 EBS volume to 30 GiB and inspected the partition layout with `lsblk`.
- Verified Linux and Docker storage status before continuing deployment.
- Terminated the temporary EC2 environment after testing to avoid unnecessary cloud cost and exposure.

## Troubleshooting Example

During deployment, Docker failed with:

```text
no space left on device
```

I diagnosed the issue using:

```bash
df -h
lsblk
docker system df
```

The issue was caused by the large number of Docker image layers required by the microservices and observability stack. The backing EBS volume was expanded to provide additional storage capacity.

Other troubleshooting included:

- Correcting the EC2 SSH username from `ec2-user` to `ubuntu` for the Ubuntu AMI.
- Verifying the `kubectl` binary checksum before installation.
- Distinguishing Windows PowerShell commands from Linux commands inside the SSH session.
- Recognizing that `kubectl` reporting `localhost:8080 refused` indicated no Kubernetes cluster context was configured yet, rather than a failed kubectl installation.

## Application Validation

The application was successfully reachable from AWS and this user flow was validated:

```text
Storefront -> Product -> Shopping Cart
```

This demonstrated that the containerized application stack was running and that multiple services were participating in the request flow.

## Observability Components

### OpenTelemetry Collector
Receives, processes, and exports telemetry produced by instrumented services.

### Prometheus
Provides metrics collection and querying.

### Grafana
Provides dashboards and visualization for operational and application metrics.

### Jaeger
Provides distributed tracing for investigating request paths across microservices.

## Security Practices

This was a temporary portfolio lab, not a production environment.

- SSH key-based EC2 access.
- No private keys committed to GitHub.
- No AWS passwords, access keys, or credential files stored in this repository.
- No claim that the temporary HTTP endpoint was production-secure.
- Temporary infrastructure was terminated after validation.
- Future AWS integrations should use IAM roles and least-privilege permissions instead of long-lived credentials.

## Useful Validation Commands

```bash
# Docker
docker --version
docker compose version
docker compose ps
docker ps

# Terraform
terraform version

# Kubernetes client
kubectl version --client

# Linux storage
df -h
lsblk

# Docker storage
docker system df
```

## Next Enhancements

- Add screenshots of the validated AWS-hosted storefront.
- Capture Jaeger distributed traces.
- Add Grafana dashboards for request rate, errors, and latency.
- Document RED metrics: Rate, Errors, and Duration.
- Add OpenTelemetry log collection.
- Add alerting rules.
- Provision AWS infrastructure with Terraform.
- Deploy the application to Amazon EKS.
- Implement GitOps with Argo CD and Helm.
- Add HTTPS/TLS through a load balancer or ingress.
- Use least-privilege IAM roles for AWS integrations.
- Add CloudWatch integration.
- Correlate deployments with traces and metrics.

## Attribution

This portfolio project uses the OpenTelemetry Demo ecosystem as its application foundation.

- OpenTelemetry Demo: https://github.com/open-telemetry/opentelemetry-demo
- OpenTelemetry Demo documentation: https://opentelemetry.io/docs/demo/
- Deployment-learning reference used during the lab: https://github.com/iam-veeramalla/ultimate-devops-project-demo

The upstream source remains subject to its applicable open-source license terms.
