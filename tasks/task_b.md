## Modify File Content

### Instructions

- Create a playbook called /home/student/ansible/issue.yml
- The playbook runs on all inventory hosts

- The playbook replace the content of /etc/issue with a single line of text

### Tasks

- On hosts in the dev host group line = "Development"
- On hosts in the test host group line = "Test"
- On hosts in the prod host group line = "Production"

### Solution

```
---
- name: Creating file /etc/issue
  hosts: all
  tasks:
    - name: Creating file
      file:
        path: /etc/issue
        state: present

- name: Replace /etc/issue content
  hosts: dev
  vars:
    line: "Development"
  tasks:
    - name: Replacing /etc/issue content
      lineinfile:
        path: /etc/issue
        line: "{{line}}"

- name: Replace /etc/issue content
  hosts: test
  vars:
    line: "Test"
  tasks:
    - name: Replacing /etc/issue content
      lineinfile:
        path: /etc/issue
        line: "{{line}}"

- name: Replace /etc/issue content
  hosts: prod
  vars:
    line: "Production"
  tasks:
    - name: Replacing /etc/issue content
      lineinfile:
        path: /etc/issue
        line: "{{line}}"
```
