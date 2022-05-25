# How to install MariaDB Galera Cluster On Ubuntu

First provision 3 virtual machine with Ubuntu 20.04
| Hostname | IP address | CPU | Memory | Remark |
| ------------ | -------------- | --- | ------ | --------------------------------- |
| galera-ha-01 | 192.168.200.21 | 2 | 4 | HA Proxy |
| galera-db-01 | 192.168.200.11 | 2 | 4 | |
| galera-db-02 | 192.168.200.12 | 2 | 4 | |
| galera-db-03 | 192.168.200.13 | 2 | 4 | |

### Step 1 Update Ubuntu Server (On all nodes)

```
$ sudo apt update
$ sudo apt -y upgrade && sudo systemctl reboot
```

Fix Hosts (All hosts)

```
sudo -i
echo '192.168.200.21 galera-ha-01' >> /etc/hosts
echo '192.168.200.11 galera-db-01' >> /etc/hosts
echo '192.168.200.12 galera-db-02' >> /etc/hosts
echo '192.168.200.13 galera-db-03' >> /etc/hosts
```

### Step 2 Install MariaDB 10.6

Set up the repository

```
$ sudo apt update
$ sudo apt -y install software-properties-common curl

$ curl -fsSL 'https://mariadb.org/mariadb_release_signing_key.asc' | sudo apt-key add -

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/mariadb.list
deb https://mariadb.mirror.liquidtelecom.com/repo/10.6/ubuntu focal main
EOF

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/mariadb.list
deb https://mariadb.mirror.liquidtelecom.com/repo/10.5/ubuntu focal main
EOF
```

```
$ sudo apt update
$ sudo apt -y install mariadb-server mariadb-client
$ sudo systemctl enable mariadb --now
```

```
$ sudo mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] y
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.
```

```
$ mysql -u root -p

MariaDB [(none)]> SELECT VERSION();
+-------------------------------------+
| VERSION()                           |
+-------------------------------------+
| 10.6.1-MariaDB-1:10.6.1+maria~focal |
+-------------------------------------+
1 row in set (0.000 sec)

MariaDB [(none)]> exit;
```

# How to install Percona XtraBackup On Ubuntu

```
$ curl -O https://repo.percona.com/apt/percona-release_latest.$(lsb_release -sc)_all.deb

$ sudo dpkg -i percona-release_latest.$(lsb_release -sc)_all.deb

$ sudo apt update

$ apt -y install percona-xtrabackup-24 qpress

```
