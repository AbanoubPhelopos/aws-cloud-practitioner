# AWS Security Fundamentals & Compliance

> ⏱️ **Estimated Study Time:** 20 minutes  
> 🎯 **CCP Exam Weight:** ~20-25% (Domain 2: Security & Compliance)

---

## The Big Picture

AWS provides a **comprehensive suite of security services** to protect your infrastructure, data, and applications. Understanding IAM, Security Groups, encryption, and compliance programs is essential for the CCP exam and real-world AWS usage.

---

## AWS Security Services Overview

```mermaid
graph TD
    Security[🔒 AWS Security Services] --> IAM[🔐 Identity & Access<br/>IAM, MFA, Cognito]
    Security --> Network[🛡️ Network Security<br/>Security Groups, NACLs, WAF]
    Security --> Data[📊 Data Protection<br/>KMS, Encryption, Macie]
    Security --> Monitor[📈 Monitoring<br/>CloudTrail, GuardDuty]
    Security --> Compliance[📜 Compliance<br/>Artifact, Audit Manager, Config]
    
    style Security fill:#FF9900,color:#000
```

---

## 1. AWS IAM (Identity and Access Management)

**Definition:** Service that **manages access** to AWS resources by controlling who is authenticated and authorized to use them.

```mermaid
graph TD
    IAM[🔐 AWS IAM] --> Users[👤 IAM Users<br/>People or applications]
    IAM --> Groups[👥 IAM Groups<br/>Collection of users]
    IAM --> Roles[🎭 IAM Roles<br/>Temporary credentials]
    IAM --> Policies[📜 IAM Policies<br/>Permissions document]
    
    Users --> U1[Long-term credentials<br/>Username + password]
    Roles --> R1[Temporary credentials<br/>For services & users]
    Policies --> P1[JSON document<br/>Allow/Deny permissions]
    
    style IAM fill:#FF9900,color:#000
```

### IAM Core Concepts

| Component | Purpose | Use Case |
|-----------|---------|----------|
| **IAM User** | Identity for a person or application | Individual team members |
| **IAM Group** | Collection of users with shared permissions | All developers, all admins |
| **IAM Role** | Temporary identity with specific permissions | EC2 accessing S3, cross-account access |
| **IAM Policy** | JSON document defining permissions | Allow/deny specific actions |

### IAM Policy Structure

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### IAM Best Practices

```mermaid
graph TD
    Best[🔐 IAM Best Practices] --> B1[👤 One IAM user per person]
    Best --> B2[👥 Use groups for permissions]
    Best --> B3[🎭 Use roles for applications]
    Best --> B4[🔑 Enable MFA for privileged users]
    Best --> B5[📜 Grant least privilege]
    Best --> B6[🔄 Rotate credentials regularly]
    Best --> B7[📊 Use AWS Organizations]
    
    style Best fill:#FF9900,color:#000
    style B1 fill:#51CF66,color:#fff
    style B2 fill:#51CF66,color:#fff
    style B3 fill:#51CF66,color:#fff
```

> 🎯 **Exam Tip:** Follow the **principle of least privilege** — grant only the permissions needed to perform a task. Never use the root account for daily tasks.

### Root Account Best Practices

| Practice | Reason |
|----------|--------|
| **Don't use root for daily tasks** | Create IAM users instead |
| **Enable MFA on root** | Extra security layer |
| **Lock away root credentials** | Use only for account management |
| **Create admin IAM user** | For administrative tasks |

---

## 2. Security Groups

**Definition:** **Virtual firewall** for EC2 instances that controls inbound and outbound traffic at the instance level.

```mermaid
graph LR
    Internet((🌍 Internet)) -->|Inbound traffic| SG[🔒 Security Group]
    SG -->|Allow port 443| EC2[🖥️ EC2 Instance]
    EC2 -->|Outbound| SG
    SG -->|Allow all| Internet
    
    style SG fill:#FF9900,color:#000
    style EC2 fill:#4DABF7,color:#fff
```

### Security Group Characteristics

| Feature | Description |
|---------|-------------|
| **Level** | Instance-level (attached to ENI) |
| **State** | Stateful (return traffic auto-allowed) |
| **Rules** | Allow rules only (no explicit deny) |
| **Default** | Deny all inbound, allow all outbound |
| **Scope** | Specific region and VPC |
| **Attachment** | Can be attached to multiple instances |

### Security Group Rules

| Direction | Default | Common Rules |
|-----------|---------|--------------|
| **Inbound** | Deny all | Allow SSH (22) from specific IP |
| | | Allow HTTP (80) from anywhere |
| | | Allow HTTPS (443) from anywhere |
| **Outbound** | Allow all | Allow all (default) |

### Troubleshooting Connection Issues

```mermaid
graph TD
    Issue[🔍 Connection Issue] --> Q{Error Type?}
    
    Q -->|⏱️ Timeout<br/>Connection hangs| SG[🔒 Security Group Issue<br/>Traffic blocked]
    Q -->|❌ Connection Refused<br/>Immediate rejection| App[💻 Application Issue<br/>Not running or crashed]
    
    SG --> Fix1[Check SG inbound rules]
    App --> Fix2[Check application status<br/>and logs]
    
    style SG fill:#FFD700,color:#000
    style App fill:#FF6B6B,color:#fff
```

> 🎯 **Exam Tip:** **Timeout** = Security Group issue (traffic blocked). **Connection Refused** = Application issue (not running).

### Common Ports Reference

| Port | Protocol | Use Case |
|------|----------|----------|
| **22** | SSH | Logging into Linux instances |
| **21** | FTP | Uploading files to file share |
| **80** | HTTP | Accessing unsecured websites |
| **443** | HTTPS | Accessing secured websites |
| **3389** | RDP | Logging into Windows instances |
| **3306** | MySQL | MySQL database connections |
| **5432** | PostgreSQL | PostgreSQL database connections |

---

## 3. Data Protection & Encryption

```mermaid
graph TD
    Encryption[🔐 AWS Encryption] --> AtRest[💾 Encryption at Rest]
    Encryption --> InTransit[🌐 Encryption in Transit]
    
    AtRest --> AR1[KMS<br/>Key Management Service]
    AtRest --> AR2[S3 Server-Side Encryption]
    AtRest --> AR3[EBS Volume Encryption]
    AtRest --> AR4[RDS Encryption]
    
    InTransit --> IT1[TLS/SSL Certificates]
    InTransit --> IT2[HTTPS endpoints]
    InTransit --> IT3[VPN connections]
    
    style Encryption fill:#FF9900,color:#000
    style AtRest fill:#4DABF7
    style InTransit fill:#51CF66,color:#fff
```

### AWS KMS (Key Management Service)

**Definition:** Managed service that makes it easy to create and control **encryption keys** used to encrypt your data.

```mermaid
graph LR
    App[📱 Application] -->|Encrypt/Decrypt| KMS[🔑 AWS KMS]
    KMS -->|Manages| Keys[Encryption Keys]
    Keys -->|Protect| Data[📊 Your Data]
    
    style KMS fill:#FF9900,color:#000
```

### KMS Key Types

| Type | Description | Use Case |
|------|-------------|----------|
| **AWS Managed Keys** | Created and managed by AWS | Free, automatic rotation |
| **Customer Managed Keys** | You create and manage | Full control, audit trail |
| **Custom Key Store** | Using CloudHSM | Regulatory compliance |

---

## 4. AWS WAF (Web Application Firewall)

**Definition:** Firewall that protects web applications from **common web exploits** (SQL injection, XSS, etc.).

```mermaid
graph LR
    User[👤 User] -->|HTTP/HTTPS| CF[⚡ CloudFront/ALB]
    CF -->|Inspect traffic| WAF[🛡️ AWS WAF]
    WAF -->|Allow or Block| App[📱 Application]
    
    WAF --> Rules[Rules]
    Rules --> R1[IP-based rules]
    Rules --> R2[SQL injection protection]
    Rules --> R3[XSS protection]
    Rules --> R4[Rate limiting]
    
    style WAF fill:#FF9900,color:#000
```

### WAF Protection Features

| Feature | Description |
|---------|-------------|
| **SQL Injection Protection** | Blocks malicious SQL queries |
| **Cross-Site Scripting (XSS)** | Prevents script injection attacks |
| **IP Allow/Block Lists** | Geographic or IP-based restrictions |
| **Rate Limiting** | Prevents DDoS and brute force attacks |
| **Bot Protection** | Identifies and blocks malicious bots |

---

## 5. AWS Shield

**Definition:** **DDoS protection** service that safeguards applications from Distributed Denial of Service attacks.

```mermaid
graph TD
    Shield[🛡️ AWS Shield] --> Standard[Shield Standard<br/>FREE - All AWS customers]
    Shield --> Advanced[Shield Advanced<br/>Paid - Enhanced protection]
    
    Standard --> S1[Network layer protection]
    Standard --> S2[Automatic mitigation]
    
    Advanced --> A1[Application layer protection]
    Advanced --> A2[24/7 DRT response team]
    Advanced --> A3[Cost protection]
    
    style Standard fill:#51CF66,color:#fff
    style Advanced fill:#FFD700,color:#000
```

---

## 6. Monitoring & Logging

```mermaid
graph TD
    Monitor[📊 AWS Monitoring Services] --> CloudTrail[📜 CloudTrail<br/>API audit log]
    Monitor --> CloudWatch[📈 CloudWatch<br/>Metrics & alarms]
    Monitor --> GuardDuty[🚨 GuardDuty<br/>Threat detection]
    Monitor --> Config[⚙️ AWS Config<br/>Resource compliance]
    
    CloudTrail --> CT1[Who did what?<br/>When? From where?]
    CloudWatch --> CW1[Performance metrics<br/>Logs & alarms]
    GuardDuty --> GD1[ML-based threat detection<br/>Anomaly identification]
    Config --> CF1[Resource configuration<br/>Compliance tracking]
    
    style Monitor fill:#FF9900,color:#000
```

### Monitoring Services Comparison

| Service | Purpose | Key Feature |
|---------|---------|-------------|
| **CloudTrail** | Audit API calls | Who, what, when, where |
| **CloudWatch** | Monitor resources | Metrics, logs, alarms |
| **GuardDuty** | Threat detection | ML-based anomaly detection |
| **AWS Config** | Track configurations | Compliance and change history |
| **VPC Flow Logs** | Network traffic | IP traffic information |
| **Trusted Advisor** | Optimization | Cost, security, performance checks |

---

## 7. Compliance Programs

**Definition:** AWS maintains **compliance certifications** and provides tools to help you meet regulatory requirements.

```mermaid
graph TD
    Compliance[📜 AWS Compliance] --> Programs[Certifications]
    Compliance --> Tools[Compliance Tools]
    
    Programs --> P1[SOC 1, 2, 3]
    Programs --> P2[PCI DSS Level 1]
    Programs --> P3[ISO 27001, 27017, 27018]
    Programs --> P4[HIPAA, GDPR]
    Programs --> P5[FedRAMP]
    
    Tools --> T1[AWS Artifact<br/>Compliance reports]
    Tools --> T2[AWS Audit Manager<br/>Audit automation]
    Tools --> T3[AWS Config<br/>Compliance rules]
    
    style Compliance fill:#FF9900,color:#000
```

### AWS Artifact

**Definition:** Self-service portal to **access AWS compliance reports** and agreements.

```mermaid
graph LR
    Artifact[📜 AWS Artifact] --> Reports[Compliance Reports]
    Artifact --> Agreements[Legal Agreements]
    
    Reports --> R1[SOC reports]
    Reports --> R2[PCI reports]
    Reports --> R3[ISO reports]
    
    Agreements --> A1[BAA<br/>Business Associate Agreement]
    Agreements --> A2[NDA]
    
    style Artifact fill:#FF9900,color:#000
```

---

## 8. Data Protection Best Practices

```mermaid
graph TD
    DP[🛡️ Data Protection] --> Encrypt[🔐 Encrypt Everything]
    Encrypt --> E1[Encrypt at rest<br/>S3, EBS, RDS]
    Encrypt --> E2[Encrypt in transit<br/>TLS/SSL, HTTPS]
    
    DP --> Backup[💾 Regular Backups]
    Backup --> B1[Automated backups]
    Backup --> B2[Cross-Region replication]
    Backup --> B3[Test recovery procedures]
    
    DP --> Access[🔑 Access Control]
    Access --> AC1[Least privilege access]
    Access --> AC2[MFA for sensitive operations]
    Access --> AC3[Audit access with CloudTrail]
    
    style DP fill:#FF9900,color:#000
```

### Encryption Decision Tree

```mermaid
flowchart TD
    Start{Need to encrypt data?} --> Q1{Where is data?}
    
    Q1 -->|S3| S3Enc[Enable S3 encryption<br/>SSE-S3, SSE-KMS, SSE-C]
    Q1 -->|EBS volumes| EBSEnc[Enable EBS encryption<br/>at volume creation]
    Q1 -->|RDS databases| RDSEnc[Enable RDS encryption<br/>at instance creation]
    Q1 -->|In transit| TransitEnc[Use TLS/SSL<br/>HTTPS endpoints]
    
    style S3Enc fill:#51CF66,color:#fff
    style EBSEnc fill:#51CF66,color:#fff
    style RDSEnc fill:#51CF66,color:#fff
    style TransitEnc fill:#51CF66,color:#fff
```

> 🎯 **Exam Tip:** Encryption should be enabled **at creation time** for resources like EBS volumes and RDS instances. Enabling later is more complex.

---

## Security Services Summary

| Service | Purpose | Key Use Case |
|---------|---------|--------------|
| **IAM** | Identity & access management | User/role management |
| **Security Groups** | Instance firewall | Network access control |
| **KMS** | Encryption key management | Encrypt data |
| **WAF** | Web application firewall | Protect web apps |
| **Shield** | DDoS protection | Prevent DDoS attacks |
| **CloudTrail** | API audit logging | Track API calls |
| **CloudWatch** | Monitoring & alarms | Performance monitoring |
| **GuardDuty** | Threat detection | Identify security threats |
| **AWS Config** | Configuration compliance | Track resource changes |
| **AWS Artifact** | Compliance reports | Access compliance docs |

---

## Quick Reference

| Concept | Key Point |
|---------|-----------|
| **IAM** | Manage users, groups, roles, policies |
| **Least Privilege** | Grant only necessary permissions |
| **Security Groups** | Stateful instance-level firewall |
| **Network ACLs** | Stateless subnet-level firewall |
| **KMS** | Managed encryption keys |
| **Encryption** | At rest and in transit |
| **WAF** | Web application firewall |
| **Shield** | DDoS protection |
| **CloudTrail** | API audit log |
| **Root Account** | Lock away, use IAM users instead |

---

## 📝 Knowledge Check

<details>
<summary><strong>Q1: According to the AWS Shared Responsibility Model, who is responsible for patching the guest OS on an EC2 instance?</strong></summary>

**A.** AWS  
**B.** The customer  
**C.** Both share equally  
**D.** AWS Support  

**Answer: B** — In the Shared Responsibility Model, AWS manages the host OS and hypervisor, but the customer is responsible for patching and securing the guest operating system on EC2 instances.
</details>

<details>
<summary><strong>Q2: What is the difference between a Security Group and a Network ACL?</strong></summary>

**A.** Security Groups are stateless, NACLs are stateful  
**B.** Security Groups operate at instance level (stateful), NACLs at subnet level (stateless)  
**C.** They are the same thing  
**D.** Security Groups are for Linux, NACLs are for Windows  

**Answer: B** — Security Groups operate at the instance level and are stateful (return traffic is automatically allowed). Network ACLs operate at the subnet level and are stateless (return traffic must be explicitly allowed).
</details>

<details>
<summary><strong>Q3: Which service provides managed encryption keys for protecting your data?</strong></summary>

**A.** AWS WAF  
**B.** AWS Shield  
**C.** AWS KMS  
**D.** AWS GuardDuty  

**Answer: C** — AWS Key Management Service (KMS) makes it easy to create and manage encryption keys used to encrypt your data across AWS services.
</details>

<details>
<summary><strong>Q4: What should you do with the AWS root account?</strong></summary>

**A.** Use it for daily tasks  
**B.** Share it with team members  
**C.** Lock it away, enable MFA, use IAM users instead  
**D.** Delete it  

**Answer: C** — The root account should be locked away, have MFA enabled, and only be used for account management tasks. Create IAM users with appropriate permissions for daily tasks following the principle of least privilege.
</details>

---

## Navigation

⬅️ Previous: [Containers & Orchestration](../03-aws-services/07-containers-orchestration.md) | ➡️ Next: [Cloud Adoption Framework](./02-cloud-adoption-framework.md)  
🏠 [Back to README](../../README.md)

---

*Part of the [AWS Cloud Practitioner Study Notes](../../README.md).*