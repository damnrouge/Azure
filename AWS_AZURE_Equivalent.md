```mermaid
graph TD
    A[Compute Services] --> B[EC2 Elastic Compute Cloud] --> C[Azure Virtual Machines (VMs)]
    A --> D[Lambda] --> E[Azure Functions]
    A --> F[Elastic Beanstalk] --> G[Azure App Service]
    A --> H[Amazon Lightsail] --> I[Azure Virtual Machine Scale Sets]

    J[Storage Services] --> K[S3 Simple Storage Service] --> L[Azure Blob Storage]
    J --> M[EBS Elastic Block Store] --> N[Azure Disk Storage]
    J --> O[Glacier] --> P[Azure Blob Storage Archive Tier]
    J --> Q[FSx Managed File Storage] --> R[Azure NetApp Files]

    S[Networking Services] --> T[VPC Virtual Private Cloud] --> U[Azure Virtual Network (VNet)]
    S --> V[Elastic Load Balancer ELB] --> W[Azure Load Balancer]
    S --> X[Amazon Route 53] --> Y[Azure DNS]
    S --> Z[Direct Connect] --> AA[Azure ExpressRoute]
    S --> AB[CloudFront] --> AC[Azure CDN]

    AD[Database Services] --> AE[RDS Relational Database Service] --> AF[Azure SQL Database]
    AD --> AG[DynamoDB] --> AH[Azure Cosmos DB]
    AD --> AI[Redshift] --> AJ[Azure Synapse Analytics]
    AD --> AK[Aurora] --> AL[Azure Database for MySQL/PostgreSQL]

    AM[Identity & Access Management] --> AN[IAM Identity and Access Management] --> AO[Azure Active Directory (AAD)]
    AM --> AP[Cognito] --> AQ[Azure AD B2C]
    AM --> AR[STS Security Token Service] --> AS[Azure AD Identity Protection]

    AT[Security Services] --> AU[GuardDuty] --> AV[Azure Sentinel]
    AT --> AW[Inspector] --> AX[Azure Security Center]
    AT --> AY[WAF Web Application Firewall] --> AZ[Azure Web Application Firewall]
    AT --> BA[Macie] --> BB[Azure Purview]

    BC[Monitoring & Management] --> BD[CloudWatch] --> BE[Azure Monitor]
    BC --> BF[CloudTrail] --> BG[Azure Activity Log]
    BC --> BH[AWS Config] --> BI[Azure Policy]
    BC --> BJ[Systems Manager] --> BK[Azure Automation]

    BL[Artificial Intelligence & Machine Learning] --> BM[SageMaker] --> BN[Azure Machine Learning]
    BL --> BO[Rekognition] --> BP[Azure Cognitive Services - Vision]
    BL --> BQ[Lex] --> BR[Azure Bot Services]
    BL --> BS[Polly] --> BT[Azure Speech Services]

    BU[Serverless Computing] --> BV[Lambda] --> BW[Azure Functions]
    BU --> BX[API Gateway] --> BY[Azure API Management]
    BU --> BZ[Step Functions] --> CA[Azure Logic Apps]

    CB[Backup & Disaster Recovery] --> CC[AWS Backup] --> CD[Azure Backup]
    CB --> CE[Elastic Disaster Recovery] --> CF[Azure Site Recovery]
```
