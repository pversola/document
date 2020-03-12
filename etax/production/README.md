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
1 | [ETXNPAPRDLB02](./production/ETXNPAPRDLB02.md) | Centos 7 | 2 | 4 GB | 80 GB | 10.148.2.12 | -- | Proxy
2 | [ETXNPAPRDAP01](./production/ETXNPAUATAP01.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.11 | -- | K8s Master, Worker
2 | [ETXNPAPRDAP02](./production/ETXNPAUATAP02.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.12 | -- | K8s Master, Worker
2 | [ETXNPAPRDAP03](./production/ETXNPAUATAP03.md) | Centos 7 | 4 | 8 GB | 80 GB | 10.148.3.13 | -- | K8s Worker
2 | [ETXNPAPRDAC01](./production/ETXNPAUATAP03.md) | Window Server 2016 | 2 | 4 GB | 80 GB | 10.148.3.111 | -- | Web Archive
4 | [ETXNPAPRDFS01](./production/ETXNPAUATDB02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.11 |-- | File Server
4 | [ETXNPAPRDDB01](./production/ETXNPAUATDB01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 |-- | MsSQL
4 | [ETXNPAPRDDB02](./production/ETXNPAUATDB02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 |-- | MsSQL
4 | [ETXNPAPRDMT01](./production/ETXNPAUATDB01.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.21 |-- | Monitor
4 | [ETXNPAPRDLG02](./production/ETXNPAUATDB02.md) | Centos 7 | 4 | 8 GB | 120 GB | 10.148.4.22 |-- | Log



