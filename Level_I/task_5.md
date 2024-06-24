## Task #5 - Create Files on App Servers using Ansible

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
- name: Create and configure webdata.txt file
  hosts: app_servers
  become: true
  tasks:
    - name: Create /tmp/webdata.txt file
      file:
        path: /tmp/webdata.txt
        state: touch

    - name: Set permissions of /tmp/webdata.txt to 0744
      file:
        path: /tmp/webdata.txt
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: '0744'

    - name: Ensure correct ownership on /tmp/webdata.txt
      file:
        path: /tmp/webdata.txt
        owner: "{{ item.owner }}"
        group: "{{ item.owner }}"
        state: file
      loop:
        - { owner: "tony", hostname: "stapp01" }
        - { owner: "steve", hostname: "stapp02" }
        - { owner: "banner", hostname: "stapp03" }
      when: inventory_hostname == item.hostname

```
```shell
$ ansible-playbook -i inventory playbook.yml
```


