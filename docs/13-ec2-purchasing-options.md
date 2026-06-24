# EC2 Purchasing Options

## The Big Picture

Amazon EC2 offers **7 purchasing options** to suit different workload requirements and budgetary considerations. Choosing the right option is critical for **cost optimization**.

---

## EC2 Purchasing Options Overview

```mermaid
graph TD
    PO[EC2 Purchasing Options] --> OD[On-Demand Instances]
    PO --> RI[Reserved Instances]
    PO --> SP[Savings Plans]
    PO --> Spot[Spot Instances]
    PO --> DH[Dedicated Hosts]
    PO --> DI[Dedicated Instances]
    PO --> CR[Capacity Reservations]
    
    OD --> ODFlex[Pay-as-you-go<br/>No commitment]
    RI --> RISave[72% discount<br/>1-3 year commitment]
    SP --> SPFlex[Savings across<br/>EC2, Lambda, Fargate]
    Spot --> SpotBid[Up to 90% savings<br/>Bid on spare capacity]
    DH --> DHPhys[Physical server<br/>Single customer]
    DI --> DIExclusive[Dedicated hardware<br/>Shared account]
    CR --> CRGuar[Guaranteed capacity<br/>In specific AZ]
```

### Quick Comparison

| Option | Discount | Commitment | Best For |
|--------|----------|------------|----------|
| **On-Demand** | 0% | None | Unpredictable workloads |
| **Reserved Instances** | Up to 72% | 1-3 years | Steady-state workloads |
| **Savings Plans** | Up to 72% | 1-3 years | Flexible usage |
| **Spot Instances** | Up to 90% | None | Fault-tolerant workloads |
| **Dedicated Hosts** | None | On-demand/reserved | Compliance, licensing |
| **Dedicated Instances** | None | Per instance | Hardware isolation |
| **Capacity Reservations** | Variable | None/Specific | Capacity guarantee |

---

## 1. On-Demand Instances

```mermaid
graph LR
    OD[On-Demand Instances] --> Features[Features]
    Features --> F1[Pay-as-you-go]
    Features --> F2[No long-term commitments]
    Features --> F3[Start/stop flexibility]
    Features --> F4[No upfront payments]
    Features --> F5[No contracts]
    
    OD --> Use[Use Cases]
    Use --> U1[Variable Workloads]
    Use --> U2[Development and Testing]
    Use --> U3[Short-Term Projects]
```

### Key Characteristics

| Aspect | Details |
|--------|---------|
| **Pricing Model** | Pay-as-you-go |
| **Commitment** | None |
| **Payment** | No upfront costs |
| **Flexibility** | Start and stop instances as needed |

### Use Cases

| Use Case | Description |
|----------|-------------|
| **Variable Workloads** | Unpredictable usage patterns or fluctuating workloads |
| **Development and Testing** | Temporary environments without long-term commitments |
| **Short-Term Projects** | Quick provisioning for short-term initiatives |

---

## 2. Reserved Instances

```mermaid
graph TD
    RI[Reserved Instances] --> Features[Features]
    Features --> F1[72% discount vs On-Demand]
    Features --> F2[Reserve specific attributes]
    Features --> F3[1 or 3-year terms]
    Features --> F4[Capacity reservation]
    
    RI --> Reserve[Reserve Attributes]
    Reserve --> R1[Instance Type]
    Reserve --> R2[Region]
    Reserve --> R3[Tenancy]
    Reserve --> R4[OS]
    
    RI --> Use[Use Cases]
    Use --> U1[Steady-state workloads]
    Use --> U2[Predictable usage patterns]
    Use --> U3[Capacity planning]
```

### Reserved Attributes

| Attribute | Description |
|-----------|-------------|
| **Instance Type** | Specific instance type (e.g., m5.large) |
| **Region** | Specific AWS region |
| **Tenancy** | Shared or dedicated |
| **OS** | Linux, Windows, etc. |

### Use Cases

| Use Case | Description |
|----------|-------------|
| **Steady-State Usage** | Predictable workload patterns |
| **Capacity Planning** | Secure reserved capacity for anticipated workloads |

---

## 3. Savings Plans

```mermaid
graph TD
    SP[Savings Plans] --> Features[Features]
    Features --> F1[Flexible pricing model]
    Features --> F2[Savings across services]
    Features --> F3[Significant discounts vs On-Demand]
    Features --> F4[Flexible commitment options]
    
    SP --> Covers[Covers]
    Covers --> C1[EC2 Instances]
    Covers --> C2[AWS Lambda]
    Covers --> C3[Fargate]
    
    SP --> Use[Ideal For]
    Use --> U1[Consistent usage workloads]
    Use --> U2[Flexibility in instance types]
    Use --> U3[Flexibility in sizes]
```

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Flexibility** | Choose between different commitment options |
| **Broad Coverage** | Compute usage across EC2, Lambda, Fargate |
| **Significant Discounts** | Compared to On-Demand rates |

---

## 4. Spot Instances

```mermaid
graph LR
    Spot[Spot Instances] --> Features[Features]
    Features --> F1[Bid on unused EC2 capacity]
    Features --> F2[Up to 90% savings]
    Features --> F3[Reduced rates]
    Features --> F4[Fault-tolerant applications]
    
    Spot --> Use[Use Cases]
    Use --> U1[Resilient to failure workloads]
    Use --> U2[Batch jobs]
    Use --> U3[Data analysis]
    Use --> U4[Image processing]
    
    Spot --> Warning[⚠️ Can be interrupted<br/>if AWS needs capacity]
```

### Key Characteristics

| Aspect | Details |
|--------|---------|
| **Pricing** | Up to 90% less than On-Demand |
| **Capacity** | Spare EC2 capacity |
| **Risk** | Can be interrupted with 2-minute notice |
| **Best For** | Fault-tolerant, flexible workloads |

### Use Cases

| Use Case | Description |
|----------|-------------|
| **Fault-tolerant Workloads** | Resilient to failure |
| **Batch Jobs** | Processing jobs that can be interrupted |
| **Data Analysis** | Analytics that can resume |
| **Image Processing** | Rendering and processing tasks |

---

## 5. Dedicated Hosts

```mermaid
graph TD
    DH[Dedicated Hosts] --> Features[Features]
    Features --> F1[Physical servers dedicated<br/>to single customer]
    Features --> F2[Complete isolation]
    Features --> F3[Control over instance placement]
    Features --> F4[Compliance support]
    
    DH --> Benefits[Benefits]
    Benefits --> B1[Compliance and regulatory]
    Benefits --> B2[Bring your own license]
    Benefits --> B3[Microsoft, Oracle, etc.]
    
    DH --> Use[Use Cases]
    Use --> U1[Sensitive data]
    Use --> U2[Patient health records]
    Use --> U3[Regulatory requirements]
    Use --> U4[License-restricted software]
```

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Compliance** | Meet regulatory requirements |
| **Licensing** | Use eligible software licenses (Microsoft, Oracle, etc.) |
| **Isolation** | Physical server dedicated to single customer |
| **Control** | Instance placement control |

### Use Cases

| Use Case | Description |
|----------|-------------|
| **Sensitive Data** | Healthcare organizations with patient data subject to strict regulations |
| **License Restrictions** | Software requiring specific hardware |
| **Compliance** | Regulatory requirements demanding dedicated infrastructure |

---

## 6. Dedicated Instances

```mermaid
graph TD
    DI[Dedicated Instances] --> Features[Features]
    Features --> F1[Physical servers exclusively<br/>for single customer]
    Features --> F2[Enhanced security and isolation]
    Features --> F3[Predictable performance]
    Features --> F4[No control over instance placement]
    Features --> F5[Can move hardware after Stop/Start]
    
    DI --> Note[Note: May share hardware<br/>with other instances<br/>in same account]
    
    DI --> Use[Use Cases]
    Use --> U1[Sensitive data]
    Use --> U2[Banking institutions]
    Use --> U3[Strict compliance requirements]
```

### Dedicated Hosts vs Dedicated Instances

| Aspect | Dedicated Hosts | Dedicated Instances |
|--------|----------------|-------------------|
| **Physical Server** | Fully dedicated to you | Dedicated hardware, but can move |
| **Instance Placement** | Full control | No control |
| **Hardware Movement** | Stays on same host | Can move after Stop/Start |
| **Licensing** | Bring your own | Limited BYOL options |

---

## 7. Capacity Reservations

```mermaid
graph TD
    CR[Capacity Reservations] --> Features[Features]
    Features --> F1[Reserve capacity for specific<br/>instance types in AZ]
    Features --> F2[Guaranteed capacity availability]
    Features --> F3[Flexibility to launch when needed]
    Features --> F4[Works with On-Demand, RI, SP]
    
    CR --> Works[Works With]
    Works --> W1[On-Demand instances]
    Works --> W2[Reserved Instances]
    Works --> W3[Savings Plans]
    
    CR --> Use[Use Cases]
    Use --> U1[Predictable performance]
    Use --> U2[Capacity assurance]
    Use --> U3[Mission-critical applications]
```

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Guaranteed Capacity** | Resource availability when needed |
| **Flexible Launch** | Launch instances within reservation as needed |
| **Multiple Pricing** | Combine with On-Demand, RI, or Savings Plans |

---

## How to Choose EC2 Purchasing Options

### Decision Framework

```mermaid
graph TD
    Start[Need to Choose Option] --> Q1{Workload Type?}
    
    Q1 -->|Predictable/Steady| Q2{Long-term?}
    Q2 -->|Yes| RI[Reserved Instances]
    Q2 -->|No| SP[Savings Plans]
    
    Q1 -->|Unpredictable| Q3{Fault-tolerant?}
    Q3 -->|Yes| Spot[Spot Instances]
    Q3 -->|No| OD[On-Demand]
    
    Q1 -->|Compliance Required| Q4{Software License?}
    Q4 -->|Yes| DH[Dedicated Hosts]
    Q4 -->|No| DI[Dedicated Instances]
    
    Q1 -->|Capacity Critical| CR[Capacity Reservations]
```

### Step-by-Step Selection Process

```mermaid
graph TD
    Step[Selection Process] --> S1[1. Define Workload Requirements]
    S1 --> S2[2. Analyze Performance Characteristics]
    S2 --> S3[3. Consider Cost Optimization]
    S3 --> S4[4. Review Use Case Suitability]
    S4 --> S5[5. Evaluate Instance Families]
    S5 --> S6[6. Monitor and Optimize]
    
    S1 --> D1[Compute, memory, storage,<br/>networking needs]
    S2 --> D2[CPU performance, memory,<br/>network performance]
    S3 --> D3[On-Demand, RI, Savings Plans,<br/>Spot options]
    S4 --> D4[General Purpose, Compute,<br/>Memory, Storage Optimized]
    S5 --> D5[Testing and benchmarking]
    S6 --> D6[Continuous monitoring,<br/>identify optimization]
```

### Detailed Steps

#### Step 1: Define Workload Requirements

| Factor | Considerations |
|--------|---------------|
| Compute | CPU cores needed |
| Memory | RAM requirements |
| Storage | EBS, Instance Store needs |
| Networking | Bandwidth requirements |

#### Step 2: Analyze Performance Characteristics

| Factor | Considerations |
|--------|---------------|
| CPU Performance | Processor speed, architecture |
| Memory Size | GB of RAM |
| Network Performance | Bandwidth, latency |

#### Step 3: Consider Cost Optimization

| Option | When to Use |
|--------|------------|
| **On-Demand** | Short-term, unpredictable |
| **Reserved Instances** | Long-term, predictable |
| **Savings Plans** | Flexible compute usage |
| **Spot Instances** | Fault-tolerant, flexible timing |

#### Step 4: Review Use Case Suitability

| Family | Best For |
|--------|----------|
| General Purpose | Web servers, code repos |
| Compute Optimized | Batch processing, HPC |
| Memory Optimized | In-memory DBs, caching |
| Storage Optimized | OLTP, data warehousing |

#### Step 5: Evaluate Instance Families

```mermaid
graph LR
    Eval[Evaluation Methods] --> T[Testing]
    Eval --> B[Benchmarking]
    
    T --> T1[Validate performance]
    T --> T2[Confirm suitability]
    
    B --> B1[Measure metrics]
    B --> B2[Compare options]
```

#### Step 6: Monitor and Optimize

```mermaid
graph TD
    Monitor[Continuous Monitoring] --> M1[Track performance metrics]
    Monitor --> M2[Track usage patterns]
    Monitor --> M3[Identify optimization opportunities]
    Monitor --> M4[Make adjustments as needed]
    Monitor --> M5[Meet evolving requirements]
```

---

## Comparison Matrix

| Option | Discount | Commitment | Interruptible | Use Case |
|--------|----------|------------|----------------|----------|
| **On-Demand** | 0% | None | No | Unpredictable |
| **Reserved** | Up to 72% | 1-3 years | No | Steady-state |
| **Savings Plans** | Up to 72% | 1-3 years | No | Flexible usage |
| **Spot** | Up to 90% | None | Yes (2-min notice) | Fault-tolerant |
| **Dedicated Hosts** | None | On-demand/Reserved | No | Compliance |
| **Dedicated Instances** | None | Per instance | No | Hardware isolation |
| **Capacity Reservations** | Variable | None/Specific | No | Capacity guarantee |

---

## Cost Optimization Strategy

```mermaid
graph TD
    Strategy[Cost Optimization Strategy] --> S1[Use Spot for fault-tolerant]
    Strategy --> S2[Use Reserved for predictable]
    Strategy --> S3[Use On-Demand for variable]
    Strategy --> S4[Combine options]
    Strategy --> S5[Monitor continuously]
    
    S1 --> Spot[Spot Instances<br/>Up to 90% off]
    S2 --> RI[Reserved Instances<br/>Up to 72% off]
    S3 --> OD[On-Demand<br/>Full price, no commitment]
    S4 --> Mix[Mix for optimal cost]
    S5 --> Optimize[Regular review]
```

### Combined Approach Example

```mermaid
graph TD
    App[Application Workload] --> Baseline[Baseline: Reserved<br/>For predictable load]
    App --> Burst[Burst: Spot<br/>For flexible additional capacity]
    App --> Dev[Dev/Test: On-Demand<br/>For temporary environments]
    
    Baseline --> B1[60% of capacity<br/>72% discount]
    Burst --> Bu1[30% of capacity<br/>Up to 90% off]
    Dev --> D1[10% of capacity<br/>Pay-as-you-go]
```

---

## Key Takeaways

1. **7 Purchasing Options** for different needs:
   - On-Demand, Reserved, Savings Plans, Spot, Dedicated Hosts, Dedicated Instances, Capacity Reservations
2. **Discount Levels**:
   - Reserved/Savings Plans: up to 72%
   - Spot: up to 90%
3. **Best for Each**:
   - On-Demand: Variable, short-term
   - Reserved: Steady-state, predictable
   - Savings Plans: Flexible usage across services
   - Spot: Fault-tolerant workloads
   - Dedicated Hosts: Compliance, licensing
   - Dedicated Instances: Hardware isolation
   - Capacity Reservations: Guaranteed capacity
4. **Selection Process**: Define → Analyze → Cost Optimize → Review → Evaluate → Monitor
5. **Spot Instances** can be interrupted with 2-minute notice
6. **Dedicated Hosts** give full control over placement; Dedicated Instances can move
7. **Combined Approach** often provides best cost optimization
8. **Continuous Monitoring** essential for ongoing optimization

---

## Next Steps

⬅️ Previous: [EC2 Instance Types](./12-ec2-instance-types.md) | ➡️ Next: [EBS Volumes](./14-ebs-volumes.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*