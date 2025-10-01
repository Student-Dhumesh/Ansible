## Create a Web Content Direcotry

### Instructions

- Create a playbook called /home/student/ansible/webcontent.yml

- The playbook runs on managed nodes in the dev host group

### Tasks

- Create a directory /webdev with following requirements
- It is owned by the webdev group
- Permissions 
- owner = read + write + execute
- group = read + write + execute
- other = read + execute
- It has the special permission - SGID
- Create a symbolic link /var/www/html/webdev pointing to /webdev

- Create the file /web/index.html with a single line of text that reads - "Development"


## Solution

```
---
- name: Create a web content directory
  hosts: dev
  vars:
    group: webdev
    src: /webdev
    mode: 2775
    dest: /var/www/html/webdev

  tasks: 
    - name: Creating "{{group}}" group
      group:
        name: "{{group}}"
        state: present

    - name: Creating "{{src}}" directory
      file:
        path: "{{src}}"
        group: "{{group}}"
        mode: "{{mode}}"

    - name: Creating a symbolic link "{{src}}" to "{{dest}}"
      file:
        src: "{{src}}"
        dest: "{{dest}}"
        state: link
```
