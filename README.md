# Assignment 7: Disaster Recovery & High Availability

## ✅ Objective
Design a robust cloud-native Disaster Recovery (DR) and High Availability (HA) strategy for an enterprise application deployed on Microsoft Azure.

---

## ☁️ Disaster Recovery (DR) Plan

### 🔁 Backup Strategy
- Perform **daily full and incremental backups** of app data and databases.
- Store backups in **Azure Blob Storage** using **RA-GRS** (Read-access geo-redundant storage) for cross-region replication.
- Retention policy:
  - Full backups: 30 days
  - Incremental backups: 7 days

### 🧯 Recovery Strategy
- Monthly testing of backups by restoring into a sandbox environment.
- Use Azure CLI scripts or Azure Site Recovery to automate restoration steps.
- Leverage **Azure Site Recovery** for region-to-region failover of Virtual Machines.

---

## 📈 High Availability (HA) Strategy

- Use **Azure Load Balancer** or **Azure Application Gateway** to distribute traffic across multiple App Service instances or Virtual Machines.
- Deploy compute and database resources in **multiple Availability Zones**.
- Configure **Azure Traffic Manager** for geo-routing and regional failover.
- Use **Azure SQL Active Geo-Replication** or **Cosmos DB multi-region writes** for highly available database access.

---

## ⏱️ Recovery Objectives

| Metric | Description |
|--------|-------------|
| **RTO (Recovery Time Objective)** | ≤ 15 minutes – maximum downtime acceptable |
| **RPO (Recovery Point Objective)** | ≤ 5 minutes – maximum data loss acceptable |

---

## 🛡️ Azure Services Used

| Azure Service         | Purpose                                          |
|-----------------------|--------------------------------------------------|
| Azure Backup          | Automatic backups for VMs, databases, files      |
| Azure Site Recovery   | Region failover and disaster recovery orchestration |
| Azure Traffic Manager | DNS-level global load balancing and failover     |
| Azure Blob Storage    | Geo-redundant storage for backups and snapshots  |
| Azure SQL Geo-Replication | Continuous data replication across regions |

---

## 🧪 Automated Azure Backup (CLI Example)

### 🔹 Step 1: Create a Recovery Services Vault
```bash
az backup vault create \
  --name MyRecoveryVault \
  --resource-group MyResourceGroup \
  --location eastus
```

### 🔹 Step 2: Enable VM Backup
```bash
az backup protection enable-for-vm \
  --vault-name MyRecoveryVault \
  --resource-group MyResourceGroup \
  --vm MyVM \
  --policy-name DefaultPolicy

```
### 🔹 Step 3: Trigger an On-Demand Backup
```bash
az backup protection backup-now \
  --resource-group MyResourceGroup \
  --vault-name MyRecoveryVault \
  --container-name MyVM \
  --item-name MyVM \
  --retain-until 2025-04-30
