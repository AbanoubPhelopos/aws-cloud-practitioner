# AWS Global Infrastructure

## The Big Picture

AWS Global Infrastructure is the foundation that enables cloud computing to deliver compute power, storage, and services **anywhere in the world**. Understanding how AWS structures its global network is essential for designing resilient, high-performance applications.

---

## AWS Global Infrastructure Overview

```mermaid
graph TD
    A[AWS Global Infrastructure] --> B[Regions]
    A --> C[Availability Zones]
    A --> D[Edge Locations]
    
    B --> B1[36 Independent Regions Worldwide]
    B --> B2[Multiple AZs per Region]
    
    C --> C1[One or More Data Centers]
    C --> C2[Independent Power/Cooling/Networking]
    C --> C3[Connected via Low-Latency Links]
    
    D --> D1[CloudFront CDN]
    D --> D2[Route53 DNS]
    D --> D3[Edge Caching]
```

---

## AWS Regions

An **AWS Region** is a geographical area containing multiple Availability Zones.

### Key Facts

| Attribute | Details |
|-----------|---------| 
| **Total Regions** | 36 completely independent regions globally |
| **AZs per Region** | Multiple Availability Zones |
| **Isolation** | Regions are fully isolated from each other |

### Region Selection Criteria

```mermaid
flowchart TD
    A[Select AWS Region] --> B[Latency]
    A --> C[Available Services]
    A --> D[Cost]
    A --> E[Compliance]
    
    B --> B1[Closest to Users]
    C --> C1[Some Services Limited<br/>in Certain Regions]
    D --> D1[Pricing Varies by<br/>Region]
    E --> E1[Data Residency<br/>Requirements]
```

When choosing an AWS Region, consider:

1. **Latency** - Proximity to your users for optimal performance
2. **Available Services** - Some services may not be available in all regions
3. **Cost** - Prices vary between regions
4. **Compliance** - Data residency and regulatory requirements

### Example Regions

| Region Code | Location |
|-------------|----------|
| `us-east-1` | US East (N. Virginia) |
| `eu-west-3` | Europe (Paris) |
| `eu-south-2` | Europe (Spain) |
| `ap-southeast-1` | Asia Pacific (Singapore) |

---

## Availability Zones (AZs)

An **Availability Zone (AZ)** is composed of one or more data centers equipped with **independent power, cooling, and networking** infrastructure to ensure operational resilience.

```mermaid
flowchart LR
    subgraph Region["AWS Region"]
        direction LR
        AZ1[AZ-1<br/>Data Center]
        AZ2[AZ-2<br/>Data Center]
        AZ3[AZ-3<br/>Data Center]
        
        AZ1 <-->|Low Latency| AZ2
        AZ2 <-->|Low Latency| AZ3
        AZ1 <-->|Low Latency| AZ3
    end
    
    Internet[Internet] -->|Traffic| Region
```

### AZ Naming Convention

- `eu-south-2a` - First AZ in Spain region
- `eu-south-2b` - Second AZ in Spain region
- `eu-south-2c` - Third AZ in Spain region
- `eu-west-3a` - First AZ in Paris region
- `eu-west-3b` - Second AZ in Paris region
- `eu-west-3c` - Third AZ in Paris region
- `ap-southeast-2a` - First AZ in Sydney region
- `ap-southeast-2b` - Second AZ in Sydney region
- `ap-southeast-2c` - Third AZ in Sydney region

### AZ Count per Region

| Attribute | Details |
|-----------|---------|
| **Typical Count** | 3 AZs per region |
| **Minimum** | 3 AZs |
| **Maximum** | 6 AZs |

### Why AZs Matter

| Feature | Benefit |
|---------|---------|
| **Independent Power** | Failure in one AZ doesn't affect others |
| **Independent Cooling** | Environmental issues contained |
| **Independent Networking** | Network partitions don't cascade |
| **Low-Latency Links** | AZs work together as a unified region |
| **Operational Resilience** | Applications can span multiple AZs |

---

## AWS Data Centers

AWS Data Centers are the physical facilities that house the infrastructure powering AWS services.

```mermaid
flowchart TD
    DC[AWS Data Centers] --> Security[Security]
    DC --> Scalability[Scalability]
    DC --> Redundancy[Redundancy]
    DC --> Compliance[Compliance]
    DC --> Energy[Energy Efficiency]
    
    Security --> S1[Advanced Access Controls]
    Security --> S2[24/7 Monitoring]
    
    Scalability --> Sc1[Easily Scale Resources]
    Scalability --> Sc2[Meet Growing Demands]
    
    Redundancy --> R1[Redundant Power Systems]
    Redundancy --> R2[Redundant Cooling]
    
    Compliance --> C1[Strict Privacy Standards]
    Compliance --> C2[Industry Certifications]
    
    Energy --> E1[Green Technologies]
    Energy --> E2[Minimal Environmental Impact]
```

### Key Characteristics

| Characteristic | Description |
|----------------|-------------|
| **Security** | Equipped with advanced security measures; access restricted to authorized personnel |
| **Location** | Located worldwide to reduce latency and improve performance |
| **Scalability** | Designed to easily scale resources to meet growing demands |
| **Redundancy** | Built with redundant power and cooling systems to ensure uptime |
| **Compliance** | Adhere to strict compliance standards for data privacy and security |
| **Energy Efficiency** | Utilize green technologies to minimize environmental impact |

---

## AWS Points of Presence

AWS Points of Presence provide low-latency content delivery and edge computing capabilities.

```mermaid
graph TD
    POP[AWS Points of Presence] --> EL[Edge Locations]
    POP --> RC[Regional Caches]
    
    EL --> EL1[400+ Global Edge Locations]
    EL --> EL2[Content Caching]
    EL --> EL3[DNS - Route53]
    
    RC --> RC1[10+ Regional Caches]
    RC --> RC2[Dynamic Content Delivery]
    
    POP --> Global[Global Coverage]
    Global --> G1[90+ Cities]
    Global --> G2[40+ Countries]
```

### Key Facts

| Attribute | Details |
|-----------|---------|
| **Total Points of Presence** | 400+ |
| **Edge Locations** | 400+ globally |
| **Regional Caches** | 10+ |
| **Geographic Spread** | 90+ cities in 40+ countries |

### Services Using Points of Presence

| Service | Purpose |
|---------|---------|
| **CloudFront** | Content delivery network (CDN) |
| **Route53** | DNS service |
| **Edge Caching** | Cache frequently accessed content closer to users |

---

## Shared Responsibility Model

The **Shared Responsibility Model** is a security framework where protection of cloud computing resources is shared between **AWS** (security **of** the cloud) and **customers** (security **in** the cloud).

```mermaid
graph TD
    SRM[Shared Responsibility Model] --> AWS[AWS Responsibility]
    SRM --> Customer[Customer Responsibility]
    
    AWS --> AWS1[Security OF the Cloud]
    AWS --> AWS2[Hardware & Software]
    AWS --> AWS3[Networking & Facilities]
    AWS --> AWS4[Physical Security]
    AWS --> AWS5[Environmental Controls]
    
    Customer --> C1[Security IN the Cloud]
    Customer --> C2[Your Data]
    Customer --> C3[Access Management]
    Customer --> C4[Application Security]
    Customer --> C5[Service Configuration]
```

### AWS Responsibility - Security OF the Cloud

| Aspect | Description |
|--------|-------------|
| **Physical Security** | Data centers with restricted access, security guards, CCTV |
| **Environmental Controls** | Power, cooling, fire suppression systems |
| **Infrastructure** | Hardware, software, networking, facilities |
| **Core Services** | EC2, S3, DynamoDB underlying infrastructure |

### Customer Responsibility - Security IN the Cloud

| Aspect | Description |
|--------|-------------|
| **Data** | Encrypting data, managing access to data |
| **Applications** | Application-level security, vulnerability management |
| **Identity Management** | IAM roles, policies, and access controls |
| **Service Configuration** | Security settings for individual services |

### Collaboration

> ⚠️ **Important:** Both AWS and customers must work together to ensure a secure and compliant cloud environment.
>
> There are multiple shared responsibility models that come with each AWS service. Make sure to understand the specific responsibilities for each service you use!

---

## Responsibility by Service Model

### Layer Breakdown

| Layer | On-Premises | Private Cloud | IaaS | PaaS | SaaS |
|-------|-------------|---------------|------|------|------|
| **Data & Access** | You | You | You | You | You |
| **Applications** | You | You | You | You | Provider |
| **Runtime** | You | You | You | Provider | Provider |
| **Operating System** | You | You | You | Provider | Provider |
| **Virtual Machine** | You | Provider | Provider | Provider | Provider |
| **Compute** | You | Provider | Provider | Provider | Provider |
| **Networking** | You | Provider | Provider | Provider | Provider |
| **Storage** | You | Provider | Provider | Provider | Provider |

---

## AWS Services & Architecture Components

### Compute & Containers

```mermaid
flowchart TD
    A[Compute Services] --> B[EC2]
    A --> C[Lambda]
    A --> D[ECS/ECR]
    
    B --> B1[Virtual Instances]
    B --> B2[Auto Scaling Group]
    B --> B3[Compute Optimized]
    B --> B4[Memory Optimized]
    
    C --> C1[Serverless]
    C --> C2[Event-Driven]
    C --> C3[Pay per execution]
    
    D --> D1[Container Orchestration]
    D --> D2[Container Registry]
```

| Service | Description | Use Case |
|---------|-------------|----------|
| **EC2** | Elastic Compute Cloud - Virtual servers | General-purpose compute |
| **Lambda** | Serverless compute service | Event-driven functions |
| **ECS** | Elastic Container Service | Container orchestration |
| **ECR** | Elastic Container Registry | Store container images |

### EC2 Instance Types

| Type | Focus | Example Workloads |
|------|-------|-------------------|
| **Compute Optimized** | CPU | ML inference, scientific computing |
| **Memory Optimized** | RAM | In-memory databases, caching |

### Storage & Databases

```mermaid
flowchart LR
    S[Storage] --> S3[S3<br/>Object Storage]
    S --> EBS[EBS<br/>Block Storage]
    S --> EFS[EFS<br/>File System]
    
    D[Databases] --> Dynamo[DynamoDB<br/>NoSQL]
    D --> RDS[RDS<br/>Relational]
```

| Service | Type | Description |
|---------|------|-------------|
| **S3** | Object Storage | Scalable storage with encryption keys |
| **EBS** | Block Storage | Persistent block storage for EC2 (e.g., 50 GB) |
| **EFS** | File System | Managed network file system |
| **DynamoDB** | NoSQL | Managed NoSQL database |
| **RDS** | Relational | Managed relational database service |

### Networking, Management & Security

```mermaid
flowchart TD
    N[Networking] --> ELB[ELB<br/>Elastic Load Balancer]
    N --> CloudWatch[CloudWatch<br/>Monitoring]
    N --> IAM[IAM<br/>Access Control]
    N --> SNS[SNS<br/>Notifications]
    
    ELB --> ELB1[Distributes Traffic<br/>Across EC2]
    CloudWatch --> CW1[Logs & Alarms]
    IAM --> IAM1[Permissions & Roles]
    SNS --> SNS1[Pub/Sub Messaging]
```

| Service | Category | Description |
|---------|----------|-------------|
| **ELB** | Networking | Distributes incoming traffic across EC2 instances |
| **IAM** | Security | Manages permissions and identity access |
| **CloudWatch** | Management | Infrastructure monitoring, logs, alarms |
| **SNS** | Messaging | Pub/sub messaging and notifications |

---

## AWS Pricing Options

```mermaid
graph LR
    P[Pricing] --> OD[On-Demand]
    P --> RI[Reserved Instances]
    
    OD --> OD1[Pay by second/hour]
    OD --> OD2[No commitment]
    
    RI --> RI1[Multi-year commitment]
    RI --> RI2[Discounted rate]
    RI --> RI3[Save up to 75%]
```

### On-Demand vs Reserved

| Aspect | On-Demand | Reserved Instances |
|--------|-----------|-------------------|
| **Commitment** | None | 1 or 3 years |
| **Pricing** | Pay per second/hour | Discounted rate |
| **Savings** | Full price | Up to 75% savings |
| **Use Case** | Short-term, variable workloads | Predictable, steady-state workloads |

---

## Key Takeaways

1. **AWS Regions** contain multiple Availability Zones and are completely isolated (36 regions globally)
2. **Availability Zones** provide operational resilience (min 3, max 6 AZs per region)
3. **Points of Presence** - 400+ Edge Locations & 10+ Regional Caches across 90+ cities in 40+ countries
4. **Data Centers** are secured, scalable, redundant, compliant, and energy efficient
5. **Shared Responsibility Model** - AWS handles "Security OF the Cloud", customers handle "Security IN the Cloud"
6. **Service Models** define responsibility layers - as you move to PaaS/SaaS, AWS manages more
7. **EC2** provides virtual servers with various instance types (compute/memory optimized)
8. **Lambda** enables serverless, event-driven computing
9. **Storage Services**: S3 (objects), EBS (block), EFS (file)
10. **Database Services**: DynamoDB (NoSQL), RDS (relational)
11. **Networking**: ELB distributes traffic, IAM manages access
12. **Monitoring**: CloudWatch for logs, alarms, and performance
13. **Pricing**: On-Demand (flexible) vs Reserved (discounted commitment)

---

## Next Steps

⬅️ Previous: [Cloud Models & Pricing](./02-cloud-models-and-pricing.md) | ➡️ Next: [Cloud Adoption Framework](./04-cloud-adoption-framework.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../README.md).*
