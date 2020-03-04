# Environment
## Dev
> HSM ip address `10.1.248.204:1792`

---

## Prod
> GMC ip address `10.1.254.206:9961`  
> HSM ip address `10.1.254.204:1792`


---

No | Host Name | OS | CPU | MEMORY | DISK | IP Addr | EXT IP Addr
--- | --- | --- | --- | --- | --- | --- | ---
1 | EXTNPAUATLB01 | Centos 7 | 2 | 4 GB | 80 GB | 10.148.1.14 | 139.5.146.54
2 | EXTNPAUATAP01 | Centos 7 | 4 | 8 GB | 80 GB | 10.148.1.16 | 139.5.146.128
3 | ETXNPAUATAC01 | Window Server 2016 | 2 | 4 GB | 80 GB | 10.148.1.9 | 139.5.146.211
4 | ETXNPAUATDB01 | Centos 7 | 4 | 8 GB | 120 GB | 10.148.1.17 | 103.74.254.43 
5 | ETXNPAUATVPN01 | FusionHub | 2 | 4 GB | 80 GB | 10.148.1.5 | 103.74.255.238

---

### Service
No | Service | TCP | TCP (Usage) | UDP  | UCP (Usage)
--- | --- | --- | --- | --- | ---
1 | SSH | 22 | 42200 | - | -
2 | RDP | 3389 | 49833 | - | -
3 | SNMP | - | - | 161, 162 | -
---

### Database
No | Product | TCP | TCP (Usage) | UDP  | UCP (Usage)
--- | --- | --- | --- | --- | ---
1 | MsSQL | 1433 | 33411 | 1434 | 33412
2 | Oracle | 1521 | 33451 | - | -
3 | MySQL | 3306 | 33421 | - | -
4 | MariaDB | 3306 | 33431 | - | -
5 | Postgres | 5432 | 33461 | - | -
6 | MongoDB | 27017 | 33441 | - | -

---

### Application
No | TCP | TCP (Usage)
--- | --- | ---
1 | 80 | 55001 - 55xxx