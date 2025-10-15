# RHCE 9

---
## 1
### Install and Configure Ansible on the control node as follows:

1. Install the required packages
2. Create a static inventory file called /home/student/ansible/inventory as follows:
- node1 is a member of the dev host group
- node2 is a member of the test host group
- node3 and node4 is a member of the prod host group
- node5 is a member of the balancers host group
- The prod group is a member of the webservers host group

3. Create a configuration file called ansible.cfg as follows:

- The host inventory file /home/student/ansible/inventory is defined
- The location of roles used in playbooks is defined as /home/student/ansible/roles
- The Install Ansible Content Collection location /home/student/ansible/mycollection

---
---
## 2
### Create repo.yml for configuring repository in all nodes.
Repository 1
- name = baseos-internal
- description = Baseos Description
- url = http://content/rhel9.0/x86_64/dvd/BaseOS
- gpg is enabled
- gpgkey = http://content.example.com/rhel9.0/x86_64/dvd/RPM-GPG-KEY-redhat-release
- repository is enabled

Repository 2
- name = appstream-internal
- description = App Description
- url = http://content/rhel9.0/x86_64/dvd/AppStream
- gpg is enabled
- gpgkey = http://content.example.com/rhel9.0/x86_64/dvd/RPM-GPG-KEY-redhat-release
- repository is enabled

---
---
## 3
### Create a directory 'roles' under /home/student/ansible

1. Create a playbook called requirements.yml under the roles directory and download the given roles under the 'roles' directory using galaxy command under it.

2. Role name should be balancer and download using this url
```
http://18.117.238.146/role1.tar.gz
```

3. Role name phpinfo and download using this url
``` 
http://18.117.238.146/role2.tar.gz
```

---
---
## 4
### Create offline role named apache under roles directory.
1. Install httpd package and the service should be start and enable the httpd service.
2. Host the web page using the template.j2
3. The template.j2 should contain
```
Welcome to HOSTNAME ON IPADDRESS
```
Where HOSTNAME is fully qualified domain name.

4. Create a playbook named apache_role.yml and run the role in webservers group

---
---
## 5
### Create a playbook called roles.yml and it should run balancer and phpinfo roles.
Use roles from Ansible Galaxy

Create a playbook called /home/admin/ansible/roles.yml
- The palybook contains a play that runs on host in the balances host group and uses the balances role.
- This role configures a service to load balance web server request between hosts in the webserver host group.
- Browsing to host in the balances host group (for example http://node5.lab.example.com) produces the following output:
```
Welcome to node3.lab.example.com on 172.25.250.12
```
- Reloading the Browser produces output from the alternet web server:
```
Welcome to node4.lab.example.com on 172.25.250.13
```
- The Playbook contains a play the runs on hosts in webserver host group and user the phpinfo role.
- Browsing to host in the webserver host group with the URL /hell.php produces the following output:
```
Hello PHP World from FQDN
```
- For example Browsing to http://node3.lab.example.com/hello.php produces the following output:
```
Hello PHP World from node3.lab.example.com
```
- along with various details of the PHP configuration include the version of PHP that is installed.
- Similarly, Browsing to http://node4.lab.example.com/hello.php, produces the following output:
```
Hello PHP World from node4.lab.example.com
``` 
- along with various details of the PHP configuration including the version of PHP that is installed


---
---
## 6
### Installing Ansible Content Collections
http://rhls.lab.example.com/materials
tar file

installs the collection in the local collections directory.

download tar file url given on exam

for practice you can use

```
https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/community-general-11.4.0.tar.gz
```

```
https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/ansible-posix-2.1.0.tar.gz
```


---
---
## 7
### Install packages in multiple group.
1. Install php and mariadb-server packages in dev and test group.
2. Install "RPM Development Tools" group package in prod group.
3. Update all packages in dev group.
4. Use separate play for each task and playbook name should be packages.yml.


---
---
## 8
### Create a playbook webcontent.yml and it should run on dev group.
1. Create a directory /devweb and it should be owned by devops group.
2. /devweb directory should have context type as "httpd"
3. Assign the permission for user=rwx,group=rwx,others=rx and group special permission should be applied to /devweb.
4. Create an index.html file under /devweb directory and the file should have the content
```
Developement
```
5. Link the /devweb directory to /var/www/html/devweb.


---
---
## 9
### Create hardware report using playbook in all nodes.
1. Download hwreport.txt from the url and save as it under /root/hwreport.txt

```
http://18.117.238.146/hwreport.empty
```


- /root/hwreport.empty should have the content with control node informations as key=value.

```
#hwreport
HOSTNAME=
MEMORY=
BIOS=
CPU=
DISK_SIZE_VDA=
DISK_SIZE_VDB=
```
2. If there is no information it have to show "NONE".
3. playbook name should be hwreport.yml.


---
---
## 10
### Replace the file /etc/issue on all managed nodes.
1. In dev group /etc/issue should have the content "Developement".
2. In test group /etc/issue should have the content "Test".
3. In prod group /etc/issue should have the content "Production".
4. Playbook name issue.yml and run in all managed nodes.


---
---
## 11
### Download the file 

```
http://18.117.238.146/hosts.j2
```

1. hosts.j2 is having the content.
```
127.0.0.1 localhost.localdomain localhost
192.168.0.1 localhost.localdomain localhost
```

2. The file should collect all node information like ipaddress,fqdn,hostname
and it should be the same as in the /etc/hosts file, if playbook run in all the managed node it must store in /etc/myhosts.

- Finally /etc/myhosts file should contains like.

```
127.0.0.1 localhost.localdomain localhost
192.168.0.1 localhost.localdomain localhost
172.25.250.10 node1.lab.example.com node1
172.25.250.11 node2.lab.example.com node2
172.25.250.12 node3.lab.example.com node3
172.25.250.13 node4.lab.example.com node4
172.25.250.14 node4.lab.example.com node5
```

3. Playbook name hosts.yml and run in on dev group.


---
---
## 12
### Create a variable file vault.yml and that file should contains the variable and its value.

- pw_developer is value lamdev
- pw_manager is value lammgr

1. vault.yml file should be encrpted using the password "P@sswOrd".
2. Store the password in secret.txt file and which is used for encrypt the variable file.



---
---
## 13
### Download the variable file 
```
http://18.117.238.146/user_list.yml
```

Playbook name users.yml and run in all nodes using two variable files user_list.yml and
vault.yml

---
- Create a group devops
- Create user from users variable who's job is equal to developer and need to be in opsdev group
- Assign a password using SHA512 format and run the playbook on dev and test.
- User password is {{ pw_developer }}
---
- Create a group opsmgr
- Create user from users variable who's job is equal to manager and need to be in opsmgr group
- Assign a password using SHA512 format and run the playbook on prod.
- User password is {{ pw_manager }}
---
- Use when condition for each play.

---
---
## 14
### Rekey the variable file from 

```
http://18.117.238.146/secret.yml
```

1. Old password: cisco
2. New password: redhat

---
---
## 15
### Create a cronjob for user student in all nodes, the playbook name crontab.yml and the job details are below
- Every 2 minutes the job will execute logger "EX294 in progress"

---
---
## 16
### Create & use a logical Volume
Create a playbook called /home/admin/ansible/ansible/lv.yml that runs on all managed nodes that does the following:

Creates a logical volume with these requirement:

- The logical Volume is created in the research volume group
- The logical volume name is data
- The logical volume size is 1500 Mib
- Format the logical volume with the ext4 file system
- If the requested logical volume size cannot be created, the error message could not create logical volume of that size should be displayed and size 800 MiB should be used instead.
- If the volume research does not exist, the error message volume group does not exist should be displayed does not mount the logical volume in any way.

---
---
## 17
### Use a RHEL timesysnc system role

Create a playbook called /home/admin/ansible/timesync.yml that:
- Runs on all managed nodes
- Uses the timesync role
- Configures the role to use the currently active NTP provider
- Configure the role to use the time server classroom.lab.example.com
- Configure the role to enable the iburst parameter

---
---
## 18
### Use Selinux role for all nodes
create selinux.yml and set selinux enforcing