# Module 09 — Cloud Migrations with AWS DMS

Hands-on lab demonstrating a database lift-and-shift migration from a simulated on-premises MySQL server to Amazon RDS using AWS Database Migration Service (DMS).

## What This Lab Covers

- **7Rs Framework**: Rehost (lift-and-shift) migration strategy
- **AWS DMS**: Full-load database migration with replication instance, source/target endpoints, and migration task
- **Amazon RDS**: MySQL 8.0 target database in private subnets
- **EC2**: Simulated on-premises source server running MySQL
- **IAM**: DMS VPC role configuration for subnet group access
- **Infrastructure as Code**: All 20 resources provisioned with Terraform

## Architecture

```
Public Subnet (us-east-1a)       Private Subnets (us-east-1a/b)
┌─────────────────────┐          ┌──────────────────────────────┐
│  EC2 Source Server  │          │   DMS Replication Instance   │
│  (Simulated OnPrem) │─────────►│   (dms.c5.large)             │
│  MySQL + appdb      │  source  │          │                   │
│  10.0.1.69          │ endpoint │          │ target endpoint   │
└─────────────────────┘          │          ▼                   │
                                 │   RDS MySQL 8.0 Target       │
                                 │   (db.t3.micro, private)     │
                                 └──────────────────────────────┘
```

## Migration Results

| Metric | Result |
|---|---|
| Migration Type | Full Load |
| Tables Migrated | 1 (customers) |
| Rows Transferred | 5 |
| Completion Status | ✅ stopped · 100% |
| Terraform Resources | 20 |

## Key Lessons Learned

- **DMS instance classes in us-east-1**: No t-class instances exist — minimum is `dms.c5.large`. Confirmed via `aws dms describe-orderable-replication-instances`.
- **IAM propagation timing**: The `dms-vpc-role` must fully propagate before the DMS subnet group creates. Terraform `depends_on` is essential.
- **7Rs in practice**: Rehost delivers immediate cloud benefits (managed infrastructure, cost optimization) without application changes — the fastest migration path.

## Technologies Used

- AWS Database Migration Service (DMS)
- Amazon RDS MySQL 8.0
- EC2 (Amazon Linux 2023)
- VPC, Subnets, Security Groups
- IAM (DMS VPC role)
- Terraform
- AWS CLI

## Portfolio

Part of [Eli's AWS Cloud Architecture Portfolio](https://d2xgifno1trxs4.cloudfront.net) — a 9-module hands-on lab series covering core cloud architecture domains.
