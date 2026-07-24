# AWS Certified Cloud Practitioner (CLF-C02) — Complete Study Guide

> A detailed, exam-focused reference covering every domain, task statement, and key term you need for the **AWS Certified Cloud Practitioner (CLF-C02)** exam.

---

## Table of Contents

1. [Exam Overview](#1-exam-overview)
2. [Domain 1: Cloud Concepts (24%)](#2-domain-1-cloud-concepts-24)
3. [Domain 2: Security and Compliance (30%)](#3-domain-2-security-and-compliance-30)
4. [Domain 3: Cloud Technology and Services (34%)](#4-domain-3-cloud-technology-and-services-34)
5. [Domain 4: Billing, Pricing, and Support (12%)](#5-domain-4-billing-pricing-and-support-12)
6. [Quick-Reference Service Cheat Sheet](#6-quick-reference-service-cheat-sheet)
7. [Exam-Day Keyword Triggers](#7-exam-day-keyword-triggers)
8. [Study Plan & Resources](#8-study-plan--resources)

---

## 1. Exam Overview

| Attribute | Detail |
|---|---|
| **Exam code** | CLF-C02 (live since September 19, 2023) |
| **Level** | Foundational |
| **Questions** | 65 total — **50 scored**, 15 unscored (unmarked; treat all as scored) |
| **Question types** | Multiple choice (1 correct) and multiple response (2+ correct) |
| **Duration** | 90 minutes |
| **Scoring** | Scaled score 100–1000; **passing score = 700** (pass/fail) |
| **Cost** | 100 USD |
| **Validity** | 3 years |
| **Delivery** | Pearson VUE / PSI test center **or** online proctored |
| **Recommended experience** | ~6 months of AWS exposure (not mandatory) |

### Domain Weightings

| Domain | Name | Weight |
|---|---|---|
| 1 | Cloud Concepts | **24%** |
| 2 | Security and Compliance | **30%** |
| 3 | Cloud Technology and Services | **34%** |
| 4 | Billing, Pricing, and Support | **12%** |

> **Strategy note:** Domains 2 + 3 make up ~64% of scored content. Security/compliance and the core service catalog are where most points live.

---

## 2. Domain 1: Cloud Concepts (24%)

### 2.1 What Is Cloud Computing

**Cloud computing** = on-demand delivery of IT resources (compute, storage, databases, networking) over the internet with **pay-as-you-go pricing**. You provision what you need when you need it, without owning physical hardware.

**Key terms:** on-demand, self-service, provisioning, pay-as-you-go, utility computing.

### 2.2 Six Advantages (Benefits) of Cloud Computing

Memorize these — they appear frequently and by name:

1. **Trade capital expense (CapEx) for variable expense (OpEx)** — pay only for what you consume instead of investing upfront in data centers.
2. **Benefit from massive economies of scale** — AWS aggregates usage from many customers, achieving lower pay-as-you-go prices.
3. **Stop guessing capacity** — scale up or down as needed; no over-provisioning or under-provisioning.
4. **Increase speed and agility** — resources are a click away; provisioning drops from weeks to minutes.
5. **Stop spending money running and maintaining data centers** — focus on your business, not on racking servers.
6. **Go global in minutes** — deploy in multiple AWS Regions worldwide with low latency.

### 2.3 Core Value Concepts

| Term | Meaning |
|---|---|
| **Elasticity** | Automatically acquire resources when needed and release them when not (scale in/out). |
| **Scalability** | Ability to grow (or shrink) capacity to meet demand — vertical (bigger instance) or horizontal (more instances). |
| **Agility** | Speed to build, test, and deploy; rapid experimentation. |
| **High Availability (HA)** | System stays operational with minimal downtime (redundancy across AZs). |
| **Fault tolerance** | System continues operating even when a component fails. |
| **Reliability** | Consistent performance and recovery from failures. |
| **Elastic vs. Scalable** | Elasticity = automatic + reactive; Scalability = capacity headroom. |

### 2.4 Cloud Computing Models (Service Models)

| Model | You Manage | AWS Manages | Example |
|---|---|---|---|
| **IaaS** (Infrastructure as a Service) | OS, apps, data, runtime | Virtualization, servers, storage, networking | Amazon EC2 |
| **PaaS** (Platform as a Service) | Apps, data | OS, runtime, infra | AWS Elastic Beanstalk, RDS |
| **SaaS** (Software as a Service) | Just use it | Everything | Amazon WorkMail, Chime, third-party software |

### 2.5 Cloud Deployment Models

| Model | Description |
|---|---|
| **Cloud (Public)** | Fully in the cloud; born-in-the-cloud or fully migrated. |
| **Hybrid** | Mix of cloud + on-premises (connected via VPN/Direct Connect). Common for gradual migration or compliance. |
| **On-premises (Private cloud)** | Resources deployed in your own data center using virtualization/resource management (e.g., AWS Outposts extends AWS on-prem). |

### 2.6 AWS Well-Architected Framework

A set of best practices for designing reliable, secure, efficient, cost-effective workloads. **Six Pillars:**

1. **Operational Excellence** — run and monitor systems; continuous improvement, automation.
2. **Security** — protect data, systems, assets; identity management, least privilege.
3. **Reliability** — recover from failures, meet demand; auto-scaling, backups.
4. **Performance Efficiency** — use resources efficiently; right-sizing, serverless.
5. **Cost Optimization** — avoid unnecessary costs; pay for what you need.
6. **Sustainability** — minimize environmental impact of workloads.

> **Tool:** AWS Well-Architected Tool reviews workloads against these pillars.

### 2.7 AWS Cloud Adoption Framework (AWS CAF)

Guidance for organizational cloud migration/transformation, organized into **six perspectives** (first three = business capabilities, last three = technical capabilities):

1. **Business** — align IT with business outcomes.
2. **People** — roles, skills, culture, change management.
3. **Governance** — manage/measure cloud investments; risk.
4. **Platform** — build/maintain cloud infrastructure.
5. **Security** — meet security objectives.
6. **Operations** — run, monitor, and recover IT workloads.

### 2.8 Cloud Economics

| Term | Meaning |
|---|---|
| **CapEx** | Capital Expenditure — large upfront hardware/data-center spend (on-prem model). |
| **OpEx** | Operational Expenditure — ongoing pay-as-you-go spend (cloud model). |
| **TCO** | Total Cost of Ownership — full cost of owning/running infrastructure (compute, power, cooling, admin). |
| **Economies of scale** | Lower per-unit cost as AWS serves many customers. |
| **Right-sizing** | Matching instance types/sizes to actual workload needs. |

### 2.9 Cloud Migration Strategies — The "7 Rs"

1. **Rehost** ("lift and shift") — move as-is.
2. **Replatform** ("lift, tinker, and shift") — minor optimizations.
3. **Repurchase** ("drop and shop") — move to a different product (often SaaS).
4. **Refactor / Re-architect** — redesign using cloud-native features.
5. **Relocate** — move infrastructure (e.g., VMware) without changes.
6. **Retain** ("revisit") — keep on-prem for now.
7. **Retire** — decommission what's no longer needed.

---

## 3. Domain 2: Security and Compliance (30%)

### 3.1 AWS Shared Responsibility Model

The single most-tested concept in this domain. Responsibility is **split**:

| Party | Responsible For | Phrase |
|---|---|---|
| **AWS** | Security **OF** the cloud | Hardware, global infrastructure (Regions, AZs, edge), managed-service software, physical facilities, networking, virtualization host |
| **Customer** | Security **IN** the cloud | Data, IAM, OS patching (on EC2), network/firewall config, encryption settings, application security, client-side data |

**Shifts by service type:**
- **IaaS (EC2):** customer manages OS patches, security groups, app security → more responsibility.
- **PaaS (RDS, Lambda):** AWS handles OS/runtime patching; customer manages data, access, config.
- **SaaS:** AWS handles almost everything; customer manages data and user access.

**Always the customer's job:** data classification, IAM/credentials, and encryption choices.

### 3.2 AWS Identity and Access Management (IAM)

Free, global service to control **authentication** (who) and **authorization** (what they can do).

| Component | Definition |
|---|---|
| **Root user** | Account owner created at signup; **full access**. Lock it down: enable MFA, don't use for daily tasks, delete/rotate root access keys. |
| **IAM User** | An identity for a person or app; has long-term credentials. |
| **IAM Group** | Collection of users; attach policies to manage many users at once. |
| **IAM Role** | Temporary credentials assumed by users, apps, or AWS services (no long-term keys). Used for **cross-account access** and **EC2 → service** access. |
| **IAM Policy** | JSON document defining permissions (Effect, Action, Resource, Condition). Attached to users, groups, or roles. |
| **Principle of Least Privilege** | Grant only the permissions required — a core best practice. |
| **MFA** | Multi-Factor Authentication — adds a second factor (device/token) beyond a password. |
| **IAM Identity Center** (formerly AWS SSO) | Centralized SSO access to multiple AWS accounts and applications. |

**Policy types:** Identity-based, Resource-based, AWS-managed vs. Customer-managed, SCPs (Service Control Policies — org-level guardrails).

### 3.3 AWS Security Services

| Service | Purpose |
|---|---|
| **AWS Shield** | DDoS protection. **Standard** (free, automatic); **Advanced** (paid, enhanced, 24/7 DRT support). |
| **AWS WAF** (Web Application Firewall) | Filters malicious web traffic (SQL injection, XSS) at CloudFront/ALB/API Gateway. |
| **Amazon GuardDuty** | Intelligent **threat detection** — continuously monitors for malicious activity using ML/logs. |
| **Amazon Inspector** | Automated **vulnerability** assessment for EC2, containers, and Lambda. |
| **Amazon Macie** | Uses ML to **discover and protect sensitive data** (e.g., PII) in Amazon S3. |
| **AWS KMS** (Key Management Service) | Create/manage **encryption keys**; integrated with most AWS services. |
| **AWS CloudHSM** | Dedicated **hardware security module** for key storage (single-tenant, compliance). |
| **AWS Secrets Manager** | Store/rotate secrets (DB passwords, API keys). |
| **AWS Certificate Manager (ACM)** | Provision/manage SSL/TLS certificates. |
| **AWS Firewall Manager** | Centrally manage WAF/Shield/security-group rules across accounts. |
| **AWS Network Firewall** | Managed network firewall for VPCs. |
| **Amazon Detective** | Analyze/investigate security findings root cause. |
| **AWS Security Hub** | Central dashboard aggregating security findings and compliance checks. |
| **AWS Audit Manager** | Automates evidence collection for audits. |

### 3.4 Encryption

| Term | Meaning |
|---|---|
| **Encryption at rest** | Data encrypted while stored (S3, EBS, RDS) — via KMS. |
| **Encryption in transit** | Data encrypted while moving (TLS/SSL, HTTPS). |
| **Symmetric vs. asymmetric** | Same key vs. public/private key pair. |

### 3.5 Compliance & Governance

| Service / Term | Purpose |
|---|---|
| **AWS Artifact** | Self-service portal for **compliance reports** (SOC, PCI, ISO) and agreements. |
| **AWS Config** | Records/audits resource configurations; checks compliance over time. |
| **AWS CloudTrail** | Logs **API calls / account activity** (who did what, when) — auditing & governance. |
| **Amazon CloudWatch** | Monitoring/observability — metrics, logs, alarms, dashboards. |
| **AWS Trusted Advisor** | Recommendations across cost, performance, **security**, fault tolerance, service limits. |
| **Compliance programs** | HIPAA, GDPR, PCI DSS, SOC 1/2/3, ISO 27001, FedRAMP — AWS supports many; check Artifact. |

### 3.6 Security Best Practices Summary

- Enable **MFA** everywhere, especially root.
- Follow **least privilege**; use **roles** over long-term keys.
- Rotate credentials; don't embed keys in code.
- Use **AWS Organizations + SCPs** for multi-account guardrails.
- Encrypt data at rest and in transit.
- Monitor with CloudTrail + CloudWatch + GuardDuty.

---

## 4. Domain 3: Cloud Technology and Services (34%)

### 4.1 Ways to Interact with AWS

| Method | Description |
|---|---|
| **AWS Management Console** | Web-based GUI. |
| **AWS CLI** | Command-line interface for scripting/automation. |
| **AWS SDKs** | Language-specific libraries (Python/Boto3, Java, JS, etc.) to build apps. |
| **Infrastructure as Code (IaC)** | **AWS CloudFormation** (templates), **AWS CDK** (code). |

### 4.2 AWS Global Infrastructure

| Term | Definition |
|---|---|
| **Region** | Geographic area with multiple isolated data centers (e.g., us-east-1). Choose based on latency, compliance, price, service availability. |
| **Availability Zone (AZ)** | One or more discrete data centers within a Region, isolated for fault tolerance. Deploy across **multiple AZs** for HA. |
| **Edge Location** | Endpoints for **content caching** (CloudFront) — closer to users, lower latency. More numerous than Regions. |
| **Regional Edge Cache** | Larger cache between origin and edge locations. |
| **AWS Local Zones** | Place compute/storage closer to large population centers (low latency). |
| **AWS Wavelength** | Embeds compute in 5G networks for ultra-low latency. |
| **AWS Outposts** | Runs AWS infrastructure **on-premises** (hybrid). |

### 4.3 Compute Services

| Service | Description |
|---|---|
| **Amazon EC2** | Resizable **virtual servers**. Instance families: General Purpose (T, M), Compute Optimized (C), Memory Optimized (R, X), Storage Optimized (I, D), Accelerated/GPU (P, G). |
| **AWS Lambda** | **Serverless** functions; run code without managing servers; pay per request/duration; event-driven. |
| **Amazon ECS** | Elastic Container Service — Docker container orchestration (AWS-native). |
| **Amazon EKS** | Managed **Kubernetes**. |
| **AWS Fargate** | **Serverless compute for containers** (no EC2 management) — works with ECS/EKS. |
| **Amazon ECR** | Elastic Container Registry — store Docker images. |
| **AWS Elastic Beanstalk** | **PaaS** — deploy/manage web apps; AWS handles capacity, scaling, load balancing. |
| **AWS Batch** | Run batch computing jobs at scale. |
| **Amazon Lightsail** | Simple, low-cost VPS for small apps/websites (bundled pricing). |
| **AWS Outposts** | AWS compute on-premises. |
| **EC2 Auto Scaling** | Automatically add/remove EC2 instances based on demand. |
| **Elastic Load Balancing (ELB)** | Distributes traffic across targets. Types: **ALB** (HTTP/HTTPS, layer 7), **NLB** (TCP/UDP, layer 4, high perf), **GWLB** (appliances), **CLB** (legacy). |

**EC2 tenancy:** Shared, Dedicated Instances, Dedicated Hosts.

### 4.4 Storage Services

| Service | Type | Description |
|---|---|---|
| **Amazon S3** | Object storage | Virtually unlimited, durable (11 nines), scalable. Data in **buckets**. |
| **Amazon EBS** | Block storage | Persistent volumes attached to EC2 (like a virtual hard drive); AZ-scoped. |
| **Amazon EFS** | File storage | Managed elastic **NFS** file system; shared across many EC2s; Linux. |
| **Amazon FSx** | File storage | Managed file systems: **FSx for Windows**, **FSx for Lustre** (HPC), NetApp ONTAP, OpenZFS. |
| **AWS Storage Gateway** | Hybrid | Connect on-prem to cloud storage. |
| **AWS Backup** | Backup | Centralized backup across AWS services. |
| **AWS Snow Family** | Data transfer | **Snowcone / Snowball / Snowmobile** — physically move large data to AWS (offline). |

**Amazon S3 Storage Classes:**
- **S3 Standard** — frequent access.
- **S3 Intelligent-Tiering** — auto-moves data between tiers based on access.
- **S3 Standard-IA** (Infrequent Access) / **S3 One Zone-IA** — cheaper, less frequent.
- **S3 Glacier Instant Retrieval / Flexible Retrieval / Deep Archive** — archival, lowest cost, higher retrieval time.
- **S3 features:** versioning, lifecycle policies, encryption, static website hosting, cross-region replication.

### 4.5 Database Services

| Service | Type | Description |
|---|---|---|
| **Amazon RDS** | Relational (managed) | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server. Handles patching, backups, HA (Multi-AZ). |
| **Amazon Aurora** | Relational | AWS's MySQL/PostgreSQL-compatible DB; up to 5x/3x faster; highly available. |
| **Amazon DynamoDB** | NoSQL | Serverless key-value/document DB; single-digit millisecond latency; auto-scaling. |
| **Amazon Redshift** | Data warehouse | Petabyte-scale analytics / OLAP. |
| **Amazon ElastiCache** | In-memory cache | Redis / Memcached — sub-millisecond performance. |
| **Amazon DocumentDB** | Document (MongoDB-compatible) | JSON document DB. |
| **Amazon Neptune** | Graph | Graph database. |
| **Amazon MemoryDB** | In-memory (Redis) | Durable in-memory DB. |
| **Amazon Keyspaces** | Wide-column | Apache Cassandra-compatible. |
| **Amazon QLDB** | Ledger | Immutable, cryptographically verifiable transaction log. |
| **AWS DMS** | Migration | Database Migration Service — migrate DBs with minimal downtime. |

### 4.6 Networking & Content Delivery

| Service | Description |
|---|---|
| **Amazon VPC** | Virtual Private Cloud — isolated virtual network; define subnets, route tables, gateways. |
| **Subnets** | Public (internet-facing) / Private segments within a VPC. |
| **Security Groups** | **Stateful** virtual firewall at instance level (allow rules only). |
| **Network ACLs (NACLs)** | **Stateless** firewall at subnet level (allow + deny rules). |
| **Internet Gateway (IGW)** | Enables VPC internet access. |
| **NAT Gateway** | Lets private subnets reach the internet outbound only. |
| **Amazon Route 53** | Scalable **DNS** + domain registration + health checks + routing policies (latency, geolocation, weighted, failover). |
| **Amazon CloudFront** | **CDN** — caches content at edge locations for low latency. |
| **AWS Direct Connect** | Dedicated **private physical** network link from on-prem to AWS (consistent, high bandwidth). |
| **AWS VPN** | Encrypted connection over the internet (Site-to-Site, Client VPN). |
| **AWS Transit Gateway** | Hub connecting multiple VPCs and on-prem networks. |
| **Elastic Load Balancing** | Distributes incoming traffic (see Compute). |
| **AWS Global Accelerator** | Improves availability/performance using the AWS global network. |
| **VPC Peering** | Connect two VPCs privately. |
| **AWS PrivateLink** | Private connectivity to services without internet exposure. |

### 4.7 Analytics Services

| Service | Description |
|---|---|
| **Amazon Athena** | Serverless SQL queries directly on S3 data. |
| **Amazon Kinesis** | Real-time streaming data ingestion/processing. |
| **AWS Glue** | Serverless ETL (extract, transform, load) + data catalog. |
| **Amazon EMR** | Managed big-data (Hadoop, Spark). |
| **Amazon QuickSight** | Business intelligence / dashboards. |
| **Amazon OpenSearch Service** | Search and log analytics. |
| **AWS Lake Formation** | Build/manage data lakes. |
| **Amazon Redshift** | Data warehousing (see Databases). |

### 4.8 Application Integration

| Service | Description |
|---|---|
| **Amazon SQS** | Simple Queue Service — fully managed **message queue** (decoupling). |
| **Amazon SNS** | Simple Notification Service — **pub/sub** messaging, notifications (SMS/email/HTTP). |
| **Amazon EventBridge** | Serverless event bus connecting apps. |
| **AWS Step Functions** | Orchestrate workflows / state machines. |
| **Amazon MQ** | Managed message broker (ActiveMQ/RabbitMQ). |
| **Amazon API Gateway** | Create/publish/manage APIs at scale. |

### 4.9 Management & Governance

| Service | Description |
|---|---|
| **Amazon CloudWatch** | Metrics, logs, alarms, dashboards (monitoring). |
| **AWS CloudTrail** | API activity/audit logging. |
| **AWS Config** | Track/audit resource configuration and compliance. |
| **AWS CloudFormation** | Infrastructure as Code via templates (JSON/YAML). |
| **AWS Systems Manager** | Operational management of resources at scale (patching, run commands). |
| **AWS Organizations** | Central multi-account management + consolidated billing + SCPs. |
| **AWS Control Tower** | Set up/govern a secure multi-account landing zone. |
| **AWS Trusted Advisor** | Best-practice recommendations (5 categories). |
| **AWS License Manager** | Manage software licenses. |
| **AWS Health Dashboard** | Status of AWS services + personalized account events. |
| **AWS Well-Architected Tool** | Review workloads vs. framework pillars. |
| **AWS Managed Services (AMS)** | AWS operates your infrastructure for you. |

### 4.10 Developer Tools

| Service | Description |
|---|---|
| **AWS CodeCommit** | Managed Git repositories. |
| **AWS CodeBuild** | Build/test code. |
| **AWS CodeDeploy** | Automate deployments. |
| **AWS CodePipeline** | CI/CD orchestration. |
| **AWS Cloud9** | Cloud-based IDE. |
| **AWS X-Ray** | Analyze/debug distributed applications. |
| **AWS CDK** | Define infrastructure in familiar programming languages. |

### 4.11 AI / Machine Learning Services

| Service | Description |
|---|---|
| **Amazon SageMaker** | Build, train, deploy ML models end-to-end. |
| **Amazon Rekognition** | Image/video analysis. |
| **Amazon Comprehend** | Natural language processing (NLP). |
| **Amazon Polly** | Text-to-speech. |
| **Amazon Transcribe** | Speech-to-text. |
| **Amazon Translate** | Language translation. |
| **Amazon Lex** | Conversational chatbots. |
| **Amazon Textract** | Extract text/data from documents. |
| **Amazon Kendra** | Intelligent enterprise search. |
| **Amazon Q** | Generative-AI assistant. |
| **Amazon Bedrock** | Build generative-AI apps with foundation models. |

### 4.12 Migration & Transfer

| Service | Description |
|---|---|
| **AWS Migration Hub** | Track migrations in one place. |
| **AWS Application Migration Service (MGN)** | Lift-and-shift servers to AWS. |
| **AWS DMS** | Database Migration Service. |
| **AWS DataSync** | Automate data transfer to/from AWS. |
| **AWS Snow Family** | Offline bulk data transfer. |
| **AWS Transfer Family** | Managed SFTP/FTPS/FTP into S3/EFS. |


---

## 5. Domain 4: Billing, Pricing, and Support (12%)

### 5.1 Fundamentals of AWS Pricing

AWS pricing rests on **three core principles**:
1. **Pay for what you use** — pay-as-you-go, no long-term commitment required.
2. **Pay less when you reserve** — commit (Reserved/Savings Plans) for lower rates.
3. **Pay less with volume-based discounts / as AWS grows** — the more you use, the less per unit; prices drop over time.

**Free options:** Always Free, 12-Months Free, and Trials under the **AWS Free Tier**.

### 5.2 EC2 Pricing Models

| Model | Description | Best For |
|---|---|---|
| **On-Demand** | Pay per second/hour, no commitment. | Short-term, unpredictable, dev/test. |
| **Reserved Instances (RIs)** | 1- or 3-year commitment for big discount (up to ~72%). Standard vs. Convertible. | Steady, predictable workloads. |
| **Savings Plans** | Commit to $/hour of usage for 1–3 years (flexible across services). | Consistent compute spend, flexibility. |
| **Spot Instances** | Bid on spare capacity (up to ~90% off); can be interrupted. | Fault-tolerant, flexible, batch. |
| **Dedicated Hosts** | Physical server dedicated to you. | Compliance / licensing (BYOL). |
| **Dedicated Instances** | Isolated hardware at instance level. | Isolation requirements. |
| **Capacity Reservations** | Reserve capacity in a specific AZ. | Guaranteed availability. |

### 5.3 Billing & Cost Management Tools

| Tool | Purpose |
|---|---|
| **AWS Pricing Calculator** | Estimate costs **before** deploying. |
| **AWS Billing Dashboard** | View/manage bills. |
| **AWS Cost Explorer** | Visualize, analyze, and forecast spend over time. |
| **AWS Budgets** | Set custom cost/usage thresholds + alerts. |
| **AWS Cost and Usage Report (CUR)** | Most detailed, granular billing data. |
| **AWS Cost Anomaly Detection** | ML-based detection of unusual spend. |
| **Cost Allocation Tags** | Tag resources to categorize/track costs. |
| **AWS Trusted Advisor** | Cost-optimization recommendations. |
| **Amazon CloudWatch Billing Alarms** | Alert when charges exceed a threshold. |

### 5.4 AWS Organizations & Consolidated Billing

| Term | Description |
|---|---|
| **AWS Organizations** | Manage multiple AWS accounts centrally. |
| **Consolidated Billing** | One bill for all accounts; **volume discounts** aggregated across accounts; **shared Free Tier / Reserved Instance benefits**. |
| **Organizational Units (OUs)** | Group accounts for policy management. |
| **Service Control Policies (SCPs)** | Guardrails limiting what accounts/OUs can do. |
| **Management (payer) account** | The root account that pays and administers. |

### 5.5 AWS Support Plans

| Plan | Cost | Key Features |
|---|---|---|
| **Basic** | Free | Docs, whitepapers, forums, **Trusted Advisor (core checks)**, Personal Health Dashboard. |
| **Developer** | Paid (from ~$29/mo) | Business-hours **email** access to Cloud Support Associates; general guidance. |
| **Business** | Paid (from ~$100/mo) | **24/7 phone/email/chat**, full Trusted Advisor, third-party software support, **1-hour response** for production-down. |
| **Enterprise On-Ramp** | Paid (from ~$5,500/mo) | **30-min** response for business-critical, pool of TAMs, Cost Optimization workshops. |
| **Enterprise** | Paid (from ~$15,000/mo) | **15-min** response for business-critical, dedicated **Technical Account Manager (TAM)**, Concierge, well-architected reviews, IEM. |

**Key differentiators to memorize:**
- **TAM (dedicated)** → Enterprise On-Ramp (pool) & Enterprise (dedicated).
- **Full Trusted Advisor** → Business plan and above.
- **24/7 phone support** → Business plan and above.
- **Fastest response (15 min)** → Enterprise.

### 5.6 Other Support & Resources

| Resource | Purpose |
|---|---|
| **AWS Support Center** | Create/track support cases. |
| **AWS Trusted Advisor** | 5 categories: Cost Optimization, Performance, Security, Fault Tolerance, Service Limits. |
| **AWS Health Dashboard** | Service health + personalized events. |
| **AWS Knowledge Center / re:Post** | Community Q&A. |
| **AWS Professional Services / Partner Network (APN)** | Consultants and partners. |
| **AWS Marketplace** | Buy/deploy third-party software; consolidated billing. |
| **AWS IQ** | On-demand help from AWS-certified experts. |
| **AWS Managed Services (AMS)** | AWS operates infrastructure on your behalf. |

---

## 6. Quick-Reference Service Cheat Sheet

| Need | Service |
|---|---|
| Virtual server | Amazon EC2 |
| Serverless function | AWS Lambda |
| Run containers (serverless) | AWS Fargate |
| Object storage | Amazon S3 |
| Block storage for EC2 | Amazon EBS |
| Shared file storage (Linux) | Amazon EFS |
| Managed relational DB | Amazon RDS / Aurora |
| Managed NoSQL DB | Amazon DynamoDB |
| Data warehouse | Amazon Redshift |
| In-memory cache | Amazon ElastiCache |
| DNS | Amazon Route 53 |
| CDN | Amazon CloudFront |
| Private network | Amazon VPC |
| Dedicated on-prem link | AWS Direct Connect |
| Message queue | Amazon SQS |
| Pub/sub notifications | Amazon SNS |
| Monitoring & metrics | Amazon CloudWatch |
| API activity logging | AWS CloudTrail |
| Config auditing | AWS Config |
| Infrastructure as code | AWS CloudFormation |
| Identity & access | AWS IAM |
| DDoS protection | AWS Shield |
| Web app firewall | AWS WAF |
| Threat detection | Amazon GuardDuty |
| Sensitive data discovery (S3) | Amazon Macie |
| Vulnerability scanning | Amazon Inspector |
| Compliance reports | AWS Artifact |
| Encryption keys | AWS KMS |
| Cost analysis | AWS Cost Explorer |
| Cost alerts | AWS Budgets |
| Cost estimate (pre-deploy) | AWS Pricing Calculator |
| Best-practice checks | AWS Trusted Advisor |
| Multi-account management | AWS Organizations |
| Offline bulk data transfer | AWS Snow Family |
| PaaS web app deploy | AWS Elastic Beanstalk |
| Simple low-cost VPS | Amazon Lightsail |

---

## 7. Exam-Day Keyword Triggers

Learn to map question **keywords** to the right answer:

| If the question says... | Think... |
|---|---|
| "Most cost-effective" / "lowest cost" | Spot, S3 Glacier, Reserved, right-sizing |
| "Least operational overhead" / "fully managed" | Serverless: Lambda, Fargate, S3, DynamoDB, RDS |
| "Highly available" / "fault tolerant" | Multiple AZs, Auto Scaling, ELB, Multi-AZ RDS |
| "Global users / low latency content" | CloudFront (CDN), Route 53, Global Accelerator |
| "Dedicated private connection to on-prem" | AWS Direct Connect |
| "Encrypted connection over internet" | AWS VPN |
| "Who did what in my account" (audit) | AWS CloudTrail |
| "Monitor performance / set alarms" | Amazon CloudWatch |
| "Compliance report / audit artifact" | AWS Artifact |
| "Detect threats automatically" | Amazon GuardDuty |
| "Find PII in S3" | Amazon Macie |
| "Recommendations across cost/security/perf" | AWS Trusted Advisor |
| "Estimate cost before building" | AWS Pricing Calculator |
| "Decouple application components" | Amazon SQS / SNS |
| "Temporary credentials / cross-account" | IAM Roles |
| "One bill for many accounts" | AWS Organizations (Consolidated Billing) |
| "Dedicated technical account manager (TAM)" | Enterprise / Enterprise On-Ramp Support |
| "Extend AWS to on-premises" | AWS Outposts |

---

## 8. Study Plan & Resources

### Suggested 3–4 Week Plan

- **Week 1 — Cloud Concepts + Global Infrastructure:** benefits of cloud, Well-Architected pillars, CAF, Regions/AZs/edge.
- **Week 2 — Security & Compliance:** Shared Responsibility Model, IAM deeply, security services, encryption, Artifact/CloudTrail/Config.
- **Week 3 — Core Services:** compute, storage (S3 classes!), databases, networking (VPC, security groups vs. NACLs).
- **Week 4 — Billing/Pricing/Support + practice exams:** pricing models, cost tools, support plans, then full-length practice tests.

### Readiness Signal

Consistently scoring **80%+ on multiple full-length practice exams** is a strong sign you're ready (aim above the 700/1000 pass line with margin).

### Official & Recommended Resources

- **AWS Skill Builder** — free *AWS Certified Cloud Practitioner* course + official practice question set.
- **AWS Exam Guide (CLF-C02)** and **Sample Questions** PDF — the source of truth for scope.
- **AWS Whitepapers:** *Overview of Amazon Web Services*, *How AWS Pricing Works*, *AWS Well-Architected Framework*, *AWS Cloud Adoption Framework*.
- **AWS Free Tier** — hands-on practice.
- Third-party practice exams (e.g., Tutorials Dojo) for realistic question banks.

### Exam-Day Tips

- Arrive/log in 15–30 minutes early.
- Read carefully — watch for "MOST," "LEAST," "BEST," and multiple-response cues.
- Eliminate obviously wrong options first.
- **Flag and move on** — you have plenty of time (65 questions / 90 min); revisit hard ones.
- No penalty for guessing — **answer every question**.

---

*This guide covers the CLF-C02 exam scope at a foundational level. Always confirm the latest details against the official AWS Certified Cloud Practitioner exam guide, as AWS periodically updates services and content.*
