# AWS Networking Services

> ⏱️ **Estimated Study Time:** 18 minutes  
> 🎯 **CCP Exam Weight:** ~10% (Domain 3: Cloud Technology & Services)

---

## The Big Picture

AWS provides a comprehensive suite of **networking services** that enable secure, scalable, and high-performance communication between resources and end users. Understanding VPC, Route 53, CloudFront, and Direct Connect is essential for designing cloud architectures.

---

## AWS Networking Services Overview

```mermaid
graph TD
    Net[🌐 AWS Networking] --> VPC[🔒 Amazon VPC<br/>Virtual Private Cloud]
    Net --> Route53[🗺️ Route 53<br/>DNS Service]
    Net --> CF[⚡ CloudFront<br/>CDN]
    Net --> DX[🔌 Direct Connect<br/>Dedicated Connection]
    Net --> VPN[🔐 VPN<br/>Secure Tunnel]
    Net --> TGW[🌍 Transit Gateway<br/>Network Hub]
    
    VPC --> V1[Isolated network<br/>in AWS]
    Route53 --> R1[Domain name<br/>resolution]
    CF --> C1[Content delivery<br/>at edge]
    DX --> D1[Dedicated network<br/>connection]
    VPN --> VPN1[Encrypted<br/>connection over internet]
    TGW --> T1[Connect multiple<br/>VPCs and networks]
    
    style Net fill:#FF9900,color:#000
```

---

## 1. Amazon VPC (Virtual Private Cloud)

**Definition:** Your **logically isolated virtual network** in AWS where you launch AWS resources. You have complete control over the network environment.

```mermaid
graph TB
    subgraph VPC["🔒 VPC (10.0.0.0/16)"]
        subgraph Subnet1["📁 Public Subnet (10.0.1.0/24)"]
            Web1[🌐 Web Server 1]
            Web2[🌐 Web Server 2]
            IGW[🌍 Internet Gateway]
        end
        
        subgraph Subnet2["📁 Private Subnet (10.0.2.0/24)"]
            App1[⚙️ App Server 1]
            App2[⚙️ App Server 2]
        end
        
        subgraph Subnet3["📁 Database Subnet (10.0.3.0/24)"]
            DB1[(🗄️ Database 1)]
            DB2[(🗄️ Database 2)]
        end
        
        IGW --> Web1
        IGW --> Web2
        Web1 --> App1
        Web2 --> App2
        App1 --> DB1
        App2 --> DB2
    end
    
    style VPC fill:#FFE6E6
    style Subnet1 fill:#E6F3FF
    style Subnet2 fill:#FFF4E6
    style Subnet3 fill:#E6FFE6
```

### VPC Components

| Component | Purpose |
|-----------|---------|
| **Subnets** | Logical subdivisions of VPC (public or private) |
| **Route Tables** | Control traffic flow between subnets and gateways |
| **Internet Gateway** | Enables internet access for public subnets |
| **NAT Gateway** | Allows private subnets to access internet (outbound only) |
| **Security Groups** | Instance-level firewall (stateful) |
| **Network ACLs** | Subnet-level firewall (stateless) |
| **VPC Peering** | Connect two VPCs privately |

### Public vs Private Subnets

| Feature | Public Subnet | Private Subnet |
|---------|--------------|----------------|
| **Internet Access** | Direct (via Internet Gateway) | Outbound only (via NAT) |
| **Public IP** | Required for inbound | Not required |
| **Use Case** | Web servers, load balancers | Databases, backend servers |
| **Route to IGW** | Yes | No (except via NAT) |

### Security Groups vs Network ACLs

```mermaid
graph LR
    SG[🔒 Security Groups<br/>Instance-level<br/>Stateful] --> S1[Allow rules only<br/>Return traffic auto-allowed]
    NACL[🛡️ Network ACLs<br/>Subnet-level<br/>Stateless] --> N1[Allow and Deny rules<br/>Return traffic must be explicit]
    
    style SG fill:#51CF66,color:#fff
    style NACL fill:#4DABF7,color:#fff
```

| Feature | Security Groups | Network ACLs |
|---------|----------------|--------------|
| **Level** | Instance | Subnet |
| **State** | Stateful | Stateless |
| **Rules** | Allow only | Allow and Deny |
| **Default** | Deny inbound, allow outbound | Allow all (default NACL) |
| **Association** | Multiple instances | One per subnet |

---

## 2. Route 53 (DNS Service)

**Definition:** AWS's **scalable DNS (Domain Name System)** web service that routes user requests to AWS infrastructure or external endpoints.

```mermaid
graph TD
    User[👤 User types<br/>example.com] --> R53[🗺️ Route 53<br/>DNS Service]
    R53 -->|Resolves to| ELB[⚖️ Load Balancer]
    R53 -->|Resolves to| S3[📦 S3 Website]
    R53 -->|Resolves to| EC2[🖥️ EC2 Instance]
    
    R53 -->|Features| F1[Domain Registration]
    R53 -->|Features| F2[DNS Routing]
    R53 -->|Features| F3[Health Checks]
    R53 -->|Features| F4[Traffic Flow]
    
    style R53 fill:#FF9900,color:#000
```

### Route 53 Routing Policies

```mermaid
graph TD
    Policies[🗺️ Route 53 Routing Policies] --> Simple[🎯 Simple<br/>One record, multiple IPs]
    Policies --> Weighted[⚖️ Weighted<br/>Percentage-based]
    Policies --> Latency[⏱️ Latency-based<br/>Lowest latency Region]
    Policies --> Failover[🔄 Failover<br/>Active/Passive]
    Policies --> Geolocation[🌍 Geolocation<br/>User location]
    Policies --> Geoproximity[📍 Geoproximity<br/>Bias toward Region]
    Policies --> MultiValue[🎲 Multi-value<br/>Up to 8 healthy records]
    
    style Policies fill:#FF9900,color:#000
    style Simple fill:#4DABF7
    style Weighted fill:#FF6B6B,color:#fff
    style Latency fill:#FFD700,color:#000
    style Failover fill:#51CF66,color:#fff
    style Geolocation fill:#87CEEB
    style Geoproximity fill:#DDA0DD
    style MultiValue fill:#FFA500
```

### Routing Policy Use Cases

| Policy | Use Case |
|--------|----------|
| **Simple** | Single resource, no special routing |
| **Weighted** | A/B testing, gradual migration (10% to new, 90% to old) |
| **Latency** | Route users to lowest-latency Region |
| **Failover** | Active-passive disaster recovery |
| **Geolocation** | Localized content based on user location |
| **Multi-Value** | Return up to 8 healthy records for load balancing |

### Health Checks

```mermaid
graph LR
    HC[🏥 Route 53 Health Check] --> Monitor[Monitor Endpoint]
    Monitor --> Status{Endpoint<br/>Healthy?}
    Status -->|Yes| Route[Route Traffic]
    Status -->|No| Failover[Failover to<br/>Backup]
    
    style HC fill:#FF9900,color:#000
    style Route fill:#51CF66,color:#fff
    style Failover fill:#FF6B6B,color:#fff
```

> 🎯 **Exam Tip:** Route 53 is named after **Port 53**, the DNS port. It's AWS's DNS service that also supports domain registration.

---

## 3. CloudFront (CDN)

**Definition:** **Content Delivery Network (CDN)** that delivers content (websites, videos, APIs) from **400+ edge locations** worldwide with low latency.

```mermaid
graph LR
    User[👤 User in Tokyo] -->|Request| Edge1[📡 Edge Location<br/>Tokyo]
    User2[👤 User in London] -->|Request| Edge2[📡 Edge Location<br/>London]
    
    Edge1 -->|Cache miss| S3[📦 S3 Bucket<br/>Origin]
    Edge2 -->|Cache hit| Cached[💾 Cached Content]
    
    S3 -.->|First request| Edge1
    Edge1 -.->|Cache for future| Edge1
    
    style User fill:#FFD700,color:#000
    style User2 fill:#FFD700,color:#000
    style Edge1 fill:#51CF66,color:#fff
    style Edge2 fill:#51CF66,color:#fff
    style S3 fill:#FF9900,color:#000
    style Cached fill:#4DABF7,color:#fff
```

### CloudFront Features

| Feature | Description |
|---------|-------------|
| **Edge Locations** | 400+ globally for low latency |
| **Caching** | Content cached at edge for fast delivery |
| **HTTPS** | Secure content delivery |
| **Geo-restriction** | Restrict content by geographic location |
| **Signed URLs** | Time-limited access to private content |
| **Lambda@Edge** | Run code at edge locations |
| **Origin Shield** | Additional caching layer to reduce origin load |

### CloudFront Origins

```mermaid
graph TD
    CF[⚡ CloudFront] --> O1[📦 S3 Bucket]
    CF --> O2[🖥️ EC2 Instance]
    CF --> O3[⚖️ Load Balancer]
    CF --> O4[🌐 Custom Origin<br/>On-premises server]
    
    style CF fill:#FF9900,color:#000
    style O1 fill:#4DABF7
    style O2 fill:#FF6B6B
    style O3 fill:#FFD700,color:#000
    style O4 fill:#51CF66,color:#fff
```

> 🎯 **Exam Tip:** CloudFront integrates seamlessly with S3 for static content delivery. Use **Origin Access Identity (OAI)** to securely serve private S3 content via CloudFront.

---

## 4. Direct Connect

**Definition:** Dedicated **private network connection** between your datacenter and AWS, bypassing the public internet.

```mermaid
graph LR
    DC[🏢 Your Datacenter] -->|Dedicated<br/>fiber| DX[🔌 Direct Connect<br/>Location]
    DX -->|Private<br/>connection| VPC[🔒 AWS VPC]
    
    Internet((🌍 Internet)) -.->|Public| DC
    Internet -.->|Public| VPC
    
    style DC fill:#FF6B6B,color:#fff
    style DX fill:#FFD700,color:#000
    style VPC fill:#FF9900,color:#000
```

### Direct Connect Benefits

| Benefit | Description |
|---------|-------------|
| **Reduced Bandwidth Costs** | Predictable pricing vs internet egress |
| **Consistent Performance** | Dedicated connection, not shared |
| **Enhanced Security** | Private connection, no internet traversal |
| **Lower Latency** | Direct path to AWS |
| **Higher Throughput** | 1 Gbps, 10 Gbps, or 100 Gbps options |

### Direct Connect vs VPN

| Feature | Direct Connect | Site-to-Site VPN |
|---------|---------------|------------------|
| **Connection** | Dedicated fiber | Encrypted over internet |
| **Setup Time** | Weeks to months | Minutes |
| **Bandwidth** | 1-100 Gbps | Up to 1.25 Gbps per tunnel |
| **Latency** | Consistent | Variable |
| **Cost** | Higher upfront, lower per-GB | Lower upfront, higher per-GB |
| **Best For** | High throughput, consistent performance | Quick setup, low to moderate bandwidth |

---

## 5. Site-to-Site VPN

**Definition:** **Encrypted IPsec tunnel** over the public internet connecting your on-premises network to your VPC.

```mermaid
graph LR
    DC[🏢 On-Premises<br/>Datacenter] -->|IPsec<br/>VPN Tunnel| VPC[🔒 AWS VPC]
    
    Internet((🌍 Internet)) -.->|Encrypted<br/>traffic| DC
    Internet -.->|Encrypted<br/>traffic| VPC
    
    style DC fill:#FF6B6B,color:#fff
    style VPC fill:#FF9900,color:#000
```

### VPN Use Cases

- **Quick connectivity** without waiting for Direct Connect
- **Backup** for Direct Connect (redundancy)
- **Low to moderate bandwidth** requirements
- **Temporary connections** during migration

---

## 6. Transit Gateway

**Definition:** **Network transit hub** that connects multiple VPCs and on-premises networks through a single gateway.

```mermaid
graph TD
    TGW[🌍 Transit Gateway<br/>Central Hub]
    
    TGW --> VPC1[🔒 VPC 1<br/>Production]
    TGW --> VPC2[🔒 VPC 2<br/>Development]
    TGW --> VPC3[🔒 VPC 3<br/>Testing]
    TGW --> DX[🔌 Direct Connect<br/>On-premises]
    TGW --> VPN[🔐 VPN<br/>Branch office]
    
    style TGW fill:#FF9900,color:#000
    style VPC1 fill:#4DABF7
    style VPC2 fill:#FF6B6B,color:#fff
    style VPC3 fill:#51CF66,color:#fff
```

### Transit Gateway Benefits

| Benefit | Description |
|---------|-------------|
| **Simplified Network** | Connect many VPCs through one hub |
| **Scalable** | Supports thousands of VPCs |
| **Centralized Routing** | Single point of routing control |
| **Cross-Region Peering** | Connect VPCs across Regions |
| **Cost-Effective** | Reduces VPC peering complexity |

---

## Load Balancer Overview

```mermaid
graph TD
    LB[⚖️ AWS Load Balancers] --> ALB[🌐 Application Load Balancer<br/>Layer 7]
    LB --> NLB[🚀 Network Load Balancer<br/>Layer 4]
    LB --> GWLB[🔀 Gateway Load Balancer<br/>Layer 3]
    
    ALB --> ALBUse[HTTP/HTTPS<br/>Content-based routing]
    NLB --> NLBUse[TCP/UDP<br/>Ultra-high performance]
    GWLB --> GWLBUse[Layer 3<br/>Third-party appliances]
    
    style LB fill:#FF9900,color:#000
    style ALB fill:#4DABF7
    style NLB fill:#FF6B6B,color:#fff
    style GWLB fill:#FFD700,color:#000
```

### Load Balancer Comparison

| Feature | ALB | NLB | GWLB |
|---------|-----|-----|------|
| **Layer** | 7 (Application) | 4 (Transport) | 3 (Network) |
| **Protocol** | HTTP/HTTPS | TCP, UDP, TLS | IP |
| **Use Case** | Web apps, microservices | Gaming, IoT, real-time | Network appliances, firewalls |
| **Performance** | High | Very high (millions of req/sec) | High |
| **Static IP** | No | Yes | Yes |
| **Content Routing** | Yes (path, host, headers) | No | No |

> 🎯 **Exam Tip:** Choose **ALB for HTTP/HTTPS web applications**, **NLB for extreme performance/TCP**, and **GWLB for third-party network appliances**.

---

## Networking Decision Flowchart

```mermaid
flowchart TD
    Start{Need networking<br/>solution?} --> Q1{Isolated network<br/>in AWS?}
    
    Q1 -->|Yes| VPC[🔒 Use VPC<br/>Define subnets, routing]
    Q1 -->|No| Q2{Need DNS or<br/>domain registration?}
    
    Q2 -->|Yes| Route53[🗺️ Use Route 53<br/>DNS and routing]
    Q2 -->|No| Q3{Need global<br/>content delivery?}
    
    Q3 -->|Yes| CF[⚡ Use CloudFront<br/>CDN with edge caching]
    Q3 -->|No| Q4{Need dedicated<br/>connection?}
    
    Q4 -->|Yes| DX[🔌 Use Direct Connect<br/>High bandwidth, consistent]
    Q4 -->|No| VPN[🔐 Use VPN<br/>Quick encrypted connection]
    
    style VPC fill:#FF9900,color:#000
    style Route53 fill:#FF9900,color:#000
    style CF fill:#FF9900,color:#000
    style DX fill:#FF9900,color:#000
    style VPN fill:#FF9900,color:#000
```

---

## Quick Reference

| Service | Purpose | Best For |
|---------|---------|----------|
| **VPC** | Isolated virtual network | All AWS resources |
| **Route 53** | DNS and domain registration | Domain routing, health checks |
| **CloudFront** | CDN for global content delivery | Static/dynamic content, APIs |
| **Direct Connect** | Dedicated private connection | High bandwidth, consistent performance |
| **VPN** | Encrypted internet tunnel | Quick connectivity, low-moderate bandwidth |
| **Transit Gateway** | Network hub for multiple VPCs | Complex multi-VPC architectures |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: What is the main purpose of an Amazon VPC?</strong></summary>

**A.** Object storage  
**B.** Logically isolated virtual network in AWS  
**C.** Content delivery network  
**D.** DNS service  

**Answer: B** — An Amazon VPC (Virtual Private Cloud) is your logically isolated virtual network in AWS where you launch resources. You have complete control over the network configuration.
</details>

<details>
<summary><strong>Q2: Which Route 53 routing policy routes users to the Region with the lowest latency?</strong></summary>

**A.** Weighted  
**B.** Geolocation  
**C.** Latency-based  
**D.** Failover  

**Answer: C** — Latency-based routing routes users to the AWS Region that provides the lowest network latency for their location, improving performance.
</details>

<details>
<summary><strong>Q3: What is the main benefit of using CloudFront?</strong></summary>

**A.** Reduce latency by caching content at edge locations  
**B.** Store unlimited data  
**C.** Run serverless functions  
**D.** Manage DNS records  

**Answer: A** — CloudFront is a CDN that caches content at 400+ edge locations worldwide, reducing latency by serving content from the location closest to users.
</details>

<details>
<summary><strong>Q4: When should you use Direct Connect instead of VPN?</strong></summary>

**A.** When you need a quick temporary connection  
**B.** When you need high bandwidth and consistent performance  
**C.** When you have no on-premises infrastructure  
**D.** When you want to use the public internet  

**Answer: B** — Direct Connect provides a dedicated, private fiber connection with consistent high bandwidth (1-100 Gbps) and lower latency. Use it when you need high throughput and consistent performance. VPN is better for quick setup and lower bandwidth needs.
</details>

---

## Navigation

⬅️ Previous: [Storage Services](./02-storage.md) | ➡️ Next: [Databases](./04-databases.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*