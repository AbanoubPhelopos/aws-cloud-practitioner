# Containers & Orchestration

> ⏱️ **Estimated Study Time:** 22 minutes  
> 🎯 **CCP Exam Weight:** ~5-8% (Domain 3: Cloud Technology & Services)

---

## The Big Picture

**Containers** revolutionized how applications are developed, deployed, and scaled. They provide lightweight, portable, and efficient isolation compared to traditional virtual machines. Understanding containers, Docker, and Kubernetes is increasingly important for modern cloud architectures.

---

## VMs vs Containers - Core Difference

```mermaid
graph TB
    subgraph VM["🖥️ Server Virtualization (VMs)"]
        VM1[VM 1] --> VM1A[App] --> VM1L[Libs] --> VM1OS[Guest OS Linux]
        VM2[VM 2] --> VM2A[App] --> VM2L[Libs] --> VM2OS[Guest OS Windows]
        VM3[VM 3] --> VM3A[App] --> VM3L[Libs] --> VM3OS[Guest OS Linux]
        VM1OS & VM2OS & VM3OS --> Hyper[Hypervisor<br/>ESXi, Hyper-V, KVM] --> VMHW[Physical Hardware]
    end
    
    subgraph Container["📦 OS Virtualization (Containers)"]
        C1[CTR 1] --> C1A[App] --> C1L[Libs]
        C2[CTR 2] --> C2A[App] --> C2L[Libs]
        C3[CTR 3] --> C3A[App] --> C3L[Libs]
        C1L & C2L & C3L --> CR[Container Runtime<br/>Docker, containerd]
        CR --> HostOS[Host OS<br/>Shared Linux Kernel] --> CHW[Physical Hardware]
    end
    
    style VM fill:#FFE6E6
    style Container fill:#E6F7E6
```

### Critical Distinction

```mermaid
graph LR
    SV[Server Virtualization] --> SVNote[Different OS types allowed<br/>Linux VM, Windows VM, macOS VM]
    OSV[OS Virtualization] --> OSVNote[Must share same kernel<br/>All Linux OR all Windows]
    
    style SV fill:#4DABF7
    style OSV fill:#FF6B6B,color:#fff
```

### VMs vs Containers Comparison

| Aspect | Virtual Machines | Containers |
|--------|------------------|------------|
| **Isolation Level** | Hardware-level (100%) | OS-level process (~80%) |
| **OS Flexibility** | Different OS per VM | Must share same kernel |
| **Resource Overhead** | Heavy (GBs per VM) | Light (MBs per container) |
| **Startup Time** | Minutes (OS boot) | Seconds (process start) |
| **Density** | 10-50 VMs per host | 100s-1000s per host |
| **Security Isolation** | Stronger | Weaker (improving) |
| **Best Use Cases** | Different OS, strong isolation, legacy | Microservices, cloud-native, CI/CD |

### Resource Efficiency Example

```
VM:        App (100MB) + OS (2GB) + Overhead = ~2.5 GB
Container: App (100MB) + Shared Libs (~50MB) = ~150 MB
                          ↑
                10-15x MORE EFFICIENT!
```

---

## Container Architecture

```mermaid
graph TD
    User[👤 User Space<br/>Applications] --> SysCall[System Calls]
    SysCall --> Kernel[🔧 Kernel Space<br/>Kernel Management]
    Kernel --> HW[🖥️ Hardware<br/>CPU, RAM, Disk, Network]
    
    User -.->|Containers share<br/>kernel| Kernel
    
    style User fill:#87CEEB
    style Kernel fill:#FF6B6B,color:#fff
```

### Container Isolation Mechanisms

```mermaid
graph TD
    Iso[🔒 Container Isolation] --> NS[🌐 Linux Namespaces<br/>Isolation]
    Iso --> CG[📊 Cgroups<br/>Resource Limits]
    
    NS --> NS1[PID Namespace<br/>Process isolation]
    NS --> NS2[Mount Namespace<br/>File system isolation]
    NS --> NS3[Network Namespace<br/>Network isolation]
    NS --> NS4[User Namespace<br/>User/group isolation]
    
    CG --> CG1[CPU allocation limits]
    CG --> CG2[Memory usage limits]
    CG --> CG3[I/O bandwidth limits]
    CG --> CG4[Priority management]
    
    style Iso fill:#FF9900,color:#000
    style NS fill:#4DABF7
    style CG fill:#51CF66,color:#fff
```

---

## Container Technology Evolution

```mermaid
timeline
    title Container Technology Evolution
    2000-2001 : FreeBSD Jails
              : First mainstream OS-level virtualization
    2004-2005 : Solaris Containers/Zones
              : Enterprise-grade isolation
    2008 : LXC
         : Brought containers to Linux (cgroups + namespaces)
    2013 : 🐳 Docker
         : Revolutionary moment - made containers mainstream
    2014-2015 : ☸️ Kubernetes
              : Google's container orchestration at scale
    2015-Present : Ecosystem
                  : Podman, containerd, CRI-O
```

### Why Docker Succeeded

```mermaid
mindmap
  root((Docker Success<br/>Factors))
    🛠️ Developer Experience
      Simple CLI
      Easy to use
    📦 Portability
      Build once, run anywhere
    💾 Layered Filesystem
      Efficient storage
    🌐 Docker Hub
      Centralized registry
    📝 Dockerfile
      Infrastructure as code
    🌟 Ecosystem
      Compose, Swarm
      Third-party integrations
    ⏰ Perfect Timing
      Microservices era
      DevOps movement
```

---

## Container Isolation Levels

```mermaid
graph TD
    Strength[🔒 Isolation Strength] --> VM[VMs: 100%]
    Strength --> Container[Containers: 80%]
    
    VM --> V100[Fully Isolated:<br/>Complete OS, Kernel,<br/>Hardware abstraction]
    VM --> VIndep[Independent:<br/>Own time, security,<br/>kernel functions]
    
    Container --> C80[Isolated:<br/>Processes, File Systems,<br/>Network, Users]
    Container --> CShared[Shared with Host:<br/>Kernel, System Time,<br/>Core Security]
    
    style VM fill:#51CF66,color:#fff
    style Container fill:#FFD700,color:#000
```

### Security Implication

```mermaid
graph TD
    Attack[⚔️ Kernel Attack] --> Kernel[Shared Kernel<br/>COMPROMISED]
    
    Kernel --> C1[Container 1<br/>❌ IMPACTED]
    Kernel --> C2[Container 2<br/>❌ IMPACTED]
    Kernel --> C3[Container 3<br/>❌ IMPACTED]
    
    style Attack fill:#FF6B6B,color:#fff
    style Kernel fill:#FF6B6B,color:#fff
    style C1 fill:#FF9999
    style C2 fill:#FF9999
    style C3 fill:#FF9999
```

> 🎯 **Exam Tip:** If the shared kernel is compromised, **ALL containers** on that host are impacted. VMs provide superior isolation because each has its own kernel.

---

## VM vs Micro VM vs Container

| Factor | Virtual Machine (VM) | Micro VM | Containers |
|--------|---------------------|----------|------------|
| **Creation Time** | 60-120 seconds | 15-30 seconds | 2-5 seconds |
| **Isolation** | 100% (Full) | 100% (Same as VM) | 80% (Medium) |
| **Memory Footprint** | ~2GB OS | ~300MB OS | ~50MB |
| **Use Cases** | Critical/Legacy Apps | Secure Microservices | Rapid Scaling |

### Use Case Selection

| Workload Type | Best Choice | Reason |
|---------------|-------------|--------|
| **Critical/Legacy Apps** (CRM, ERP, DB) | VM | Full isolation, security |
| **Secure Microservices** | Micro VM | VM isolation + container speed |
| **Rapid Scaling Microservices** | Container | Speed and scaling capability |

### VM vs Container Security Architecture

```mermaid
graph TD
    subgraph VM_Arch["🖥️ VM Architecture (100% Isolation)"]
        Host1[Physical Server] --> HW1[Hardware]
        HW1 --> Hyper1[Hypervisor Layer]
        Hyper1 --> VM_A[VM 1]
        Hyper1 --> VM_B[VM 2]
        Hyper1 --> VM_C[VM 3]
        VM_A --> VH1[Virtual Hardware]
        VM_A --> OS1[Own OS + Kernel]
        VM_B --> VH2[Virtual Hardware]
        VM_B --> OS2[Own OS + Kernel]
    end
    
    subgraph Container_Arch["📦 Container Architecture (80% Isolation)"]
        Host2[Physical Server] --> HW2[Hardware]
        HW2 --> SharedKernel[SHARED Kernel<br/>⚠️ Single point of failure]
        SharedKernel --> C1[Container 1<br/>App + Libs]
        SharedKernel --> C2[Container 2<br/>App + Libs]
        SharedKernel --> C3[Container 3<br/>App + Libs]
        SharedKernel --> NS[Linux Namespaces]
        SharedKernel --> CG[Cgroups]
    end
    
    style VM_Arch fill:#E6F7E6
    style Container_Arch fill:#FFE6E6
    style SharedKernel fill:#FF6B6B,color:#fff
```

### Attack Vector Comparison

```mermaid
graph TD
    VM_Attack[⚔️ Attack on VM 1] --> V1[VM 1 COMPROMISED]
    V1 -.->|Cannot bypass| Hyper[Hypervisor PROTECTED]
    Hyper -.->|Protects| V2[VM 2 SAFE ✅]
    Hyper -.->|Protects| V3[VM 3 SAFE ✅]
    
    Container_Attack[⚔️ Kernel Attack] --> CK[Shared Kernel COMPROMISED]
    CK --> C1[Container 1 IMPACTED ❌]
    CK --> C2[Container 2 IMPACTED ❌]
    CK --> C3[Container 3 IMPACTED ❌]
    
    style VM_Attack fill:#FFD700,color:#000
    style V1 fill:#FF6B6B,color:#fff
    style V2 fill:#51CF66,color:#fff
    style V3 fill:#51CF66,color:#fff
    style Container_Attack fill:#FF6B6B,color:#fff
    style CK fill:#FF6B6B,color:#fff
    style C1 fill:#FF9999
    style C2 fill:#FF9999
    style C3 fill:#FF9999
```

> ⚠️ **Security Warning:** If a vulnerability allows an attack on the shared Linux Kernel, **ALL containers** running on that OS are compromised. In VMs, each has its own kernel, containing the attack.

---

## Monolithic vs Microservices Architecture

### Monolithic Application (Example: Facebook)

```mermaid
graph TD
    Mono[Facebook Monolith<br/>Single Unified Codebase] --> Chat[Chat Module<br/>Messaging System]
    Mono --> Posts[Posts Module<br/>Content System]
    Mono --> Ads[Ads Module<br/>Advertisement System]
    Mono --> Reels[Reels Module<br/>Video System]
    
    Mono -.->|❌ SPOF| Fail[One bug crashes<br/>entire application]
    Mono -.->|❌ Scaling Issue| Diff[Difficult to scale<br/>individual features]
    
    style Mono fill:#FF6B6B,color:#fff
    style Fail fill:#FFD700,color:#000
```

### Microservices Architecture (Example: Facebook Rebuilt)

```mermaid
graph TD
    Micro[Microservices] --> CS[Chat Service<br/>Independent Deploy<br/>Auto-scalable<br/>Own Database]
    Micro --> PS[Posts Service<br/>Independent Deploy<br/>Auto-scalable<br/>Own Database]
    Micro --> AS[Ads Service<br/>Independent Deploy<br/>Auto-scalable<br/>Own Database]
    Micro --> RS[Reels Service<br/>Independent Deploy<br/>Auto-scalable<br/>Own Database]
    
    Micro -.->|✅ Fault Isolation| Good[Services fail independently<br/>Scale individually]
    
    style Micro fill:#51CF66,color:#fff
    style Good fill:#FFD700,color:#000
```

### Microservices Scaling Example

```mermaid
graph TD
    Scale[Microservices Auto-Scaling] --> C1[Chat Service<br/>3 instances<br/>Scaled from 1 → 3<br/>due to high demand]
    Scale --> P1[Posts Service<br/>1 instance<br/>Normal load]
    Scale --> M1[Media Service<br/>2 instances<br/>Scaled 1 → 2<br/>during peak hours]
    
    Scale --> Process[Auto-Scaling Process]
    Process --> M[Monitor: CPU, memory, requests]
    Process --> A[Analyze: Compare to thresholds]
    Process --> U[Scale UP: Add instances]
    Process --> D[Scale DOWN: Remove excess]
    Process --> B[Balance: Distribute traffic]
    
    style C1 fill:#FF6B6B,color:#fff
    style M1 fill:#FFA500
    style P1 fill:#51CF66,color:#fff
```

### Failure Impact Comparison

```mermaid
graph LR
    MonoFail[❌ Monolithic Failure<br/>100% Application Down<br/>One component crashes entire app]
    MicroFail[✅ Microservices Failure<br/>15% Application Down<br/>Only affected service goes down]
    
    style MonoFail fill:#FF6B6B,color:#fff
    style MicroFail fill:#51CF66,color:#fff
```

### Microservices Benefits

| Benefit | Description |
|---------|-------------|
| **Fault Isolation** | If one service fails, others remain operational |
| **Independent Scaling** | Scale specific services based on demand |
| **Technology Choice** | Different services can use different tech stacks |
| **Rapid Scaling** | Containers fit perfectly for microservices needs |

---

## Container Orchestration with Kubernetes

**Definition:** **Kubernetes (K8s)** is an open-source container orchestration platform that automates deployment, scaling, and management of containerized applications.

```mermaid
graph TD
    K8s[☸️ Kubernetes Cluster<br/>Container Orchestration Engine]
    
    K8s --> N1[Node 1 Worker]
    K8s --> N2[Node 2 Worker]
    
    N1 --> P1[Pod 1]
    N1 --> P2[Pod 2]
    N1 --> P3[Pod 3]
    
    N2 --> P4[Pod 4]
    N2 --> P5[Pod 5]
    N2 --> P6[Pod 6]
    
    K8s --> KFeatures[Key Features]
    KFeatures --> KF1[Auto-healing]
    KFeatures --> KF2[Auto-scaling]
    KFeatures --> KF3[Service Discovery]
    KFeatures --> KF4[Rolling Updates]
    
    style K8s fill:#FF9900,color:#000
    style KF1 fill:#51CF66,color:#fff
    style KF2 fill:#51CF66,color:#fff
    style KF3 fill:#4DABF7,color:#fff
    style KF4 fill:#4DABF7,color:#fff
```

### Kubernetes Key Features

| Feature | Description |
|---------|-------------|
| **Auto-healing** | Automatically restart failed containers |
| **Auto-scaling** | Scale pods based on demand |
| **Service Discovery** | Containers find each other automatically |
| **Rolling Updates** | Update without downtime |
| **Load Balancing** | Distribute traffic across pods |
| **Self-healing** | Replace unhealthy containers |

### Kubernetes Operations Flow

```mermaid
graph TD
    KOps[Kubernetes Process Flow] --> Deploy[1. Deploy<br/>Applications as pods<br/>across cluster nodes]
    Deploy --> Monitor[2. Monitor<br/>Continuously monitor<br/>pod health & resources]
    Monitor --> Scale[3. Auto-scale<br/>Scale pods based on demand]
    Scale --> Heal[4. Self-heal<br/>Restart failed pods<br/>Replace unhealthy containers]
    
    style KOps fill:#FF9900,color:#000
    style Deploy fill:#4DABF7,color:#fff
    style Monitor fill:#FFD700,color:#000
    style Scale fill:#51CF66,color:#fff
    style Heal fill:#FF6B6B,color:#fff
```

### Orchestration Tools Comparison

| Tool | Type | Scale |
|------|------|-------|
| **VMware vCenter** | VM Orchestration | Enterprise VM Management |
| **Docker Swarm** | Container Orchestration | Smaller scale |
| **Kubernetes (K8s)** | Container Orchestration | Most famous, large scale |

### VMware vCenter (Server Virtualization Management)

```mermaid
graph TD
    vCenter[VMware vCenter<br/>Centralized Management] --> VM1[VM1: Web Server<br/>8GB RAM]
    vCenter --> VM2[VM2: Database<br/>16GB RAM]
    vCenter --> VM3[VM3: App Server<br/>4GB RAM]
    
    vCenter --> Features[Key Features]
    Features --> F1[Live Migration]
    Features --> F2[High Availability]
    Features --> F3[Resource Pools]
    Features --> F4[DRS - Distributed<br/>Resource Scheduler]
    
    style vCenter fill:#FF9900,color:#000
    style F1 fill:#51CF66,color:#fff
    style F2 fill:#51CF66,color:#fff
```

> 📌 **Key Insight:** Management & Orchestration is the **third vital layer** beyond just hypervisors. Hypervisors alone cannot provide advanced features like live migration, failover, and HA.

---

## AWS Container Services

```mermaid
graph TD
    Containers[📦 AWS Container Services] --> ECS[🎛️ Amazon ECS<br/>Elastic Container Service]
    Containers --> EKS[☸️ Amazon EKS<br/>Elastic Kubernetes Service]
    Containers --> Fargate[🚀 AWS Fargate<br/>Serverless Containers]
    Containers --> ECR[📚 Amazon ECR<br/>Elastic Container Registry]
    
    ECS --> ECSUse[AWS-managed Docker<br/>orchestration]
    EKS --> EKSUse[Managed Kubernetes<br/>on AWS]
    Fargate --> FargateUse[Run containers<br/>without managing servers]
    ECR --> ECRUse[Store and manage<br/>Docker images]
    
    style Containers fill:#FF9900,color:#000
    style ECS fill:#4DABF7
    style EKS fill:#51CF66,color:#fff
    style Fargate fill:#FFD700,color:#000
    style ECR fill:#DDA0DD
```

### AWS Container Services Comparison

| Service | Purpose | Best For |
|---------|---------|----------|
| **Amazon ECS** | AWS-managed Docker orchestration | AWS-native container workloads |
| **Amazon EKS** | Managed Kubernetes service | Kubernetes workloads on AWS |
| **AWS Fargate** | Serverless compute for containers | Run containers without managing servers |
| **Amazon ECR** | Managed Docker container registry | Store, manage, and deploy container images |

---

## Container Security: Root vs Rootless

### The Root Privilege Problem

```mermaid
graph TD
    Problem[⚠️ Root Privilege Issue] --> Context[Container tools often<br/>require root daemon]
    Context --> Risk[Risk]
    Risk --> R1[Attack on container<br/>with root privileges]
    R1 --> R2[Could compromise kernel]
    R2 --> R3[All containers affected]
    
    style Problem fill:#FFD700,color:#000
    style R3 fill:#FF6B6B,color:#fff
```

### Docker vs Podman

```mermaid
graph LR
    Docker[🐳 Docker<br/>Requires root daemon] --> DIssue[⚠️ High-privilege attack risk]
    
    Podman[🦭 Podman<br/>Rootless by default] --> PBenefit[✅ Limited privileges<br/>Reduces kernel risk]
    
    style Docker fill:#FFCCCC
    style Podman fill:#CCFFCC
```

| Container Technology | Default Privilege | Security Implication |
|---------------------|-------------------|---------------------|
| **Docker** | Requires root daemon | High-privilege attack risk |
| **Podman (Red Hat)** | Rootless by default | Limited privileges, safer |

---

## Live Migration (Virtualization Feature)

**Live Migration** is the ability to transfer a running VM from one physical server to another **with minimal downtime** (2-5 seconds). This was impossible before virtualization.

```mermaid
graph TD
    Before[Pre-Virtualization<br/>Physically transport server<br/>via vehicle and airplane] -->|Old Way| Slow[Slow, downtime required]
    
    VM[Virtual Machine = Folder<br/>Collection of files<br/>virtual hardware, disk binaries] --> LM[Live Migration<br/>Transfer folder/files<br/>across network]
    
    LM --> Amazing[✨ Unprecedented Feature<br/>Video continues playing<br/>during transfer<br/>No interruption]
    
    style Before fill:#FF6B6B,color:#fff
    style VM fill:#4DABF7
    style LM fill:#51CF66,color:#fff
    style Amazing fill:#FFD700,color:#000
```

### Live Migration Sequence

```mermaid
graph LR
    Src[Source Server<br/>Cairo, Egypt<br/>Running VM<br/>High CPU Load<br/>Memory: 4GB Active]
    Dst[Target Server<br/>Frankfurt, Germany<br/>Ready to Receive<br/>Available Resources]
    
    Src -->|Step 1| PCopy[Pre-copy<br/>Memory Pages]
    PCopy -->|Step 2| Dirty[Dirty Pages<br/>Sync Changes]
    Dirty -->|Step 3| Stop[Stop & Copy<br/>Brief Pause<br/>2-5 seconds]
    Stop -->|Step 4| Resume[Resume<br/>Start Target VM<br/>Video continues<br/>Active processes]
    Resume --> Dst
    
    Src <-.->|10 Gbps Network| Dst
    
    style Src fill:#FFA500
    style Dst fill:#51CF66,color:#fff
    style Stop fill:#FF6B6B,color:#fff
```

### Live Migration Benefits

| Benefit | Description |
|---------|-------------|
| **Zero Downtime Maintenance** | Move VMs during hardware updates |
| **Load Balancing** | Distribute load across hosts |
| **Hardware Failure Recovery** | Move VM before host fails |
| **Geographic Optimization** | Move closer to users |
| **Seamless Experience** | No service interruption |

---

## House vs Apartment Analogy

```mermaid
graph TD
    Analogy[🏘️ Architecture Analogy] --> VM[VMs = Separate Houses]
    Analogy --> Container[Containers = Apartment Building]
    
    VM --> V1[Plot of land = Physical Server]
    VM --> V2[Plot Manager = Hypervisor]
    VM --> V3[Houses = Virtual Machines]
    VM --> V4[Fire in one house<br/>stays contained]
    
    Container --> C1[Building = Physical Server]
    Container --> C2[Foundation/Plumbing = Shared Kernel]
    Container --> C3[Apartments = Containers]
    Container --> C4[Structural failure<br/>affects all residents]
    
    style VM fill:#51CF66,color:#fff
    style Container fill:#FFD700,color:#000
```

---

## Container Security Best Practices

```mermaid
graph TD
    Best[🔒 Container Security Best Practices] --> B1[Avoid root privileges]
    Best --> B2[Use rootless containers]
    Best --> B3[Don't expose hosts to internet]
    Best --> B4[Regular kernel updates]
    Best --> B5[Use minimal base images]
    Best --> B6[Scan for vulnerabilities]
    Best --> B7[Network segmentation]
    
    style Best fill:#FF9900,color:#000
    style B1 fill:#51CF66,color:#fff
    style B2 fill:#51CF66,color:#fff
```

| Practice | Reason |
|----------|--------|
| **Avoid root privileges** | Prevents kernel-level attacks |
| **Use rootless containers** | Reduces attack surface (e.g., Podman) |
| **Don't expose hosts to internet** | Container hosts should only egress |
| **Regular kernel updates** | Patch kernel vulnerabilities |
| **Use minimal base images** | Reduce attack surface |
| **Scan for vulnerabilities** | Detect known issues |

---

## When to Use VMs vs Containers

```mermaid
flowchart TD
    Start{Choose VM or Container?} --> F1{Need different OS?}
    F1 -->|Yes| VM[🖥️ Use VMs]
    F1 -->|No| F2{Need strong isolation?}
    
    F2 -->|Yes, Critical| VM
    F2 -->|No| F3{Need rapid scaling?}
    
    F3 -->|Yes| Container[📦 Use Containers]
    F3 -->|No| F4{Need legacy support?}
    
    F4 -->|Yes| VM
    F4 -->|No| Either[Either works]
    
    style VM fill:#51CF66,color:#fff
    style Container fill:#FFD700,color:#000
```

### Decision Matrix

| Use Case | Recommendation | Reason |
|----------|---------------|--------|
| **Different OS requirements** | VMs | OS-level isolation |
| **Critical applications** (CRM, ERP) | VMs | Strong isolation, security |
| **Microservices** | Containers | Rapid scaling, efficiency |
| **CI/CD pipelines** | Containers | Fast startup, ephemeral |
| **Legacy applications** | VMs | OS flexibility, compatibility |
| **Development environments** | Containers | Quick provisioning |

---

## Hybrid Approaches (The Future)

| Technology | Description |
|------------|-------------|
| **Kubernetes on VMs** | Container orchestration on VM infrastructure |
| **OpenShift Virtualization** | Manage both VMs and containers in single platform |
| **Kata Containers** | Containers inside lightweight VMs for VM-level security |
| **Firecracker (AWS)** | Micro-VMs starting in milliseconds |

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **Containers** | Lightweight, OS-level virtualization, share kernel |
| **VMs** | Hardware-level virtualization, isolated kernels |
| **Isolation** | VMs = 100%, Containers = 80% |
| **Startup** | Containers = seconds, VMs = minutes |
| **Density** | 100s-1000s containers vs 10-50 VMs per host |
| **Docker** | Made containers mainstream (2013) |
| **Kubernetes** | Container orchestration at scale |
| **AWS ECS** | Managed Docker orchestration |
| **AWS EKS** | Managed Kubernetes |
| **AWS Fargate** | Serverless containers |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: What is the main difference between containers and virtual machines?</strong></summary>

**A.** Containers are more expensive  
**B.** Containers share the host kernel; VMs have their own kernels  
**C.** Containers require more memory  
**D.** Containers are slower to start  

**Answer: B** — Containers share the host operating system kernel and use OS-level virtualization (~80% isolation), while VMs have their own guest OS and kernels (100% isolation). This makes containers more lightweight but less isolated.
</details>

<details>
<summary><strong>Q2: What is Kubernetes?</strong></summary>

**A.** A container runtime  
**B.** A container orchestration platform that automates deployment, scaling, and management  
**C.** A virtual machine hypervisor  
**D.** A storage service  

**Answer: B** — Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications across clusters of hosts.
</details>

<details>
<summary><strong>Q3: Which AWS service provides serverless container execution without managing servers?</strong></summary>

**A.** Amazon EC2  
**B.** Amazon ECS  
**C.** AWS Fargate  
**D.** Amazon EKS  

**Answer: C** — AWS Fargate is a serverless compute engine for containers that works with both ECS and EKS. It allows you to run containers without provisioning or managing servers.
</details>

<details>
<summary><strong>Q4: What is the main security risk of containers compared to VMs?</strong></summary>

**A.** Containers are more expensive  
**B.** Containers share the host kernel, so a kernel vulnerability affects all containers  
**C.** Containers cannot be encrypted  
**D.** Containers require more network bandwidth  

**Answer: B** — Containers share the host OS kernel, so if the kernel is compromised (via a kernel vulnerability or privilege escalation), ALL containers on that host are affected. VMs have separate kernels, providing better isolation.
</details>

---

## Navigation

⬅️ Previous: [Virtualization Fundamentals](./06-virtualization-fundamentals.md) | ➡️ Next: [Security Fundamentals](../04-security-architecture/01-security-fundamentals.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*