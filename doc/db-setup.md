# Database Setup

Note that this configuration uses a VM to host the database, instead of a managed instance.

## 1. VM Setup


### 1a. Creating a new instance

_Note: This is all done within Nectar._

* Availability zone: `QRISCloud`
* Security: Set the `ssh`, `database`, and `egress_all` security groups.
* Instance name: `db-prod`
* Flavor: `m3.medium`
* Locality: `None`


### 1b. Update the DNS record

Log in to [GoDaddy](https://dcc.godaddy.com/manage/dns?domainName=ECOGENOMIC.ORG) 
to update the DNS record:

```
Type:         A
Host:         db.gtdb
Points to:    ip.address.of.server
TTL:          Default (1 Hour)
```


## 2. Follow the steps in Step 2 of "vm_setup.md" to configure access

## 3. Installing PostgreSQL

### 3a. Installation

Note: Discourse requires PostgreSQL >=13.

```shell
sudo apt-get update
sudo apt-get upgrade

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql

sudo apt-get install postgresql-13
```

### 3a. Configuration

#### Database:

TODO

#### Accounts:

```shell
sudo -u postgres createuser --superuser root

sudo -u postgres psql
ALTER USER root PASSWORD 'use the password in BitWarden';
\q

TODO: Make this just the IP address of the Nectar VM + UQ internal 
sudo echo "host    all     all             130.102.13.60                 md5" >> /etc/postgresql/13/main/pg_hba.conf

sudo -u postgres createdb discourse
sudo -u postgres createdb gtdb_r95
```

#### Restore databases (optional) :

```shell
pg_dump --host=qv4q4q5knjl.db.cloud.edu.au --username=postgres --password -F t gtdb_r95 > /tmp/gtdb_r95.tar
pg_restore --dbname=gtdb_r95 --no-privileges --no-owner --username=root --password /tmp/gtdb_r95.tar
```

