# Scalability & High Availability

> ⏱️ **Estimated Study Time:** 20 minutes  
> 🎯 **CCP Exam Weight:** ~12-15% (Domain 3: Cloud Technology & Services)

---

## The Big Picture

**Scalability** and **High Availability (HA)** are two fundamental architectural concepts that ensure your application can handle increasing load and remain operational despite failures. AWS provides Auto Scaling Groups and Load Balancers to achieve both. These concepts are heavily tested on the CCP exam.

---

## Core Concepts

```mermaid
graph TD
    Concept[🎯 Core Architecture Concepts] --> Scale[📈 Scalability<br/>Handle increasing load]
    Concept --> HA[🛡️ High Availability<br/>Survive failures]
    Concept --> Elastic[⚡ Elasticity<br/>Auto-scaling]
    Concept --> Agility[🏃 Agility<br/>Quick provisioning]
    
    Scale --> S1[Vertical<br/>Horizontal]
    HA --> H1[Multi-AZ<br/>Redundancy]
    Elastic --> E1[Pay-per-use<br/>Auto-adjust]
    Agility --> A1[Weeks → minutes]
    
    style Concept fill:#FF9900,color:#000
    style Scale fill:#4DABF7
    style HA fill:#FF6B6B,color:#fff
    style Elastic fill:#51CF66,color:#fff
    style Agility fill:#FFD700,color:#000
```

### Key Definitions

| Concept | Definition |
|---------|------------|
| **Scalability** | Ability to handle **increasing loads** by adapting (scale up or out) |
| **High Availability** | Ability to **remain operational** despite failures (run in multiple AZs) |
| **Elasticity** | **Automatic scaling** based on demand in a scalable system |
| **Agility** | Ability to **quickly provision** new IT resources (weeks → minutes) |

> 🎯 **Exam Tip:** Scalability is about **handling more load**. High Availability is about **surviving failures**. They are related but distinct concepts.

---

## Two Types of Scalability

```mermaid
graph LR
    Scale[📈 Scalability] --> V[📏 Vertical Scalability<br/>Scale Up/Down]
    Scale --> H[↔️ Horizontal Scalability<br/>Scale Out/In]
    
    V --> VEx[Increase instance size<br/>t2.micro → t2.large]
    H --> HEx[Add more instances<br/>1 → 100 instances]
    
    V --> VUse[Non-distributed systems<br/>e.g., databases]
    H --> HUse[Distributed systems<br/>e.g., web servers]
    
    style V fill:#FF6B6B,color:#fff
    style H fill:#4DABF7,color:#fff
```

### Vertical vs Horizontal Comparison

| Aspect | Vertical Scalability | Horizontal Scalability |
|--------|---------------------|------------------------|
| **Action** | Increase size of instance | Increase number of instances |
| **Also Known As** | Scale Up / Scale Down | Scale Out / Scale In |
| **Example** | t2.nano → u-12tb1.metal | 1 instance → 10 instances |
| **Use Case** | Non-distributed systems (databases) | Distributed systems (web servers, microservices) |
| **Limit** | Hardware maximum | Practically unlimited |
| **Downtime** | May require restart | No downtime |

### Vertical Scaling Example

```mermaid
graph LR
    Before[t2.nano<br/>0.5 GB RAM<br/>1 vCPU] -->|Scale Up| After[u-12tb1.metal<br/>12.3 TB RAM<br/>448 vCPUs]
    
    style Before fill:#FFD700,color:#000
    style After fill:#51CF66,color:#fff
```

### Horizontal Scaling Example

```mermaid
graph TD
    LB[⚖️ Load Balancer] --> I1[Instance 1]
    LB --> I2[Instance 2]
    LB --> I3[Instance 3]
    LB --> I4[Instance 4]
    LB --> I5[Instance N...]
    
    ASG[📈 Auto Scaling Group] -.->|Manages| I1
    ASG -.->|Manages| I2
    ASG -.->|Manages| I3
    ASG -.->|Manages| I4
    ASG -.->|Manages| I5
    
    style LB fill:#FF9900,color:#000
    style ASG fill:#51CF66,color:#fff
```

---

## Scalability vs High Availability

```mermaid
graph LR
    S[📈 Scalability] --> SDef[Handle increasing load<br/>Can it handle more users?]
    HA[🛡️ High Availability] --> HADef[Survive failures<br/>Will it stay up if something fails?]
    
    S --> Related[They are related<br/>but distinct]
    HA --> Related
    
    style S fill:#4DABF7,color:#fff
    style HA fill:#FF6B6B,color:#fff
```

### Key Differences

| Aspect | Scalability | High Availability |
|--------|-------------|-------------------|
| **Goal** | Handle increased load | Survive failures |
| **Question** | "Can it handle more users?" | "Will it stay up if AZ fails?" |
| **Implementation** | Scale up/out | Multi-AZ deployment |
| **Focus** | Capacity | Reliability |

---

## High Availability (HA)

**Definition:** Systems remain **operational** despite failures by running across **multiple Availability Zones**.

```mermaid
graph TD
    HA[🛡️ High Availability Goal] --> Goal[Survive Data Center Loss]
    
    Goal --> G1[Run in at least 2 AZs]
    Goal --> G2[Auto Scaling Groups across AZs]
    Goal --> G3[Load Balancers across AZs]
    Goal --> G4[Goal: 99.99% uptime]
    
    style HA fill:#FF9900,color:#000
    style Goal fill:#FF6B6B,color:#fff
```

### HA Architecture Example

```mermaid
graph TD
    User[👥 Users] --> ELB[⚖️ Load Balancer]
    
    subgraph Region["📍 AWS Region"]
        ELB --> AZ1[🏢 AZ 1]
        ELB --> AZ2[🏢 AZ 2]
        ELB --> AZ3[🏢 AZ 3]
        
        AZ1 --> I1A[Instance 1]
        AZ1 --> I2A[Instance 2]
        
        AZ2 --> I1B[Instance 3]
        AZ2 --> I2B[Instance 4]
        
        AZ3 --> I1C[Instance 5]
        AZ3 --> I2C[Instance 6]
    end
    
    ASG[📈 Auto Scaling Group] -.->|Manages all| AZ1
    ASG -.->|Manages all| AZ2
    ASG -.->|Manages all| AZ3
    
    style ELB fill:#FF9900,color:#000
    style AZ1 fill:#87CEEB
    style AZ2 fill:#90EE90
    style AZ3 fill:#FFB6C1
```

### AZ Failure Scenario

```mermaid
graph LR
    AZ1[🏢 AZ 1<br/>❌ FAILED]
    AZ2[🏢 AZ 2<br/>✅ HEALTHY]
    AZ3[🏢 AZ 3<br/>✅ HEALTHY]
    
    AZ1 -.->|Failure detected| ELB[⚖️ Load Balancer]
    ELB -->|Route traffic to healthy| AZ2
    ELB -->|Route traffic to healthy| AZ3
    
    style AZ1 fill:#FF6B6B,color:#fff
    style AZ2 fill:#51CF66,color:#fff
    style AZ3 fill:#51CF66,color:#fff
    style ELB fill:#FFD700,color:#000
```

> 🎯 **Exam Tip:** High Availability requires deploying across **at least 2 Availability Zones**. Single AZ = single point of failure.

---

## Load Balancers

**Definition:** Servers that **distribute incoming traffic** across multiple downstream EC2 instances to ensure availability and scalability.

```mermaid
graph TD
    LB[⚖️ Load Balancer] --> Functions[Functions]
    Functions --> F1[Distribute traffic]
    Functions --> F2[Single point of access]
    Functions --> F3[Handle failures]
    Functions --> F4[Health checks]
    Functions --> F5[SSL termination]
    Functions --> F6[High availability]
    
    style LB fill:#FF9900,color:#000
```

### Three Types of AWS Load Balancers

```mermaid
graph TD
    LB[⚖️ AWS Load Balancers] --> ALB[🌐 Application Load Balancer<br/>Layer 7]
    LB --> NLB[🚀 Network Load Balancer<br/>Layer 4]
    LB --> GWLB[🔀 Gateway Load Balancer<br/>Layer 3]
    
    ALB --> ALBUse[HTTP/HTTPS<br/>Content-based routing<br/>Path/Host routing]
    NLB --> NLBUse[TCP/UDP<br/>Ultra-high performance<br/>Millions of req/sec]
    GWLB --> GWLBUse[Layer 3<br/>Third-party appliances<br/>Network firewalls]
    
    style ALB fill:#4DABF7
    style NLB fill:#FF6B6B,color:#fff
    style GWLB fill:#FFD700,color:#000
```

### Load Balancer Comparison

| Feature | ALB | NLB | GWLB |
|---------|-----|-----|------|
| **Layer** | 7 (Application) | 4 (Transport) | 3 (Network) |
| **Protocol** | HTTP/HTTPS | TCP, UDP, TLS | IP |
| **Use Case** | Web apps, microservices, containers | Gaming, IoT, real-time | Network appliances, firewalls |
| **Performance** | High | Very high (millions of req/sec) | High |
| **Static IP** | No | Yes | Yes |
| **Content Routing** | Yes (path, host, headers) | No | No |
| **Pricing** | Per hour + LCU | Per hour + NLCU | Per hour + GLCU |

### Load Balancer Selection Guide

```mermaid
flowchart TD
    Start{Need Load Balancer?} --> Q1{Application Type?}
    
    Q1 -->|HTTP/HTTPS<br/>Web App| ALB[🌐 Use ALB<br/>Layer 7 features]
    Q1 -->|TCP/UDP<br/>Extreme Performance| NLB[🚀 Use NLB<br/>Ultra-low latency]
    Q1 -->|Layer 3<br/>Third-party appliances| GWLB[🔀 Use GWLB<br/>Network appliances]
    
    style ALB fill:#4DABF7,color:#fff
    style NLB fill:#FF6B6B,color:#fff
    style GWLB fill:#FFD700,color:#000
```

> 🎯 **Exam Tip:** Choose **ALB for web applications** (HTTP/HTTPS). Choose **NLB for extreme performance** (gaming, IoT). Choose **GWLB for third-party network appliances**.

---

## Auto Scaling Groups (ASG)

**Definition:** Group of EC2 instances managed together to **automatically adjust capacity** based on demand, maintaining performance and optimizing costs.

```mermaid
graph TD
    ASG[📈 Auto Scaling Group] --> Goals[Goals]
    Goals --> G1[Scale out<br/>Add instances]
    Goals --> G2[Scale in<br/>Remove instances]
    Goals --> G3[Maintain min/max]
    Goals --> G4[Register with LB]
    Goals --> G5[Replace unhealthy]
    
    style ASG fill:#FF9900,color:#000
```

### ASG Key Capabilities

| Capability | Description |
|------------|-------------|
| **Scale Out** | Add EC2 instances to handle increased load |
| **Scale In** | Remove EC2 instances to reduce costs |
| **Maintain Capacity** | Keep instances between min and max |
| **Auto Register** | Register new instances with Load Balancer |
| **Health Replacement** | Automatically replace unhealthy instances |

### ASG Configuration

```mermaid
graph TD
    ASG[📈 ASG Configuration] --> Min[Minimum Capacity<br/>e.g., 2 instances]
    ASG --> Max[Maximum Capacity<br/>e.g., 10 instances]
    ASG --> Desired[Desired Capacity<br/>e.g., 4 instances]
    ASG --> Health[Health Checks<br/>EC2 + ELB]
    ASG --> Launch[Launch Template<br/>Instance configuration]
    ASG --> Scaling[Scaling Policies<br/>When to scale]
    
    style ASG fill:#FF9900,color:#000
```

### ASG Capacity Settings

| Setting | Description | Example |
|---------|-------------|---------|
| **Minimum** | Lowest number of instances always running | 2 |
| **Maximum** | Highest number of instances allowed | 10 |
| **Desired** | Target number at any time | 4 |

---

## Auto Scaling Strategies

```mermaid
graph TD
    Strategies[📈 ASG Scaling Strategies] --> Manual[👤 Manual Scaling]
    Strategies --> Dynamic[⚡ Dynamic Scaling]
    
    Dynamic --> Simple[📊 Simple/Step Scaling]
    Dynamic --> Target[🎯 Target Tracking Scaling]
    Dynamic --> Scheduled[📅 Scheduled Scaling]
    Dynamic --> Predictive[🤖 Predictive Scaling]
    
    style Manual fill:#87CEEB
    style Simple fill:#FFD700,color:#000
    style Target fill:#51CF66,color:#fff
    style Scheduled fill:#DDA0DD
    style Predictive fill:#FF6B6B,color:#fff
```

### Strategy Comparison

| Strategy | How It Works | Trigger | Best For |
|----------|-------------|---------|----------|
| **Manual** | Adjust ASG size manually | Human action | One-time events, predictable changes |
| **Simple/Step** | CloudWatch alarm triggers action | CPU > 70% → Add 2 instances | Simple thresholds |
| **Target Tracking** | Maintain target metric value | Keep CPU at 40% | Most common use case |
| **Scheduled** | Plan scaling based on time | Increase at 5 PM Friday | Known traffic patterns |
| **Predictive** | ML-based forecasting | Forecasted demand | Complex patterns |

### Target Tracking Scaling Example

```mermaid
graph TD
    TT[🎯 Target Tracking] --> Goal[Goal: Keep CPU at 40%]
    
    Goal --> Monitor[Monitor CPU usage]
    Monitor --> Q{CPU > 40%?}
    Q -->|Yes| Out[Scale OUT<br/>Add instances]
    Q -->|No| Q2{CPU < 40%?}
    Q2 -->|Yes| In[Scale IN<br/>Remove instances]
    Q2 -->|No| Monitor
    
    style TT fill:#51CF66,color:#fff
    style Out fill:#FF6B6B,color:#fff
    style In fill:#FFD700,color:#000
```

> 🎯 **Exam Tip:** Target Tracking is the **most common** scaling strategy. It maintains a target metric (like 40% CPU) by automatically scaling.

---

## Complete HA + Scalable Architecture

```mermaid
graph TD
    User[👥 Users] --> R53[🗺️ Route 53<br/>DNS]
    R53 --> ELB[⚖️ Load Balancer]
    
    subgraph Region["📍 AWS Region"]
        ELB --> ASG[📈 Auto Scaling Group]
        
        subgraph AZ1["🏢 AZ 1"]
            ASG --> I1[EC2 Instance 1]
        end
        
        subgraph AZ2["🏢 AZ 2"]
            ASG --> I2[EC2 Instance 2]
        end
        
        subgraph AZ3["🏢 AZ 3"]
            ASG --> I3[EC2 Instance 3]
        end
    end
    
    CW[📊 CloudWatch<br/>Metrics & Alarms] -.->|Triggers| ASG
    CW -.->|Triggers| ELB
    
    style User fill:#FFD700,color:#000
    style R53 fill:#FF9900,color:#000
    style ELB fill:#FF9900,color:#000
    style ASG fill:#51CF66,color:#fff
    style AZ1 fill:#87CEEB
    style AZ2 fill:#90EE90
    style AZ3 fill:#FFB6C1
```

### Health Check & Replacement Flow

```mermaid
graph TD
    ELB[⚖️ Load Balancer] --> Check[Health Check]
    Check --> H1[Instance 1<br/>✅ HEALTHY]
    Check --> H2[Instance 2<br/>❌ UNHEALTHY]
    Check --> H3[Instance 3<br/>✅ HEALTHY]
    
    H2 -.->|Remove from LB| ELB
    ASG[📈 Auto Scaling Group] -.->|Detect unhealthy| H2
    ASG -.->|Launch replacement| New[New Instance]
    New -.->|Register with LB| ELB
    
    style H1 fill:#51CF66,color:#fff
    style H2 fill:#FF6B6B,color:#fff
    style H3 fill:#51CF66,color:#fff
    style New fill:#51CF66,color:#fff
```

---

## Scalability vs Elasticity vs Agility

```mermaid
graph TD
    Concept[🎯 Three Related Concepts] --> S[📈 Scalability]
    Concept --> E[⚡ Elasticity]
    Concept --> A[🏃 Agility]
    
    S --> SDef[Handle larger load<br/>Scale up or out]
    E --> EDef[Automatic scaling<br/>Based on load<br/>Pay-per-use]
    A --> ADef[Quick provisioning<br/>Weeks → minutes]
    
    E --> Requires[Requires Scalability]
    A --> Enables[Enables Elasticity]
    
    style S fill:#4DABF7,color:#fff
    style E fill:#FF6B6B,color:#fff
    style A fill:#FFD700,color:#000
```

### When to Use Each Scaling Type

```mermaid
flowchart TD
    Start{Need to scale?} --> Q{Distributed<br/>system?}
    
    Q -->|No<br/>e.g., Database| V[📏 Use Vertical Scaling<br/>Scale up instance]
    Q -->|Yes<br/>e.g., Web Server| H[↔️ Use Horizontal Scaling<br/>Add more instances]
    
    V --> Limit{Hardware<br/>limit?}
    Limit -->|No| VGo[Continue scaling up]
    Limit -->|Yes| Consider[Consider migration<br/>to distributed]
    
    H --> Tools[Required Tools]
    Tools --> T1[Auto Scaling Groups]
    Tools --> T2[Load Balancers]
    
    style V fill:#FF6B6B,color:#fff
    style H fill:#4DABF7,color:#fff
```

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **Scalability** | Handle increasing load (vertical or horizontal) |
| **High Availability** | Survive failures (deploy across multiple AZs) |
| **Elasticity** | Automatic scaling based on demand |
| **Vertical Scaling** | Increase instance size (scale up/down) |
| **Horizontal Scaling** | Add more instances (scale out/in) |
| **Load Balancers** | Distribute traffic (ALB, NLB, GWLB) |
| **Auto Scaling Groups** | Automatically adjust capacity |
| **Target Tracking** | Most common scaling strategy |
| **Multi-AZ** | Required for high availability |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: What is the difference between scalability and high availability?</strong></summary>

**A.** They are the same thing  
**B.** Scalability handles increasing load; HA survives failures  
**C.** Scalability is for databases; HA is for web servers  
**D.** Scalability is cheaper than HA  

**Answer: B** — Scalability is the ability to handle increasing loads by adapting (scaling up or out). High Availability is the ability to remain operational despite failures (running in multiple AZs). They are related but distinct concepts.
</details>

<details>
<summary><strong>Q2: Which load balancer should you use for an HTTP/HTTPS web application with content-based routing?</strong></summary>

**A.** Network Load Balancer  
**B.** Gateway Load Balancer  
**C.** Application Load Balancer  
**D.** Classic Load Balancer  

**Answer: C** — Application Load Balancer (ALB) is a Layer 7 load balancer that supports HTTP/HTTPS and provides content-based routing (path, host, headers). Use it for web applications and microservices.
</details>

<details>
<summary><strong>Q3: What is the most common Auto Scaling strategy that maintains a target metric value?</strong></summary>

**A.** Manual Scaling  
**B.** Simple/Step Scaling  
**C.** Target Tracking Scaling  
**D.** Predictive Scaling  

**Answer: C** — Target Tracking Scaling is the most common strategy. It maintains a target metric value (like 40% CPU) by automatically scaling out or in based on the metric. For example, if CPU exceeds 40%, ASG adds instances; if below 40%, it removes them.
</details>

<details>
<summary><strong>Q4: How many Availability Zones should you deploy across to achieve high availability?</strong></summary>

**A.** 1 AZ  
**B.** At least 2 AZs  
**C.** At least 3 AZs  
**D.** All AZs in the Region  

**Answer: B** — High Availability requires deploying across **at least 2 Availability Zones**. This protects against data center failures within a Region. Best practice is to use 3 AZs for maximum resilience.
</details>

---

## Navigation

⬅️ Previous: [Databases](./04-databases.md) | ➡️ Next: [Virtualization Fundamentals](./06-virtualization-fundamentals.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*