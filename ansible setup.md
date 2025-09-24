# Ansible

## Setup For Ansible

- We need 2 or 3 machines 

1. control-node
2. managed-node1
3. managed-node2

- You can set hostname of machines through

```
hostnamectl set-hostname control-node   # on main machine

hostnamectl set-hostname managed-node1  # on remote machine

hostnamectl set-hostname managed-node1  # on remote machine
```

<br>
<br>

## In cntrol node

### Installation of ansible 

- First we need to configure yum repos

```
vim /etc/yum.repos.d/aa.repo
```

- Paste this into aa.repo file

```

[AppStream]
name=AppStream
baseurl=https://repo.almalinux.org/almalinux/9/AppStream/x86_64/os
enable=1
gpgcheck=0

[BaseOS]
name=BaseOS
baseurl=https://repo.almalinux.org/almalinux/9/BaseOS/x86_64/os
enable=1
gpgcheck=0

```

- Check repolist through

```
yum repolist
```

- Install EPEL - Extra Package for Enterprise Linux

```
dnf install epel-release

or

dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
- Now you can install ansible through

```
dnf install ansible 
```

- Verify it through

```
ansible --version
```
<br>
<br>

Next

- Open /etc/hosts file

```
vim /etc/hosts
```

- Make entries 

```
# <remote-machine-ip-address>    hostname    alias
# Example

172.0.1.94  managed-node1   node1
172.0.1.95  managed-node2   node2

```

- Verify through 

```
ping node1
ping node2
```

- On remote machines like node1, node2 
- Create a user with any name in our case student

```
useradd student
passwd student
```

- Connect to remote machine through ssh

```
ssh student@node1
ssh student@node2
```

- It ask for password everytime so we can
- On control-node

```
ssh-keygen

ssh-copy-id node1 node2
```



<br>
<br>

Next

- Create a user with anyname in our case student

```
useradd student
passwd student

su - student
```

- In student home directory create a directory

    ansible

- In ansible directory create 2 files
1. ansible.cfg
2. inventory

```
cd ~

mkdir ansible

cd /home/student/ansible

touch ansible.cfg inventory

```

- Inside ansible.cfg 

```
[defaults]
inventory=/home/student/ansible/inventory

# ask-pass = true
```

- Inside inventory

```
node1
node2

[webserver]
node1

[database]
node2
```


- Verify all these configuration

```
ansible -m ping webserver

ansible -m ping database
```