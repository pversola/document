# NFS
## How to install and config

***NFS server*** (Default port **2049**)
1. install nfs packages  
   `sudo yum -y install nfs-utils`  
2. Create share directory
   `mkdir -p /srv/nfs/k8s`
3. Change permission   
   `chown -R nfsnobody:nfsnobody /srv/nfs/`  
   `chmod -R 775 /srv/nfs/`
4. Config export share path `vi /etc/exports`
   ```
   /srv/nfs/k8s    *(rw,sync,no_subtree_check,no_root_squash,insecure)
   ```
5. run `export -rf` and restart service  
   `systemctl restart nfs-server`
6. Verify export  
   `showmount -e`

***Client***
1. install snmp packages  
   `sudo yum -y install nfs-utils` 
2. Create mount share directory  
   `mkdir -p /mnt/nfs/k8s`
3. Mount network file system  
   `mount -t nfs 10.255.6.41:/srv/nfs/k8s /mnt/nfs/k8s`
4. Config mount permanent ***(do not in K8S cluster)***  
   `echo '10.255.6.41:/srv/nfs/k8s /mnt/nfs/k8s nfs defaults 0 0' >> /etc/fstab`
5. How to unmount network file system  
   `unmount /mnt/nfs/k8s`