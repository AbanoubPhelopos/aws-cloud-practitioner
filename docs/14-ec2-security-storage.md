# EC2 Security & Storage

## The Big Picture

This module covers essential EC2 security and storage concepts: **Security Groups, EBS Volumes, Snapshots, AMIs, Instance Store, EFS, and FSx**.

---

## I. Security in EC2

### Security Groups Overview

```mermaid
graph TD
    SG[Security Groups] --> Fire[Firewall for EC2]
    SG --> Reg[Regulate Traffic]
    SG --> Rules[Sole purpose: Rules]
    
    Reg --> Reg1[Inbound traffic]
    Reg --> Reg2[Outbound traffic]
    
    Rules --> R1[By IP address]
    Rules --> R2[Reference other Security Groups]
```

### Security Group Characteristics

| Characteristic | Description |
|----------------|-------------|
| **Purpose** | Firewall for EC2 instances |
| **Composition** | Consists solely of rules |
| **Scope** | Specific region and VPC combination |
| **Attachment** | Can be attached to multiple instances |
| **Operation** | External to EC2 - blocked traffic never reaches the instance |

### What Security Groups Regulate

```mermaid
graph TD
    Reg[Security Groups Regulate] --> P[Access to specific ports]
    Reg --> IP[Authorized IP ranges<br/>IPv4 and IPv6]
    Reg --> In[Control of inbound traffic<br/>from others to instance]
    Reg --> Out[Control of outbound traffic<br/>from instance to others]
```

### Default Behaviors

| Traffic Direction | Default Behavior |
|-------------------|------------------|
| **Inbound** | **Blocked** by default |
| **Outbound** | **Allowed** by default |

### Troubleshooting Connection Issues

```mermaid
graph TD
    Issue[Connection Issue] --> Q{Error Type?}
    
    Q -->|Timeout<br/>Connection hangs| SG[Security Group Issue<br/>Traffic blocked]
    Q -->|Connection Refused<br/>Immediate rejection| App[Application Issue<br/>Not running or crashed]
    
    SG --> Fix1[Check SG inbound rules]
    App --> Fix2[Check application status<br/>and logs]
```

> ⚠️ **Key Insight:**
> - **Timeout** = Security Group issue (traffic blocked)
> - **Connection Refused** = Application issue (not running or crashed)

### SSH Best Practice

```mermaid
graph TD
    Best[Best Practice] --> BP[Maintain separate SG<br/>for SSH access]
    
    BP --> B1[Restrict SSH access]
    BP --> B2[Limit to specific IPs]
    BP --> B3[Better security management]
    BP --> B4[Easier access auditing]
```

### Useful Ports Reference

| Port | Protocol | Use Case |
|------|----------|----------|
| **22** | SSH | Logging into Linux instances |
| **21** | FTP | Uploading files to a file share |
| **22** | SFTP | Securely uploading files via SSH |
| **80** | HTTP | Accessing unsecured websites |
| **443** | HTTPS | Accessing secured websites |
| **3389** | RDP | Logging into Windows instances |

---

## II. EBS Volumes

### What is EBS?

```mermaid
graph TD
    EBS[Elastic Block Store Volume] --> Net[Network Drive]
    EBS --> Att[Attached while running]
    EBS --> Persist[Persists data after termination]
    EBS --> OneAZ[Bound to specific AZ]
    EBS --> USB[Network USB Stick]
    
    Net --> Note[Not physical drive<br/>Uses network<br/>Some latency]
```

### EBS Key Characteristics

| Characteristic | Description |
|----------------|-------------|
| **Type** | Network drive (not physical) |
| **Attachment** | Mounted to one instance at a time (CCP level) |
| **AZ Binding** | Locked to specific Availability Zone |
| **Latency** | Some network latency (not physical disk) |
| **Detachable** | Can be moved between instances quickly |

### AZ Binding Example

```mermaid
graph TD
    subgraph Region1["us-east-1"]
        AZ1a[AZ us-east-1a]
        AZ1b[AZ us-east-1b]
    end
    
    EBS1[EBS Volume<br/>us-east-1a] --> AZ1a
    EBS1 -.->|❌ Cannot attach| AZ1b
    
    AZ1b --> Instance[EC2 Instance<br/>us-east-1b]
```

> ⚠️ EBS volumes are locked to an AZ. A volume in `us-east-1a` **cannot** be attached to an instance in `us-east-1b`.

### Moving Volumes Across AZs

```mermaid
graph TD
    EBS1[EBS Volume<br/>us-east-1a] --> Snapshot[Create Snapshot]
    Snapshot --> Copy[Copy to us-east-1b]
    Copy --> EBS2[New EBS Volume<br/>us-east-1b]
    EBS2 --> Instance[EC2 Instance<br/>us-east-1b]
```

### EBS Capacity and Billing

| Aspect | Description |
|--------|-------------|
| **Provisioning** | Provisioned capacity (GBs and IOPS) |
| **Billing** | Based on provisioned capacity, not usage |
| **Scaling** | Can be increased over time |

### Free Tier

| Tier | Details |
|------|---------|
| **Storage** | 30 GB free EBS storage per month |
| **Types** | General Purpose (SSD) or Magnetic |

### Delete on Termination Attribute

```mermaid
graph TD
    DoT[Delete on Termination] --> Default[Default Behavior]
    Default --> Root[Root EBS Volume<br/>DELETED by default]
    Default --> Other[Other EBS Volumes<br/>NOT DELETED by default]
    
    Root --> Control[Can be controlled via<br/>AWS Console / CLI]
    Other --> Control
    
    Control --> UC[Use Case: Preserve root volume<br/>when instance is terminated]
```

---

## III. EBS Snapshots

### What is an EBS Snapshot?

```mermaid
graph TD
    Snap[EBS Snapshot] --> Def[Definition]
    Def --> D1[Backup of EBS volume<br/>at specific point in time]
    
    Snap --> Features[Features]
    Features --> F1[No need to detach volume<br/>(but recommended for consistency)]
    Features --> F2[Can be copied across AZs]
    Features --> F3[Can be copied across Regions]
```

### Snapshot Recommendations

| Recommendation | Reason |
|----------------|--------|
| **Stop instance before snapshot** | Ensures data consistency |
| **Detach before snapshot** | Recommended for consistency |
| **Copy across AZs/Regions** | Disaster recovery |

### EBS Snapshot Features

```mermaid
graph TD
    Features[Snapshot Features] --> Arch[Snapshot Archive]
    Features --> Recycle[Recycle Bin]
    
    Arch --> A1[75% cheaper storage]
    Arch --> A2[24-72 hours restore time]
    
    Recycle --> R1[Retention rules]
    Recycle --> R2[1 day to 1 year retention]
    Recycle --> R3[Recover from accidental deletion]
```

### Snapshot Archive

| Aspect | Details |
|--------|---------|
| **Purpose** | Long-term, infrequent access |
| **Cost** | 75% cheaper than standard |
| **Restore Time** | 24-72 hours |

### Recycle Bin for Snapshots

| Aspect | Details |
|--------|---------|
| **Purpose** | Recover from accidental deletion |
| **Retention** | 1 day to 1 year |
| **Configuration** | Set retention rules |

---

## IV. Amazon Machine Image (AMI)

### What is an AMI?

```mermaid
graph TD
    AMI[Amazon Machine Image] --> Def[Definition]
    Def --> D1[Customization of EC2 instance]
    Def --> D2[Add software, configuration, OS, monitoring]
    
    AMI --> Benefit[Benefits]
    Benefit --> B1[Faster boot/configuration]
    Benefit --> B2[Software pre-packaged]
    Benefit --> B3[Region-specific<br/>can be copied]
```

### AMI Sources

```mermaid
graph TD
    AMI[AMI Sources] --> Pub[Public AMI<br/>AWS provided]
    AMI --> Own[Your Own AMI<br/>You make and maintain]
    AMI --> Market[AWS Marketplace AMI<br/>Someone else made<br/>potentially sells]
    AMI --> Comm[Community AMI<br/>From AWS users<br/>NOT verified by AWS]
```

### AMI Source Comparison

| Source | Description | Verified |
|--------|-------------|----------|
| **Public AMI** | AWS-provided | ✅ Yes |
| **Your Own AMI** | You create and maintain | ✅ You verify |
| **Marketplace AMI** | Third-party, potentially paid | ✅ AWS verifies |
| **Community AMI** | AWS users share | ❌ Not verified |

### Creating an AMI

```mermaid
graph TD
    Step1[1. Launch EC2 instance] --> Step2[2. Customize it]
    Step2 --> Step3[3. Stop instance<br/>for data integrity]
    Step3 --> Step4[4. Build AMI<br/>creates EBS snapshots]
    Step4 --> Step5[5. Launch new instances<br/>from custom AMI]
```

---

## V. EC2 Image Builder

### What is EC2 Image Builder?

```mermaid
graph TD
    Builder[EC2 Image Builder] --> Purpose[Purpose]
    Purpose --> P1[Automate creation of VMs<br/>or container images]
    Purpose --> P2[Automate AMI creation, maintenance,<br/>validation, and testing]
    
    Builder --> Features[Features]
    Features --> F1[Scheduled execution]
    Features --> F2[Weekly or when packages update]
    Features --> F3[Free service<br/>pay only for underlying resources]
```

### Image Builder Workflow

```mermaid
graph LR
    A[Schedule] --> B[Build]
    B --> C[Test]
    C --> D[Validate]
    D --> E[Distribute]
    E --> A
```

---

## VI. EC2 Instance Store

### Instance Store vs EBS

```mermaid
graph TD
    Compare[Storage Options] --> EBS[EBS Volumes]
    Compare --> Inst[EC2 Instance Store]
    
    EBS --> EBS1[Network drives<br/>Good but limited performance<br/>Persistent storage]
    Inst --> Inst1[High-performance hardware<br/>Better I/O performance<br/>Ephemeral storage]
    
    Inst --> Risk[Risks]
    Risk --> R1[Data lost if instance stopped]
    Risk --> R2[Data lost if hardware fails]
    Risk --> R3[Your responsibility: backups, replication]
```

### When to Use Instance Store

```mermaid
graph TD
    Use[Use Cases for Instance Store] --> U1[Buffer]
    Use --> U2[Cache]
    Use --> U3[Scratch data]
    Use --> U4[Temporary content]
    
    Note[⚠️ NOT for: persistent data<br/>long-term storage]
```

### Storage Comparison Matrix

| Feature | EBS | Instance Store |
|---------|-----|----------------|
| **Performance** | Good (limited) | High I/O |
| **Persistence** | Persists after termination | Lost on stop/termination |
| **Type** | Network drive | Hardware-attached |
| **Backups** | Snapshots | Your responsibility |
| **Use Case** | Persistent data | Buffer, cache, scratch |

---

## VII. EFS - Elastic File System

### What is EFS?

```mermaid
graph TD
    EFS[Elastic File System] --> Def[Definition]
    Def --> D1[Managed NFS]
    Def --> D2[Mounted on 100s of EC2]
    Def --> D3[Linux EC2 only]
    Def --> D4[Multi-AZ support]
    
    EFS --> Features[Features]
    Features --> F1[Highly available]
    Features --> F2[Scalable]
    Features --> F3[Expensive (3x gp2)]
    Features --> F4[Pay per use]
    Features --> F5[No capacity planning]
```

### EFS Key Characteristics

| Characteristic | Description |
|----------------|-------------|
| **Type** | Managed NFS (Network File System) |
| **Mounting** | 100s of EC2 instances |
| **OS Support** | Linux EC2 instances only |
| **Availability** | Multi-AZ |
| **Pricing** | Pay per use, no capacity planning |
| **Cost** | 3x gp2 price (expensive) |

### EFS Infrequent Access (EFS-IA)

```mermaid
graph TD
    IA[EFS-IA] --> Purpose[Storage Class for Cost Optimization]
    Purpose --> P1[Files not accessed daily]
    Purpose --> P2[Up to 92% lower cost vs Standard]
    
    IA --> How[How it Works]
    How --> H1[Automatic file movement based on access time]
    How --> H2[Lifecycle Policy configuration]
    How --> H3[Transparent to applications]
```

### EFS-IA Configuration

| Aspect | Details |
|--------|---------|
| **Cost Savings** | Up to 92% lower than Standard |
| **Trigger** | Last access time |
| **Configuration** | Lifecycle Policy (e.g., 60 days no access) |
| **Transition** | Transparent to applications |

---

## VIII. Amazon FSx

### What is FSx?

```mermaid
graph TD
    FSx[Amazon FSx] --> Def[Managed File Storage]
    
    FSx --> Win[FSx for Windows File Server]
    Win --> W1[Managed Windows-native file system]
    Win --> W2[SMB protocol]
    Win --> W3[Microsoft Active Directory integration]
    
    FSx --> Lustre[FSx for Lustre]
    Lustre --> L1[High-performance file system]
    Lustre --> L2[Compute-intensive workloads]
    Lustre --> L3[Integrates with Amazon S3]
```

### FSx Options

| Option | Protocol | Use Case |
|--------|----------|----------|
| **FSx for Windows File Server** | SMB | Windows-native file systems |
| **FSx for Lustre** | Lustre | HPC, compute-intensive workloads |

### FSx Key Features

| Feature | Description |
|---------|-------------|
| **Performance** | High performance |
| **Scalability** | Easily scalable |
| **Backups** | Built-in backup capabilities |
| **Integration** | AWS services integration |

---

## IX. EC2 Storage Options Summary

```mermaid
graph TD
    Storage[EC2 Storage Options] --> EBS[EBS Volumes]
    Storage --> Inst[Instance Store]
    Storage --> EFS[EFS]
    Storage --> FSx[FSx]
    
    EBS --> EBS1[Block storage<br/>Single instance<br/>Persistent]
    Inst --> Inst1[Block storage<br/>Single instance<br/>Ephemeral]
    EFS --> EFS1[Network file system<br/>Multi-instance<br/>Linux]
    FSx --> FSx1[Managed file systems<br/>Windows/HPC]
```

### Storage Decision Matrix

| Need | Best Choice |
|------|-------------|
| **Persistent block storage for single instance** | EBS |
| **High-performance temporary storage** | Instance Store |
| **Shared file system for multiple Linux instances** | EFS |
| **Windows-native file system** | FSx for Windows |
| **HPC / compute-intensive workloads** | FSx for Lustre |
| **Infrequent access file storage** | EFS-IA |

---

## Key Takeaways

### Security
1. **Security Groups** = firewall for EC2
2. **Default**: Inbound blocked, Outbound allowed
3. **Scope**: Region and VPC combination
4. **Timeout** = SG issue, **Connection Refused** = Application issue
5. **Best Practice**: Separate SG for SSH access

### Storage
1. **EBS** = Network drive, bound to AZ, persistent
2. **Snapshots** = Backup of EBS, can copy across AZs/Regions
3. **Snapshot Archive** = 75% cheaper, 24-72 hour restore
4. **Recycle Bin** = Recover deleted snapshots
5. **AMI** = Custom EC2 template, multiple sources
6. **EC2 Image Builder** = Automated AMI creation
7. **Instance Store** = High-performance, ephemeral storage
8. **EFS** = Managed NFS for Linux, multi-AZ
9. **EFS-IA** = Up to 92% cheaper for infrequent access
10. **FSx** = Managed Windows/HPC file systems

---

## Next Steps

⬅️ Previous: [EC2 Purchasing Options](./13-ec2-purchasing-options.md) | ➡️ Next: [Load Balancing, Auto Scaling, and Route 53](./15-load-balancing.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*