fetch-command-out
=========

Ansible Role: fetch-command-out
------------

As executing the playbook, the role is copied from the target host in background and save it as a text file.
The role allows to get the command list of specific output files simply.

Install
------------

```
# ansible-galaxy install --roles-path ./roles tksarah.fetch-command-out
```


Requirements
------------

* Ansible version 2.0 <

Role Variables
--------------

Available variables are listed below,

**defaults/main.yml:**
```
savedir: "fetched"
remtemp: "/tmp/remtemp"
```

All you have to do is create a text file as below.


```
1,<command>,<output file>
2,<command>,<output file>
3,<command>,<output file>
```

**Ex) sample_comlist.txt:**

```
1,"ls -l /etc","ls_l"
2,"hostid","hostid_out"
3,"cat /etc/hosts","cat_hosts"
```

Dependencies
------------

None

Example Playbook
----------------

```
- name: Playbook for fetching files
  hosts: "{{ target }}"
  gather_facts: no

  become: true
  become_user: root

  vars_prompt:
    - { name: "target" , prompt: "Input target host" ,  default: all , private: no }
    - { name: "inputcommand" , prompt: "Input your command list" , default: sample_comlist.txt , private: no }
    - { name: "remtemp" , prompt: "Input remote temp directory " , default: /tmp/remtemp , private: no }
    - { name: "savedir" , prompt: "Input a save directory" , default: fetched , private: no }

  roles:
    - tksarah.fetch-command-out
```

License
-------

BSD

Author Information
------------------

fetch-command-out role was written by: T.Kuramochi
