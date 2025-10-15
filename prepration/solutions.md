## RHCE 9 Exam Practice

---

## 1
### Install and Confiugure Ansible

- Install packages:
```
sudo yum install ansible-navigator ansible tree vim -y
```
● Create directories:
```
mkdir /home/student/ansible
```
```
mkdir /home/student/ansible/roles
```
```
mkdir /home/student/ansible/mycollection
```
```
cd /home/student/ansible
```

```
vim inventory
```
```
[dev]
node1

[test]
node2

[prod]
node3
node4

[balancers]
node5

[webservers:children]
prod
```

```
vim ansible.cfg
```
```
[defaults]

inventory = /home/student/ansible/inventory

roles_path = /home/student/ansible/roles:/usr/share/ansible/roles

collections_path = /home/student/ansible/mycollection/.ansible/collections/usr/share/ansible/collections

[privilege_escalation]
become = true
become_method = sudo
become_user = root
```

● Test connection:
```
ansible all -m ping
```

---
---
## 2
### Confiugure Yum Repositories (repo.yml)

```
vim repo.yml
```

```
---
  - name: Yum repos configuration
    hosts: all
    tasks:
        - name: AppStream Configuration
          yum_repository:
            name: AppStream
            description: AppStream
            baseurl: http://content.example.com/rhel9.0-x86_64-dvd/AppStream
            gpgcheck: true
            gpgkey: http://content.example.com/rhel9.0-x86_64-dvd/RPM-GPG-KEY-redhat-release
            enabled: true
        - name: Add BaseOS repository
          yum_repository:
            name: BaseOS
            description: BaseOS
            baseurl: http://content.example.com/rhel9.0-x86_64-dvd/BaseOS
            gpgcheck: true
            gpgkey: http://content.example.com/rhel9.0-x86_64-dvd/RPM-GPG-KEY-redhat-release
            enabled: true
```

---
---

## 3
### Download and Install roles via requirements.yml

```
cd roles
```

```
vim requirement.yml
```
```
--- 
    - src: http://18.117.238.146/role1.tar.gz
      name: balancer
    
    - src: http://18.117.238.146/role2.tar.gz
      name: phpinfo
```
```
ansible-galaxy install -r requirements.yml -p .
```

---
---
## 4
### Create & apply offline apache role

```
cd roles
```

```
ansible-galaxy init apache
```

```
vim apache/templates/template.j2
```
```
Welcome to {{ ansible_fqdn }} on {{ ansible_default_ipv4.address }}
```
```
vim apache/tasks/main.yml
```
```
    - name: Install Apache
      dnf:
        name: httpd
        state: latest

    - name: Start & enable httpd
      service:
        name: httpd
        state: started
        enabled: true

    - name: Enable firewalld
      service:
        name: firewalld
        state: restarted
        enabled: true

    - name: Allow HTTP in firewall
      firewalld:
        service: http
        state: enabled
        permanent: true
        immediate: true

    - name: Deploy web page template
      template:
        src: template.j2
        dest: /var/www/html/index.html
```

```
cd /home/student/ansible
```
```
vim apache_role.yml
```
```
---
  - name: Deploy apache role
    hosts: webservers
    roles:
        - apache
```
```
ansible-playbook apache_role.yml
```


---
---

## 5
### Install Ansible Galaxy Collections

```
cd /home/student/ansible/mycollection
```

```
wget https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/community-general-11.4.0.tar.gz
```
```
wget https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/ansible-posix-2.1.0.tar.gz
```
```
ansible-galaxy collection install community-general-11.4.0.tar.gz -p .
```
```
ansible-galaxy collection install ansible-posix-2.1.0.tar.gz -p .
```

---
---

## 6
### Install & manage packages and groups
```
vim packages.yml
```
```
---
  - name: on dev and test group
    hosts: dev, test
    tasks:
        - name: install php and mariadb
          dnf:
            name:
                - php
                - mariadb-server
            state: present
            
  - name: on prod group
    hosts: prod
    tasks:
        - name: install development tools
          dnf:
            name: "@Development Tools"
            state: present

  - name: on dev group
    hosts: dev
    tasks:
        - name: update all packages
          dnf:
            name: "*"
            state: latest

```

---
---

## 7
### Create Web Content Directory

```
vim webcontent.yml
```
```
---
  - name: on dev group
    hosts: dev
    tasks:
        - name: create directory
          file:
            path: /devweb
            state: directory
            mode: '2775'
            group: devops
            setype: httpd_sys_content_t

        - name: create symbolic link
          file:
            src: /devweb
            dest: /var/www/html/devweb
            state: link

        - name: copy index.html
          copy:
            content: "Development"
            dest: /devweb/index.html
            setype: httpd_sys_content_t

        - name: Permit HTTPS
          firewalld:
            service: https
            permanent: true
            state: enabled
            immediate: true
```

---
---

## 8
### Replace /etc/issue Content 

```
vim issue.yml
```

```
---
  - name: on dev group
    hosts: dev
    tasks:
        - name: set /etc/issue
          copy:
            content: "Development"
            dest: /etc/issue

  - name: on test group
    hosts: test
    tasks:
        - name: set /etc/issue
          copy:
            content: "Test"
            dest: /etc/issue

  - name: on prod group
    hosts: prod
    tasks:
        - name: Set /etc/issue
          copy:
            content: "Production"
            dest: /etc/issue
```

---
---


## 9
### Generate /etc/myhosts from Template (hosts.yml)
```
127.0.0.1 localhost.localdomain localhost
192.168.0.1 localhost.localdomain localhost

{% for host in groups['all'] %}
{{ hostvars[host].ansible_default_ipv4.address }} {{ hostvars[host].ansible_fqdn }} {{ hostvars[host].ansible_hostname }}
{% endfor %}
```

```
vim hosts.yml
```

```
---
  - name: Deploy /etc/myhosts template
    hosts: dev
    tasks:
        - name: Deploy template
          template:
            src: hosts.j2
            dest: /etc/myhosts
```

---
---

## 10
### Use Roles for Load Balancer and PHP Info

```
vim roles.yml
```

```
---
  - name: Use balancer role
    hosts: balancers
    roles:
        - balancer

  - name: Use phpinfo role
    hosts: webservers
    roles:
        - phpinfo

```
---
---

## 11
### Encrypt Variables with Vault 

```
vim vault.yml
```
```
---
    pwdeveloper: lamdev
    pwmanager: lammgr
```
```
echo "PsswOrd" > secret.txt
```
add in ansible.cfg

```
vault-password-file=secret.txt
```



---
---
## 12
### Rekey Encrypted Variable Files

```
wget http://18.117.238.146/secret.yml
```

```
ansible-vault rekey solaris.yml
```
Password: cisco
New password: redhat

---
---

## 13
### Manage Users with Variables (users.yml)

```
wget http://18.117.238.146/user_list.yml
```

```
vim user.yml
```

```
---
  - name: Create group opsdev and users (job=developer)
    hosts: dev, test
    vars_files:
        - userlist.yml
        - vault.yml
    tasks:
        - name: Create opsdev group
          ansible.builtin.group:
          name: devops
            state: present
    
        - name: Create users (developer)
          ansible.builtin.user:
            name: "{{ item.name }}"
            groups: opsdev
            password: "{{ pwdeveloper }}"
          loop: "{{ users }}"
          when: item.job == 'developer'

  - name: Create group opsmgr and users (job=manager)
    hosts: prod
    vars_files:
        - userlist.yml
        - vault.yml
    tasks:
        - name: Create opsmgr group
          ansible.builtin.group:
            name: opsmgr
            state: present

        - name: Create users (manager)
          ansible.builtin.user:
            name: "{{ item.name }}"
            groups: opsmgr
            password: "{{ pwmanager }}"
          loop: "{{ users }}"
          when: item.job == 'manager'
```

---
---

## 14
### Create a Cron Job 
```
vim crontab.yml
```

```
---
  - name: Setup cron job
    hosts: all
    tasks:
        - name: Create cron job for user student
          cron:
            name: "RH294 in progress"
            minute: "*/2"
            user: student
            job: "logger RH294 in progress"
            state: present
```
---
---

## 15
### Hardware Report via Template 
```
vim hwreport.yml
```

Download template and run:
```
---
  - name: Create hardware report
    hosts: all
    tasks:
        - name: Download hwreport template
          get_url:
          url: http://18.117.238.146/hwreport.empty
          dest: /root/hwreport.txt

        - name: Replace values in hwreport.txt
          replace:
            path: /root/hwreport.txt
            regexp: "{{ item.oldvalue }}"
            replace: "{{ item.newvalue }}"
        loop:
            - { oldvalue: 'hostname', newvalue: "{{ ansible_hostname | default('NONE') }}" }
            - { oldvalue: 'biosversion', newvalue: "{{ ansible_bios_version | default('NONE') }}" }
            - { oldvalue: 'memory', newvalue: "{{ ansible_memtotal_mb | default('NONE') }}" }
            - { oldvalue: 'vdasize', newvalue: "{{ ansible_devices.vda.size | default('NONE') }}" }
            - { oldvalue: 'vdbsize', newvalue: "{{ ansible_devices.vdb.size | default('NONE') }}" }
```

---
---

## 16
### Logical Volume Creation 

```
vim ansiblelv.yml
```
```
---
  - name: Create logical volumes
    hosts: all
    tasks:
        - name: Fail if volume group doesn't exist
          fail:
            msg: 'Volume group does not exist'
          when: ansible_lvm.vgs.research is not defined

        - name: Create 1500M LVM if enough space
          lvol:
            vg: research
            lv: data
            size: 1500m
          when: ansible_lvm.vgs.research.free_g >= 1.6

        - name: Create 800M LVM if not enough space
          lvol:
            vg: research
            lv: data
            size: 800m
          when: ansible_lvm.vgs.research.free_g < 1.6

        - name: Format LVM with ext4
          filesystem:
            fstype: ext4
            dev: /dev/research/data
```

---
---

## 17
### Confiugure Timesync System Role 
```
timesync.yml
```

```
---
  - name: Use timesync system role
    hosts: all
    vars:
        timesync_ntp_servers:
        - hostname: classroom.lab.example.com
        iburst: yes
    roles:
        - rhel-system-roles.timesync
```

---
---

## 18
### Confiugure SELinux with System Role 
```
vim selinux.yml
```

```
---
  - name: Set SELinux enforcing
    hosts: all
    vars:
        selinux_state: enforcing
    roles:
        - rhel-system-roles.selinux
```