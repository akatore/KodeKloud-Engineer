The Nautilus DevOps team needs to copy data from the `jump host` to all `application servers` in `Stratos DC` using Ansible. Execute the task with the following details:

a. Create an inventory file `/home/thor/ansible/inventory` on `jump_host` and add all application servers as managed nodes.

b. Create a playbook `/home/thor/ansible/playbook.yml` on the `jump host` to copy the `/usr/src/data/index.html` file to all application servers, placing it at `/opt/data`.

`Note:` Validation will run the playbook using the command `ansible-playbook -i inventory playbook.yml`. Ensure the playbook functions properly without any extra arguments.




``` shell
vi inventory
```
``` yaml
[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password=Am3ric@ 
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password=BigGr33n

[lb_server]
stlb01 ansible_host=172.16.238.14 ansible_user=loki ansible_password=Mischi3f

[db_server]
stdb01 ansible_host=172.16.239.10 ansible_user=peter ansible_password=Sp!dy

[storage_server]
ststor01 ansible_host=172.16.238.15 ansible_user=natasha ansible_password=Bl@kW

[backup_server]
stbkp01 ansible_host=172.16.238.16 ansible_user=clint ansible_password=H@wk3y3

[mail_server]
stmail01 ansible_host=172.16.238.17 ansible_user=groot ansible_password=Gr00T123

[jump_server]
jump_host ansible_host=jump_host.stratos.xfusioncorp.com ansible_user=thor ansible_password=mjolnir123

[ci_cd_server]
jenkins ansible_host=172.16.238.19 ansible_user=jenkins ansible_password=j@rv!s
```

``` shell
vi playbook.yml
```

``` yaml
---
- name: Copy index.html file to all application servers at '/opt/data'
  hosts: "{{ targetHost }}"
  become: yes
  tasks:
    - name: copy
      copy:
        src: /usr/src/data/index.html
        dest: /opt/data/index.html 
```
``` shell
ansible-playbook -i inventory playbook.yml -e "targetHost=app_servers"
```
``` shell
ansible-playbook -i inventory playbook.yml -e "targetHost=app_servers"

PLAY [Copy index.html file to all application servers at '/opt/data'] ************************************************

TASK [Gathering Facts] ***********************************************************************************************
ok: [stapp03]
ok: [stapp02]
ok: [stapp01]

TASK [copy] **********************************************************************************************************
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP ***********************************************************************************************************
stapp01                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp02                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
stapp03                    : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
