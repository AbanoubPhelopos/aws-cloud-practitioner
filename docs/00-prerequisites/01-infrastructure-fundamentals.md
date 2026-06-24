# Infrastructure Prerequisites

> ⏱️ **Estimated Study Time:** 25 minutes  
> 🎯 **CCP Exam Weight:** ~5% (Foundational context for cloud adoption)

---

## The Big Picture

Before cloud computing existed, organizations ran their own **datacenters** with physical servers, networking equipment, and storage. Understanding this **pre-cloud infrastructure** is essential to appreciate *why* cloud computing is revolutionary. This module covers the foundational concepts: SPOF, fault tolerance, RAID, server types, clustering, and multi-tier architecture.

---

## 1. Application Environment Components

Every application requires a **complete ecosystem** to function — six essential layers:

```mermaid
graph TD
    AppEnv[📱 Application Environment] --> App[Application Layer<br/>Your Code]
    AppEnv --> Libs[Libraries & Dependencies]
    AppEnv --> Bin[Binaries & Supporting Software]
    AppEnv --> OS[Operating System Layer]
    AppEnv --> HW[Hardware Layer<br/>CPU | RAM | Disk]
    AppEnv --> Net[Networking]
    
    OS --> OS1[Linux]
    OS --> OS2[Windows]
    OS --> OS3[macOS]
    
    HW --> H1[CPU Processing]
    HW --> H2[Memory]
    HW --> H3[Storage]
    
    style AppEnv fill:#FF9900,color:#000
```

### Key Components

| Component | Purpose |
|-----------|---------|
| **Libraries** | Pre-written code collections for common functionality |
| **Dependencies** | Software packages required for the application to run |
| **Binaries** | Compiled executable programs and supporting software |
| **Operating System** | Software managing hardware and providing services |
| **Hardware** | Physical resources: CPU, RAM, Disk |
| **Networking** | Connectivity for external communication |

---

## 2. Functional vs Non-Functional Requirements

```mermaid
graph LR
    Req[Application Requirements] --> Func[⚙️ Functional]
    Req --> NonFunc[🛡️ Non-Functional]
    
    Func --> F1[What the app does]
    Func --> F2[UI, business logic, features]
    
    NonFunc --> NF1[How well it performs]
    NonFunc --> NF2[Speed, security, availability]
    NonFunc --> NF3[Infrastructure responsibility]
    
    style Func fill:#4DABF7
    style NonFunc fill:#FF6B6B,color:#fff
```

| Requirement Type | Description | Examples |
|------------------|-------------|----------|
| **Functional** | Define **what** the application does | User authentication, data processing, payments |
| **Non-Functional** | Define **how well** it performs | Security, performance, availability, scalability |

> ⚠️ **Critical:** Non-functional requirements are essential. Without them, even the best-designed application will fail. Users expect services to be **fast, secure, and always available**.

---

## 3. Single Point of Failure (SPOF)

A **Single Point of Failure** is any component that, if it fails, causes the **entire system to become unavailable**.

```mermaid
flowchart TD
    App[Application on PC] --> CPU[CPU]
    App --> RAM[RAM]
    App --> Disk[Disk]
    App --> PSU[Power Supply]
    
    CPU -.->|Failure| Fail1[System Down]
    RAM -.->|Failure| Fail2[System Down]
    Disk -.->|Failure| Fail3[System Down]
    PSU -.->|Failure| Fail4[System Down]
    
    Fail1 --> Down[Application Unavailable]
    Fail2 --> Down
    Fail3 --> Down
    Fail4 --> Down
    
    style Down fill:#FF6B6B,color:#fff
```

### Common Failure Scenarios

| Component | Failure Impact |
|-----------|---------------|
| **CPU** | Application can't process requests |
| **RAM** | System crashes or data corruption |
| **Storage** | Data loss and unavailability |
| **Power Supply** | Immediate system shutdown |
| **Network Interface** | Application becomes unreachable |

---

## 4. Fault Tolerance

**Fault Tolerance** is the system's ability to **continue operating during failures** through redundancy and intelligent failover.

### Two-Step Process

```mermaid
flowchart TD
    Step1[Step 1: Identify Fault Domain<br/>Find the SPOF] --> Q[Which component<br/>causes failure?]
    Q --> Example[Example: Single Hard Disk]
    Example --> Step2[Step 2: Implement Redundancy<br/>+ Automatic Failover]
    Step2 --> Solution[RAID Solution]
    Solution --> Result[System Continues Despite Failures]
    
    style Step1 fill:#FFD700,color:#000
    style Step2 fill:#51CF66,color:#fff
```

---

## 5. RAID (Redundant Array of Independent Disks)

**RAID** combines multiple physical disks into logical units for **redundancy and performance**.

### Common RAID Levels

```mermaid
graph LR
    RAID[RAID Levels] --> R0[RAID 0<br/>Striping]
    RAID --> R1[RAID 1<br/>Mirroring]
    RAID --> R5[RAID 5<br/>Striping + Parity]
    RAID --> R6[RAID 6<br/>Double Parity]
    RAID --> R10[RAID 10<br/>Mirror + Stripe]
    
    R0 --> R0D[⚡ Performance<br/>❌ No redundancy]
    R1 --> R1D[🛡️ Redundancy<br/>50% capacity lost]
    R5 --> R5D[⚖️ Balance<br/>1 disk for parity]
    R6 --> R6D[🛡️+ Extra protection<br/>2 disks for parity]
    R10 --> R10D[⭐ Best of both<br/>50% capacity lost]
    
    style R0 fill:#FFD700,color:#000
    style R1 fill:#51CF66,color:#fff
    style R5 fill:#4DABF7
    style R6 fill:#87CEEB
    style R10 fill:#FF6B6B,color:#fff
```

### RAID Comparison Table

| RAID Level | Description | Fault Tolerance | Capacity Penalty | Performance |
|------------|-------------|-----------------|------------------|-------------|
| **RAID 0** | Striping only | ❌ None | None | ⚡ Fastest |
| **RAID 1** | Mirroring | ✅ 1 disk failure | 50% lost | Good reads |
| **RAID 5** | Striping + Parity | ✅ 1 disk failure | 1 disk lost | Good |
| **RAID 6** | Double Parity | ✅ 2 disk failures | 2 disks lost | Good |
| **RAID 10** | Mirror + Stripe | ✅ Multiple failures | 50% lost | ⚡ Fastest |

### RAID 1 (Mirroring) Example

```mermaid
flowchart LR
    Write[Data Write Request] --> Controller[RAID Controller]
    Controller --> D1[Disk 1<br/>Mirror]
    Controller --> D2[Disk 2<br/>Mirror]
    Controller --> D3[Disk 3<br/>Failed ❌]
    
    D1 --> Data1[Data A + Data B]
    D2 --> Data2[Data A + Data B]
    
    style D3 fill:#FF6B6B,color:#fff
```

---

## 6. Server Types

Servers come in different physical forms optimized for specific environments.

```mermaid
graph LR
    Servers[Server Types] --> Tower[Tower]
    Servers --> Rack[Rack-Mount]
    Servers --> Blade[Blade]
    
    Tower --> T1[Highest Space]
    Tower --> T2[Lowest Cost]
    
    Rack --> R1[Medium Space]
    Rack --> R2[Medium Cost]
    
    Blade --> B1[Lowest Space]
    Blade --> B2[Highest Cost]
    
    style Tower fill:#51CF66,color:#fff
    style Rack fill:#4DABF7
    style Blade fill:#DDA0DD
```

### Server Type Comparison

| Type | Space | Cost | Cabling | Management | Best For |
|------|-------|------|---------|------------|----------|
| **Tower** | Highest | Lowest | Difficult | Individual KVM | Small businesses |
| **Rack-Mount** | Medium | Medium | Better | KVM Switch | Medium datacenters |
| **Blade** | Lowest | Highest | Easiest | Integrated KVM | Large datacenters |

> 📌 **U-Unit:** 1U = 1.75 inches (44.45mm) in height. A typical rack is 42U tall.

### Rack Server Example

```mermaid
graph TD
    Rack[42U Rack] --> U1[1U - Server 1]
    Rack --> U2[2U - Server 2]
    Rack --> U3[1U - Server 3]
    Rack --> U4[4U - Storage Array]
    Rack --> KVM[KVM Switch<br/>Centralized Management]
    
    style Rack fill:#FF9900,color:#000
```

---

## 7. Server Components: ECC Memory & Redundant PSUs

### ECC Memory (Error-Correcting Code)

```mermaid
flowchart TD
    Normal[Normal Operation] --> Read1[Read Address 0x1000]
    Read1 --> Check1[CPU reads + verifies ECC]
    Check1 --> Pass[✓ ECC Check PASSED]
    Pass --> Good[Data is good]
    
    Error[Error Detection] --> Read2[Read Address 0x2000]
    Read2 --> Check2[CPU reads + verifies ECC]
    Check2 --> Fail[✗ ECC Check FAILED]
    Fail --> Mark[CPU + OS mark as BAD]
    Mark --> Continue[System continues<br/>without bad address]
    
    style Pass fill:#51CF66,color:#fff
    style Fail fill:#FF6B6B,color:#fff
```

> ⚠️ Normal PCs can't detect "bad addresses" in RAM, causing **Blue Screen of Death** (Windows) or **Purple Screen of Death** (VMware).

### Redundant Power Supplies

```mermaid
flowchart TD
    Power[AC Power Input] --> P1[PSU 1 Active]
    Power --> P2[PSU 2 Active]
    Power --> P3[PSU 3 Failed]
    
    P1 --> Components[Server Components<br/>Still Powered]
    P2 --> Components
    P3 -.->|Failed| X[✗]
    
    Components --> HotSwap[Hot-swappable<br/>replacement]
    
    style P3 fill:#FF6B6B,color:#fff
    style Components fill:#51CF66,color:#fff
```

---

## 8. Remote Management: IPMI / iLO / iDRAC

Remote management allows administrators to manage servers **from anywhere in the world**.

```mermaid
graph LR
    Admin[Admin in Cairo] -->|HTTPS + VPN| Internet((Internet))
    Internet -->|HTTPS + VPN| Server[Server in Germany]
    
    Server --> IPMI[IPMI Card<br/>iLO/iDRAC]
    IPMI --> BIOS[Server BIOS]
    IPMI --> CPU[CPU/RAM/Storage]
    
    Admin --> Tasks[Admin Tasks]
    Tasks --> T1[Power On/Off]
    Tasks --> T2[Access BIOS]
    Tasks --> T3[Mount Virtual CD/DVD]
    Tasks --> T4[Install OS Remotely]
    
    style Admin fill:#FFD700,color:#000
    style IPMI fill:#FF9900,color:#000
```

### IPMI Vendor Implementations

| Vendor | Implementation |
|--------|---------------|
| **HP** | iLO (Integrated Lights-Out) |
| **Dell** | iDRAC (Dell Remote Access Controller) |
| **IBM** | IMM (Integrated Management Module) |
| **Cisco** | CIMC (Cisco Integrated Management Controller) |

> ⚠️ **Security:** Never expose IPMI interfaces directly to the internet. Always use VPN or secure internal networks.

---

## 9. Server Clustering

**Clustering** connects multiple devices to work together for redundancy and performance.

```mermaid
graph TD
    Cluster[Clustering Types] --> HA[High Availability<br/>Failover]
    Cluster --> LB[Load Balancing]
    Cluster --> HPC[High-Performance<br/>Computing]
    
    HA --> H1[Active/Passive]
    HA --> H2[Heartbeat Monitoring]
    HA --> H3[Data Replication]
    
    LB --> L1[Active/Active]
    LB --> L2[Traffic Distribution]
    LB --> L3[Health Checks]
    
    HPC --> HP1[Intensive Computing]
    HPC --> HP2[Parallel Processing]
    
    style HA fill:#FF6B6B,color:#fff
    style LB fill:#4DABF7
    style HPC fill:#FFD700,color:#000
```

### Clustering Comparison

| Type | Configuration | Purpose | Best For |
|------|--------------|---------|----------|
| **HA/Failover** | Active/Passive | Redundancy | Databases |
| **Load Balancing** | Active/Active | Distribute load | Web servers |
| **HPC** | Parallel | Intensive computation | Scientific apps |

---

## 10. Load Spike & DDoS Scenarios

### Load Spike Challenges

```mermaid
graph TD
    Normal[Normal Traffic] --> Spike[High Load Events]
    Spike --> BF[Black Friday]
    Spike --> Sale[Sales Events]
    Spike --> Peak[Traffic Peaks]
    
    Peak --> Solution[Solution: Load Balancing]
    Solution --> S1[Add more servers]
    Solution --> S2[Distribute load]
    Solution --> S3[Scale horizontally]
    
    style Spike fill:#FF6B6B,color:#fff
    style Solution fill:#51CF66,color:#fff
```

### DoS vs DDoS Attacks

```mermaid
graph TD
    Attack[Attack Types] --> DoS[DoS Attack]
    Attack --> DDoS[DDoS Attack]
    
    DoS --> D1[Single Attacker]
    D1 --> D2[10,000 connections]
    D2 --> D3[Server overloaded]
    D3 --> D4[Solution: Block IP]
    
    DDoS --> DD1[Botnet of<br/>Compromised Computers]
    DD1 --> DD2[1000+ connections each]
    DD2 --> DD3[Hard to distinguish<br/>from legitimate traffic]
    DD3 --> DD4[Solution: WAF +<br/>DDoS Protection]
    
    style DoS fill:#FFD700,color:#000
    style DDoS fill:#FF6B6B,color:#fff
```

> ⚠️ **Critical:** Adding more servers will **NOT solve a DDoS attack** — you need specialized protection like WAF or DDoS protection services.

---

## 11. Multi-Tier Architecture

Putting all components on a single server creates an obvious SPOF. The solution is **Multi-Tier Architecture**.

```mermaid
graph TD
    Internet[Internet] --> Pres[Presentation Tier]
    Pres --> App[Application Tier]
    App --> DB[Database Tier]
    
    Pres --> P1[Reverse Proxy]
    Pres --> P2[Web Application Firewall]
    Pres --> P3[Load Balancer]
    
    App --> A1[Load Balancer]
    App --> A2[App Servers 1-6]
    
    DB --> D1[Primary Database]
    DB --> D2[Secondary Database]
    DB --> D3[Read Replica]
    DB --> D4[Backup Database]
    
    style Internet fill:#FFD700,color:#000
    style Pres fill:#FF6B6B,color:#fff
    style App fill:#4DABF7
    style DB fill:#51CF66,color:#fff
```

### Tier Functions and Security Zones

| Tier | Network Exposure | Primary Function | Clustering Type |
|------|-----------------|------------------|-----------------|
| **Presentation** | Internet-Exposed | Security, Traffic Management | Load Balancing |
| **Application** | Internal Network | Business Logic Processing | Load Balancing |
| **Database** | Most Secure Network | Data Storage & Management | HA/Replication |

### Multi-Tier Benefits

```mermaid
graph TD
    Benefits[Multi-Tier Benefits] --> B1[Eliminates SPOFs]
    Benefits --> B2[Security through segmentation]
    Benefits --> B3[Independent scaling]
    Benefits --> B4[Specialized optimization]
    
    style Benefits fill:#FF9900,color:#000
```

---

## 12. Cost of High Availability

### High Availability Math

```mermaid
graph TD
    Service[One Digital Service] --> AL[Application Layer: 2 servers]
    Service --> DL[Database Layer: 2 servers]
    
    AL --> Total[Total: 4 servers<br/>minimum per service]
    DL --> Total
    
    Total --> Scale[Cost Scale]
    Scale --> S1[1 service = 4 servers]
    Scale --> S2[5 services = 20 servers]
    Scale --> S3[10 services = 40 servers]
    Scale --> S4[50 services = 200 servers]
    
    style Total fill:#FF6B6B,color:#fff
```

### The Utilization Problem

```mermaid
graph TD
    Util[Server Utilization Crisis] --> Wasted[70-85% WASTED]
    Util --> Used[15-25% USED]
    
    Wasted --> P1[Financial waste]
    Wasted --> P2[Oversized infrastructure]
    Wasted --> P3[High operational costs]
    Wasted --> P4[Environmental impact]
    
    style Wasted fill:#FF6B6B,color:#fff
    style Used fill:#51CF66,color:#fff
```

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **SPOF** | Any component whose failure brings down the entire system |
| **Fault Tolerance** | System continues operating despite failures |
| **RAID** | Redundant Array of Independent Disks (RAID 0, 1, 5, 6, 10) |
| **ECC Memory** | Detects and corrects memory errors |
| **Server Types** | Tower (cheap), Rack (balanced), Blade (dense) |
| **IPMI/iLO/iDRAC** | Remote server management |
| **Clustering** | HA (Active/Passive), LB (Active/Active), HPC (Parallel) |
| **Multi-Tier** | Presentation → Application → Database |
| **HA Cost** | 4 servers minimum per service |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: What is a Single Point of Failure (SPOF)?</strong></summary>

**A.** A server with high availability  
**B.** Any component that, if it fails, causes the entire system to become unavailable  
**C.** A type of RAID configuration  
**D.** A clustering technique  

**Answer: B** — A SPOF is any component (CPU, RAM, disk, network interface, power supply) whose failure would bring down the entire system. Eliminating SPOFs requires redundancy.
</details>

<details>
<summary><strong>Q2: Which RAID level provides the BEST performance AND redundancy?</strong></summary>

**A.** RAID 0  
**B.** RAID 1  
**C.** RAID 5  
**D.** RAID 10  

**Answer: D** — RAID 10 combines mirroring (RAID 1) and striping (RAID 0), providing both excellent performance and redundancy. The trade-off is 50% capacity loss (like RAID 1).
</details>

<details>
<summary><strong>Q3: What is the minimum number of servers needed for high availability of one service?</strong></summary>

**A.** 1 server  
**B.** 2 servers  
**C.** 4 servers  
**D.** 10 servers  

**Answer: C** — For high availability, you need a minimum of 4 servers per service: 2 application servers (for load balancing/failover) and 2 database servers (for primary/standby or replication).
</details>

<details>
<summary><strong>Q4: What is the purpose of ECC memory?</strong></summary>

**A.** Faster data access  
**B.** Detecting and correcting memory errors  
**C.** Encrypting data in memory  
**D.** Increasing memory capacity  

**Answer: B** — ECC (Error-Correcting Code) memory detects and corrects single-bit memory errors, preventing system crashes and data corruption that would otherwise cause Blue/Purple Screen of Death errors.
</details>

---

## Navigation

⬅️ Previous: (Start) | ➡️ Next: [Introduction to Cloud Computing](../01-cloud-fundamentals/01-introduction-to-cloud.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*