Here's the **expanded GitHub README** that includes **tiny, simple foundational topics** for both Azure and GCP – things that AWS experts often overlook but are critical to know.

```markdown
# AWS → Azure → GCP: Service Mapping & 20-Day Mastery Guide

> **For AWS experts** who need to become proficient in Azure and GCP quickly.  
> Focus on parallels, not re-learning. Complete in 20 days (3-4 hours/day).

---

## 📋 Table of Contents
- [Core Service Mapping](#-core-service-mapping)
- [Tiny but Critical Topics (Azure & GCP)](#-tiny-but-critical-topics-azure--gcp)
- [Extended Service Mappings](#-extended-service-mappings)
- [One-Liner Takeaways](#-one-liner-takeaways)
- [20-Day Learning Sprint](#-20-day-learning-sprint)
- [Daily Non-Negotiables](#-daily-non-negotiables)
- [Pitfalls to Avoid](#-pitfalls-to-avoid)
- [CLI Quick Reference](#-cli-quick-reference)

---

## 📊 Core Service Mapping Table

| AWS Service | Azure Equivalent | GCP Equivalent | Key Differences (Across All Three) |
|-------------|------------------|----------------|--------------------------------------|
| **EC2** | Virtual Machines | Compute Engine | • **Azure**: Availability Sets (fault/update domains) vs AWS AZs<br>• **GCP**: Zonal MIGs + global load balancing<br>• GCP has **sole-tenant nodes** (like AWS Dedicated Hosts) |
| **S3** | Storage Accounts (Blob) | Cloud Storage | • **Azure**: Containers inside storage accounts; Hot/Cool/Archive tiers<br>• **GCP**: Buckets (globally unique names); **Autoclass** for auto-tiering<br>• GCP has **object versioning** but no bucket-level version toggle |
| **IAM** | Entra ID + RBAC | Cloud IAM | • **Azure**: Separates identity (Entra ID) from permissions (RBAC)<br>• **GCP**: **Primitive/Predefined/Custom** roles; service accounts are first-class<br>• GCP allows **IAM conditions** (like AWS condition keys) |
| **VPC** | Virtual Network (VNet) | VPC (Global) | • **Azure**: Subnets mandatory; NSGs = Security Groups<br>• **GCP**: **Global VPC** (subnets per region); firewall rules are global<br>• GCP has **Shared VPC** (org-level) like AWS Transit Gateway |
| **Lambda** | Azure Functions | Cloud Functions + Cloud Run | • **Azure**: Durable Functions for stateful workflows<br>• **GCP**: Cloud Functions Gen 2; **Cloud Run** (containers) is more popular<br>• GCP has **1st gen vs 2nd gen** – 2nd gen supports longer timeouts |
| **RDS** | Azure SQL / PostgreSQL flexible server | Cloud SQL | • **Azure**: Managed instance vs single server<br>• **GCP**: **Cloud SQL**; **AlloyDB** (PostgreSQL-compatible, faster)<br>• GCP has **Cloud Spanner** (global SQL) – no direct AWS equivalent |

---

## 🧵 Tiny but Critical Topics (Azure & GCP)

> **For AWS experts**: These are the "simple" things that will trip you up if you skip them.

### 🔷 Azure – The Small Stuff

| Tiny Topic | What AWS Users Assume | Azure Reality |
|------------|----------------------|----------------|
| **Resource Groups** | "Like a tag or CloudFormation stack" | **Everything must be in one.** You can't create a VM without specifying a resource group. Resources can't live outside one. |
| **Subscriptions** | "Like an AWS account" | Sort of, but **one Azure AD tenant can have multiple subscriptions**. Billing and policy boundaries live here. |
| **Management Groups** | "Like AWS Organizations" | Yes, but they sit **above subscriptions**. Hierarchies: Management Group → Subscription → Resource Group → Resource. |
| **Regions vs Availability Zones** | "Regions have AZs" | Azure has **region pairs** (e.g., East US + West US) for disaster recovery. Not all regions have AZs. Check before deploying. |
| **Azure CLI Structure** | `aws service action` | `az group create` (noun-verb order). Group first, then resource type. `az vm create`, `az storage account create`. |
| **Storage Account Access Tiers** | "S3 has standard, IA, Glacier" | Hot → Cool → Archive (30-day minimum for Cool, 180 for Archive). **Tier can only go from Hot → Cool → Archive, not back easily.** |
| **NSG Rules** | "Like Security Groups" | **Stateful** (like AWS), but rules are evaluated in **priority order** (lower numbers first). Default deny at end. |
| **Azure Functions Triggers** | "Like Lambda triggers" | Must have a **trigger binding** defined in `function.json`. HTTP, Blob, Queue, Timer – each requires explicit configuration. |
| **Managed Disks** | "Like EBS" | **OS Disk + Data Disk**. OS disk is 30GB by default (Windows) or 30GB (Linux). Can't detach OS disk while VM runs. |
| **Public IP Types** | "Elastic IP" | **Basic vs Standard SKU**. Standard is zone-redundant, Basic is not. Standard requires NSG rules to allow traffic. |

### 🔷 GCP – The Small Stuff

| Tiny Topic | What AWS Users Assume | GCP Reality |
|------------|----------------------|--------------|
| **Projects** | "Like an AWS account" | **Everything lives in a project.** Project ID is globally unique. Billing, APIs, IAM, resources – all scoped to project. |
| **Folders** | "Like AWS Organizations OUs" | Yes. Hierarchy: Organization → Folder → Project → Resource. **Required for org-level policy.** |
| **Labels vs Tags** | "Tags = metadata" | **Labels** (key-value pairs, like AWS tags). **Network Tags** (firewall rules). **IAM Tags** (conditional access). Three different things. |
| **Regions & Zones** | "Region has multiple AZs" | Yes, but GCP has **3-4 zones per region**. Some regions have fewer. `us-central1` has `a,b,c,f` (no `d` or `e`). |
| **gcloud Config** | "CLI just works" | **You must set:** `gcloud config set project PROJECT_ID`. Also `gcloud config set compute/region` and `compute/zone`. Forgetting = wrong project. |
| **Service Accounts** | "IAM roles for services" | **First-class resources.** Create them, then assign to Compute Engine, Cloud Functions, etc. **Can't attach IAM role directly** – must attach service account. |
| **Cloud Storage Buckets** | "S3 bucket names" | **Globally unique across all GCP.** Not just your project. `gs://my-bucket` must be unique worldwide. |
| **Firewall Rules** | "Security Groups" | **Global by default.** Rules apply to all regions unless restricted by `source_ranges` or `target_tags`. Priority order (lower = higher priority). |
| **Preemptible VMs** | "Spot Instances" | **Max 24-hour runtime.** No auto-recovery. 60-80% cheaper. Great for batch jobs, bad for web servers. |
| **Cloud Run vs Cloud Functions** | "Lambda vs Fargate" | **Cloud Run** (containers, any runtime) is more flexible. **Cloud Functions** (code only) is simpler but limited. GCP recommends Cloud Run for new projects. |

### 🔷 Both Azure & GCP – Common Confusions

| Confusion | AWS Way | Azure/GCP Reality |
|-----------|---------|--------------------|
| **Global Resources** | Some services are global (IAM, CloudFront) | **Azure**: Some are global (Entra ID, Front Door), most are regional. **GCP**: VPC is global, disks are zonal. |
| **Default Limits** | Soft limits, can request increase | **Azure**: Hard limits per subscription. **GCP**: Quotas per project, can request increase but takes time. |
| **Free Tier** | 12 months for new accounts | **Azure**: 12 months + always-free services. **GCP**: $300 credit for 90 days + always-free services (e.g., f1-micro VM). |
| **Billing Alerts** | CloudWatch billing alarms | **Azure**: Cost Management + budgets. **GCP**: Budgets & alerts in Billing console. Both require separate setup. |
| **CLI Output Formats** | `--output json` (default) | **Azure**: `--output table` (default). Use `--output json`. **GCP**: JSON by default. Use `--format=json`. |

---

## 🔄 Extended Service Mappings

| AWS Service | Azure Equivalent | GCP Equivalent |
|-------------|------------------|----------------|
| **DynamoDB** | Cosmos DB (NoSQL) | Firestore + Bigtable |
| **EKS (K8s)** | AKS | GKE |
| **CloudFront** | Azure Front Door + CDN | Cloud CDN |
| **Route 53** | Azure DNS | Cloud DNS |
| **CloudWatch** | Azure Monitor | Cloud Monitoring + Logging |
| **Secrets Manager** | Key Vault | Secret Manager |
| **SQS** | Storage Queues + Service Bus | Pub/Sub |
| **SNS** | Service Bus Topics + Event Grid | Pub/Sub |
| **CloudFormation** | ARM/Bicep | Deployment Manager |
| **Elastic Beanstalk** | App Service | App Engine |
| **EBS** | Managed Disks | Persistent Disks |
| **ELB** | Azure Load Balancer + Application Gateway | Cloud Load Balancing |

---

## 🎯 One-Liner Takeaways

| Cloud | Philosophy |
|-------|------------|
| **Azure** | "Everything is a resource group + role assignment – think hierarchical management first." |
| **GCP** | "Global networking, projects as isolation units, and push serverless (Cloud Run) before VMs." |

---

## 🗓️ 20-Day Learning Sprint

### Days 1–4: Azure Foundation
- Map EC2 → VM, S3 → Blob, IAM → Entra ID + RBAC
- **Tiny topics**: Resource Groups, Subscriptions, NSG priority rules
- Deploy VM + Storage Account + VNet manually
- Use Azure CLI (`az vm create`), set up RBAC roles
- Deploy Azure Function triggered by Blob upload

### Days 5–8: Azure Depth
- Resource Groups, Azure Policy, Management Groups
- Managed Identity, Storage tiers (Hot/Cool/Archive)
- Build: Static site + Azure Functions + Cosmos DB
- **Tiny topic**: Azure Function triggers vs Lambda triggers

### Days 9–12: GCP Foundation
- Map EC2 → Compute Engine, S3 → Cloud Storage, IAM → Cloud IAM
- Understand **Global VPC** (subnets per region)
- **Tiny topics**: Projects, Folders, Service Accounts, gcloud config
- Deploy VM + Cloud Storage + Cloud Function
- Learn **GKE** and **BigQuery**

### Days 13–16: GCP Depth
- Shared VPC, Cloud Run, Filestore, Cloud Spanner
- **Tiny topics**: Preemptible VMs, Cloud Run vs Cloud Functions
- Deploy container to Cloud Run + Cloud SQL + IAM

### Days 17–18: Multi-Cloud & Tooling
- Write Terraform config for all three clouds
- Learn workload identity federation

### Days 19–20: Migration Drills
- Mock migrations: AWS → Azure → GCP
- Practice CLI side-by-side
- Convert IAM policies across all three

---

## 🧠 Daily Non-Negotiables

| Time | Action |
|------|--------|
| **Morning (15 min)** | Write 3 AWS services → map to Azure + GCP |
| **Hands-on (2 hrs)** | Deploy using CLI only (no console after day 2) |
| **Evening (30 min)** | Note 1 "WTF difference" and solve it |

---

## ⚠️ Pitfalls to Avoid

| Cloud | Common Trap |
|-------|--------------|
| **Azure** | Getting stuck on classic vs ARM – use ARM/Bicep only |
| **Azure** | Forgetting that not all regions have Availability Zones |
| **GCP** | Forgetting `gcloud config set project` – project switching is critical |
| **GCP** | Using Cloud Functions when Cloud Run would be better |
| **Both** | Assuming "global" works like AWS – GCP global VPC is great; Azure global VNet peering is messy |

---

## 📚 CLI Quick Reference

| Task | AWS CLI | Azure CLI | GCloud CLI |
|------|---------|-----------|-------------|
| List VMs | `aws ec2 describe-instances` | `az vm list` | `gcloud compute instances list` |
| Create bucket | `aws s3 mb s3://bucket` | `az storage account create` | `gcloud storage buckets create gs://bucket` |
| List IAM roles | `aws iam list-roles` | `az role definition list` | `gcloud iam roles list` |
| Create VM | `aws ec2 run-instances` | `az vm create` | `gcloud compute instances create` |
| Set default region | N/A (in config) | `az configure --defaults location=useast` | `gcloud config set compute/region us-central1` |

---

## 🏁 Getting Started

```bash
# Create free accounts
https://azure.microsoft.com/en-us/free/
https://cloud.google.com/free

# Install CLIs
az --version
gcloud --version

# Azure login
az login

# GCP login
gcloud auth login

# Set default project (GCP) - DON'T SKIP
gcloud config set project YOUR_PROJECT_ID

# Set default subscription (Azure)
az account set --subscription "YOUR_SUBSCRIPTION_NAME"

# Set default region (both)
az configure --defaults location=eastus
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
```

---

## ✅ Final Check (End of Day 20)

You should be able to:

- [ ] Deploy a three-tier app (web + app + db) in each cloud from CLI
- [ ] Set up least-privilege IAM roles in all three
- [ ] Explain how to migrate a workload from AWS → Azure and AWS → GCP
- [ ] Translate an AWS policy to Azure RBAC and GCP IAM
- [ ] Answer: "What's a resource group?" (Azure) and "What's a project?" (GCP)
- [ ] Answer: "How do storage tiers work in Azure vs GCP?"
- [ ] Answer: "What's the difference between Cloud Run and Cloud Functions?"

---

**Happy multi-cloud learning!** 🚀
```

The key additions now include:
- **Azure tiny topics**: Resource Groups, Subscriptions, Management Groups, NSG priority, Storage tiers, Function triggers, Managed Disks, Public IP SKUs
- **GCP tiny topics**: Projects, Folders, Labels vs Tags, Zones, Service accounts, Bucket uniqueness, Firewall rules, Preemptible VMs, Cloud Run vs Functions
- **Both clouds**: Global resources, quotas, free tier, billing alerts, CLI output formats

These are the small things that AWS experts miss but will bite you in production.
