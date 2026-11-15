# AWS Auto Scaling & CI/CD for a 3-Tier Application

> **Project evidence and implementation walkthrough**
>
> This document presents the AWS infrastructure, application deployment, CI/CD workflow,
> auto-healing behavior, scaling tests, Terraform outputs, and teardown evidence.

---

## 1. Architecture Overview

The architecture diagram below summarizes the 3-tier deployment, network layout,
traffic flow, SSH access topology, CI/CD workflow, and security model.

![Evidence screenshot 1](images/image4.png)

## 2. Part One — Auto Scaling for a 3-Tier Application

### 2.1 Application Load Balancer — Resource Map

The ALB resource map shows the listener, forwarding rule, target group, and registered
backend instances.

![Evidence screenshot 2](images/image15.png)

### 2.2 Network Mapping

The network mapping shows the Availability Zones and public subnets associated with
the load balancer.

![Evidence screenshot 3](images/image9.png)

### 2.3 Target Group Health

The backend target group has **two registered EC2 instances**, and both are reporting
a **Healthy** status in this evidence.

![Evidence screenshot 4](images/image19.png)

### 2.4 Auto Scaling Group

The Auto Scaling Group configuration shows the backend instance capacity and its
associated launch template.

![Evidence screenshot 5](images/image17.png)

### 2.5 Launch Template

The launch template defines the configuration used when the Auto Scaling Group
launches backend EC2 instances.

![Evidence screenshot 6](images/image20.png)

### 2.6 Security Groups

The following screenshot shows the security groups configured for the infrastructure.

![Evidence screenshot 7](images/image18.png)

### 2.7 EC2 Instance Layout

The EC2 environment uses the following instance roles:

- **Bastion host:** Used as the SSH entry point for accessing private instances.
- **Database EC2 instance:** Hosts the database for centralized data storage.
- **Backend EC2 instances:** Created and managed by the Auto Scaling Group.

**Implementation note:** Amazon RDS and AWS CodeDeploy were not configured because
the IAM user used for this work did not have the required permissions. Therefore,
the database and deployment flow shown in the architecture use the implemented
EC2-based setup.

![Evidence screenshot 8](images/image7.png)

### 2.8 Application Output

The application dashboard below shows the deployed application running in the
AWS environment.

![Evidence screenshot 9](images/image6.png)

---

## 3. Part Two — CI/CD with GitHub Actions

### 3.1 GitHub Actions Secrets

The screenshot shows the repository secrets configured for the workflow.

![Evidence screenshot 10](images/image22.png)

### 3.2 CI/CD Workflow Execution

The following screenshots provide evidence of the CI/CD workflow and repository
deployment process.

**Workflow run and deployment output**

![Evidence screenshot 11](images/image1.png)

**Repository and project structure**

![Evidence screenshot 12](images/image16.png)

**Deployment job details**

![Evidence screenshot 13](images/image13.png)

### 3.3 Successful CI/CD — Application Verification

The application screenshot shows the result after a successful CI/CD deployment.

- **Before deployment:** The button label was `Save Measurement`.
- **After successful deployment:** The button label is `Add Measurement`.

This visible UI change is used as application-level evidence that the updated
version was deployed.

![Evidence screenshot 14](images/image21.png)

---

## 4. Self-Healing Test Evidence

### 4.1 Terminating a Backend EC2 Instance

The first screenshot shows the backend EC2 instance selected for termination
during the self-healing test.

![Evidence screenshot 15](images/image12.png)

### 4.2 Automatic Instance Replacement

After the instance was terminated, the Auto Scaling Group initiated a replacement
backend EC2 instance. The screenshot captures the new instance during initialization.

![Evidence screenshot 16](images/image14.png)

---

## 5. Scale-Out / Scale-In Test Evidence

### 5.1 Generating CPU Load

CPU stress was generated using `stress-ng` to test the Auto Scaling behavior.

The terminal evidence shows the CPU stress command running with a 360-second
duration and CPU load configured for the available processors.

![Evidence screenshot 17](images/image11.png)

![Evidence screenshot 18](images/image3.png)

### 5.2 Scale-Out Result

During the CPU load test, the Auto Scaling Group initiated an additional backend
EC2 instance. The screenshot below shows the resulting EC2 instance list.

![Evidence screenshot 19](images/image8.png)

---

## 6. Terraform Infrastructure as Code (IaC) Output

The Terraform output below records the deployed infrastructure values, including
the ALB DNS name, Auto Scaling Group name, Bastion public IP, database private IP,
subnet IDs, and VPC ID.

![Evidence screenshot 20](images/image10.png)

---

## 7. Project Repositories

- **3-Tier Application Project:** [three-tier-aws-deployment](https://github.com/Tanvir-Alam625/three-tier-aws-deployment)
- **Terraform IaC and Documentation:** [aws-alb-asg-IaC](https://github.com/Tanvir-Alam625/aws-alb-asg-IaC)

---

## 8. Infrastructure Teardown Evidence

### 8.1 Terraform Destroy Execution

The terminal screenshot shows the Terraform destroy operation running against
the deployed infrastructure.

![Evidence screenshot 21](images/image2.png)

### 8.2 Successful Teardown

Terraform reported that the destroy operation completed successfully, with
**39 resources destroyed**.

![Evidence screenshot 22](images/image5.png)

---

## 9. Evidence Summary

This document contains screenshots covering:

- The 3-tier AWS architecture and network design.
- ALB, target group, Auto Scaling Group, launch template, and security groups.
- EC2 instance roles and the deployed application.
- GitHub Actions secrets and CI/CD execution.
- Application verification after deployment.
- Auto Scaling self-healing after instance termination.
- CPU stress testing and scale-out behavior.
- Terraform outputs and successful infrastructure teardown.