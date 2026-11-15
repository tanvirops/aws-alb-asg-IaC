# AWS Auto Scaling & CI/CD for a 3-Tier Application

> **Project evidence and implementation walkthrough**
>
> This document presents the AWS infrastructure, application deployment, CI/CD workflow,
> auto-healing behavior, scaling tests, Terraform outputs, and teardown evidence.

---

## 1. Architecture Overview

The architecture diagram below summarizes the 3-tier deployment, network layout,
traffic flow, SSH access topology, CI/CD workflow, and security model.
<img width="1536" height="1024" alt="image4" src="https://github.com/user-attachments/assets/fefd2ad9-94ba-4649-8b2f-99c0fd90c1e8" />


## 2. Part One — Auto Scaling for a 3-Tier Application

### 2.1 Application Load Balancer — Resource Map

The ALB resource map shows the listener, forwarding rule, target group, and registered
backend instances.
