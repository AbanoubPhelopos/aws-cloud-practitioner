# Amazon EC2 - Elastic Compute Cloud

> ⏱️ **Estimated Study Time:** 25 minutes  
> 🎯 **CCP Exam Weight:** ~15-20% (Domain 3: Cloud Technology & Services)

---

## The Big Picture

**Amazon EC2 (Elastic Compute Cloud)** is AWS's flagship compute service, providing **virtual servers** in the cloud. It's an Infrastructure as a Service (IaaS) offering that allows you to rent virtual machines (instances) to run your applications. EC2 is the foundation of most AWS architectures and is heavily tested on the CCP exam.

---

## EC2 at a Glance

| Attribute | Detail |
|-----------|--------|
| **Full Name** | Elastic Compute Cloud |
| **Category** | Infrastructure as a Service (IaaS) |
| **Launch Year** | 2008 |
| **Use Case** | Virtual servers in the cloud |
| **Pricing** | Pay only for what you use |
| **OS Support** | Linux, Windows, macOS |

---

## EC2 Core Capabilities

```mermaid
graph TD
    EC2[🖥️ Amazon EC2 Capabilities] --> C1[💻 Renting Virtual Machines]
    EC2 --> C2[💾 Storing Data on Virtual Drives]
    EC2 --> C3[⚖️ Distributing Load]
    EC2 --> C4[📈 Scaling Services]
    EC2 --> C5[🔧 OS Flexibility]
    
    C1 --> EC2Service[EC2 Instances]
    C2 --> EBS[EBS Volumes]
    C3 --> ELB[Elastic Load Balancer]
    C4 --> ASG[Auto Scaling Group]
    C5 --> OSPicker[Multiple OS Options]
    
    style EC2 fill:#FF9900,color:#000
```

### Detailed Capabilities

| Capability | AWS Service | Description |
|------------|-------------|-------------|
| **Renting Virtual Machines** | EC2 | Run virtual servers in the cloud |
| **Storing Data** | EBS | Persistent block storage for EC2 |
| **Distributing Load** | ELB | Distribute traffic across multiple instances |
| **Scaling Services** | ASG | Automatically adjust capacity based on demand |
| **OS Flexibility** | Multiple AMIs | Choose Linux, Windows, or custom OS |

---

## EC2 Instance Types

### 6 Instance Categories

```mermaid
graph TD
    IT[🖥️ EC2 Instance Types] --> GP[⚖️ General Purpose]
    IT --> CO[🔥 Compute Optimized]
    IT --> MO[💾 Memory Optimized]
    IT --> AC[⚡ Accelerated Computing]
    IT --> SO[📦 Storage Optimized]
    IT --> HPC[🚀 HPC Optimized]
    
    GP --> GPUse[Web servers<br/>Small databases<br/>Code repositories]
    CO --> COUse[Batch processing<br/>Gaming servers<br/>ML inference]
    MO --> MOUse[In-memory databases<br/>Real-time analytics<br/>Caching]
    AC --> ACUse[Machine learning<br/>Graphics rendering<br/>Scientific computing]
    SO --> SOUse[Data warehousing<br/>OLTP systems<br/>Distributed file systems]
    HPC --> HPCUse[Scientific modeling<br/>High-performance computing<br/>Research]
    
    style IT fill:#FF9900,color:#000
    style GP fill:#4DABF7
    style CO fill:#FF6B6B
    style MO fill:#FFD700,color:#000
    style AC fill:#FFA500
    style SO fill:#90EE90
    style HPC fill:#DDA0DD
```

### Instance Type Details

| Type | Best For | Key Characteristic | Example Series |
|------|----------|-------------------|----------------|
| **General Purpose** | Diverse workloads | Balance of compute, memory, networking | T3, T4g, M5, M6i |
| **Compute Optimized** | CPU-intensive tasks | High-performance processors | C5, C6i, C7g |
| **Memory Optimized** | Large datasets in memory | High memory-to-CPU ratio | R5, R6i, X2idn |
| **Accelerated Computing** | GPU workloads | Hardware accelerators (GPU/FPGA) | P4, G5, DL1 |
| **Storage Optimized** | Storage-intensive tasks | High sequential I/O performance | I3, I4i, D2 |
| **HPC Optimized** | Maximum compute power | Highest performance, lowest latency | Hpc6id, Hpc7g |

---

## EC2 Naming Convention

**Format:** `<class><generation>.<size>`

**Example:** `m5.2xlarge`

```mermaid
graph LR
    Name[m5.2xlarge] --> Class[m<br/>Instance Class]
    Name --> Gen[5<br/>Generation]
    Name --> Size[2xlarge<br/>Size]
    
    Class --> C1[m = General Purpose]
    Gen --> G1[AWS improves<br/>over time]
    Size --> S1[More resources<br/>than smaller sizes]
    
    style Name fill:#FF9900,color:#000
    style Class fill:#4DABF7
    style Gen fill:#FF6B6B
    style Size fill:#90EE90
```

### Naming Breakdown

| Component | Example | Meaning |
|-----------|---------|---------|
| **Instance Class** | `m` | Type (General Purpose, Compute, Memory, etc.) |
| **Generation** | `5` | Version - newer generations are more powerful and cost-effective |
| **Size** | `2xlarge` | Resources within the class (nano, micro, small, medium, large, xlarge, 2xlarge, etc.) |

### Size Scale Example (General Purpose - m5 family)

| Size | vCPUs | RAM (GB) | Use Case |
|------|-------|----------|----------|
| `nano` | 2 | 0.5 | Minimal workloads |
| `micro` | 2 | 1 | Low-traffic sites |
| `small` | 2 | 2 | Small applications |
| `medium` | 2 | 4 | Web servers |
| `large` | 2 | 8 | Medium applications |
| `xlarge` | 4 | 16 | Large applications |
| `2xlarge` | 8 | 32 | High-performance apps |
| `4xlarge` | 16 | 64 | Enterprise workloads |

> 🎯 **Exam Tip:** Know that `m5.2xlarge` = General Purpose, 5th generation, 2xlarge size. The first letter indicates the family, the number is the generation.

---

## Right Sizing Strategy

**Definition:** Matching instance types and sizes to workload requirements for optimal performance and cost.

```mermaid
graph TD
    RS[📏 Right Sizing] --> Def[Definition]
    RS --> Key[Key Principle]
    RS --> Strategy[Strategy]
    
    Def --> D1[Match instance to<br/>actual workload needs]
    
    Key --> K1[Most powerful ≠ best<br/>due to cloud elasticity]
    
    Strategy --> S1[Start small]
    Strategy --> S2[Monitor usage]
    Strategy --> S3[Scale up if needed]
    Strategy --> S4[Review regularly]
    
    S1 --> S1D[t3.micro<br/>1 vCPU, 1GB]
    S2 --> S2D[Track CPU, memory,<br/>network metrics]
    S3 --> S3D[Upgrade to<br/>larger size]
    S4 --> S4D[Identify cost<br/>optimization]
    
    style RS fill:#51CF66,color:#fff
```

### Right Sizing Process

```mermaid
flowchart LR
    Start[Start Small<br/>t3.micro] --> Monitor[Monitor<br/>CloudWatch]
    Monitor --> Analyze[Analyze<br/>Performance]
    Analyze --> Adjust{Underutilized<br/>or Overloaded?}
    Adjust -->|Overloaded| ScaleUp[Scale Up<br/>Larger instance]
    Adjust -->|Underutilized| ScaleDown[Scale Down<br/>Smaller instance]
    ScaleUp --> Monitor
    ScaleDown --> Monitor
    
    style Start fill:#FFD700,color:#000
    style Monitor fill:#4DABF7
    style Analyze fill:#FFA500
    style ScaleUp fill:#FF6B6B,color:#fff
    style ScaleDown fill:#51CF66,color:#fff
```

### Right Sizing Tools

| Tool | Purpose |
|------|---------|
| **CloudWatch** | Monitor performance metrics (CPU, memory, network) |
| **Cost Explorer** | Analyze spending patterns and identify savings |
| **Trusted Advisor** | AWS optimization recommendations |
| **AWS Compute Optimizer** | ML-powered right-sizing recommendations |

---

## User Data Scripts

**Definition:** Scripts that run **once** during the first boot of an EC2 instance, used for bootstrapping and automation.

```mermaid
graph TD
    Launch[🚀 Launch EC2 Instance] --> UD[📜 User Data Script]
    UD --> Execute[▶️ Executes Once<br/>During Initial Startup]
    Execute --> Bootstrap[🔧 Bootstrapping]
    
    Bootstrap --> T1[📦 Install Updates]
    Bootstrap --> T2[💿 Install Software]
    Bootstrap --> T3[📥 Download Files]
    Bootstrap --> T4[⚙️ Custom Configuration]
    
    style Launch fill:#FF9900,color:#000
    style UD fill:#4DABF7,color:#fff
    style Bootstrap fill:#51CF66,color:#fff
```

### Common Use Cases

| Task | Example |
|------|---------|
| **Install Updates** | `yum update -y` |
| **Install Web Server** | `yum install -y httpd` |
| **Download Files** | `aws s3 cp s3://bucket/file .` |
| **Start Services** | `systemctl start httpd` |
| **Configure App** | Custom configuration scripts |

### Example: Web Server Setup

```bash
#!/bin/bash
# User Data Script Example
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from EC2!</h1>" > /var/www/html/index.html
```

> 🎯 **Exam Tip:** User Data scripts run **only once** at first launch. To re-run, you need to create a new instance or update the AMI.

---

## EC2 Purchasing Options (7 Options)

```mermaid
graph TD
    PO[💰 EC2 Purchasing Options] --> OD[⏱️ On-Demand]
    PO --> RI[📅 Reserved Instances]
    PO --> SP[📊 Savings Plans]
    PO --> Spot[💸 Spot Instances]
    PO --> DH[🏢 Dedicated Hosts]
    PO --> DI[🔒 Dedicated Instances]
    PO --> CR[📍 Capacity Reservations]
    
    OD --> OD1[Pay by second/hour<br/>No commitment]
    RI --> RI1[Up to 72% savings<br/>1-3 year commitment]
    SP --> SP1[Up to 72% savings<br/>Flexible across services]
    Spot --> Spot1[Up to 90% savings<br/>Interruptible]
    DH --> DH1[Physical server<br/>Compliance, licensing]
    DI --> DI1[Dedicated hardware<br/>Shared account]
    CR --> CR1[Guaranteed capacity<br/>Specific AZ]
    
    style PO fill:#FF9900,color:#000
```

### Detailed Comparison

| Option | Discount | Commitment | Interruptible | Best For |
|--------|----------|------------|----------------|----------|
| **On-Demand** | 0% | None | No | Unpredictable workloads, dev/test |
| **Reserved Instances** | Up to 72% | 1-3 years | No | Steady-state workloads |
| **Savings Plans** | Up to 72% | 1-3 years | No | Flexible usage across EC2, Lambda, Fargate |
| **Spot Instances** | Up to 90% | None | Yes (2-min notice) | Fault-tolerant, flexible workloads |
| **Dedicated Hosts** | None | On-demand/reserved | No | Compliance, BYOL (bring your own license) |
| **Dedicated Instances** | None | Per instance | No | Hardware isolation |
| **Capacity Reservations** | Variable | None/Specific | No | Guaranteed capacity in specific AZ |

### Purchasing Options Decision Tree

```mermaid
flowchart TD
    Start{Need to choose<br/>purchasing option?} --> Q1{Workload<br/>predictable?}
    
    Q1 -->|Yes| Q2{Long-term<br/>commitment OK?}
    Q1 -->|No| Q3{Fault-tolerant?}
    
    Q2 -->|Yes| RI[📅 Reserved Instances<br/>72% savings]
    Q2 -->|No| SP[📊 Savings Plans<br/>72% savings, flexible]
    
    Q3 -->|Yes| Spot[💸 Spot Instances<br/>90% savings]
    Q3 -->|No| OD[⏱️ On-Demand<br/>Full price, no commitment]
    
    Start --> Q4{Compliance/<br/>licensing needed?}
    Q4 -->|Yes| DH[🏢 Dedicated Hosts<br/>Full control]
    
    Start --> Q5{Need guaranteed<br/>capacity?}
    Q5 -->|Yes| CR[📍 Capacity Reservations]
    
    style RI fill:#51CF66,color:#fff
    style SP fill:#51CF66,color:#fff
    style Spot fill:#FFD700,color:#000
    style OD fill:#FF6B6B,color:#fff
    style DH fill:#DDA0DD
    style CR fill:#87CEEB
```

### Reserved Instance Attributes

When you reserve an instance, you specify:
- **Instance Type** (e.g., m5.large)
- **Region** (e.g., us-east-1)
- **Tenancy** (shared or dedicated)
- **OS** (Linux, Windows)

> 🎯 **Exam Tip:** Spot Instances can be interrupted with **2-minute notice**. Use them for fault-tolerant workloads only.

### Combined Approach Example

```mermaid
graph TD
    App[📱 Application Workload] --> Baseline[📊 Baseline: Reserved<br/>60% capacity, 72% off]
    App --> Burst[⚡ Burst: Spot<br/>30% capacity, 90% off]
    App --> Dev[🛠️ Dev/Test: On-Demand<br/>10% capacity, full price]
    
    Baseline --> B1[Predictable load]
    Burst --> Bu1[Flexible additional capacity]
    Dev --> D1[Temporary environments]
    
    style Baseline fill:#51CF66,color:#fff
    style Burst fill:#FFD700,color:#000
    style Dev fill:#FF6B6B,color:#fff
```

---

## EC2 Instance Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Pending: Launch
    Pending --> Running: Instance starts
    Running --> Stopping: Stop command
    Stopping --> Stopped: Shutdown complete
    Stopped --> Running: Start command
    Running --> Rebooting: Reboot command
    Rebooting --> Running: Reboot complete
    Running --> Terminating: Terminate command
    Stopped --> Terminating: Terminate command
    Terminating --> Terminated: Deletion complete
    Terminated --> [*]
    
    note right of Running: Billing active
    note right of Stopped: No compute billing<br/>(EBS charges apply)
    note left of Terminated: Permanent deletion<br/>Cannot recover
```

### Lifecycle States

| State | Description | Billing |
|-------|-------------|---------|
| **Pending** | Instance is being launched | No |
| **Running** | Instance is active and operational | Yes |
| **Stopping** | Instance is shutting down | Yes |
| **Stopped** | Instance is shut down | No compute (EBS charges apply) |
| **Rebooting** | Instance is restarting | Yes |
| **Terminating** | Instance is being deleted | Yes |
| **Terminated** | Instance is permanently deleted | No |

---

## EC2 Related Services

```mermaid
graph LR
    EC2[🖥️ EC2] --> EBS[💾 EBS<br/>Block Storage]
    EC2 --> ELB[⚖️ ELB<br/>Load Balancing]
    EC2 --> ASG[📈 ASG<br/>Auto Scaling]
    EC2 --> SG[🔒 Security Groups<br/>Network Firewall]
    EC2 --> AMI[📦 AMI<br/>Templates]
    EC2 --> ENI[🌐 ENI<br/>Networking]
    EC2 --> CW[📊 CloudWatch<br/>Monitoring]
    
    EBS --> Block[Persistent<br/>block storage]
    ELB --> Dist[Distribute<br/>traffic]
    ASG --> Dyn[Dynamic<br/>scaling]
    SG --> FW[Firewall<br/>rules]
    AMI --> Temp[Pre-configured<br/>images]
    ENI --> VNet[Virtual<br/>network interface]
    CW --> Mon[Metrics<br/>and logs]
    
    style EC2 fill:#FF9900,color:#000
```

---

## EC2 vs Traditional Servers

```mermaid
graph LR
    subgraph Traditional["🏢 Traditional Servers"]
        T1[Physical Hardware]
        T2[Weeks to procure]
        T3[Fixed capacity]
        T4[Manual scaling]
        T5[High upfront cost]
        T6[Your maintenance]
    end
    
    subgraph EC2["☁️ Amazon EC2"]
        E1[Virtual Hardware]
        E2[Minutes to launch]
        E3[Elastic capacity]
        E4[Auto scaling]
        E5[Pay-as-you-go]
        E6[AWS managed]
    end
    
    Traditional -.->|Cloud Migration| EC2
    
    style Traditional fill:#FFE6E6
    style EC2 fill:#E6F7E6
```

### Comparison Table

| Aspect | Traditional Servers | Amazon EC2 |
|--------|--------------------|-----------|
| **Procurement** | Weeks to months | Minutes |
| **Capacity** | Fixed | Elastic |
| **Scaling** | Manual, time-consuming | Automated, instant |
| **Cost Model** | High upfront (CapEx) | Pay-as-you-go (OpEx) |
| **Maintenance** | Customer managed | AWS managed infrastructure |
| **Geographic Reach** | Limited | Global (36+ Regions) |
| **Availability** | Single location | Multi-AZ, Multi-Region |

---

## Instance Selection Decision Flow

```mermaid
flowchart TD
    Start[Need to choose<br/>instance type?] --> Q1{Primary workload?}
    
    Q1 -->|Web/App servers| GP[⚖️ General Purpose]
    Q1 -->|Batch/ML/HPC| CO[🔥 Compute Optimized]
    Q1 -->|In-memory DB/Cache| MO[💾 Memory Optimized]
    Q1 -->|ML Training/GPU| AC[⚡ Accelerated Computing]
    Q1 -->|OLTP/Data Warehouse| SO[📦 Storage Optimized]
    Q1 -->|Scientific Compute| HPC[🚀 HPC Optimized]
    
    GP --> Size{Choose Size}
    CO --> Size
    MO --> Size
    AC --> Size
    SO --> Size
    HPC --> Size
    
    Size --> Small[Start Small<br/>t3.micro]
    Small --> Monitor[Monitor<br/>Performance]
    Monitor --> Adjust[Scale as<br/>Needed]
    
    style GP fill:#4DABF7
    style CO fill:#FF6B6B
    style MO fill:#FFD700,color:#000
    style AC fill:#FFA500
    style SO fill:#90EE90
    style HPC fill:#DDA0DD
```

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **EC2** | Elastic Compute Cloud - virtual servers (IaaS) |
| **Instance Types** | 6 categories: General, Compute, Memory, Accelerated, Storage, HPC |
| **Naming** | `<class><generation>.<size>` (e.g., m5.2xlarge) |
| **Right Sizing** | Start small, monitor, scale as needed |
| **User Data** | Scripts run once at first launch |
| **Purchasing** | 7 options: On-Demand, Reserved, Savings Plans, Spot, Dedicated Hosts/Instances, Capacity Reservations |
| **Lifecycle** | Pending → Running → Stopped/Terminated |
| **Related Services** | EBS, ELB, ASG, Security Groups, AMI |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: What does the "m" in "m5.2xlarge" indicate?</strong></summary>

**A.** Memory optimized  
**B.** General purpose instance class  
**C.** Multi-threaded processor  
**D.** Medium pricing tier  

**Answer: B** — The "m" indicates the instance class. In AWS naming convention, "m" stands for General Purpose instances. The "5" is the generation, and "2xlarge" is the size.
</details>

<details>
<summary><strong>Q2: Which EC2 purchasing option offers up to 90% savings but can be interrupted with 2-minute notice?</strong></summary>

**A.** On-Demand Instances  
**B.** Reserved Instances  
**C.** Spot Instances  
**D.** Dedicated Hosts  

**Answer: C** — Spot Instances offer up to 90% savings compared to On-Demand pricing, but AWS can interrupt them with a 2-minute notice when it needs the capacity back. Use them for fault-tolerant workloads.
</details>

<details>
<summary><strong>Q3: When does an EC2 User Data script execute?</strong></summary>

**A.** Every time the instance reboots  
**B.** Once during the initial launch  
**C.** Continuously in the background  
**D.** Only when manually triggered  

**Answer: B** — User Data scripts execute once during the initial launch of an EC2 instance. They're used for bootstrapping tasks like installing software, downloading files, or configuring the environment.
</details>

<details>
<summary><strong>Q4: Which instance type should you choose for an in-memory database?</strong></summary>

**A.** General Purpose  
**B.** Compute Optimized  
**C.** Memory Optimized  
**D.** Storage Optimized  

**Answer: C** — Memory Optimized instances have a high memory-to-CPU ratio, making them ideal for in-memory databases, real-time analytics, and caching workloads.
</details>

---

## Navigation

⬅️ Previous: [AWS Data Centers](../02-aws-infrastructure/03-data-centers.md) | ➡️ Next: [Storage Services](./02-storage.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*