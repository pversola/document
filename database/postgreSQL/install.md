# Install PostgreSQL

### Step 1: Update Ubuntu Server

```
$ sudo apt update
$ sudo apt -y install \
    ca-certificates \
    curl
```

Add repository

```
$ curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/pgdg.list
deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main
EOF
```

### Step 2: Install PostgreSQL

```
$ sudo apt update
$ sudo apt install postgresql postgresql-contrib
```

```
$ sudo su - postgres
$ psql
# \conninfo
```
