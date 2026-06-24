# AWS Shared Responsibility Model

> ⏱️ **Estimated Study Time:** 12 minutes  
> 🎯 **CCP Exam Weight:** ~10-15% (Domain 2: Security & Compliance)

---

## The Big Picture

The **Shared Responsibility Model** is AWS's security framework that defines who is responsible for what. Simply put: **AWS is responsible for security OF the cloud (infrastructure), while you are responsible for security IN the cloud (your data and configurations)**. This model is heavily tested on the CCP exam.

---

## Core Concept

```mermaid
graph TD
    SRM[🔒 Shared Responsibility Model] --> AWS[☁️ AWS Responsibility<br/>Security OF the Cloud]
    SRM --> You[🧑‍💻 Customer Responsibility<br/>Security IN the Cloud]
    
    AWS --> A1[Physical security<br/>of data centers]
    AWS --> A2[Hardware & software<br/>infrastructure]
    AWS --> A3[Network infrastructure]
    AWS --> A4[Virtualization layer]
    
    You --> Y1[Your data]
    You --> Y2[Access management<br/>IAM]
    You --> Y3[Application security]
    You --> Y4[OS patches<br/>on EC2]
    You --> Y5[Security configuration<br/>of services]
    
    style SRM fill:#FF9900,color:#000
    style AWS fill:#4DABF7,color:#fff
    style You fill:#FF6B6B,color:#fff
```

> 🎯 **Exam Tip:** Memorize the phrase: **"AWS is responsible for the cloud itself; you're responsible for what you put in it."**

---

## Visual Responsibility Breakdown

```mermaid
graph TB
    subgraph Cloud["☁️ AWS Responsibility - Security OF the Cloud"]
        A1[🏢 Physical Security<br/>Data center access, guards, CCTV]
        A2[🖥️ Infrastructure<br/>Servers, storage, networking hardware]
        A3[💾 Hardware/Software<br/>Host OS, virtualization layer]
        A4[🌐 Network Infrastructure<br/>Regions, AZs, Edge Locations]
    end
    
    subgraph Customer["🧑‍💻 Customer Responsibility - Security IN the Cloud"]
        C1[🔐 Identity & Access Management<br/>IAM users, roles, policies]
        C2[📊 Data Protection<br/>Encryption at rest and in transit]
        C3[🖥️ Operating System<br/>Patches, updates on EC2]
        C4[💻 Applications<br/>Code security, vulnerability management]
        C5[⚙️ Configuration<br/>Security group rules, network ACLs]
        C6[🔑 Credentials<br/>Passwords, API keys, access keys]
    end
    
    style Cloud fill:#E3F2FD
    style Customer fill:#FFE6E6
```

---

## Responsibility by Service Model

As you move from IaaS to SaaS, AWS takes on **more** responsibility, and you take on **less**.

```mermaid
graph TD
    Models[📊 Responsibility by Service Model] --> OnPrem[🏢 On-Premises<br/>You: Everything]
    Models --> IaaS[🖥️ IaaS<br/>You: OS, Apps, Data]
    Models --> PaaS[⚙️ PaaS<br/>You: Apps, Data]
    Models --> SaaS[📦 SaaS<br/>You: Just use it]
    
    OnPrem --> O1[100% Your responsibility]
    IaaS --> I1[~80% Your, 20% AWS]
    PaaS --> P1[~40% Your, 60% AWS]
    SaaS --> S1[~10% Your, 90% AWS]
    
    style OnPrem fill:#FF6B6B,color:#fff
    style IaaS fill:#FFA500,color:#fff
    style PaaS fill:#FFD700,color:#000
    style SaaS fill:#51CF66,color:#fff
```

### Detailed Layer Breakdown

| Layer | On-Premises | IaaS (EC2) | PaaS (RDS) | SaaS (Chime) |
|-------|-------------|------------|------------|--------------|
| **Applications** | 🧑‍💻 You | 🧑‍💻 You | 🧑‍💻 You | ☁️ AWS |
| **Data** | 🧑‍💻 You | 🧑‍💻 You | 🧑‍💻 You | 🧑‍💻 You |
| **Runtime** | 🧑‍💻 You | 🧑‍💻 You | ☁️ AWS | ☁️ AWS |
| **Operating System** | 🧑‍💻 You | 🧑‍💻 You | ☁️ AWS | ☁️ AWS |
| **Virtualization** | 🧑‍💻 You | ☁️ AWS | ☁️ AWS | ☁️ AWS |
| **Compute** | 🧑‍💻 You | ☁️ AWS | ☁️ AWS | ☁️ AWS |
| **Storage** | 🧑‍💻 You | ☁️ AWS | ☁️ AWS | ☁️ AWS |
| **Networking** | 🧑‍💻 You | ☁️ AWS | ☁️ AWS | ☁️ AWS |
| **Physical Security** | 🧑‍💻 You | ☁️ AWS | ☁️ AWS | ☁️ AWS |

---

## AWS Responsibilities (Security OF the Cloud)

```mermaid
mindmap
  root((AWS<br/>Responsibilities))
  🏢 Physical Security
    Data center access control
    Security guards
    CCTV monitoring
    Biometric access
  🖥️ Infrastructure
    Hardware maintenance
    Server replacement
    Network equipment
  💾 Software
    Host OS patches
    Hypervisor security
    Service software
  🌐 Network
    Global backbone
    Internet connectivity
    DDoS protection baseline
  ⚡ Power & Cooling
    UPS systems
    Backup generators
    Precision cooling
  🌍 Compliance
    Infrastructure certifications
    SOC, PCI, ISO, HIPAA
```

### Key AWS Responsibilities

| Responsibility | Description |
|----------------|-------------|
| **Physical Security** | Data centers with restricted access, guards, CCTV, biometric authentication |
| **Hardware Maintenance** | Server replacement, hardware lifecycle management |
| **Host OS & Hypervisor** | Patching and securing the virtualization layer |
| **Network Infrastructure** | Global backbone, Regions, AZs, Edge Locations |
| **Service Availability** | Ensuring services run and scale as designed |
| **Compliance Certifications** | SOC, PCI DSS, ISO 27001, HIPAA, GDPR |

---

## Customer Responsibilities (Security IN the Cloud)

```mermaid
mindmap
  root((Customer<br/>Responsibilities))
  🔐 Identity & Access
    IAM users and roles
    Multi-factor authentication
    Password policies
  📊 Data Protection
    Encryption at rest
    Encryption in transit
    Backup strategies
  🖥️ EC2 Operating System
    OS patches and updates
    Antivirus software
    Firewall configuration
  💻 Applications
    Secure coding practices
    Vulnerability scanning
    Dependency management
  ⚙️ Configuration
    Security group rules
    Network ACLs
    S3 bucket policies
  🔑 Credentials
    API key management
    Access key rotation
    Secrets management
```

### Key Customer Responsibilities

| Responsibility | Examples |
|----------------|----------|
| **IAM** | Create users, roles, policies, enable MFA |
| **Data Encryption** | Encrypt S3 buckets, RDS databases, EBS volumes |
| **OS Patching** | Update Windows/Linux on EC2 instances |
| **Security Groups** | Configure inbound/outbound rules |
| **Application Security** | Secure code, vulnerability management |
| **Network Configuration** | VPC setup, subnet design, NACLs |
| **Backup & Recovery** | EBS snapshots, S3 versioning, cross-Region replication |

---

## Real-World Examples

### Example 1: Amazon EC2

```mermaid
graph LR
    subgraph AWS["☁️ AWS Manages"]
        A1[Physical security<br/>of the host]
        A2[Hypervisor security]
        A3[Network connectivity]
    end
    
    subgraph You["🧑‍💻 You Manage"]
        Y1[Guest OS patches<br/>Windows/Linux updates]
        Y2[Security group rules]
        Y3[IAM roles for EC2]
        Y4[Data encryption<br/>on EBS volumes]
    end
    
    style AWS fill:#E3F2FD
    style You fill:#FFE6E6
```

### Example 2: Amazon S3

```mermaid
graph LR
    subgraph AWS["☁️ AWS Manages"]
        A1[Infrastructure security]
        A2[Storage system patches]
        A3[Network protection]
    end
    
    subgraph You["🧑‍💻 You Manage"]
        Y1[Bucket policies]
        Y2[Access control lists]
        Y3[Encryption settings]
        Y4[Versioning configuration]
        Y5[Public access settings]
    end
    
    style AWS fill:#E3F2FD
    style You fill:#FFE6E6
```

### Example 3: Amazon RDS

```mermaid
graph LR
    subgraph AWS["☁️ AWS Manages"]
        A1[OS patching]
        A2[Database patches]
        A3[Infrastructure]
        A4[Automated backups]
    end
    
    subgraph You["🧑‍💻 You Manage"]
        Y1[Database access control]
        Y2[Encryption settings]
        Y3[Network configuration]
        Y4[Application-level security]
    end
    
    style AWS fill:#E3F2FD
    style You fill:#FFE6E6
```

> 🎯 **Exam Tip:** For managed services (RDS, Lambda), AWS handles more. For IaaS (EC2), you handle OS patching.

---

## Customer Responsibility Summary by Service Type

| Service Type | Examples | Customer Responsibility |
|--------------|----------|------------------------|
| **Compute (IaaS)** | EC2, EBS | OS patches, security groups, IAM roles, data encryption |
| **Containers** | ECS, EKS | Container images, task definitions, IAM roles |
| **Storage** | S3, EFS | Bucket policies, encryption, access control |
| **Database (PaaS)** | RDS, DynamoDB | Database access, encryption, VPC configuration |
| **Serverless** | Lambda | Function code, IAM roles, environment variables |
| **Networking** | VPC | Subnet design, security groups, NACLs, route tables |

---

## Common Misconceptions

```mermaid
graph TD
    Misconception[❌ Common Misconceptions] --> M1["AWS secures everything"]
    M1 --> R1[✅ Correct: You secure your data, apps, and configurations]
    
    Misconception --> M2["I don't need to patch managed services"]
    M2 --> R2[✅ Correct: AWS patches managed services, but you patch EC2 OS]
    
    Misconception --> M3["Encryption is AWS's job"]
    M3 --> R3[✅ Correct: You must enable and configure encryption]
    
    Misconception --> M4["Security groups are AWS's responsibility"]
    M4 --> R4[✅ Correct: You configure security group rules]
    
    style Misconception fill:#FF6B6B,color:#fff
    style R1 fill:#51CF66,color:#fff
    style R2 fill:#51CF66,color:#fff
    style R3 fill:#51CF66,color:#fff
    style R4 fill:#51CF66,color:#fff
```

---

## Shared Responsibility Visualization

```mermaid
graph TD
    Stack[📚 Cloud Stack] --> L1[Layer 1: Data]
    Stack --> L2[Layer 2: Applications]
    Stack --> L3[Layer 3: Runtime]
    Stack --> L4[Layer 4: OS]
    Stack --> L5[Layer 5: Virtualization]
    Stack --> L6[Layer 6: Compute]
    Stack --> L7[Layer 7: Storage]
    Stack --> L8[Layer 8: Network]
    Stack --> L9[Layer 9: Physical]
    
    L1 -->|You| C[🧑‍💻 Customer]
    L2 -->|You| C
    L3 -->|Varies| V{Varies by<br/>service}
    L4 -->|Varies| V
    L5 -->|AWS| A[☁️ AWS]
    L6 -->|AWS| A
    L7 -->|AWS| A
    L8 -->|AWS| A
    L9 -->|AWS| A
    
    style C fill:#FF6B6B,color:#fff
    style A fill:#4DABF7,color:#fff
    style V fill:#FFD700,color:#000
```

---

## Quick Reference

| Aspect | AWS Responsibility | Customer Responsibility |
|--------|-------------------|------------------------|
| **Physical Security** | ✅ Data centers, guards, CCTV | ❌ |
| **Hardware/Network** | ✅ Servers, switches, routers | ❌ |
| **Virtualization** | ✅ Hypervisor, host OS | ❌ |
| **Guest OS (EC2)** | ❌ | ✅ Patch and update |
| **Applications** | ❌ | ✅ Secure code |
| **Data** | ❌ | ✅ Encrypt, backup |
| **Access Control** | ❌ | ✅ IAM, MFA, policies |
| **Configuration** | ❌ | ✅ Security groups, NACLs |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: According to the Shared Responsibility Model, who is responsible for patching the guest operating system on an EC2 instance?</strong></summary>

**A.** AWS  
**B.** The customer  
**C.** Both share equally  
**D.** A third party  

**Answer: B** — In the Shared Responsibility Model, AWS is responsible for the host OS and hypervisor, but the customer is responsible for patching and securing the guest operating system on EC2 instances.
</details>

<details>
<summary><strong>Q2: Which of the following is AWS responsible for?</strong></summary>

**A.** Configuring IAM policies  
**B.** Encrypting customer data  
**C.** Physical security of data centers  
**D.** Patching the guest OS on EC2  

**Answer: C** — AWS is responsible for the physical security of data centers, including access control, guards, CCTV, and environmental controls. Customers are responsible for IAM, data encryption, and guest OS patching.
</details>

<details>
<summary><strong>Q3: For Amazon RDS, who is responsible for database engine patching?</strong></summary>

**A.** The customer  
**B.** AWS  
**C.** Both share equally  
**D.** The database vendor  

**Answer: B** — Amazon RDS is a managed service, so AWS handles database engine patching, OS patching, and infrastructure. The customer is responsible for database access control, encryption settings, and application-level security.
</details>

---

## Navigation

⬅️ Previous: [AWS Global Infrastructure](./01-global-infrastructure.md) | ➡️ Next: [AWS Data Centers](./03-data-centers.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*