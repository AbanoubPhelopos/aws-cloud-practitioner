# Amazon EC2 Instances

## The Big Picture

> **Amazon EC2 Instances = a virtual server in the AWS Cloud.** When you launch an EC2 instance, the instance type that you specify determines the hardware available to your instance.

EC2 allows users to rent **virtual servers** (known as instances) on which they can run their own applications. Each instance type offers a different balance of compute, memory, network, and storage resources.

---

## What is EC2?

```mermaid
graph TD
    EC2[Amazon EC2<br/>Elastic Compute Cloud] --> Virt[Virtual Server<br/>in the Cloud]
    EC2 --> IaaS[Infrastructure as a Service]
    EC2 --> Rent[Rent Virtual Machines]
    
    Virt --> IT[Instance Type<br/>Determines Hardware]
    IT --> Compute[Compute Power]
    IT --> Memory[Memory]
    IT --> Network[Network]
    IT --> Storage[Storage]
```

### Definition

| Attribute | Description |
|-----------|-------------|
| **EC2** | Elastic Compute Cloud |
| **Instance** | Virtual server in AWS Cloud |
| **Instance Type** | Determines the hardware configuration |
| **Category** | Infrastructure as a Service (IaaS) |
| **Function** | Renting virtual machines to run applications |

---

## EC2 Capabilities Overview

```mermaid
graph TD
    Capabilities[EC2 Capabilities] --> C1[Renting Virtual Machines]
    Capabilities --> C2[Storing Data on Virtual Drives]
    Capabilities --> C3[Distributing Load]
    Capabilities --> C4[Scaling Services]
    Capabilities --> C5[Operating System Flexibility]
    Capabilities --> C6[Cost Optimization]
    
    C1 --> EC2[EC2]
    C2 --> EBS[EBS Volumes]
    C3 --> ELB[Elastic Load Balancer]
    C4 --> ASG[Auto Scaling Group]
    C5 --> OS[Multiple OS Support]
    C6 --> Scale[Up/Down on Demand]
```

### Core EC2 Capabilities

| Capability | AWS Service | Description |
|------------|-------------|-------------|
| **Renting Virtual Machines** | EC2 | Run virtual servers in the cloud |
| **Storing Data on Virtual Drives** | EBS | Persistent block storage |
| **Distributing Load** | ELB | Distribute traffic across machines |
| **Scaling Services** | ASG | Auto Scaling Group for dynamic scaling |

---

## Instance Types and Use Cases

```mermaid
graph TD
    IT[Instance Types] --> GP[General Purpose]
    IT --> CO[Compute Optimized]
    IT --> MO[Memory Optimized]
    IT --> SO[Storage Optimized]
    IT --> GPU[GPU/Accelerated Computing]
    
    GP --> GP1[Web Servers<br/>Small Databases]
    CO --> CO1[Batch Processing<br/>Gaming Servers]
    MO --> MO1[In-Memory Databases<br/>Real-time Analytics]
    SO --> SO1[Distributed File Systems<br/>Data Warehousing]
    GPU --> GPU1[Machine Learning<br/>Graphics Rendering]
```

### Instance Type Categories

| Category | Use Cases | Characteristics |
|----------|-----------|-----------------|
| **General Purpose** | Web servers, small databases | Balance of compute, memory, networking |
| **Compute Optimized** | Batch processing, gaming servers | High-performance processors |
| **Memory Optimized** | In-memory databases, real-time analytics | High memory-to-CPU ratio |
| **Storage Optimized** | Distributed file systems, data warehousing | High sequential read/write |
| **GPU Instances** | Machine learning, graphics rendering | Parallel processing capabilities |

---

## Key EC2 Features

### 1. Flexibility and Scalability

```mermaid
graph LR
    Demand[Demand] --> Up[Scale Up<br/>Add Resources]
    Demand --> Down[Scale Down<br/>Remove Resources]
    
    Up --> Performance[Performance Scalability]
    Down --> CostOpt[Cost Optimization]
    
    Performance --> Apps[Applications handle<br/>more load]
    CostOpt --> SaveMoney[Pay only for<br/>what you use]
```

| Feature | Benefit |
|---------|---------|
| **Scale Up** | Add resources as demand increases |
| **Scale Down** | Reduce resources when demand drops |
| **Cost Optimization** | Pay only for what you use |
| **Performance Scalability** | Handle increased workloads |

### 2. Operating System Support

```mermaid
graph TD
    OS[Operating System Support] --> Linux[Linux Variants]
    OS --> Windows[Windows Server]
    OS --> Custom[Custom AMIs]
    
    Linux --> L1[Amazon Linux]
    Linux --> L2[Ubuntu]
    Linux --> L3[Red Hat]
    Linux --> L4[Debian]
    
    Windows --> W1[Windows Server 2019]
    Windows --> W2[Windows Server 2022]
    
    Custom --> C1[Bring Your Own Image]
    Custom --> C2[Pre-configured Environments]
```

### 3. Common Use Cases

```mermaid
graph TD
    UC[EC2 Common Use Cases] --> Hosting[Hosting Websites]
    UC --> Apps[Running Applications]
    UC --> Data[Data Processing Tasks]
    UC --> Dev[Development & Testing]
    UC --> ML[Machine Learning]
    UC --> Batch[Batch Processing]
    
    Hosting --> Web[Web Applications<br/>Static Sites<br/>Dynamic Sites]
    Apps --> Enterprise[Enterprise Apps<br/>Microservices<br/>APIs]
    Data --> Processing[Big Data<br/>ETL Jobs<br/>Analytics]
    Dev --> Env[Dev Environments<br/>Testing<br/>CI/CD]
```

---

## User Data Script

### Bootstrapping with User Data

```mermaid
graph TD
    Launch[Launch EC2 Instance] --> UD[User Data Script]
    UD --> Execute[Executes Once<br/>During Initial Startup]
    Execute --> Bootstrap[Bootstrapping]
    
    Bootstrap --> T1[Install Updates]
    Bootstrap --> T2[Install Software]
    Bootstrap --> T3[Download Files]
    Bootstrap --> T4[Custom Configuration]
```

### Key Concepts

| Concept | Description |
|---------|-------------|
| **User Data Script** | Script executed at instance launch |
| **Bootstrapping** | Executing commands upon a machine's startup |
| **Execution Time** | Runs **once** during initial startup |
| **Purpose** | Automate startup tasks |

### User Data Common Tasks

```mermaid
graph LR
    Tasks[User Data Tasks] --> Updates[Installing Updates]
    Tasks --> Software[Installing Software]
    Tasks --> Downloads[Downloading Files<br/>from Internet]
    Tasks --> Custom[Any Other<br/>Custom Tasks]
    
    Updates --> Security[Security Patches]
    Updates --> Patches[System Patches]
    
    Software --> AppSW[Application Software]
    Software --> Deps[Dependencies]
    
    Downloads --> Config[Config Files]
    Downloads --> Assets[Application Assets]
```

### Example: Web Server Setup

```mermaid
graph TD
    Example[User Data Script Example] --> Step1[Step 1: Update System]
    Step1 --> Step2[Step 2: Install Apache/Nginx]
    Step2 --> Step3[Step 3: Download Web Files]
    Step3 --> Step4[Step 4: Start Web Server]
    Step4 --> Result[EC2 Instance<br/>Running Web Server<br/>with Static Web Page]
```

---

## EC2 in the AWS Ecosystem

### Related Services

```mermaid
graph LR
    EC2[EC2] --> EBS[EBS<br/>Storage]
    EC2 --> ELB[ELB<br/>Load Balancing]
    EC2 --> ASG[ASG<br/>Auto Scaling]
    EC2 --> SG[Security Groups<br/>Network Security]
    EC2 --> AMI[AMI<br/>Templates]
    EC2 --> ENI[ENI<br/>Networking]
    
    EBS --> Block[Block Storage<br/>Volumes]
    ELB --> Dist[Distribute Traffic]
    ASG --> Dyn[Dynamic Scaling]
    SG --> FW[Firewall Rules]
    AMI --> Temp[Pre-configured<br/>Server Images]
    ENI --> VNet[Virtual Network<br/>Interfaces]
```

### Service Relationships

| Service | Role | Relationship to EC2 |
|---------|------|---------------------|
| **EBS** | Storage | Persistent block storage for EC2 |
| **ELB** | Load Balancer | Distributes traffic across EC2 instances |
| **ASG** | Auto Scaling | Automatically adjusts EC2 capacity |
| **Security Groups** | Network Security | Virtual firewall for EC2 |
| **AMI** | Templates | Pre-configured images for EC2 launch |
| **ENI** | Networking | Virtual network interface for EC2 |

---

## EC2 Pricing Models

```mermaid
graph TD
    Pricing[EC2 Pricing Models] --> OD[On-Demand]
    Pricing --> RI[Reserved Instances]
    Pricing --> SP[Savings Plans]
    Pricing --> Spot[Spot Instances]
    Pricing --> DH[Dedicated Hosts]
    
    OD --> OD1[Pay by second/hour<br/>No commitment]
    RI --> RI1[1 or 3 year commitment<br/>Up to 75% savings]
    SP --> SP1[Flexible commitment<br/>Across services]
    Spot --> Spot1[Up to 90% savings<br/>Interruptible]
    DH --> DH1[Physical server<br/>Compliance needs]
```

### Pricing Comparison

| Model | Commitment | Savings | Use Case |
|-------|------------|---------|----------|
| **On-Demand** | None | None | Short-term, unpredictable workloads |
| **Reserved** | 1-3 years | Up to 75% | Predictable workloads |
| **Savings Plans** | 1-3 years | Up to 72% | Flexible compute usage |
| **Spot** | None | Up to 90% | Fault-tolerant, flexible workloads |
| **Dedicated Hosts** | On-demand or reservation | None | Compliance, licensing |

---

## EC2 Instance Lifecycle

```mermaid
graph LR
    Launch[Launch] --> Pending[Pending]
    Pending --> Running[Running]
    Running --> Stopping[Stopping]
    Stopping --> Stopped[Stopped]
    Stopped --> Running
    Running --> Rebooting[Rebooting]
    Rebooting --> Running
    Running --> Terminating[Terminating]
    Terminating --> Terminated[Terminated]
    
    Stopped -.->|Terminate| Terminated
```

### Lifecycle States

| State | Description |
|-------|-------------|
| **Pending** | Instance is being launched |
| **Running** | Instance is active and billing |
| **Stopping** | Instance is shutting down |
| **Stopped** | Instance is shut down, not billing for compute |
| **Rebooting** | Instance is restarting |
| **Terminating** | Instance is being deleted |
| **Terminated** | Instance is permanently deleted |

---

## Benefits of EC2

```mermaid
mindmap
  root((EC2 Benefits))
    Cost
      Pay-as-you-go
      No upfront investment
      Reserved pricing options
    Scalability
      Vertical scaling
      Horizontal scaling
      Auto scaling
    Flexibility
      Multiple OS support
      Custom configurations
      Various instance types
    Security
      Security groups
      Network isolation
      IAM integration
    Reliability
      High availability
      Fault tolerance
      Multiple regions
```

### Key Advantages

| Benefit | Description |
|---------|-------------|
| **Cost-Effective** | Pay-as-you-go pricing, no upfront costs |
| **Scalable** | Scale up/down based on demand |
| **Flexible** | Choose OS, configuration, instance type |
| **Secure** | Built-in security features and IAM integration |
| **Reliable** | High availability across multiple AZs |
| **Global** | Deploy in any region worldwide |

---

## EC2 vs Traditional Servers

```mermaid
graph LR
    subgraph Traditional["Traditional Servers"]
        T1[Physical Hardware]
        T2[Long procurement]
        T3[Fixed capacity]
        T4[Manual scaling]
        T5[High upfront cost]
    end
    
    subgraph EC2["Amazon EC2"]
        E1[Virtual Hardware]
        E2[Minutes to launch]
        E3[Elastic capacity]
        E4[Auto scaling]
        E5[Pay-as-you-go]
    end
```

### Comparison

| Aspect | Traditional Servers | Amazon EC2 |
|--------|--------------------|-----------| 
| **Procurement** | Weeks to months | Minutes |
| **Capacity** | Fixed | Elastic |
| **Scaling** | Manual, time-consuming | Automated, instant |
| **Cost Model** | High upfront | Pay-as-you-go |
| **Maintenance** | Customer managed | AWS managed infrastructure |
| **Geographic Reach** | Limited | Global regions |

---

## Key Takeaways

1. **EC2** = Elastic Compute Cloud = Virtual servers in AWS
2. **Instance Type** determines hardware: compute, memory, network, storage
3. **EC2 is IaaS** - Infrastructure as a Service
4. **Core Capabilities**:
   - Rent virtual machines (EC2)
   - Store data on virtual drives (EBS)
   - Distribute load (ELB)
   - Scale services (ASG)
5. **Instance Types** optimized for: General Purpose, Compute, Memory, Storage, GPU
6. **Multiple OS Support**: Linux, Windows, custom AMIs
7. **User Data Script**: Runs once at launch for bootstrapping
8. **Common Tasks**: Install updates, install software, download files, custom config
9. **Related Services**: EBS, ELB, ASG, Security Groups, AMI, ENI
10. **Pricing Options**: On-Demand, Reserved, Savings Plans, Spot, Dedicated Hosts
11. **Benefits**: Cost-effective, scalable, flexible, secure, reliable, global

---

## Next Steps

⬅️ Previous: [Network & Storage Deep Dive](./10-network-storage.md) | ➡️ Next: [EC2 Instance Types](./12-ec2-instance-types.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*