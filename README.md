# Enterprise AWS VPC Infrastructure

## Why I Built This
This is the foundational network layer I originally built for my fintech vault project, pulled 
out into its own repo to prove Infrastructure-as-Code and zero-trust networking as a standalone 
skill, not just a buried detail in a bigger build.

## What I Built
- **Zero-Trust Topology:** Isolated private subnets for sensitive backend services (Lambda, 
  databases), with zero direct route to the internet.
- **Cost-Optimized Private Routing:** An S3 Gateway VPC Endpoint routes internal traffic to AWS 
  services on the private backbone, bypassing NAT Gateway data-processing costs entirely.
- **Automated Provisioning:** 100% of the network (VPC, subnets, route tables, endpoints) is 
  defined in Terraform (HCL) for repeatable, drift-resistant deployment.
- **Least-Privilege Ready:** Structured to integrate directly with strict IAM role boundaries 
  and customer-managed KMS encryption.

## Why I Made These Calls
I chose the Gateway Endpoint over a NAT Gateway because the traffic pattern here only ever needs 
to reach S3. NAT Gateways bill per hour plus per-GB processed, which is real cost for traffic 
that never needed general internet access. This only works because the pattern is narrow. A 
workload needing broader outbound internet access would still need a NAT Gateway.

I also enforced isolation at the routing level, not just via security groups. The private 
subnet simply has no route to an Internet Gateway, so even a misconfigured security group can't 
expose it.

## Tech Stack
AWS (VPC, Subnets, Route Tables, Gateway Endpoints), Terraform

## What I'd Do Next
This is single-AZ right now. At real scale I'd replicate across multiple Availability Zones for 
availability, split Terraform state per environment instead of one shared state file, and enable 
VPC Flow Logs to CloudWatch so network traffic is actually auditable rather than assumed correct.
