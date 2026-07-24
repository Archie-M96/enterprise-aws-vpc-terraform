# Secure AWS VPC Foundation (Terraform)

## Why I Built This
This repository serves as the master network architecture for my broader cloud security 
portfolio (including a serverless fintech vault and compliance monitor). I needed to prove I 
could define a strict, zero-trust network boundary using Infrastructure as Code. Rather than 
relying on default VPCs, this project demonstrates how to construct an isolated, observable 
environment designed to keep sensitive data entirely off the public internet.

## What I Built
- **Network Isolation:** Provisioned a custom VPC utilizing strict private subnets, with no 
  direct route to an Internet Gateway.
- **Secure Routing:** Configured S3 Gateway VPC Endpoints so internal resources (like Lambda) 
  reach AWS S3 over the internal AWS backbone, bypassing the public internet entirely.
- **Baseline Observability:** Enabled VPC Flow Logs, routed to a CloudWatch log group with a 
  strict 7-day retention policy, to capture network traffic at the foundation level. Deeper 
  anomaly detection and SNS-based alerting on this traffic is built out as its own dedicated 
  project (see `centralized-network-observability-dashboard`), rather than duplicated here.
- **IAM:** Configured a dedicated IAM role with a trust policy scoped specifically to allow the 
  VPC Flow Logs service to write to the CloudWatch log group, nothing broader.
- **Deployment:** Authored in Terraform and deployed manually via the AWS CLI with local state 
  management (`terraform.tfstate`).

## Why I Made These Calls
- **VPC Endpoints over NAT Gateways:** NAT Gateways charge hourly and per-GB, and don't add 
  privacy benefit here since the traffic only ever needs to reach S3. A Gateway Endpoint gives 
  the same "off the public internet" posture at zero data-processing cost.
- **Cost & Lifecycle Management:** The 7-day log retention window prevents runaway storage cost. 
  The whole environment was Free Tier-scoped and destroyed immediately after testing to maintain 
  a $0.00 footprint.
- **Local State:** For this foundational sprint, I kept Terraform state local to focus entirely 
  on getting the network routing, IAM scoping, and Flow Logs pipeline correct before adding the 
  operational complexity of remote state.

## What I'd Do Next
The network architecture is secure, but the deployment methodology needs to mature for a 
multi-developer environment. Next:
1. **Remote State Management:** Migrate `terraform.tfstate` to an S3 backend with DynamoDB state 
   locking, to prevent concurrent-modification corruption.
2. **Infrastructure CI/CD:** Wire this repo to a GitHub Actions pipeline instead of local 
   `terraform apply` runs.
3. **Security Scanning:** Add `tfsec` or Checkov to that pipeline to validate security group and 
   network configs automatically before every deployment.
