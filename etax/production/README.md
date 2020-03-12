# Environment
## Dev
> HSM ip address `10.1.248.204:1792`

---

## Prod
> GMC ip address `10.1.254.206:9961`  
> HSM ip address `10.1.254.204:1792`


---

## Standard Server Name

No | Detail | Lenght 
---| --- | ---
1 | Project Name | 3
2 | Site Name | 3
3 | Environment | 3
4 | Service | 2
5 | Running | 2

Example
 - Project: ETAX --> ETX
 - Site: NIPA CLOUD --> NPA
 - Environment: Develop | UAT | PRODUCTION --> PRD
 - Serveice: Database --> DB
 - Running: 1 --> 01

 Host Name is `ETXNPAPRDDB01` 

---
## Server

No | Host Name | OS | CPU | MEMORY | DISK | IP Addr | EXT IP Addr | Remark
--- | --- | --- | --- | --- | --- | --- | --- | ---
1 | [ETXNPAPRDLB01](./production/ETXNPAPRDLB01.md) | Centos 7 | 2 | 4 GB | 80 GB | 10.148.2.11 | -- | Proxy
2 | [ETXNPAPRDLB02](./production/ETXNPAPRDLB02.md) | Centos 7 | 2 | 4 GB | 80 GB | 10.148.2.12 | -- | Proxy
3 | [ETXNPAPRDAP01](./production/ETXNPAUATAP01.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.11 | -- | K8s Master, Worker
4 | [ETXNPAPRDAP02](./production/ETXNPAUATAP02.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.12 | -- | K8s Master, Worker
5 | [ETXNPAPRDAP03](./production/ETXNPAUATAP03.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.13 | -- | K8s Worker
6 | [ETXNPAPRDAC01](./production/ETXNPAPRDAC01.md) | Window Server 2016 | 2 | 4 GB | 80 GB | 10.148.3.111 | -- | Web Archive
7 | [ETXNPAPRDFS01](./production/ETXNPAPRDFS01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.11 |-- | File Server
8 | [ETXNPAPRDDB01](./production/ETXNPAUATDB01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 |-- | MsSQL
9 | [ETXNPAPRDDB02](./production/ETXNPAUATDB02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 |-- | MsSQL
10 | [ETXNPAPRDBK01](./production/ETXNPAPRDBK01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.111 |-- | Backup
11 | [ETXNPAPRDMT01](./production/ETXNPAPRDMT01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 |-- | Monitor
12 | [ETXNPAPRDLG02](./production/ETXNPAPRDLG02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 |-- | Log



