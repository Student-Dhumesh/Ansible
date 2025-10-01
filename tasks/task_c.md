## Create user accounts

### Instructions

- Manually create a file called /home/student/ansible/user_list.yml with the following content

```
---
  user: 
    - name: joe
    - name: bob
    - name: alice
```

- Create a password file called /home/student/ansible/passwords.yml that contains the required password variables in plain yaml format

### Tasks

- Create a playbook file called /home/student/ansible/users.yml that create user accounts for all user accounts for all users listed in user_list.yml by using a loop

- All users should be created on managed nodes

- Assigned a password from the variables defined in passwords.yml (password must be stored as SHA512 hashes)

- Added as members of a suppplementary group devops

- The playbook should directly include the variables file /home/student/ansible/passwords.yml

### Solution

```
---
- name: Create User Accounts
  hosts: all
  vars_files:
    - ./user_list.yml
    - ./passwords.yml
  vars:
    sup_group: devops
  tasks:
    - name: Creating users 
      user:
        name: "{{user}}"
        password: "{{password}}"
        groups: "{{sup_group}}"
        state: present
      loop:
        - user
        - password
```

