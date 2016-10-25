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
* Perl

Role Variables
--------------

Available variables are listed below,

**defaults/main.yml:**
```
savedir: "fetched"
remtemp: "/tmp/remtemp"
```

**vars/com_vars.yml:**

```
lists:
  - { com: 'ls -l /etc' , ofile: 'ls_l' }
  - { com: 'hostid' , ofile: 'hostid_out' }
  - { com: 'cat /etc/hosts' , ofile: 'cat_hosts' }
```

This vars file will be created with automatic when you run a playbook.
All you have to do is create a text file as below.


```
<command>,<output file>
<command>,<output file>
<command>,<output file>
```

**Ex) sample_comlist.txt:**

```
'ls -l /etc','ls_l'
'hostid','hostid_out'
'cat /etc/hosts','cat_hosts'
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

  pre_tasks:
    - name: Pre get command list
      local_action: raw ./roles/tksarah.fetch-command-out/tools/create_vars.pl "{{ inputcommand }}"
      become: false

  roles:
    - tksarah.fetch-command-out
```

License
-------

BSD

Author Information
------------------

fetch-command-out role was written by: T.Kuramochi
