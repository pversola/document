# SSH
## How to custom port default
- edit sshd config  
`vi /etc/ssh/sshd_config`  
enable Port and custom port (Ex. custom form  port 22 to `5022`)  
```
# If you want to change the port on a SELinux system, you have to tell
# SELinux about this change.
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
#
Port 5022
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::
```  

- Restart ssh service  
`systemctl restart sshd`  

- SELinux this change.  
`semanage port -a -t ssh_port_t -p tcp 5022`  

- edit firewall config  
`vi /usr/lib/firewalld/services/ssh.xml`  

```
<port protocol="tcp" port="5022"/>
```