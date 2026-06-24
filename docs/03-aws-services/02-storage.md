# AWS Storage Services

> ⏱️ **Estimated Study Time:** 20 minutes  
> 🎯 **CCP Exam Weight:** ~10% (Domain 3: Cloud Technology & Services)

---

## The Big Picture

AWS offers **multiple storage services** optimized for different use cases — from object storage to block storage to file systems. Understanding when to use each service is critical for designing efficient, cost-effective applications and is frequently tested on the CCP exam.

---

## Storage Service Categories

```mermaid
graph TD
    Storage[💾 AWS Storage Services] --> Object[📦 Object Storage<br/>S3]
    Storage --> Block[🧱 Block Storage<br/>EBS, Instance Store]
    Storage --> File[📁 File Storage<br/>EFS, FSx]
    Storage --> Archive[🗄️ Archive Storage<br/>S3 Glacier]
    
    Object --> O1[Unlimited files<br/>HTTP access]
    Block --> B1[EC2 attached<br/>Low latency]
    File --> F1[Shared file system<br/>Multiple instances]
    Archive --> A1[Long-term backup<br/>Cheap storage]
    
    style Storage fill:#FF9900,color:#000
    style Object fill:#4DABF7
    style Block fill:#FF6B6B
    style File fill:#51CF66
    style Archive fill:#FFD700,color:#000
```

---

## Storage Service Overview

| Service | Type | Use Case | Access Pattern |
|---------|------|----------|----------------|
| **Amazon S3** | Object Storage | Websites, backups, data lakes, static content | HTTP/HTTPS API |
| **Amazon EBS** | Block Storage | EC2 boot volumes, databases | Attached to EC2 |
| **Instance Store** | Block Storage | Temporary, high-performance storage | Attached to EC2 |
| **Amazon EFS** | File Storage | Shared file system for Linux | NFS protocol |
| **Amazon FSx** | File Storage | Windows file servers, HPC | SMB, Lustre |
| **S3 Glacier** | Archive Storage | Long-term backup, compliance | Retrieval API |

---

## 1. Amazon S3 (Simple Storage Service)

**Definition:** Object storage service offering **unlimited capacity**, **11 9s of durability** (99.999999999%), and HTTP-based access.

```mermaid
graph LR
    User[👤 User/Application] -->|HTTP/HTTPS| S3[📦 Amazon S3]
    S3 -->|Store| Buckets[Buckets]
    Buckets -->|Contain| Objects[Objects<br/>Files + Metadata]
    
    S3 -.->|Features| F1[Versioning]
    S3 -.->|Features| F2[Lifecycle Policies]
    S3 -.->|Features| F3[Encryption]
    S3 -.->|Features| F4[Access Control]
    
    style S3 fill:#FF9900,color:#000
```

### S3 Key Characteristics

| Attribute | Detail |
|-----------|--------|
| **Type** | Object storage |
| **Max Object Size** | 5 TB per object |
| **Durability** | 99.999999999% (11 9s) |
| **Availability** | 99.99% |
| **Access** | HTTP/HTTPS API |
| **Pricing** | Pay for storage used + requests |
| **Use Cases** | Websites, backups, data lakes, static content, big data analytics |

### S3 Storage Classes

```mermaid
graph TD
    S3[📦 S3 Storage Classes] --> Standard[💎 S3 Standard<br/>Frequently accessed]
    S3 --> IA[💾 S3 Standard-IA<br/>Infrequent access]
    S3 --> OneZone[📍 S3 One Zone-IA<br/>Single AZ, cheaper]
    S3 --> Glacier[🧊 S3 Glacier<br/>Archive, minutes-hours]
    S3 --> Deep[❄️ S3 Glacier Deep Archive<br/>Long-term, 12+ hours]
    S3 --> Intelligent[🧠 S3 Intelligent-Tiering<br/>Automatic optimization]
    
    Standard --> S1[High availability<br/>Multi-AZ]
    IA --> I1[Lower cost<br/>Multi-AZ]
    OneZone --> O1[20% cheaper than IA<br/>Single AZ]
    Glacier --> G1[Very cheap<br/>Minutes to hours retrieval]
    Deep --> D1[Cheapest<br/>12+ hours retrieval]
    Intelligent --> It1[Auto-moves data<br/>between tiers]
    
    style S3 fill:#FF9900,color:#000
    style Standard fill:#FF6B6B,color:#fff
    style IA fill:#4DABF7
    style OneZone fill:#FFD700,color:#000
    style Glacier fill:#87CEEB
    style Deep fill:#90EE90
    style Intelligent fill:#DDA0DD
```

### S3 Use Cases

| Use Case | Storage Class |
|----------|---------------|
| **Website assets** | S3 Standard |
| **Disaster recovery backups** | S3 Standard-IA or Glacier |
| **Data analytics** | S3 Standard |
| **Compliance archives** | S3 Glacier Deep Archive |
| **Unknown access patterns** | S3 Intelligent-Tiering |

> 🎯 **Exam Tip:** S3 provides **11 9s of durability** — your data is safe even if 2 facilities are destroyed simultaneously.

---

## 2. Amazon EBS (Elastic Block Store)

**Definition:** Persistent **block storage** volumes that attach to EC2 instances, providing low-latency storage for operating systems, databases, and applications.

```mermaid
graph LR
    EC2[🖥️ EC2 Instance] -->|Mounted as| EBS[💾 EBS Volume]
    EBS -->|Characteristics| C1[Network drive]
    EBS -->|Characteristics| C2[Bound to AZ]
    EBS -->|Characteristics| C3[Persists after termination]
    EBS -->|Characteristics| C4[Can be detached/reattached]
    
    style EC2 fill:#FF9900,color:#000
    style EBS fill:#4DABF7,color:#fff
```

### EBS Key Characteristics

| Attribute | Detail |
|-----------|--------|
| **Type** | Block storage (network drive) |
| **Attachment** | One EC2 instance at a time (CCP level) |
| **AZ Binding** | Locked to specific Availability Zone |
| **Latency** | Low (microseconds) |
| **Persistence** | Data survives instance termination |
| **Billing** | Provisioned capacity (GB and IOPS) |

### EBS Volume Types

```mermaid
graph TD
    EBS[💾 EBS Volume Types] --> GP3[⚖️ General Purpose SSD<br/>gp3 - Balanced]
    EBS --> IO2[🚀 Provisioned IOPS SSD<br/>io2 - High performance]
    EBS --> ST1[📊 Throughput Optimized HDD<br/>st1 - Big data]
    EBS --> SC1[💰 Cold HDD<br/>sc1 - Infrequent access]
    
    GP3 --> GP3Use[Boot volumes,<br/>general workloads]
    IO2 --> IO2Use[Critical databases,<br/>IOPS-intensive]
    ST1 --> ST1Use[Data warehouses,<br/>log processing]
    SC1 --> SC1Use[Cheaper cold storage,<br/>infrequent access]
    
    style EBS fill:#FF9900,color:#000
    style GP3 fill:#51CF66,color:#fff
    style IO2 fill:#FF6B6B,color:#fff
    style ST1 fill:#4DABF7
    style SC1 fill:#FFD700,color:#000
```

### AZ Binding Constraint

```mermaid
graph TD
    subgraph Region["📍 us-east-1 Region"]
        AZa[🏢 AZ us-east-1a]
        AZb[🏢 AZ us-east-1b]
    end
    
    EBSa[💾 EBS Volume<br/>us-east-1a] -->|✅ Can attach| Insta[EC2 Instance<br/>us-east-1a]
    EBSa -.->|❌ Cannot attach| Instb[EC2 Instance<br/>us-east-1b]
    
    style EBSa fill:#4DABF7,color:#fff
    style Insta fill:#51CF66,color:#fff
    style Instb fill:#FF6B6B,color:#fff
```

> 🎯 **Exam Tip:** EBS volumes are **locked to one AZ**. To move across AZs, you must create a snapshot and restore it in the target AZ.

### Moving EBS Volumes Across AZs

```mermaid
graph LR
    EBS1[💾 EBS Volume<br/>us-east-1a] -->|Step 1| Snap[📸 Create Snapshot]
    Snap -->|Step 2| Copy[📋 Copy Snapshot<br/>to us-east-1b]
    Copy -->|Step 3| EBS2[💾 New EBS Volume<br/>us-east-1b]
    EBS2 -->|Step 4| Instance[🖥️ EC2 Instance<br/>us-east-1b]
    
    style EBS1 fill:#4DABF7
    style Snap fill:#FFD700,color:#000
    style Copy fill:#FFD700,color:#000
    style EBS2 fill:#4DABF7
    style Instance fill:#51CF66,color:#fff
```

### EBS Snapshots

**Definition:** Point-in-time backup of an EBS volume stored in S3.

```mermaid
graph TD
    Snap[📸 EBS Snapshot] --> F1[Incremental backups]
    Snap --> F2[Stored in S3]
    Snap --> F3[Can copy across AZs]
    Snap --> F4[Can copy across Regions]
    Snap --> F5[Use for backup/DR]
    
    Snap -.->|Features| Archive[🗄️ Snapshot Archive<br/>75% cheaper, 24-72hr restore]
    Snap -.->|Features| Recycle[♻️ Recycle Bin<br/>Recover deleted snapshots]
    
    style Snap fill:#FF9900,color:#000
```

### Snapshot Archive & Recycle Bin

| Feature | Purpose | Benefit |
|---------|---------|---------|
| **Snapshot Archive** | Long-term, infrequent access | 75% cheaper storage, 24-72 hour restore time |
| **Recycle Bin** | Recover from accidental deletion | 1 day to 1 year retention |

### Delete on Termination

```mermaid
graph TD
    DoT[⚙️ Delete on Termination] --> Default[Default Behavior]
    Default --> Root[Root Volume<br/>DELETED by default]
    Default --> Other[Additional Volumes<br/>NOT DELETED by default]
    
    Root -.->|Can change| Control[Configurable via<br/>Console/CLI]
    Other -.->|Can change| Control
    
    style DoT fill:#FFD700,color:#000
    style Root fill:#FF6B6B,color:#fff
    style Other fill:#51CF66,color:#fff
```

---

## 3. Instance Store

**Definition:** **Temporary block storage** physically attached to the host machine. Provides highest I/O performance but data is lost when instance stops.

```mermaid
graph LR
    Host[🖥️ Physical Host] --> Inst[💾 Instance Store<br/>Local SSD/HDD]
    Inst --> EC2[EC2 Instance]
    
    Inst -.->|Risk| R1[Data lost if<br/>instance stops]
    Inst -.->|Risk| R2[Data lost if<br/>hardware fails]
    Inst -.->|Risk| R3[Your responsibility:<br/>backups, replication]
    
    Inst -.->|Use| U1[Buffer/Cache]
    Inst -.->|Use| U2[Scratch data]
    Inst -.->|Use| U3[Temporary content]
    
    style Inst fill:#FFD700,color:#000
```

### Instance Store vs EBS

| Feature | Instance Store | EBS |
|---------|---------------|-----|
| **Performance** | Very high I/O | Good (network latency) |
| **Persistence** | Lost on stop/termination | Persists after termination |
| **Type** | Hardware-attached | Network drive |
| **Backups** | Your responsibility | Snapshots |
| **Use Case** | Buffer, cache, scratch | Persistent data, databases |

> 🎯 **Exam Tip:** Instance Store is **ephemeral** — data is lost when the instance stops. Use only for temporary data.

---

## 4. Amazon EFS (Elastic File System)

**Definition:** Managed **Network File System (NFS)** for Linux instances. Provides shared file storage that can be mounted on hundreds of EC2 instances simultaneously.

```mermaid
graph TB
    subgraph EFS["📁 Amazon EFS"]
        FS[Shared File System]
        FS --> Data1[Data 1]
        FS --> Data2[Data 2]
        FS --> Data3[Data 3]
    end
    
    subgraph Instances["🖥️ Multiple EC2 Instances"]
        EC2a[Linux EC2 1]
        EC2b[Linux EC2 2]
        EC2c[Linux EC2 3]
    end
    
    EC2a -.->|Mount via NFS| FS
    EC2b -.->|Mount via NFS| FS
    EC2c -.->|Mount via NFS| FS
    
    style EFS fill:#51CF66,color:#fff
    style Instances fill:#FF9900,color:#000
```

### EFS Key Characteristics

| Attribute | Detail |
|-----------|--------|
| **Type** | Managed NFS (Network File System) |
| **OS Support** | Linux only |
| **Mounting** | Hundreds of EC2 instances |
| **Availability** | Multi-AZ by default |
| **Scaling** | Automatic, no capacity planning |
| **Pricing** | Pay per use (more expensive than EBS) |
| **Cost** | ~3x gp2 EBS price |

### EFS Infrequent Access (EFS-IA)

```mermaid
graph TD
    EFSIA[💰 EFS-IA] --> Purpose[Cost Optimization]
    Purpose --> P1[Files not accessed daily]
    Purpose --> P2[Up to 92% lower cost]
    
    EFSIA --> How[How It Works]
    How --> H1[Automatic file movement<br/>based on access time]
    How --> H2[Lifecycle Policy<br/>e.g., 60 days no access]
    How --> H3[Transparent to applications]
    
    style EFSIA fill:#51CF66,color:#fff
```

---

## 5. Amazon FSx

**Definition:** Managed file storage for **Windows** and **HPC workloads**. Provides fully managed Windows file servers and high-performance Lustre file systems.

```mermaid
graph TD
    FSx[📁 Amazon FSx] --> Win[🪟 FSx for Windows<br/>File Server]
    FSx --> Lustre[🚀 FSx for Lustre]
    FSx --> ONTAP[💾 FSx for NetApp ONTAP]
    FSx --> OpenZFS[📦 FSx for OpenZFS]
    
    Win --> W1[SMB protocol<br/>Windows-native]
    Win --> W2[Active Directory<br/>integration]
    
    Lustre --> L1[High-performance<br/>HPC workloads]
    Lustre --> L2[S3 integration]
    Lustre --> L3[Compute-intensive<br/>workloads]
    
    style FSx fill:#FF9900,color:#000
```

### FSx Options Comparison

| Option | Protocol | Use Case | Performance |
|--------|----------|----------|-------------|
| **FSx for Windows** | SMB | Windows-native file systems, Active Directory | High |
| **FSx for Lustre** | Lustre | HPC, ML, compute-intensive workloads | Very high |
| **FSx for ONTAP** | NFS/SMB | NetApp features, snapshots, replication | High |
| **FSx for OpenZFS** | NFS | ZFS features, snapshots, compression | High |

---

## Storage Decision Matrix

```mermaid
flowchart TD
    Start{Need storage?} --> Q1{Type of access?}
    
    Q1 -->|HTTP API<br/>Unlimited files| S3[📦 S3<br/>Object Storage]
    Q1 -->|Block storage<br/>Single EC2| Q2{Need persistence?}
    
    Q2 -->|Yes| EBS[💾 EBS<br/>Persistent block]
    Q2 -->|No, temporary| Inst[⚡ Instance Store<br/>Ephemeral block]
    
    Q1 -->|Shared file system<br/>Multiple EC2| Q3{Which OS?}
    
    Q3 -->|Linux| EFS[📁 EFS<br/>NFS file system]
    Q3 -->|Windows| FSxW[🪟 FSx for Windows]
    Q3 -->|HPC| FSxL[🚀 FSx for Lustre]
    
    Start --> Archive{Long-term<br/>archive?}
    Archive -->|Yes| Glacier[🧊 S3 Glacier<br/>Cheap archive]
    
    style S3 fill:#4DABF7,color:#fff
    style EBS fill:#FF6B6B,color:#fff
    style Inst fill:#FFD700,color:#000
    style EFS fill:#51CF66,color:#fff
    style FSxW fill:#87CEEB
    style FSxL fill:#DDA0DD
    style Glacier fill:#90EE90
```

### Storage Selection Guide

| Need | Best Choice | Reason |
|------|-------------|--------|
| **Static website assets** | S3 | Object storage, HTTP access, cheap |
| **EC2 boot volume** | EBS | Persistent block storage |
| **Shared file system (Linux)** | EFS | NFS, multi-AZ, scales automatically |
| **Windows file server** | FSx for Windows | SMB protocol, AD integration |
| **HPC workloads** | FSx for Lustre | High-performance, S3 integration |
| **Temporary high-speed storage** | Instance Store | Fastest I/O, ephemeral |
| **Long-term backup** | S3 Glacier | Cheapest storage, compliance |
| **Database storage** | EBS | Low latency, persistent |

---

## Amazon Machine Image (AMI)

**Definition:** Template containing **OS, application server, and applications** needed to launch an EC2 instance.

```mermaid
graph TD
    AMI[📦 Amazon Machine Image] --> Contains[Contains]
    Contains --> C1[Operating System]
    Contains --> C2[Application Server]
    Contains --> C3[Applications]
    Contains --> C4[Launch Permissions]
    
    AMI --> Sources[Sources]
    Sources --> S1[Public AMI<br/>AWS provided]
    Sources --> S2[Your Own AMI<br/>Custom]
    Sources --> S3[Marketplace AMI<br/>Third-party]
    Sources --> S4[Community AMI<br/>Shared by users]
    
    style AMI fill:#FF9900,color:#000
```

### AMI Sources Comparison

| Source | Description | Verified | Cost |
|--------|-------------|----------|------|
| **Public AMI** | AWS-provided | ✅ Yes | Free |
| **Your Own AMI** | You create and maintain | ✅ You verify | Free |
| **Marketplace AMI** | Third-party, possibly paid | ✅ AWS verifies | Varies |
| **Community AMI** | Shared by AWS users | ❌ Not verified | Free (use caution) |

### Creating a Custom AMI

```mermaid
graph LR
    Step1[1. Launch EC2<br/>instance] --> Step2[2. Customize<br/>instance]
    Step2 --> Step3[3. Stop instance<br/>for integrity]
    Step3 --> Step4[4. Build AMI<br/>creates snapshots]
    Step4 --> Step5[5. Launch new<br/>instances from AMI]
    
    style Step1 fill:#4DABF7,color:#fff
    style Step2 fill:#FFA500
    style Step3 fill:#FFD700,color:#000
    style Step4 fill:#FF6B6B,color:#fff
    style Step5 fill:#51CF66,color:#fff
```

### EC2 Image Builder

**Definition:** Fully managed service that **automates** the creation, patching, and testing of AMIs.

```mermaid
graph LR
    A[📅 Schedule] --> B[🔨 Build]
    B --> C[🧪 Test]
    C --> D[✅ Validate]
    D --> E[📤 Distribute]
    E --> A
    
    style A fill:#4DABF7,color:#fff
    style B fill:#FFA500
    style C fill:#FFD700,color:#000
    style D fill:#FF6B6B,color:#fff
    style E fill:#51CF66,color:#fff
```

**Benefits:**
- Scheduled execution (weekly or on package updates)
- Automated testing and validation
- Free service (pay only for underlying resources)

---

## Quick Reference

| Service | Type | Best For | Key Feature |
|---------|------|----------|-------------|
| **S3** | Object | Websites, backups, data lakes | 11 9s durability, unlimited capacity |
| **EBS** | Block | EC2 boot volumes, databases | Persistent, low latency, AZ-bound |
| **Instance Store** | Block | Temporary, high-speed | Fastest I/O, ephemeral |
| **EFS** | File | Shared Linux file system | Multi-AZ, NFS, auto-scaling |
| **FSx** | File | Windows, HPC | SMB, Lustre, high performance |
| **Glacier** | Archive | Long-term backup | Cheapest storage |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: Which storage service provides object storage with HTTP-based access?</strong></summary>

**A.** Amazon EBS  
**B.** Amazon S3  
**C.** Amazon EFS  
**D.** Instance Store  

**Answer: B** — Amazon S3 is object storage accessed via HTTP/HTTPS API. It's ideal for websites, backups, data lakes, and static content.
</details>

<details>
<summary><strong>Q2: What is the main constraint of EBS volumes?</strong></summary>

**A.** Limited storage capacity  
**B.** Locked to a specific Availability Zone  
**C.** Cannot be backed up  
**D.** Only works with Linux  

**Answer: B** — EBS volumes are locked to the Availability Zone in which they were created. To move an EBS volume to a different AZ, you must create a snapshot and restore it in the target AZ.
</details>

<details>
<summary><strong>Q3: Which storage service should you use for a shared file system across multiple Linux EC2 instances?</strong></summary>

**A.** Amazon S3  
**B.** Amazon EBS  
**C.** Amazon EFS  
**D.** Instance Store  

**Answer: C** — Amazon EFS provides a managed NFS file system that can be mounted on hundreds of Linux EC2 instances simultaneously. It supports multi-AZ access and scales automatically.
</details>

<details>
<summary><strong>Q4: What happens to data in an Instance Store when an EC2 instance stops?</strong></summary>

**A.** Data is automatically backed up  
**B.** Data is lost  
**C.** Data is moved to S3  
**D.** Data is encrypted  

**Answer: B** — Instance Store provides temporary, ephemeral storage. Data is lost when the instance stops, terminates, or if the underlying hardware fails. Use it only for temporary data like buffers or caches.
</details>

---

## Navigation

⬅️ Previous: [Amazon EC2](./01-compute-ec2.md) | ➡️ Next: [Networking](./03-networking.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*