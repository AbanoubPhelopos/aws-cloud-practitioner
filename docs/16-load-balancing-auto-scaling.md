# Load Balancing & Auto Scaling

## The Big Picture

This module covers **Load Balancers** (distributing traffic across instances) and **Auto Scaling Groups** (dynamically adjusting capacity) - the two key AWS services for building highly available and scalable applications.

---

## I. Load Balancers Overview

```mermaid
graph TD
    LB[Load Balancers] --> Def[Definition]
    Def --> D1[Servers that distribute<br/>incoming internet traffic<br/>across multiple downstream servers]
    
    LB --> Purpose[Purpose]
    Purpose --> P1[Distribute traffic]
    Purpose --> P2[Single point of access]
    Purpose --> P3[Handle failures]
    Purpose --> P4[Health checks]
    Purpose --> P5[SSL termination]
    Purpose --> P6[High availability]
```

### What Load Balancers Do

| Function | Description |
|----------|-------------|
| **Distribute Traffic** | Across multiple downstream instances |
| **Single Point of Access** | Single DNS endpoint for your application |
| **Failure Handling** | Seamlessly handle failures of downstream instances |
| **Health Checks** | Perform regular health checks on instances |
| **SSL Termination** | Handle HTTPS encryption/decryption |
| **High Availability** | Ensure HA across zones |

---

## Why Use Elastic Load Balancer (ELB)?

```mermaid
graph TD
    ELB[Elastic Load Balancer] --> Managed[Managed Service]
    
    Managed --> M1[AWS ensures functionality]
    Managed --> M2[Handles upgrades]
    Managed --> M3[Handles maintenance]
    Managed --> M4[Ensures high availability]
    
    ELB --> Limits[Limitations]
    Limits --> L1[Limited configuration options]
    Limits --> L2[But reduces setup effort]
    Limits --> L3[And reduces maintenance cost]
```

### ELB Benefits vs Custom Load Balancer

| Aspect | Custom Load Balancer | ELB |
|--------|---------------------|-----|
| **Setup** | Manual, complex | Quick, easy |
| **Maintenance** | Your responsibility | AWS managed |
| **Upgrades** | Manual | Automatic |
| **High Availability** | Build yourself | Built-in |
| **Configuration** | Fully customizable | Limited but sufficient |

---

## Four Types of Load Balancers

```mermaid
graph TD
    LB[AWS Load Balancers] --> ALB[Application Load Balancer<br/>Layer 7]
    LB --> NLB[Network Load Balancer<br/>Layer 4]
    LB --> GWLB[Gateway Load Balancer<br/>Layer 3]
    LB --> CLB[Classic Load Balancer<br/>Layer 4 & 7<br/>RETIRED 2023]
    
    ALB --> ALBUse[HTTP/HTTPS traffic]
    NLB --> NLBUse[Ultra-high performance<br/>TCP traffic]
    GWLB --> GWLBUse[Layer 3 operations]
```

### Load Balancer Types Comparison

| Type | Layer | Use Case | Status |
|------|-------|----------|--------|
| **Application Load Balancer (ALB)** | Layer 7 | HTTP/HTTPS traffic | ✅ Active |
| **Network Load Balancer (NLB)** | Layer 4 | TCP, ultra-high performance | ✅ Active |
| **Gateway Load Balancer** | Layer 3 | Layer 3 routing | ✅ Active |
| **Classic Load Balancer (CLB)** | Layer 4 & 7 | Legacy | ❌ Retired 2023 |

### Detailed Comparison

```mermaid
graph TD
    Compare[Load Balancer Comparison] --> ALB[ALB<br/>Layer 7<br/>HTTP/HTTPS<br/>Content-based routing]
    Compare --> NLB[NLB<br/>Layer 4<br/>TCP/UDP<br/>Ultra-high performance<br/>Static IP]
    Compare --> GWLB[GWLB<br/>Layer 3<br/>Gateway operations<br/>Third-party appliances]
    
    ALB --> ALBUse[Web applications<br/>Microservices<br/>Container-based apps]
    NLB --> NLBUse[Gaming<br/>IoT<br/>Real-time applications]
    GWLB --> GWLBUse[Firewalls<br/>IDS/IPS<br/>Network appliances]
```

---

## II. Auto Scaling Groups (ASG)

### Why Auto Scaling?

```mermaid
graph TD
    Why[Why ASG?] --> Problem[Problem]
    Problem --> P1[Load fluctuates in real life]
    Problem --> P2[Sites/apps have variable demand]
    
    Why --> Solution[Cloud Solution]
    Solution --> S1[Quickly provision servers]
    Solution --> S2[Quickly decommission servers]
```

### ASG Goals

```mermaid
graph TD
    ASG[Auto Scaling Group Goals] --> G1[Scale out<br/>Add EC2 instances]
    ASG --> G2[Scale in<br/>Remove EC2 instances]
    ASG --> G3[Maintain min/max instances]
    ASG --> G4[Register new instances with LB]
    ASG --> G5[Replace unhealthy instances]
    
    ASG --> Benefit[Benefit]
    Benefit --> B1[Optimize capacity]
    Benefit --> B2[Reduce costs]
    Benefit --> B3[Run only necessary instances]
```

### ASG Capabilities

| Capability | Description |
|------------|-------------|
| **Scale Out** | Add EC2 instances to handle increased load |
| **Scale In** | Remove EC2 instances to reduce excess capacity |
| **Maintain Capacity** | Keep within specified minimum and maximum |
| **Auto Register** | Register new instances with load balancer |
| **Health Replacement** | Replace unhealthy instances |

---

## III. Auto Scaling Strategies

```mermaid
graph TD
    Strategies[ASG Scaling Strategies] --> Manual[Manual Scaling]
    Strategies --> Dynamic[Dynamic Scaling]
    
    Dynamic --> Simple[Simple/Step Scaling]
    Dynamic --> Target[Target Tracking Scaling]
    Dynamic --> Scheduled[Scheduled Scaling]
    Dynamic --> Predictive[Predictive Scaling]
```

### 1. Manual Scaling

```mermaid
graph LR
    Manual[Manual Scaling] --> M1[Adjust ASG size<br/>manually as needed]
    
    When[When to Use] --> W1[Predictable events<br/>One-time changes]
```

| Aspect | Description |
|--------|-------------|
| **How** | Manually adjust ASG size |
| **When** | One-time changes, predictable events |
| **Use Case** | Special events, maintenance windows |

### 2. Simple / Step Scaling

```mermaid
graph TD
    Step[Simple/Step Scaling] --> Alarm[CloudWatch Alarm]
    Alarm --> CPU{CPU > 70%?}
    CPU -->|Yes| Add[Add 2 instances]
    CPU -->|No| CPU2{CPU < 30%?}
    CPU2 -->|Yes| Remove[Remove 1 instance]
    CPU2 -->|No| Wait[Wait for next check]
```

| Aspect | Description |
|--------|-------------|
| **How** | CloudWatch alarm triggers scaling action |
| **Action** | Add/remove specific number of instances |
| **Example** | Add 2 units if CPU > 70%, Remove 1 if CPU < 30% |

### 3. Target Tracking Scaling

```mermaid
graph TD
    Target[Target Tracking Scaling] --> Goal[Maintain target metric]
    Goal --> G1[Keep average ASG CPU<br/>around 40%]
    
    Target --> Auto[Automatic Adjustment]
    Auto --> A1[Scale out if CPU > 40%]
    Auto --> A2[Scale in if CPU < 40%]
```

| Aspect | Description |
|--------|-------------|
| **How** | Maintain a target metric value |
| **Example** | Keep average ASG CPU at 40% |
| **Behavior** | Automatically scale up or down |

### 4. Scheduled Scaling

```mermaid
graph TD
    Schedule[Scheduled Scaling] --> Plan[Plan for known patterns]
    Plan --> P1[Increase min capacity to 10<br/>at 5 pm on Fridays]
    
    When[When to Use] --> W1[Predictable patterns<br/>Time-based]
```

| Aspect | Description |
|--------|-------------|
| **How** | Plan scaling actions based on known patterns |
| **Example** | Increase capacity to 10 at 5 PM Fridays |
| **Use Case** | Predictable traffic spikes |

### 5. Predictive Scaling

```mermaid
graph TD
    Predictive[Predictive Scaling] --> ML[Machine Learning]
    ML --> Forecast[Forecast future traffic]
    Forecast --> Provision[Provision EC2 instances<br/>in advance]
    
    When[When to Use] --> W1[Complex patterns<br/>Need forecast]
```

| Aspect | Description |
|--------|-------------|
| **How** | Uses Machine Learning to forecast traffic |
| **Behavior** | Automatically provisions instances in advance |
| **Use Case** | Complex traffic patterns |

---

## Scaling Strategies Comparison

```mermaid
graph TD
    Compare[Strategy Comparison] --> M[Manual]
    Compare --> S[Simple/Step]
    Compare --> T[Target Tracking]
    Compare --> Sc[Scheduled]
    Compare --> P[Predictive]
    
    M --> MLevel[Manual effort: High]
    S --> SLevel[Manual effort: Low<br/>Reactive]
    T --> TLevel[Manual effort: Very Low<br/>Continuous]
    Sc --> ScLevel[Manual effort: Medium<br/>Time-based]
    P --> PLevel[Manual effort: Very Low<br/>Proactive]
```

### Strategy Selection Matrix

| Strategy | Automation Level | Trigger | Best For |
|----------|----------------|---------|----------|
| **Manual** | None | Human action | One-time events |
| **Simple/Step** | Reactive | CloudWatch alarm | Simple thresholds |
| **Target Tracking** | Continuous | Target metric | CPU/memory at level |
| **Scheduled** | Time-based | Time schedule | Known patterns |
| **Predictive** | ML-based | Forecast | Complex patterns |

---

## IV. Integration Architecture

### Complete HA + Scalable Architecture

```mermaid
graph TD
    User[Users] --> R53[Route 53<br/>DNS]
    R53 --> LB[Elastic Load Balancer]
    
    subgraph Region["AWS Region"]
        LB --> ASG[Auto Scaling Group]
        
        subgraph AZ1["Availability Zone 1"]
            ASG --> I1[EC2 Instance 1]
        end
        
        subgraph AZ2["Availability Zone 2"]
            ASG --> I2[EC2 Instance 2]
        end
        
        subgraph AZ3["Availability Zone 3"]
            ASG --> I3[EC2 Instance 3]
        end
    end
    
    CW[CloudWatch<br/>Metrics & Alarms] -.->|Triggers| ASG
    CW -.->|Triggers| LB
```

### Health Check Flow

```mermaid
graph TD
    LB[Load Balancer] --> Check[Health Check]
    Check --> H1[Instance 1<br/>HEALTHY ✓]
    Check --> H2[Instance 2<br/>UNHEALTHY ✗]
    Check --> H3[Instance 3<br/>HEALTHY ✓]
    
    H2 -.->|Remove from LB| LB
    ASG[Auto Scaling Group] -.->|Replace unhealthy| New[Launch New Instance]
    New -.->|Register with LB| LB
```

---

## V. Load Balancer + ASG Benefits

```mermaid
mindmap
  root((LB + ASG Benefits))
    High Availability
      Multi-AZ deployment
      Health check replacement
      Automatic failover
    Scalability
      Auto scaling based on load
      Multiple scaling strategies
      Cost optimization
    Operational Excellence
      Managed service
      Automatic updates
      Integration with CloudWatch
    Security
      SSL termination
      Security groups integration
      WAF integration
```

---

## VI. Load Balancer Selection Guide

### Decision Tree

```mermaid
graph TD
    Start[Need Load Balancer] --> Q1{Application Type?}
    
    Q1 -->|HTTP/HTTPS<br/>Web App| ALB[Use Application Load Balancer]
    Q1 -->|TCP/UDP<br/>Extreme Performance| NLB[Use Network Load Balancer]
    Q1 -->|Layer 3<br/>Third-party appliances| GWLB[Use Gateway Load Balancer]
    Q1 -->|Legacy| CLB[Classic Load Balancer<br/>⚠️ RETIRED]
    
    ALB --> AFeature[Content-based routing<br/>Path/Host routing<br/>Container support]
    NLB --> NFeature[Static IP<br/>Ultra-high performance<br/>Millions of req/sec]
    GWLB --> GFeature[Transparent network gateway<br/>Third-party appliances]
```

### Recommendation Matrix

| Use Case | Recommended LB | Reason |
|----------|---------------|--------|
| **Web application (HTTP/HTTPS)** | ALB | Layer 7 features, content routing |
| **Microservices / Containers** | ALB | Container support, path routing |
| **Gaming / Real-time** | NLB | Ultra-low latency, static IP |
| **IoT / MQTT** | NLB | TCP support, performance |
| **Network appliances** | GWLB | Layer 3 transparency |
| **Hybrid architectures** | GWLB | Third-party integration |

---

## VII. ASG Configuration Components

```mermaid
graph TD
    ASGConfig[ASG Configuration] --> Min[Minimum Capacity]
    ASGConfig --> Max[Maximum Capacity]
    ASGConfig --> Desired[Desired Capacity]
    ASGConfig --> HealthCheck[Health Checks]
    ASGConfig --> Scaling[Scaling Policies]
    ASGConfig --> Launch[Launch Template/Config]
    
    Min --> MinEx[Minimum instances to maintain]
    Max --> MaxEx[Maximum instances allowed]
    Desired --> DesiredEx[Target number of instances]
    HealthCheck --> HEx[EC2 + ELB health checks]
    Scaling --> SEx[Scaling policies]
    Launch --> LEx[Instance configuration]
```

### Capacity Settings

| Setting | Description |
|---------|-------------|
| **Minimum** | Lowest number of instances always running |
| **Maximum** | Highest number of instances allowed |
| **Desired** | Target number at any time |

---

## Key Takeaways

### Load Balancers
1. **4 Types**: ALB (L7), NLB (L4), GWLB (L3), CLB (retired)
2. **Use cases**: ALB for HTTP/HTTPS, NLB for TCP/performance, GWLB for L3
3. **Benefits**: HA, health checks, SSL termination, single access point
4. **Managed service**: AWS handles maintenance, upgrades, HA

### Auto Scaling Groups
1. **Purpose**: Dynamic capacity adjustment
2. **Capabilities**: Scale out/in, health replacement, LB integration
3. **5 Strategies**: Manual, Simple/Step, Target Tracking, Scheduled, Predictive
4. **Integration**: Works with ELB, CloudWatch
5. **Cost optimization**: Run only necessary instances

### Together
1. **HA + Scalability**: Combine LB + ASG for robust architecture
2. **Multi-AZ**: Deploy across multiple Availability Zones
3. **Health checks**: Automatic replacement of unhealthy instances
4. **Scaling triggers**: CloudWatch metrics drive scaling decisions

---

## Next Steps

⬅️ Previous: [Scalability & HA](./15-scalability-high-availability.md) | ➡️ Next: [Route 53, RDS, and Aurora](./17-route53-rds-aurora.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*
