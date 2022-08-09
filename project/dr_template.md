# Infrastructure

## AWS Zones
Identify your zones here
- Primary zone: us-east-2
  - AZS: "us-east-2a", "us-east-2b", "us-east-2c"
  
- Secondary zone: us-west-1
  - AZS: "us-west-1a", "us-west-1b"

## Servers and Clusters

### Table 1.1 Summary
| Asset name               | Brief description                                                                                             | AWS size eg. t3.micro (if applicable, not all assets will have a size) | Number of nodes/replicas or just how many of a particular asset | Identify if this asset is deployed to DR, replicated, created in multiple locations or just stored elsewhere  |
|--------------------------|---------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| EC2 instance             | use for VM Ubuntu-Web server                                                                                  | t3.micro                                                               | 3 servers per region                                            | DR                                                                                                            |
| EC2 instance             | use for EKS worker node                                                                                       | t3.medium                                                              | 2 nodes per region                                              | DR                                                                                                            |
| Key pair                 | use for accessing EC2 instance                                                                                |                                                                        | 1 key pair per region                                           | DR                                                                                                            |
| Load Balancer            | automatically distributes the incoming traffic across multiple EC2 instances (Ubuntu-Web servers) and Grafana | alb/nlb                                                                | 2 per region                                                    | DR                                                                                                            |
| Public IP                |                                                                                                               |                                                                        | 1                                                               | created in multiple locations                                                                                 |
| RDS Database cluster     | each cluster has 2 instance nodes, 1 is writer instance (primary) and 1 is read instance (secondary)          | Aurora MySQL/db.t2.small                                               | 2 instance nodes for each cluster                               | DR and 1 region is primary and 2 region is replica cluster                                                    |
| Security Groups          | configure traffic for EC2 instances, EKS cluster and RDS cluster                                              |                                                                        | 3 SGs per region                                                | DR                                                                                                            |
| VPC                      | Network                                                                                                       |                                                                        | 1 per region                                                    | DR                                                                                                            |
| Subnet                   | Subnet                                                                                                        |                                                                        | 3 per VPC                                                       | DR                                                                                                            |
| IAM roles                | control access eks clusters and eks workers                                                                   |                                                                        | 2 per region                                                    | DR                                                                                                            |
| AMI                      | Image for EC2 Ubuntu-Web instance                                                                             |                                                                        | 1 per region                                                    | created in multiple locations                                                                                 |
| Prometheus/Grafana stack | Monitoring stack                                                                                              |                                                                        |                                                                 |                                                                                                               |


### Descriptions
More detailed descriptions of each asset identified above.
- EC2 instance use for VM Ubuntu-Web servers and EKS worker nodes.
- Keypair use for accessing EC2 instances.
- Load Balancer use for distributing the incoming traffic across multiple EC2 instances (Ubuntu-Web servers) and Grafana automatically.
- Security Groups use for configuring traffic for EC2 instances, EKS cluster and RDS cluster.
- RDS is Database cluster.
- VPC/Subnet/Public IP are network elements.
- IAM roles use for control access eks clusters and eks workers.
- AMI is image for EC2 Ubuntu-Web instance.
- Prometheus/Grafana stack is a monitoring stack.

## DR Plan
### Pre-Steps:
List steps you would perform to setup the infrastructure in the other region. It doesn't have to be super detailed, but high-level should suffice.
- Create new bucket in new region and copy AMI from us-east-1 to this bucket
- Create new keypair with name udacity
- Edit some value for terraform config for new region such as azs, arn of primary RDS cluster...
- Deploy VPC, Subnet, LB, EC2 instance for Ubuntu-Web, EKS Cluster and RDS Cluster via Terraform
- Deploy Monitoring Stack 

## Steps:
You won't actually perform these steps, but write out what you would do to "fail-over" your application and database cluster to the other region. Think about all the pieces that were setup and how you would use those in the other region
- Point DNS to the LB (of Ubuntu-Web/Grafana) in new region
- Stop RDS cluster in primary zone, and the RDS cluster in secondary zone will be promoted to primary.
