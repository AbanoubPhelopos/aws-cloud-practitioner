# EC2 Instance Types & Sizing

## The Big Picture

EC2 offers a **variety of instance types** designed for different use cases. Choosing the right type is critical for **optimal performance and cost**. This module explores instance categories, sizing configuration, and the concept of **Right Sizing**.

---

## EC2 Sizing & Configuration

When configuring an EC2 instance, you need to specify several key attributes:

```mermaid
graph TD
    Config[EC2 Instance Configuration] --> OS[Operating System]
    Config --> CPU[Compute Power & Cores]
    Config --> RAM[Random-Access Memory]
    Config --> Storage[Storage Space]
    Config --> Network[Network Card]
    Config --> FW[Firewall Rules]
    Config --> Bootstrap[Bootstrap Script]
    
    OS --> OS1[Linux]
    OS --> OS2[Windows]
    OS --> OS3[macOS]
    
    CPU --> CPU1[CPU capacity]
    CPU --> CPU2[Number of cores]
    
    RAM --> RAM1[Amount needed for<br/>optimal performance]
    
    Storage --> S1[Network-attached<br/>EBS / EFS]
    Storage --> S2[Hardware<br/>EC2 Instance Store]
    
    Network --> N1[Network card speed]
    Network --> N2[Public IP address]
    
    FW --> FW1[Security Group<br/>Inbound/Outbound rules]
    
    Bootstrap --> B1[User Data script<br/>Configure at first launch]
```

### Configuration Attributes

| Attribute | Description | Options |
|-----------|-------------|---------|
| **Operating System** | OS to run on instance | Linux, Windows, macOS |
| **CPU** | Processing capacity | Varies by instance type |
| **RAM** | Memory size | Varies by instance type |
| **Storage** | Storage space | Network-attached (EBS/EFS) or Hardware (Instance Store) |
| **Network Card** | Network performance | Speed configuration, Public IP |
| **Firewall Rules** | Network security | Security Groups (inbound/outbound) |
| **Bootstrap Script** | Initial configuration | EC2 User Data |

---

## EC2 Naming Convention

AWS follows a **specific naming convention** for EC2 instances:

```mermaid
graph LR
    Name[Instance Name: m5.2xlarge] --> Class[Instance Class: m]
    Name --> Gen[Generation: 5]
    Name --> Size[Size: 2xlarge]
    
    Class --> C1[m = General Purpose]
    Gen --> G1[AWS improves over time]
    Size --> S1[Size within the class]
```

### Naming Breakdown

| Component | Example | Meaning |
|-----------|---------|---------|
| **Instance Class** | `m` | Type of instance (general purpose, compute, memory, etc.) |
| **Generation** | `5` | Version - AWS improves over time |
| **Size** | `2xlarge` | Size within the instance class |

> **Example:** `m5.2xlarge` = General Purpose, 5th generation, 2xlarge size

---

## EC2 Instance Type Categories

```mermaid
graph TD
    IT[EC2 Instance Types] --> GP[General Purpose]
    IT --> CO[Compute Optimized]
    IT --> MO[Memory Optimized]
    IT --> AC[Accelerated Computing]
    IT --> SO[Storage Optimized]
    IT --> HPC[HPC Optimized]
    
    GP --> GP1[Web servers, code repos]
    CO --> CO1[Batch processing, HPC, ML]
    MO --> MO1[In-memory databases, caching]
    AC --> AC1[GPU, ML, graphics]
    SO --> SO1[OLTP, data warehousing]
    HPC --> HPC1[Scientific computing]
```

### Instance Type Overview

| Type | Best For | Key Characteristic |
|------|----------|-------------------|
| **General Purpose** | Diverse workloads | Balanced resources |
| **Compute Optimized** | Compute-intensive tasks | High-performance processors |
| **Memory Optimized** | Large data sets in memory | High memory-to-CPU ratio |
| **Accelerated Computing** | GPU workloads | Hardware accelerators |
| **Storage Optimized** | Storage-intensive tasks | High sequential I/O |
| **HPC Optimized** | High-performance computing | Maximum compute power |

---

## General Purpose Instances

```mermaid
graph LR
    GP[General Purpose] --> Bal[Balances Resources]
    Bal --> B1[Compute]
    Bal --> B2[Memory]
    Bal --> B3[Networking]
    
    GP --> Use[Use Cases]
    Use --> U1[Web Servers]
    Use --> U2[Code Repositories]
```

| Aspect | Details |
|--------|---------|
| **Description** | Great for diverse workloads like web servers and code repositories |
| **Balance** | Compute + Memory + Networking resources effectively |
| **Use Cases** | Web servers, code repositories, small databases |

---

## Compute Optimized Instances

```mermaid
graph TD
    CO[Compute Optimized] --> Ideal[Ideal for compute-intensive<br/>tasks demanding<br/>high-performance processors]
    
    Ideal --> Use[Suitable For]
    Use --> S1[Batch processing workloads]
    Use --> S2[Media transcoding]
    Use --> S3[High-performance web servers]
    Use --> S4[High-performance computing HPC]
    Use --> S5[Scientific modeling and ML]
    Use --> S6[Dedicated gaming servers]
```

| Aspect | Details |
|--------|---------|
| **Description** | Ideal for compute-intensive tasks demanding high-performance processors |
| **Best For** | Batch processing, media transcoding, HPC |
| **Use Cases** | High-performance web servers, scientific modeling, ML, gaming servers |

---

## Memory Optimized Instances

```mermaid
graph TD
    MO[Memory Optimized] --> Fast[Fast performance for<br/>workloads processing<br/>large data sets in memory]
    
    Fast --> Use[Suitable For]
    Use --> S1[High-performance relational<br/>non-relational databases]
    Use --> S2[Distributed web-scale<br/>cache stores]
    Use --> S3[In-memory databases<br/>optimized for BI]
    Use --> S4[Real-time processing<br/>of large unstructured data]
```

| Aspect | Details |
|--------|---------|
| **Description** | Fast performance for workloads processing large data sets in memory |
| **Best For** | In-memory databases, real-time analytics |
| **Use Cases** | High-performance databases, distributed caches, BI applications |

---

## Storage Optimized Instances

```mermaid
graph TD
    SO[Storage Optimized] --> Ideal[Ideal for storage-intensive<br/>tasks with high, sequential<br/>read and write access<br/>to large data sets on local storage]
    
    Ideal --> Use[Suitable For]
    Use --> S1[High-frequency OLTP systems]
    Use --> S2[Relational and NoSQL databases]
    Use --> S3[Cache for in-memory databases<br/>e.g., Redis]
    Use --> S4[Data warehousing applications]
    Use --> S5[Distributed file systems]
```

| Aspect | Details |
|--------|---------|
| **Description** | Ideal for storage-intensive tasks with high sequential read/write access |
| **Best For** | OLTP systems, data warehousing |
| **Use Cases** | Relational/NoSQL databases, Redis cache, distributed file systems |

---

## Instance Features Overview

```mermaid
mindmap
  root((Instance Features))
    Performance
      CPU capacity
      Memory
      Network bandwidth
      Storage I/O
    Storage Options
      EBS - Network
      EFS - Network file
      Instance Store - Hardware
    Networking
      Network card speed
      Public IP
      Enhanced networking
    Security
      Security groups
      Network ACLs
      IAM roles
    Management
      User Data scripts
      CloudWatch monitoring
      Tags & metadata
```

---

## Measuring Instance Performance

```mermaid
graph TD
    Perf[Instance Performance] --> P1[vCPUs<br/>Virtual CPU count]
    Perf --> P2[Clock Speed<br/>GHz]
    Perf --> P3[Memory<br/>RAM in GB]
    Perf --> P4[Network<br/>Gbps bandwidth]
    Perf --> P5[Storage I/O<br/>IOPS, throughput]
    Perf --> P6[EBS Optimization<br/>Dedicated bandwidth]
    
    P1 --> Apps[Application performance]
    P2 --> Compute[Compute performance]
    P3 --> Memory[Memory-intensive tasks]
    P4 --> Network[Network performance]
    P5 --> Storage[Storage performance]
    P6 --> EBS[EBS performance]
```

---

## AWS Right Sizing

### What is Right Sizing?

```mermaid
graph TD
    RS[Right Sizing] --> Def[Definition]
    Def --> D1[Matching instance types and sizes<br/>to workload requirements<br/>for optimal performance and cost]
    
    RS --> Key[Key Principle]
    Key --> K1[Selecting the most powerful<br/>isn't always the best<br/>due to cloud elasticity]
    
    RS --> Strategy[Strategy]
    Strategy --> S1[Start with smaller instances]
    Strategy --> S2[Scaling up is easy]
    Strategy --> S3[Review deployed instances<br/>for downsizing opportunities]
    Strategy --> S4[Reduce costs without<br/>compromising capacity]
```

### Right Sizing Process

```mermaid
graph LR
    Start[Start Small] --> Monitor[Monitor Usage]
    Monitor --> Analyze[Analyze Performance]
    Analyze --> Adjust[Adjust Size]
    Adjust --> Review[Continuous Review]
    Review --> Monitor
    
    Start --> S1[Begin with smaller<br/>instance types]
    Monitor --> M1[Track CloudWatch metrics]
    Analyze --> A1[Identify underutilized<br/>resources]
    Adjust --> Ad1[Scale up or down<br/>as needed]
    Review --> R1[Requirements change<br/>over time]
```

### When to Right Size

```mermaid
graph TD
    When[Right Sizing Timing] --> Before[Before Migration]
    When --> After[After Onboarding]
    When --> Continuous[Continuously]
    
    Before --> B1[Optimize before<br/>moving to AWS]
    After --> A1[Review initial setup]
    Continuous --> C1[Requirements change<br/>over time]
    Continuous --> C2[New usage patterns<br/>emerge]
```

### Right Sizing Tools

| Tool | Description |
|------|-------------|
| **CloudWatch** | Monitor performance metrics |
| **Cost Explorer** | Analyze spending patterns |
| **Trusted Advisor** | AWS optimization recommendations |
| **Third-party tools** | Various optimization platforms |

---

## Instance Selection Strategy

### Decision Flow

```mermaid
flowchart TD
    Start[Need to Choose Instance] --> Q1{What's the primary workload?}
    
    Q1 -->|Web/App servers| GP[General Purpose]
    Q1 -->|Batch/ML/HPC| CO[Compute Optimized]
    Q1 -->|In-memory DB/Cache| MO[Memory Optimized]
    Q1 -->|ML Training/GPU| AC[Accelerated Computing]
    Q1 -->|OLTP/Data Warehouse| SO[Storage Optimized]
    Q1 -->|Scientific Compute| HPC[HPC Optimized]
    
    GP --> Size{Choose Size}
    CO --> Size
    MO --> Size
    AC --> Size
    SO --> Size
    HPC --> Size
    
    Size --> Small[Start Small]
    Small --> Monitor[Monitor]
    Monitor --> Adjust[Scale as Needed]
```

### Selection Matrix

| Workload | Recommended Type | Reason |
|----------|-----------------|--------|
| **Web Server** | General Purpose | Balanced resources |
| **Game Server** | Compute Optimized | High CPU performance |
| **Redis Cache** | Memory Optimized | Fast in-memory access |
| **ML Training** | Accelerated Computing | GPU acceleration |
| **OLTP Database** | Storage Optimized | High I/O performance |
| **HPC Cluster** | HPC Optimized | Maximum compute power |

---

## Comparison: All Instance Types

```mermaid
graph TD
    Compare[Instance Type Comparison] --> GP[General Purpose<br/>Balanced]
    Compare --> CO[Compute Optimized<br/>High CPU]
    Compare --> MO[Memory Optimized<br/>High RAM]
    Compare --> AC[Accelerated Computing<br/>GPU/HW]
    Compare --> SO[Storage Optimized<br/>High I/O]
    Compare --> HPC[HPC Optimized<br/>Max Compute]
    
    GP --> GPMetric[Balance: 1:4 CPU:RAM<br/>Network: Moderate]
    CO --> COMetric[High vCPU<br/>Low RAM ratio]
    MO --> MOMetric[High RAM:CPU ratio<br/>Large memory]
    AC --> ACMetric[GPU/FPGA<br/>Specialized HW]
    SO --> SOMetric[High IOPS<br/>Sequential I/O]
    HPC --> HPCMetric[Highest performance<br/>Lowest latency]
```

---

## Best Practices

### Start Small, Scale Up

```mermaid
graph LR
    A[Start: t3.micro<br/>1 vCPU, 1GB RAM] -->|Monitor| B[t3.small<br/>1 vCPU, 2GB RAM]
    B -->|Monitor| C[t3.medium<br/>2 vCPU, 4GB RAM]
    C -->|Monitor| D[m5.large<br/>2 vCPU, 8GB RAM]
    D -->|Monitor| E[m5.xlarge<br/>4 vCPU, 16GB RAM]
    
    A -.->|If insufficient| B
    B -.->|If insufficient| C
    C -.->|If insufficient| D
    D -.->|If insufficient| E
```

### Best Practices Checklist

```mermaid
graph TD
    BP[Best Practices] --> B1[Start with smallest<br/>instance that works]
    BP --> B2[Monitor CPU, memory,<br/>network metrics]
    BP --> B3[Right size before migration]
    BP --> B4[Review regularly<br/>after deployment]
    BP --> B5[Use Cost Explorer<br/>for spending analysis]
    BP --> B6[Use Trusted Advisor<br/>for recommendations]
    BP --> B7[Consider Reserved Instances<br/>for predictable workloads]
    BP --> B8[Match instance to<br/>workload requirements]
```

---

## Key Takeaways

1. **EC2 Configuration** includes: OS, CPU, RAM, Storage, Network, Firewall, Bootstrap Script
2. **Naming Convention**: `m5.2xlarge` = class.generation.size
3. **6 Instance Categories**:
   - General Purpose (balanced)
   - Compute Optimized (high CPU)
   - Memory Optimized (high RAM)
   - Accelerated Computing (GPU)
   - Storage Optimized (high I/O)
   - HPC Optimized (maximum performance)
4. **Right Sizing** = matching instance types/sizes to workload requirements
5. **Cloud elasticity** means you don't need to start with the most powerful
6. **Start small and scale up** based on actual usage
7. **Continuous review** - requirements change over time
8. **Tools for Right Sizing**: CloudWatch, Cost Explorer, Trusted Advisor
9. **Use User Data scripts** for initial configuration automation
10. **Choose appropriate storage**: EBS/EFS (network) vs Instance Store (hardware)

---

## Next Steps

⬅️ Previous: [Amazon EC2](./11-amazon-ec2.md) | ➡️ Next: [EC2 Pricing & Purchasing](./13-ec2-pricing.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*