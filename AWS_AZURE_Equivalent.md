| Category          | Azure Service               | AWS Service                | Key Differences                                                                 |
|-------------------|-----------------------------|----------------------------|---------------------------------------------------------------------------------|
| **Compute**       |                             |                            |                                                                                 |
| Virtual Machines  | Azure Virtual Machines      | Amazon EC2                 | Azure better for Windows, AWS has more instance types                           |
| Serverless        | Azure Functions             | AWS Lambda                 | AWS has more event sources, Azure integrates with Microsoft stack               |
| Containers        | AKS (Azure Kubernetes)      | Amazon EKS                 | AKS simpler setup, EKS deeper AWS integration                                   |
| PaaS              | Azure App Service           | Elastic Beanstalk          | Azure better for .NET, Beanstalk more language-agnostic                         |

| **Storage**       |                             |                            |                                                                                 |
| Object Storage    | Azure Blob Storage          | Amazon S3                  | S3 more mature, Blob Storage better Azure integration                           |
| Block Storage     | Azure Managed Disks         | Amazon EBS                 | EBS more performance options, Azure better for Windows                          |
| File Storage      | Azure Files                 | Amazon EFS                 | EFS Linux-optimized, Azure Files supports SMB/NFS                               |
| Archive Storage   | Azure Archive Storage       | Amazon Glacier             | Similar retrieval times and pricing                                             |

| **Databases**     |                             |                            |                                                                                 |
| Relational DB     | Azure SQL Database          | Amazon RDS                 | Azure optimized for MS SQL, RDS supports more engines                           |
| NoSQL             | Azure Cosmos DB             | Amazon DynamoDB            | CosmosDB multi-model, DynamoDB serverless-first                                 |
| Caching           | Azure Cache for Redis       | Amazon ElastiCache         | Similar performance characteristics                                             |

| **Networking**    |                             |                            |                                                                                 |
| CDN               | Azure CDN                   | Amazon CloudFront          | CloudFront has more edge locations globally                                     |
| Load Balancing    | Azure Load Balancer         | AWS ALB/NLB                | AWS offers separate ALB (application) and NLB (network) load balancers          |
| DNS               | Azure DNS                   | Amazon Route 53            | Route 53 has more advanced routing policies                                     |
| Hybrid Cloud      | Azure ExpressRoute/VPN      | AWS Direct Connect/VPN     | AWS has more global points of presence                                          |

| **AI/ML**         |                             |                            |                                                                                 |
| Machine Learning  | Azure Machine Learning      | Amazon SageMaker           | SageMaker more mature, Azure integrates with Microsoft tools                    |
| Cognitive Services| Azure Cognitive Services    | AWS AI Services            | Azure offers pre-built APIs for vision, speech, etc.                            |
