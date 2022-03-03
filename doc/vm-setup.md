# VM Setup

Below are a list of all commands run during the installation process on a clean 
VM.

## 1. Creating a new instance

Mostly defaults can be used, if in doubt follow 
[official support](https://support.ehelp.edu.au/support/solutions/articles/6000055446-accessing-instances).

* Source: `NeCTAR Ubuntu 20.04 LTS (Focal) amd64`
* Favour: `m3.small`
* Security: Set the `ssh`, `database`, and `egress_all` security groups.
* Ensure the security allows SSH access
* You should add your SSH public key by using the import "Key Pair" option.

## 2. Instance setup

### 2a. Connecting for the first time
Connect to the instance using the root account:

```shell
ssh ubuntu@ip.address.of.server
```

### 2b. Creating your own account

You should now provision yourself (and others) new accounts:

```shell
sudo useradd -m -s /bin/bash USERNAME 
sudo passwd USERNAME
sudo usermod -aG sudo USERNAME
```

Add your SSH key to the list of authorised users to log in to that account:

```shell
sudo su - USERNAME
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

### 2c. Increasing the swap size

By default, the swap size is ~100 MB for Nectar instances. The following increases it to 1 GB.

```shell
sudo swapoff -a
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
grep SwapTotal /proc/meminfo
```

### 2d. Setting the local date and time

```shell
sudo timedatectl set-timezone Australia/Brisbane
sudo timedatectl set-ntp on
```

## 3. Software installation

In order to run the instance the following software are required:

* [Docker](https://docs.docker.com/engine/install/ubuntu/#installation-methods)
* [Docker compose](https://docs.docker.com/compose/install/)

### 3a. Software configuration

The default subnet that Docker uses conflicts with the UQ VPN, you will need 
to update `/etc/docker/daemon.json` to be something like this:

```json
{
  "default-address-pools" : [
    {
      "base" : "192.168.56.0/16",
      "size" : 24
    }
  ]
}
```

Once you're done, run `systemctl restart docker`

## 4. Clone the repository

_Note: This requires that you have [set up an SSH key with GitHub](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)._

### 4a. Setting up an SSH key

```shell
ssh-keygen -t ed25519 -C "USER@EMAIL.COM"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 4b. Adding the SSH key to your GitHub account

Follow the [guide](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account) 
or add [directly](https://github.com/settings/keys) to your GitHub account.

```shell
cat ~/.ssh/id_ed25519.pub
```

### 4c. Cloning the repository

```shell
git clone git@github.com:Ecogenomics/db.gtdb.ecogenomic.org.git /var/db.gtdb.ecogenomic.org
```
