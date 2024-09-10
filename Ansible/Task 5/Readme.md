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

``` yaml
---
- name: Create and configure /tmp/nfsdata.txt on all app servers
  hosts: app_servers
  become: yes
  tasks:
    - name: Create a blank file /tmp/nfsdata.txt
      file:
        path: /tmp/nfsdata.txt
        state: touch
        mode: '0755'

    - name: Set ownership on app server 1
      file:
        path: /tmp/nfsdata.txt
        owner: tony
        group: tony
      when: inventory_hostname == 'stapp01'

    - name: Set ownership on app server 2
      file:
        path: /tmp/nfsdata.txt
        owner: steve
        group: steve
      when: inventory_hostname == 'stapp02'

    - name: Set ownership on app server 3
      file:
        path: /tmp/nfsdata.txt
        owner: banner
        group: banner
      when: inventory_hostname == 'stapp03'
```

``` shell
thor@jumphost ~$ cd playbook/
thor@jumphost ~/playbook$ vi inventory
thor@jumphost ~/playbook$ vi playbook.yml
thor@jumphost ~/playbook$ ansible-playbook playbook.yml -i inventory 

PLAY [Create and configure /tmp/nfsdata.txt on all app servers] ********************************************

TASK [Gathering Facts] *************************************************************************************
ok: [stapp03]
ok: [stapp02]
ok: [stapp01]

TASK [Create a blank file /tmp/nfsdata.txt] ****************************************************************
changed: [stapp01]
changed: [stapp03]
changed: [stapp02]

TASK [Set ownership on app server 1] ***********************************************************************
skipping: [stapp02]
skipping: [stapp03]
changed: [stapp01]

TASK [Set ownership on app server 2] ***********************************************************************
skipping: [stapp01]
skipping: [stapp03]
changed: [stapp02]

TASK [Set ownership on app server 3] ***********************************************************************
skipping: [stapp01]
skipping: [stapp02]
changed: [stapp03]

PLAY RECAP *************************************************************************************************
stapp01                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp02                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
stapp03                    : ok=3    changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   

thor@jumphost ~/playbook$ 
```
