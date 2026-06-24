# AWS Pricing Models

> ⏱️ **Estimated Study Time:** 18 minutes  
> 🎯 **CCP Exam Weight:** ~12-15% (Domain 4: Billing & Pricing)

---

## The Big Picture

AWS uses a **pay-as-you-go pricing model** that provides flexibility and cost optimization. Understanding the 4 core pricing principles and EC2 purchasing options is essential for cost management and heavily tested on the CCP exam.

---

## 4 Core AWS Pricing Principles

```mermaid
mindmap
  root((AWS Pricing<br/>Principles))
    💰 Pay as You Go
      No upfront investment
      Pay for what you use
      Based on consumption
    📅 Save When You Reserve
      1-3 year commitment
      Up to 75% savings
      Predictable workloads
    📊 Pay Less by Using More
      Volume-based discounts
      More = lower per-unit
      Tiered pricing
    📉 Pay Less as AWS Grows
      Operational efficiencies
      Passed to customers
      Prices decrease over time
```

> 🎯 **Exam Tip:** Memorize the 4 pricing principles: **Pay as you go · Reserve · Volume discounts · AWS growth savings**.

---

## Principle 1: Pay as You Go

```mermaid
graph LR
    Traditional[🏢 Traditional IT] -->|Upfront| CapEx[$50K-$1M+<br/>Before use]
    Cloud[☁️ Pay as You Go] -->|Usage-based| OpEx[Pay only for<br/>what you use]
    
    Traditional --> T1[Long procurement cycles]
    Cloud --> C1[Deploy in minutes]
    
    style Traditional fill:#FF6B6B,color:#fff
    style Cloud fill:#51CF66,color:#fff
```

### Pay as You Go Details

| Feature | Description |
|---------|-------------|
| **Investment** | No upfront cost |
| **Payment** | Pay only for individual services used |
| **Basis** | Actual resource consumption |
| **Example** | EC2 for 3 hours = billed for 3 hours |

---

## Principle 2: Save When You Reserve

```mermaid
graph LR
    OnDemand[⏱️ On-Demand<br/>Full price] -->|Commit 1-3 years| Reserved[📅 Reserved<br/>Up to 75% off]
    
    OnDemand --> O1[No commitment]
    Reserved --> R1[Significant savings]
    
    style OnDemand fill:#FF6B6B,color:#fff
    style Reserved fill:#51CF66,color:#fff
```

### Reserved Services

| Service | Reservation Type |
|---------|------------------|
| **EC2 Reserved Instances** | Instance type, Region, tenancy, OS |
| **DynamoDB Reserved Capacity** | Read/write capacity |
| **RDS Reserved Instances** | DB instance type, engine |
| **Redshift Reserved Instances** | Node type |
| **ElastiCache Reserved Nodes** | Cache node type |

---

## Principle 3: Pay Less by Using More

```mermaid
graph TD
    Volume[📊 Volume-Based Discounts] --> V1[More usage =<br/>lower per-unit cost]
    
    V1 --> S3[S3 Storage<br/>Tiered pricing]
    V1 --> EC2[EC2 Data Transfer<br/>Decreasing rates]
    V1 --> CloudFront[CloudFront<br/>Volume discounts]
    
    style Volume fill:#FFD700,color:#000
```

### Volume Discount Examples

| Service | Discount Structure |
|---------|-------------------|
| **S3 Storage** | First 50 TB at $0.023/GB, next 450 TB at $0.022, etc. |
| **Data Transfer** | First 10 TB free, then decreasing rates |
| **CloudFront** | Tiered pricing based on data transfer |

---

## Principle 4: Pay Less as AWS Grows

```mermaid
graph TD
    Growth[📉 AWS Growth Savings] --> G1[Operational efficiencies]
    G1 --> G2[Passed to customers]
    G2 --> G3[Prices decrease over time]
    
    G3 --> G4[Economies of scale]
    G3 --> G5[Technology innovation]
    
    style Growth fill:#51CF66,color:#fff
```

---

## EC2 Purchasing Options

AWS offers **7 purchasing options** for EC2 to suit different workload requirements:

```mermaid
graph TD
    PO[💰 EC2 Purchasing Options] --> OD[⏱️ On-Demand]
    PO --> RI[📅 Reserved Instances]
    PO --> SP[📊 Savings Plans]
    PO --> Spot[💸 Spot Instances]
    PO --> DH[🏢 Dedicated Hosts]
    PO --> DI[🔒 Dedicated Instances]
    PO --> CR[📍 Capacity Reservations]
    
    style PO fill:#FF9900,color:#000
```

### Quick Comparison Table

| Option | Discount | Commitment | Interruptible | Best For |
|--------|----------|------------|----------------|----------|
| **On-Demand** | 0% | None | No | Unpredictable workloads |
| **Reserved Instances** | Up to 72% | 1-3 years | No | Steady-state workloads |
| **Savings Plans** | Up to 72% | 1-3 years | No | Flexible usage |
| **Spot Instances** | Up to 90% | None | Yes (2-min notice) | Fault-tolerant workloads |
| **Dedicated Hosts** | None | On-demand/reserved | No | Compliance, licensing |
| **Dedicated Instances** | None | Per instance | No | Hardware isolation |
| **Capacity Reservations** | Variable | None/Specific | No | Guaranteed capacity |

---

### 1. On-Demand Instances

```mermaid
graph LR
    OD[⏱️ On-Demand] --> F1[Pay-as-you-go]
    OD --> F2[No commitment]
    OD --> F3[Start/stop anytime]
    OD --> F4[Pay by second/hour]
    
    style OD fill:#FF6B6B,color:#fff
```

| Aspect | Detail |
|--------|--------|
| **Pricing** | Full price, no discount |
| **Commitment** | None |
| **Flexibility** | Maximum |
| **Best For** | Variable workloads, dev/test, short-term projects |

---

### 2. Reserved Instances

```mermaid
graph TD
    RI[📅 Reserved Instances] --> F1[Up to 72% savings]
    RI --> F2[1 or 3-year terms]
    RI --> F3[Reserve specific attributes]
    RI --> F4[Capacity reservation]
    
    F3 --> A1[Instance type]
    F3 --> A2[Region]
    F3 --> A3[Tenancy]
    F3 --> A4[OS]
    
    style RI fill:#51CF66,color:#fff
```

| Aspect | Detail |
|--------|--------|
| **Discount** | Up to 72% vs On-Demand |
| **Commitment** | 1 or 3 years |
| **Attributes** | Instance type, Region, tenancy, OS |
| **Best For** | Steady-state, predictable workloads |

---

### 3. Savings Plans

```mermaid
graph TD
    SP[📊 Savings Plans] --> F1[Flexible pricing]
    SP --> F2[Savings across services]
    SP --> F3[EC2, Lambda, Fargate]
    SP --> F4[1-3 year commitment]
    
    style SP fill:#51CF66,color:#fff
```

| Aspect | Detail |
|--------|--------|
| **Discount** | Up to 72% |
| **Commitment** | 1-3 years (hourly spend) |
| **Flexibility** | EC2, Lambda, Fargate |
| **Best For** | Flexible compute usage across services |

---

### 4. Spot Instances

```mermaid
graph LR
    Spot[💸 Spot Instances] --> F1[Up to 90% savings]
    Spot --> F2[Bid on spare capacity]
    Spot --> F3[Interruptible]
    Spot --> F4[Fault-tolerant workloads]
    
    F3 --> Warn[⚠️ 2-minute notice<br/>before interruption]
    
    style Spot fill:#FFD700,color:#000
    style Warn fill:#FF6B6B,color:#fff
```

| Aspect | Detail |
|--------|--------|
| **Discount** | Up to 90% |
| **Capacity** | Spare EC2 capacity |
| **Risk** | Can be interrupted with 2-minute notice |
| **Best For** | Fault-tolerant, flexible workloads |

---

### 5. Dedicated Hosts

```mermaid
graph TD
    DH[🏢 Dedicated Hosts] --> F1[Physical server<br/>single customer]
    DH --> F2[Compliance support]
    DH --> F3[BYOL - Bring Your Own License]
    DH --> F4[Full placement control]
    
    style DH fill:#DDA0DD
```

| Aspect | Detail |
|--------|--------|
| **Type** | Physical server dedicated to you |
| **Discount** | None (premium pricing) |
| **Licensing** | Bring your own (Microsoft, Oracle) |
| **Best For** | Compliance, licensing requirements |

---

### 6. Dedicated Instances

```mermaid
graph TD
    DI[🔒 Dedicated Instances] --> F1[Dedicated hardware]
    DI --> F2[Shared account]
    DI --> F3[No placement control]
    DI --> F4[Can move hardware after stop/start]
    
    style DI fill:#87CEEB
```

| Aspect | Detail |
|--------|--------|
| **Type** | Hardware dedicated to your account |
| **Placement** | No control |
| **Movement** | Can move after stop/start |
| **Best For** | Hardware isolation |

---

### 7. Capacity Reservations

```mermaid
graph TD
    CR[📍 Capacity Reservations] --> F1[Reserve capacity in AZ]
    CR --> F2[Guaranteed availability]
    CR --> F3[Works with On-Demand/RI/SP]
    CR --> F4[Flexible launch]
    
    style CR fill:#4DABF7
```

| Aspect | Detail |
|--------|--------|
| **Purpose** | Reserve capacity for specific AZ |
| **Guarantee** | Capacity always available |
| **Compatibility** | Works with On-Demand, RI, Savings Plans |
| **Best For** | Mission-critical applications needing capacity |

---

## Purchasing Decision Flowchart

```mermaid
flowchart TD
    Start{Need to choose<br/>purchasing option?} --> Q1{Workload<br/>predictable?}
    
    Q1 -->|Yes| Q2{Long-term<br/>commitment OK?}
    Q1 -->|No| Q3{Fault-tolerant?}
    
    Q2 -->|Yes| RI[📅 Reserved Instances<br/>72% savings]
    Q2 -->|No| SP[📊 Savings Plans<br/>72% savings, flexible]
    
    Q3 -->|Yes| Spot[💸 Spot Instances<br/>90% savings]
    Q3 -->|No| OD[⏱️ On-Demand<br/>No commitment]
    
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

---

## Combined Approach Example

```mermaid
graph TD
    App[📱 Application Workload] --> Baseline[📊 Baseline: Reserved<br/>60% capacity]
    App --> Burst[⚡ Burst: Spot<br/>30% capacity]
    App --> Dev[🛠️ Dev/Test: On-Demand<br/>10% capacity]
    
    Baseline --> B1[72% discount<br/>for predictable load]
    Burst --> Bu1[90% discount<br/>for flexibility]
    Dev --> D1[Pay-as-you-go<br/>for temporary env]
    
    style Baseline fill:#51CF66,color:#fff
    style Burst fill:#FFD700,color:#000
    style Dev fill:#FF6B6B,color:#fff
```

### Cost Optimization Strategy

| Workload Type | Recommended Mix |
|---------------|----------------|
| **Steady-state production** | Reserved Instances (70-80%) |
| **Variable traffic** | On-Demand (10-20%) |
| **Fault-tolerant batch jobs** | Spot Instances (10-20%) |
| **Dev/test environments** | Spot + On-Demand |
| **Compliance workloads** | Dedicated Hosts |

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **Pay as You Go** | No upfront, pay for usage |
| **Reserved** | 1-3 year commit, up to 72% off |
| **Savings Plans** | Flexible across EC2/Lambda/Fargate |
| **Spot** | Up to 90% off, interruptible |
| **Dedicated Hosts** | Physical server, BYOL |
| **Dedicated Instances** | Hardware isolation, shared account |
| **Capacity Reservations** | Guaranteed capacity in specific AZ |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: Which EC2 purchasing option offers the GREATEST discount but can be interrupted with 2-minute notice?</strong></summary>

**A.** On-Demand Instances  
**B.** Reserved Instances  
**C.** Spot Instances  
**D.** Savings Plans  

**Answer: C** — Spot Instances offer up to 90% savings (the greatest discount), but AWS can interrupt them with a 2-minute notice when it needs the capacity back. Use them only for fault-tolerant workloads.
</details>

<details>
<summary><strong>Q2: Which purchasing option is best for steady-state, predictable workloads with 1-3 year commitment?</strong></summary>

**A.** On-Demand Instances  
**B.** Reserved Instances  
**C.** Spot Instances  
**D.** Dedicated Hosts  

**Answer: B** — Reserved Instances offer up to 72% savings for 1-3 year commitments and are ideal for steady-state, predictable workloads. They also provide capacity reservation.
</details>

<details>
<summary><strong>Q3: Which option should you choose for compliance requirements that need a physical server with specific licensing?</strong></summary>

**A.** On-Demand Instances  
**B.** Reserved Instances  
**C.** Spot Instances  
**D.** Dedicated Hosts  

**Answer: D** — Dedicated Hosts provide a physical server dedicated to a single customer, allowing you to use your own licenses (BYOL) for software like Microsoft, Oracle, etc. They also support compliance and regulatory requirements.
</details>

---

## Navigation

⬅️ Previous: [Well-Architected Framework](../04-security-architecture/03-well-architected-framework.md) | ➡️ Next: [Billing Management](./02-billing-management.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*