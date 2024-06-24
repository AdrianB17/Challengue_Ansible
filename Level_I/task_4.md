## Task #4 - Copy Data to App Servers using Ansible

The Nautilus DevOps team needs to copy data from the jump host to all application servers in Stratos DC using Ansible. Execute the task with the following details:

a. Create an inventory file /home/thor/ansible/inventory on jump_host and add all application servers as managed nodes.
b. Create a playbook /home/thor/ansible/playbook.yml on the jump host to copy the /usr/src/data/index.html file to all application servers, placing it at /opt/data.

Solution:

```shell
$ vi /ansible/inventory
```

```ini
[app_servers]
stapp01.stratos.xfusioncorp.com ansible_host=172.16.238.10 ansible_user=<username> ansible_password=<password>
stapp02.stratos.xfusioncorp.com ansible_host=172.16.238.11 ansible_user=<username> ansible_password=<password>
stapp03.stratos.xfusioncorp.com ansible_host=172.16.238.12 ansible_user=<username> ansible_password=<password>
```
```shell
$ vi /ansible/playbook.yml
```

```yaml
---
- name: Copy index.html to application servers
  hosts: app_servers
  become: true  # Gain elevated privileges for copying
  tasks:
    - name: Copy index.html file
      copy:
        src: /usr/src/data/index.html
        dest: /opt/data
        mode: 0644  # Set file permissions (read-write for owner, read-only for group and others)
```
```shell
$ ansible-playbook -i inventory playbook.yml
```

