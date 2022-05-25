# Kubernetes
## Imprement
***All Node***  
1. Login root `sudo su -`
2. Update OS `yum update -y`
3. config hosts  
   `echo '10.255.6.31 node1' >> /etc/hosts`  
   `echo '10.255.6.32 node2' >> /etc/hosts`  
   `echo '10.255.6.33 node3' >> /etc/hosts`
4. Disable firewall (if have local firewall)  
   `systemctl stop firewalld`  
   `systemctl disable firewalld`  
5. Generate ssh key  
   `ssh-keygen -t rsa`
6. manual copy public key /root/.ssh/id_rsa.pub to all node at /root/.ssh/authorized_keys  
   `cat /root/.ssh/id_rsa.pub` copy public key.  
   `vi /root/.ssh/authorized_keys` paste publickey and save.  
   check ssh key with command  `ssh root@<IP ADDRESS>` all node

***Node1 Only***  
1. install git  `yum install git -y`  
1. install python & pip   
   `yum install python3-pip python-pip python3-setuptools python-setuptools -y`
1. upgrade pip latest version  
   `pip3 install --upgrade pip`
1. pull kubespray from repository  
   `git clone https://github.com/kubernetes-sigs/kubespray.git  /opt/kubespray`
1. go to  directory   
   `cd /opt/kubespray`
1. git change branch  
   `git checkout release-2.12`
1. Install dependencies from **requirements.txt**  
   `pip3 install -r requirements.txt`
1. Copy **inventory/sample** as **inventory/mycluster**  
   `cp -rfp inventory/sample inventory/mycluster`
1. Update Ansible inventory file with inventory builder  
   `declare -a IPS=(10.255.6.31 10.255.6.32 10.255.6.33)`  
   `CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}`
1. Review and change parameters  
   `cat inventory/mycluster/group_vars/all/all.yml`  
   `cat inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml`
1. Deploy Kubespray with Ansible Playbook - run the playbook as **root**
   `ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml`
1. After Install finish check kubernetes with `kubectl get nodes`


***How to get image is available***  
`kubectl get pods -o=jsonpath='{range .items[*]}{range .spec.containers[*]}{.image}{", "}{"\n"}{end}{end}'`  



***How to install ingress-nginx kubectl plugin***
1. install krew
   - Make sure that **git** is installed.
   - Run this command to download and install krew
   ```
   (
     set -x; cd "$(mktemp -d)" &&
     curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/krew.{tar.gz,yaml}" &&
     tar zxvf krew.tar.gz &&
     KREW=./krew-"$(uname | tr '[:upper:]' '[:lower:]')_amd64" &&
     "$KREW" install --manifest=krew.yaml --archive=krew.tar.gz &&
     "$KREW" update
   )
   ```
   - Add ***$HOME/.krew/bin*** directory to your PATH environment variable
   ```
   # vi ~/.bash_profile

   ...
   PATH=$PATH:${KREW_ROOT:-$HOME/.krew}/bin
   ...
   
   # source ~/.bash_profile
   ```
2. install the plugin. Then run   
   `kubectl krew install ingress-nginx`
3. Setting ***--service-node-port-range***  
   update file **/etc/kubernetes/manifests/kube-apiserver.yaml** and update the line from `--service-node-port-range=30000-32767` to `--service-node-port-range=1-32767`
