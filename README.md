# AWS VPC Peering Project

A hands-on AWS networking lab demonstrating private connectivity between two Amazon Virtual Private Clouds (VPCs) using VPC peering, route tables, security groups, and a bastion host.

## Project details

- **Student:** Soham Samanta
- **Batch:** AWS INTERNSHIP BATCH 1.1
- **AWS Region:** Asia Pacific (Mumbai), `ap-south-1`
- **Project:** VPC Peering and Private Network Connectivity

## Architecture

See [`architecture.png`](architecture.png) for the logical network diagram. The diagram represents the configuration during testing; the lab resources were subsequently removed during cleanup.

### Network plan

| Resource | Name | CIDR / Private IP | Purpose |
|---|---|---|---|
| VPC A | `ewb-vpc-a` | `10.10.0.0/16` | Bastion and App-A network |
| Public subnet | `ewb-a-public` | `10.10.1.0/24` | Bastion host |
| Private subnet | `ewb-a-private` | `10.10.2.0/24` | App-A |
| VPC B | `ewb-vpc-b` | `10.20.0.0/16` | App-B network |
| Private subnet | `ewb-b-private` | `10.20.1.0/24` | App-B |
| Bastion | `ewb-bastion` | `10.10.1.10` | SSH entry point; public IPv4 was enabled during the lab |
| App-A | `ewb-app-a` | `10.10.2.10` | Private workload; no public IPv4 |
| App-B | `ewb-app-b` | `10.20.1.10` | Private workload; no public IPv4 |

## Routing and security

During testing, the VPC peering connection was active and route tables included routes to the remote VPC CIDR through the peering connection:

- `ewb-rt-a-public`: local route, default route to the Internet Gateway, and route to `10.20.0.0/16` through peering.
- `ewb-rt-a-private`: local route and route to `10.20.0.0/16` through peering.
- `ewb-rt-b-private`: local route and route to `10.10.0.0/16` through peering.

Security groups limited SSH access to the bastion from the operator's public IPv4 `/32`, SSH to private hosts from the bastion, and ICMP traffic between the application instances for connectivity tests.

## Validation results

The submitted terminal screenshots document:

- App-A to App-B ping: 4 packets received, 0% packet loss.
- App-B to App-A ping: 4 packets received, 0% packet loss.
- SSH access to App-B through the bastion host.
- A controlled ICMP failure after temporarily removing the relevant inbound rule, followed by successful connectivity after restoring the rule.

**Evidence note:** The initial baseline ping screenshot from before the peering routes were configured was not captured. The report identifies this transparently and does not present a fabricated screenshot.

## Cleanup

After testing, the lab resources were cleaned up. The supplied evidence documents termination of the three EC2 instances, cleared EBS volumes and snapshots, removal of the peering connection and routes, security groups, subnets, custom route tables, Internet Gateway, and both lab VPCs.

## Repository contents

- `README.md` — project overview, network plan, test summary, and cleanup notes.
- `architecture.png` — separate logical architecture diagram.
- `Soham_Samanta_EWB_VPC_Peering.pdf` — project report with configuration, test, and cleanup evidence.

## Security

This repository intentionally excludes private keys, passwords, AWS access keys, session tokens, and other credentials. Do not commit `.pem` files or secrets.

## Disclaimer

This is a student lab project. Resource names, CIDRs, and results are documented from the lab evidence supplied for the report. The architecture image describes the test setup, not currently running AWS resources.
