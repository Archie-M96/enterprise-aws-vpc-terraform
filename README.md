# Enterprise AWS VPC Infrastructure

**Infrastructure-as-Code (IaC) deployment of a secure, highly available AWS Virtual Private Cloud using Terraform.**

This repository contains the foundational network architecture originally developed for a secure serverless financial document vault. It demonstrates the ability to programmatically deploy and manage cloud networking with a strict focus on zero-trust routing and data isolation.

## Architecture & Security Features

* **Zero-Trust Network Topology:** Provisioned isolated private subnets for sensitive backend services (Lambda, databases) to ensure zero public internet exposure.
* **Cost-Optimized Private Routing:** Engineered an S3 Gateway VPC Endpoint to route traffic directly to AWS services on the internal backbone, bypassing NAT Gateway data processing costs.
* **Automated Provisioning:** 100% of the network infrastructure (VPCs, Subnets, Route Tables, Endpoints) is defined in Terraform (HCL) for repeatable, drift-resistant deployment.
* **Least-Privilege Design:** Structured to seamlessly integrate with strict IAM role boundaries and customer-managed KMS encryption standards.

## Tech Stack
* **Cloud Provider:** Amazon Web Services (AWS)
* **IaC Tool:** Terraform
* **Core Services:** VPC, Subnets, Route Tables, Gateway Endpoints
