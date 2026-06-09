# Zabbix template to monitor Veeam Agent for Linux Free backup job status

Monitor Veeam Agent for Linux Free backup job status and metrics using Zabbix.
This project provides a template and script to monitor the latest backup job status, performance metrics, and session details from Veeam Agent for Linux free standalone version.

<img width="1230" height="275" alt="image" src="https://github.com/user-attachments/assets/84357424-6822-4b81-a918-fdc1f551cfbb" />

## 💬 Feedback & Issues

Feedback, suggestions, and issue reports are always welcome — feel free to open an issue or contact me directly.

---
## 📦 Zabbix Items Overview


| Name | Key | Type | Description |
|-----|-----|------|-------------|
| Veeam valfree Get metrics | `system.run[sudo /etc/zabbix/scripts/zbx_veeam_get_metrics.sh]` | Dependent item | Main item that executes the collection script |
| Backup Data Processed | `veeam.valfree.data.processed` | Dependent item | Amount of backup data processed |
| Backup Data Transferred | `veeam.valfree.data.transferred` | Dependent item | Amount of backup data transferred |
| Backup Job End | `veeam.valfree.job.end` | Dependent item | Backup job end timestamp |
| Backup Job Name | `veeam.valfree.job.name` | Dependent item | Name of the backup job |
| Backup Job Start | `veeam.valfree.job.start` | Dependent item | Backup job start timestamp |
| Backup Job Status | `veeam.valfree.job.status` | Dependent item | Status of the backup job (Success, Failed, etc.) |
| Backup Session ID | `veeam.valfree.job.id` | Dependent item | Session ID of the backup job |

## 🚨 Zabbix Trigger Overview

| Severity | Name                                                  | Expression                                                                 |
|----------|-------------------------------------------------------|----------------------------------------------------------------------------|
| Average  | Veeam Linux Agent: Last Backup Failed    | `last(/Veeam Agent for Linux Free/veeam.valfree.job.status)="Failed"`|

---
## 🧩 Compatibility & Requirements

- Debian-based environment with Veeam Agent for Linux Free Edition
- Tested with Zabbix 6.4
- system.run allowed for server on agent
---

## ⚙️ Setup Script

### Quick install (skip steps 1-3)

```bash
sudo mkdir /etc/zabbix/scripts
sudo wget -P /etc/zabbix/scripts https://raw.githubusercontent.com/uw-asa/zabbix-veeam-linux-free-agent-template/refs/heads/zabbix6/zbx_veeam_get_metrics.sh
sudo chmod +x /etc/zabbix/scripts/zbx_veeam_get_metrics.sh
sudo adduser zabbix veeam
echo 'AllowKey=system.run[/etc/zabbix/scripts/zbx_veeam_get_metrics.sh]' | sudo tee /etc/zabbix/zabbix_agent2.d/veeam-linux-free.conf
sudo systemctl restart zabbix-agent2.service
```

### 1. Create script
```bash
sudo nano /etc/zabbix/scripts/zbx_veeam_get_metrics.sh
```
Copy script content from GitHub and save

### 2. Make script executable:
```bash
sudo chmod +x /etc/zabbix/scripts/zbx_veeam_get_metrics.sh
```

### 3. Configure permissions for Zabbix

Add zabbix to the veeam users group, so sudo access is not required

```bash
sudo adduser zabbix veeam
```

Tell Zabbix it's ok to run our script by copying this text:

```INI
AllowKey=system.run[/etc/zabbix/scripts/zbx_veeam_get_metrics.sh]
```

To a new Zabbix config file:

```bash
sudo nano /etc/zabbix/zabbix_agent2.d/veeam-linux-free.conf
```

### 4. Import Zabbix Template (Server)

Import the provided Zabbix template XML file into your Zabbix frontend. Assign the template to the desired hosts.

---


## ⚠️ Important Notes

- This template monitors Veeam Agent for Linux Free Edition (standalone).

---
Created with ❤️ by **databloat**  
Project: https://github.com/databloat

