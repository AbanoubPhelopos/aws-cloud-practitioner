# Scalability & High Availability

## The Big Picture

**Scalability** is the ability of an application or system to handle increasing loads by adapting. This module covers scalability types, the relationship with High Availability, and how to implement them in EC2.

---

## Scalability Overview

```mermaid
graph TD
    Scal[Scalability] --> Def[Ability to handle<br/>increasing loads by adapting]
    
    Def --> Types[Two Types]
    Types --> V[Vertical Scalability]
    Types --> H[Horizontal Scalability<br/>Elasticity]
    
    Scal --> Compare[Related Concepts]
    Compare --> HA[High Availability]
```

### Key Definitions

| Concept | Definition |
|---------|-----------|
| **Scalability** | Ability to accommodate larger loads by enhancing hardware (scale up) or adding nodes (scale out) |
| **Elasticity** | Automatic scaling based on load in a scalable system (pay-per-use) |
| **Agility** | Quickly provision new IT resources (weeks → minutes) |

---

## Two Types of Scalability

```mermaid
graph LR
    S[Scalability] --> V[Vertical Scalability<br/>Scale Up/Down]
    S --> H[Horizontal Scalability<br/>Scale Out/In<br/>Elasticity]
    
    V --> VEx[Increase instance size<br/>t2.micro → t2.large]
    H --> HEx[Add more instances<br/>Multiple smaller instances]
```

### Comparison

| Aspect | Vertical Scalability | Horizontal Scalability |
|--------|---------------------|------------------------|
| **Action** | Increase size of instance | Increase number of instances |
| **Example** | t2.micro → t2.large | 1 instance → 10 instances |
| **Also Known As** | Scale Up/Down | Scale Out/In, Elasticity |
| **Use Case** | Non-distributed systems (databases) | Distributed systems (web servers) |
| **Limit** | Hardware limitations | Practically unlimited |

---

## Call Center Analogy

```mermaid
graph TD
    Call[Call Center<br/>Handling Calls] --> V[Vertical Scalability]
    Call --> H[Horizontal Scalability]
    
    V --> VEx[Add more resources<br/>Upgrade hardware<br/>Better phone system<br/>More powerful servers]
    H --> HEx[Add more agents<br/>Distribute calls<br/>across multiple people]
```

| Type | Analogy |
|------|---------|
| **Vertical Scalability** | Upgrade server hardware to handle increased call volume |
| **Horizontal Scalability** | Add more call center agents to distribute workload |

---

## Vertical Scalability

```mermaid
graph LR
    V[Vertical Scalability] --> Before[Smaller Instance]
    V --> After[Larger Instance]
    
    Before[Before: t2.nano<br/>0.5 GB RAM<br/>1 vCPU] --> After
    
    After[After: u-12tb1.metal<br/>12.3 TB RAM<br/>448 vCPUs]
```

### Key Characteristics

| Aspect | Description |
|--------|-------------|
| **Action** | Increase the size of the instance |
| **Use Case** | Non-distributed systems (e.g., databases) |
| **Limit** | Hardware limitations (maximum instance size) |

### Example Scale Range

```mermaid
graph LR
    A[t2.nano<br/>0.5GB RAM<br/>1 vCPU] --> B[t2.micro<br/>1GB RAM<br/>1 vCPU]
    B --> C[t2.large<br/>8GB RAM<br/>2 vCPU]
    C --> D[u-12tb1.metal<br/>12.3TB RAM<br/>448 vCPU]
```

---

## Horizontal Scalability

```mermaid
graph TD
    H[Horizontal Scalability] --> Action[Add More Instances]
    Action --> A1[Instance 1]
    Action --> A2[Instance 2]
    Action --> A3[Instance 3]
    Action --> AN[Instance N...]
    
    H --> Tools[Tools Required]
    Tools --> T1[Auto Scaling Groups]
    Tools --> T2[Load Balancers]
```

### Key Characteristics

| Aspect | Description |
|--------|-------------|
| **Action** | Increase number of instances |
| **Tools** | Auto Scaling Groups + Load Balancers |
| **Use Case** | Distributed systems (web servers, microservices) |
| **Limit** | Practically unlimited |

---

## High Availability (HA)

```mermaid
graph TD
    HA[High Availability] --> Def[Definition]
    Def --> D1[Systems remain operational<br/>despite failures]
    Def --> D2[Minimize downtime]
    Def --> D3[Continuous service availability]
    
    HA --> Implement[Implementation]
    Implement --> I1[Run in at least 2 AZs]
    Implement --> I2[Auto Scaling Groups across AZs]
    Implement --> I3[Load Balancers across AZs]
    Implement --> I4[Goal: survive data center loss]
```

### High Availability Goals

| Goal | Description |
|------|-------------|
| **Survive Data Center Loss** | Continue operating despite AZ failure |
| **Minimize Downtime** | Reduce service interruptions |
| **Multi-AZ Deployment** | Run across at least 2 Availability Zones |
| **Continuous Availability** | Service always accessible |

---

## Scalability vs High Availability

```mermaid
graph LR
    S[Scalability] --> SDef[Handle increasing load]
    HA[High Availability] --> HADef[Survive failures]
    
    S --> Related[They are related<br/>but distinct]
    HA --> Related
```

### Key Differences

| Aspect | Scalability | High Availability |
|--------|-------------|-------------------|
| **Goal** | Handle increased load | Survive failures |
| **Question** | "Can it handle more users?" | "Will it stay up if something fails?" |
| **Implementation** | Scale up/out | Multi-AZ, redundancy |
| **Focus** | Capacity | Reliability |

---

## High Availability & Scalability for EC2

```mermaid
graph TD
    ECHAS[EC2 High Availability & Scalability] --> VS[Vertical Scaling<br/>Scale Up/Down]
    ECHAS --> HS[Horizontal Scaling<br/>Scale Out/In]
    ECHAS --> HA[High Availability]
    
    VS --> VSEx[Increase instance size<br/>t2.nano → u-12tb1.metal]
    
    HS --> HSEx[Increase number of instances<br/>Use ASG + ELB]
    
    HA --> HAEx[Run across multiple AZs<br/>ASG + ELB across AZs]
```

### Vertical Scaling Details

```mermaid
graph LR
    Before[t2.nano<br/>0.5 GB RAM<br/>1 vCPU] -->|Scale Up| After[u-12tb1.metal<br/>12.3 TB RAM<br/>448 vCPUs]
    
    style Before fill:#ffcc00
    style After fill:#00cc66
```

### Horizontal Scaling Details

```mermaid
graph TD
    LB[Load Balancer] --> I1[Instance 1]
    LB --> I2[Instance 2]
    LB --> I3[Instance 3]
    LB --> I4[Instance 4]
    LB --> I5[Instance N...]
    
    ASG[Auto Scaling Group] -.->|Manages| I1
    ASG -.->|Manages| I2
    ASG -.->|Manages| I3
    ASG -.->|Manages| I4
    ASG -.->|Manages| I5
```

### High Availability Details

```mermaid
graph TD
    User[Users] --> LB[Load Balancer]
    
    subgraph AZ1["AZ 1"]
        LB --> I1A[Instance 1<br/>App Server]
        LB --> I2A[Instance 2<br/>App Server]
    end
    
    subgraph AZ2["AZ 2"]
        LB --> I1B[Instance 3<br/>App Server]
        LB --> I2B[Instance 4<br/>App Server]
    end
    
    subgraph AZ3["AZ 3"]
        LB --> I1C[Instance 5<br/>App Server]
        LB --> I2C[Instance 6<br/>App Server]
    end
    
    ASG[Auto Scaling Group] -.->|Manages all| AZ1
    ASG -.->|Manages all| AZ2
    ASG -.->|Manages all| AZ3
```

---

## Scalability vs Elasticity vs Agility

```mermaid
graph TD
    Compare[Three Concepts] --> S[Scalability]
    Compare --> E[Elasticity]
    Compare --> A[Agility]
    
    S --> SDef[Handle larger load<br/>Scale up or out]
    E --> EDef[Automatic scaling<br/>Based on load<br/>Pay-per-use]
    A --> ADef[Quick provisioning<br/>Weeks → minutes]
    
    E --> Requires[Requires Scalability]
    S --> Independent[Independent concept]
```

### Detailed Comparison

| Concept | Definition | Key Feature |
|---------|------------|-------------|
| **Scalability** | Ability to accommodate larger load | Scale up (hardware) or scale out (nodes) |
| **Elasticity** | Automatic scaling in scalable system | Pay-per-use, demand matching, cost optimization |
| **Agility** | Quickly provision new IT resources | Reduces time from weeks to minutes |

### Relationship Diagram

```mermaid
graph TD
    Agile[Agility<br/>Quick Provisioning] --> Elastic[Elasticity<br/>Auto-scaling]
    Elastic --> Scale[Scalability<br/>Handle Larger Load]
    
    Scale --> Up[Scale Up<br/>Vertical]
    Scale --> Out[Scale Out<br/>Horizontal]
```

### Practical Example

```mermaid
graph TD
    EC[Elasticity] --> Requires[Requires]
    Requires --> Scale[Scalable System]
    Scale --> Sc1[Scale Up capability]
    Scale --> Sc2[Scale Out capability]
    
    EC --> Benefits[Benefits]
    Benefits --> B1[Pay-per-use]
    Benefits --> B2[Demand matching]
    Benefits --> B3[Cost optimization]
```

---

## When to Use Each Scaling Type

### Vertical Scaling Decision

```mermaid
graph TD
    Q{Need to Scale?} --> Dist{Distributed System?}
    
    Dist -->|No<br/>e.g., Database| V[Use Vertical Scaling]
    Dist -->|Yes<br/>e.g., Web Server| H[Consider Horizontal Scaling]
    
    V --> Limit{Hardware limit<br/>not reached?}
    Limit -->|Yes| VGo[Scale up instance]
    Limit -->|No| HA[Consider migration to distributed]
```

### Horizontal Scaling Decision

```mermaid
graph TD
    Q{Need to Scale?} --> Dist{Distributed System?}
    
    Dist -->|Yes| H[Use Horizontal Scaling]
    Dist -->|No| V[Consider Vertical first]
    
    H --> Tools[Required Tools]
    Tools --> T1[Auto Scaling Groups]
    Tools --> T2[Load Balancers]
```

### Use Cases Comparison

| Scenario | Recommended Approach |
|----------|---------------------|
| **Traditional RDBMS database** | Vertical Scaling |
| **NoSQL database (DynamoDB)** | Horizontal Scaling |
| **Web server fleet** | Horizontal Scaling |
| **Single application instance** | Vertical Scaling |
| **Microservices** | Horizontal Scaling |

---

## AWS Tools for Scalability & HA

```mermaid
graph TD
    Tools[AWS Tools] --> ASG[Auto Scaling Groups]
    Tools --> ELB[Elastic Load Balancing]
    Tools --> Route53[Route 53]
    
    ASG --> ASG1[Auto scale instances]
    ASG --> ASG2[Replace unhealthy]
    ASG --> ASG3[Multi-AZ support]
    
    ELB --> ELB1[Distribute traffic]
    ELB --> ELB2[Health checks]
    ELB --> ELB3[SSL termination]
    
    Route53 --> R1[DNS failover]
    Route53 --> R2[Health checks]
    Route53 --> R3[Multi-region routing]
```

### Tools Summary

| Tool | Purpose |
|------|---------|
| **Auto Scaling Groups (ASG)** | Automatically scale EC2 instances |
| **Elastic Load Balancer (ELB)** | Distribute traffic across instances |
| **Route 53** | DNS failover and traffic routing |

---

## Implementation Architecture

### HA + Scalable Web Application

```mermaid
graph TD
    User[Users] --> R53[Route 53<br/>DNS]
    R53 --> ELB[Elastic Load Balancer]
    
    subgraph Region["AWS Region"]
        ELB --> AZ1[AZ 1]
        ELB --> AZ2[AZ 2]
        ELB --> AZ3[AZ 3]
        
        AZ1 --> I1[Web Servers]
        AZ2 --> I2[Web Servers]
        AZ3 --> I3[Web Servers]
        
        ASG[Auto Scaling Group] -.->|Manages| I1
        ASG -.->|Manages| I2
        ASG -.->|Manages| I3
    end
    
    User -.->|Failure in AZ 1| ELB
    ELB -.->|Routes to healthy AZs| AZ2
    ELB -.->|Routes to healthy AZs| AZ3
```

---

## Key Takeaways

1. **Scalability** = Handle increasing loads by adapting
2. **Two Types**:
   - **Vertical**: Increase instance size (Scale Up/Down)
   - **Horizontal**: Increase instance count (Scale Out/In)
3. **Vertical Scalability**:
   - Common for non-distributed systems (databases)
   - Limited by hardware
   - Example: t2.nano → u-12tb1.metal
4. **Horizontal Scalability**:
   - Uses Auto Scaling Groups + Load Balancers
   - Practically unlimited
5. **High Availability**:
   - Survive failures, minimize downtime
   - Run in at least 2 AZs
   - ASG + ELB across multiple AZs
6. **Scalability vs HA**:
   - Scalability = capacity
   - HA = reliability
7. **Elasticity** = automatic scaling based on load
8. **Agility** = quick resource provisioning
9. **AWS Tools**: ASG, ELB, Route 53
10. **Best Practice**: Combine vertical + horizontal + HA for robust architecture

---

## Next Steps

⬅️ Previous: [EC2 Security & Storage](./14-ec2-security-storage.md) | ➡️ Next: [Load Balancing, Auto Scaling, and Route 53](./15-load-balancing.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*