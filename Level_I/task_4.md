## Task #4 - Copy Data to App Servers using Ansible

Modify the default configuration of Ansible to enable the use of `mariyam` as the default SSH user for all hosts.

Solution:

```shell
$ vi /etc/ansible/ansible.cfg
```

```ini
[defaults]
remote_user=mariyam
```
